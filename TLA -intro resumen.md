---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-03-04
Materia: "[[TLA.base|TLA]]"
temas:
  - Autómatas
  - Alfabetos y cadenas
  - Lenguajes
  - Gramáticas
  - Jerarquía de Chomsky
---
x# TLA - Introducción (Resumen)

Resumen de la clase 1 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## ¿Qué es la teoría de autómatas?

La teoría de autómatas es el estudio de **máquinas abstractas**. En los años 30, Alan Turing estudió estas máquinas para describir los límites entre lo que una máquina de cálculo podía y no podía hacer.

- Permite modelar el funcionamiento de una computadora ideal.
- Separa problemas **computables** de los **insolubles** (NP-difíciles).
- Se complementa con las **gramáticas formales** (Noam Chomsky) para modelar lenguajes.
- Los autómatas y gramáticas formales se usan en el diseño y construcción de software.
- La teoría de problemas intratables permite deducir si un problema se puede resolver eficientemente o si se requiere una aproximación/heurística.

---

## Conceptos fundamentales

### Alfabeto

Un **alfabeto** $(\Sigma)$ es un conjunto finito no vacío de símbolos.

- $\Sigma = \{0, 1\}$ — alfabeto binario
- $\Sigma = \{a, b, c, \dots, z\}$ — alfabeto de letras minúsculas

### Cadena

Una **cadena** (o palabra) es una secuencia finita de símbolos seleccionados de algún alfabeto.

### Longitud de una cadena

La **longitud** de una cadena es la cantidad de posiciones que ocupan sus símbolos.

> *Ejemplo:* $00001010$ es una palabra de longitud 8.

### Cadena vacía

La cadena vacía tiene longitud 0 y se denota $\lambda$ (o $\epsilon$ en alguna bibliografía).

### Potencias de un alfabeto

$$\Sigma^k = \{\omega \text{ con símbolos en } \Sigma \ / \ |\omega| = k\}$$

> [!example] *Ejemplo:* Si $\Sigma = \{0, 1\}$:
> - $\Sigma^0 = \{\lambda\}$
> - $\Sigma^1 = \{0, 1\}$
> - $\Sigma^2 = \{00, 01, 10, 11\}$

### Clausuras de un alfabeto

- **Clausura de Kleene:** $\Sigma^* = \Sigma^0 \cup \Sigma^1 \cup \dots \cup \Sigma^n$
- **Clausura positiva:** $\Sigma^+ = \Sigma^1 \cup \Sigma^2 \cup \dots \cup \Sigma^n$ (excluye $\lambda$)

### Concatenación de cadenas

Si $x = a_1 a_2 \dots a_i$ y $y = b_1 b_2 \dots b_j$, entonces:

$$xy = a_1 a_2 \dots a_i b_1 b_2 \dots b_j$$

**Propiedades:**

- **Cerrada:** $\forall x, y \in \Sigma^* : x \cdot y \in \Sigma^*$
- **Asociativa:** $\forall x, y, z \in \Sigma^* : x \cdot (y \cdot z) = (x \cdot y) \cdot z$
- **Elemento neutro:** $\forall x \in \Sigma^* : \lambda \cdot x = x \cdot \lambda = x$

### Prefijos y sufijos

Si $z = xy$, entonces $x$ es **prefijo** de $z$ e $y$ es **sufijo** (posfijo) de $z$. Son *propios* si son distintos de $\lambda$.

### Reverso de una palabra

Si $w = a_1 a_2 \dots a_n$, entonces la palabra inversa o **refleja** es:

$$w^R = a_n a_{n-1} \dots a_1$$

---

## Lenguajes sobre un alfabeto

Un **lenguaje** sobre $\Sigma$ es cualquier subconjunto de $\Sigma^*$:

$$L \subseteq \Sigma^*$$

### Operaciones con lenguajes

Operaciones de conjuntos estándar: **unión**, **intersección**, **diferencia**.

Operaciones adicionales:

- **Producto (concatenación):**
$$L_1 \cdot L_2 = \{w \ / \ w = xy, \ x \in L_1 \land y \in L_2\}$$

- **Potencia:**
$$L^i = L \cdot L \cdots L \quad (i \text{ veces})$$

- **Clausura positiva:**
$$L^+ = \bigcup_{i=1}^{\infty} L^i$$

- **Clausura de Kleene:**
$$L^* = \bigcup_{i=0}^{\infty} L^i$$

---

## Definiciones recursivas

Las definiciones recursivas tienen un **caso base** (estructuras elementales) y un **paso de inducción** (estructuras complejas a partir de las anteriores).

### Definición alternativa de cadena

- **Base:** $\lambda$ es una cadena.
- **Paso inductivo:** si $t \in \Sigma$ y $\omega \in \Sigma^*$ ya es cadena, entonces $t\omega$ es cadena.

### Definición alternativa de potencia de un alfabeto

- **Base:** si $i = 0$, entonces $\Sigma^i = \{\lambda\}$
- **Paso inductivo:** si $i > 0$, entonces $\Sigma^i = \{a \cdot \alpha \ / \ a \in \Sigma \land \alpha \in \Sigma^{i-1}\}$

### Definición alternativa de reverso

- **Base:** el reverso de $\lambda$ es $\lambda$.
- **Paso inductivo:** si $t \in \Sigma$ y $\omega \in \Sigma^*$, entonces $(t\omega)^r = (\omega)^r t$.

---

## Inducción estructural

Cuando una estructura fue definida recursivamente, se pueden probar teoremas sobre ella mediante **inducción estructural**:

1. **Base:** se prueba $S(X)$ para la(s) estructura(s) base $X$.
2. **Paso inductivo:** se toma una estructura $X$ formada a partir de $Y_1, Y_2, \dots, Y_k$; se dan por ciertas $S(Y_1), S(Y_2), \dots, S(Y_k)$ y se usan para probar $S(X)$.

> [!example]- Ejemplo: nodos de un árbol = arcos + 1
> **Definición recursiva de árbol:**
> - *Base:* un único nodo es un árbol.
> - *Paso inductivo:* si $T_1, T_2, \dots, T_k$ son árboles, se construye un nuevo árbol con un nodo raíz $N$ conectado a las raíces de cada $T_i$.
>
> **Prueba:** Si $T$ tiene $n$ nodos y $e$ arcos, entonces $n = e + 1$.
> - *Base:* un nodo, 0 arcos → $1 = 0 + 1$ ✓
> - *Inductivo:* $n = n_1 + n_2 + \dots + n_k + 1$ y $e = e_1 + e_2 + \dots + e_k + k$. Como $n_i = e_i + 1$, se tiene $n = (e_1+1) + (e_2+1) + \dots + (e_k+1) + 1 = e + 1$ ✓

---

## Gramáticas

Una **gramática** es un sistema matemático para definir un lenguaje. Es una 4-tupla:

$$G = (N, \Sigma, P, S)$$

| Componente | Descripción |
|---|---|
| $N$ | Conjunto finito de símbolos **no terminales** (variables) |
| $\Sigma$ | Conjunto finito de símbolos **terminales**, con $N \cap \Sigma = \emptyset$ |
| $P$ | Conjunto de **producciones** de la forma $\alpha \to \beta$ |
| $S$ | **Símbolo inicial** ($S \in N$) |

### Forma sentencial

- **Base:** $S$ es una forma sentencial.
- **Paso inductivo:** si $\alpha\beta\gamma$ es forma sentencial y $\beta \to \delta \in P$, entonces $\alpha\delta\gamma$ también lo es.

Una forma sentencial que sólo contiene terminales (o es $\lambda$) se llama **sentencia**.

### Derivación

Si $\alpha\beta\gamma$ es cadena en $(N \cup \Sigma)^*$ y $\beta \to \delta$ es producción, entonces:

$$\alpha\beta\gamma \Rightarrow_G \alpha\delta\gamma$$

### Lenguaje generado

$$L(G) = \{\omega \in \Sigma^* \ / \ S \overset{*}{\Rightarrow} \omega\}$$

---

## Jerarquía de Chomsky

Las gramáticas se clasifican según el formato de sus producciones. Si un lenguaje es generado por una gramática de tipo X, el lenguaje es de tipo X.

| Tipo | Nombre | Restricción de producciones | Lenguajes | Máquina |
|---|---|---|---|---|
| **0** | Sin restricciones | $\alpha \to \beta$ con $\alpha \in (N \cup \Sigma)^+$, $\beta \in (N \cup \Sigma)^*$ | Recursivamente enumerables ($L_0$) | Máquina de Turing |
| **1** | Sensibles al contexto | $\alpha \to \beta$ con $|\alpha| \leq |\beta|$ (excepto $S \to \lambda$) | Sensibles al contexto ($L_1$) | Autómatas linealmente acotados |
| **2** | Libres de contexto | $A \to \alpha$ (un no terminal a la izquierda) | Libres de contexto ($L_2$) | Autómatas con pila |
| **3** | Regulares | $A \to bC$, $A \to b$, $A \to \lambda$ (lineales por derecha o izquierda) | Regulares ($L_3$) | Autómatas finitos |

> [!info] Inclusión de clases
> $L_3 \subset L_2 \subset L_1 \subset L_0$
>
> También existen **lenguajes no enumerables** que no son generados por ninguna gramática.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[Alfabeto]] — alfabetos y cadenas
- [[TLA -Lenguajes Regulares]] — tema siguiente
- [[TLA -Mega Resumen Final]] — resumen integrador

<!-- notas-relacionadas:fin -->
