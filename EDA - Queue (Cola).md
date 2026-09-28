---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Queue
  - Cola
  - FIFO
  - Bounded
  - Unbounded
---
# EDA — Queue (Cola)

## Resumen

### Queue / Cola / FIFO
Colección de datos ordenada por **orden de llegada**. Acceso por dos extremos: **FIRST** (el más antiguo) y **LAST** (el más reciente).

**Operaciones:**
| Operación | Descripción |
|---|---|
| `queue(e)` | Agrega elemento → pasa a ser el nuevo LAST |
| `dequeue()` | Quita FIRST (destructiva; error si vacía) |
| `peek()` | Retorna FIRST sin quitarlo (error si vacía) |
| `isEmpty()` | true si no hay elementos |
| `size()` | (opcional) cantidad de elementos |

**Casos de uso:**
- **Recursos compartidos**: impresora única → pedidos se encolan.
- **Múltiples recursos administrados centralizadamente** (ej: cajas de un supermercado).
- **BFS en grafos**: algoritmo necesita posponer el procesamiento de vértices.
- **Pipes entre procesos**: productor/consumidor asíncronos con distinta velocidad.

### Implementaciones

**Bounded Queue** (tamaño límite conocido):
- Puede usarse arreglo circular con espacio pre-alocado.
- `enqueue`/`dequeue` en O(1) gracias al tratamiento circular.
- Agregar método privado `isFull()`.

**Unbounded Queue** (sin límite):
- Lista lineal simplemente encadenada → push al final, pop al frente, ambas O(1).
- ArrayList no es apto porque inserción/borrado al frente es O(N).

> **Regla:** para unbounded → lista encadenada; para bounded → indistinto.

## Notas
- A diferencia del Stack (solo acceso por tope), la Queue tiene dos extremos distinguidos.
- Java: `java.util.LinkedList` implementa `Queue`.

## Preguntas
- ¿Por qué usar arreglo para bounded queue es seguro pero no para unbounded?
- ¿Cómo funciona el tratamiento circular de un arreglo para implementar la queue?
- ¿Cuál es la diferencia entre una queue y un stack en términos de orden de salida?

[Data Structures and Algorithms](Categories/Data%20Structures%20and%20Algorithms.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [EDA - Stack](EDA%20-%20Stack.md) — FIFO vs LIFO
- [EDA - Listas Lineales](EDA%20-%20Listas%20Lineales.md) — implementación subyacente

**Otras materias**

- **Protos**  [5. Protos - Transporte](5.%20Protos%20-%20Transporte.md) — buffers y ventana deslizante
- **SO**  [Scheduling](Scheduling.md) — colas de listos y de prioridad

<!-- notas-relacionadas:fin -->
