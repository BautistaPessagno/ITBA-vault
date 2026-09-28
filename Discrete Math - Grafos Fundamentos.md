---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Discrete Math.base|Discrete Math]]"
temas:
  - Grafos
  - Definiciones
  - Grado
  - Bipartito
  - Grafo Regular
  - Tipos de Grafos
---
# Discrete Math — Grafos: Fundamentos

## Resumen

### Definición de Grafo
Un grafo G = (V, E) con **V** = vértices y **E** = aristas.  
**Grafo simple:** sin lazos ni multiaristas.  
**Multigrafo:** puede tener multiaristas.  
**Dígrafo:** aristas tienen dirección (cola → cabeza).

### Grado de un Vértice
- **gr(v):** número de aristas propias incidentes en v + 2 × (lazos en v).
- **Dígrafo:** grado de salida (cola en v) y grado de entrada (cabeza en v).

### Teoremas Clave

**Teorema 1.1.2 (Euler):** La suma de los grados es el doble del número de aristas:
$$\sum_{i=1}^{n} gr(v_i) = 2|E|$$

**Corolario 1.1.3:** En todo grafo hay un número par de vértices con grado impar.

**Proposición 1.1.1:** Un grafo simple no trivial debe tener al menos un par de vértices con el mismo grado.

**Para Dígrafos (Teo 1.1.6):** Tanto la suma de grados de salida como la de entrada es igual al número de aristas.

### Tipos de Grafos

| Tipo | Descripción |
|---|---|
| **Grafo Completo Kₙ** | n vértices, todos conectados entre sí |
| **Grafo Bipartito** | V se divide en dos conjuntos; aristas solo entre conjuntos |
| **Grafo Bipartito Completo Kₘ,ₙ** | m y n vértices; cada uno del primer grupo conectado a todos del segundo |
| **Grafo k-regular** | Todos los vértices tienen grado k |
| **Path Pₙ** | n vértices en línea recta; \|V\| = \|E\| + 1 |
| **Ciclo Cₙ** | n vértices en círculo; \|V\| = \|E\| |
| **HiperCubo Qₙ** | Vértices = cadenas de bits de largo n; aristas si difieren en 1 bit |

**Grafo de Petersen:** 3-regular, famoso ejemplo no-planar y sin ciclo hamiltoniano.

### Subgrafo
Un subgrafo H de G es un grafo con V_H ⊆ V_G y E_H ⊆ E_G.

### Isomorfismo (Sección 2.3)
Dos grafos son isomorfos si existe una biyección f: V_G → V_H que:
- Preserva la cantidad de multiaristas entre cada par de vértices (multigrafo).
- Preserva la adyacencia en grafos simples.

## Notas
- Un grafo bipartito no puede tener lazos.
- Teo 1.1.5: dada una secuencia no negativa con suma par → existe un grafo con esos grados.
- **X(bipartito) = 2** (si tiene al menos una arista).

## Preguntas
- ¿Por qué la suma de grados siempre es par?
- ¿Qué diferencia hay entre grafo regular y grafo completo?
- ¿Cuántas aristas tiene Kₙ?

[Discrete Math](Categories/Discrete%20Math.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Discrete Math)**

- [Discrete Math - Caminos y Conexidad](Discrete%20Math%20-%20Caminos%20y%20Conexidad.md) — tema siguiente
- [Discrete Math - Árboles y Recorridos](Discrete%20Math%20-%20Árboles%20y%20Recorridos.md) — árboles

**Otras materias**

- **EDA**  [EDA - Grafos](EDA%20-%20Grafos.md) — representación e implementación

<!-- notas-relacionadas:fin -->
