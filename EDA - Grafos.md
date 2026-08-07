---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Grafos
  - Dijkstra
  - Matriz de Adyacencia
  - Lista de Adyacencia
---
# EDA — Grafos

## Resumen

### Representaciones de Grafos
Un grafo G = (V, E) tiene vértices (V) y aristas (E).

**Formas de representar:**
| Representación | Ideal para | Notas |
|---|---|---|
| Matriz de adyacencia | Grafos **densos** | `A[u][v] = 1` si hay arista. `Aⁿ[u][v]` = cantidad de caminos de largo n. |
| Lista de adyacencia | Grafos **esparcidos** | Cada vértice tiene una lista de vecinos |
| Matriz de incidencia | — | Filas = vértices, columnas = aristas |
| Lista de incidencia | — | — |

La cátedra usa principalmente **matriz de adyacencia**.

**Construcción:** se puede usar un *Graph Factory* o un *Graph Builder*.

### Dijkstra — Camino Mínimo
Algoritmo para encontrar el camino de menor peso desde un vértice origen a todos los demás en un grafo con pesos **no negativos**.

**Idea:**
1. Inicializar distancias: 0 para el origen, ∞ para los demás.
2. En cada iteración: extraer el vértice no visitado con menor distancia.
3. Actualizar distancias de sus vecinos.
4. Marcar como visitado.

**Complejidad:**
- Con arreglo simple: O(V²)
- Con heap de prioridad: O((V + E) log V)

## Notas
- BFS usa una Queue; DFS usa un Stack (o recursión).
- Los grafos dirigidos se representan con matrices de adyacencia asimétrica.
- Dijkstra NO funciona con aristas de peso negativo → usar Bellman-Ford en ese caso.

## Preguntas
- ¿Cuándo conviene lista de adyacencia sobre matriz de adyacencia?
- ¿Por qué `Aⁿ[u][v]` da la cantidad de caminos de largo n?
- ¿Cuál es la limitación principal de Dijkstra?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [[EDA - Árboles]] — árboles como caso particular

**Otras materias**

- **Discrete Math**  [[Discrete Math - Caminos y Conexidad]] — teoría de caminos
- **Discrete Math**  [[Discrete Math - Grafos Fundamentos]] — definiciones formales
- **Protos**  [[7. Protos - Routing]] — Dijkstra en el ruteo real
- **TLA**  [[TLA -Análisis Semántico]] — grafo de dependencias y orden topológico

<!-- notas-relacionadas:fin -->
