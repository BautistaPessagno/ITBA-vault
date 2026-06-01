---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - consultas
Created: 2026-03-0314:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Orden de evaluacion de clausulas
  - Funciones de agregacion
  - Subconsultas (IN, EXISTS, ANY, ALL)
  - WHERE vs HAVING
  - SELECT FROM WHERE basico
  - DISTINCT y ORDER BY
  - BETWEEN LIKE IN
  - GROUP BY y HAVING
  - Operaciones conjuntistas (UNION, INTERSECT, EXCEPT)
  - JOINs (INNER, LEFT, RIGHT, FULL OUTER, CROSS)
  - CASE COALESCE CAST
  - Vistas (Views)
---
# BD clase 8 — SQL Consultas

## Resumen

SQL es un lenguaje de consulta declarativo que opera sobre **multiconjuntos** (bags) de tuplas, no sobre conjuntos puros. A diferencia del Algebra Relacional, SQL **no elimina duplicados** salvo que se pida explicitamente con `DISTINCT` o se usen operaciones conjuntistas (`UNION`, `INTERSECT`, `EXCEPT`). Esta clase cubre la sintaxis completa de recupero de datos: desde las clausulas basicas hasta agrupacion, subconsultas, juntas y vistas.

### Orden de evaluacion de clausulas

El DBMS no evalua las clausulas en el orden en que se escriben. Entender el **orden real de ejecucion** es clave para saber que alias estan disponibles y donde pueden usarse funciones de agregacion.

| Paso | Clausula   | Funcion                                                     |
| ---- | ---------- | ----------------------------------------------------------- |
| 1    | `FROM`     | Identifica las tablas y calcula productos cartesianos/joins |
| 2    | `WHERE`    | Filtra tuplas individuales (condicion sobre filas)          |
| 3    | `GROUP BY` | Agrupa tuplas por atributos coincidentes                    |
| 4    | `HAVING`   | Filtra grupos completos (condicion sobre agregados)         |
| 5    | `ORDER BY` | Ordena el resultado                                         |
| 6    | `SELECT`   | Proyecta atributos y aplica funciones de agregacion         |

> [!important] SELECT se evalua ultimo
> Un alias definido con `AS` en el `SELECT` **no puede usarse** en `WHERE`, `GROUP BY` ni `HAVING` porque esas clausulas se evaluan antes. PostgreSQL relaja esto parcialmente, pero el estandar ANSI no lo permite.

Sintaxis general de una consulta:

```sql
SELECT [DISTINCT] atrib1 [AS alias1], ..., atribN [AS aliasN]
FROM lista_de_tablas
[WHERE condicion]
[GROUP BY lista_atributos
  [HAVING condicion_grupo]]
[ORDER BY lista_atributos [ASC|DESC]]
```

### SELECT, FROM y WHERE basicos

- **FROM**: indica las tablas que participan. Soporta alias con `AS` (equivale al renombramiento del Algebra Relacional). Las tablas pueden ser persistentes o temporarias (subconsultas, que deben recibir un nombre obligatorio).
- **WHERE**: equivale al operador de seleccion del Algebra Relacional. Filtra tuplas cuya condicion se evalua como verdadera.
- **SELECT**: equivale a la proyeccion. `*` selecciona todos los atributos. `DISTINCT` elimina duplicados en el resultado.

```sql
-- Proyeccion con filtro
SELECT ISBN, precio
FROM libro
WHERE precio > 100 AND precio < 2000;
```

> [!tip] Ambiguedades en atributos
> Si dos tablas en el `FROM` comparten nombre de atributo, hay que **prefijar** con el nombre de tabla: `editorial.codEdit`. Si la misma tabla aparece mas de una vez, se necesitan **alias** obligatorios (`FROM libro AS l1, libro AS l2`).

### DISTINCT y ORDER BY

- **DISTINCT** se aplica a **todo el resultado**, no a columnas individuales. Solo se escribe una vez.
- **ORDER BY** permite `ASC` (ascendente, por defecto) o `DESC` por cada atributo. No tiene equivalente en el Algebra Relacional.

```sql
SELECT DISTINCT vendido_por
FROM articulo
ORDER BY vendido_por DESC;
```

### Operadores en WHERE: BETWEEN, LIKE, IN

| Operador | Uso | Ejemplo |
|----------|-----|---------|
| `BETWEEN x AND y` | Rango inclusivo | `WHERE precio BETWEEN 100 AND 2000` |
| `LIKE` | Patron de strings (`%` = cualquier substring, `_` = un caracter) | `WHERE nombre LIKE '%Perez%'` |
| `IN (lista)` | Pertenencia a un conjunto | `WHERE precio IN (10, 20, 30)` |
| `IS NULL` / `IS NOT NULL` | Comparacion de nulos (no usar `= NULL`) | `WHERE precio IS NOT NULL` |

> [!important] NULL no se compara con =
> `WHERE atr = NULL` siempre se evalua como **FALSE**. Para verificar nulidad usar exclusivamente `IS NULL` o `IS NOT NULL`.

```sql
-- LIKE: libros cuya descripcion empieza con 'Introduccion'
SELECT ISBN, precio
FROM libro
WHERE descrip LIKE 'Introduccion %';

-- IN con subconsulta
SELECT ISBN, precio
FROM libro
WHERE codEdit IN (SELECT codEdit FROM editorial);
```

### Funciones de agregacion

Las **funciones de agregacion** (o sumarizacion) reemplazan un grupo de valores por un unico valor. Solo pueden aparecer en `SELECT` o `HAVING`.

| Funcion | Descripcion | Acepta DISTINCT | Comportamiento con NULL |
|---------|-------------|:---:|---|
| `COUNT(col)` | Cantidad de valores no nulos | Si | Ignora NULLs; devuelve 0 si vacio |
| `COUNT(*)` | Cantidad de tuplas (filas) | No | **No ignora NULLs** |
| `SUM(col)` | Suma de valores | Si | Ignora NULLs; devuelve NULL si vacio |
| `AVG(col)` | Promedio de valores | Si | Ignora NULLs; devuelve NULL si vacio |
| `MAX(col)` | Valor maximo | No tiene sentido | Ignora NULLs; devuelve NULL si vacio |
| `MIN(col)` | Valor minimo | No tiene sentido | Ignora NULLs; devuelve NULL si vacio |

> [!important] Reglas criticas de las funciones de agregacion
> - **No se pueden anidar**: `SUM(AVG(sueldo))` es invalido. Para lograrlo, usar subconsultas.
> - Si aparece una funcion de agregacion en `SELECT`, **todo atributo no agregado debe estar en `GROUP BY`**.
> - `AVG(col)` y `SUM(col)/COUNT(*)` **no son equivalentes** cuando hay NULLs. `AVG` ignora NULLs en el denominador, `COUNT(*)` los cuenta.

```sql
-- Trampa clasica: AVG vs SUM/COUNT
SELECT AVG(edad) FROM empleado;          -- 30.25 (sobre 4 valores no nulos)
SELECT SUM(edad)/COUNT(*) FROM empleado; -- 20.16 (divide por 6 filas totales)
```

Ejemplo con tabla donde todos los valores son NULL:

```sql
SELECT MAX(nro), MIN(nro), AVG(nro), SUM(nro), COUNT(nro), COUNT(*)
FROM sorpresa;
-- Resultado: NULL  NULL  NULL  NULL  0  3
```

### GROUP BY y HAVING

**GROUP BY** arma subgrupos de tuplas con valores coincidentes en los atributos indicados. Cada grupo produce **una unica tupla** en el resultado.

- Atributos en `SELECT`: deben ser o bien **atributos de agrupacion** o bien estar **afectados por una funcion de agregacion**.
- `GROUP BY` considera que **dos NULLs son equivalentes** (los agrupa juntos), a diferencia de `WHERE NULL = NULL` que da FALSE.

```sql
-- Sueldo promedio por edad
SELECT edad, AVG(sueldo)
FROM empleado
GROUP BY edad;
```

**HAVING** filtra **grupos completos** despues de armarlos. Se evalua en el paso 4 (despues de `GROUP BY`).

```sql
-- Sueldo promedio por edad, solo para mayores de 25, con al menos 2 representantes
SELECT AVG(sueldo), edad
FROM empleado
WHERE edad > 25
GROUP BY edad
HAVING COUNT(edad) >= 2;
```

> [!tip] WHERE vs HAVING: regla rapida
> - **WHERE** filtra **tuplas individuales** antes de agrupar.
> - **HAVING** filtra **grupos** despues de agrupar (usa funciones de agregacion).
> - Poner en `WHERE` todo lo que puedas: reduce el tamanio de los grupos y mejora el rendimiento.

Ejemplo paso a paso del motor:

| Paso | Accion | Resultado |
|------|--------|-----------|
| 1. WHERE | Elimina tuplas con edad <= 25 | Quedan Juan(34), Clara(30), Jose(34) |
| 2. GROUP BY | Agrupa por edad | Grupo 30: {Clara}, Grupo 34: {Juan, Jose} |
| 3. HAVING | Descarta grupos con COUNT < 2 | Solo queda Grupo 34 |
| 4. SELECT | Proyecta AVG(sueldo) y edad | (2100, 34) |

### Subconsultas: IN, EXISTS, ANY, ALL

Las **subconsultas** (o consultas anidadas) permiten usar el resultado de un `SELECT` dentro de otro. La consulta interna tiene prioridad; para referenciar atributos de la consulta externa se usa el prefijo de tabla.

| Operador | Semantica | Ejemplo tipico |
|----------|-----------|----------------|
| `IN` / `NOT IN` | Pertenencia a un conjunto | `WHERE codEdit IN (SELECT codEdit FROM editorial)` |
| `EXISTS` / `NOT EXISTS` | Verdadero si subconsulta no es vacia | `WHERE EXISTS (SELECT * FROM ...)` |
| `= ANY` / `<> ANY` / `> ANY` | Compara contra **alguno** de la lista | `WHERE precio > ANY (SELECT ...)` |
| `= ALL` / `<> ALL` / `> ALL` | Compara contra **todos** de la lista | `WHERE precio >= ALL (SELECT ...)` |

> [!important] Equivalencias utiles
> - `IN` es equivalente a `= ANY` (o `= SOME`).
> - `NOT IN` es equivalente a `<> ALL`.
> - En PostgreSQL, `ANY` y `SOME` **no aceptan listas fijas**, solo subconsultas. `IN` si acepta listas fijas.

**EXISTS** (consultas correlacionadas): se evalua para cada tupla de la consulta externa. Si la subconsulta devuelve al menos una fila, la tupla externa aparece en el resultado.

```sql
-- Libros que tienen editorial asociada
SELECT *
FROM libro
WHERE EXISTS (SELECT *
              FROM editorial
              WHERE editorial.codEdit = libro.codEdit);

-- Libros que NO tienen editorial asociada
SELECT *
FROM libro
WHERE NOT EXISTS (SELECT *
                  FROM editorial
                  WHERE editorial.codEdit = libro.codEdit);
```

**Patron para "para todo" (division relacional)** con doble NOT EXISTS:

```sql
-- Proveedores que abastecen TODOS los productos de software
SELECT p.nombre
FROM proveedor p
WHERE NOT EXISTS (
    SELECT *
    FROM articulo a
    WHERE a.vendido_por = 'Software'
      AND NOT EXISTS (
          SELECT *
          FROM provee h
          WHERE a.codigo = h.codigo
            AND p.nombre = h.nombre));
```

Alternativa con COUNT:

```sql
SELECT nombre
FROM provee, articulo
WHERE articulo.vendido_por = 'Software'
  AND provee.codigo = articulo.codigo
GROUP BY nombre
HAVING COUNT(*) = (SELECT COUNT(*) FROM articulo WHERE vendido_por = 'Software');
```

### JOINs en SQL

SQL-2 permite usar juntas directamente en la clausula `FROM`.

| Tipo de JOIN | Sintaxis | Comportamiento |
|---|---|---|
| **CROSS JOIN** | `r CROSS JOIN s` o `r, s` | Producto cartesiano |
| **INNER JOIN** | `r [INNER] JOIN s ON cond` | Solo tuplas que matchean (theta join) |
| **NATURAL JOIN** | `r NATURAL JOIN s` | Join por igualdad de atributos con mismo nombre; columnas repetidas aparecen una sola vez |
| **LEFT OUTER JOIN** | `r LEFT OUTER JOIN s ON cond` | Todas las de r; NULLs donde s no matchea |
| **RIGHT OUTER JOIN** | `r RIGHT OUTER JOIN s ON cond` | Todas las de s; NULLs donde r no matchea |
| **FULL OUTER JOIN** | `r FULL OUTER JOIN s ON cond` | Todas las de ambas; NULLs donde no hay match |

> [!tip] JOIN a secas en SQL vs Algebra Relacional
> En el Algebra Relacional, el "join a secas" es el **natural join**. En SQL, `JOIN` sin especificar es el **theta join** (requiere condicion con `ON`). No confundirlos.

```sql
-- INNER JOIN explicito
SELECT descripcion, nombre
FROM articulo JOIN provee ON articulo.codigo = provee.codigo;

-- LEFT OUTER JOIN: conserva articulos sin proveedor
SELECT descripcion, nombre
FROM articulo LEFT OUTER JOIN provee ON articulo.codigo = provee.codigo;

-- Equivalente sin sintaxis JOIN (DBMS antiguos)
SELECT descripcion, nombre
FROM articulo, provee
WHERE articulo.codigo = provee.codigo;
```

### Operaciones conjuntistas: UNION, INTERSECT, EXCEPT

Estas operaciones **eliminan duplicados** automaticamente (comportamiento de conjunto). Para conservar duplicados, usar `ALL`.

| Operacion | SQL | Algebra Relacional |
|-----------|-----|--------------------|
| Union | `(SELECT ...) UNION (SELECT ...)` | r U s |
| Interseccion | `(SELECT ...) INTERSECT (SELECT ...)` | r ∩ s |
| Diferencia | `(SELECT ...) EXCEPT (SELECT ...)` | r - s |

> [!important] Compatibilidad de esquemas
> Las consultas combinadas deben tener el **mismo numero y tipo de atributos**. Si ambos resultados coinciden en valor, `UNION` devuelve una sola tupla (usar `UNION ALL` para mantener duplicados).

```sql
-- Precio maximo de hardware y de software
(SELECT MAX(precio) FROM articulo, provee
 WHERE articulo.codigo = provee.codigo AND vendido_por = 'Software')
UNION
(SELECT MAX(precio) FROM articulo, provee
 WHERE articulo.codigo = provee.codigo AND vendido_por = 'Hardware');
```

### CASE, COALESCE y CAST

**CASE**: condicional en linea, usable en `SELECT`, `WHERE` o `HAVING`.

```sql
SELECT CASE
         WHEN nota < 4 THEN 'Desaprobado'
         WHEN nota > 7 THEN 'Muy Bien'
         ELSE 'Aprobado'
       END AS clasificacion, legajo
FROM inscripto;
```

**COALESCE**: devuelve el primer argumento no NULL. Util para reemplazar NULLs antes de aplicar funciones de agregacion.

```sql
-- Reemplazar NULL por 0 antes de promediar
SELECT AVG(COALESCE(acum, 0))
FROM (SELECT SUM(monto)
      FROM penalizacion RIGHT OUTER JOIN jugador
        ON penalizacion.codigo = jugador.codigo
      GROUP BY jugador.codigo) AS AUXI(acum);
```

**CAST**: convierte tipos de datos para evitar truncamiento o incompatibilidades.

```sql
SELECT AVG(CAST(COALESCE(acum, 0) AS DECIMAL(5,2)))
FROM (...) AS AUXI(acum);
```

### Vistas (Views)

Una **vista** es una tabla virtual definida por una consulta. No almacena datos: **referencia** las tablas base. Cambios en las tablas base se reflejan automaticamente en la vista.

```sql
CREATE VIEW personal
AS SELECT nombre, sueldo
   FROM empleado
   WHERE nombre NOT IN (SELECT jefe FROM departamento)
     AND trabaja_en IS NOT NULL;

-- Usar la vista como cualquier tabla
SELECT * FROM personal WHERE sueldo > 1500;
```

> [!important] Restricciones de las vistas
> - No se permite `ORDER BY` en la definicion de la vista (ANSI; PostgreSQL lo permite).
> - **Actualizacion limitada**: no se pueden modificar vistas derivadas de joins, con funciones de agregacion, o con atributos derivados.
> - Son un mecanismo de **seguridad**: permiten exponer solo ciertos atributos/tuplas a cada usuario.

## Notas

- Las tablas en SQL son **multiconjuntos** (bags), no conjuntos. Esto significa que pueden tener filas duplicadas, lo cual es relevante para funciones de agregacion.
- `COUNT(DISTINCT col)` vs `COUNT(col)` puede dar resultados muy diferentes. Siempre pensar si necesitamos contar valores unicos o totales.
- Para resolver la **division relacional** ("para todo") en SQL: usar el patron de **doble NOT EXISTS** o la alternativa con `GROUP BY + HAVING COUNT`.
- **Funciones de agregacion + NULL** es fuente de errores frecuentes en examenes. Recordar que `AVG` ignora NULLs pero `COUNT(*)` no.
- Los alias del `SELECT` no se pueden usar en `WHERE` ni `HAVING` (el motor aun no los conoce).
- `UNION` elimina duplicados; `UNION ALL` los conserva. Usar `ALL` cuando se necesite o cuando se sepa que no hay duplicados (mejora rendimiento).

## Preguntas

- Si `AVG(col)` ignora NULLs y `COUNT(*)` no, que pasa cuando se calcula `SUM(col)/COUNT(*)` con NULLs presentes? Por que da diferente a `AVG(col)`?
- En que casos conviene usar `EXISTS` en vez de `IN`? Cuando la subconsulta es grande, EXISTS puede ser mas eficiente porque puede dejar de buscar al encontrar la primera coincidencia.
- Por que `GROUP BY` considera NULLs como equivalentes si `WHERE NULL = NULL` da FALSE?
- Puede una vista definida sobre un JOIN ser actualizable? En general no, pero que excepciones existen segun el DBMS?
- Cuando es necesario usar `DISTINCT` dentro de `COUNT` en un `HAVING`? Depende de si el atributo contado es clave candidata o no.
