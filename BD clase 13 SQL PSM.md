---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - PSM
Created: 2026-03-03 14:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Módulos Persistentes Almacenados (PSM)
  - CREATE FUNCTION y CREATE PROCEDURE
  - Diferencias función vs procedimiento
  - Variables, constantes y asignación
  - Sentencias de control de flujo
  - Manejo de excepciones y condiciones
  - Bloques anidados y propagación de errores
---
# BD clase 13 — SQL/PSM

## Resumen

### Qué es un PSM

Un **PSM** (Persistent Stored Module) es código compilado y almacenado en la base de datos que se ejecuta si se tiene permiso de ejecución. Anteriormente se los conocía como **Stored Procedures**. Se invocan explícitamente desde otro PSM, desde un **trigger** o desde una aplicación cliente.

Los DBMS que soportan PSM potencian un lenguaje **declarativo** como SQL con extensiones de **lenguaje procedural** para manejar el flujo de control. SQL3 estandarizó la sintaxis, aunque las implementaciones siguen siendo dependientes de cada DBMS.

> [!important]
> Dentro de un PSM **NO** se permite el `SELECT` tradicional. Se utiliza la variante **`SELECT INTO`** para asignar resultados a variables.

> [!important]
> Los nombres de parámetros o variables locales **NO deben coincidir** con nombres de atributos de tabla, porque estos últimos tienen prioridad. Siempre agregar un prefijo o sufijo a las variables locales y parámetros formales.

---

### Funciones vs Procedimientos

SQL3 diferencia conceptualmente dos tipos de módulos:

| Aspecto | **Función** | **Procedimiento** |
|---|---|---|
| **Invocación** | `func_name(params)` en cláusulas SQL (SELECT, WHERE, etc.) | `CALL proc_name([params])` |
| **Valor de retorno** | Un valor único escalar obligatorio (en su nombre) | Nada (`void`) |
| **Parámetros** | Solo de entrada (**IN**) | **IN**, **OUT**, **INOUT** |
| **Uso en SQL** | Puede participar donde se usan funciones built-in | No puede participar de cláusulas SQL |
| **Salida múltiple** | No (solo retorna un escalar) | Parámetros OUT/INOUT pueden devolver escalares o resultsets dinámicos |

> [!tip]
> En **PostgreSQL** no existe `PROCEDURE` como en el estándar ANSI. Todo se maneja con `FUNCTION`. Una función con `RETURNS VOID` y sin parámetros OUT/INOUT se comporta como procedimiento. Los pasajes de parámetros en PostgreSQL son **solo por valor**.

---

### CREATE FUNCTION

La **función** devuelve un valor escalar obligatorio en su nombre. Los parámetros son solo de entrada (`IN`).

**Sintaxis ANSI:**

```sql
CREATE FUNCTION nombreFuncion ( [IN] param tipo, ... )
    RETURNS tipo_devuelto
    LANGUAGE SQL
    [ DETERMINISTIC | NOT DETERMINISTIC ]
    [ NO SQL | CONTAINS SQL | READS SQL | MODIFIES SQL DATA ]
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
BEGIN
    sentencias_SQL;
END
```

**Sintaxis PostgreSQL (caso 1 -- todos los parámetros IN):**

```sql
CREATE [OR REPLACE] FUNCTION nombreFuncion ( parametros )
    RETURNS tipo_devuelto
    AS $$
DECLARE
    -- variables locales
BEGIN
    sentencias_SQL;
END
$$ LANGUAGE plpgsql
[ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ];
```

**Sintaxis PostgreSQL (caso 2 -- existen parámetros OUT o INOUT):**

```sql
CREATE [OR REPLACE] FUNCTION nombreFuncion ( IN x tipo, OUT rta tipo )
    AS $$
DECLARE
    -- variables locales
BEGIN
    rta := expresion;
    RETURN;  -- sin valor, retorna en el nombre
END
$$ LANGUAGE plpgsql;
```

**Cláusulas opcionales ANSI:**

| Cláusula | Significado |
|---|---|
| `DETERMINISTIC` | Mismos parámetros + misma BD = mismo resultado. El DBMS puede cachear. |
| `NOT DETERMINISTIC` (default) | Resultado depende de valores no determinables (random, fecha actual, etc.) |
| `NO SQL` | No invoca nada del motor (ej: biblioteca externa) |
| `CONTAINS SQL` (default) | Contiene funciones SQL pero no accede a la BD |
| `READS SQL` | Contiene sentencias que **leen** la BD (SELECT) |
| `MODIFIES SQL DATA` | Invoca sentencias que **modifican** la BD (INSERT, UPDATE, DELETE) |
| `RETURNS NULL ON NULL INPUT` | Si algún parámetro es NULL, devuelve NULL sin ejecutar |
| `CALLED ON NULL INPUT` (default) | Ejecuta la función aunque algún parámetro sea NULL |

> [!important]
> En PostgreSQL, si hay parámetros OUT/INOUT, **omitir** la cláusula `RETURNS` explicita. El compilador la genera automáticamente. Si hay un solo parámetro OUT/INOUT genera `RETURNS tipo`; si hay más de uno genera `RETURNS record`.

**Ejemplo completo -- función escalar (PostgreSQL):**

```sql
CREATE OR REPLACE FUNCTION reverse2(IN cartel VARCHAR)
RETURNS VARCHAR
AS $$
DECLARE
    rta VARCHAR(100) DEFAULT '';
    len INT;
BEGIN
    len := LENGTH(cartel);
    WHILE (len > 0) LOOP
        rta := rta || substr(cartel, len, 1);
        len := len - 1;
    END LOOP;
    RETURN rta;
END;
$$ LANGUAGE plpgsql
RETURNS NULL ON NULL INPUT;
```

**Ejemplo -- función con múltiples salidas (RECORD):**

```sql
CREATE OR REPLACE FUNCTION simplifiedfraction(
    INOUT numerator integer,
    INOUT denominator integer)
AS $$
DECLARE
    auxi INT;
BEGIN
    auxi := MCD(numerator, denominator);
    numerator := numerator / auxi;
    denominator := denominator / auxi;
END;
$$ LANGUAGE plpgsql;
-- El compilador genera: RETURNS record
```

**Destrucción:**

```sql
DROP FUNCTION nombreFuncion [ (tipo, tipo, ...) ];
```

---

### CREATE PROCEDURE

El **procedimiento** no devuelve valor en su nombre. Permite parámetros **IN**, **OUT** e **INOUT**. Los parámetros actuales de tipo OUT/INOUT deben ser **lvalues**.

**Sintaxis ANSI:**

```sql
CREATE PROCEDURE nombreProcedimiento ( [IN|OUT|INOUT] param tipo, ... )
    LANGUAGE SQL
    [ DETERMINISTIC | NOT DETERMINISTIC ]
    [ NO SQL | CONTAINS SQL | READS SQL | MODIFIES SQL DATA ]
    [ DYNAMIC RESULT SETS ]
BEGIN
    sentencias_SQL;
END
```

**Invocación ANSI:**

```sql
CALL nombreProc([parametros]);
```

**Ejemplo ANSI:**

```sql
CREATE PROCEDURE ratio (IN titulo VARCHAR(100),
                        IN largo INT, OUT rta REAL)
    LANGUAGE SQL
    DETERMINISTIC
    CONTAINS SQL
SET rta = CHAR_LENGTH(titulo) / largo;
```

> [!tip]
> En **PostgreSQL** no existe `PROCEDURE`. Para emularlo se usa una `FUNCTION` que devuelve `VOID` con parámetros solo `IN`. Se invoca con `PERFORM nombreFuncionVoid(params)` desde dentro de otro PSM.

---

### Declaración de variables y constantes

Las variables se declaran en la zona `DECLARE`, **antes** del bloque `BEGIN` (en PostgreSQL) o **dentro** del bloque (en ANSI).

**Variables:**

```sql
DECLARE
    vble1 tipo1 DEFAULT valor1;   -- ANSI
    vble2 tipo2 := valor2;        -- PostgreSQL (tambien = valor2)
```

**Constantes (solo PostgreSQL):**

```sql
DECLARE
    PI CONSTANT FLOAT := 3.14159;
    -- Inicializar siempre: si se omite contendra NULL y no se puede asignar despues
```

**Atributos %TYPE y %ROWTYPE (PostgreSQL):**

```sql
DECLARE
    v_nombre empleado.nombre%TYPE;   -- hereda el tipo de la columna
    v_reg    empleado%ROWTYPE;       -- hereda la estructura de toda la fila
```

> [!important]
> Las variables no inicializadas arrancan en **NULL**. Operar con NULL produce NULL: `cont := cont + 1` deja `cont` en NULL si no fue inicializada. Siempre inicializar con `DEFAULT` o `:=`.

> [!tip]
> Usar `%TYPE` y `%ROWTYPE` reduce el costo de mantenimiento: si cambian los tipos en las tablas, las variables del PSM se adaptan automáticamente en tiempo de ejecución.

---

### Asignación de valores

| ANSI | PostgreSQL |
|---|---|
| `SET vble = valor;` | `vble := valor;` o `vble = valor;` |

---

### Sentencias de control de flujo

#### Decisión: IF

```sql
-- ANSI
IF condicion1 THEN
    sentencias1;
ELSEIF condicion2 THEN
    sentencias2;
ELSE
    sentenciasN;
END IF;

-- PostgreSQL (ELSIF en vez de ELSEIF)
IF condicion1 THEN
    sentencias1;
ELSIF condicion2 THEN
    sentencias2;
ELSE
    sentenciasN;
END IF;
```

#### Decisión: CASE

```sql
-- Forma 1: CASE simple
CASE expresion
    WHEN cte1 THEN sentencia1;
    WHEN cte2 THEN sentencia2;
    ELSE sentenciaN;
END CASE;

-- Forma 2: CASE buscado
CASE
    WHEN exp_booleana1 THEN sentencia1;
    WHEN exp_booleana2 THEN sentencia2;
    ELSE sentenciaN;
END CASE;
```

#### Repetición: WHILE

```sql
-- ANSI
WHILE condicion DO
    sentencias;
END WHILE;

-- PostgreSQL
WHILE condicion LOOP
    sentencias;
END LOOP;
```

#### Repetición: LOOP (infinito, se sale con EXIT)

```sql
LOOP
    sentencias;
    EXIT WHEN condicion;  -- PostgreSQL
END LOOP;
```

#### Repetición: FOR (solo PostgreSQL)

```sql
FOR vble IN [REVERSE] limiteInf .. limiteSup [BY step] LOOP
    sentencias;
END LOOP;
```

#### Repetición: REPEAT (solo ANSI)

```sql
REPEAT
    sentencias;
UNTIL condicion
END REPEAT;
```

**Tabla comparativa de repetición:**

| Sentencia | ANSI | PostgreSQL |
|---|---|---|
| `LOOP ... END LOOP` | Si | Si |
| `WHILE ... DO/LOOP` | `WHILE cond DO ... END WHILE` | `WHILE cond LOOP ... END LOOP` |
| `FOR` | No | `FOR v IN a..b LOOP ... END LOOP` |
| `REPEAT ... UNTIL` | Si | No |
| Salir de un ciclo | `LEAVE rotulo;` | `EXIT;` / `EXIT WHEN cond;` |

> [!tip]
> PostgreSQL ofrece `RAISE NOTICE '% %', var1, var2;` para imprimir en la salida estandar (similar a `printf` pero sin especificar tipos, solo `%`).

---

### Manejo de excepciones y condiciones

Luego de cada sentencia SQL, el DBMS comunica el resultado a traves del **SQLCA** (SQL Communication Area) con dos variables: **SQLCODE** y **SQLSTATE**.

| Estado | SQLCODE | SQLSTATE (primeros 2 chars) |
|---|---|---|
| OK (exitosa) | 0 | 00 |
| NOT FOUND | 100 | 02 |
| SQLWARNING | >0 y <>100 | 01 |
| SQLEXCEPTION | <0 | Cualquier otra |

> [!important]
> Los valores concretos de `SQLSTATE` y `SQLCODE` **no son coincidentes** entre distintos DBMS, salvo los prefijos. No asumir portabilidad de códigos específicos.

#### Manejador ANSI (DECLARE HANDLER)

ANSI diferencia entre **condiciones** (NOT FOUND, SQLWARNING -- casos normales) y **excepciones** (violaciones de restricciones -- casos anormales). Si no hay manejador para una condición, continúa la ejecución. Si no hay manejador para una excepción, **termina** el PSM inmediatamente.

```sql
-- Sintaxis ANSI
DECLARE { CONTINUE | EXIT | UNDO }
    HANDLER FOR { SQLEXCEPTION | SQLWARNING | NOT FOUND | SQLSTATE 'xxxxx' }
    sentencias;
```

- **CONTINUE**: sigue con la siguiente sentencia a la que produjo el error
- **EXIT**: sale del bloque que contiene la sentencia problemática
- **UNDO**: igual que EXIT pero hace **ROLLBACK** de todas las sentencias SQL del bloque

**Alias para condiciones:**

```sql
DECLARE mi_error CONDITION FOR SQLSTATE '23505';

DECLARE EXIT HANDLER FOR mi_error
    SET error = SQLSTATE;
```

**Lanzar excepciones propias (ANSI):**

```sql
SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Sueldo excede al del gerente';
-- RESIGNAL: solo desde dentro de un manejador, redefine la excepcion
RESIGNAL SQLSTATE '45001' SET MESSAGE_TEXT = 'Error redefinido';
```

#### Manejador PostgreSQL (EXCEPTION)

PostgreSQL **no diferencia** entre condiciones y excepciones: todo es una excepción. La zona `EXCEPTION` va siempre **al final** del bloque, justo antes del `END`.

```sql
BEGIN
    -- sentencias
EXCEPTION
    WHEN division_by_zero THEN
        RAISE NOTICE '% %', SQLSTATE, SQLERRM;
    WHEN unique_violation THEN
        RAISE NOTICE 'PK duplicada';
    WHEN OTHERS THEN
        RAISE NOTICE 'Error inesperado: % %', SQLSTATE, SQLERRM;
END;
```

**Algoritmo PostgreSQL ante una excepción:**

1. **Deshace** (rollback) todas las sentencias SQL realizadas en el bloque donde se produjo la excepción
2. Busca manejador en el bloque:
   - **Si lo hay**: le transfiere el control y la ejecución continúa normal
   - **Si no lo hay y hay bloque externo**: **propaga** la excepción al bloque que lo contiene (que también hará rollback de sus sentencias SQL)
   - **Si no lo hay y es el bloque más externo**: termina anormalmente y relanza la excepción al invocador

> [!important]
> En PostgreSQL, `SQLSTATE` y `SQLERRM` **solo pueden consultarse** dentro de la cláusula `EXCEPTION`.

**Lanzar excepciones propias (PostgreSQL):**

```sql
-- Relanzar la misma excepcion recibida
RAISE division_by_zero;

-- Lanzar una nueva excepcion personalizada
RAISE EXCEPTION 'Sueldo excede limite'
    USING ERRCODE = '45000';
```

> [!tip]
> **Atrapar excepciones donde se producen** para evitar propagación. No mezclar sentencias SQL que no lanzan excepción con otras que sí en el mismo bloque, porque el rollback deshace **todas** las sentencias SQL del bloque.

---

### Bloques y scope

Un PSM está formado por bloques `BEGIN/END`. Los bloques pueden **anidarse** para definir menor alcance de variables (scope).

**Estructura ANSI vs PostgreSQL:**

| ANSI | PostgreSQL |
|---|---|
| `BEGIN` | `DECLARE ... BEGIN` |
| Declaraciones al inicio del bloque | Declaraciones **antes** del bloque |
| Excepciones dentro del bloque | `EXCEPTION` **al final** del bloque |
| `END;` | `END;` |

**Template de bloques separados para manejo independiente de errores:**

```sql
CREATE OR REPLACE FUNCTION mi_funcion(...)
RETURNS VARCHAR AS $$
BEGIN
    -- Bloque aislado para operacion 1
    BEGIN
        INSERT INTO tabla1 VALUES (...);
    EXCEPTION
        WHEN OTHERS THEN
            RAISE NOTICE 'Error en tabla1, continuo';
    END;

    -- Bloque aislado para operacion 2
    BEGIN
        INSERT INTO tabla2 VALUES (...);
    EXCEPTION
        WHEN OTHERS THEN
            RAISE NOTICE 'Error en tabla2, continuo';
    END;

    RETURN 'OK';
END;
$$ LANGUAGE plpgsql;
```

> [!tip]
> Demasiados bloques anidados complican la lectura. Es más prolijo definir PSMs separados e invocarlos. La clave es detectar **cuáles sentencias SQL funcionan como un todo** (si falla una, deben deshacerse todas juntas) -- esas van en el mismo bloque/PSM.

---

## Notas

- El `SELECT INTO` dentro de un PSM asigna el resultado de un query a variables locales. No confundir con el `SELECT INTO` que crea tablas.
- PostgreSQL ignora los topes de longitud para `CHAR` y `VARCHAR` en parámetros formales y retorno: toman la extensión del parámetro actual.
- `PERFORM` en PostgreSQL reemplaza a `SELECT` cuando no se necesita el resultado (típico para invocar funciones VOID).
- La palabra clave en PostgreSQL para el condicional es `ELSIF` (no `ELSEIF` como en ANSI).
- `DO $$ ... $$;` permite ejecutar bloques anónimos en PostgreSQL sin crear una función persistente. Útil para pruebas.

## Preguntas

- En PostgreSQL, si una excepción se propaga desde un bloque interno a uno externo y este último tiene un `EXCEPTION WHEN OTHERS`, el rollback afecta las sentencias SQL del bloque externo tambien?
- Si se declara un parámetro INOUT en PostgreSQL pero el pasaje es por valor, el parámetro actual del invocador nunca se modifica. Entonces, cual es la diferencia práctica entre OUT e INOUT en PostgreSQL?
- Cuando se usa `RETURNS NULL ON NULL INPUT` y la función tiene múltiples parámetros, basta con que **uno** sea NULL para que devuelva NULL?
- Los bloques anónimos (`DO $$...$$`) pueden hacer rollback parcial con bloques anidados de la misma forma que las funciones?
