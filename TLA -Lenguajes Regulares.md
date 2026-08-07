---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-04-01"
Materia: "[[TLA.base|TLA]]"
temas:
  - Lenguajes Regulares
  - Gramáticas Regulares
  - Lema de Bombeo
  - Propiedades de clausura
---

# TLA - Lenguajes Regulares (Resumen)

Resumen de la clase 5 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Lenguajes Regulares

Son **lenguajes regulares**:

- Los lenguajes reconocidos por los **Autómatas Finitos**.
- Los lenguajes descriptos mediante las **Expresiones Regulares**.
- Los lenguajes generados por las **Gramáticas Regulares**.

---

## ¿Cómo demostrar que $L$ es Regular?

$L$ es regular si se encuentra:

1. Un **Autómata Finito** que lo reconozca, o
2. Una **Expresión Regular** que lo describa, o
3. Una **Gramática Regular** que lo genere.

---

## Conversión de Autómata Finito a Gramática Regular

> [!info] Teorema
> Si $L$ es un lenguaje aceptado por un autómata finito $M$, entonces existe una gramática regular $G$ tal que $L = L(M) = L(G)$.

**Entrada:** $M = \langle Q, \Sigma, q_0, F, \delta \rangle$

**Salida:** $G = \langle Q, \Sigma, q_0, P \rangle$ donde $P$ se obtiene por las reglas:

- Si $\delta(q, a) = p$, añadir a $P$ la regla $q \to ap$
- Si $q_f \in F$, añadir a $P$ la regla $q_f \to \lambda$

---

## Conversión de Gramática Regular a Autómata Finito

> [!info] Teorema
> Si $L$ es un lenguaje generado por una gramática regular $G$, entonces existe un autómata finito $M$ tal que $L = L(M) = L(G)$.

**Entrada:** $G = \langle V, \Sigma, S, P \rangle$

**Salida:** $M = \langle V \cup \{q_f\}, \Sigma, S, \{q_f\}, \delta \rangle$ donde $\delta$ se obtiene por las reglas:

- Si $A \to aB \in P \Rightarrow \delta(A, a) = B$
- Si $A \to a \in P \Rightarrow \delta(A, a) = q_f$
- Si $A \to \lambda \in P \Rightarrow \delta(A, \lambda) = q_f$

> [!warning] Importante
> La gramática debe ser **Lineal Derecha**: $A \to tB$

---

## ¿Cómo demostrar que $L$ NO es Regular?

- Todo **lenguaje finito** es regular.
- Si un lenguaje es **infinito**, para demostrar que no es regular se usa el **Lema de Bombeo**.

---

## Lema de Bombeo para Lenguajes Regulares

> [!info] Lema de Bombeo
> Si $L$ es un lenguaje regular infinito, entonces:
>
> $\exists n / \forall \omega \in L, |\omega| \geq n$, podemos dividir $\omega$ en 3 cadenas, $\omega = xyz$ de modo que:
>
> 1. $y \neq \lambda$
> 2. $|xy| \leq n$
> 3. $\forall k \geq 0: xy^kz \in L$

---

### Demostración

Si $L$ es un lenguaje regular infinito, entonces $L = L(A)$ para algún AFD $A = \langle Q, \Sigma, q_0, \delta, F \rangle$ donde $\#Q = n$.

Consideremos $\omega \in L, |\omega| \geq n$, por ejemplo $\omega = a_1 a_2 a_3 \dots a_m$ con $m \geq n$ y $a_i \in \Sigma, \forall i$.

Para $i = 0, 1, 2, \dots, n$ definimos el estado $p_i$ como $\hat{\delta}(q_0, a_1 a_2 \dots a_i) = p_i$ (con $q_0 = p_0$).

No es posible que todos los $n+1$ estados $p_i$ sean diferentes, pues solo hay $n$ estados distintos. Por lo tanto, existen dos enteros distintos $i$ y $j$, con $0 \leq i < j \leq n$, tales que $p_i = p_j$.

Descomponemos $\omega = xyz$ como:

$$x = a_1 a_2 \dots a_i, \quad y = a_{i+1} \dots a_j, \quad z = a_{j+1} \dots a_m$$

- $x$ puede estar vacía si $i = 0$, y $z$ puede estarlo si $j = m = n$.
- Pero $y$ no puede estar vacía porque $i < j$ (estricto).

**Para $k = 0$:** recibe $xz = a_1 \dots a_i \cdot a_{j+1} \dots a_m$. Como $\hat{\delta}(q_0, xz) = \hat{\delta}(p_i, z) = p_f$, entonces $xz$ es aceptada.

**Para $k > 0$:** recibe $xy^kz$. Como $\hat{\delta}(q_0, xy \dots yz) = \hat{\delta}(p_i, y \dots yz) = \dots = \hat{\delta}(p_j, z) = p_f$, entonces $xy^kz$ es aceptada.

Por lo tanto, $\forall k \geq 0, xy^kz \in L(A)$.

---

### ¿Cómo se usa el Lema de Bombeo?

Se **supone** que el lenguaje es regular y que debe cumplir el lema de bombeo. El objetivo es **contradecir** el enunciado, mostrando que no se cumple.

| El lema dice…                                            | Para contradecir, hay que encontrar…      |             |                                                       |     |                |
| -------------------------------------------------------- | ----------------------------------------- | ----------- | ----------------------------------------------------- | --- | -------------- |
| $\exists n$ tal que $\forall \omega \in L,$              | $\omega$                                  | $\geq n$... | **UNA** palabra de longitud $\geq n$ que no lo cumpla |     |                |
| $\exists$ partición $\omega = xyz$ con las 3 condiciones | Que para **cualquier** partición posible: |             |                                                       |     |                |
| 1. $y \neq \lambda$                                      | 1. $y = \lambda$, o bien:                 |             |                                                       |     |                |
| 2. $                                                     | xy                                        | $\leq n$    | $2.$                                                  | xy  | $> n$, o bien: |
| 3. $xy^kz \in L, \forall k \geq 0$                       | 3. $\exists k \geq 0 \mid xy^kz \notin L$ |             |                                                       |     |                |
|                                                          |                                           |             |                                                       |     |                |

---

### Ejemplo 1: $L = \{\omega \in \{a,b\}^* \mid \omega = a^m b^{2m}\}$ no es regular

Supongo que sí lo es, y que cumple el lema de bombeo.

Entonces existe $\omega \in L$ tal que $|\omega| \geq N$, por ejemplo $\omega = a^N b^{2N}$, cuya $|\omega| = 3N$.

Existe partición $\omega = xyz$ con $|xy| \leq N$, por lo que $xy = a^{|xy|}$ y $z = a^{N-|xy|} b^{2N}$.

Tiene que comprobarse que $xy^iz \in L, \forall i \geq 0$.

Para $i = 0$: $xy^0z = a^{|x|} \cdot a^{N-|xy|} b^{2N} = a^{N-|y|} b^{2N}$.

Como $|y| \geq 1$, la cantidad de $b$ no es el doble de la cantidad de $a$, por lo que $xy^0z \notin L$.

**Por lo tanto, $L$ no es regular.**

---

### Ejemplo 2: $L = \{\omega \in \{a\}^* \mid \omega = a^m \text{ con } m = n^2\}$ no es regular

Supongo que sí lo es, y que cumple el lema de bombeo.

Existe $\omega \in L$ tal que $|\omega| \geq N$, por ejemplo $\omega = a^{N^2}$, cuya $|\omega| = N^2$.

Existe partición $\omega = xyz$ con $|xy| \leq N$, por lo que $xy = a^{|xy|}$ y $z = a^{N^2 - |xy|}$.

Para $i = 2$: $xy^2z = a^{|xy|+|y|} \cdot a^{N^2 - |xy|} = a^{N^2 + |y|}$.

- Como $|y| \geq 1$: $N^2 < N^2 + |y|$ ... (a)
- Como $|xy| \leq N$: $|y| \leq N \leq 2N < 2N+1$, por lo tanto $N^2 + |y| < N^2 + 2N + 1 = (N+1)^2$ ... (b)

Por (a) y (b): $N^2 < N^2 + |y| < (N+1)^2$, es decir, no es un cuadrado perfecto.

Por lo tanto $xy^2z \notin L$.

**Por lo tanto, $L$ no es regular.**

---

## Propiedades de clausura de los Lenguajes Regulares

Si $L$ y $M$ son lenguajes regulares:

| Propiedad | Justificación |
|---|---|
| $L \cup M$ es regular | Usando expresiones regulares |
| $L^*$ es regular | Usando expresiones regulares |
| $L \cdot M$ es regular | Usando expresiones regulares |
| $\overline{L}$ es regular | Cambiando finales por no finales en el AFD |
| $L \cap M$ es regular | Dos demostraciones (ver abajo) |
| $L - M$ es regular | Usando la intersección: $L - M = L \cap \overline{M}$ |
| $L^R$ es regular | Por inducción |

---

### Intersección de Lenguajes Regulares

> [!info] Teorema
> Si $L$ y $M$ son lenguajes regulares, entonces $L \cap M$ también es regular.

**Demostración 1 (por De Morgan):**

$$L \cap M = \overline{\overline{L} \cup \overline{M}}$$

Y ya se vio que la unión y el complemento preservan la regularidad.

**Demostración 2 (producto cartesiano de autómatas):**

Sean $A_L = \langle Q_L, \Sigma, q_L, \delta_L, F_L \rangle$ y $A_M = \langle Q_M, \Sigma, q_M, \delta_M, F_M \rangle$ los AFD para $L$ y $M$ respectivamente.

Se construye:

$$A = \langle Q_L \times Q_M, \Sigma, (q_L, q_M), \delta, F_L \times F_M \rangle$$

con $\delta((p, q), a) = (\delta_L(p, a), \delta_M(q, a))$.

Entonces $L(A) = L \cap M$, ya que:

$$\forall \omega \in \Sigma^*: \omega \in L(A) \iff \hat{\delta}((q_L, q_M), \omega) \in F_L \times F_M$$

$$\hat{\delta}((q_L, q_M), \omega) = (\hat{\delta}_L(q_L, \omega), \hat{\delta}_M(q_M, \omega)) \in F_L \times F_M$$

Esto ocurre si $\hat{\delta}_L(q_L, \omega) \in F_L \wedge \hat{\delta}_M(q_M, \omega) \in F_M$, es decir $\omega \in L \wedge \omega \in M \iff \omega \in L \cap M$.

---

## Preguntas

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -intro resumen]] — tema anterior
- [[TLA -Expresiones Regulares]] — tema siguiente
- [[TLA -Autómatas Finitos Determinísticos]] — el modelo que los reconoce
- [[TLA -Formas Normales y Lema de Bombeo CFL]] — el lema de bombeo para CFL

<!-- notas-relacionadas:fin -->
