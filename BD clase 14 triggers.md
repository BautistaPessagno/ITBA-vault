---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - triggers
Created: 2026-03-0314:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Triggers (disparadores)
  - Restricciones SQL2 vs SQL3
  - CREATE TRIGGER sintaxis
  - BEFORE vs AFTER vs INSTEAD OF
  - FOR EACH ROW vs FOR EACH STATEMENT
  - Variables OLD y NEW
  - Trigger functions en PostgreSQL
  - Assertions
  - Auditoría con triggers
  - Trigger cascading
---

# BD clase 14 — Triggers

## Resumen

### Restricciones en SQL2 vs SQL3

El modelo relacional ofrece restricciones de **dominio**, **clave** y **datos** (integridad de entidad, referencial, dependencias funcionales). SQL2 responde a una operacion invalida **rechazandola**. SQL3 introduce una novedad: permite llevar a la base de datos a un **estado compatible** con la restriccion, ejecutando acciones antes o despues de la operacion solicitada.

| Aspecto | SQL2 | SQL3 |
|---|---|---|
| Estrategia | Rechaza operacion invalida | Ejecuta acciones para mantener consistencia |
| Mecanismo | CHECK, PK, FK, UNIQUE, DEFAULT | Triggers (disparadores) |
| Alcance | Restriccion de estado | Restriccion + accion automatica |
| Evaluacion CHECK | Solo en INSERT/UPDATE | N/A (usa triggers) |
| Subqueries en CHECK | ANSI si, PostgreSQL **no** | N/A |

> [!important]
> Los triggers son **mas costosos** en ejecucion que las restricciones de SQL2 (PK, FK, DEFAULT, NOT NULL). No definir triggers para lo que puede hacerse con SQL2.

### Assertions (SQL2 — nivel esquema)

Una **assertion** es una restriccion a nivel esquema que, a diferencia de los CHECK, **garantiza la consistencia en todo momento** y no solo en cada insercion o actualizacion. Se evalua ante **cualquier operacion** sobre las tablas involucradas.

```sql
-- Sintaxis generica
CREATE ASSERTION nombre_assertion CHECK (expresion);

-- Ejemplo: ningun empleado no-gerente puede superar el sueldo de un gerente
CREATE ASSERTION SueldoTope
CHECK ( NOT EXISTS (
    SELECT * FROM empleado
    WHERE cargo <> 'Gerente'
    AND sueldo > ANY (SELECT sueldo FROM empleado WHERE cargo = 'Gerente')
));

-- Ejemplo: no mas secretarias que vendedores
CREATE ASSERTION NoDemasiadasSecretarias
CHECK (
    (SELECT COUNT(*) FROM empleado WHERE cargo = 'Secretaria')
    <=
    (SELECT COUNT(*) FROM empleado WHERE cargo = 'Vendedor')
);
```

> [!important]
> PostgreSQL **no implementa ASSERTIONS**. Las reemplaza por **triggers**.

### Que es un trigger

Un **trigger** (disparador o activador) es un **elemento activo** almacenado en la base de datos que se ejecuta automaticamente cuando ocurre un evento especificado. Es la implementacion de **Sistemas de Bases de Datos Activos** (Active Database Systems): reglas ECA (Evento-Condicion-Accion).

El programador debe especificar:
1. **Evento**: INSERT, DELETE o UPDATE (los que modifican datos)
2. **Condicion** (opcional): clausula WHEN que filtra cuando ejecutar la accion
3. **Accion**: codigo que se ejecuta si el evento ocurre y la condicion es verdadera

Ventajas:
- **Codigo compilado**: no se pierde tiempo parseando en cada ejecucion
- **Ya testeado**: menor probabilidad de error
- **Centralizado**: reduce trafico entre aplicaciones y servidor
- **Seguridad**: el usuario que dispara el trigger no necesita permisos sobre las tablas que el trigger modifica, solo sobre la tabla del evento

> [!tip]
> Casos de uso tipicos: **auditoria**, **atributos derivados**, **reglas complejas de negocio**, **actualizacion de vistas complejas**.

### CREATE TRIGGER — Sintaxis

**Sintaxis ANSI generica:**

```sql
CREATE TRIGGER nombreTrigger
{ BEFORE | AFTER }
{ INSERT | DELETE | UPDATE [OF listaAtributos] } ON nombreTabla
[ REFERENCING OLD AS tuplaVieja NEW AS tuplaNueva
              OLD TABLE AS tablaVieja NEW TABLE AS tablaNueva ]
[ FOR EACH { ROW | STATEMENT } ]
[ WHEN (condicion) ]
BEGIN ATOMIC
    variasAcciones;
END;
```

**Sintaxis PostgreSQL:**

```sql
-- Crear la trigger function primero
CREATE OR REPLACE FUNCTION mi_funcion() RETURNS TRIGGER
AS $$
BEGIN
    -- acciones del trigger
    RETURN NEW;  -- o RETURN OLD, o RETURN NULL
END;
$$ LANGUAGE plpgsql;

-- Luego crear el trigger
CREATE TRIGGER nombreTrigger
{ BEFORE | AFTER | INSTEAD OF }
{ INSERT | DELETE | UPDATE [OF listaAtributos] } ON nombreTabla
[ FOR EACH { ROW | STATEMENT } ]
[ WHEN (condicion) ]
EXECUTE PROCEDURE mi_funcion();

-- Destruccion
DROP TRIGGER nombreTrigger ON nombreTabla;
```

> [!important]
> En PostgreSQL las reglas **no se colocan en el cuerpo del trigger**. Se invoca un PSM especial llamado **trigger function** que debe declarar `RETURNS TRIGGER`. Esta funcion **no puede declarar parametros formales**.

### BEFORE vs AFTER vs INSTEAD OF

| Timing | Orden de ejecucion | Puede modificar NEW? | Puede cancelar con RETURN NULL? | Uso con |
|---|---|---|---|---|
| **BEFORE** | Accion del trigger → Evento | Si (row-level) | Si (row-level) | Solo **tablas** |
| **AFTER** | Evento → Accion del trigger | No (ya se ejecuto el evento) | No (valor de retorno ignorado) | Solo **tablas** |
| **INSTEAD OF** | Solo la accion (reemplaza el evento) | N/A | Si | Solo **vistas** |

**Cuando usar cada uno:**

| Situacion | Timing recomendado |
|---|---|
| Validar/modificar datos antes de que se persistan | **BEFORE** |
| Convertir a mayusculas, ajustar valores | **BEFORE** |
| Registrar auditoria despues de un cambio confirmado | **AFTER** |
| Operaciones sobre otras tablas que dependen del estado post-evento | **AFTER** |
| Hacer actualizables vistas complejas (con JOIN, GROUP BY, etc.) | **INSTEAD OF** |
| El trigger modifica/borra tuplas de la **misma tabla** del evento | **AFTER** (PostgreSQL exige esto) |

> [!important]
> Si el cuerpo del trigger necesita hacer DELETE/UPDATE sobre la **misma tabla** del evento, PostgreSQL **solo permite AFTER**. Con BEFORE podria generar inconsistencias.

### FOR EACH ROW vs FOR EACH STATEMENT

| Aspecto | FOR EACH ROW | FOR EACH STATEMENT |
|---|---|---|
| Ejecucion | Una vez **por cada tupla** afectada | Una sola vez **por sentencia SQL** |
| Variables OLD/NEW | **Disponibles** (referencian la tupla individual) | **No disponibles** (error en tiempo de ejecucion si se usan) |
| Default | No | **Si** (es la opcion por defecto) |
| Uso tipico | Validacion por tupla, auditoria detallada, modificar valores | Auditoria global, conteos, acciones que no dependen de tuplas individuales |
| Con INSTEAD OF | Obligatorio (unico permitido) | No se puede usar |

```sql
-- Row-level: se ejecuta N veces si el UPDATE afecta N tuplas
CREATE TRIGGER auditar_cada_fila
AFTER UPDATE ON empleado
FOR EACH ROW
EXECUTE PROCEDURE fn_auditoria();

-- Statement-level: se ejecuta 1 sola vez sin importar cuantas tuplas se afecten
CREATE TRIGGER auditar_sentencia
BEFORE INSERT ON empleado
FOR EACH STATEMENT
EXECUTE PROCEDURE fn_auditoria_global();
```

> [!tip]
> Si necesitas acceder a `OLD` o `NEW`, **obligatoriamente** debes usar `FOR EACH ROW`. Intentar acceder a estas variables en un statement-level trigger produce un **error en tiempo de ejecucion**.

### Referencias NEW y OLD

Las variables `NEW` y `OLD` son **registros** que representan la tupla nueva y vieja respectivamente. Su disponibilidad depende del evento y nivel del trigger:

| Evento | OLD | NEW |
|---|---|---|
| **INSERT** | No disponible | Tupla que se va a insertar |
| **UPDATE** | Tupla antes del cambio | Tupla despues del cambio |
| **DELETE** | Tupla que se va a borrar | No disponible |

En **statement-level triggers**: ni `OLD` ni `NEW` estan disponibles (error en runtime).

```sql
-- Ejemplo: acceder a valores previos y nuevos en un UPDATE
CREATE OR REPLACE FUNCTION log_cambios() RETURNS TRIGGER AS $$
BEGIN
    raise notice 'Antes: %', OLD;
    raise notice 'Despues: %', NEW;
    -- Acceder a campos individuales
    raise notice 'Sueldo viejo: %, Sueldo nuevo: %', OLD.sueldo, NEW.sueldo;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

> [!important]
> En un BEFORE DELETE row-level trigger, si haces `RETURN NEW`, como NEW es NULL en un DELETE, **se cancela el borrado** (se interpreta como RETURN NULL). Siempre usar `RETURN OLD` (o cualquier valor no NULL) en triggers de DELETE para que el evento se ejecute.

### Valor de retorno de la trigger function

El valor que retorna la trigger function tiene **significado diferente** segun el contexto:

**Row-level BEFORE / INSTEAD OF:**

| Retorno | Efecto |
|---|---|
| `RETURN NEW` (o valor no NULL) | Terminacion **normal**: se ejecuta el evento con el valor retornado |
| `RETURN NULL` | Terminacion **normal** pero **salta** el evento y triggers subsiguientes para esa tupla |
| Excepcion no atrapada | Terminacion **anormal**: ROLLBACK de todo (accion + evento) |

**Row-level AFTER:**

| Retorno | Efecto |
|---|---|
| Cualquier valor (NULL o no) | Se **ignora** (el evento ya se ejecuto) |
| Excepcion no atrapada | **ROLLBACK** de todo (accion + evento) |

**Statement-level (cualquier timing):**

| Retorno | Efecto |
|---|---|
| Cualquier valor (NULL o no) | Se **ignora** |
| Excepcion no atrapada | **ROLLBACK** de todo |

> [!tip]
> Para **forzar el fallo** de un trigger y deshacer todo: lanzar una excepcion que no sea atrapada. Esto funciona en todos los casos: `RAISE EXCEPTION 'mensaje' USING ERRCODE = 'PP111';`

### Variables de entorno del trigger (PostgreSQL)

Ademas de `OLD` y `NEW`, la trigger function recibe:

| Variable | Descripcion |
|---|---|
| `TG_NARGS` | Cantidad de parametros actuales pasados al trigger |
| `TG_ARGV[]` | Arreglo con los parametros (llegan como texto). Acceso fuera de limites devuelve NULL |
| `TG_OP` | Operacion que disparo el trigger: `'INSERT'`, `'UPDATE'`, `'DELETE'` |
| `TG_TABLE_NAME` | Nombre de la tabla asociada al trigger |
| `TG_WHEN` | `'BEFORE'`, `'AFTER'` o `'INSTEAD OF'` |

### Trigger cascading y recursion

Un trigger puede disparar otro trigger si su accion realiza operaciones DML sobre tablas que tienen triggers asociados. Sin embargo:

- PostgreSQL **no permite triggers recursivos directos**: no se puede hacer `INSERT INTO tablaA` dentro de un trigger cuyo evento es `INSERT ON tablaA` (produce **stack overflow**).
- La recursion **indirecta** tambien puede ocurrir por error (trigger A modifica tabla B, trigger de B modifica tabla A).

```sql
-- INCORRECTO: produce recursion infinita
CREATE OR REPLACE FUNCTION mal_trigger() RETURNS TRIGGER AS $$
BEGIN
    -- Esto dispara el mismo trigger infinitamente
    INSERT INTO empleado VALUES (NEW.legajo, NEW.nombre, NEW.sueldo);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

> [!important]
> Un trigger **no puede contener** sentencias transaccionales (`COMMIT`, `ROLLBACK`). El trigger forma parte de la transaccion que lo disparo. Si falla, se deshace **todo** (evento + accion).

### Restricciones de PostgreSQL

- **BEFORE/AFTER**: solo con tablas (no vistas)
- **INSTEAD OF**: solo con vistas (no tablas), no permite WHEN, solo FOR EACH ROW
- Clausula **WHEN**: no puede contener subqueries ni invocar PSMs
- Se pueden combinar **multiples eventos** en un trigger: `INSERT OR UPDATE ON tabla`
- **No crear** triggers para restricciones expresables con PK, FK, CHECK, DEFAULT o NOT NULL

### Patrones comunes

**Auditoria:**

```sql
CREATE OR REPLACE FUNCTION fn_auditoria() RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO auditoria VALUES (current_timestamp, user);
    RETURN NEW;  -- BEFORE: hacer efectivo el evento
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_audit
BEFORE INSERT ON empleado
FOR EACH ROW
EXECUTE PROCEDURE fn_auditoria();
```

**Validacion con rechazo (regla de negocio):**

```sql
CREATE OR REPLACE FUNCTION fn_validar_sueldo() RETURNS TRIGGER AS $$
DECLARE
    max_sueldo FLOAT;
BEGIN
    SELECT MAX(sueldo) INTO max_sueldo FROM empleado;
    IF (NEW.sueldo > max_sueldo) THEN
        RAISE EXCEPTION 'SUELDO MUY ALTO' USING ERRCODE = 'PP111';
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_validar
BEFORE INSERT ON empleado
FOR EACH ROW
EXECUTE PROCEDURE fn_validar_sueldo();
```

**Transformacion de datos (BEFORE + modificar NEW):**

```sql
CREATE OR REPLACE FUNCTION fn_uppercase() RETURNS TRIGGER AS $$
BEGIN
    NEW.nombre := UPPER(NEW.nombre);
    RETURN NEW;  -- el valor retornado es el que se persiste
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_upper
BEFORE INSERT OR UPDATE ON empleado
FOR EACH ROW
EXECUTE PROCEDURE fn_uppercase();
```

**Insercion automatica en otra tabla (caso INSCRIPTO → EXAMEN):**

```sql
CREATE OR REPLACE FUNCTION fn_acta_final() RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO examen (legajo, codMateria, fecha)
    VALUES (NEW.legajo, NEW.codMateria, NEW.fecha);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_aprobado
BEFORE UPDATE OF fecha, nota ON inscripto
FOR EACH ROW
WHEN (NEW.nota >= 4)
EXECUTE PROCEDURE fn_acta_final();
```

**Actualizacion de vistas complejas (INSTEAD OF):**

```sql
CREATE OR REPLACE FUNCTION fn_borrar_caros() RETURNS TRIGGER AS $$
BEGIN
    DELETE FROM provee
    WHERE nombre = OLD.nombre AND precio = OLD.maximoPrecio;
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_vista
INSTEAD OF DELETE ON maximal
FOR EACH ROW
EXECUTE PROCEDURE fn_borrar_caros();
```

> [!tip]
> Sobre **seguridad**: el creador (owner) del trigger necesita permisos sobre las tablas de las acciones, pero los usuarios que lo disparan **no necesitan permisos** sobre esas tablas — solo sobre la tabla del evento. Esto permite automatizar operaciones sin dar permisos explicitos.

## Notas

- El CHECK con SELECT dentro **no es equivalente** a una FK: el CHECK solo se evalua en INSERT/UPDATE, pero si luego se borra la tupla referenciada, no se detecta la inconsistencia. Siempre preferir FK.
- PostgreSQL no permite subqueries en CHECK (ni a nivel atributo ni a nivel tabla).
- En la clausula WHEN del trigger se puede usar `NEW` y `OLD` pero **no subqueries ni llamadas a PSMs**.
- Cuando un UPDATE no afecta ninguna tupla (ej: `WHERE legajo = -3`), PostgreSQL detecta que es imposible y **ni siquiera transfiere el control** al trigger.
- Si un BEFORE UPDATE row-level trigger modifica `NEW` y luego retorna `NEW`, el **valor modificado** es el que se persiste en la tabla (no el original del UPDATE).
- Con AFTER, el retorno se ignora y **no se pueden modificar los datos** que ya fueron escritos. La unica opcion es hacer ROLLBACK lanzando excepcion.

## Preguntas

1. Si tengo un BEFORE INSERT trigger que retorna NULL, la insercion en auditoria dentro del trigger se mantiene pero el INSERT original no se ejecuta. En que situaciones es util este comportamiento "parcial" que no es atomico como en ANSI?
2. Dado que PostgreSQL no implementa ASSERTIONS, como se implementaria un trigger equivalente a la assertion "el promedio de sueldos por cargo no puede superar 5000"? Habria que poner triggers en INSERT, UPDATE y DELETE sobre empleado?
3. Si hay multiples triggers BEFORE sobre la misma tabla y evento, en que orden se ejecutan? El resultado de uno alimenta al siguiente?
4. En un escenario de INSTEAD OF sobre una vista con JOIN de 3 tablas, como se diseña el trigger para mantener consistencia si el INSERT en la vista debe afectar a las 3 tablas subyacentes?
5. Por que PostgreSQL exige AFTER (no BEFORE) cuando el trigger modifica la misma tabla del evento? Que inconsistencia especifica se evita?

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (BD)**

- [[BD clase 13 SQL PSM]] — clase anterior
- [[BD clase 16 programacion embebida]] — clase siguiente
- [[BD clase 7 SQL DDL y DML]] — restricciones declarativas vs activas
- [[TODO]] — el TP usa triggers

<!-- notas-relacionadas:fin -->
