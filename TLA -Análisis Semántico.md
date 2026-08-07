---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-05-27"
Materia: "[[TLA.base|TLA]]"
temas:
  - Análisis Semántico
  - Sintáctico vs Semántico
  - Definición Dirigida por la Sintaxis (DDS)
  - Atributos sintetizados y heredados
  - DDS S-atribuida
  - DDS L-atribuida
  - Árbol decorado/anotado
  - Grafo de dependencias
  - Orden topológico
  - Esquemas de Traducción Dirigidos por la Sintaxis (ETDS)
  - Esquemas de traducción postfijos
  - ETDS para DDS L-atribuidas
  - Implementación de DDS L-atribuidas
---
# TLA - Análisis Semántico (Resumen)

Resumen de la clase 11 — *Autómatas, Teoría de Lenguajes y Compiladores* (Lic. Ana María Arias Roig).

---

## Motivación

Las clases anteriores construyeron el **análisis sintáctico**: métodos LL y LR que, dado un flujo de tokens, verifican la estructura gramatical y construyen el árbol de derivación. El **análisis semántico** es el siguiente paso del front-end de un compilador: a partir del árbol sintáctico, calcula el *significado* de la cadena (traducción a código intermedio, inferencia de tipos, evaluación de expresiones, etc.). El mecanismo formal es la **Definición Dirigida por la Sintaxis (DDS)** y su variante operacional, los **Esquemas de Traducción (ETDS)**.

---

## Sintáctico vs Semántico

> [!example] Ejemplo motivador
> Sea $G = \langle \{L,E,T,F\}, \{+,*,(,),\textbf{digit}\}, L, P \rangle$ con producciones:
> $$L \to E \qquad E \to E+T \mid T \qquad T \to T*F \mid F \qquad F \to (E) \mid \textbf{digit}$$
>
> Para la cadena $3 * 5 + 4$:
> - Análisis **sintáctico**: la cadena es válida ✓
> - **Traducción** al valor: $19$ ✓
> - **Traducción** postfija: $3\;5\;4\;{*}\;{+}$ ✓
> - Traducción a binario: error — $3$ no es un número binario ✗

---

## Definición Dirigida por la Sintaxis (DDS)

> [!info] Definición
> Una **DDS** es una gramática libre de contexto que asocia:
> 1. A cada símbolo de la gramática, un conjunto de **atributos**.
> 2. A cada producción, un conjunto de **reglas semánticas** para calcular los valores de los atributos de los símbolos que aparecen en la producción.
>
> Un árbol de análisis sintáctico que muestra los valores de los atributos se llama **árbol anotado** o **árbol decorado**.

---

## Atributos

Un atributo puede ser un valor numérico, de cadena, una tabla de referencia, un objeto. Si $X$ es un símbolo y $a$ es uno de sus atributos, se escribe $X.a$.

- Para los **no terminales**: existen atributos **sintetizados** y **heredados**.
- Para los **terminales**: sólo pueden ser **atributos sintetizados** (su valor lo provee el analizador léxico).

### Atributos Sintetizados

> [!info] Definición
> Dada $A \to \alpha$, y sea $A.a = f(\alpha_1.y_1, \alpha_2.y_2, \dots, \alpha_n.y_n)$ una regla semántica para calcular el atributo $a$. $A.a$ es un **atributo sintetizado**, ya que depende del valor de los atributos de los símbolos que están a la derecha de la producción (hijos de $A$ en el árbol sintáctico).

> [!example] Ejemplo — gramática de expresiones con atributos sintetizados
> $G = \langle \{L,E,T,F\}, \{+,*,(,),\textbf{digit}\}, L, P \rangle$
>
> | Producción | Regla semántica |
> |---|---|
> | 1) $L \to E\;\mathbf{n}$ | $L.val = E.val$ |
> | 2) $E \to E_1 + T$ | $E.val = E_1.val + T.val$ |
> | 3) $E \to T$ | $E.val = T.val$ |
> | 4) $T \to T_1 * F$ | $T.val = T_1.val \times F.val$ |
> | 5) $T \to F$ | $T.val = F.val$ |
> | 6) $F \to (E)$ | $F.val = E.val$ |
> | 7) $F \to \textbf{digit}$ | $F.val = \textbf{digit}.lexval$ |

### DDS S-atribuida

> [!info] Definición
> Una DDS donde **todos** los atributos son sintetizados se denomina **S-Atribuida**.
>
> Si además no tiene efectos globales, se denomina **Gramática con Atributos**.
>
> Efectos globales: actualización de una tabla, modificación de una variable global, impresión de algún resultado como salida.

> [!example] Implementación en Yacc
> La DDS S-atribuida de expresiones se implementa en Yacc declarando las reglas semánticas como acciones:
> ```yacc
> expr : expr '+' term   { $$ = $1 + $3; }
>      | term
>      ;
> term : term '*' factor { $$ = $1 * $3; }
>      | factor
>      ;
> factor : '(' expr ')'  { $$ = $2; }
>        | DIGIT
>        ;
> ```
> El programa Yacc imprime $E.val$ (efecto global).

### Árbol Decorado — evaluación ascendente

Para la cadena $3 * 5 + 4\;\mathbf{n}$, el árbol decorado tiene en la raíz $L.val = 19$. Como todos los atributos son sintetizados, la evaluación es **ascendente** (se evalúan los hijos de un nodo antes que él mismo).

---

## Atributos Heredados

> [!info] Definición
> Dada $A \to \alpha$, y sea $\alpha_i.a = f(\alpha_1.y_1, \dots, \alpha_n.y_n, A.b)$ una regla semántica. $\alpha_i.a$ es un **atributo heredado**, ya que depende del valor de atributos de los símbolos en la producción (hermanos o padre en el árbol sintáctico).

> [!example] Ejemplo — gramática con atributos heredados
> $G = \langle \{T, T', F\}, \{*, \textbf{digit}\}, T, P \rangle$
>
> | Producción | Regla semántica |
> |---|---|
> | 1) $T \to FT'$ | $T'.inh = F.val$ |
> |  | $T.val = T'.syn$ |
> | 2) $T' \to *\,FT'_1$ | $T'_1.inh = T'.inh \times F.val$ |
> |  | $T'.syn = T'_1.syn$ |
> | 3) $T' \to \lambda$ | $T'.syn = T'.inh$ |
> | 4) $F \to \textbf{digit}$ | $F.val = \textbf{digit}.lexval$ |
>
> - Los no terminales $T$ y $F$ tienen un atributo sintetizado $val$.
> - El no terminal $T'$ tiene dos atributos: uno sintetizado ($syn$) y otro heredado ($inh$).
> - El terminal $\textbf{digit}$ tiene un atributo sintetizado $lexval$, retornado por el analizador léxico.

Para la cadena $3 * 5$, el árbol decorado propaga $F.val = 3$ como $T'.inh = 3$, luego $T'_1.inh = 3 \times 5 = 15$, y finalmente $T.val = T'.syn = 15$.

---

## Orden de Evaluación de una DDS

### Grafo de Dependencias

> [!info] Definición
> Un **grafo de dependencias** permite determinar el orden de evaluación de los atributos para un árbol sintáctico. Un arco de un atributo $M$ a otro $N$ significa que el valor de $M$ es necesario para calcular $N$.

**Construcción del grafo:**

> [!note] Algoritmo — Grafo de Dependencias
> **Nodos:** para cada nodo $X$ en el árbol sintáctico, para cada atributo $a$ de $X$: construir un nodo con etiqueta $X.a$.
>
> **Arcos:** para cada nodo $X$ en el árbol sintáctico, para cada regla semántica $v := f(v_1, v_2, \dots, v_k)$ asociada con la producción que dio lugar a $X$ y sus hijos: para $i$ desde 1 hasta $k$: construir un arco dirigido desde $v_i$ hasta $v$.

### Orden Topológico

> [!info] Definición
> Si un grafo de dependencias tiene un arco del nodo $M$ al nodo $N$, entonces los atributos en $M$ deben evaluarse **antes** que los atributos en $N$.
>
> La secuencia de evaluación de los nodos es $N_1, N_2, \dots N_k$, si $\forall i < j$, hay un arco de $N_i$ a $N_j$.

> [!warning] Importante
> Si hubiera ciclos no hay orden topológico del grafo y por lo tanto tampoco hay forma de evaluar la DDS para ese árbol.

---

## Definiciones S-Atribuidas

> [!info]
> Una DDS es **S-atribuida** si todos los atributos son sintetizados.

Se pueden evaluar todos los atributos de abajo hacia arriba. Lo más simple es evaluar en un recorrido **post-orden** (izq-der-raíz) del árbol sintáctico:

> [!note] Algoritmo Postorder
> ```
> Postorder(N):
>   Para cada hijo C de N, desde la izquierda:
>     Postorder(C)
>   Evaluar atributos asociados a N.
> ```

Como este orden es el mismo que usa un analizador LR para hacer las reducciones, al evaluar atributos sintetizados los puede almacenar en la pila mientras efectúa el análisis sintáctico, sin tener que crear otros nodos explícitamente.

---

## Definiciones L-Atribuidas

> [!info] Definición
> Una DDS es **L-atribuida** si los atributos son:
> 1. Sintetizados, o
> 2. Heredados, pero con reglas semánticas **limitadas** (para que los arcos en el grafo vayan de izquierda a derecha — *Left to right*).
>
> Si hay una producción $A \to X_1 X_2 \cdots X_n$, y un atributo $X_i.a$ se calcula por una regla semántica asociada a esta producción, entonces la regla semántica puede usar:
> - **(a)** Atributos heredados asociados a $A$.
> - **(b)** Atributos heredados o sintetizados asociados con los símbolos $X_1, X_2, \dots, X_{i-1}$ situados a la izquierda de $X_i$.
> - **(c)** Atributos heredados o sintetizados asociados a $X_i$, pero sólo de forma tal que no se formen ciclos en el grafo de dependencia.

> [!example] Ejemplo — no es L-atribuida
> Sea la producción $A \to BC$ con reglas:
> - $A.s = B.b$ → $A.s$ es sintetizado (depende de hijo $B$): ✓
> - $B.b = f(C.c, A.s)$ → $B.b$ depende de $C$, que está a la **derecha** de $B$ en $A \to BC$: no cumple (b); además forma ciclo: no cumple (c).
>
> **¡No es L-atribuida!**

---

## Esquemas de Traducción Dirigidos por la Sintaxis (ETDS)

> [!info] Definición
> Un **ETDS** es una gramática libre de contexto con fragmentos de programa en los cuerpos de las producciones: **acciones semánticas** que pueden aparecer en cualquier posición dentro del cuerpo de la producción.
>
> Por convención, las acciones semánticas se escriben entre llaves.

### Esquemas de Traducción Postfijos

Cuando la DDS es **S-atribuida**, se puede construir un ETDS en el cual cada acción se ubica al **final** de la producción y se ejecuta cuando se hace una **reducción** en el análisis sintáctico LR.

Pueden implementarse durante el análisis LR ejecutando las acciones en el momento de la reducción. Para eso, los atributos de cada símbolo se ponen también en la pila.

> [!example] Ejemplo — ETDS postfijo para gramática de expresiones
>
> | Producción | Acción semántica |
> |---|---|
> | $L \to E\,\mathbf{n}$ | $\{print(E.val);\}$ |
> | $E \to E_1 + T$ | $\{E.val = E_1.val + T.val;\}$ |
> | $E \to T$ | $\{E.val = T.val;\}$ |
> | $T \to T_1 * F$ | $\{T.val = T_1.val \times F.val;\}$ |
> | $T \to F$ | $\{T.val = F.val;\}$ |
> | $F \to (E)$ | $\{F.val = E.val;\}$ |
> | $F \to \textbf{digit}$ | $\{F.val = \textbf{digito}.lexval;\}$ |

### Esquemas de Traducción para DDS L-Atribuidas

Cuando la DDS es **L-atribuida**, se puede construir un ETDS a partir de las siguientes reglas:

1. **Colocar la acción que calcula los atributos heredados** para un no terminal $A$ inmediatamente **antes** de la ocurrencia de $A$ en el cuerpo de la producción. Si varios atributos heredados para $A$ dependen unos de otros en forma acíclica, ordenar la evaluación para que los que se necesiten primero se calculen antes.
2. **Ubicar las acciones que calculan un atributo sintetizado** para la parte izquierda de una producción, al **final** del cuerpo de esa producción.

> [!example] Ejemplo tipo TeX — cajas de texto (*Boxes*)
> Lenguaje donde `a sub i sub j` representa $a_{ij}$. Gramática:
> $$G = \langle \{B\}, \{\text{sub},(,),\text{text}\}, B, P \rangle \qquad B \to BB \mid B\,\text{sub}\,B \mid (B) \mid \text{text}$$
>
> **Atributos:**
> - $B.ps$: *point size* — tamaño del cuerpo de la caja.
> - $B.ht$: *height* — distancia desde la parte superior hasta la línea de base.
> - $B.dp$: *depth* — distancia desde la línea de base hasta la parte inferior.
>
> **DDS:**
>
> | Producción | Regla semántica |
> |---|---|
> | $1.\; S \to B$ | $B.ps = 10$ |
> | $2.\; B \to B_1 B_2$ | $B_1.ps = B.ps$ |
> |  | $B_2.ps = B.ps$ |
> |  | $B.ht = \max(B_1.ht, B_2.ht)$ |
> |  | $B.dp = \max(B_1.dp, B_2.dp)$ |
> | $3.\; B \to B_1\,\text{sub}\,B_2$ | $B_1.ps = B.ps$ |
> |  | $B_2.ps = 0.7 \times B.ps$ |
> |  | $B.ht = \max(B_1.ht,\; B_2.ht - 0.25 \times B.ps)$ |
> |  | $B.dp = \max(B_1.dp,\; B_2.dp + 0.25 \times B.ps)$ |
> | $4.\; B \to (B_1)$ | $B_1.ps = B.ps$ |
> |  | $B.ht = B_1.ht$ |
> |  | $B.dp = B_1.dp$ |
> | $5.\; B \to \text{text}$ | $B.ht = \text{getHt}(B.ps, \text{text}.lexval)$ |
> |  | $B.dp = \text{getDp}(B.ps, \text{text}.lexval)$ |
>
> **ETDS correspondiente:**
>
> ```
> 1. S → { B.ps = 10; }
>         B
>
> 2. B → { B₁.ps = B.ps; }
>         B₁
>         { B₂.ps = B.ps }
>         B₂
>         { B.ht = MAX(B₁.ht, B₂.ht);
>           B.dp = MAX(B₁.dp, B₂.dp); }
>
> 3. B → { B₁.ps = B.ps; }
>         B₁ sub { B₂.ps = 0.7 × B.ps }
>         B₂
>         { B.ht = MAX(B₁.ht, B₂.ht − 0.25 × B.ps);
>           B.dp = MAX(B₁.dp, B₂.dp + 0.25 × B.ps); }
>
> 4. B → ( { B₁.ps = B.ps; }
>           B₁
>           { B.ht = B₁.ht; B.dp = B₁.dp; }
>           )
>
> 5. B → text
>         { B.ht = getHt(B.ps, text.lexval);
>           B.dp = getDp(B.ps, text.lexval); }
> ```

---

## Implementación de DDS L-Atribuidas

Hay tres métodos principales:

1. **Construir el árbol sintáctico y completarlo con las anotaciones** (árbol decorado). Si se presentan circuitos, no se podrá resolver.

2. **Construir el árbol sintáctico, agregar acciones, y ejecutar las acciones en preorden** (raíz-izq-der). Sirve para cualquier definición L-atribuida:

> [!note] Algoritmo Preorder (L-atribuida)
> ```
> Visitar(b):
>   Para cada hijo a de b:
>     Evaluar los atributos heredados de a (de izquierda a derecha)
>     Visitar(a)
>   Evaluar los atributos sintetizados de b.
> ```

3. **Usar un analizador descendente recursivo.** La función para el no terminal $A$ recibe los atributos heredados de $A$ como argumentos, y retorna los atributos sintetizados de $A$.

---

## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -Análisis Ascendente]] — tema anterior
- [[TLA -Máquina de Turing]] — tema siguiente
- [[frontend]] — la fase que sigue al parser

**Otras materias**

- **EDA**  [[EDA - Grafos]] — grafo de dependencias y orden topológico
- **EDA**  [[EDA - Árboles]] — árbol decorado

<!-- notas-relacionadas:fin -->
