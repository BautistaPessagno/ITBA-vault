---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-04-24"
Materia: "[[TLA.base|TLA]]"
temas:
  - Autómatas de Pila
  - Configuraciones
  - Aceptación por estado final
  - Aceptación por pila vacía
  - Autómata de Pila Determinístico
  - Jerarquía de lenguajes
  - Gramáticas Libres de Contexto
  - Derivaciones a izquierda y a derecha
  - Árboles de derivación
  - Gramáticas ambiguas
  - Lenguajes inherentemente ambiguos
---
# TLA - Autómatas de Pila y Gramáticas Libres de Contexto (Resumen)

Resumen de la clase 6 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación: limitaciones de los Autómatas Finitos

Los **Autómatas Finitos** sólo disponen de una cantidad finita de estados como memoria, por lo que no pueden reconocer lenguajes que requieran contar símbolos arbitrariamente (por ejemplo $L = \{a^n b^n \mid n \geq 0\}$ o los palíndromos).

Para capturar esta clase más amplia de lenguajes se introduce una **memoria auxiliar con estructura de pila**, dando lugar a los **Autómatas de Pila (AP)**.

---

## Definición formal de Autómata de Pila

> [!info] Definición
> Un **Autómata de Pila** es una 7-tupla:
>
> $$AP = \langle Q, \Sigma, \Gamma, \delta, q_0, z_0, F \rangle$$
>
> donde:
> - $Q$: conjunto finito de estados.
> - $\Sigma$: alfabeto de entrada.
> - $\Gamma$: alfabeto de pila.
> - $q_0 \in Q$: estado inicial.
> - $z_0 \in \Gamma$: símbolo inicial de la pila (un único símbolo).
> - $F \subseteq Q$: conjunto de estados finales.
> - $\delta$: función de transición
>
> $$\delta: Q \times (\Sigma \cup \{\lambda\}) \times \Gamma \to \mathcal{P}(Q \times \Gamma^*)$$

La transición $\delta(q, a, Z) = \{(r_1, \beta_1), (r_2, \beta_2), \dots\}$ indica que estando en el estado $q$, leyendo el símbolo $a$ (o $\lambda$) de la entrada y con $Z$ en el tope de la pila, el autómata puede pasar a cualquiera de los estados $r_i$ y reemplazar $Z$ por $\beta_i \in \Gamma^*$ (reemplazar, agregar o quitar símbolos del tope).

> [!warning] Importante
> La transición **sólo consulta el tope de la pila**. Puede, en un mismo paso, quitar símbolos, dejar igual el tope o apilar uno o varios símbolos.

---

### Ejemplo concreto

> [!example] Ejemplo
> Sea $A = \langle \{q_0, q_1, q_2, q_3, q_f\}, \{a, b, c\}, \{z_0, A, D\}, \delta, q_0, z_0, \{q_f\} \rangle$ con la siguiente función de transición:
>
> | Estado | Entrada | Tope | Destinos (estado, nuevo tope) |
> |---|---|---|---|
> | $q_0$ | $a$ | $z_0$ | $\{(q_1, A z_0)\}$ |
> | $q_1$ | $a$ | $A$ | $\{(q_1, AA)\}$ |
> | $q_1$ | $b$ | $A$ | $\{(q_2, D)\}$ |
> | $q_2$ | $b$ | $D$ | $\{(q_2, D)\}$ |
> | $q_2$ | $c$ | $D$ | $\{(q_3, \lambda)\}$ |
> | $q_3$ | $\lambda$ | $z_0$ | $\{(q_f, z_0)\}$ |
>
> ```mermaid
> stateDiagram-v2
>     [*] --> q0
>     q0 --> q1 : a, z0 / A z0
>     q1 --> q1 : a, A / A A
>     q1 --> q2 : b, A / D
>     q2 --> q2 : b, D / D
>     q2 --> q3 : c, D / λ
>     q3 --> qf : λ, z0 / z0
>     qf --> [*]
> ```

---

## Configuraciones

### Configuración instantánea

Una **configuración instantánea** describe el estado completo del autómata en un instante dado:

$$[q, \omega, \alpha]$$

- $q \in Q$: estado actual.
- $\omega \in \Sigma^*$: cadena de entrada que resta por leer.
- $\alpha \in \Gamma^*$: contenido actual de la pila (con el tope a la izquierda).

Si $\omega = \lambda$, no queda entrada por analizar.

### Configuración inicial

$$[q_0, \omega, z_0]$$

donde $\omega$ es la cadena de entrada completa y $z_0$ el símbolo inicial de la pila.

### Secuencia de configuraciones

Se definen dos tipos de **movimiento** entre configuraciones:

- **Movimiento por lectura** de símbolo $a \in \Sigma$:
  $$[q, a\omega, Z\alpha] \vdash [r, \omega, \beta\alpha] \quad \text{si } (r, \beta) \in \delta(q, a, Z)$$

- **Movimiento $\lambda$** (siempre aplicable, no consume entrada):
  $$[q, \omega, Z\alpha] \vdash [r, \omega, \beta\alpha] \quad \text{si } (r, \beta) \in \delta(q, \lambda, Z)$$

La clausura reflexivo-transitiva se denota $\vdash^*$ y representa una secuencia de cero o más movimientos.

---

## Lenguaje aceptado por un Autómata de Pila

Existen **dos criterios de aceptación** equivalentes en cuanto a los lenguajes que definen.

### Aceptación por estado final

$$L_{pf}(A) = \{\omega \in \Sigma^* \mid [q_0, \omega, z_0] \vdash^* [q_f, \lambda, \alpha], \; q_f \in F, \; \alpha \in \Gamma^*\}$$

La cadena debe consumirse completamente y el autómata debe quedar en un estado final (el contenido de la pila no importa).

### Aceptación por pila vacía

$$L_{pv}(A) = \{\omega \in \Sigma^* \mid [q_0, \omega, z_0] \vdash^* [p, \lambda, \lambda], \; p \in Q\}$$

La cadena debe consumirse completamente y la pila debe quedar vacía (el estado final no importa; de hecho, $F$ suele considerarse vacío).

---

## Equivalencia entre modos de aceptación

> [!info] Teorema
> Un lenguaje $L$ es aceptado por algún AP por estado final **si y sólo si** $L$ es aceptado por algún AP por pila vacía.

Los dos AP **no son necesariamente iguales**: dado uno, se construye el otro mediante las siguientes conversiones.

### Conversión: pila vacía → estado final

> [!info] Construcción
> Sea $P_v = \langle Q, \Sigma, \Gamma, \delta_v, q_0, z_0 \rangle$ un AP que acepta $L$ por pila vacía. Se construye:
>
> $$P_f = \langle Q \cup \{p_0, p_f\}, \Sigma, \Gamma \cup \{T\}, \delta_f, p_0, T, \{p_f\} \rangle$$
>
> donde $T \notin \Gamma$ es un nuevo marcador de fondo de pila y $\delta_f$ se define por:
>
> 1. $\delta_f(p_0, \lambda, T) = \{(q_0, z_0 T)\}$ — se apila el símbolo inicial de $P_v$ sobre $T$.
> 2. $\forall q \in Q, \, \forall a \in \Sigma \cup \{\lambda\}, \, \forall Y \in \Gamma: \; \delta_f(q, a, Y) = \delta_v(q, a, Y)$ — se copian las transiciones originales.
> 3. $\forall q \in Q: \; \delta_f(q, \lambda, T) = \{(p_f, \lambda)\}$ — si se llega a ver el marcador $T$ (porque $P_v$ vació su pila), se pasa al nuevo estado final.

**Idea:** el marcador $T$ queda siempre debajo de la pila original, de modo que sólo queda expuesto cuando $P_v$ habría vaciado la pila. En ese momento $P_f$ acepta pasando a $p_f$.

### Conversión: estado final → pila vacía

> [!info] Construcción
> Sea $P_f = \langle Q, \Sigma, \Gamma, \delta_f, q_0, z_0, F \rangle$ un AP que acepta $L$ por estado final. Se construye:
>
> $$P_v = \langle Q \cup \{p_0, p\}, \Sigma, \Gamma \cup \{T\}, \delta_v, p_0, T \rangle$$
>
> con $T \notin \Gamma$ y $\delta_v$:
>
> 1. $\delta_v(p_0, \lambda, T) = \{(q_0, z_0 T)\}$
> 2. $\forall q \in Q, \, \forall a \in \Sigma \cup \{\lambda\}, \, \forall Y \in \Gamma: \; \delta_v(q, a, Y) = \delta_f(q, a, Y)$
> 3. $\forall q_f \in F, \, \forall Y \in \Gamma \cup \{T\}: \; \delta_v(q_f, \lambda, Y) \supseteq \{(p, \lambda)\}$ — desde cualquier estado final se puede pasar al estado de vaciado.
> 4. $\forall Y \in \Gamma \cup \{T\}: \; \delta_v(p, \lambda, Y) = \{(p, \lambda)\}$ — en $p$ se vacía la pila por completo.

**Idea:** el marcador $T$ impide que $P_v$ "acepte por error" cuando $P_f$ vacía su pila sin haber llegado a un estado final. Sólo tras alcanzar un estado final se pasa a $p$, que vacía toda la pila (incluyendo $T$).

---

## Autómata de Pila Determinístico (APD)

> [!info] Definición
> Un AP $\langle Q, \Sigma, \Gamma, \delta, q_0, z_0, F \rangle$ es **determinístico (APD)** si cumple:
>
> 1. $\forall q \in Q, \, \forall a \in \Sigma \cup \{\lambda\}, \, \forall X \in \Gamma: \; |\delta(q, a, X)| \leq 1$.
> 2. Si $\delta(q, a, X) \neq \emptyset$ para algún $a \in \Sigma$, entonces $\delta(q, \lambda, X) = \emptyset$.

La segunda condición evita que coexistan una transición por lectura y una transición $\lambda$ desde la misma configuración, lo que generaría no-determinismo real.

> [!warning] Importante
> A diferencia de los Autómatas Finitos, **los APD son estrictamente menos poderosos que los AP no-determinísticos**: existen lenguajes libres de contexto que requieren no-determinismo.

---

## Lenguajes Regulares y APD

> [!info] Teorema
> Todo lenguaje regular puede ser aceptado por un APD.

**Construcción:** sea $A = \langle Q, \Sigma, \delta_A, q_0, F \rangle$ un AFD que reconoce $L$. Se define:

$$P = \langle Q, \Sigma, \{z_0\}, \delta_P, q_0, z_0, F \rangle$$

con transición

$$\delta_P(q, a, z_0) = \{(p, z_0)\} \iff \delta_A(q, a) = p$$

La pila se utiliza trivialmente: $z_0$ está siempre en el tope y nunca cambia, así que $P$ simula a $A$ ignorando la pila.

**Demostración (bosquejo):** por inducción sobre $|\omega|$ se prueba

$$[q_0, \omega, z_0] \vdash^* [p, \lambda, z_0] \iff \hat{\delta}_A(q_0, \omega) = p$$

de donde se sigue $L(P) = L(A)$.

---

## APD reconoce lenguajes no regulares

> [!example] Ejemplo
> $L = \{\omega c \omega^R \mid \omega \in \{a, b\}^*\}$ (palíndromos sobre $\{a,b\}$ con una marca central $c$) **no es regular** pero existe un APD que lo reconoce.
>
> **Idea:** mientras se lee $\omega$ antes de la $c$, se apilan los símbolos leídos. Al leer $c$, se cambia de fase. Luego, por cada símbolo leído se exige que coincida con el tope de la pila y se desapila. Se acepta si al terminar la entrada el estado es final (o la pila queda con sólo $z_0$).

---

## Jerarquía de lenguajes

$$L_R \subsetneq L_{APD} \subsetneq L_{AP}$$

- $L_R$: lenguajes **regulares**.
- $L_{APD}$: lenguajes aceptados por algún **APD** (lenguajes libres de contexto determinísticos).
- $L_{AP}$: lenguajes aceptados por algún **AP no-determinístico** — coinciden con los **lenguajes libres de contexto (GLC)**.

Las inclusiones son **estrictas**:

- $L_R \subsetneq L_{APD}$: $\{\omega c \omega^R\}$ es aceptado por APD pero no es regular.
- $L_{APD} \subsetneq L_{AP}$: existen lenguajes libres de contexto (por ejemplo $\{\omega \omega^R \mid \omega \in \{a,b\}^*\}$, sin marca central) que sólo pueden reconocerse con no-determinismo.

| Clase de lenguaje | Modelo que lo reconoce |
|---|---|
| Regulares | AF (AFD, AFND) |
| Libres de contexto determinísticos | APD |
| Libres de contexto | AP (no-determinístico) |

---

---

## Gramáticas Libres de Contexto (GLC)

### Definición formal

> [!info] Definición
> Una **Gramática** $G = \langle V, \Sigma, S, P \rangle$ es **libre de contexto** si todas sus producciones son de la forma:
>
> $$A \to \beta, \quad \beta \in (V \cup \Sigma)^*$$
>
> donde:
> - $V$: conjunto de variables (no terminales).
> - $\Sigma$: alfabeto de terminales.
> - $S \in V$: símbolo inicial.
> - $P$: conjunto finito de producciones.

La notación $A \Rightarrow \beta$ indica que $A$ se deriva en un paso a $\beta$. La clausura reflexivo-transitiva $\Rightarrow^*$ representa cero o más pasos. El **lenguaje generado** por $G$ es:

$$L(G) = \{\omega \in \Sigma^* \mid S \Rightarrow^* \omega\}$$

---

### Derivaciones a izquierda y a derecha

En cada paso de derivación se elige qué variable reemplazar. Dos estrategias canónicas:

- **Derivación a izquierda**: en cada paso se reemplaza la variable situada **más a la izquierda**.
- **Derivación a derecha**: en cada paso se reemplaza la variable situada **más a la derecha**.

> [!example] Ejemplo
> Sea $G = \langle \{S, A, B\}, \{a, b\}, S, P \rangle$ con:
>
> $$P = \{S \to AB \mid a,\quad A \to aBa \mid a,\quad B \to ABb \mid b\}$$
>
> **Derivación a izquierda** (resultado: $aabbab$):
> $$S \Rightarrow AB \Rightarrow aBaB \Rightarrow aABbaB \Rightarrow aaBbaB \Rightarrow aabbaB \Rightarrow aabbab$$
>
> **Derivación a derecha** (resultado: $aabb$):
> $$S \Rightarrow AB \Rightarrow AABb \Rightarrow AAbb \Rightarrow Aabb \Rightarrow aabb$$

---

### Árboles de derivación

> [!info] Definición
> Sea $G = \langle N, \Sigma, S, P \rangle$. Un **árbol de derivación** $\tau(A)$ es un grafo tal que:
>
> 1. Si $A \in N$, entonces $\tau(A)$ es un árbol de derivación cuya representación es un único nodo etiquetado $A$.
> 2. Si $\tau(A)$ es un árbol de derivación y $X$ es una hoja con $X \in N$ tal que $X \to Y_1 Y_2 \dots Y_k \in P$, entonces el grafo resultante de conectar $X$ con aristas a $k$ nuevos nodos $Y_1, Y_2, \dots, Y_k$ también es un árbol de derivación.

El **resultado** (o *yield*) de un árbol de derivación es la concatenación de las etiquetas de sus hojas de izquierda a derecha.

> [!note]
> Toda derivación (a izquierda, a derecha, u otra) corresponde a un árbol de derivación. El árbol abstrae el orden de los reemplazos: sólo importa qué producción se usó en cada nodo.

---

### Gramáticas ambiguas

> [!warning] Definición
> Una GLC $G$ es **ambigua** si existe alguna cadena $\omega \in L(G)$ que tiene **dos árboles de derivación distintos** para $S$.
>
> Equivalentemente: $G$ es ambigua sii existe $\omega \in L(G)$ con dos **derivaciones más a la izquierda distintas** desde $S$.

> [!example] Ejemplo — gramática aritmética ambigua
> Sea $G = \langle \{E\}, \{+, *, 0, 1\}, E, P \rangle$ con:
>
> $$P = \{E \to E + E \mid E * E \mid 0 \mid 1\}$$
>
> La cadena `0+1*0` admite dos árboles de derivación distintos:
>
> - **Árbol 1** (agrupa como `(0+1)*0`):
>   $E \Rightarrow E*E \Rightarrow E+E*E \Rightarrow 0+E*E \Rightarrow 0+1*E \Rightarrow 0+1*0$
>
> - **Árbol 2** (agrupa como `0+(1*0)`):
>   $E \Rightarrow E+E \Rightarrow 0+E \Rightarrow 0+E*E \Rightarrow 0+1*E \Rightarrow 0+1*0$
>
> Ambas son derivaciones más a la izquierda desde $S$, lo que confirma que $G$ es ambigua.

---

### Eliminación de la ambigüedad

La ambigüedad puede eliminarse **reformulando** la gramática para forzar precedencia y asociatividad, aunque **no existe un algoritmo general** que determine si una GLC arbitraria es ambigua.

La técnica estándar es **estratificar** las variables por nivel de prioridad:

| Gramática (con problema) | Gramática (corregida) |
|---|---|
| $E \to E + E \mid T$ | $E \to E + T \mid T$ |
| $T \to T * T \mid I$ | $T \to T * I \mid I$ |
| $I \to 0 \mid 1$ | $I \to 0 \mid 1$ |

La versión izquierda marca **precedencia** (`*` sobre `+`) pero no resuelve la **asociatividad**. La versión derecha, al usar $E \to E + T$ (recursividad a izquierda), fuerza asociatividad a izquierda para `+`, y análogamente $T \to T * I$ para `*`.

> [!info] Lenguaje inherentemente ambiguo
> Un lenguaje $L$ es **inherentemente ambiguo** (o intrínsecamente ambiguo) si **toda** gramática libre de contexto que lo genera es ambigua.

> [!note] Continuación — clase 7
> En la clase siguiente se prueba la equivalencia GLC $\leftrightarrow$ AP no-determinístico y se introducen las Formas Normales de Chomsky y Greibach, junto con el Lema de Bombeo para LLC. Ver: [[TLA -Formas Normales y Lema de Bombeo CFL]].

---

## Preguntas

-
