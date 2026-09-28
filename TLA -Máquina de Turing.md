---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-05-20"
Materia: "[[TLA.base|TLA]]"
temas:
  - Máquina de Turing
  - Definición formal (tupla)
  - Función de transición
  - Configuración instantánea
  - Secuencia de configuraciones
  - Lenguaje aceptado por una MT
  - Lenguajes recursivamente enumerables y recursivos (decidibles)
  - Extensiones de la MT
  - MT con varias cintas
  - Autómatas Acotados Linealmente (AAL)
  - Funciones recursivas parciales y totales
---
# TLA - Máquina de Turing (Resumen)

Resumen de la clase 10 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación

Las clases anteriores construyeron la teoría de autómatas y gramáticas para los lenguajes libres de contexto (Tipo 2): AP ↔ GLC, con Formas Normales y el Lema de Bombeo CFL. Esta clase sube en la jerarquía de Chomsky hacia los **lenguajes sensibles al contexto** (Tipo 1) y los **lenguajes recursivamente enumerables** (Tipo 0), introduciendo el modelo de cómputo más general: la **Máquina de Turing**.

---

## Máquinas de Turing

Es una máquina teórica que consta de:

- Una **unidad de control** que se encuentra en un estado cualquiera de un conjunto finito.
- Una **cinta** dividida en **casillas**, en cada una de las cuales está un símbolo.
- Al inicio, la **entrada** (cadena de símbolos de un alfabeto) está en la cinta y las restantes casillas tienen un símbolo llamado **espacio en blanco**.
- Una **cabeza de cinta** que apunta a una casilla.

En cada movimiento la máquina:
1. Cambia de estado.
2. Escribe un símbolo en la casilla apuntada.
3. Mueve la cabeza a izquierda o a derecha.

### Definición General

> [!info] Definición — Máquina de Turing
> Una MT es una tupla:
> $$MT = \langle Q,\Sigma,\Gamma,\delta,q_0,B,F \rangle$$
>
> - $Q$: conjunto finito de estados.
> - $\Sigma$: alfabeto de entrada ($\Sigma \subseteq \Gamma$).
> - $\Gamma$: alfabeto de cinta.
> - $\delta$: función de transición ($\delta: Q \times \Gamma \to Q \times \Gamma \times \{I,D\}$).
> - $q_0 \in Q$: estado inicial.
> - $B$: espacio en blanco ($B \in \Gamma,\; B \notin \Sigma$).
> - $F$: conjunto de estados finales o de aceptación ($F \subseteq Q$).

---

## Función de Transición

> [!info] Definición
> $$\delta: Q \times \Gamma \to Q \times \Gamma \times \{I,D\}$$
>
> Es decir: $\delta(q,X) = (p, Y, \text{Sentido})$
>
> El resultado, si está definido, tiene:
> - $p$: estado al que pasa la máquina.
> - $Y$: símbolo de $\Gamma$ que se escribe en la cinta.
> - $\text{Sentido}$: dirección en que la cabeza se mueve — izquierda (I) o derecha (D).

> [!warning] Importante
> La función **puede estar indefinida** para algunos argumentos. Cuando no está definida, la máquina se detiene.

> [!example] Ejemplo — MT que acepta $L = \{0^n1^n \mid n > 0\}$
> Estados: $q_0, q_1, q_2, q_3, q_4$ ($q_4$ es el estado de aceptación).
>
> Tabla de transiciones:
>
> | Estado | $0$ | $1$ | $X$ | $Y$ | $B$ |
> |---|---|---|---|---|---|
> | $q_0$ | $(q_1,X,D)$ | — | — | $(q_3,Y,D)$ | — |
> | $q_1$ | $(q_1,0,D)$ | $(q_2,Y,I)$ | — | $(q_1,Y,D)$ | — |
> | $q_2$ | $(q_2,0,I)$ | — | $(q_0,X,D)$ | $(q_2,Y,I)$ | — |
> | $q_3$ | — | — | — | $(q_3,Y,D)$ | $(q_4,B,D)$ |
> | $q_4$ | — | — | — | — | — |
>
> **La cadena 0011 es aceptada:**
> $$q_0 0011 \mapsto Xq_1 011 \mapsto X0q_1 11 \mapsto Xq_2 0Y1 \mapsto q_2 X0Y1 \mapsto Xq_0 0Y1$$
> $$\mapsto XXq_1 Y1 \mapsto XXYq_1 1 \mapsto XXq_2 YY \mapsto Xq_2 XYY \mapsto XXq_0 YY$$
> $$\mapsto XXYq_3 Y \mapsto XXYYq_3 B \mapsto XXYYBq_4 B \quad \checkmark$$
>
> **Pero no acepta 0010:**
> $$q_0 0010 \mapsto Xq_1 010 \mapsto X0q_1 10 \mapsto Xq_2 0Y0 \mapsto q_2 X0Y0$$
> $$\mapsto Xq_0 0Y0 \mapsto XXq_1 Y0 \mapsto XXYq_1 0 \mapsto XXY0q_1 B \quad \text{(bloqueada)}$$

---

## Configuración Instantánea

> [!info] Definición
> Una **configuración instantánea** es una descripción de la MT en un momento dado. Se denota $\alpha_1 q \alpha_2$, donde:
> - $q$: estado actual de la MT.
> - $\alpha_1 \in \Gamma^*$: contenido de la cinta a la **izquierda** del cursor (sin incluir la celda apuntada).
> - $\alpha_2 \in \Gamma^*$: contenido de la cinta a la **derecha** del cursor (incluyendo la celda apuntada).

---

## Secuencia de Configuraciones

El símbolo $\mapsto_M$ (o simplemente $\mapsto$) indica un movimiento válido entre dos configuraciones.

**Movimiento a izquierda** — si $\delta(q, X_i) = (p, Y, I)$:
$$X_1 X_2 \cdots X_{i-1}\, \mathbf{q}\, X_i X_{i+1} \cdots X_n \;\mapsto_M\; X_1 X_2 \cdots X_{i-2}\, \mathbf{p}\, X_{i-1} Y X_{i+1} \cdots X_n$$

La cabeza ahora apunta a la casilla $i-1$.

**Movimiento a derecha** — si $\delta(q, X_i) = (p, Y, D)$:
$$X_1 X_2 \cdots X_{i-1}\, \mathbf{q}\, X_i X_{i+1} \cdots X_n \;\mapsto_M\; X_1 X_2 \cdots X_{i-1} Y\, \mathbf{p}\, X_{i+1} \cdots X_n$$

La cabeza ahora apunta a la casilla $i+1$.

---

## Lenguaje Aceptado por una MT

> [!info] Definición
> El lenguaje aceptado por una MT $M$ es:
> $$L(M) = \{\omega \in \Sigma^* \mid q_0\omega \mapsto^* \alpha p\beta,\; p \in F\}$$

- Si $M$ se detiene en un estado final de aceptación $\Rightarrow \omega \in L(M)$.
- Si $M$ se detiene en un estado no final $\Rightarrow \omega \notin L(M)$.
- Si $M$ **no se detiene** $\Rightarrow \omega \notin L(M)$.

> [!warning] Importante
> Al conjunto de lenguajes aceptados por una MT se los denomina **lenguajes recursivamente enumerables**.
>
> **Aceptación por "parada"**: ocurre cuando $\delta(q, X)$ no está definida (no hay movimiento). Los lenguajes reconocidos por MT que **siempre se detienen** se llaman **recursivos** o **decidibles**.

Jerarquía:

$$\text{Decidable} \subsetneq \text{Recursivamente Enumerable} \subsetneq \text{Undecidable}$$

---

## Extensiones de las Máquinas de Turing

Las siguientes variantes son **equivalentes** en poder de reconocimiento a la MT estándar:

> [!warning] MT con cinta infinita en una dirección
> $L$ es reconocido por una MT con cinta infinita en **ambas** direcciones si y sólo si es reconocido por una MT con cinta infinita en **una sola** dirección.

> [!warning] MT no determinista
> Si $L$ es reconocido por una MT **no determinista**, entonces $L$ es reconocido por una MT **determinista**.

> [!warning] MT con varias cintas
> $L$ es reconocido por una MT con **varias cintas** si y sólo si es reconocido por una MT con **una sola cinta**.

### MT con Varias Cintas

> [!info] Definición
> Una MT de $k$ cintas define:
> $$\delta: Q \times \Gamma^k \to Q \times (\Gamma \times \{I,D,S\})^k$$
>
> Inicialmente:
> 1. La entrada se coloca en la primera cinta.
> 2. Todas las casillas de las demás cintas contienen espacios en blanco.
> 3. La unidad de control se encuentra en el estado inicial.
> 4. La cabeza de la primera cinta apunta al extremo izquierdo de la entrada.
> 5. Las cabezas de las restantes cintas apuntan a una casilla arbitraria.
>
> En cada movimiento la máquina: cambia de estado, escribe un símbolo en cada cinta (o el mismo), y mueve cada cabeza a derecha, izquierda o no se mueve (independientemente).

---

## Autómatas Acotados Linealmente (AAL)

> [!info] Definición
> Un **AAL** es una Máquina de Turing **no determinista** que usa una cinta de longitud finita que es función lineal de la longitud de la cadena de entrada.
>
> Es una Tupla $AAL = \langle Q, \Sigma, \Gamma, \delta, q_0, B, F \rangle$ donde $\Sigma$ contiene dos símbolos especiales: `#` y `$` (marcadores izquierdo y derecho), que evitan que la cabeza abandone la zona de la entrada.

> [!info] Lenguaje aceptado por un AAL
> Si $G$ es una gramática de **Tipo 1** (sensible al contexto), $G = \langle V, \Sigma, P, S \rangle$, entonces existe un AAL que acepta $L(G)$.

> [!example] Ejemplo — $L = \{wcw \mid w \in \{a,b\}^*\}$
> Se construye un AAL con estados $q_0, q_1, q_2, q_3, q_4, q_5, q_6, q_f$:
>
> - $q_0$: lee el primer símbolo no marcado a la izquierda de `c` (lo reemplaza por `X`).
> - $q_1/q_2$: busca el símbolo correspondiente a la derecha de `c`.
> - $q_3/q_4/q_5$: verifica coincidencia.
> - $q_6$: verifica que todos los símbolos estén marcados (acepta en $q_f$).

> [!example] Análisis con AAL — ¿son sensibles al contexto?
>
> | Lenguaje | ¿AAL? | Razón |
> |---|---|---|
> | $L = \{a^{n^2} \mid n \geq 1\}$ | **NO** | Requiere calcular $n^2$, necesita un "cálculo auxiliar" fuera de la cinta. |
> | $L = \{a^{2^n} \mid n \geq 0\}$ | **SÍ** | Se puede ir marcando un símbolo de la izquierda y borrando uno de la derecha, repitiendo hasta que quede un solo símbolo. |

---

## Funciones Recursivas Parciales

La MT puede verse como un **computador de funciones** $\mathbb{N} \to \mathbb{N}$.

- Los enteros se representan en **unario**: $i \geq 0$ se representa por $1^i$.
- Si una función tiene $k$ argumentos $i_1, i_2, \dots, i_k$, estos se colocan en la cinta separados por $0$ (u otro símbolo separador).
- Si la MT se detiene con una cinta que consiste de $1^M$, entonces $f(i_1, i_2, \dots, i_k) = M$.

> [!info] Función recursiva total vs parcial
> - Si $f(i_1, i_2, \dots, i_k)$ está definida para **toda** tupla, entonces $f$ es una **función recursiva total**.
> - Una función computada por una MT se llama **función recursiva parcial** (puede no estar definida para algunas entradas).
>
> Todas las funciones aritméticas comunes en enteros, tales como la multiplicación, $n!$, $2^n$, son funciones recursivas totales.

> [!example] Ejemplo — $f(x,y) = x + y$
> $f(2,4)$ en la cinta se representa inicialmente como $1101111$ (o $11 * 1111$ con otro separador).
>
> Cuando la MT se detenga, la cinta debe contener $111111$, ya que $f(2,4) = 6$.

---

## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [TLA -Análisis Semántico](TLA%20-Análisis%20Semántico.md) — tema anterior
- [TLA -Máquina de Turing (Parte 2)](TLA%20-Máquina%20de%20Turing%20%28Parte%202%29.md) — continuación
- [TLA -Autómatas de Pila](TLA%20-Autómatas%20de%20Pila.md) — jerarquía de Chomsky

**Otras materias**

- **EDA**  [EDA - Algoritmos y Complejidad](EDA%20-%20Algoritmos%20y%20Complejidad.md) — qué es computable y a qué costo

<!-- notas-relacionadas:fin -->
