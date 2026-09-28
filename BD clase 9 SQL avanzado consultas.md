---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - consultas-avanzadas
Created: 2026-03-03 14:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - completitud relacional
  - clausura transitiva
  - operador de punto fijo
  - CTE (WITH y WITH RECURSIVE)
  - recursion en SQL3
  - recursion lineal
---

# BD clase 9 — SQL Avanzado: Consultas

## Resumen

Esta clase introduce el concepto de **consultas recursivas** en SQL3, algo que SQL2 y el algebra relacional clasica no pueden expresar. El eje central es la **clausula WITH** (CTE) y su variante **WITH RECURSIVE**, que permite calcular la **clausura transitiva** de una relacion mediante el operador de **punto fijo**.

### Completitud de lenguajes

Existen dos nociones de completitud que no deben confundirse:

- **Completitud relacional** (Codd, 1972): un lenguaje puede expresar todo lo que expresa el algebra relacional.
- **Completitud computable** (Chandra y Horel, 1979): un lenguaje puede expresar todas las consultas Turing-computables.

| Propiedad | Algebra relacional | Calculo relacional seguro | SQL2 | SQL3 |
|---|---|---|---|---|
| Relacionalmente completo | Si (es la referencia) | Si | Si (y mas: agrupacion, agregacion) | Si |
| Computablemente completo | No | No | No | No (pero agrega recursion) |

El algebra relacional **no puede expresar**: clausura transitiva, conteo de tuplas, test de paridad. SQL3 resuelve la clausura transitiva agregando recursion, pero sigue sin ser computablemente completo.

> [!important] La diferencia entre "relacionalmente completo" y "computablemente completo" es clave. SQL2 supera al algebra relacional en algunas cosas (GROUP BY, funciones de agregacion) pero no puede expresar recursion. SQL3 agrega recursion pero tampoco alcanza completitud computable.

### Clausura transitiva

La **clausura transitiva** R+ de una relacion binaria R es la **menor relacion transitiva que contiene a R**. Si R ya es transitiva, R+ = R. Si no lo es, R+ se obtiene agregando la minima cantidad de tuplas necesarias para garantizar transitividad.

Una relacion R es **transitiva** si:

> Para todo x1, x2, x3 en el dominio: si (x1, x2) esta en R y (x2, x3) esta en R, entonces (x1, x3) esta en R.

**Ejemplo intuitivo**: la relacion "hay eje directo entre" nodos de un grafo no es transitiva (salvo que el grafo sea completo), pero su clausura transitiva representa "hay camino entre".

> [!tip] Pensar la clausura transitiva como "expandir relaciones directas a relaciones alcanzables transitivamente". Es el patron detras de jerarquias (jefe -> superior), rutas (vuelo directo -> itinerario), dependencias, etc.

### Operador de Punto Fijo

Para calcular la clausura transitiva se introduce el **operador de punto fijo** en el algebra relacional.

**Punto fijo de una funcion f**: un valor x tal que f(x) = x. El **minimo punto fijo** es el menor de todos los puntos fijos.

En BD, dada una ecuacion f(R) = R donde f es una expresion del algebra relacional, el **minimo punto fijo** R* cumple:

1. f(R*) = R* (es punto fijo)
2. Si R' es otro punto fijo, entonces R* esta contenido en R' (es el minimo)

Por el **teorema de Tarski**, si f es **monotona** (R1 contenido en R2 implica f(R1) contenido en f(R2)), el minimo punto fijo existe y es unico.

**Algoritmo iterativo** que usa el DBMS internamente:

```
PtoFijo <- R

DO
    auxi <- PtoFijo
    PtoFijo <- f(PtoFijo) UNION PtoFijo
UNTIL auxi = PtoFijo

RETURN PtoFijo
```

La funcion f que genera transitividad es:

> f = { (x1, x3) | existe x2 tal que R(x1, x2) y R(x2, x3) }

Cada iteracion agrega tuplas nuevas derivadas de la composicion de las existentes, hasta que no se generan mas (punto fijo alcanzado).

> [!important] El teorema que garantiza la correccion del algoritmo demuestra tres cosas: (1) R esta contenido en PtoFijo, (2) PtoFijo es transitiva, (3) PtoFijo es minimal. La demostracion de minimalidad usa induccion sobre las iteraciones.

### CTE: Clausula WITH (no recursiva)

SQL3 introduce la **clausula WITH** para definir **tablas temporarias** (Common Table Expressions) que existen solo durante la ejecucion de la consulta, sin almacenarse en la base.

**Sintaxis basica:**

```sql
WITH nombreRelacion(columnas) AS (
    SELECT ...
)
SELECT ... FROM nombreRelacion ...;
```

Se pueden definir **multiples CTEs** separadas por coma:

```sql
WITH
    cte1 AS (SELECT legajo, UPPER(nombre) AS N FROM ALUMNO),
    cte2 AS (SELECT codigo, legajo FROM cursa WHERE codigo > 2000)
SELECT cte1.*, cte2.codigo
FROM cte1, cte2
WHERE cte1.legajo = cte2.legajo;
```

> [!tip] Las CTEs no recursivas son utiles para descomponer consultas complejas en pasos legibles. Cada CTE es como una "variable temporal" con nombre que se puede referenciar en el SELECT final o en CTEs posteriores.

### CTE Recursiva: WITH RECURSIVE

La verdadera novedad de SQL3 es poder usar **WITH RECURSIVE** para resolver consultas que requieren **clausura transitiva**.

**Estructura obligatoria:**

```sql
WITH RECURSIVE nombreTabla(columnas) AS (
    -- Caso base (R0): NO referencia a nombreTabla
    SELECT ... FROM tablaBase
    UNION
    -- Paso recursivo: referencia a nombreTabla (maximo 1 vez)
    SELECT ... FROM nombreTabla, tablaBase
    WHERE ...
)
SELECT ... FROM nombreTabla;
```

| Parte | Rol | Restriccion |
|---|---|---|
| Antes del UNION | **Caso base** (R0) | No puede referenciar la CTE recursiva |
| Despues del UNION | **Paso inductivo** | Puede referenciar la CTE, pero **solo 1 vez** (recursion lineal) |
| SELECT final | Consulta sobre el resultado | Obligatorio para usar la tabla generada |

> [!important] SQL3 solo soporta **recursion lineal**: la tabla recursiva puede aparecer como maximo 1 vez en el paso recursivo. Si se necesita un producto cartesiano de la tabla consigo misma, hay que reescribir la consulta usando la tabla base original en uno de los lados del JOIN.

### Patron: Reescritura de recursion no lineal a lineal

Cuando la logica pide un producto cartesiano de la relacion recursiva consigo misma, se reescribe usando la tabla base en uno de los operandos.

**Ejemplo**: SuperiorDe (jerarquia de jefes).

Version **no valida** (no lineal, SuperiorDe aparece 2 veces):

```sql
-- NO VALIDO en SQL3
WITH RECURSIVE SuperiorDe(X, Y) AS (
    SELECT P1, P2 FROM JefeDe
    UNION
    SELECT Sup1.X, Sup2.Y
    FROM SuperiorDe Sup1, SuperiorDe Sup2
    WHERE Sup1.Y = Sup2.X
)
SELECT X, Y FROM SuperiorDe;
```

Version **valida** (lineal, SuperiorDe aparece 1 sola vez):

```sql
WITH RECURSIVE SuperiorDe(X, Y) AS (
    SELECT P1, P2 FROM JefeDe
    UNION
    SELECT SuperiorDe.X, JefeDe.P2
    FROM SuperiorDe, JefeDe
    WHERE SuperiorDe.Y = JefeDe.P1
)
SELECT X, Y FROM SuperiorDe;
```

La clave de la reescritura: P1 es SuperiorDe P2 si P1 es JefeDe P2 directamente, **o bien** P1 es SuperiorDe alguna persona Pn tal que Pn es JefeDe P2 directamente. Se reemplaza una de las referencias recursivas por la tabla base.

> [!tip] Para linealizar: identificar cual de las dos referencias recursivas puede sustituirse por la tabla base original. Generalmente es la que aporta "un paso mas" en la cadena transitiva.

### Ejemplo completo: Itinerarios de vuelos

Dado el esquema:

```sql
CREATE TABLE vuelo(
    origen   VARCHAR(20),
    destino  VARCHAR(20),
    salida   TIMESTAMP,
    arribo   TIMESTAMP
);
```

Para calcular todos los **itinerarios posibles** (conexiones directas e indirectas con escalas validas):

```sql
WITH RECURSIVE combinacion(origen, destino, salida, arribo) AS (
    -- Caso base: vuelos directos
    SELECT * FROM vuelo
    UNION
    -- Paso recursivo: extender itinerarios existentes
    SELECT vuelo.origen, combinacion.destino,
           vuelo.salida, combinacion.arribo
    FROM vuelo, combinacion
    WHERE vuelo.destino = combinacion.origen
      AND vuelo.arribo < combinacion.salida  -- escala valida
)
SELECT * FROM combinacion;
```

La condicion `vuelo.arribo < combinacion.salida` asegura que el pasajero llega antes de que salga el siguiente tramo. Esto es lo que hace que no todas las combinaciones sean validas y que el punto fijo dependa de los datos temporales concretos.

> [!important] En el ejemplo de vuelos, el resultado de la clausura transitiva **depende de las restricciones temporales**, no solo de la topologia origen-destino. Un cambio en los horarios puede hacer que itinerarios multi-escala dejen de ser alcanzables.

### CTE vs Subconsulta: cuando usar cada una

| Criterio | CTE (WITH) | Subconsulta |
|---|---|---|
| **Legibilidad** | Alta: nombra y separa cada paso | Baja si hay anidamiento profundo |
| **Reutilizacion** | Se puede referenciar multiples veces en la misma query | Hay que repetir la subconsulta |
| **Recursion** | Si (WITH RECURSIVE) | No es posible |
| **Rendimiento** | Depende del motor; algunos materializan la CTE | Generalmente inline, optimizable |
| **Caso tipico** | Consultas complejas, jerarquias, pasos intermedios | Filtros simples, EXISTS, IN |

> [!tip] Regla practica: si necesitas **recursion**, no hay alternativa: WITH RECURSIVE. Si necesitas **reusar** el resultado intermedio, CTE. Si es un filtro puntual, una subconsulta clasica puede ser mas directa.

### Test de paridad: limite del punto fijo

El operador de punto fijo **no sirve** para todo. No puede resolver el **test de paridad** (determinar si la cantidad de tuplas es par) porque requeriria una variable que oscile entre 0 y 1, lo cual no es expresable en el algebra relacional.

Esto marca un limite claro: el punto fijo expande significativamente el poder expresivo (clausuras transitivas, jerarquias, grafos), pero **no hace al lenguaje computablemente completo**.

## Notas

- El DBMS ejecuta la recursion internamente con el algoritmo iterativo de punto fijo: arranca con el caso base y aplica el paso recursivo hasta que no se generan tuplas nuevas.
- La demostracion de que el algoritmo de punto fijo calcula efectivamente la clausura transitiva tiene tres partes (contiene a R, es transitiva, es minimal). La minimalidad se demuestra por **induccion**.
- En el ejemplo de SuperiorDe, el DBMS necesita 4 iteraciones para llegar al punto fijo (la cuarta no agrega nada nuevo).
- Las CTEs no recursivas son puramente una herramienta de **organizacion y legibilidad**; no agregan poder expresivo nuevo respecto de subconsultas.
- SQL3 deliberadamente limita a **recursion lineal** (una sola referencia recursiva) para evitar problemas de terminacion y complejidad computacional.

## Preguntas

1. Si una relacion binaria R ya es transitiva, que devuelve el algoritmo de punto fijo? (R+ = R, el algoritmo termina en la primera iteracion sin agregar tuplas)
2. Por que SQL3 restringe a recursion lineal y no permite la no lineal? Que problemas trae la recursion no lineal?
3. En el ejemplo de vuelos, como cambiaria el resultado si se eliminara la condicion temporal `vuelo.arribo < combinacion.salida`?
4. Como se demostraria formalmente que el algoritmo iterativo siempre termina para relaciones finitas?
5. Se podria usar WITH RECURSIVE para detectar ciclos en un grafo? Que precauciones habria que tomar para evitar recursion infinita?

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (BD)**

- [BD clase 8 SQL consultas](BD%20clase%208%20SQL%20consultas.md) — clase anterior
- [BD clase 11 normalizacion parte 1](BD%20clase%2011%20normalizacion%20parte%201.md) — clase siguiente

**Otras materias**

- **Discrete Math**  [Discrete Math - Caminos y Conexidad](Discrete%20Math%20-%20Caminos%20y%20Conexidad.md) — clausura transitiva = alcanzabilidad en un grafo
- **EDA**  [EDA - Grafos](EDA%20-%20Grafos.md) — recorrer un grafo con WITH RECURSIVE
- **PI**  [PI - Recursividad en C](PI%20-%20Recursividad%20en%20C.md) — recursión y caso base

<!-- notas-relacionadas:fin -->
