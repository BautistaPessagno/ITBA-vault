---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-04-29"
Materia: "[[TLA.base|TLA]]"
temas:
  - Forma Normal de Chomsky
  - Forma Normal de Greibach
  - Equivalencia GLC y Autómatas de Pila
  - Gramáticas ambiguas y APD
  - Lema de Bombeo para LLC
  - Propiedades de los Lenguajes Libres de Contexto
---
# TLA - Formas Normales y Lema de Bombeo para LLC (Resumen)

Resumen de la clase 7 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación

En la clase anterior se estableció que los Autómatas de Pila no-determinísticos reconocen exactamente los **Lenguajes Libres de Contexto (LLC)**. Esta clase completa ese cuadro desde el lado gramatical: se normalizan las GLC (Formas Normal de Chomsky y de Greibach), se demuestra formalmente la equivalencia GLC ↔ AP, se caracteriza la no-regularidad de los LLC mediante el Lema de Bombeo, y se estudian sus propiedades de cierre.

---

## Forma Normal de Chomsky (FNC)

> [!info] Definición
> Una GLC está en **Forma Normal de Chomsky** si todas sus producciones son de la forma:
>
> $$A \to t \qquad \text{o} \qquad A \to BC$$
>
> con $A, B, C \in N$ y $t \in \Sigma$.

> [!info] Propiedad
> Cualquier árbol de derivación para una gramática en FNC resulta ser un **árbol binario**.

Todo LLC sin $\lambda$ puede generarse con una GLC en FNC.

### Algoritmo de conversión a FNC

**Simplificaciones preliminares** (en orden):

1. **Eliminar símbolos inútiles** — un símbolo $X \in (N \cup \Sigma)^*$ es *útil* si existe derivación $S \Rightarrow^* \alpha X \beta \Rightarrow^* \omega$ con $\omega \in \Sigma^*$. Se eliminan los improductivos primero, luego los inalcanzables.
2. **Eliminar producciones-$\lambda$** — un símbolo $X$ es *anulable* si $X \Rightarrow^* \lambda$. Para cada producción que contenga un símbolo anulable, agregar la variante omitiendo dicho símbolo (en todas las combinaciones posibles). Finalmente eliminar las producciones $A \to \lambda$.
3. **Eliminar producciones unitarias** — son las de la forma $A \to B$ con $B \in N$. Identificar todos los *pares unitarios* $(A, B)$ tales que $A \Rightarrow^* B$ sólo mediante producciones unitarias; para cada par, si $B \to \alpha$ es no-unitaria, agregar $A \to \alpha$ y luego eliminar las unitarias originales.

**Pasos finales** (una vez hechas las simplificaciones):

1. **Variables únicamente en cuerpos de longitud $\geq 2$**: para cada terminal $a$ que aparezca en el cuerpo de una producción de longitud 2 o más, agregar una nueva variable $C_a$ con producción $C_a \to a$ y reemplazar todas las ocurrencias de $a$ en esos cuerpos por $C_a$.
2. **Descomponer cuerpos de longitud $\geq 3$**: introducir variables auxiliares $E_1, E_2, \dots$ en "cascada" de modo que cada producción quede con exactamente dos variables en el cuerpo.

### Ejemplo completo (FNC)

Sea $G = \langle \{S, A, B, C\}, \{a, b\}, P, S \rangle$ con:

$$P = \{S \to ASB \mid a,\quad A \to aAS \mid a \mid \lambda,\quad B \to SbS \mid A,\quad C \to bC \mid Ca\}$$

#### Paso 1 — Eliminar símbolos inútiles

$X \in (N \cup \Sigma)^*$ es útil para $G = \langle N, \Sigma, P, S \rangle$ si existe derivación $S \Rightarrow^* \alpha X \beta \Rightarrow^* \omega$ con $\omega \in \Sigma^*$.

- **Productivos**: $A$ es productivo ($A \to a$); $S$ es productivo ($S \to a$); como $A$ es productivo, $B$ es productivo ($B \to A \to a$). $C$ es improductivo — se elimina junto con sus producciones.
- **Alcanzables** (sobre $G_1 = \langle \{S,A,B\}, \{a,b\}, P_1, S \rangle$): $S$ es alcanzable; como $S$ es alcanzable, $A$ y $B$ también lo son.

$$P_1 = \{S \to ASB \mid a,\quad A \to aAS \mid a \mid \lambda,\quad B \to SbS \mid A\}$$

#### Paso 2 — Eliminar producciones-$\lambda$ (símbolos anulables)

- $A$ es anulable ($A \to \lambda$).
- Como $A$ es anulable, $B$ también lo es ($B \to A \Rightarrow^* \lambda$).

Para cada producción con anulables, agregar variantes omitiendo los anulables:

Como $A$ es anulable: $S \to SB \mid AS$ (variantes de $ASB$ sin A); $A \to aS$ (variante de $aAS$ sin $A$); se descarta $A \to \lambda$.

Como $B$ es anulable: $S \to AS$ (variante de $ASB$ sin $B$). Se agrega también $S \to S$ (variante de $ASB$ sin $A$ ni $B$, pero es unitaria y se tratará luego).

$$P_2 = \{S \to ASB \mid a \mid SB \mid AS \mid S,\quad A \to aAS \mid a \mid aS,\quad B \to SbS \mid A\}$$

#### Paso 3 — Eliminar producciones unitarias

Pares unitarios: $\{(S,S),\, (B,A)\}$.

- Se elimina $S \to S$.
- Como $(B, A)$ es par unitario, $B$ hereda las producciones no-unitarias de $A$: $B \to aAS \mid a \mid aS$.

$$P_3 = \{S \to ASB \mid a \mid SB \mid AS,\quad A \to aAS \mid a \mid aS,\quad B \to SbS \mid aAS \mid a \mid aS\}$$

#### Paso 4 — Variables únicamente en cuerpos de longitud $\geq 2$

Agregar $C \to a$ y $D \to b$ para reemplazar los terminales en cuerpos mixtos:

$$S \to ASB \mid a \mid SB \mid AS, \quad A \to CAS \mid a \mid CS, \quad B \to SDS \mid CAS \mid a \mid CS, \quad C \to a, \quad D \to b$$

#### Paso 5 — Descomponer cuerpos de longitud $\geq 3$

Agregar variables $E_1 \to SB$, $E_2 \to AS$, $E_3 \to DS$:

$$S \to AE_1 \mid a \mid SB \mid AS, \quad E_1 \to SB$$
$$A \to CE_2 \mid a \mid CS, \quad E_2 \to AS$$
$$B \to SE_3 \mid CE_2 \mid a \mid CS, \quad E_3 \to DS$$
$$C \to a, \quad D \to b$$

$$G_{fnc} = \langle \{a,b\},\; \{S,A,B,C,D,E_1,E_2,E_3\},\; S,\; P_{fnc} \rangle$$

---

## Forma Normal de Greibach (FNG)

> [!info] Definición
> Una GLC está en **Forma Normal de Greibach** si todas sus producciones son de la forma:
>
> $$A \to t \qquad \text{o} \qquad A \to t\,\alpha$$
>
> con $t \in \Sigma$ y $\alpha \in V^*$.

Para obtener la FNG se efectúan las siguientes simplificaciones:

1. Eliminar símbolos inútiles.
2. Eliminar producciones nulas (excepto $S \to \lambda$).
3. Eliminar producciones unitarias ($A \to B$).
4. Eliminar **recursividad a izquierda** ($A \to A\alpha$).
5. Llevar a la forma pedida.

> [!note] Nota
> La conversión a FNG se desarrolla en la práctica.

---

## Equivalencia GLC ↔ Autómatas de Pila

> [!info] Teorema
> Un lenguaje $L$ es libre de contexto **si y sólo si** es aceptado por algún Autómata de Pila.

Las dos direcciones de la equivalencia se construyen explícitamente.

---

### De GLC a AP

Dada una GLC $G = \langle N, \Sigma, P, S \rangle$, se construye el AP de un único estado:

$$P_v = \langle \{q\},\; \Sigma,\; N \cup \Sigma,\; \delta,\; q,\; S \rangle$$

que acepta por **pila vacía**, con función de transición definida por dos reglas:

- **Regla 1** (por producción): $\forall A \to \beta \in P:\quad \delta(q, \lambda, A) = \{(q, \beta)\}$
- **Regla 2** (por terminal): $\forall a \in \Sigma:\quad \delta(q, a, a) = \{(q, \lambda)\}$

La idea es simular una derivación más a la izquierda: el tope de la pila contiene la forma sentencial izquierda pendiente; si es una variable, se la reemplaza no-deterministamente; si es un terminal, se lo consume de la entrada.

> [!example] Ejemplo
> Sea $G = \langle \{S, A\}, \{a, b\}, S, P_g \rangle$ con $P_g = \{S \to aAa \mid bSb \mid \lambda,\; A \to aAa \mid b\}$.
>
> El AP $P_v = \langle \{q\}, \{a,b\}, \{S, A, a, b\}, \delta, q, S \rangle$ con transiciones:
>
> | Entrada | Tope | Destinos (estado, nuevo tope) |
> |---|---|---|
> | $\lambda$ | $S$ | $\{(q, aAa)\}$, $\{(q, bSb)\}$, $\{(q, \lambda)\}$ |
> | $\lambda$ | $A$ | $\{(q, aAa)\}$, $\{(q, b)\}$ |
> | $a$ | $a$ | $\{(q, \lambda)\}$ |
> | $b$ | $b$ | $\{(q, \lambda)\}$ |

---

### De AP a GLC

Dado un AP $M = \langle Q, \Sigma, \Gamma, \delta, q_0, z_0 \rangle$ (que acepta por pila vacía), se construye $G = \langle N, \Sigma, R, S \rangle$ donde:

$$N = \{S\} \cup \{[pXq] \mid p \in Q,\; X \in \Gamma,\; q \in Q\}$$

La variable $[pXq]$ representa "a partir del estado $p$ con $X$ en el tope de la pila, se consume entrada hasta llegar al estado $q$ con la pila vacía".

**Producciones de $R$**:

1. $\forall q \in Q:\quad S \to [q_0\, z_0\, q] \in R$

2. Si $(q_1, B_1 B_2 \dots B_m) \in \delta(q, a, A)$, agregar para toda combinación de estados intermedios $q_2, \dots, q_{m+1} \in Q$ con $q_{m+1} = p$:

$$[qAp] \to a\,[q_1 B_1 q_2]\,[q_2 B_2 q_3]\,\dots\,[q_m B_m q_{m+1}]$$

3. Si $(p, \lambda) \in \delta(q, a, A)$, agregar: $[qAp] \to a$

Luego se analizan las producciones y se descartan las **improductivas** e **inalcanzables**.

> [!example] Ejemplo
> Sea $M = \langle \{q_0, q_1\},\; \{0,1\},\; \{X, z_0\},\; \delta,\; q_0,\; z_0 \rangle$ con:
>
> | | | Transiciones |
> |---|---|---|
> | $\delta(q_0, 0, z_0)$ | $=$ | $\{(q_0, X z_0)\}$ |
> | $\delta(q_0, 0, X)$ | $=$ | $\{(q_0, X X)\}$ |
> | $\delta(q_0, 1, X)$ | $=$ | $\{(q_1, \lambda)\}$ |
> | $\delta(q_1, 1, X)$ | $=$ | $\{(q_1, \lambda)\}$ |
> | $\delta(q_1, \lambda, X)$ | $=$ | $\{(q_1, \lambda)\}$ |
> | $\delta(q_1, \lambda, z_0)$ | $=$ | $\{(q_1, \lambda)\}$ |
>
> Las posibles variables son $[q_0 z_0 q_0]$, $[q_1 z_0 q_0]$, $[q_0 z_0 q_1]$, $[q_1 z_0 q_1]$, $[q_0 X q_0]$, $[q_1 X q_0]$, $[q_0 X q_1]$, $[q_1 X q_1]$.
>
> Luego de generar las producciones y descartar las improductivas e inalcanzables, se renombran los símbolos definitivos:
>
> $$A = [q_0 z_0 q_1],\quad B = [q_0 X q_1],\quad C = [q_1 z_0 q_1],\quad D = [q_1 X q_1]$$
>
> Quedando $G_{final} = \langle \{S, A, B, C, D\},\; \{0,1\},\; R,\; S \rangle$ con:
>
> $$R = \{\,S \to A,\quad A \to 0BC,\quad B \to 0BD \mid 1,\quad C \to \lambda,\quad D \to 1 \mid \lambda\,\}$$
>
> Luego se puede convertir a alguna Forma Normal o realizarle simplificaciones.

---

## Gramáticas Ambiguas y APD

> [!warning] Importante
> - Todos los lenguajes que son aceptados por un APD tienen gramáticas **no ambiguas**.
> - Hay lenguajes generados por gramáticas no ambiguas que **no pueden ser reconocidos por ningún APD**.

> [!example] Ejemplo
> $G = \langle \{S\}, \{0,1\}, R, S \rangle$ con $R = \{S \to 0S0 \mid 1S1 \mid \lambda\}$.
>
> Es una gramática no ambigua, y $L(G) = \{\omega \omega^R \mid \omega \in \{0,1\}^*\}$ (palíndromos sin marca central), pero ya vimos que no es posible encontrar un APD que reconociera el mismo lenguaje.

---

## Lema de Bombeo para LLC

### Tamaño de los árboles de derivación en FNC

> [!info] Teorema
> Dada una gramática $G = \langle \Sigma, V, S, P \rangle$ en FNC y un árbol de derivación $\tau(S)$ cuyo resultado es la palabra $\omega$, si la longitud del camino más largo (altura) es $n$, entonces:
>
> $$|\omega| \leq 2^{n-1}$$

**Demostración** — por inducción en $n$:

- **Base** $n = 1$: el árbol consta únicamente de la raíz $S$ y una hoja etiquetada con un terminal $t$, por lo que $|\omega| = 1 = 2^{1-1} = 2^{n-1}$. ✓

- **Paso inductivo** — sea $n = h$, asumiendo que se cumple para $h-1$: siendo que $G$ está en FNC, la primera producción es de tipo $S \to BC$. Cualquier camino en los subárboles de $B$ y $C$ tiene longitud $\leq h-1$. Por hipótesis inductiva $|\omega|_B \leq 2^{h-2}$ y $|\omega|_C \leq 2^{h-2}$. Por lo tanto:
  $$|\omega| \leq |\omega|_B + |\omega|_C \leq 2 \cdot 2^{h-2} = 2^{h-1} = 2^{n-1} \quad \checkmark$$

---

### Enunciado del Lema

> [!info] Lema de Bombeo (LLC)
> Sea $L$ un Lenguaje Libre de Contexto. Entonces existe una constante $p$ tal que si $\alpha$ es una cadena de $L$ con $|\alpha| \geq p$, podemos escribir $\alpha = rxyzs$ satisfaciendo:
>
> 1. $|xyz| \leq p$ — la parte central no es demasiado larga.
> 2. $xz \neq \lambda$ — al menos una de las cadenas a bombear no es vacía.
> 3. $\forall i \geq 0:\quad rx^i y z^i s \in L$.
>
> Formalmente:
> $$\forall L \text{ LLC}:\; \exists p > 0 \left( \forall \alpha \in L:\; |\alpha| \geq p \Rightarrow \exists r,x,y,z,s \,/\; \alpha = rxyzs \;\land\; |xyz| \leq p \;\land\; (x \neq \lambda \lor z \neq \lambda) \;\land\; (\forall i \geq 0:\; rx^i y z^i s \in L) \right)$$

---

### Demostración del Lema

Se encuentra una gramática $G = \langle \Sigma, V, P, S \rangle$ en FNC tal que $L(G) = L - \{\lambda\}$.

> [!note] Observaciones
> - **Obs. 1**: si $L = \emptyset$, no se viola el enunciado (no existe $\alpha \in L$).
> - **Obs. 2**: la FNC permite encontrar gramática para $L - \{\lambda\}$; eso no importa ya que las palabras del lema deben tener longitud $|\alpha| \geq p > 0$.

Sea $|V| = m$, se elige $p = 2^m$. Por el teorema anterior, todo árbol de altura $n$ genera una palabra de longitud $\leq 2^{n-1}$; para generar $\alpha$ con $|\alpha| \geq 2^m$ se necesita altura $\geq m+1$, pues de lo contrario $|\alpha| \leq 2^{m-1} = p/2$.

En el camino más largo (de longitud $\geq m+1$) hay $m+1$ variables. Como sólo existen $m$ variables distintas, por el Principio del Palomar **alguna variable se repite**. Sean $A_i = A_j$ con $k-m \leq i < j \leq k$:

Se divide el árbol en tres sub-derivaciones. Llamando $A = A_i = A_j$:

1. $A_0 \Rightarrow^* rAs$
2. $A \Rightarrow^* xAz$
3. $A \Rightarrow^* y$

Como $G$ no tiene producciones unitarias (ni $\lambda$-producciones tras la simplificación), $xz \neq \lambda$.

**Demostración de que $\forall i \geq 0:\; rx^i y z^i s \in L$** — por inducción en $i$:

- **Base** $i = 0$: $A_0 \Rightarrow^* rAs \Rightarrow^* rys \in L$ (por (1) y (3)). ✓

- **Hipótesis inductiva** $i = h$: $rx^h y z^h s \in L$.

- **Paso** $i = h+1$:
  Por (1): $A_0 \Rightarrow^* rAs$.
  Por HI: $A \Rightarrow^* x^h A z^h$ (aplicando (2) $h$ veces), luego $A_0 \Rightarrow^* rx^h Az^h s$.
  Aplicando (2) una vez más: $A_0 \Rightarrow^* rx^{h+1}Az^{h+1}s$.
  Por (3): $A_0 \Rightarrow^* rx^{h+1}yz^{h+1}s \in L$. ✓

---

## Propiedades de los LLC

| Operación | ¿Cerrada? | Observación |
|---|---|---|
| Unión | Sí | Si $L_1$ LLC y $L_2$ LLC, entonces $L_1 \cup L_2$ LLC |
| Concatenación | Sí | Si $L_1$ LLC y $L_2$ LLC, entonces $L_1 L_2$ LLC |
| Clausura $L^+$ y $L^*$ | Sí | Si $L$ LLC, entonces $L^+$ y $L^*$ son LLC |
| Reverso | Sí | Si $L$ LLC, entonces $L^R$ es LLC |
| Intersección | **No** | Ver contraejemplo |
| Intersección con LR | Sí | Si $L_1$ LR y $L_2$ LLC, entonces $L_1 \cap L_2$ LLC |

### No cierre bajo intersección

> [!example] Contraejemplo
> Sean:
> $$L_1 = \{0^n 1^n 2^i \mid n \geq 1,\; i \geq 1\} \qquad L_2 = \{0^i 1^n 2^n \mid n \geq 1,\; i \geq 1\}$$
>
> Ambos son LLC. Sin embargo:
> $$L_1 \cap L_2 = \{0^n 1^n 2^n \mid n \geq 1\}$$
>
> que no es libre de contexto (se puede demostrar con el Lema de Bombeo para LLC).

---

## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -Autómatas de Pila]] — tema anterior
- [[TLA -Análisis Sintáctico]] — tema siguiente
- [[TLA -Lenguajes Regulares]] — lema de bombeo para regulares
- [[TLA -Guía Parcial 2 (paso a paso)]] — resolución paso a paso

<!-- notas-relacionadas:fin -->
