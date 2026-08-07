---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-03-18"
Materia: "[[TLA.base|TLA]]"
temas:
  - Autómatas Finitos
  - AFD
  - Función de transición
  - Minimización de AFD
  - Estados equivalentes
---
# TLA - Autómatas Finitos Determinísticos (Resumen)

Resumen de la clase 2 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Definición general de Autómata Finito

Un **autómata finito** (AF) es una 5-tupla:

$$AF = (Q, \Sigma, \delta, q_0, F)$$

| Componente | Descripción |
|---|---|
| $Q$ | Conjunto finito de **estados** |
| $\Sigma$ | **Alfabeto** de entrada |
| $q_0 \in Q$ | **Estado inicial** |
| $F \subseteq Q$ | Conjunto de **estados finales** (o de aceptación) |
| $\delta$ | **Función de transición**: $\delta: Q \times (\Sigma \cup \{\lambda\}) \to \mathcal{P}(Q)$ |

---

## Función de transición: representación

La función de transición se puede representar de dos formas:

### Tabla de transición

Filas = estados, columnas = símbolos. Cada celda indica el estado destino. Los estados finales se marcan con `*`.

### Diagrama de transición (grafo)

- Cada **nodo** (círculo) representa un estado.
- Cada **nodo doble** representa un estado final.
- Una **flecha** apuntando a un nodo indica el estado inicial.
- Cada **arco dirigido** representa una transición; la etiqueta indica el símbolo consumido.

> [!example] Ejemplo
> $M = (\{q_0, q_1, q_2, q_3, t\}, \{0,1\}, \delta, q_0, \{q_3\})$
>
> | $\delta$ | 0 | 1 |
> |---|---|---|
> | $q_0$ | $q_1$ | $t$ |
> | $q_1$ | $q_2$ | $q_3$ |
> | $q_2$ | $t$ | $q_3$ |
> | $*q_3$ | $t$ | $t$ |
> | $t$ | $t$ | $t$ |
>
> ```mermaid
> stateDiagram-v2
>     direction LR
>     [*] --> q0
>     q0 --> q1 : 0
>     q0 --> t : 1
>     q1 --> q2 : 0
>     q1 --> q3 : 1
>     q2 --> t : 0
>     q2 --> q3 : 1
>     q3 --> t : 0
>     q3 --> t : 1
>     t --> t : 0
>     t --> t : 1
>     q3:::final
>     classDef final stroke-width:3px
> ```

---

## Autómatas Finitos Determinísticos (AFD)

### Función de transición

En un AFD, la función de transición se define como:

$$\delta: Q \times \Sigma \to Q$$

A cada par (estado, símbolo) le corresponde **uno y sólo un** estado.

### Función de transición extendida $\hat{\delta}$

La función de transición extendida describe lo que ocurre cuando se parte de cualquier estado y se sigue una **secuencia** de entradas. Se construye por inducción a partir de $\delta$.

$$\hat{\delta}: Q \times \Sigma^* \to Q$$

**Definición inductiva** sobre la longitud de la cadena de entrada:

- **Base:** $\hat{\delta}(q, \lambda) = q$
- **Paso inductivo:** Si $\omega = \omega'a$, entonces $\hat{\delta}(q, \omega) = \delta(\hat{\delta}(q, \omega'), a)$

> [!example] Ejemplo
> Con $M = (\{q_0, q_1, q_2, q_3, t\}, \{0,1\}, \delta, q_0, \{q_3\})$ del ejemplo anterior:
>
> $$\hat{\delta}(q_0, \text{"001"}) = \delta(\hat{\delta}(q_0, \text{"00"}), 1)$$
> $$\hat{\delta}(q_0, \text{"00"}) = \delta(\hat{\delta}(q_0, \text{"0"}), 0)$$
> $$\hat{\delta}(q_0, \text{"0"}) = \delta(\hat{\delta}(q_0, \lambda), 0) = \delta(q_0, 0) = q_1$$
>
> Resolviendo:
> - $\hat{\delta}(q_0, \text{"0"}) = q_1$
> - $\hat{\delta}(q_0, \text{"00"}) = \delta(q_1, 0) = q_2$
> - $\hat{\delta}(q_0, \text{"001"}) = \delta(q_2, 1) = q_3$

---

## Configuración instantánea

La **configuración instantánea** es una descripción del autómata finito en un momento dado. Se denota con un par $[q, \omega]$, donde $q \in Q$ y $\omega \in \Sigma^*$.

### Secuencia de configuraciones

El símbolo $\mapsto$ indica el movimiento válido entre dos configuraciones:

$$[q, a\omega] \mapsto [p, \omega] \iff \delta(q, a) = p$$

**Definición inductiva:**

- **Base:** Si $I$ es una configuración instantánea, $I \mapsto^* I$ es una secuencia válida.
- **Paso inductivo:** $I \mapsto^* J$ es válida si existe alguna configuración $K$ tal que $I \mapsto K$ y $K \mapsto^* J$.

> [!example] Ejemplo
> Con el autómata $M$ anterior y la cadena "001":
>
> $$[q_0, \text{"001"}] \mapsto [q_1, \text{"01"}] \mapsto [q_2, \text{"1"}] \mapsto [q_3, \lambda]$$
>
> Es decir: $[q_0, \text{"001"}] \mapsto^* [q_3, \lambda]$

---

## Lenguaje aceptado por un AFD

El lenguaje de un AFD se define de dos formas equivalentes:

**Mediante función de transición extendida:**

$$L = \{\omega \in \Sigma^* \mid \hat{\delta}(q_0, \omega) \in F\}$$

**Mediante secuencia de configuraciones:**

$$L = \{\omega \in \Sigma^* \mid [q_0, \omega] \mapsto^* [p, \lambda],\ p \in F\}$$

> [!example] Ejemplo
> Sea $A = (\{I, A, F\}, \{a, b\}, \delta, I, \{F\})$ con:
>
> | $\delta$ | a | b |
> |---|---|---|
> | $I$ | $A$ | $F$ |
> | $A$ | $A$ | $I$ |
> | $*F$ | $A$ | $F$ |
>
> ¿La palabra "ababb" pertenece al lenguaje?
>
> $[I, ababb] \mapsto [A, babb] \mapsto [I, abb] \mapsto [A, bb] \mapsto [I, b] \mapsto [F, \lambda]$
>
> Como $F \in F$, la palabra **pertenece** al lenguaje.

---

## Determinismo

Dados una palabra $\omega \in \Sigma^*$ y un AFD, sólo hay **una** secuencia de configuraciones:

$$[q_0, \omega] \mapsto \dots \mapsto [q_{final}, \lambda]$$

Se demuestra por inducción en la longitud de $\omega$ y contradicción.

---

## Equivalencia de autómatas

Dos AFD $M$ y $M'$ son **equivalentes** si y sólo si:

$$L(M) = L(M')$$

Es decir, reconocen el **mismo lenguaje**. Si una palabra es aceptada por uno y rechazada por el otro, no son equivalentes.

> [!info] Propiedad clave
> La minimización de dos autómatas equivalentes produce el **mismo** AFD mínimo (salvo renombramiento de estados).

---

## AFD Mínimo

### Estados accesibles

Un estado $q_i \in Q$ es **accesible** si:

$$\exists \alpha \in \Sigma^* \mid \hat{\delta}(q_0, \alpha) = q_i$$

Equivalentemente: $\exists \alpha \in \Sigma^* \mid [q_0, \alpha] \mapsto^* [q_i, \lambda]$

**Construcción inductiva del conjunto de estados accesibles:**

- **Base:** $Q' = \{q_0\}$ es accesible (ya que $\hat{\delta}(q_0, \lambda) = q_0$).
- **Paso inductivo:** Si $S \subseteq Q$ es un conjunto de estados accesibles, para cada $a \in \Sigma$ y $q_i \in S$, el conjunto $S' = \{q_j \in Q \mid \delta(q_i, a) = q_j\}$ es de estados accesibles.

---

### Estados equivalentes o indistinguibles

Los estados $p$ y $q$ son **indistinguibles** si para toda cadena de entrada $\omega$, $\hat{\delta}(p, \omega)$ es de aceptación si y sólo si $\hat{\delta}(q, \omega)$ también lo es:

$$\forall \omega \in \Sigma^*: \hat{\delta}(p, \omega) \in F \iff \hat{\delta}(q, \omega) \in F$$

Los estados $p$ y $q$ son **distinguibles** si existe al menos una cadena $\omega$ tal que uno llega a un estado final y el otro no:

$$\exists \omega \in \Sigma^* \mid \hat{\delta}(p, \omega) \in F \land \hat{\delta}(q, \omega) \notin F$$

**Algoritmo para detectar estados distinguibles:**

- **Base:** Si $p \in F$ y $q \notin F$ (o viceversa), el par $\{p, q\}$ es distinguible.
- **Paso inductivo:** Si para algún $a \in \Sigma$, $\delta(p,a) = r$ y $\delta(q,a) = s$, y $\{r, s\}$ son distinguibles, entonces $\{p, q\}$ son distinguibles.

---

### La indistinguibilidad es una relación de equivalencia

**Reflexiva:** $p$ es indistinguible de $p$.

$$\forall \omega \in \Sigma^*: \hat{\delta}(p, \omega) \in F \iff \hat{\delta}(p, \omega) \in F$$

**Simétrica:** Si $p$ es indistinguible de $q$, entonces $q$ es indistinguible de $p$.

**Transitiva:** Si $\{p, q\}$ y $\{q, r\}$ son indistinguibles, entonces $\{p, r\}$ también lo son.

*Demostración (transitiva):* Se supone que $\{p, r\}$ son distinguibles. Entonces $\exists \omega$ tal que $\hat{\delta}(p, \omega) \in F$ y $\hat{\delta}(r, \omega) \notin F$. Analizando $\hat{\delta}(q, \omega)$: si es de aceptación, $\{q, r\}$ serían distinguibles; si no es de aceptación, $\{p, q\}$ serían distinguibles. Ambos casos contradicen la hipótesis.

---

### Indistinguibilidad de orden $k$

La **indistinguibilidad de orden $k$** ($E_k$) considera palabras de longitud $k$. Es una relación de equivalencia y determina una **partición** del conjunto de estados en clases de equivalencia.

**Lema 1:** Dado un autómata, se cumple que:

$$\frac{Q}{E_0} = \{F, Q - F\}$$

*Demostración:* $pE_0q$ significa que $\forall \omega \in \Sigma^* \land |\omega| = 0$: $\hat{\delta}(p, \lambda) \in F \iff \hat{\delta}(q, \lambda) \in F$. Como $\hat{\delta}(p, \lambda) = p$ y $\hat{\delta}(q, \lambda) = q$, entonces $pE_0q \iff p \in F \iff q \in F$.

**Lema 2:** Dado un autómata y dos estados $p, q \in Q$:

$$pE_{n+1}q \iff \forall a \in \Sigma: \delta(p, a) E_n \delta(q, a)$$

*Demostración:* Si $\forall a \in \Sigma: \delta(p,a) E_n \delta(q,a)$, siendo $\delta(p,a) = r$ y $\delta(q,a) = s$, entonces $\forall \omega \in \Sigma^* \land |\omega| = n: \hat{\delta}(r, \omega) \in F \iff \hat{\delta}(s, \omega) \in F$. Por lo tanto, para $\omega' = a\omega$ con $|\omega'| = n+1$: $\hat{\delta}(p, \omega') \in F \iff \hat{\delta}(q, \omega') \in F$, es decir, $pE_{n+1}q$.

---

### Algoritmo de construcción del conjunto cociente

**Entrada:** $Q$
**Salida:** Conjunto cociente $\frac{Q}{E}$

1. $\frac{Q}{E_0} = \{F, Q - F\}$
2. Generar $\frac{Q}{E_{i+1}}$ a partir de $\frac{Q}{E_i}$: los estados $p$ y $q$ pertenecen a la misma clase en $\frac{Q}{E_{i+1}}$ si y sólo si:
   - $p$ y $q$ pertenecen a la misma clase en $\frac{Q}{E_i}$, **y**
   - $\forall a \in \Sigma$, $\delta(p,a)$ y $\delta(q,a)$ pertenecen a la misma clase en $\frac{Q}{E_i}$
3. Si $\frac{Q}{E_{i+1}} = \frac{Q}{E_i}$, entonces $\frac{Q}{E} = \frac{Q}{E_i}$. Sino, volver al paso 2.

> [!example] Ejemplo
> Sea $A = (\{p, r, q, s, t\}, \{0,1\}, \delta, p, \{s, t\})$
>
> | $\delta$ | 0 | 1 |
> |---|---|---|
> | $p$ | $r$ | $q$ |
> | $r$ | $q$ | $*s$ |
> | $q$ | $q$ | $*t$ |
> | $*s$ | $*s$ | $*s$ |
> | $*t$ | $*t$ | $*t$ |
>
> **Paso 0:** $\frac{Q}{E_0} = \{\{s, t\}, \{p, r, q\}\}$
>
> Analizando las transiciones de los estados no finales:
> - $\delta(p, 0) = r \notin F$, $\delta(p, 1) = q \notin F$
> - $\delta(r, 0) = q \notin F$, $\delta(r, 1) = s \in F$
> - $\delta(q, 0) = q \notin F$, $\delta(q, 1) = t \in F$
>
> Como $r$ y $q$ van a un estado final con 1, pero $p$ no, se separa $p$:
>
> **Paso 1:** $\frac{Q}{E_1} = \{\{s, t\}, \{p\}, \{r, q\}\}$
>
> **Paso 2:** $\frac{Q}{E_2} = \{\{s, t\}, \{p\}, \{r, q\}\}$
>
> Como $\frac{Q}{E_2} = \frac{Q}{E_1}$, el conjunto cociente es $\frac{Q}{E} = \{\{s, t\}, \{p\}, \{r, q\}\}$.

---

### Algoritmo de construcción de AFD mínimo

**Entrada:** $A = (Q, \Sigma, \delta, q_0, F)$
**Salida:** $A' = (Q', \Sigma, \delta', q_0', F')$ equivalente a $A$ con mínima cantidad de estados.

1. **Eliminar estados inaccesibles** desde $q_0$.
2. **Construir el conjunto cociente** $\frac{Q}{E}$.
3. Definir $A' = (Q', \Sigma, \delta', q_0', F')$ donde:
   - $Q' = \frac{Q}{E}$
   - $q_0'$ es la clase de equivalencia que contiene a $q_0$
   - $F' = \{s \in \frac{Q}{E} \mid s \cap F \neq \emptyset\}$
   - $\delta'(s_i, a) = s_j \iff \exists p \in s_i \land \exists q \in s_j \mid \delta(p, a) = q$

> [!example] AFD mínimo del ejemplo anterior
> Con $S = \{s,t\}$, $P = \{p\}$, $Q = \{r,q\}$:
>
> - $Q' = \{S, P, Q\}$
> - $q_0' = P$
> - $F' = \{S\}$
>
> | $\delta'$ | 0 | 1 |
> |---|---|---|
> | $P$ | $Q$ | $Q$ |
> | $Q$ | $Q$ | $*S$ |
> | $*S$ | $*S$ | $*S$ |
>
> ```mermaid
> stateDiagram-v2
>     direction LR
>     [*] --> P
>     P --> Q : 0
>     P --> Q : 1
>     Q --> Q : 0
>     Q --> S : 1
>     S --> S : 0
>     S --> S : 1
>     S:::final
>     classDef final stroke-width:3px
> ```

---

### Teorema: unicidad del AFD mínimo

> [!info] Teorema
> El autómata $A'$ obtenido con el algoritmo anterior es **equivalente** al autómata $A$ y es **mínimo** (el número de estados de $A'$ es menor o igual que el de cualquier otro AFD equivalente a $A$).

**Demostración (esquema):**

1. **Equivalencia:** Los estados de $A'$ corresponden a las clases de equivalencia de $A$. Cualquier transición $\delta(p, x) = q$ se corresponde con $\delta'(P, x) = Q$ (donde $p \in P$ y $q \in Q$). Por lo tanto, para cualquier $\omega \in \Sigma^*$, si $A$ consume $\omega$ en un estado $p$, $A'$ consume $\omega$ en la clase que contiene a $p$, y ambos estados son finales o ambos no finales.

2. **Minimalidad (por absurdo):** Suponer que existe $A'' = (Q'', \Sigma, \delta'', q_0'', F'')$ equivalente con menos estados. Entonces existen $\alpha, \beta$ tales que $\hat{\delta}(q_0', \alpha) = p$ y $\hat{\delta}(q_0', \beta) = q$ en $A'$, pero $\hat{\delta}''(q_0'', \alpha) = \hat{\delta}''(q_0'', \beta) = r$ en $A''$. Como $A'$ fue obtenido por el algoritmo, $p$ y $q$ son distinguibles, pero en $A''$ llevan al mismo estado, lo cual es una contradicción.

> [!info] Unicidad
> El AFD mínimo es **único**, salvo renombramiento de estados.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -Autómatas Finitos No Determinísticos]] — tema siguiente
- [[TLA -Expresiones Regulares]] — equivalencia ER-AF
- [[TLA -Lenguajes Regulares]] — qué reconocen

**Otras materias**

- **Arqui**  [[Integrados Compuertas y decodificadores]] — circuito secuencial = AFD en hardware
- **Protos**  [[5. Protos - Transporte]] — TCP se especifica como máquina de estados

<!-- notas-relacionadas:fin -->
