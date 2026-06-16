---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-05-06
Materia: "[[TLA.base|TLA]]"
temas:
  - Análisis Sintáctico
  - Métodos descendentes y ascendentes
  - Análisis Sintáctico de Descenso Recursivo
  - Eliminación de recursividad por izquierda
  - Factorización por izquierda
  - Conjunto de PRIMEROS y SIGUIENTES
  - Gramáticas LL(1)
  - Análisis Sintáctico Predictivo No Recursivo
  - Análisis Sintáctico de Desplazamiento-Reducción
  - Ítems y autómata LR(0)
  - Tabla SLR(1)
  - LR Canónico y LALR
  - Generador Yacc
---
# TLA - Análisis Sintáctico (Resumen)

Resumen de la clase 8 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación

Las clases anteriores construyeron la teoría de autómatas y gramáticas: AF ↔ ER ↔ Lenguajes Regulares; AP ↔ GLC ↔ LLC, con sus Formas Normales y el Lema de Bombeo. Esta clase aplica ese marco al front-end de un compilador: el **analizador sintáctico** toma el flujo de tokens producido por el analizador léxico y verifica (y construye) la estructura gramatical del programa.

---

## Análisis Sintáctico

> [!info] Definición
> El **análisis sintáctico (parsing)** es el proceso de determinar cómo puede derivarse una cadena de terminales mediante una gramática.

La función del **analizador sintáctico** es obtener una cadena de tokens del analizador léxico y verificar que la cadena pueda generarse mediante la gramática. Para ello construye un **árbol de análisis** (árbol de derivación) en el que la raíz se etiqueta con el símbolo inicial y cada hoja con un terminal o con $\lambda$.

### Métodos de análisis sintáctico

> [!warning] Importante
> En todos los métodos, la entrada se explora de **izquierda a derecha**, un símbolo a la vez. Los métodos LL y LR —los más eficientes— sólo funcionan para subclases de gramáticas, pero esas subclases son suficientemente expresivas para los lenguajes de programación modernos.

| Tipo | Dirección del árbol | Derivación producida |
|---|---|---|
| **Descendentes** (top-down) | raíz → hojas | por izquierda |
| **Ascendentes** (bottom-up) | hojas → raíz | inversa (reducciones) |

> [!example] Ejemplo
> Sea $G = \langle \{S,A,B\},\{a,b,c\},S,P\rangle$ con $P = \{S \to bS \mid cAB,\; A \to bA \mid a,\; B \to aBc \mid b\}$. ¿Reconoce $\mathbf{bcbab}$?
>
> **Descendente** (derivación por izquierda):
> $$S \Rightarrow bS \Rightarrow bcAB \Rightarrow bcbAB \Rightarrow bcbaB \Rightarrow bcbab \checkmark$$
>
> **Ascendente** (reducciones sucesivas):
> $$bcbab \Rightarrow bcbAb \Rightarrow bcAb \Rightarrow bcAB \Rightarrow bS \Rightarrow S \checkmark$$

---

## Análisis Sintáctico Descendente

En cada paso del análisis descendente el problema clave es determinar **qué producción aplicar** para el no-terminal que se expande. Hay tres variantes:

- **Descenso recursivo** — caso general; puede requerir *backtracking*.
- **Predictivo** — sin *backtracking*; funciona para gramáticas LL(k).
- **Predictivo no recursivo** — igual que el anterior pero con pila explícita.

### Análisis Sintáctico de Descenso Recursivo

Consiste en un conjunto de funciones mutuamente recursivas, una por cada símbolo no terminal. La ejecución empieza invocando la función del símbolo inicial y termina con éxito si todo el buffer fue consumido.

> [!note] Pseudocódigo
> ```
> Principal
> Inicio
>   t apunta al inicio de ω
>   Si S(t) y t == EOF:  Acepta ω
>   Sino:                No acepta ω
> Fin
>
> Función A(t)  retorna booleano        // A → α₁ | α₂ | ... | αₖ
> Inicio
>   i ← 1
>   hacer
>     resultado ← procesar(A → αᵢ, t)
>     i ← i + 1
>   mientras resultado es False y i ≤ k
>   retornar resultado
> Fin
>
> Función procesar(A → X₁X₂...Xₙ, t)  retorna booleano
> Inicio
>   resultado ← True
>   para j = 1..n según Xⱼ:
>     Xⱼ ∈ Σ:  si t == Xⱼ → consumir(t),  sino → retornar False
>     Xⱼ ∈ N:  resultado ← Xⱼ(t);  si resultado == False → retornar False
>     Xⱼ = λ:  resultado ← True      // símbolo vacío, no hace nada
>   retornar resultado
> Fin
> ```

> [!example] Ejemplo
> Sea $G$: $S \to cAd$, $A \to ab \mid a$. Traza para $\omega = cad$:
>
> 1. Principal: t apunta a `c`. Invoca $S$(`c`).
> 2. $S$: procesar($S \to cAd$): t==`c` → consumir → t=`a`.
> 3. Invoca $A$(`a`): intenta procesar($A \to ab$): t==`a` → consumir → t=`d`. Pero t==`b`? NO → retorna False. Backtrack, t vuelve a `a`.
> 4. Intenta procesar($A \to a$): t==`a` → consumir → t=`d`. Retorna **True**.
> 5. Vuelve a $S$: t==`d` → consumir → t=EOF. Retorna **True**.
> 6. Principal: t==EOF → **Acepta** $cad$ ✓.

> [!warning] Importante
> - La cadena se analiza de izquierda a derecha; siempre se elige la derivación más a la izquierda.
> - **Si la gramática es recursiva por izquierda, el analizador entra en ciclo infinito** ($A(t)$ llama a $A(t)$ indefinidamente).

### Eliminación de Recursividad por Izquierda

> [!info] Definición
> Una gramática es **recursiva por izquierda** si tiene un no terminal $A$ tal que $A \Rightarrow^+ A\alpha$. La forma **inmediata** es cuando existe directamente $A \to A\alpha$.

**Eliminación de recursividad inmediata** — agrupar las producciones de $A$:
$$A \to A\alpha_1 \mid \cdots \mid A\alpha_m \mid \beta_1 \mid \cdots \mid \beta_n \qquad (\text{ningún } \beta_i \text{ empieza con } A)$$
y reemplazar por:
$$A \to \beta_1 A' \mid \cdots \mid \beta_n A' \qquad\qquad A' \to \alpha_1 A' \mid \cdots \mid \alpha_m A' \mid \lambda$$

**Algoritmo general** para eliminar toda recursividad por izquierda.

**Entrada**: $G = \langle N, \Sigma, P, S \rangle$ sin ciclos ($A \Rightarrow^+ A$) ni $\lambda$-producciones.
**Salida**: $G'$ equivalente sin recursividad a izquierda.
**Pasos**:
1. Ordenar los no terminales $A_1, A_2, \dots, A_n$.
2. Para cada $i$ de 1 a $n$:
   - Para cada $j$ de 1 a $i-1$:
     - Sustituir cada producción $A_i \to A_j\gamma$ por $A_i \to \delta_1\gamma \mid \delta_2\gamma \mid \cdots \mid \delta_k\gamma$, donde $A_j \to \delta_1 \mid \cdots \mid \delta_k$ son las producciones actuales de $A_j$.
   - Eliminar la recursividad inmediata por izquierda en $A_i$.

> [!example] Ejemplo
> $G$: $S \to Aa \mid b$, $A \to Ac \mid Sd \mid \lambda$.
>
> Con $A_1 = S$, $A_2 = A$. Para $i=2, j=1$: sustituir $A \to Sd$ usando $S \to Aa \mid b$ → $A \to Aad \mid bd$. Producciones de $A$: $A \to Ac \mid Aad \mid bd \mid \lambda$.
>
> Eliminar recursividad inmediata en $A$: $\alpha_1 = c$, $\alpha_2 = ad$; $\beta_1 = bd$, $\beta_2 = \lambda$.
>
> Resultado: $S \to Aa \mid b$, $\quad A \to bdA' \mid A'$, $\quad A' \to cA' \mid adA' \mid \lambda$.

---

## Análisis Sintáctico Predictivo

Una forma simple de análisis de descenso recursivo es el **análisis sintáctico predictivo**, que no requiere *backtracking*: el símbolo de pre-análisis determina sin ambigüedad qué producción aplicar para cada no terminal.

Se basa en los conjuntos de **PRIMEROS** y **SIGUIENTES**, que permiten elegir la producción con base en el siguiente símbolo de entrada.

### Factorización por Izquierda

Preprocesamiento necesario cuando varias alternativas de un no terminal comparten el mismo prefijo.

**Algoritmo de factorización por izquierda**.
**Entrada**: $G = \langle N, \Sigma, P, S \rangle$.
**Salida**: $G'$ factorizada por izquierda.
**Pasos**:
1. Por cada no terminal $A$, encontrar el prefijo $\alpha$ más largo común a dos o más alternativas.
2. Si $\alpha \neq \lambda$, reemplazar $A \to \alpha\beta_1 \mid \cdots \mid \alpha\beta_n \mid \gamma$ por:
$$A \to \alpha A' \mid \gamma \qquad\qquad A' \to \beta_1 \mid \cdots \mid \beta_n$$

> [!example] Ejemplo
> $S \to iEtS \mid iEtSeS \mid a$ (i = *if*, t = *then*, e = *else*, $a$ = expr, $E$ = cond).
>
> Prefijo común: $iEtS$. Factorizada:
> $$S \to iEtSS' \mid a \qquad S' \to eS \mid \lambda$$

### Conjunto de PRIMEROS

> [!info] Definición
> $$\text{PRIMERO}(\alpha) = \{t \in \Sigma \mid \alpha \Rightarrow^* t\beta\} \cup \{\lambda \mid \alpha \Rightarrow^* \lambda\}$$
>
> Es el conjunto de terminales que pueden **comenzar** las cadenas derivadas a partir de $\alpha$.

**Reglas para calcular PRIMERO**$(X)$, $\forall X \in N$:
1. $\forall t \in \Sigma:\; \text{PRIMERO}(t) = \{t\}$.
2. Si $X \to Y_1 Y_2 \cdots Y_k$: agregar $t \in \text{PRIMERO}(X)$ si $\exists\, i$ tal que $t \in \text{PRIMERO}(Y_i)$ y $\lambda \in \text{PRIMERO}(Y_1) \cap \cdots \cap \text{PRIMERO}(Y_{i-1})$.
3. Si $X \to \lambda \in P$: agregar $\lambda$ a $\text{PRIMERO}(X)$.

### Conjunto de SIGUIENTES

> [!info] Definición
> $$\text{SIGUIENTE}(A) = \{t \in \Sigma \mid S \Rightarrow^* \alpha A t \beta\} \cup \{\$ \mid S \Rightarrow^* \alpha A\}$$
>
> Es el conjunto de terminales que pueden aparecer **a la derecha** de $A$ en alguna forma sentencial. El símbolo $\$$ indica fin de cadena (EOF).

**Reglas para calcular SIGUIENTE**$(A)$, $\forall A \in N$:
1. Agregar $\$$ a $\text{SIGUIENTE}(S)$ (símbolo inicial).
2. Si $A \to \alpha B\beta \in P$: agregar $\text{PRIMERO}(\beta) \setminus \{\lambda\}$ a $\text{SIGUIENTE}(B)$.
3. Si $A \to \alpha B \in P$, o $A \to \alpha B\beta$ con $\lambda \in \text{PRIMERO}(\beta)$: agregar $\text{SIGUIENTE}(A)$ a $\text{SIGUIENTE}(B)$.

---

## Gramáticas LL(1)

Los analizadores sintácticos predictivos pueden construirse a partir de una clase de gramáticas llamadas **LL(1)**.

> [!info] Definición
> **LL(1)** = **L**eft-to-right scan · **L**eftmost derivation · **1** símbolo de anticipación.
>
> $G$ es LL(1) sii para toda producción $X \to \alpha_1 \mid \alpha_2 \mid \cdots \mid \alpha_n$:
> - $\text{PRIMERO}(\alpha_i) \cap \text{PRIMERO}(\alpha_j) = \emptyset$ para todo $i \neq j$.
> - Si $X \Rightarrow^* \lambda$, entonces $\text{PRIMERO}(X) \cap \text{SIGUIENTE}(X) = \emptyset$.

> [!warning] Importante
> Para que una gramática pueda ser LL(1) debe ser **no ambigua**, **sin recursividad por izquierda** y **factorizada por izquierda**. Pero hay gramáticas para las cuales ninguna modificación produce LL(1) (ej. aquellas con ambigüedad inherente).

### Tabla de análisis sintáctico predictivo

Construir tabla $M[A, a]$ (filas: no terminales; columnas: terminales $\cup\; \{\$\}$):

**Algoritmo**: para cada producción $A \to \alpha$:
1. $\forall a \in \text{PRIMERO}(\alpha)$: $M[A, a] = A \to \alpha$.
2. Si $\lambda \in \text{PRIMERO}(\alpha)$: $\forall b \in \text{SIGUIENTE}(A)$ (incluido $\$$): $M[A, b] = A \to \alpha$.
3. Las celdas no asignadas son **error**.

> [!warning] Importante
> Para cada gramática LL(1), cada entrada en $M$ identifica en forma única una producción. Si $G$ es recursiva por izquierda o no está factorizada, $M$ tendrá celdas con múltiples definiciones.

> [!example] Ejemplo
> Sea $G = \langle \{E,E',T,T',F\},\{+,*,(,),id\},E,P\rangle$ con:
> $$E \to TE' \quad E' \to +TE' \mid \lambda \quad T \to FT' \quad T' \to *FT' \mid \lambda \quad F \to (E) \mid id$$
>
> **PRIMEROS**:
> $$\text{PRIMERO}(E) = \text{PRIMERO}(T) = \text{PRIMERO}(F) = \{(,\; id\}$$
> $$\text{PRIMERO}(E') = \{+,\; \lambda\} \qquad \text{PRIMERO}(T') = \{*,\; \lambda\}$$
>
> **SIGUIENTES**:
> $$\text{SIGUIENTE}(E) = \{\$,\; )\} \qquad \text{SIGUIENTE}(E') = \text{SIGUIENTE}(E) = \{\$,\; )\}$$
> $$\text{SIGUIENTE}(T) = \{+\} \cup \text{SIGUIENTE}(E) = \{+,\; \$,\; )\}$$
> $$\text{SIGUIENTE}(T') = \text{SIGUIENTE}(T) = \{+,\; \$,\; )\}$$
> $$\text{SIGUIENTE}(F) = \{*\} \cup \text{SIGUIENTE}(T') = \{*,\; +,\; \$,\; )\}$$
>
> **Tabla $M$**:
>
> | NT | $id$ | $+$ | $*$ | $($ | $)$ | $\$$ |
> |---|---|---|---|---|---|---|
> | $E$ | $E \to TE'$ | — | — | $E \to TE'$ | — | — |
> | $E'$ | — | $E' \to +TE'$ | — | — | $E' \to \lambda$ | $E' \to \lambda$ |
> | $T$ | $T \to FT'$ | — | — | $T \to FT'$ | — | — |
> | $T'$ | — | $T' \to \lambda$ | $T' \to *FT'$ | — | $T' \to \lambda$ | $T' \to \lambda$ |
> | $F$ | $F \to id$ | — | — | $F \to (E)$ | — | — |

---

## Análisis Sintáctico Predictivo No Recursivo

Se puede construir un analizador predictivo **no recursivo** mediante el mantenimiento explícito de una pila, en vez de hacerlo mediante llamadas recursivas implícitas. Este analizador imita una derivación por la izquierda.

**Componentes**: buffer de entrada ($\omega\$$), pila de símbolos gramaticales (fondo $\$$, tope $S$ al inicio), tabla de análisis $M$, flujo de salida.

El analizador es controlado por el programa que considera $X$ (tope de la pila) y $a$ (símbolo de entrada actual).

> [!note] Algoritmo predictivo no recursivo
> ```
> Configuración inicial: pila = [S, $], entrada = ω$
> Sea a el primer símbolo de ω
> Mientras (X = tope de pila) ≠ $:
>   Si X == a (terminal):
>     Desapilar X,  consumir a
>   Si no, si X es terminal y X ≠ a:
>     error()
>   Si no, si M[X, a] es celda error:
>     error()
>   Si no  (M[X, a] = X → Y₁Y₂...Yₖ):
>     Emitir producción X → Y₁Y₂...Yₖ
>     Desapilar X
>     Apilar Yₖ, ..., Y₂, Y₁  (Y₁ queda en el tope)
> Si tope == $ y a == $: Aceptar
> ```

> [!example] Ejemplo
> Misma gramática anterior, traza para $\omega = id + id * id$:
>
> | # | Pila | Entrada | Acción |
> |---|---|---|---|
> | 1 | $E\$$ | $id+id*id\$$ | Emitir $E \to TE'$ |
> | 2 | $TE'\$$ | $id+id*id\$$ | Emitir $T \to FT'$ |
> | 3 | $FT'E'\$$ | $id+id*id\$$ | Emitir $F \to id$ |
> | 4 | $id\;T'E'\$$ | $id+id*id\$$ | Relacionar $id$ |
> | 5 | $T'E'\$$ | $+id*id\$$ | Emitir $T' \to \lambda$ |
> | 6 | $E'\$$ | $+id*id\$$ | Emitir $E' \to +TE'$ |
> | 7 | $+TE'\$$ | $+id*id\$$ | Relacionar $+$ |
> | 8 | $TE'\$$ | $id*id\$$ | Emitir $T \to FT'$ |
> | 9 | $FT'E'\$$ | $id*id\$$ | Emitir $F \to id$ |
> | 10 | $id\;T'E'\$$ | $id*id\$$ | Relacionar $id$ |
> | 11 | $T'E'\$$ | $*id\$$ | Emitir $T' \to *FT'$ |
> | 12 | $*FT'E'\$$ | $*id\$$ | Relacionar $*$ |
> | 13 | $FT'E'\$$ | $id\$$ | Emitir $F \to id$ |
> | 14 | $id\;T'E'\$$ | $id\$$ | Relacionar $id$ |
> | 15 | $T'E'\$$ | $\$$ | Emitir $T' \to \lambda$ |
> | 16 | $E'\$$ | $\$$ | Emitir $E' \to \lambda$ |
> | 17 | $\$$ | $\$$ | **Aceptar** |

---

## Análisis Sintáctico Ascendente

La forma más general del análisis ascendente es el **Análisis Sintáctico de Desplazamiento-Reducción**. El proceso consiste en **reducir** la cadena $\omega$ hasta el símbolo inicial $S$. La clase más extensa de gramáticas para la cual pueden construirse estos analizadores son las **gramáticas LR**.

### Análisis Sintáctico de Desplazamiento-Reducción

En cada paso de **reducción**, una subcadena $\beta$ que coincide con el cuerpo de alguna producción $A \to \beta$ se reemplaza por $A$. Es el proceso inverso de una derivación.

**Desplazamiento** — mover un símbolo de la cadena de entrada al tope de la pila de análisis.

> [!info] Definición — Pivote
> Dada $\alpha\rho\beta$ forma sentencial de la gramática, $\rho$ es **pivote** si y sólo si $\exists (N \to \rho) \in P \;\land\; S \Rightarrow^* \alpha N\beta \Rightarrow \alpha\rho\beta$.
>
> Cada vez que tenemos $\rho$ en el tope de la pila se puede hacer una reducción.

> [!info] Definición — Prefijos Viables
> Dada $\alpha\rho\beta$ forma sentencial derecha con $\rho$ pivote, $\gamma$ es **prefijo viable** si $\gamma$ es prefijo de $\alpha\rho$.
>
> **El contenido de la pila siempre es un prefijo viable.**

**Algoritmo D-R**:
- **Configuración inicial**: $\omega\$$ en el buffer; $\$$ en el fondo de la pila.
- **Pasos**: repetir — desplazar cero o más símbolos a la pila; reducir una cadena $\beta$ del tope por $A$ si $A \to \beta \in P$ — hasta que pila $= \$S$ y entrada $= \$$.

> [!example] Ejemplo
> $G$: $E \to E+T \mid T$, $T \to T*F \mid F$, $F \to (E) \mid id$. Traza para $id * id$:
>
> | Pila | Entrada | Acción |
> |---|---|---|
> | $\$$ | $id*id\$$ | Desplazar |
> | $\$\;id$ | $*id\$$ | Reducir $F \to id$ |
> | $\$F$ | $*id\$$ | Reducir $T \to F$ |
> | $\$T$ | $*id\$$ | Desplazar |
> | $\$T*$ | $id\$$ | Desplazar |
> | $\$T*id$ | $\$$ | Reducir $F \to id$ |
> | $\$T*F$ | $\$$ | Reducir $T \to T*F$ |
> | $\$T$ | $\$$ | Reducir $E \to T$ |
> | $\$E$ | $\$$ | **Aceptar** |
>
> Derivación inversa: $id*id \Rightarrow F*id \Rightarrow T*id \Rightarrow T*F \Rightarrow T \Rightarrow E$.
> Corresponde a la derivación por la derecha: $E \Rightarrow T \Rightarrow T*F \Rightarrow T*id \Rightarrow F*id \Rightarrow id*id$.

> [!warning] Importante
> No toda GLC admite análisis D-R sin ambigüedad. Hay configuraciones donde no se puede decidir:
> - **Conflicto D-R** (*Desplazamiento–Reducción*): no se sabe si desplazar o reducir.
> - **Conflicto R-R** (*Reducción–Reducción*): hay varias reducciones posibles para el mismo tope.

---

## Análisis Sintáctico LR

> [!info] Definición
> **LR(k)** = **L**eft-to-right scan · **R**ightmost derivation in reverse · **k** símbolos de anticipación.
>
> La clase de gramáticas LR es un superconjunto estricto de las LL. Por lo tanto, las gramáticas LR describen más lenguajes.

**Jerarquía de clases** (gramáticas no ambiguas):
$$\text{LL}(0) \subsetneq \text{LL}(1) \subsetneq \text{LL}(k) \subsetneq \text{LR}(k)$$
$$\text{LR}(0) \subsetneq \text{SLR}(1) \subsetneq \text{LALR}(1) \subsetneq \text{LR}(1) \subsetneq \text{LR}(k)$$

### Ítems y Autómata LR(0)

Un analizador LR realiza las decisiones de desplazamiento-reducción mediante un autómata que lleva registro de la posición en el análisis.

> [!info] Definición
> Un **ítem LR(0)** (o *elemento LR(0)*) es una producción con un punto $\bullet$ en alguna posición: $[A \to \alpha_1 \bullet \alpha_2]$.
>
> El punto indica cuánto del cuerpo ya se ha visto. La producción $A \to \lambda$ produce un único ítem $[A \to \bullet]$.

Por ejemplo, $A \to XYZ$ produce los cuatro ítems:
$$[A \to \bullet XYZ] \quad [A \to X \bullet YZ] \quad [A \to XY \bullet Z] \quad [A \to XYZ \bullet]$$

> [!info] Construcción — autómata LR(0)
> - **Estados**: conjuntos de ítems de la colección canónica LR(0).
> - **Transiciones**: función $\text{IR-A}(I, X)$ (*goto*).
> - **Estado inicial**: $\text{CLAUSURA}(\{[S' \to \bullet S]\})$, con $G'$ la gramática aumentada ($S' \to S$ nueva producción).

**Algoritmo CLAUSURA(I)**:
- $J \leftarrow I$.
- Repetir: para cada $[A \to \alpha \bullet B\beta] \in J$ y cada $B \to \gamma \in P$, agregar $[B \to \bullet\gamma]$ a $J$ si no está.
- Hasta punto fijo. Retornar $J$.

**Función IR-A$(I, X)$**:
$$\text{IR-A}(I, X) = \text{CLAUSURA}\bigl(\{[A \to \alpha X \bullet \beta] \mid [A \to \alpha \bullet X\beta] \in I\}\bigr)$$

**Colección canónica LR(0)**:
$$C = \{\text{CLAUSURA}(\{[S' \to \bullet S]\})\}$$
Repetir: para cada $I \in C$ y cada símbolo gramatical $X$, si $\text{IR-A}(I,X) \neq \emptyset$ y $\text{IR-A}(I,X) \notin C$, agregar $\text{IR-A}(I,X)$ a $C$. Hasta punto fijo.

> [!example] Ejemplo
> Para $G' = \langle \{E',E,T,F\},\{+,*,(,),id\},E',P\rangle$ con $E' \to E$, $E \to E+T \mid T$, $T \to T*F \mid F$, $F \to (E) \mid id$:
>
> $I_0 = \text{CLAUSURA}(\{[E' \to \bullet E]\})$ contiene 7 ítems: $[E' \to \bullet E]$, $[E \to \bullet E+T]$, $[E \to \bullet T]$, $[T \to \bullet T*F]$, $[T \to \bullet F]$, $[F \to \bullet(E)]$, $[F \to \bullet id]$.
>
> La colección canónica completa tiene 12 estados ($I_0$ a $I_{11}$), con transiciones $\text{IR-A}(I_0, E) = I_1$, $\text{IR-A}(I_0, T) = I_2$, $\text{IR-A}(I_0, id) = I_5$, etc.

### Algoritmo de Análisis Sintáctico LR

Un analizador LR consta de: entrada, salida, **pila de estados**, programa controlador y **tabla** con **ACCIÓN** e **IR-A**.

**Función ACCIÓN**$[i, a]$ ($i$ = estado, $a$ = terminal o $\$$):
- **(a) Shift** $j$: desplazar $a$ a la pila usando el estado $j$.
- **(b) Reduce** $A \to \beta$: sacar $|\beta|$ estados de la pila; sea $t$ el nuevo tope, apilar $\text{IR-A}[t, A]$.
- **(c) Accept**: aceptar.
- **(d) Error**.

> [!note] Algoritmo LR
> ```
> Configuración inicial: pila = [s₀],  entrada = ω$
> Mientras(1):
>   Sea s = tope de la pila,  a = primer símbolo de entrada
>   Según ACCIÓN[s, a]:
>     Caso desplazar t:
>       Meter t en la pila;  consumir a
>     Caso reducir A → β:
>       Sacar |β| estados de la pila
>       Sea t = nuevo tope;  apilar IR-A[t, A]
>       Emitir producción A → β
>     Caso aceptar:
>       break
>     Caso error:
>       Manejar error
> ```

> [!warning] Importante
> Todos los analizadores LR se comportan de esta manera; la única diferencia entre uno y otro es la información en los campos ACCIÓN e IR-A de la tabla.

### Tabla SLR(1)

**Algoritmo de construcción de la tabla SLR(1)** para $G$.
**Entrada**: gramática aumentada $G'$.
**Salida**: funciones ACCIÓN e IR-A para $G'$.
**Pasos**:
1. Construir $C = \{I_0, \dots, I_n\}$, la colección canónica LR(0) de $G'$.
2. Para cada estado $i$ (construido a partir de $I_i$):
   - **(a)** Si $a \in \Sigma$, $[A \to \alpha \bullet a\beta] \in I_i$ y $\text{IR-A}(I_i, a) = I_j$: $\text{ACCIÓN}[i, a] = \text{desplazar } j$.
   - **(b)** Si $[A \to \alpha\bullet] \in I_i$ y $A \neq S'$: $\text{ACCIÓN}[i, a] = \text{reducir } A \to \alpha$, $\forall a \in \text{SIGUIENTE}(A)$.
   - **(c)** Si $[S' \to S\bullet] \in I_i$: $\text{ACCIÓN}[i, \$] = \text{aceptar}$.
3. $\forall A \in N$: si $\text{IR-A}(I_i, A) = I_j$, entonces $\text{IR-A}[i, A] = j$.
4. Entradas no definidas: **error**.
5. Estado inicial: el construido a partir del conjunto que contiene $[S' \to \bullet S]$.

Si resulta alguna acción conflictiva, $G$ no es SLR(1) y no se produce el analizador.

> [!warning] Importante
> - Toda gramática SLR(1) es **no ambigua**.
> - Pero **no toda gramática no ambigua es SLR(1)**.

> [!example] Ejemplo
> Numerando las producciones: $0.\;E' \to E$, $1.\;E \to E+T$, $2.\;E \to T$, $3.\;T \to T*F$, $4.\;T \to F$, $5.\;F \to (E)$, $6.\;F \to id$.
>
> Traza para $id * id + id$:
>
> | # | Pila | Símbolos | Entrada | Acción |
> |---|---|---|---|---|
> | 1 | 0 | | $id*id+id\$$ | $s5$ |
> | 2 | 0 5 | $id$ | $*id+id\$$ | $r_6: F \to id$ |
> | 3 | 0 3 | $F$ | $*id+id\$$ | $r_4: T \to F$ |
> | 4 | 0 2 | $T$ | $*id+id\$$ | $s7$ |
> | 5 | 0 2 7 | $T*$ | $id+id\$$ | $s5$ |
> | 6 | 0 2 7 5 | $T*id$ | $+id\$$ | $r_6: F \to id$ |
> | 7 | 0 2 7 10 | $T*F$ | $+id\$$ | $r_3: T \to T*F$ |
> | 8 | 0 2 | $T$ | $+id\$$ | $r_2: E \to T$ |
> | 9 | 0 1 | $E$ | $+id\$$ | $s6$ |
> | 10 | 0 1 6 | $E+$ | $id\$$ | $s5$ |
> | 11 | 0 1 6 5 | $E+id$ | $\$$ | $r_6: F \to id$ |
> | 12 | 0 1 6 3 | $E+F$ | $\$$ | $r_4: T \to F$ |
> | 13 | 0 1 6 9 | $E+T$ | $\$$ | $r_1: E \to E+T$ |
> | 14 | 0 1 | $E$ | $\$$ | **Aceptar** |

---

## Analizadores Sintácticos más Potentes

Hay dos métodos más potentes que SLR(1):
1. **LR Canónico** (o LR directamente) — usa ítems LR(1).
2. **LALR** (*Look Ahead LR*) — menos estados que LR canónico; maneja más gramáticas que SLR con tablas razonables.

### LR Canónico — Ítems LR(1)

> [!info] Definición
> Un **ítem LR(1)** tiene la forma $[A \to \alpha \bullet \beta,\; t]$, donde $A \to \alpha\beta$ es una producción y $t$ es un terminal o $\$$ (símbolo de **anticipación**).
>
> El terminal $t$, de longitud 1, da nombre al método: LR(1).

**CLAUSURA(I)** para ítems LR(1): para cada $[A \to \alpha \bullet B\beta,\; t] \in J$, cada $B \to \gamma \in G'$ y cada $b \in \text{PRIMERO}(\beta t)$: agregar $[B \to \bullet\gamma,\; b]$ a $J$.

**IR-A$(I, X)$**: igual que LR(0) pero propagando el componente $t$.

**Algoritmo de construcción de tabla LR(1)** — igual que SLR, pero la regla (b) se restringe: si $[A \to \alpha\bullet,\; a] \in I_i$ y $A \neq S'$, entonces $\text{ACCIÓN}[i, a] = \text{reducir } A \to \alpha$ sólo para ese $a$ específico (no para todo $\text{SIGUIENTE}(A)$).

### LALR — Look-Ahead LR

> [!info] Definición
> El **núcleo** (*core*) de un ítem LR(1) $[A \to \alpha \bullet \beta,\; t]$ es su primer componente $A \to \alpha\beta$.
>
> Si se tienen más de un ítem con el mismo núcleo, pueden unirse en un solo conjunto de elementos.

**Algoritmo de construcción de la tabla LALR**:
1. Construir $C = \{I_0, \dots, I_n\}$, la colección de conjuntos de ítems LR(1) para $G'$.
2. Para cada núcleo presente en $C$, reemplazar todos los conjuntos con ese mismo núcleo por su unión.
3. Sea $C' = \{J_0, \dots, J_m\}$ la colección resultante. Construir ACCIÓN e IR-A a partir de $C'$ como en LR(1).
4. Si surge alguna acción conflictiva, $G$ **no es LALR(1)**.

> [!warning] Importante
> LALR tiene **menos estados** que LR canónico, permitiendo manejar más gramáticas que SLR con tablas de tamaño razonable. Sin embargo, si dos conjuntos con igual núcleo generan conflicto al fusionarse, la gramática no es LALR(1).

> [!example] Ejemplo
> Sea $G' = \{S' \to S,\; S \to CC,\; C \to cC \mid d\}$.
>
> La colección canónica LR(1) tiene 10 estados. Los estados $I_3$ ($C \to d\bullet$, lookahead $c/d$) e $I_7$ ($C \to d\bullet$, lookahead $\$$) tienen el mismo núcleo y se fusionan en $I_{47}$ con lookahead $c/d/\$$. Similarmente $I_6$ e $I_9$ se fusionan en $I_{89}$, e $I_3$ e $I_6$ de la rama $c$ se fusionan en $I_{36}$.
>
> La tabla LALR resultante tiene **7 estados** (0, 1, 2, 36, 47, 5, 89) en lugar de los 10 del LR canónico.

---

## Generador de Analizadores Sintácticos — Yacc

Un **generador de analizadores sintácticos** es un programa que, dada la especificación de una gramática, produce otro programa capaz de analizar y traducir un archivo correspondiente a esa gramática.

**Yacc** ("*Yet Another Compiler-Compiler*") es el generador más conocido y usa el método **LALR**.

> [!note] Pipeline de Yacc
> ```
> traducir.y   ──►  [Compilador Yacc]  ──►  y.tab.c
> y.tab.c      ──►  [Compilador C]     ──►  a.out
> entrada      ──►  [a.out]            ──►  salida traducida
> ```
>
> - `traducir.y` contiene la especificación de la gramática del lenguaje $L$.
> - `y.tab.c` es la representación en C del analizador LALR generado.
> - `a.out` es un compilador/traductor de entradas escritas en el lenguaje $L$.

---

## Preguntas

-
