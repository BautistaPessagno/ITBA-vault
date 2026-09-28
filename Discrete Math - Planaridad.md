---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Discrete Math.base|Discrete Math]]"
temas:
  - Planaridad
  - Fórmula de Euler
  - Kuratowski
  - Homeomorfismo
---
# Discrete Math — Planaridad

## Resumen

### Grafo Plano
Un grafo es **plano** si puede dibujarse en el plano sin que sus aristas se crucen.  
Un **dibujo plano** es una inmersión plana específica.

### Fórmula de Euler (Sección 4)
Sea G un **grafo plano conexo** con:
- v = número de vértices
- e = número de aristas
- r = número de regiones (incluyendo la región exterior)

$$v - e + r = 2$$

**Consecuencia:** Para cualquier región R: $\sum g(R_i) = 2e$ (donde g = cantidad de aristas en el borde de esa región).

### Cotas para Grafos Planos
**Teorema 4.1.1:** Para G conexo simple con ≥ 3 aristas:
$$|E| \le 3|V| - 6$$

**Teorema 4.1.5 (Bipartito):** Para G bipartito plano conexo simple:
$$|E| \le 2|V| - 4$$

**Aplicaciones (corolarios):**
- Si |E| > 3|V| - 6 → **no es plano**.
- K₅ no es plano (5 vértices, 10 aristas: 10 > 3·5-6 = 9).
- K₃,₃ no es plano (bipartito: 9 aristas > 2·6-4 = 8).

### Homeomorfismo
Dos grafos son **homeomorfos** si se puede obtener uno del otro por subdivisión/contracción de aristas.  
La planaridad se preserva bajo homeomorfismo.

### Teorema de Kuratowski (4.3.1) ⭐ MUY IMPORTANTE
$$G \text{ es plano} \iff G \text{ no contiene ningún subgrafo homeomorfo a } K_5 \text{ ni a } K_{3,3}$$

**Consecuencia (Teo 4.1.8):** Si G contiene un subgrafo no plano → G no es plano.

### Bloques (Sección 4.2)
**Bloque:** subgrafo máximo sin vértices de corte.  
- Dos bloques distintos tienen a lo sumo un vértice en común.
- Los conjuntos de aristas de los bloques forman una partición de E_G.
- Un vértice es de **corte** ⟺ está en dos bloques distintos.

**Algoritmo de Planaridad:** G es plano ⟺ todos sus bloques son planos.

## Notas
- En la suma de grados de regiones: cada arista es compartida por exactamente 2 regiones (o cuenta doble si es borde).
- Para la heurística de construir grafos no planos: empezar con K₅ o K₃,₃ y agregar vértices/aristas.

## Preguntas
- ¿Qué significa que K₅ sea el grafo completo más pequeño no plano?
- ¿Cómo se usa la fórmula de Euler para demostrar que un grafo no es plano?
- ¿Qué es un vértice de corte?

[Discrete Math](Categories/Discrete%20Math.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Discrete Math)**

- [Discrete Math - Grafos Fundamentos](Discrete%20Math%20-%20Grafos%20Fundamentos.md) — definiciones de grafo
- [Discrete Math - Coloreo](Discrete%20Math%20-%20Coloreo.md) — planaridad y número cromático

**Otras materias**

- **EDA**  [EDA - Grafos](EDA%20-%20Grafos.md) — representación de grafos

<!-- notas-relacionadas:fin -->
