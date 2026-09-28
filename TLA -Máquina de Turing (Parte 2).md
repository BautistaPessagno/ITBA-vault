---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-06-03"
Materia: "[[TLA.base|TLA]]"
temas:
  - Recursivo y Recursivamente Enumerable
  - Gramáticas tipo 0 y lenguajes RE
  - Decidible e Indecidible
  - Problema de la Parada
  - Máquina de Turing Universal
  - Tesis de Church-Turing
  - Tipos de Problemas (tratable/intratable)
  - Jerarquía de Chomsky (lenguajes, gramáticas y máquinas)
---
# TLA - Máquina de Turing (Parte 2) (Resumen)

Resumen de la clase 11 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

> [!tip] Continuación
> Esta nota cubre los temas de **computabilidad y decidibilidad** de la segunda parte de la clase 11. Los fundamentos de la MT (definición formal, función de transición, extensiones, AAL y funciones recursivas) están en [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md).

---

## Motivación

La **teoría de autómatas** es el estudio de máquinas abstractas. Su objetivo es modelar el funcionamiento de la computadora ideal y sus resultados permiten:

- **Diseñar y construir software** (compiladores, reconocedores, intérpretes).
- **Determinar qué problemas son indecidibles**: hay preguntas que ningún algoritmo puede responder para todos los casos.

La Máquina de Turing (MT), introducida en la [Parte 1](TLA%20-Máquina%20de%20Turing.md), es el modelo más general de computación. Esta clase responde: dado ese modelo, ¿qué lenguajes puede reconocer? ¿Cuáles puede *decidir*? ¿Y qué problemas escapan a cualquier algoritmo?

---

## Repaso — Lenguaje aceptado por una MT

> [!info] Recordatorio
> $$L(M) = \{\omega \in \Sigma^* \mid q_0\omega \mapsto^* \alpha p \beta,\; p \in F\}$$
>
> - M se detiene en estado **final** $\Rightarrow$ $\omega \in L(M)$.
> - M se detiene en estado **no final** $\Rightarrow$ $\omega \notin L(M)$.
> - M **no se detiene** $\Rightarrow$ $\omega \notin L(M)$.
>
> Al conjunto de lenguajes aceptados por alguna MT se los denomina **lenguajes recursivamente enumerables**. Ver detalle completo en [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md).

---

## Recursivo y Recursivamente Enumerable

Los dos términos tienen una historia:

- El término **recursivo** es sinónimo de **decidible**. Antes de la existencia de las computadoras, los cálculos basados en la *recursión* se empleaban habitualmente como concepto de computación, en lugar de la iteración o los bucles (como ocurre en los lenguajes de programación funcional). Decir que un problema era *recursivo* aseguraba que siempre terminaría.

- El término **enumerable** deriva del hecho de que son precisamente estos lenguajes cuyos strings pueden ser *enumerados* (listados) por una MT: se podría enumerar todos los elementos del lenguaje en cierto orden.

---

## Gramáticas tipo 0 y Lenguajes Recursivamente Enumerables

> [!info] Lenguaje recursivamente enumerable
> Un lenguaje $L$ es **recursivamente enumerable** (RE) si existe una MT $M$ tal que $L(M) = L$.
>
> Las **gramáticas tipo 0** (sin restricciones) generan exactamente los mismos lenguajes que reconocen las MT.

> [!info] Lenguaje recursivo o decidible
> Un lenguaje $L$ es **recursivo** o **decidible** cuando existe una MT $M$ tal que $L(M) = L$ **y $M$ se detiene para toda entrada**.
>
> - Si es decidible, entonces $L$ es RE (se detiene para todas las palabras).
> - Para toda entrada, se detiene y determina si es aceptada o no (si no es aceptada, se detiene en estado no final).

> [!warning] Lenguaje indecidible
> Un lenguaje $L$ es **indecidible** cuando no es decidible: no hay ninguna MT que se detenga para **todas** las entradas. Esto significa que:
>
> - O bien $L$ **no es recursivamente enumerable**: hay palabras de $L$ para las que ninguna MT las reconoce.
> - O bien $L$ **es recursivamente enumerable pero no decidible**: la MT no se detiene para algunas entradas.

Diagrama de inclusión de la jerarquía:

$$\text{Regular} \subsetneq \text{LLC} \subsetneq \text{Decidible} \subsetneq \text{Turing-reconocible (RE)} \subsetneq \text{No enumerable}$$

---

## Ejemplo — MT y análisis de cadenas

> [!example] MT con estados $q_0,\; q_1,\; q_f$
>
> Tabla de transiciones ($B$ = espacio en blanco):
>
> | Estado | $0$ | $1$ | $B$ |
> |--------|-----|-----|-----|
> | $q_0$ | $(q_1, 0, D)$ | $(q_f, 0, D)$ | — |
> | $q_1$ | — | $(q_1, 1, I)$ | $(q_0, B, I)$ |
> | $q_f$ | — | — | $(q_f, B, I)$ |
>
> $q_f$ es el único estado final.
>
> **Cadena "0"** — $q_0\, 0$:
> $$q_0\, 0 \;\mapsto\; 0\, q_1\, B \;\mapsto\; q_0\, 0\, B \;\mapsto\; 0\, q_1\, B \;\mapsto\; \cdots \quad \text{(bucle infinito — no acepta)}$$
>
> **Cadena "1"** — $q_0\, 1$:
> $$q_0\, 1 \;\mapsto\; 0\, q_f\, B \;\mapsto\; q_f\, 0\, B \quad \text{(sin transición en } q_f \text{ sobre } 0 \text{, se detiene en estado final — acepta)}$$
>
> **Cadena "00"** — $q_0\, 00$:
> $$q_0\, 0\, 0 \;\mapsto\; 0\, q_1\, 0 \quad \text{(sin transición en } q_1 \text{ sobre } 0 \text{, se detiene en } q_1 \text{, no final — no acepta)}$$
>
> **Cadena "10"** — $q_0\, 10$:
> $$q_0\, 1\, 0 \;\mapsto\; 0\, q_f\, 0 \quad \text{(sin transición en } q_f \text{ sobre } 0 \text{, se detiene en estado final — acepta)}$$

---

## Problema de la Parada

> [!warning] Problema de la parada — indecidible
> **Existen máquinas de Turing para las que no es posible decidir si se van a detener ante una cadena de entrada.**
>
> Es decir: no existe ninguna MT $D$ que, dado el par $\langle M, \omega \rangle$ (descripción de una MT y una cadena), responda siempre (en tiempo finito) si $M$ se detiene con la entrada $\omega$.

El Problema de la Parada es el ejemplo paradigmático de un problema **indecidible**: existe una MT que lo reconoce (es RE) pero ninguna MT lo decide.

---

## Máquina de Turing Universal

> [!info] Definición
> Una **Máquina de Turing Universal** (MTU) es una MT capaz de **simular el procesamiento de cualquier otra MT**.
>
> Recibe como entrada el par $\langle M, \omega \rangle$ (codificación de una MT $M$ y una cadena $\omega$) y determina si $\omega \in L(M)$.

La MTU utiliza múltiples cintas:
1. La **cinta de entrada** contiene la descripción de $M$ y la cadena $\omega$.
2. La **cinta de simulación** replica el contenido de la cinta de $M$.
3. La **cinta de estado** almacena el estado actual de $M$.

La MTU es la base teórica del **computador programable**: una sola máquina puede ejecutar cualquier programa al recibirlo como dato.

---

## Tesis de Church-Turing

El problema de la decidibilidad tiene raíces históricas profundas:

- **Hilbert** (1900): ¿es posible encontrar un algoritmo, compuesto por una cantidad finita de pasos, que permita determinar la verdad o falsedad de cualquier proposición matemática? (*Entscheidungsproblem*)
- **Gödel** (1931): Teorema de incompletitud — un sistema axiomático es incompleto y existen sentencias cuya verdad o falsedad no se pueden probar.
- **Turing** (1936): primer modelo teórico de lo que luego sería un computador programable, demostrando que el *Entscheidungsproblem* es indecidible.

> [!info] Tesis de Church-Turing
> La **hipótesis de Church** o **Tesis de Church-Turing** sostiene que:
>
> > Una función es **computable** si y sólo si puede ser computada por una Máquina de Turing.

> [!warning] Estado epistémico
> La Tesis de Church-Turing **no ha podido ser demostrada**. Se asume cierta porque todos los demás formalismos de cálculo propuestos (λ-cálculo, funciones recursivas de Kleene, autómatas, etc.) terminan siendo equivalentes a las MT.

---

## Tipos de Problemas

Los problemas computacionales se clasifican según su tratabilidad:

```
              Problemas
             /         \
       Decidibles    Indecidibles
       /       \
  Tratables  Intratables
```

| Categoría | Descripción | Ejemplos |
|-----------|-------------|---------|
| **Tratables** | Decidibles con algoritmo eficiente (polinomial) | Ordenar, buscar, parsear |
| **Intratables** | Decidibles pero sin algoritmo eficiente conocido | Coloreo de grafos (mínimo nro. de colores sin vértices adyacentes del mismo color) |
| **Indecidibles** | No existe ninguna MT que siempre se detenga y dé la respuesta | Dado dos GLC, determinar si generan el mismo lenguaje; determinar si una GLC es ambigua |

---

## Lenguajes, Gramáticas y Máquinas — Jerarquía de Chomsky

La jerarquía completa que relaciona cada tipo de lenguaje con su autómata y gramática correspondiente:

| Lenguaje | Tipo (Chomsky) | Máquina reconocedora | Gramática generadora |
|----------|:--------------:|----------------------|----------------------|
| Regular | Tipo 3 | Autómata Finito (AF) | Regular |
| Libre de Contexto | Tipo 2 | Autómata de Pila (AP) | Libre de Contexto (GLC) |
| Sensible al Contexto | Tipo 1 | Autómata Linealmente Acotado (AAL) | Sensible al Contexto |
| Recursivamente Enumerable | Tipo 0 | Máquina de Turing (MT) | Tipo 0 (sin restricciones) |

> [!info] Propiedades de la jerarquía
> - Cada clase es un subconjunto estricto de la siguiente: $\text{Reg} \subsetneq \text{LLC} \subsetneq \text{LSC} \subsetneq \text{RE}$.
> - Los lenguajes **no enumerables** (no RE) existen pero ninguna MT los puede reconocer.
> - Ver los fundamentos de AAL en [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md).

---

## Recursos

- **En castellano:** [https://youtu.be/7AUIx0f1N14](https://youtu.be/7AUIx0f1N14)
- **En inglés:** [https://www.youtube.com/watch?v=macM_MtS_w4](https://www.youtube.com/watch?v=macM_MtS_w4)

---

## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md) — parte 1
- [TLA -Guía Repaso Final (poco tiempo)](TLA%20-Guía%20Repaso%20Final%20%28poco%20tiempo%29.md) — repaso final

**Otras materias**

- **EDA**  [EDA - Algoritmos y Complejidad](EDA%20-%20Algoritmos%20y%20Complejidad.md) — problemas tratables e intratables

<!-- notas-relacionadas:fin -->
