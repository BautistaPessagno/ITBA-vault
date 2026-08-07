---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Backtracking
  - Greedy
  - Divide y Conquista
  - Programación Dinámica
  - Búsqueda Exhaustiva
---
# EDA — Tipos de Algoritmos y Heurísticas

## Resumen

### Espacio de Soluciones
Muchos problemas (juegos, optimización) se modelan como un **grafo implícito** donde:
- Cada nodo representa una solución parcial.
- El nodo raíz = configuración inicial.
- Una arista nodo₁→nodo₂ existe si una regla válida lleva de nodo₁ a nodo₂.

### Técnicas Algorítmicas

#### 1. Divide y Conquista
Descompone el problema de tamaño N en subproblemas iguales. Resuelve cada subproblema de forma recursiva y combina los resultados.  
**Ej:** MergeSort, QuickSort, búsqueda binaria en arreglo ordenado, búsqueda en BST.

#### 2. Algoritmos Voraces (Greedy)
En cada etapa elige el **óptimo local** esperando llegar al óptimo global.  
No siempre garantiza el óptimo global.  
**Ej:** Kruskal (mínimo árbol generador).

#### 3. Búsqueda Exhaustiva (Fuerza Bruta)
Enumera y calcula **todas las soluciones posibles**. Evaluación de la restricción ("la mejor") se hace solo en las hojas.  
Implementación típica: recursión (DFS con Stack) o iterativo con Queue (BFS).

**Algoritmo:**
1. Si el nodo no puede expandirse más → imprimir/retornar resultado.
2. Para cada expansión posible: agregar caso pendiente → explorar → deshacer caso.

#### 4. Backtracking
**Búsqueda exhaustiva + poda:** ante restricciones, no expande nodos que no pueden conducir a la solución.

**Condición de poda:** si el acumulado actual + mínimo posible restante supera el umbral → no expandir más.  
**Ej:** N-Queens.

#### 5. Programación Dinámica
Almacena subresultados ya calculados para reutilizarlos en lugar de recalcularlos.  
**Ej:** Fibonacci, Levenshtein (distancia de edición), Dijkstra, Ackermann.

### Comparación

| Técnica | Cuándo usar |
|---|---|
| Greedy | Cuando el óptimo local es suficiente / no se necesita exactitud |
| Backtracking | Hay restricciones que permiten podar ramas |
| Prog. Dinámica | Hay subproblemas repetidos que no necesitan recalcularse |
| Divide y Conquista | El problema se descompone en subproblemas independientes del mismo tamaño |

## Notas
- Backtracking es superior a búsqueda exhaustiva cuando hay buenas restricciones de poda.
- Programación dinámica = búsqueda exhaustiva + memoización.

## Preguntas
- ¿En qué se diferencia backtracking de búsqueda exhaustiva?
- ¿Cuándo Greedy no da el óptimo global?
- ¿Por qué Fibonacci se beneficia de programación dinámica?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [[EDA - Algoritmos y Complejidad]] — por qué hacen falta heurísticas
- [[EDA - Grafos]] — greedy y backtracking sobre grafos

**Otras materias**

- **Discrete Math**  [[Discrete Math - Coloreo]] — coloreo greedy y backtracking
- **PI**  [[PI - Recursividad en C]] — backtracking y divide y conquista

<!-- notas-relacionadas:fin -->
