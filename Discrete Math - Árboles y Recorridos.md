---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Discrete Math.base|Discrete Math]]"
temas:
  - Árboles
  - Árboles Generadores
  - Recorridos
  - Ciclos Eulerianos
  - Matrices de Adyacencia
---
# Discrete Math — Árboles, Recorridos y Matrices

## Resumen

### Árbol
Un árbol es un grafo **conexo** y **acíclico** (sin ciclos).

**Propiedades equivalentes de un árbol T con n vértices:**
- T es conexo y acíclico.
- T tiene exactamente n-1 aristas.
- Entre cada par de vértices hay exactamente un camino simple.
- Es conexo minimal (quitar cualquier arista lo desconecta).
- Es acíclico maximal (agregar cualquier arista crea exactamente un ciclo).

**Número cromático:** X(árbol no trivial) = 2 (son bipartitos).

### Árboles Generadores (Spanning Trees)
Un **árbol generador** de G es un árbol que contiene todos los vértices de G.  
Todo grafo conexo tiene al menos un árbol generador.

**Árbol Generador Mínimo (MST):** árbol generador con peso total mínimo en grafos ponderados.  
**Algoritmo de Kruskal (Greedy):** agregar aristas de menor a mayor peso sin crear ciclos → MST.

### Matrices de Adyacencia
Sea Aᴳ la matriz de adyacencia de G (n×n; Aᴳ[u,v] = número de aristas u-v).

**Proposición 2.5.4:** El valor de la entrada [Aᴳ^r][u,v] es el número de caminos de largo r entre u y v.

**Proposición 2.5.6 (Dígrafos):** Ídem para caminos dirigidos.

**Proposición 2.5.5:** En un dígrafo, la suma de la fila i = grado de salida de vᵢ; suma de columna j = grado de entrada de vⱼ.

### Recorridos Eulerianos (repaso)
- **Recorrido Euleriano:** usa todas las aristas exactamente una vez.
- **Circuito Euleriano:** recorrido euleriano cerrado.
- **Condición:** G conexo es Euleriano ⟺ todos sus vértices tienen grado par.
- **Un recorrido euleriano abierto** existe ⟺ exactamente 2 vértices tienen grado impar.

### Caminos Hamiltonianos
- **Camino Hamiltoniano:** visita cada vértice exactamente una vez.
- **Ciclo Hamiltoniano:** regresa al origen.
- No existe condición necesaria y suficiente simple (problema NP-completo).

## Notas
- Kruskal necesita ordenar las aristas por peso: O(E log E).
- La potenciación de la matriz de adyacencia revela la estructura de caminos del grafo.

## Preguntas
- ¿Por qué un árbol con n vértices tiene exactamente n-1 aristas?
- ¿Cómo se usa la matriz de adyacencia para contar caminos?
- ¿Cuál es la diferencia entre un recorrido Euleriano y un camino Hamiltoniano?

[[Discrete Math.base|Discrete Math]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Discrete Math)**

- [[Discrete Math - Caminos y Conexidad]] — tema anterior
- [[Discrete Math - Grafos Fundamentos]] — definiciones de grafo

**Otras materias**

- **EDA**  [[EDA - Grafos]] — árbol generador
- **EDA**  [[EDA - Árboles]] — BST, AVL y árboles B
- **SO**  [[File System]] — jerarquía de directorios

<!-- notas-relacionadas:fin -->
