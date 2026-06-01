---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-03-18"
Materia: "[[TLA.base|TLA]]"
temas:
  - AFND
  - AFND-lambda
  - Construcción de subconjuntos
  - Clausura lambda
  - Equivalencia AFD-AFND
---
# TLA - Autómatas Finitos No Determinísticos (Resumen)

Resumen de la clase 3 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Autómatas Finitos No Determinísticos (AFND)

### Definición

Un AFND tiene la misma estructura de 5-tupla que un AFD, pero la función de transición cambia:

$$\delta: Q \times \Sigma \to \mathcal{P}(Q)$$

donde $\mathcal{P}(Q)$ denota el conjunto de todos los subconjuntos de $Q$. A cada par (estado, símbolo) le puede corresponder **un conjunto de estados** (incluyendo el conjunto vacío).

> [!example] Ejemplo
> $M = (\{q_0, q_1, q_2\}, \{0,1\}, \delta, q_0, \{q_2\})$
>
> | $\delta$ | 0 | 1 |
> |---|---|---|
> | $q_0$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
> | $q_1$ | $\emptyset$ | $\{q_2\}$ |
> | $*q_2$ | $\emptyset$ | $\emptyset$ |
>
> ```mermaid
> stateDiagram-v2
>     direction LR
>     [*] --> q0
>     q0 --> q0 : 0, 1
>     q0 --> q1 : 0
>     q1 --> q2 : 1
>     q2:::final
>     classDef final stroke-width:3px
> ```

---

### Función de transición extendida para AFND

La función $\hat{\delta}$ se redefine para los AFND:

- **Base:** $\hat{\delta}(q, \lambda) = \{q\}$
- **Paso inductivo:** Si $\omega = \omega'a$, $\hat{\delta}(q, \omega') = \{p_1, p_2, \dots, p_k\}$, y $\bigcup_{i=1}^{k} \delta(p_i, a) = \{r_1, r_2, \dots, r_m\}$, entonces:

$$\hat{\delta}(q, \omega) = \hat{\delta}(q, \omega'a) = \{r_1, r_2, \dots, r_m\}$$

> [!example] Ejemplo
> Con $M = (\{q_0, q_1, q_2\}, \{0,1\}, \delta, q_0, \{q_2\})$ del ejemplo anterior:
>
> $\hat{\delta}(q_0, 00101) = \{q_0, q_2\}$

---

### Secuencia de configuraciones para AFND

Se redefine:

$$[q, a\omega] \mapsto [p, \omega] \iff p \in \delta(q, a)$$

Notar que ahora puede haber **múltiples** secuencias de configuraciones posibles para una misma entrada.

---

### Lenguaje aceptado por un AFND

**Mediante función de transición extendida:**

$$L = \{\omega \in \Sigma^* \mid \hat{\delta}(q_0, \omega) \cap F \neq \emptyset\}$$

**Mediante secuencia de configuraciones:**

$$L = \{\omega \in \Sigma^* \mid [q_0, \omega] \mapsto^* [p, \lambda],\ p \in F\}$$

> [!info] Criterio de aceptación
> Basta con que **al menos una** de las posibles secuencias de configuraciones llegue a un estado final para que la palabra sea aceptada.

> [!example] Ejemplo
> Con $M$ del ejemplo anterior:
>
> $\hat{\delta}(q_0, 00101) = \{q_0, q_2\}$. Como $\{q_0, q_2\} \cap F = \{q_2\} \neq \emptyset$, entonces $00101 \in L(M)$.

---

## Equivalencia entre AFD y AFND

> [!info] Teorema
> Todo lenguaje que puede ser reconocido por un AFND puede ser reconocido por un AFD y viceversa.

### Construcción de subconjuntos

Sea $N = (Q_N, \Sigma, \delta_N, q_0, F_N)$ un AFND. Se construye el AFD equivalente $D = (Q_D, \Sigma, \delta_D, \{q_0\}, F_D)$ donde:

- $Q_D = \mathcal{P}(Q_N)$
- $F_D = \{S \in Q_D \mid S \cap F_N \neq \emptyset\}$
- $\forall a \in \Sigma,\ \forall S \subseteq Q_N:\ \delta_D(S, a) = \bigcup_{p \in S} \delta_N(p, a)$

> [!example] Ejemplo completo
> Con $M = (\{q_0, q_1, q_2\}, \{0,1\}, \delta, q_0, \{q_2\})$:
>
> La tabla completa de $\delta_D$ es:
>
> | $\delta_D$ | 0 | 1 |
> |---|---|---|
> | $\emptyset$ | $\emptyset$ | $\emptyset$ |
> | $\{q_0\}$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
> | $\{q_1\}$ | $\emptyset$ | $\{q_2\}$ |
> | $*\{q_2\}$ | $\emptyset$ | $\emptyset$ |
> | $\{q_0, q_1\}$ | $\{q_0, q_1\}$ | $\{q_0, q_2\}$ |
> | $*\{q_0, q_2\}$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
> | $*\{q_1, q_2\}$ | $\emptyset$ | $\{q_2\}$ |
> | $*\{q_0, q_1, q_2\}$ | $\{q_0, q_1\}$ | $\{q_0, q_2\}$ |

---

### Construcción lazy (perezosa)

Muchos estados de $\mathcal{P}(Q_N)$ son inaccesibles. La **construcción lazy** genera sólo los estados alcanzables:

**Algoritmo:**
- **Entrada:** $\mathcal{P}(Q_N)$ y $Q_D = \emptyset$
- **Salida:** $Q_D$ con sólo estados accesibles.

1. Agregar $\{q_0\}$ a $Q_D$.
2. $\forall a \in \Sigma,\ \forall S \in Q_D$: si $\delta_D(S, a) \notin Q_D$, agregar $\delta_D(S, a)$ a $Q_D$.

> [!example] Resultado con construcción lazy
> Eliminando inaccesibles del ejemplo anterior:
>
> $D = (\{\{q_0\}, \{q_0, q_1\}, \{q_0, q_2\}\}, \{0,1\}, \delta_D, \{q_0\}, \{\{q_0, q_2\}\})$
>
> | $\delta_D$ | 0 | 1 |
> |---|---|---|
> | $\{q_0\}$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
> | $\{q_0, q_1\}$ | $\{q_0, q_1\}$ | $\{q_0, q_2\}$ |
> | $*\{q_0, q_2\}$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
>
> ```mermaid
> stateDiagram-v2
>     direction LR
>     [*] --> A
>     A --> B : 0
>     A --> A : 1
>     B --> B : 0
>     B --> C : 1
>     C --> B : 0
>     C --> A : 1
>     state "{ q₀ }" as A
>     state "{ q₀, q₁ }" as B
>     state "{ q₀, q₂ }" as C
>     C:::final
>     classDef final stroke-width:3px
> ```

---

### Teorema $L(D) = L(N)$

Si $D = (Q_D, \Sigma, \delta_D, \{q_0\}, F_D)$ es el AFD construido a partir del AFND $N = (Q_N, \Sigma, \delta_N, q_0, F_N)$ mediante la construcción de subconjuntos, entonces $L(D) = L(N)$.

**Demostración (esquema):**

Primero se demuestra por inducción en $|\omega|$ que:

$$\forall \omega \in \Sigma^*: \hat{\delta}_D(\{q_0\}, \omega) = \hat{\delta}_N(q_0, \omega)$$

- **Base** ($|\omega| = 0$): $\hat{\delta}_D(\{q_0\}, \lambda) = \{q_0\}$ y $\hat{\delta}_N(q_0, \lambda) = \{q_0\}$.
- **Paso inductivo** ($|\omega| = n+1$, $\omega = \alpha t$): Por hipótesis inductiva $\hat{\delta}_D(\{q_0\}, \alpha) = \hat{\delta}_N(q_0, \alpha) = \{p_1, \dots, p_k\}$. Por definición de $\hat{\delta}_N$: $\hat{\delta}_N(q_0, \omega) = \bigcup_{i=1}^{k} \delta_N(p_i, t)$. Por la construcción de subconjuntos: $\delta_D(\{p_1, \dots, p_k\}, t) = \bigcup_{i=1}^{k} \delta_N(p_i, t)$.

Luego se demuestra $\omega \in L(N) \iff \omega \in L(D)$:

$$\omega \in L(N) \iff \hat{\delta}_N(q_0, \omega) \cap F_N \neq \emptyset \iff \hat{\delta}_D(\{q_0\}, \omega) \cap F_N \neq \emptyset \iff \hat{\delta}_D(\{q_0\}, \omega) \in F_D \iff \omega \in L(D)$$

---

## Autómatas Finitos No Determinísticos con transiciones $\lambda$ (AFND-$\lambda$)

### Definición

En un AFND-$\lambda$ la función de transición se define como:

$$\delta: Q \times (\Sigma \cup \{\lambda\}) \to \mathcal{P}(Q)$$

Se permiten transiciones en las que **no se consume ningún símbolo** de la entrada.

> [!example] Ejemplo
> $M = (\{p, q, r, s\}, \{a, b\}, \delta, p, \{p, s\})$
>
> | $\delta$ | $a$ | $b$ | $\lambda$ |
> |---|---|---|---|
> | $*p$ | $\{q\}$ | $\emptyset$ | $\emptyset$ |
> | $q$ | $\{q, r, s\}$ | $\{p, r\}$ | $\{s\}$ |
> | $r$ | $\emptyset$ | $\{p, s\}$ | $\{r, s\}$ |
> | $*s$ | $\emptyset$ | $\emptyset$ | $\{r\}$ |

---

### Clausura $\lambda$ de un estado

La **clausura $\lambda$** de un estado $q$ es el conjunto de todos los estados alcanzables desde $q$ siguiendo caminos cuyos arcos estén etiquetados con $\lambda$.

**Definición recursiva:**

- **Base:** $q \in claus_\lambda(q)$
- **Paso inductivo:** Si $p \in claus_\lambda(q)$ y $r \in \delta(p, \lambda)$, entonces $r \in claus_\lambda(q)$

> [!example] Clausuras del ejemplo anterior
> - $claus_\lambda(p) = \{p\}$
> - $claus_\lambda(q) = \{q, s, r\}$ (de $q$ por $\lambda$ a $s$; de $s$ por $\lambda$ a $r$)
> - $claus_\lambda(r) = \{r, s\}$ (de $r$ por $\lambda$ a $s$; $s$ por $\lambda$ a $r$ ya visitado)
> - $claus_\lambda(s) = \{s, r\}$ (de $s$ por $\lambda$ a $r$; $r$ por $\lambda$ a $s$ ya visitado)

---

### Función de transición extendida para AFND-$\lambda$

- **Base:** $\hat{\delta}(q, \lambda) = claus_\lambda(q)$
- **Paso inductivo:** Si $\omega = \omega'a$, $\hat{\delta}(q, \omega') = \{p_1, p_2, \dots, p_k\}$, y $\bigcup_{i=1}^{k} \delta(p_i, a) = \{r_1, r_2, \dots, r_m\}$, entonces:

$$\hat{\delta}(q, \omega) = \bigcup_{j=1}^{m} claus_\lambda(r_j)$$

> [!info] Diferencia clave con AFND
> En el AFND-$\lambda$, después de cada transición con un símbolo, se toma la **clausura $\lambda$** de los estados resultantes. Esto captura todas las transiciones $\lambda$ que puedan seguir.

> [!example] Ejemplo
> Con $M = (\{p, q, r, s\}, \{a, b\}, \delta, p, \{p, s\})$ del ejemplo anterior:
>
> $\hat{\delta}(p, \text{"aab"}) = \{p, r, s\}$

---

### Lenguaje aceptado por un AFND-$\lambda$

**Mediante función de transición extendida:**

$$L = \{\omega \in \Sigma^* \mid \hat{\delta}(q_0, \omega) \cap F \neq \emptyset\}$$

**Mediante secuencia de configuraciones:**

$$L = \{\omega \in \Sigma^* \mid [q_0, \omega] \mapsto^* [p, \lambda],\ p \in F\}$$

> [!example] Ejemplo
> $\hat{\delta}(p, \text{"aab"}) = \{p, r, s\}$. Como $\{p, r, s\} \cap F = \{p, s\} \neq \emptyset$, entonces $aab \in L(M)$.

---

## Equivalencia entre AFD y AFND-$\lambda$

> [!info] Teorema
> L es aceptado por algún AFND-$\lambda$ **si y sólo si** L es aceptado por algún AFD.

### Eliminación de transiciones $\lambda$: construcción del AFD equivalente

Sea $E = (Q_E, \Sigma, \delta_E, q_0, F_E)$ un AFND-$\lambda$. El AFD equivalente $D = (Q_D, \Sigma, \delta_D, q_D, F_D)$ se define así:

- $Q_D$ es el conjunto de subconjuntos de $Q_E$, $S \subseteq Q_E$, tales que $S = claus_\lambda(S)$
- $q_D = claus_\lambda(q_0)$
- $F_D = \{S \in Q_D \mid S \cap F_E \neq \emptyset\}$
- Se calcula $\delta_D(S, a)$ para todo $a \in \Sigma$ y todos los $S \in Q_D$:
  1. Sea $S = \{p_1, p_2, \dots, p_k\}$
  2. Calcular $\bigcup_{i=1}^{k} \delta_E(p_i, a) = \{r_1, r_2, \dots, r_m\}$
  3. Luego $\delta_D(S, a) = \bigcup_{j=1}^{m} claus_\lambda(r_j)$

> [!example] Ejemplo completo
> Con $M = (\{p, q, r, s\}, \{a, b\}, \delta, p, \{p, s\})$:
>
> **Estado inicial:** $q_D = claus_\lambda(p) = \{p\}$
>
> **Cálculo iterativo (construcción lazy):**
>
> 1. $\delta_D(\{p\}, a) = claus_\lambda(q) = \{q, r, s\}$, $\delta_D(\{p\}, b) = claus_\lambda(\emptyset) = \emptyset$
> 2. $\delta_D(\{q,r,s\}, a) = claus_\lambda(\{q,r,s\}) = \{q,r,s\}$, $\delta_D(\{q,r,s\}, b) = claus_\lambda(\{p,r,s\}) = \{p,r,s\}$
> 3. $\delta_D(\emptyset, a) = \emptyset$, $\delta_D(\emptyset, b) = \emptyset$
> 4. $\delta_D(\{p,r,s\}, a) = claus_\lambda(q) = \{q,r,s\}$, $\delta_D(\{p,r,s\}, b) = claus_\lambda(\{p,s\}) = \{p,r,s\}$
>
> **Tabla del AFD resultante:**
>
> | $\delta_D$ | $a$ | $b$ |
> |---|---|---|
> | $\to \{p\}$ | $\{q,r,s\}$ | $\emptyset$ |
> | $\{q,r,s\}$ | $\{q,r,s\}$ | $\{p,r,s\}$ |
> | $\emptyset$ | $\emptyset$ | $\emptyset$ |
> | $*\{p,r,s\}$ | $\{q,r,s\}$ | $\{p,r,s\}$ |
>
> Con $Q_D = \{\{p\}, \{q,r,s\}, \emptyset, \{p,r,s\}\}$ y $F_D = \{\{p\}, \{q,r,s\}, \{p,r,s\}\}$
>
> > Nota: $\{p\}$ también es final porque $p \in F_E$.
>
> ```mermaid
> stateDiagram-v2
>     direction LR
>     [*] --> P
>     P --> QRS : a
>     P --> V : b
>     QRS --> QRS : a
>     QRS --> PRS : b
>     V --> V : a
>     V --> V : b
>     PRS --> QRS : a
>     PRS --> PRS : b
>     state "{ p }" as P
>     state "{ q, r, s }" as QRS
>     state "∅" as V
>     state "{ p, r, s }" as PRS
>     P:::final
>     QRS:::final
>     PRS:::final
>     classDef final stroke-width:3px
> ```

---

### Demostración del teorema de equivalencia

**1a parte:** Si $L$ es aceptado por algún AFND-$\lambda$ $E$, entonces es aceptado por un AFD $D$ (construido por eliminación de transiciones $\lambda$).

Se demuestra por inducción en $|\omega|$ que $\hat{\delta}_E(q_0, \omega) = \hat{\delta}_D(claus_\lambda(q_0), \omega)$:

- **Base** ($\omega = \lambda$): $\hat{\delta}_E(q_0, \lambda) = claus_\lambda(q_0) = q_D = \hat{\delta}_D(q_D, \lambda)$
- **Paso inductivo** ($\omega = \alpha t$): Por hipótesis inductiva $\hat{\delta}_E(q_0, \alpha) = \hat{\delta}_D(q_D, \alpha) = \{p_1, \dots, p_k\}$. Luego $\hat{\delta}_E(q_0, \omega) = \bigcup_{j=1}^{m} claus_\lambda(r_j)$ donde $\{r_1, \dots, r_m\} = \bigcup_{i=1}^{k} \delta_E(p_i, t)$. Por la construcción, $\delta_D(\{p_1, \dots, p_k\}, t) = \bigcup_{j=1}^{m} claus_\lambda(r_j)$.

**2a parte:** Si $L$ es aceptado por algún AFD $D$, entonces es aceptado por un AFND-$\lambda$ $E$.

Se transforma $D$ en un AFND-$\lambda$ añadiendo:
- $\forall q \in Q_D: \delta_E(q, \lambda) = \emptyset$
- $\forall q \in Q_D,\ \forall a \in \Sigma: \delta_D(q, a) = p \Rightarrow \delta_E(\{q\}, a) = \{p\}$

Como las transiciones no cambian, toda palabra aceptada en el AFD es aceptada en el AFND-$\lambda$.
