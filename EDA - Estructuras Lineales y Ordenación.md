---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Índices
  - QuickSort
  - MergeSort
  - Generics
  - Teorema Maestro
  - Divide y Conquista
---
# EDA — Estructuras Lineales y Ordenación

## Resumen

### Índices
Estructura auxiliar que facilita la búsqueda (lookup). Compuesta por elementos que representan la información indexada (ej: búsqueda por legajo).

**Operaciones sobre un índice:** búsqueda, inserción, borrado.  
Conviene crear un índice por cada tipo de consulta.

### Teorema Maestro
Permite resolver recurrencias de la forma T(N) = a·T(N/b) + f(N) sin expandir el árbol.  
Se aplica en algoritmos Divide y Conquista donde el subproblema tiene tamaño N/b.

**Técnica Divide y Conquista:**
1. Dividir el problema en subproblemas del mismo tamaño.
2. Resolver cada subproblema de forma recursiva.
3. Combinar los resultados.

### QuickSort — O(n log n) promedio, O(n²) peor caso
- Opera *in-place* (no usa array extra).
- Elige un **pivot**, coloca menores a la izquierda y mayores a la derecha.
- Si el subarray tiene 0 o 1 elemento → ya está ordenado (caso base).

### MergeSort — O(n log n) siempre
- Divide en mitades hasta llegar a subarrays de 1 elemento.
- Sube mergeando de forma ordenada.
- No opera in-place (requiere espacio extra O(n)).

### Generics en Java
Permiten parametrizar tipos para evitar casteos en tiempo de ejecución.  
Java implementa Generics con *Erasure*: reemplaza el tipo parámetro por su bound (o `Object` si no hay) y agrega casteos automáticos.

```java
// Sin generics (peligroso)
public class Caja { Object valor; }

// Con generics
public class Caja<E extends Comparable<E>> { E valor; }
```

**Indexación genérica:** crear una colección auxiliar que sirva de índice; crear un índice por tipo de consulta.

## Notas
- QuickSort es mejor en la práctica por constantes más pequeñas; MergeSort es mejor cuando se necesita estabilidad.
- Para calcular complejidad en recurrencias: aplicar Teorema Maestro si aplica, o expandir el árbol de invocaciones.
- La búsqueda debe ser **muy eficiente** para que el índice valga la pena.

## Preguntas
- ¿Por qué QuickSort tiene O(n²) en el peor caso?
- ¿Cuándo NO aplica el Teorema Maestro?
- ¿Qué es la técnica de Erasure y qué limitaciones impone?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]
