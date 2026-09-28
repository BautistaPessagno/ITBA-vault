---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-07-02"
Materia: "[[TLA.base|TLA]]"
temas:
  - Repaso final
  - Estructura del examen
  - Lema de bombeo
  - Demostraciones
  - Análisis sintáctico
  - Máquinas de Turing
  - Gramáticas con atributos
---

# Guía de Repaso Final TLA (optimizada para poco tiempo)

Basada en 3 finales: **Dic 2022**, **Julio 2023** y **Dic 2024** (+ compilado de finales resueltos). Los tres tienen **la misma estructura fija**, así que sabés casi exactamente qué te van a tomar.

> [!important] Dato clave para administrar el tiempo
> El final es **~85% teoría** (definiciones, demostraciones, multiple choice, V/F). **Solo el Ejercicio 5 es "hacer" un autómata/MT.** Aprobás con **5 puntos** de 10. Priorizá memorizar definiciones y demostraciones exactas antes que resolver ejercicios largos.

---

## 1. Estructura fija del final (7 ejercicios)

| Ej | Pts | Tema (fijo) | Tipo de consigna |
|----|-----|-------------|------------------|
| 1 | 1,5 | Introducción / Gramáticas **o** Autómatas Finitos | Explicar o **definir recursivamente** un concepto |
| 2 | 1,5 | Autómatas Finitos **o** Autómatas de Pila | **Demostrar** un teorema |
| 3 | 2,0 | Reg / GLC / Análisis sintáctico | **Multiple choice** (4 ítems) — *el de más puntaje* |
| 4 | 1,5 | Expr. Regulares / Reg / AF | **Verdadero/Falso** con justificación breve |
| 5 | 1,0 | Autómatas de Pila **o** Máquina de Turing | **Práctico**: corregir/completar transiciones |
| 6 | 1,0 | Gramáticas con atributos **o** Introducción | Explicar o definir un concepto |
| 7 | 1,5 | Máquinas de Turing **o** Gramáticas con atributos | **Relacionar 3 conceptos** en una sola frase |

Ojo: "el examen se evalúa por lo que está escrito", usando **vocabulario y algoritmos de la teórica**. Las justificaciones cuentan.

---

## 2. Prioridades (qué rinde más por minuto)

### 🔴 Alta prioridad — aparece en los 3 finales y/o mucho puntaje

1. **Lema de bombeo (regular y LLC).** Cae en Ej 3 y Ej 4 casi siempre. No te piden aplicarlo tanto como **saber cómo se demuestra** y **la forma exacta de las condiciones** (los distractores del multiple choice cambian `|xyz|≤P`, `|xz|≥1`, `∀i≥0`, etc.).
   → [TLA -Formas Normales y Lema de Bombeo CFL](TLA%20-Formas%20Normales%20y%20Lema%20de%20Bombeo%20CFL.md), [TLA -Lenguajes Regulares](TLA%20-Lenguajes%20Regulares.md)
2. **Definiciones recursivas exactas** (Ej 1 y 6): función de transición extendida `δ̂` (AFD), clausura-λ, **forma sentencial**. Te las piden literal, recursivas (base + paso inductivo).
   → [TLA -Autómatas Finitos Determinísticos](TLA%20-Autómatas%20Finitos%20Determinísticos.md), [TLA -Autómatas Finitos No Determinísticos](TLA%20-Autómatas%20Finitos%20No%20Determinísticos.md), [TLA -intro resumen](TLA%20-intro%20resumen.md)
3. **Demostraciones "de librito" (Ej 2).** Las que se repiten:
   - AFD ⇒ AFND-λ (2022)
   - Minimización = equivalente y mínimo (algoritmo del **conjunto cociente**) (2023)
   - AP por **vaciado de pila** ⇔ AP por **estado final** (2024)
   → [TLA -Autómatas de Pila](TLA%20-Autómatas%20de%20Pila.md), [TLA -Autómatas Finitos Determinísticos](TLA%20-Autómatas%20Finitos%20Determinísticos.md)
4. **Multiple choice de análisis sintáctico** (Ej 3, 2 pts): condiciones **LL(1)**, forma de un **item LR(1)**, definición de **Primeros/Siguientes**, acción **desplazar/reducir**, **FNC/FNG**.
   → [TLA -Análisis Sintáctico](TLA%20-Análisis%20Sintáctico.md), [TLA -Análisis Ascendente](TLA%20-Análisis%20Ascendente.md)

### 🟡 Media prioridad

5. **Gramáticas con atributos** (Ej 6/7): **sintetizado vs heredado**, definición dirigida por la sintaxis (DDS), S-atribuida/ETDS postfijo.
   → [TLA -Análisis Semántico](TLA%20-Análisis%20Semántico.md)
6. **Máquinas de Turing + jerarquía de Chomsky** (Ej 7, relacionar conceptos): Autómata Linealmente Acotado (AAL), Recursivamente Enumerable, Sensible al Contexto, Decidible vs Tratable.
   → [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md), [TLA -Máquina de Turing (Parte 2)](TLA%20-Máquina%20de%20Turing%20%28Parte%202%29.md)

### 🟢 El único práctico (Ej 5)

Diseñar/corregir un **autómata de pila** o **completar una MT**. Practicá 2-3 y listo. En AP el patrón es "este autómata NO reconoce L, ¿por qué? ¿qué transiciones están mal?".

---

## 3. Qué estudiar de cada tema (con notas del vault)

**Introducción / Gramáticas** — Jerarquía de Chomsky (4 tipos), forma sentencial (def. recursiva), gramática ambigua, tipos de gramática.
→ [TLA -intro resumen](TLA%20-intro%20resumen.md)

**Autómatas Finitos** — `δ̂` recursiva (AFD y AFND-λ), clausura-λ, estados distinguibles/indistinguibles, algoritmo de minimización (conjunto cociente), equivalencias AFD↔AFND↔AFND-λ.
→ [TLA -Autómatas Finitos Determinísticos](TLA%20-Autómatas%20Finitos%20Determinísticos.md), [TLA -Autómatas Finitos No Determinísticos](TLA%20-Autómatas%20Finitos%20No%20Determinísticos.md)

**Lenguajes y Expresiones Regulares** — identidades de ER (clausura de ∅ y λ, `α+∅`, Lema de Arden `X=αX+β ⇒ X=α*β`), propiedades de cierre (intersección, unión), lema de bombeo regular.
→ [TLA -Lenguajes Regulares](TLA%20-Lenguajes%20Regulares.md), [TLA -Expresiones Regulares](TLA%20-Expresiones%20Regulares.md)

**GLC y Autómatas de Pila** — FNC y FNG, lema de bombeo para LLC, AP determinista vs no determinista, vaciado de pila ↔ estado final, lenguajes no ambiguos no reconocibles por AP determinista.
→ [TLA -Autómatas de Pila](TLA%20-Autómatas%20de%20Pila.md), [TLA -Formas Normales y Lema de Bombeo CFL](TLA%20-Formas%20Normales%20y%20Lema%20de%20Bombeo%20CFL.md)

**Análisis Sintáctico** — Descendente: LL(1) (no ambigua, sin recursividad izquierda, factorizada), Primeros/Siguientes, backtracking. Ascendente: items LR(0)/LR(1), tablas SLR(1)/LR(1), acción desplazar/reducir, conflicto desplazamiento-reducción.
→ [TLA -Análisis Sintáctico](TLA%20-Análisis%20Sintáctico.md), [TLA -Análisis Ascendente](TLA%20-Análisis%20Ascendente.md)

**Máquinas de Turing** — definición formal, configuraciones, RE vs recursivo/decidible, AAL, sensible al contexto, decidible vs tratable.
→ [TLA -Máquina de Turing](TLA%20-Máquina%20de%20Turing.md), [TLA -Máquina de Turing (Parte 2)](TLA%20-Máquina%20de%20Turing%20%28Parte%202%29.md)

**Gramáticas con atributos** — sintetizado vs heredado, DDS, S-atribuida, esquema de traducción.
→ [TLA -Análisis Semántico](TLA%20-Análisis%20Semántico.md)

Guía extra ya armada: [TLA -Guía Parcial 2 (paso a paso)](TLA%20-Guía%20Parcial%202%20%28paso%20a%20paso%29.md)

---

## 4. Ejercicios de TP recomendados (mínimos, alto impacto)

Si el tiempo es corto, hacé **1-2 de cada** priorizando los que caen como práctico (Ej 5 = AP y MT):

| Tema | TP | Ejercicios sugeridos | Por qué |
|------|----|--------------------|---------|
| AFD/AFND (minimización) | Tp03 | 3, 6, 10 | Construir AFD/AFND y transformar; base del Ej 2 |
| ¿Regular o no? (bombeo) | Tp03 | 13 | Decidir regularidad → argumento del lema |
| Expr. Regulares | Tp04 | 3, 4, 6 | Identidades y equivalencias (caen en Ej 4) |
| GLC + FNG + AP | Tp06 | 7, 8, 9 | **Diseñar/corregir AP** = práctico Ej 5 |
| Análisis descendente | Tp07 | 2, 3 | LL(1): transformar y armar tabla |
| Análisis ascendente | Tp08 | 1, 4, 5 | LR(0)/SLR(1)/LR(1) y tablas (multiple choice Ej 3) |
| Máquinas de Turing | Tp09 | 1, 2, 4 | **Completar/diseñar MT** = práctico Ej 5 |
| Gramáticas con atributos | Tp10 | 3, 4, 5 | Sintetizados/heredados (Ej 6/7) |

> [!tip] Ruta mínima si tenés muy poco tiempo
> 1) Memorizá las **definiciones recursivas** (δ̂, clausura-λ, forma sentencial). 2) Repasá **las 3 demostraciones** del Ej 2. 3) Fijate bien **las condiciones exactas** de los dos lemas de bombeo. 4) Hacé **Tp06 ej 7-9** (AP) y **Tp09 ej 1-2** (MT) para el práctico. 5) Repasá **sintetizado vs heredado** y la **jerarquía de Chomsky**. Con eso cubrís cómodo los 5 puntos.

---

## 5. Cheat-sheet de lo que se repite textual

**Función de transición extendida `δ̂` (AFD):** Base `δ̂(p,λ)=p`; Paso: para `w=xa`, `δ̂(p,xa)=δ(δ̂(p,x),a)`.

**Clausura-λ (AFND-λ):** conjunto de estados alcanzables desde `q` por arcos etiquetados con `λ`. Base: `q ∈ clausura(q)`; Paso: si `p∈clausura(q)` y `δ(p,λ)⊇{r}` ⇒ `r∈clausura(q)`.

**Forma sentencial:** Base: `S` es FS; Paso: si `αBβ` es FS y `B→γ` ⇒ `αγβ` es FS.

**Lema de bombeo regular (cómo se demuestra):** con un **AFD de N estados** (N = constante) y una cadena `ω∈L` con `|ω|≥N` (por palomar se repite un estado).

**Lema de bombeo LLC:** existe P tal que si `α∈L` y `|α|≥P`, `α=rxyzs` con `|xyz|≤P ∧ |xz|≥1 ∧ ∀i≥0: rxⁱyzⁱs ∈ L`. *(Ojo: es `|xz|≥1` y `∀i≥0`, no `>0`.)*

**Lema de Arden:** `X = αX + β` (con λ∉α) ⇒ `X = α*β`.

**Jerarquía de Chomsky:** Tipo 0 = Recursivamente Enumerable (MT) · Tipo 1 = Sensible al Contexto (AAL) · Tipo 2 = Libre de Contexto (AP) · Tipo 3 = Regular (AF).

**FNC:** producciones `A→BC` o `A→t` (sin λ). **FNG:** `A→tβ` con `t∈Σ`, `β∈V*`.

**LL(1):** no ambigua + sin recursividad a izquierda + factorizada a izquierda.

**Item LR(1):** `[A→α₁·α₂, t]` con `α₁,α₂∈(V∪Σ)*` y `t∈Σ∪{$}`.

**Atributo sintetizado:** se calcula de **hijos → padre** en el árbol. **Heredado:** al revés (del padre/hermanos hacia el hijo).

---

*Fuentes: Final 1 dic 2022, Final 24 julio 2023, Final 17 dic 2024, compilado "TLA - Finales", TPs 01-10 (ITBA 72.39).*

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [TLA -Mega Resumen Final](TLA%20-Mega%20Resumen%20Final.md) — resumen completo
- [TLA -Máquina de Turing (Parte 2)](TLA%20-Máquina%20de%20Turing%20%28Parte%202%29.md) — decidibilidad
- [TLA -Análisis Sintáctico](TLA%20-Análisis%20Sintáctico.md) — análisis sintáctico
- [ejercicios-parcial-ii](ejercicios-parcial-ii.md) — ejercitación

<!-- notas-relacionadas:fin -->
