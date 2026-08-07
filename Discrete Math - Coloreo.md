---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Discrete Math.base|Discrete Math]]"
temas:
  - Coloreo
  - Número Cromático
  - Cliques
---
# Discrete Math — Coloreo de Grafos

## Resumen

### Coloreo de Vértices
Un **coloreo** de G asigna colores a los vértices tal que vértices adyacentes tienen distinto color.  
El **número cromático X(G)** es el mínimo de colores necesarios.

### Propiedades Clave

**Proposición 5.2.1:** X(G) = 1 ⟺ G no tiene aristas.

**Proposición 5.2.2:** Si G tiene k vértices mutuamente adyacentes (clique) → X(G) ≥ k.

**Corolario 5.2.3:** X(G) ≥ ω(G), donde ω(G) es el tamaño del clique más grande.

**Proposición 5.2.4:** Si H es subgrafo de G → X(G) ≥ X(H).

**Proposición 5.2.5:** X(G + H) = X(G) + X(H) (suma de grafos).

### Números Cromáticos de Grafos Comunes

| Grafo | X |
|---|---|
| Grafo sin aristas | 1 |
| Grafo bipartito (con aristas) | 2 |
| Kₙ | n |
| Path Pₙ (n ≥ 2) | 2 |
| Árbol (no trivial) | 2 |
| Ciclo C₂ₙ (par) | 2 |
| Ciclo C₂ₙ₊₁ (impar) | 3 |
| HiperCubo Qₙ | 2 |

### Clique (ω)
Un **clique** es un subconjunto de vértices todos mutuamente adyacentes. ω(G) = tamaño del clique más grande.

### Relación con Bipartitos
Un grafo es bipartito ⟺ no tiene ciclos de longitud impar (Teorema 1.4.3).  
Todo grafo bipartito con aristas tiene X = 2.

## Notas
- ω(G) es solo una cota inferior; X(G) puede ser mayor que ω(G) (ej: grafos de Mycielski).
- El coloreo de aristas (cromaticidad de aristas) es diferente al de vértices.
- Problema P vs NP: determinar X(G) es NP-difícil en general.

## Preguntas
- ¿Por qué los ciclos impares necesitan 3 colores?
- ¿Puede X(G) > ω(G)? ¿Cuándo?
- ¿Por qué X(bipartito con aristas) = 2?

[[Discrete Math.base|Discrete Math]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Discrete Math)**

- [[Discrete Math - Planaridad]] — teorema de los 4 colores
- [[Discrete Math - Grafos Fundamentos]] — grafos bipartitos

**Otras materias**

- **EDA**  [[EDA - Grafos]] — algoritmos sobre grafos
- **EDA**  [[EDA - Tipos de Algoritmos y Heurísticas]] — coloreo greedy y backtracking

<!-- notas-relacionadas:fin -->
