---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Listas
  - Lista Simplemente Encadenada
  - Arreglos
  - Índices
---
# EDA — Listas Lineales

## Resumen

### Arreglos Ordenados como Índice
**Ventajas:**
- Permite **búsqueda binaria** → O(log n).
- Excelente para `getMaximo()` / `getMinimo()` (acceso por posición).

**Desventajas:**
- Los datos deben ser **contiguos en memoria** → insertar/borrar requiere mover elementos.
- Al agotar el espacio pre-alocado, hay que generar espacio contiguo nuevo y copiar todos los elementos (conviene hacerlo de a *chunks*).

### Lista Lineal Simplemente Encadenada
Estructura de 0 o más **nodos**. Cada nodo almacena:
1. Su **información** (dato).
2. Una **referencia al nodo siguiente**.

No requiere contigüidad en memoria → inserción/borrado sin mover elementos.

**Lista Ordenada:** variante que mantiene los nodos ordenados según algún criterio.  
Implementar la interfaz `Iterator` permite también el método `remove()`.

### Cuándo elegir cada una

| Estructura | Mejor para |
|---|---|
| Arreglo ordenado | Búsqueda frecuente, acceso por posición, getMin/Max |
| Lista encadenada | Inserción/borrado frecuente, sin necesidad de acceso por índice |
| Lista como Cola (unbounded) | FIFO con crecimiento dinámico |
| Lista como Pila | LIFO sin límite de tamaño |

## Notas
- La interface `Iterator` de Java es implementada por una clase interna (*inner class*) de la lista.
- El `remove()` del `Iterator` es opcional pero muy útil para borrado durante iteración.

## Preguntas
- ¿Por qué la búsqueda binaria requiere que el arreglo esté ordenado?
- ¿Qué es una clase interna en Java y para qué se usa en la lista?
- ¿Cuándo conviene una lista ordenada sobre un arreglo ordenado?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]
