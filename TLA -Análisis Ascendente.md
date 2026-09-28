---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-05-13"
Materia: "[[TLA.base|TLA]]"
temas:
  - Análisis Sintáctico Ascendente
  - Desplazamiento-Reducción
  - Pivote y Prefijos Viables
  - Conflictos D-R y R-R
  - Análisis Sintáctico LR
  - Jerarquía de gramáticas LR
  - Autómata LR(0) e Ítems
  - Algoritmo CLAUSURA e IR-A
  - Colección Canónica LR(0)
  - Algoritmo de Análisis LR
  - Tabla LR(0)
  - Tabla SLR(1)
  - Ítems LR(1)
  - Tabla LR(1) canónica
  - Núcleo y Tabla LALR
---
# TLA - Análisis Ascendente (Resumen)

Resumen de la clase 9 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación

En la clase anterior vimos el análisis sintáctico descendente (LL) y la idea general de los métodos ascendentes. Esta clase profundiza el análisis ascendente: primero como **desplazamiento-reducción** y luego como familia **LR**, usando autómatas LR(0), tablas SLR(1), ítems LR(1) y fusión LALR.

La idea central es mantener una pila que contiene un prefijo viable de una forma sentencial derecha y decidir, a partir del estado del autómata y del símbolo de entrada, si conviene desplazar o reducir.

---

## Análisis Sintáctico Ascendente

> [!info] Definición
> El **análisis sintáctico ascendente** es el proceso de reducir una cadena $\omega$ al símbolo inicial de la gramática.

En un método ascendente, el árbol de derivación se construye desde las hojas hacia la raíz. En cada paso se busca una subcadena que pueda ser el lado derecho de alguna producción y se reemplaza por el no terminal correspondiente.

> [!warning] Importante
> El problema clave es determinar **en qué momento hacer una reducción** y **qué producción aplicar**.

> [!example] Ejemplo
> Sea $G = \langle \{S,A,B\},\{a,b,c\},P,S\rangle$ con:
> $$S \to bS \mid cAB \qquad A \to bA \mid a \qquad B \to aBc \mid b$$
>
> Para $\omega = bcbab$, una reducción ascendente posible es:
> $$bcbab \Rightarrow bcbAb \Rightarrow bcAb \Rightarrow bcAB \Rightarrow bS \Rightarrow S$$

---

## Análisis Sintáctico de Desplazamiento-Reducción

El análisis de **desplazamiento-reducción** desplaza símbolos de la entrada a una pila hasta que una parte superior de la pila puede reemplazarse por un no terminal de la gramática.

- **Desplazamiento**: mover un símbolo de la cadena de entrada al tope de la pila.
- **Reducción**: reemplazar una subcadena $\beta$ de la pila que coincide con el cuerpo de alguna producción $A \to \beta$ por $A$.

> [!info] Definición — Pivote
> Dada $\alpha\rho\beta$, $\rho$ es **pivote** sii existe $(N \to \rho) \in P$ tal que:
> $$S \Rightarrow^* \alpha N \beta \Rightarrow \alpha \rho \beta$$

> [!info] Definición — Prefijo viable
> Dada $\alpha\rho\beta$, forma sentencial derecha, donde $\rho$ es pivote, $\gamma$ es **prefijo viable** si $\gamma$ es prefijo de $\alpha\rho$.
>
> El contenido de la pila siempre es un prefijo viable.

> [!note] Algoritmo de desplazamiento-reducción
> **Configuración inicial:**
>
> | Pila | Entrada |
> |---|---|
> | $\$$ | $\omega\$$ |
>
> ```
> Repetir
>   Desplazar cero o más símbolos de entrada a la pila.
>   Reducir una cadena β de símbolos de la parte superior de la pila a A,
>     si existe A → β en la gramática.
> Hasta que sea error o (pila == $S y entrada == $)
> ```
>
> **Configuración final:**
>
> | Pila | Entrada |
> |---|---|
> | $\$S$ | $\$$ |

> [!example] Ejemplo — conflicto en $id*id$
> Sea la gramática:
> $$E \to E+T \mid T \qquad T \to T*F \mid F \qquad F \to (E) \mid id$$
>
> Una estrategia que reduce demasiado temprano falla:
>
> | Pila | Entrada | Acción |
> |---|---|---|
> | $\$$ | $id*id\$$ | Desplazar |
> | $\$id$ | $*id\$$ | Reducir con $F \to id$ |
> | $\$F$ | $*id\$$ | Reducir con $T \to F$ |
> | $\$T$ | $*id\$$ | Reducir con $E \to T$ |
> | $\$E$ | $*id\$$ | Desplazar $*$ |
> | $\$E*$ | $id\$$ | Desplazar $id$ |
> | $\$E*id$ | $\$$ | Error: no hay reducción válida |

> [!example] Ejemplo — análisis exitoso de $id*id$
> La misma entrada se acepta si se posterga la reducción $E \to T$ hasta después de reconocer $T*F$:
>
> | Pila | Entrada | Acción |
> |---|---|---|
> | $\$$ | $id*id\$$ | Desplazar |
> | $\$id$ | $*id\$$ | Reducir con $F \to id$ |
> | $\$F$ | $*id\$$ | Reducir con $T \to F$ |
> | $\$T$ | $*id\$$ | Desplazar $*$ |
> | $\$T*$ | $id\$$ | Desplazar $id$ |
> | $\$T*id$ | $\$$ | Reducir con $F \to id$ |
> | $\$T*F$ | $\$$ | Reducir con $T \to T*F$ |
> | $\$T$ | $\$$ | Reducir con $E \to T$ |
> | $\$E$ | $\$$ | Aceptar |
>
> Derivación inversa:
> $$id*id \Rightarrow F*id \Rightarrow T*id \Rightarrow T*F \Rightarrow T \Rightarrow E$$
>
> Derivación más a la derecha correspondiente:
> $$E \Rightarrow T \Rightarrow T*F \Rightarrow T*id \Rightarrow F*id \Rightarrow id*id$$

> [!warning] Importante — Conflictos
> Hay gramáticas libres de contexto para las cuales el análisis de desplazamiento-reducción no puede decidir una acción única.
>
> - **Conflicto D-R**: no puede decidir si desplazar o reducir.
> - **Conflicto R-R**: no puede decidir qué reducción realizar.

---

## Análisis Sintáctico LR

> [!info] Definición — LR(k)
> **LR(k)** significa:
> - **L**eft-to-right scanning: la entrada se explora de izquierda a derecha.
> - **R**ightmost inverse derivation: se obtienen derivaciones por derecha revertidas.
> - $k$ símbolos de anticipación para decidir la acción.

Las gramáticas para las cuales puede construirse un analizador LR se llaman **gramáticas LR**. Son más expresivas que las gramáticas LL y son la base de analizadores sintácticos ascendentes eficientes.

Jerarquía conceptual:
$$\text{LL}(0) \subsetneq \text{LL}(1) \subsetneq \text{LL}(k) \subsetneq \text{LR}(k)$$
$$\text{LR}(0) \subsetneq \text{SLR}(1) \subsetneq \text{LALR}(1) \subsetneq \text{LR}(1) \subsetneq \text{LR}(k)$$

En texto:
$$\text{LR}(0) \to \text{SLR}(1) \to \text{LALR}(1) \to \text{LR}(1)$$

### Autómata LR(0)

> [!info] Definición — Ítem LR(0)
> Un **ítem LR(0)** es una producción con un punto en alguna posición:
> $$[A \to \alpha_1 \bullet \alpha_2]$$
>
> La gramática se aumenta con un nuevo símbolo inicial $S'$ y la producción $S' \to S$.

> [!info] Construcción — Autómata LR(0)
> - **Estados**: conjuntos de ítems de la colección canónica LR(0).
> - **Transiciones**: función $\text{IR-A}(I,X)$, donde $I$ es un conjunto de ítems y $X$ un símbolo gramatical.
> - **Estado inicial**: $\text{CLAUSURA}(\{[S' \to \bullet S]\})$.

También puede pensarse como un AFN-$\lambda$ con un ítem por estado, transiciones con símbolos gramaticales o $\lambda$, pero con $\text{CLAUSURA}$ se obtiene directamente el AFD.

### Algoritmo CLAUSURA(I)

**Entrada:** un conjunto $I$ de ítems de $G$.  
**Salida:** un conjunto $J$ de ítems de $G$, $J = \text{CLAUSURA}(I)$.  
**Pasos:**

> [!note] Algoritmo CLAUSURA(I)
> ```
> J = I
> Repetir
>   para cada [A → α•Bβ] en J:
>     para cada B → γ en G:
>       agregar [B → •γ] a J
> Hasta que no se agreguen nuevos elementos a J.
> retornar J
> ```

### Función IR-A(I, X)

**Entrada:** una $\text{CLAUSURA}(I)$ y un símbolo gramatical $X$.  
**Salida:** una $\text{CLAUSURA}(J)$.  
**Definición:** $\text{IR-A}(I,X)$ es la clausura del conjunto de todos los ítems $[A \to \alpha X \bullet \beta]$ tales que $[A \to \alpha \bullet X\beta] \in I$.

La función $\text{IR-A}(I,X)$ especifica la transición del estado $I$ con la entrada $X$ en el autómata LR(0).

### Colección canónica LR(0)

**Entrada:** una gramática aumentada $G'$.  
**Salida:** colección canónica de conjuntos de ítems LR(0).  
**Pasos:**

> [!note] Algoritmo de colección canónica LR(0)
> ```
> C = CLAUSURA({[S' → •S]})
> Repetir
>   para cada conjunto de ítems I en C:
>     para cada símbolo gramatical X:
>       Si IR-A(I, X) ≠ ∅ y IR-A(I, X) ∉ C:
>         Agregar IR-A(I, X) a C
> Hasta que no se agreguen nuevos conjuntos de ítems a C.
> retornar C
> ```

> [!example] Ejemplo pequeño — $E' \to E$, $E \to (E) \mid id$
> Para la gramática aumentada:
> $$E' \to E \qquad E \to (E) \mid id$$
>
> La colección LR(0) tiene 6 estados:
>
> | Estado | Ítems |
> |---|---|
> | $I_0$ | $E' \to \bullet E$; $E \to \bullet(E)$; $E \to \bullet id$ |
> | $I_1$ | $E' \to E\bullet$ |
> | $I_2$ | $E \to id\bullet$ |
> | $I_3$ | $E \to (\bullet E)$; $E \to \bullet(E)$; $E \to \bullet id$ |
> | $I_4$ | $E \to (E\bullet)$ |
> | $I_5$ | $E \to (E)\bullet$ |
>
> Transiciones:
>
> | Desde | Símbolo | Hacia |
> |---|---|---|
> | $I_0$ | $E$ | $I_1$ |
> | $I_0$ | $id$ | $I_2$ |
> | $I_0$ | $($ | $I_3$ |
> | $I_3$ | $id$ | $I_2$ |
> | $I_3$ | $($ | $I_3$ |
> | $I_3$ | $E$ | $I_4$ |
> | $I_4$ | $)$ | $I_5$ |

**Tabla LR(0)** para el ejemplo:

| Estado | $($ | $id$ | $)$ | $\$$ | $E$ |
|---|---|---|---|---|---|
| 0 | Shift 3 | Shift 2 | error | error | 1 |
| 1 | Accept | Accept | Accept | Accept |  |
| 2 | Reduce $E \to id$ | Reduce $E \to id$ | Reduce $E \to id$ | Reduce $E \to id$ |  |
| 3 | Shift 3 | Shift 2 | error | error | 4 |
| 4 | error | error | Shift 5 | error |  |
| 5 | Reduce $E \to (E)$ | Reduce $E \to (E)$ | Reduce $E \to (E)$ | Reduce $E \to (E)$ |  |

**Traza para $((id))\$$**:

| Paso | Pila c/símbolos | Entrada | Acción |
|---|---|---|---|
| 1 | $\$0$ | $((id))\$$ | Desplazar |
| 2 | $\$0(3$ | $(id))\$$ | Desplazar |
| 3 | $\$0(3(3$ | $id))\$$ | Desplazar |
| 4 | $\$0(3(3id2$ | $))\$$ | Reducir: $E \to id$ e ir a 4 |
| 5 | $\$0(3(3E4$ | $))\$$ | Desplazar |
| 6 | $\$0(3(3E4)5$ | $)\$$ | Reducir: $E \to (E)$ e ir a 4 |
| 7 | $\$0(3E4$ | $)\$$ | Desplazar |
| 8 | $\$0(3E4)5$ | $\$$ | Reducir: $E \to (E)$ e ir a 1 |
| 9 | $\$0E1$ | $\$$ | Aceptar: reducir $E' \to E$ |
| 10 | $\$0E'$ | $\$$ | Fin |

### Algoritmo de Análisis LR (genérico)

> [!note] Pseudocódigo
> ```
> Configuración inicial:
>   Estado inicial en pila, cadena ω en entrada.
>
> Mientras (1)
>   Sea s el estado en el tope de la pila.
>   Sea a el símbolo de entrada.
>   Según ACCIÓN[s, a]:
>     caso desplazar t:
>       meter t en la pila, consumir a.
>     caso reducir A → β:
>       sacar |β| = r símbolos de la pila.
>       sea t el tope de la pila.
>       apilar IR-A(t, A) = q.
>     caso aceptar:
>       break.
>     caso error:
>       manejar error.
> ```

> [!warning] Importante
> Todos los analizadores sintácticos LR se comportan con esta misma estructura. La única diferencia entre un analizador LR y otro es la información en los campos **ACCIÓN** e **IR-A** de la tabla.

---

## Tabla SLR(1)

**Entrada:** una gramática aumentada $G'$.  
**Salida:** las funciones ACCIÓN e IR-A para $G'$ de la tabla SLR.  
**Pasos:**

> [!info] Construcción — Tabla SLR(1)
> 1. Construir $C = \{I_0, I_1, \dots, I_n\}$, la colección de conjuntos de ítems LR(0) para $G'$.
> 2. Para el estado $i$:
>    - Si $a \in \Sigma$, $[A \to \alpha \bullet a\beta] \in I_i$ e $\text{IR-A}(I_i,a)=I_j$, establecer $\text{ACCIÓN}[i,a] = \text{desplazar } j$.
>    - Si $[A \to \alpha\bullet] \in I_i$, establecer $\text{ACCIÓN}[i,a] = \text{reducir } A \to \alpha$ para todo $a \in \text{SIGUIENTE}(A)$.
>    - Si $[S' \to S\bullet] \in I_i$, establecer $\text{ACCIÓN}[i,\$] = \text{aceptar}$.
> 3. Para todo no terminal $A$, si $\text{IR-A}(I_i,A)=I_j$, entonces $\text{IR-A}[i,A]=j$.
> 4. Todas las entradas que no estén definidas por las reglas anteriores se dejan como error.
> 5. El estado inicial es el construido a partir del conjunto que contiene $[S' \to \bullet S]$.

> [!warning] Importante
> Si resulta cualquier acción conflictiva por las reglas anteriores, la gramática no es SLR(1) y no se produce el analizador sintáctico.
>
> Toda gramática SLR(1) es no ambigua, pero no toda gramática no ambigua es SLR(1).

> [!example] Ejemplo grande — Gramática de expresiones
> Producciones:
>
> | Nro. | Producción |
> |---|---|
> | 0 | $E' \to E$ |
> | 1 | $E \to E+T$ |
> | 2 | $E \to T$ |
> | 3 | $T \to T*F$ |
> | 4 | $T \to F$ |
> | 5 | $F \to (E)$ |
> | 6 | $F \to id$ |
>
> Conjuntos de siguientes:
> $$\text{SIGUIENTE}(E') = \{\$\}$$
> $$\text{SIGUIENTE}(E) = \{\$,+,)\}$$
> $$\text{SIGUIENTE}(T) = \{\$,+,),*\}$$
> $$\text{SIGUIENTE}(F) = \{\$,+,),*\}$$

Tabla SLR(1) resultante:

| Estado | $id$ | $+$ | $*$ | $($ | $)$ | $\$$ | $E$ | $T$ | $F$ |
|---|---|---|---|---|---|---|---|---|---|
| 0 | s5 |  |  | s4 |  |  | 1 | 2 | 3 |
| 1 |  | s6 |  |  |  | acepta |  |  |  |
| 2 |  | r2 | s7 |  | r2 | r2 |  |  |  |
| 3 |  | r4 | r4 |  | r4 | r4 |  |  |  |
| 4 | s5 |  |  | s4 |  |  | 8 | 2 | 3 |
| 5 |  | r6 | r6 |  | r6 | r6 |  |  |  |
| 6 | s5 |  |  | s4 |  |  |  | 9 | 3 |
| 7 | s5 |  |  | s4 |  |  |  |  | 10 |
| 8 |  | s6 |  |  | s11 |  |  |  |  |
| 9 |  | r1 | s7 |  | r1 | r1 |  |  |  |
| 10 |  | r3 | r3 |  | r3 | r3 |  |  |  |
| 11 |  | r5 | r5 |  | r5 | r5 |  |  |  |

Donde $s_x$ significa **shift** al estado $x$ y $r_y$ significa **reduce** con la producción $y$.

**Traza para $id*id+id\$$**:

| Paso | Pila     | Símbolos | Entrada      | Acción                       |
| ---- | -------- | -------- | ------------ | ---------------------------- |
| 1    | 0        |          | $id*id+id\$$ | Desplazar                    |
| 2    | 0 5      | $id$     | $*id+id\$$   | Reducir mediante $F \to id$  |
| 3    | 0 3      | $F$      | $*id+id\$$   | Reducir mediante $T \to F$   |
| 4    | 0 2      | $T$      | $*id+id\$$   | Desplazar                    |
| 5    | 0 2 7    | $T*$     | $id+id\$$    | Desplazar                    |
| 6    | 0 2 7 5  | $T*id$   | $+id\$$      | Reducir mediante $F \to id$  |
| 7    | 0 2 7 10 | $T*F$    | $+id\$$      | Reducir mediante $T \to T*F$ |
| 8    | 0 2      | $T$      | $+id\$$      | Reducir mediante $E \to T$   |
| 9    | 0 1      | $E$      | $+id\$$      | Desplazar                    |
| 10   | 0 1 6    | $E+$     | $id\$$       | Desplazar                    |
| 11   | 0 1 6 5  | $E+id$   | $\$$         | Reducir mediante $F \to id$  |
| 12   | 0 1 6 3  | $E+F$    | $\$$         | Reducir mediante $T \to F$   |
| 13   | 0 1 6 9  | $E+T$    | $\$$         | Reducir mediante $E \to E+T$ |
| 14   | 0 1      | $E$      | $\$$         | Aceptar                      |

---

## Analizadores Sintácticos más Potentes

Hay dos métodos más potentes que SLR:

1. **LR Canónico** (o LR) — usa ítems LR(1).
2. **LALR** (*Look Ahead LR*) — tiene menos estados y tablas no tan grandes.

### Ítems LR(1)

> [!info] Definición
> Un **ítem LR(1)** tiene la forma:
> $$[A \to \alpha \bullet \beta,\; t]$$
> donde $A \to \alpha\beta$ es una producción y $t \in \Sigma \cup \{\$\}$.

El terminal $t$ es el símbolo de anticipación (*lookahead*) para decidir reducciones.

**Construcción de conjuntos de ítems LR(1):** se usa la misma idea de colección canónica, pero cada ítem conserva su lookahead.

> [!note] Algoritmo CLAUSURA LR(1)
> ```
> J = I
> Repetir
>   para cada [A → α•Bβ, t] en J:
>     para cada producción B → γ:
>       para cada b ∈ PRIMERO(βt):
>         agregar [B → •γ, b] a J
> Hasta que no se agreguen nuevos elementos a J.
> retornar J
> ```

Por ejemplo, si aparece $[A \to \alpha \bullet B\beta,\; t]$, las producciones de $B$ se agregan con lookahead en $\text{PRIMERO}(\beta t)$.

### Tabla de Análisis LR(1)

**Entrada:** una gramática aumentada $G'$.  
**Salida:** las funciones ACCIÓN e IR-A para $G'$ de la tabla LR(1).  
**Pasos:**

> [!info] Construcción — Tabla LR(1)
> 1. Construir $C = \{I_0, I_1, \dots, I_n\}$, la colección de conjuntos de ítems LR(1) para $G'$.
> 2. Para el estado $i$:
>    - Si $a \in \Sigma$, $[A \to \alpha \bullet a\beta,\; b] \in I_i$ e $\text{IR-A}(I_i,a)=I_j$, establecer $\text{ACCIÓN}[i,a] = \text{desplazar } j$.
>    - Si $[A \to \alpha\bullet,\; a] \in I_i$ y $A \neq S'$, establecer $\text{ACCIÓN}[i,a] = \text{reducir } A \to \alpha$.
>    - Si $[S' \to S\bullet,\; \$] \in I_i$, establecer $\text{ACCIÓN}[i,\$] = \text{aceptar}$.
> 3. Para todo no terminal $A$, si $\text{IR-A}(I_i,A)=I_j$, entonces $\text{IR-A}[i,A]=j$.
> 4. Todas las entradas que no estén definidas por las reglas anteriores se dejan como error.
> 5. El estado inicial es el construido a partir del conjunto que contiene $[S' \to \bullet S,\; \$]$.

> [!warning] Importante
> En LR(1), una reducción se coloca sólo en el lookahead específico del ítem. En SLR(1), en cambio, la reducción se coloca para todos los símbolos de $\text{SIGUIENTE}(A)$.

> [!example] Ejemplo LR(1) — $S \to CC$, $C \to cC \mid d$
> Gramática aumentada:
> $$S' \to S \qquad S \to CC \qquad C \to cC \mid d$$
>
> Producciones:
>
> | Nombre | Producción |
> |---|---|
> | Acc | $S' \to S$ |
> | R1 | $S \to CC$ |
> | R2 | $C \to cC$ |
> | R3 | $C \to d$ |
>
> Estados LR(1):
>
> | Estado | Ítems |
> |---|---|
> | $I_0$ | $[S' \to \bullet S,\$]$; $[S \to \bullet CC,\$]$; $[C \to \bullet cC,c/d]$; $[C \to \bullet d,c/d]$ |
> | $I_1$ | $[S' \to S\bullet,\$]$ |
> | $I_2$ | $[S \to C\bullet C,\$]$; $[C \to \bullet cC,\$]$; $[C \to \bullet d,\$]$ |
> | $I_3$ | $[C \to c\bullet C,c/d]$; $[C \to \bullet cC,c/d]$; $[C \to \bullet d,c/d]$ |
> | $I_4$ | $[C \to d\bullet,c/d]$ |
> | $I_5$ | $[S \to CC\bullet,\$]$ |
> | $I_6$ | $[C \to c\bullet C,\$]$; $[C \to \bullet cC,\$]$; $[C \to \bullet d,\$]$ |
> | $I_7$ | $[C \to d\bullet,\$]$ |
> | $I_8$ | $[C \to cC\bullet,c/d]$ |
> | $I_9$ | $[C \to cC\bullet,\$]$ |
>
> Tabla LR(1):
>
> | Estado | $c$ | $d$ | $\$$ | $S$ | $C$ |
> |---|---|---|---|---|---|
> | 0 | s3 | s4 |  | 1 | 2 |
> | 1 |  |  | acc |  |  |
> | 2 | s6 | s7 |  |  | 5 |
> | 3 | s3 | s4 |  |  | 8 |
> | 4 | r3 | r3 |  |  |  |
> | 5 |  |  | r1 |  |  |
> | 6 | s6 | s7 |  |  | 9 |
> | 7 |  |  | r3 |  |  |
> | 8 | r2 | r2 |  |  |  |
> | 9 |  |  | r2 |  |  |

### Núcleo de un Ítem LR(1)

> [!info] Definición
> El **núcleo** (*core*) de un ítem LR(1) $[A \to \alpha \bullet \beta,\; t]$ es su primer componente $A \to \alpha \bullet \beta$.

Si dos conjuntos de ítems LR(1) tienen el mismo núcleo, pueden fusionarse uniendo sus lookaheads.

En el ejemplo anterior:

- $I_3$ y $I_6$ tienen el mismo núcleo y se fusionan en $I_{36}$.
- $I_4$ y $I_7$ se fusionan en $I_{47}$.
- $I_8$ y $I_9$ se fusionan en $I_{89}$.

Por ejemplo:
$$I_3 \cup I_6 = I_{36}$$
y los lookaheads $c/d$ y $\$$ se combinan como $c/d/\$$.

### Tabla LALR

**Entrada:** una gramática aumentada $G'$.  
**Salida:** las funciones ACCIÓN e IR-A para $G'$ de la tabla LALR.  
**Pasos:**

> [!info] Construcción — Tabla LALR
> 1. Construir $C = \{I_0, I_1, \dots, I_n\}$, la colección de conjuntos de ítems LR(1) para $G'$.
> 2. Para cada núcleo presente entre los conjuntos de ítems LR(1), reemplazar todos los ítems que tengan ese mismo núcleo por su unión.
> 3. Sea $C' = \{J_0, J_1, \dots, J_m\}$ la colección resultante. Construir ACCIÓN e IR-A a partir de $C'$ como en LR(1). Si hay conflicto, la gramática no es LALR(1).
> 4. Si $J = I_1 \cup I_2 \cup \cdots \cup I_k$, entonces $\text{IR-A}(J,X)$ es la unión de los conjuntos de ítems con el mismo núcleo que los $\text{IR-A}(I_r,X)$.

> [!warning] Importante
> LALR tiene menos estados que LR canónico. Si la fusión de conjuntos con igual núcleo genera un conflicto, la gramática no es LALR(1).

> [!example] Ejemplo LALR — misma gramática
> Para $S' \to S$, $S \to CC$, $C \to cC \mid d$, la tabla LALR fusiona los 10 estados LR(1) en 7 estados:
> $$0,\;1,\;2,\;36,\;47,\;5,\;89$$
>
> | Estado | $c$ | $d$ | $\$$ | $S$ | $C$ |
> |---|---|---|---|---|---|
> | 0 | s36 | s47 |  | 1 | 2 |
> | 1 |  |  | acc |  |  |
> | 2 | s36 | s47 |  |  | 5 |
> | 36 | s36 | s47 |  |  | 89 |
> | 47 | r3 | r3 | r3 |  |  |
> | 5 |  |  | r1 |  |  |
> | 89 | r2 | r2 | r2 |  |  |

---

## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [TLA -Análisis Sintáctico](TLA%20-Análisis%20Sintáctico.md) — tema anterior
- [TLA -Análisis Semántico](TLA%20-Análisis%20Semántico.md) — tema siguiente
- [TLA -Autómatas de Pila](TLA%20-Autómatas%20de%20Pila.md) — el autómata LR(0)
- [frontend](frontend.md) — Bison genera un parser LALR

<!-- notas-relacionadas:fin -->
