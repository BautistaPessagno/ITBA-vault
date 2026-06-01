---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - DDL
  - DML
Created: 2026-03-0314:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Tipos de datos SQL
  - CREATE TABLE y restricciones
  - Claves primarias, foráneas y candidatas
  - ON DELETE / ON UPDATE en FK
  - Referencias circulares con ALTER TABLE
  - ALTER TABLE y DROP TABLE
  - INSERT, DELETE, UPDATE
  - Manejo de NULL y lógica trivaluada
  - Consultas SELECT y cláusulas
  - Funciones de agregación
  - GROUP BY y HAVING
---

# BD clase 7 — SQL DDL y DML

## Resumen

SQL (Structured Query Language) es un hibrido entre **algebra relacional** y **calculo relacional**. Desde la sintaxis se parece al calculo relacional; desde la implementacion los motores DBMS usan algebra relacional para optimizar. El estandar vigente es **SQL-3** (SQL-99), con extensiones en 2003, 2006 y 2008.

SQL se divide en dos grandes grupos de sentencias:
- **DDL** (Data Definition Language): definicion de esquemas, tablas, vistas, dominios, restricciones.
- **DML** (Data Manipulation Language): insercion, borrado, actualizacion y consulta de tuplas.

---

### Tipos de datos SQL

Elegir el tipo de dato correcto establece la **restriccion de dominio**, limita las operaciones permitidas e influye en el espacio en disco.

#### Tipos caracter

| Tipo | Descripcion |
|---|---|
| `CHAR(n)` / `CHARACTER(n)` | String de longitud **fija**. Si no se indica `n`, se asume 1. Completa con blancos si el valor es mas corto. |
| `VARCHAR(n)` / `CHAR VARYING(n)` | String de longitud **variable** con tope `n`. |

#### Tipos numericos

| Tipo                           | Descripcion                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------ |
| `INTEGER` / `INT`              | Entero, rango segun arquitectura.                                              |
| `SMALLINT`                     | Entero mas pequeno.                                                            |
| `BIGINT`                       | Entero grande (64 bits en DB2, no existe en Oracle).                           |
| `DECIMAL(p,s)` / `NUMBER(p,s)` | Punto fijo. `p` = precision total (digitos + signo), `s` = escala (decimales). |
| `FLOAT(p)`                     | Punto flotante con precision indicada.                                         |
| `REAL`                         | Punto flotante simple, dependiente de la maquina.                              |
| `DOUBLE`                       | Punto flotante doble precision.                                                |

#### Tipos temporales

| Tipo | Descripcion |
|---|---|
| `DATE` | Fecha (anio, mes, dia). |
| `TIME(fraction)` | Hora, minutos, segundos. No existe en Oracle. |
| `TIMESTAMP` | Fecha + hora con microsegundos. |
| `INTERVAL` | Duracion de un periodo. En DB2 no es un tipo propio (la resta de fechas da `DECIMAL`). |

> [!tip] Funciones temporales utiles
> - `current_date`, `current_time`, `current_timestamp` -- devuelven fecha/hora actual.
> - `EXTRACT(parte FROM expresion_temporal)` -- extrae anio, mes, dia, etc. Devuelve numerico.

#### Operadores segun tipo

- **Strings**: concatenacion con `||`
- **Numericos**: `+`, `-`, `*`, `/`
- **Temporales**: `+`, `-` (la resta de dos temporales devuelve un intervalo)

---

### CREATE TABLE — Sintaxis completa

La sentencia central del DDL. Define una tabla persistente con sus columnas y restricciones.

```sql
CREATE TABLE nombre_tabla
(
    col1  tipo1  [DEFAULT val1]  [restriccion_col1],
    col2  tipo2  [DEFAULT val2]  [restriccion_col2],
    ...
    colN  tipoN  [DEFAULT valN]  [restriccion_colN],

    -- Restricciones globales (de tabla)
    [CONSTRAINT nombre_pk] PRIMARY KEY (lista_atributos),
    [CONSTRAINT nombre_uq] UNIQUE (lista_atributos),
    [CONSTRAINT nombre_fk] FOREIGN KEY (lista_local)
        REFERENCES tabla_ref [(lista_ref)]
        [ON DELETE accion] [ON UPDATE accion],
    [CONSTRAINT nombre_ck] CHECK (condicion)
);
```

**Restricciones a nivel de columna** (inline):
- `NOT NULL`
- `PRIMARY KEY` (solo si la PK es un unico atributo)
- `UNIQUE`
- `CHECK (condicion)`
- `REFERENCES tabla_ref` (FK inline)

**Restricciones a nivel de tabla** (globales):
- `PRIMARY KEY (col1, col2, ...)` -- obligatoria cuando la PK es compuesta
- `UNIQUE (col1, col2, ...)`
- `FOREIGN KEY (cols_locales) REFERENCES tabla (cols_ref)`
- `CHECK (condicion)`

> [!important] Representar TODAS las claves candidatas
> - Una cualquiera como `PRIMARY KEY`.
> - Todas las demas como `UNIQUE` **con `NOT NULL`** en cada atributo que las integra.
> - `PRIMARY KEY` sobre una lista de atributos es **logicamente equivalente** a `UNIQUE` + `NOT NULL` en cada uno de esos atributos.
> - Si se omite `NOT NULL` en las columnas de un `UNIQUE`, no es equivalente a PK porque los NULL violarian la **restriccion de integridad de entidad**.

> [!tip] DEFAULT
> Si no se especifica `DEFAULT`, el valor por defecto implicito es `NULL`. Si la columna es `NOT NULL` y no tiene default, insertar sin valor produce error.

---

### Claves foraneas — ON DELETE / ON UPDATE

Las **foreign keys** permiten especificar acciones automaticas cuando se borra o actualiza la tupla referenciada.

```sql
FOREIGN KEY (col_local)
    REFERENCES tabla_ref (col_ref)
    ON DELETE accion
    ON UPDATE accion
```

| Accion | Efecto al borrar/actualizar la tupla referenciada |
|---|---|
| `NO ACTION` (default) / `RESTRICT` | **Rechaza** la operacion si hay tuplas que referencian. |
| `CASCADE` | **Propaga**: borra/actualiza en cascada las tuplas que referencian. |
| `SET NULL` | Pone `NULL` en los atributos de la FK de las tuplas que referencian. |
| `SET DEFAULT` | Pone el valor `DEFAULT` en los atributos de la FK. |

> [!important] Elegir la accion correcta
> La decision depende de la semantica del dominio. Preguntarse: "Si la tupla referenciada desaparece, tiene sentido que la tupla local siga existiendo?" Si la respuesta es NO, usar `CASCADE`. Si es SI, usar `SET NULL` o `NO ACTION`.

---

### Referencias circulares con ALTER TABLE

Cuando dos tablas se referencian mutuamente, no se pueden crear ambas FK en el `CREATE TABLE` porque una tabla aun no existe cuando se define la otra.

**Patron de solucion:**

```sql
-- 1. Crear la primera tabla SIN su FK hacia la segunda
CREATE TABLE departamento
(
    nombre  CHAR(50) NOT NULL,
    jefe    CHAR(100) NOT NULL,
    UNIQUE(jefe),
    PRIMARY KEY(nombre)
);

-- 2. Crear la segunda tabla CON todas sus restricciones
CREATE TABLE empleado
(
    nombre     CHAR(100) NOT NULL,
    sueldo     DECIMAL(10,2),
    trabaja_en CHAR(50),
    PRIMARY KEY(nombre),
    FOREIGN KEY(trabaja_en) REFERENCES departamento
);

-- 3. Agregar la FK faltante con ALTER TABLE
ALTER TABLE departamento
    ADD CONSTRAINT deptFK FOREIGN KEY(jefe) REFERENCES empleado;
```

> [!important] Problema de insercion con referencias cruzadas
> Si **ninguna** de las dos FK acepta NULL, no se puede insertar en ninguna tabla primero (deadlock logico). Solucion: usar **transacciones con validacion diferida** (`DEFERRABLE`), o permitir NULL en al menos una de las FK.

---

### ALTER TABLE y DROP TABLE

**ALTER TABLE** permite modificar la estructura de una tabla ya definida:

```sql
-- Agregar columna
ALTER TABLE t ADD col tipo [DEFAULT val] [restriccion];

-- Eliminar columna
ALTER TABLE t DROP col [CASCADE | RESTRICT];

-- Cambiar default
ALTER TABLE t ALTER col SET DEFAULT valor;
ALTER TABLE t ALTER col DROP DEFAULT;

-- Agregar restriccion
ALTER TABLE t ADD CONSTRAINT nombre definicion;

-- Eliminar restriccion
ALTER TABLE t DROP CONSTRAINT nombre [CASCADE | RESTRICT];
```

**DROP TABLE** elimina una tabla completa:

```sql
DROP TABLE nombre_tabla [CASCADE | RESTRICT];
```

| Opcion | Efecto |
|---|---|
| `CASCADE` | Borra tambien tablas/vistas que la referencian. |
| `RESTRICT` | Solo borra si **ninguna** otra tabla/vista la referencia. |

---

### INSERT — Insercion de tuplas

```sql
-- Insercion explicita (orden arbitrario de columnas)
INSERT INTO tabla (col1, col3) VALUES (val1, val3);

-- Insercion implicita (todas las columnas en orden del esquema)
INSERT INTO tabla VALUES (val1, val2, val3, ...);
```

- Si se omite una columna con `DEFAULT`, el DBMS usa ese default.
- Si se omite una columna sin `DEFAULT`, el DBMS inserta `NULL`.
- Si la columna es `NOT NULL` y no tiene default, se produce **error**.
- En el momento de la insercion el DBMS chequea **todas** las restricciones (dominio, PK, FK, CHECK, NOT NULL).

> [!tip] Buena practica
> Siempre listar explicitamente las columnas en el `INSERT`. Protege contra cambios futuros en el esquema.

---

### DELETE — Borrado de tuplas

```sql
-- Borrar tuplas que cumplen condicion
DELETE FROM tabla WHERE condicion;

-- Borrar TODAS las tuplas (la tabla queda vacia, no se elimina)
DELETE FROM tabla;
```

- El borrado puede disparar acciones en cascada si hay FK con `ON DELETE CASCADE`.
- Puede fallar si hay FK con `NO ACTION` y existen tuplas que referencian.

---

### UPDATE — Actualizacion de tuplas

```sql
-- Actualizar tuplas que cumplen condicion
UPDATE tabla SET col1 = val1, col2 = val2 WHERE condicion;

-- Actualizar TODAS las tuplas
UPDATE tabla SET col1 = val1;
```

- La `lista_cambios` es una lista de pares `columna = nuevo_valor`.
- Puede disparar acciones (`ON UPDATE CASCADE`, `SET NULL`, etc.).
- Se puede asignar `NULL` directamente: `SET col = NULL`.

---

### Manejo de NULL y logica trivaluada

SQL usa **logica de tres valores**: `TRUE`, `FALSE`, `UNKNOWN`.

Cualquier comparacion con NULL produce `UNKNOWN`, que en un `WHERE` se trata como `FALSE`:

```sql
-- MAL: nunca se evalua como TRUE
WHERE col = NULL
WHERE col <> NULL

-- BIEN: usar predicados especiales
WHERE col IS NULL
WHERE col IS NOT NULL
```

| Operacion | Resultado |
|---|---|
| `NULL = NULL` | UNKNOWN (tratado como FALSE en WHERE) |
| `NULL <> NULL` | UNKNOWN (tratado como FALSE en WHERE) |
| `NULL > 5` | UNKNOWN |
| `TRUE AND UNKNOWN` | UNKNOWN |
| `TRUE OR UNKNOWN` | TRUE |
| `NOT UNKNOWN` | UNKNOWN |

> [!important] NULL en asignacion vs. en condicion
> - En **condiciones** (WHERE): nunca usar `= NULL`, siempre `IS NULL`.
> - En **asignaciones** (INSERT/UPDATE): se asigna directamente `SET col = NULL` o `VALUES (NULL)`.
> - En **GROUP BY**: los NULL se consideran **equivalentes** y se agrupan juntos (excepcion a la regla general).
> - En **funciones de agregacion** (excepto `COUNT(*)`): los NULL se **descartan** antes de calcular. Si el conjunto queda vacio, devuelven NULL (excepto `COUNT` que devuelve 0).

---

### SELECT — Recupero de tuplas

SQL trabaja con **multiconjuntos** (bags), no conjuntos: no elimina duplicados salvo que se pida explicitamente.

```sql
SELECT [DISTINCT] col1 [AS alias1], ..., colN [AS aliasN]
FROM   lista_tablas
[WHERE condicion]
[GROUP BY lista_cols_agrupamiento
    [HAVING condicion_grupal]]
[ORDER BY lista_cols [ASC | DESC]]
```

**Orden real de evaluacion del DBMS** (no coincide con el orden de escritura):

1. `FROM` -- producto cartesiano / joins
2. `WHERE` -- filtro de tuplas individuales
3. `GROUP BY` -- armado de subgrupos
4. `HAVING` -- filtro de grupos
5. `ORDER BY` -- ordenamiento
6. `SELECT` -- proyeccion final

> [!tip] Consecuencia del orden de evaluacion
> Un alias definido con `AS` en el `SELECT` **no puede usarse** en `WHERE`, `GROUP BY` ni `HAVING` (porque se evalua ultimo). Si se puede usar en `ORDER BY` en algunos DBMS.

#### Tipos de JOIN en la clausula FROM

| Sintaxis SQL | Equivalente en algebra relacional |
|---|---|
| `r, s` o `r CROSS JOIN s` | Producto cartesiano (X) |
| `r [INNER] JOIN s ON cond` | Theta join (junta con condicion) |
| `r NATURAL JOIN s` | Junta natural (igualdad por nombre de atributos, sin duplicar columnas) |
| `r LEFT OUTER JOIN s ON cond` | Semijunta: conserva tuplas de **r** sin correspondencia (completa con NULL) |
| `r RIGHT OUTER JOIN s ON cond` | Semijunta: conserva tuplas de **s** sin correspondencia |
| `r FULL OUTER JOIN s ON cond` | Conserva tuplas de **ambas** relaciones sin correspondencia |

#### Operadores en WHERE

- **Comparacion**: `<`, `<=`, `>`, `>=`, `=`, `<>`
- **Logicos**: `AND`, `OR`, `NOT` (usar parentesis para evitar ambiguedades)
- **Patron**: `LIKE` con comodines `%` (cualquier substring) y `_` (un caracter)
- **Pertenencia**: `IN`, `=ANY` / `=SOME`
- **Comparacion contra conjunto**: `<ANY`, `>ALL`, `<>ALL`, etc.
- **Existencia**: `EXISTS`, `NOT EXISTS` (con subconsultas correlacionadas)
- **Rango**: `BETWEEN val1 AND val2`

---

### Funciones de agregacion

Reemplazan un grupo de valores por un unico valor. Solo pueden aparecer en `SELECT` o `HAVING`.

| Funcion | Descripcion | Acepta DISTINCT |
|---|---|---|
| `COUNT(col)` | Cantidad de valores no NULL | Si |
| `COUNT(*)` | Cantidad de tuplas (incluyendo NULL) | No |
| `SUM(col)` | Suma de valores no NULL | Si |
| `AVG(col)` | Promedio de valores no NULL | Si |
| `MAX(col)` | Maximo valor | No (no tiene sentido) |
| `MIN(col)` | Minimo valor | No (no tiene sentido) |

> [!important] Reglas criticas de las funciones de agregacion
> - **No se pueden anidar**: `SUM(AVG(col))` es invalido. Para lograrlo, usar subconsulta.
> - Si hay funcion de agregacion en SELECT **sin** GROUP BY, se aplica sobre **todas** las tuplas.
> - Si hay funcion de agregacion, **todo atributo** en SELECT debe estar agregado **o** en GROUP BY.
> - `AVG(col)` y `SUM(col)/COUNT(*)` **no son equivalentes** cuando hay NULL (COUNT(\*) cuenta filas con NULL, AVG descarta NULL antes de dividir).

---

### GROUP BY y HAVING

**GROUP BY** arma subgrupos de tuplas con valores coincidentes en los atributos indicados. Para cada subgrupo se genera una sola tupla en el resultado.

**HAVING** filtra **grupos** (no tuplas individuales como WHERE). Se evalua **despues** de armar los grupos.

```sql
-- Sueldo promedio por edad, solo para edades > 25 con al menos 2 representantes
SELECT edad, AVG(sueldo)
FROM empleado
WHERE edad > 25
GROUP BY edad
HAVING COUNT(edad) >= 2;
```

> [!tip] WHERE vs. HAVING
> - `WHERE` filtra **tuplas** antes de agrupar.
> - `HAVING` filtra **grupos** despues de agrupar.
> - Poner en WHERE todo lo que se pueda para reducir el volumen antes del agrupamiento.

---

### Funciones de transformacion

#### CASE

Permite condicionales dentro de una consulta:

```sql
SELECT CASE
    WHEN nota < 4 THEN 'Desaprobado'
    WHEN nota > 7 THEN 'Muy Bien'
    ELSE 'Aprobado'
END AS resultado, legajo
FROM inscripto;
```

#### COALESCE

Devuelve el **primer valor no NULL** de la lista de argumentos:

```sql
-- Reemplazar NULL por 0 antes de promediar
SELECT AVG(COALESCE(acum, 0))
FROM (SELECT SUM(monto)
      FROM penalizacion RIGHT OUTER JOIN jugador
           ON penalizacion.codigo = jugador.codigo
      GROUP BY jugador.codigo) AS AUXI(acum);
```

#### CAST

Transforma un tipo de dato en otro (si es compatible):

```sql
CAST(valor AS DECIMAL(5,2))
CAST(valor AS FLOAT)
```

> [!tip] Evitar truncamiento en promedios
> Si la columna es `INTEGER`, el resultado de `AVG` puede truncarse en algunos DBMS (DB2). Usar `CAST` para convertir a `FLOAT` o `DECIMAL` antes de aplicar la funcion.

---

### Dominios definidos por el usuario

SQL-2 permite crear dominios con `CREATE DOMAIN` (similar a `typedef` en C):

```sql
CREATE DOMAIN nombreTipo  CHAR(50)  NOT NULL;
CREATE DOMAIN edadTipo    INTEGER   CHECK(value > 0);
CREATE DOMAIN colorTipo   CHAR(4)   DEFAULT 'rojo'
                                    CHECK(value IN ('rojo', 'azul'));
```

> [!important] Soporte limitado
> Solo **PostgreSQL** soporta `CREATE DOMAIN`. Ni DB2 ni Oracle lo implementan. En esos casos, definir las restricciones directamente en la columna de la tabla.

---

## Notas

- SQL maneja **multisets** (bags), no sets: permite tuplas repetidas si no se definen claves. Usar `DISTINCT` para eliminar duplicados en consultas.
- Las operaciones conjuntistas (`UNION`, `INTERSECT`, `EXCEPT`) **si eliminan duplicados** por defecto. Para mantenerlos, usar `ALL`.
- La portabilidad entre DBMS se logra usando solo comandos **ANSI-compliant** y evitando extensiones propietarias.
- El `SELECT` es la **ultima clausula** que evalua el DBMS, por lo que un alias definido con `AS` en el SELECT no esta disponible en WHERE ni HAVING.
- Nombrar restricciones con `CONSTRAINT nombre` permite eliminarlas despues con `DROP CONSTRAINT nombre`.

---

## Preguntas

- En el patron de referencias circulares, si ambas FK son `NOT NULL`, como funciona exactamente la validacion diferida con transacciones `DEFERRABLE`?
- Cual es el comportamiento exacto de `SET DEFAULT` en ON DELETE si la columna no tiene DEFAULT definido?
- Las restricciones CHECK a nivel de tabla pueden referenciar otras tablas o solo la tabla propia? (Diferencia con ASSERTION en SQL-3)
- En que casos concretos conviene usar `RESTRICT` vs. `NO ACTION` en FK? Son exactamente equivalentes?
