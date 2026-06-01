---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Discrete Math.base|Discrete Math]]"
temas:
  - Caminos
  - Recorridos
  - Conexidad
  - Eulerianos
  - Hamiltonianos
  - Whitney
  - Menger
---
# Discrete Math — Caminos y Conexidad

## Resumen

### Definiciones Básicas (Sección 1.3)
- **Camino (walk) W:** secuencia alternada de vértices y aristas.
- **Longitud de un camino:** número de aristas.
- **Camino cerrado:** el vértice inicial = final.
- **Subcamino:** subsecuencia consecutiva de W.
- **Distancia d(u,v):** largo del camino más corto entre u y v (∞ si no existe).

### Tipos de Caminos (Sección 1.4)
| Tipo | Restricción |
|---|---|
| **Recorrido** | No repite aristas |
| **Camino simple** | No repite vértices (tampoco aristas) |
| **Camino trivial** | Un solo vértice, ninguna arista |

**Conexidad:** un grafo es **conexo** si para todo par (u, v) existe un camino u-v.  
**Dígrafo fuertemente conexo:** todos los vértices son mutuamente alcanzables.

### Recorridos Eulerianos
- **Recorrido Euleriano:** recorrido que usa *todas* las aristas exactamente una vez.
- **Circuito Euleriano:** recorrido euleriano cerrado.
- **Grafo Euleriano:** contiene un circuito euleriano.

> Un grafo conexo es Euleriano ⟺ todos sus vértices tienen grado par.

### Caminos Hamiltonianos
- **Camino Hamiltoniano:** visita todos los vértices exactamente una vez.
- **Ciclo Hamiltoniano:** camino hamiltoniano que regresa al inicio.
- No existe teorema sencillo equivalente al de Euler para determinar si existe.

### Conexidad k-arista y k-vértice
- **kₑ(G):** mínimo de aristas a eliminar para desconectar G.
- **kᵥ(G):** mínimo de vértices a eliminar para desconectar G.

**Corolario 3.1.6 (IMPORTANTE):**
$$k_v(G) \le k_e(G) \le \delta_{min}(G)$$

### Teorema de Whitney (3.1.7)
Sea G un grafo conexo con ≥ 3 vértices. G es **2-conexo** ⟺ para cada par de vértices existe al menos 2 caminos internamente disjuntos entre ellos.

**Corolario 3.1.8:** G es 2-conexo ⟺ para cada par de vértices existe un ciclo que los contiene.

### Teorema de Menger (3.3.1)
Sea u y v dos vértices no adyacentes en un grafo conexo G. El máximo número de **caminos internamente disjuntos** u-v = mínimo número de vértices para separar u de v.

## Notas
- Camino simple ⊂ recorrido ⊂ camino.
- Propiedades de la proposición 1.4.4: todo recorrido cerrado no trivial contiene un ciclo como subcamino.

## Preguntas
- ¿Cuál es la condición necesaria y suficiente para que un grafo sea Euleriano?
- ¿Por qué no hay condición simple para Hamiltoniano?
- ¿Qué significa 2-conexo intuitivamente?

[[Discrete Math.base|Discrete Math]]
