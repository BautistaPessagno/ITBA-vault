---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-07-10"
Materia: "[[TLA.base|TLA]]"
temas:
  - Mega resumen final
  - Definiciones recursivas
  - Demostraciones
  - Lema de bombeo
  - Autómatas de pila
  - Análisis sintáctico
  - Máquinas de Turing
  - Gramáticas con atributos
---

# TLA — Mega Resumen para el Final

> [!important] Cómo leer esta nota
> Está armada **desde los finales** (2C-2022, 1C-2023, 2C-2023, 1C-2024 x2, 2C-2024 + fotos de finales resueltos). Cubre lo que **realmente se toma**, con las **definiciones textuales** y las **demostraciones completas** del Ejercicio 2. Para la estrategia de estudio con poco tiempo mirá [[TLA -Guía Repaso Final (poco tiempo)]].

## 0. Estructura fija del final

El final son **7 ejercicios**, se aprueba con **5 / 10**, y es **~85% teoría**. La estructura casi no cambia entre fechas:

| Ej | Pts | Tema | Consigna |
|----|-----|------|----------|
| 1 | 1,5 | Introducción / AF | Explicar o **definir recursivamente** |
| 2 | 1,5 | AF / AP | **Demostrar** un teorema |
| 3 | 2,0 | Reg / GLC / Análisis sintáctico | **Multiple choice** (4 ítems) |
| 4 | 1,5 | ER / Reg / AF | **V/F** con justificación |
| 5 | 1,0 | AP / MT | **Práctico**: corregir/completar transiciones |
| 6 | 1,0 | Gramáticas con atributos / Intro | Explicar o definir |
| 7 | 1,5 | MT / Gramáticas con atributos | **Relacionar 3 conceptos** en una frase |

Se evalúa **lo que está escrito** (no "lo que se quiso poner") y usando el **vocabulario y los algoritmos de la teórica**. Las justificaciones puntúan.

---

## 1. Introducción y gramáticas

### 1.1 Definiciones básicas
- **Alfabeto** $\Sigma$: conjunto finito no vacío de símbolos. **Cadena**: secuencia finita de símbolos de $\Sigma$. **$\lambda$**: cadena vacía ($|\lambda|=0$).
- **Potencia**: $\Sigma^k=\{\omega : |\omega|=k\}$. **Kleene**: $\Sigma^*=\bigcup_{i\ge0}\Sigma^i$. **Positiva**: $\Sigma^+=\bigcup_{i\ge1}\Sigma^i$ (sin $\lambda$).
- **Lenguaje**: $L\subseteq\Sigma^*$.

### 1.2 Definiciones recursivas (caen literal en Ej 1 y 6)
Toda definición recursiva tiene **caso base** + **paso inductivo**.

> [!note] Forma sentencial
> - **Base:** $S$ es una forma sentencial (FS).
> - **Paso inductivo:** si $\alpha\beta\gamma$ es una FS y $\beta\to\delta\in P$, entonces $\alpha\delta\gamma$ es una FS.
>
> Una FS con sólo terminales (o $\lambda$) es una **sentencia**. $L(G)=\{\omega\in\Sigma^* : S\overset{*}{\Rightarrow}\omega\}$.

### 1.3 Inducción estructural
Método de prueba para estructuras definidas recursivamente:
1. **Base:** se prueba la propiedad $S(X)$ para la(s) estructura(s) base.
2. **Paso inductivo:** para una estructura $X$ formada a partir de subestructuras $Y_1,\dots,Y_k$, se asumen $S(Y_1),\dots,S(Y_k)$ (hipótesis inductiva) y con eso se prueba $S(X)$.

### 1.4 Gramática y jerarquía de Chomsky
Gramática = 4-tupla $G=(N,\Sigma,P,S)$: no terminales $N$, terminales $\Sigma$ (con $N\cap\Sigma=\emptyset$), producciones $P$ ($\alpha\to\beta$), símbolo inicial $S$.

| Tipo | Nombre | Producciones | Máquina |
|------|--------|--------------|---------|
| **0** | Sin restricción / Recursivamente Enumerable | $\alpha\to\beta$, $\alpha\in(N\cup\Sigma)^+$ | **Máquina de Turing** |
| **1** | Sensible al contexto | $|\alpha|\le|\beta|$ (salvo $S\to\lambda$) | **Autómata Linealmente Acotado (AAL)** |
| **2** | Libre de contexto | $A\to\alpha$ (un no terminal a izquierda) | **Autómata de Pila (AP)** |
| **3** | Regular | $A\to bC \mid b \mid \lambda$ (lineal a der. o izq.) | **Autómata Finito (AF)** |

Inclusión: $L_3\subset L_2\subset L_1\subset L_0$. Además hay lenguajes **no enumerables** que ninguna gramática genera.

---

## 2. Autómatas finitos

### 2.1 AFD y función de transición extendida $\hat{\delta}$
**AFD** $=\langle Q,\Sigma,\delta,q_0,F\rangle$ con $\delta:Q\times\Sigma\to Q$.

> [!note] $\hat{\delta}$ para AFD (Ej 1 típico) — $\hat{\delta}:Q\times\Sigma^*\to Q$
> - **Base:** $\hat{\delta}(p,\lambda)=p$.
> - **Paso inductivo:** si $\omega=\omega' a$, entonces $\hat{\delta}(p,\omega)=\delta(\hat{\delta}(p,\omega'),a)$.
>
> $L(A)=\{\omega\in\Sigma^* : \hat{\delta}(q_0,\omega)\in F\}$.

### 2.2 AFND y AFND-$\lambda$
- **AFND**: $\delta:Q\times\Sigma\to \mathcal{P}(Q)$. Acepta: $L=\{\omega : \hat{\delta}(q_0,\omega)\cap F\neq\emptyset\}$.
- **AFND-$\lambda$**: además permite transiciones con $\lambda$.

> [!note] Clausura-$\lambda$ de un estado (Ej 1/6 típico)
> $\text{claus}_\lambda(q)$ = estados alcanzables desde $q$ por caminos de arcos etiquetados con $\lambda$.
> - **Base:** $q\in\text{claus}_\lambda(q)$.
> - **Paso inductivo:** si $p\in\text{claus}_\lambda(q)$ y $r\in\delta(p,\lambda)$, entonces $r\in\text{claus}_\lambda(q)$.

**Equivalencia:** AFD $\equiv$ AFND $\equiv$ AFND-$\lambda$ (todos reconocen exactamente los lenguajes regulares). Las construcciones (subconjuntos, eliminación de $\lambda$) preservan el lenguaje.

### 2.3 Indistinguibilidad y minimización
- **Distinguibles:** $p,q$ son **distinguibles** si $\exists\,\omega\in\Sigma^*$ tal que $\hat{\delta}(p,\omega)\in F \land \hat{\delta}(q,\omega)\notin F$ (o viceversa). Si no existe tal $\omega$, son **indistinguibles**.
- **Indistinguibilidad de orden $k{+}1$:** $p\,E_{k+1}\,q \iff \big(\forall a\in\Sigma:\ \delta(p,a)\,E_k\,\delta(q,a)\big)$ (y $p\,E_0\,q$ sii ambos están o ambos no están en $F$).
- **Minimización (conjunto cociente):** se agrupan los estados en clases de equivalencia de indistinguibilidad; el AFD mínimo $A'$ tiene **un estado por clase**. El estado trampa/vacío **sí** tiene su clase. → demo completa en §8.4.

→ [[TLA -Autómatas Finitos Determinísticos]], [[TLA -Autómatas Finitos No Determinísticos]]

---

## 3. Expresiones y lenguajes regulares

### 3.1 Identidades de ER (caen en Ej 4 V/F)
- $\emptyset^*=\{\lambda\}$ y $\lambda^*=\{\lambda\}$. **Ojo:** $\emptyset^*\neq\emptyset$.
- $\alpha+\emptyset=\alpha$ (unión con vacío) pero $\alpha\cdot\emptyset=\emptyset$ (producto con vacío).
- $\alpha+\alpha=\alpha$; $(\alpha^*)^*=\alpha^*$.

> [!note] Lema de Arden
> $X=\alpha X+\beta \iff X=\alpha^*\beta$, **siempre que $\lambda\notin L(\alpha)$**.
>
> *Ejemplo de examen:* $E=a+bE+aE+b=(a+b)+(a+b)E \Rightarrow E=(a+b)^*(a+b)$. **(Verdadero.)**

### 3.2 Propiedades de cierre
Los regulares son **cerrados** bajo unión, concatenación, estrella, **intersección**, **complemento** y diferencia. (Por eso "la intersección de dos regulares no es regular" es **falso**.)

### 3.3 Lema de bombeo para regulares
**Enunciado:** si $L$ es regular, $\exists N$ tal que $\forall\omega\in L$ con $|\omega|\ge N$, se puede escribir $\omega=xyz$ con $|xy|\le N$, $|y|\ge1$ y $\forall i\ge0:\ xy^iz\in L$.

> [!tip] Cómo se demuestra (clave del multiple choice)
> Se usa un **AFD con $N$ estados** ($N$ = cantidad de estados = constante del lema) y una **cadena $\omega\in L$ con $|\omega|\ge N$**. Al leer $\omega$ se visitan $\ge N{+}1$ estados, así que **por palomar (pigeonhole) un estado se repite** dentro de los primeros $N$ pasos → aparece un ciclo, que es la parte $y$ que se puede "bombear".

→ [[TLA -Lenguajes Regulares]], [[TLA -Expresiones Regulares]]

---

## 4. GLC y autómatas de pila

### 4.1 Gramáticas libres de contexto
- **Ambigüedad:** $G$ es **ambigua** si **existe alguna** cadena $\alpha\in L(G)$ con **dos árboles de derivación distintos** (equivalente: dos derivaciones más a la izquierda distintas desde $S$). **Ojo:** es "existe alguna", no "para toda".
- **Forma Normal de Chomsky (FNC):** producciones $A\to BC$ o $A\to t$ (con $B,C\in N$, $t\in\Sigma$), **sin $\lambda$**. Genera $L-\{\lambda\}$.
- **Forma Normal de Greibach (FNG):** producciones $A\to t\beta$ con $t\in\Sigma$, $\beta\in N^*$.

### 4.2 Autómatas de pila
**AP** $=\langle Q,\Sigma,\Gamma,\delta,q_0,S(,F)\rangle$. Dos criterios de aceptación **equivalentes**:
- **Por estado final** ($L_{pf}$): termina en un estado de $F$ con $\omega$ consumida.
- **Por pila vacía** ($L_{pv}$): vacía la pila con $\omega$ consumida.

$L_{pf}(P_f)$ y $L_{pv}(P_v)$ reconocen la misma clase (los LLC) → demos de conversión en §8.5.

- **AP determinista $\subsetneq$ AP no determinista.** Hay LLC generados por gramáticas **no ambiguas** que **ningún AP determinista** reconoce (lenguajes *inherentemente no deterministas*), por ejemplo $L=\{\omega\alpha^R\}$ del tipo $\{\omega\omega^R\}$.
- **GLC en cualquier forma normal $\Rightarrow$ existe un AP** que reconoce el mismo lenguaje que genera la gramática.

### 4.3 Lema de bombeo para LLC
**Enunciado:** si $L$ es LLC, $\exists P$ tal que si $\alpha\in L$ y $|\alpha|\ge P$, entonces $\alpha=rxyzs$ con:
$$|xyz|\le P \ \land\ |xz|\ge1 \ \land\ \forall i\ge0:\ r\,x^i\,y\,z^i\,s\in L$$

> [!warning] Distractores típicos del multiple choice
> Es $|xz|\ge1$ (no $|xyz|\ge1$) y $\forall i\ge0$ (no $i>1$ ni $i>0$). Se demuestra con una **gramática en Forma Normal de Chomsky que genere $L(G)=L-\{\lambda\}$** (por altura del árbol se repite un no terminal en un camino).

→ [[TLA -Autómatas de Pila]], [[TLA -Formas Normales y Lema de Bombeo CFL]]

---

## 5. Análisis sintáctico

### 5.1 Descendente — LL(1)
- **Primeros($\alpha$):** $\{t\in\Sigma : \alpha\overset{*}{\Rightarrow}t\beta\}\cup\{\lambda : \alpha\overset{*}{\Rightarrow}\lambda\}$.
- **Siguientes($A$):** $\{t\in\Sigma : S\overset{*}{\Rightarrow}\alpha A t\beta\}\cup\{\$ : S\overset{*}{\Rightarrow}\alpha A\}$.
- **Condición LL(1):** la gramática debe ser **no ambigua**, **sin recursividad a izquierda** y **factorizada a izquierda**. Formalmente, para todo par de producciones $X\to\alpha_i\mid\alpha_j$ ($i\neq j$):
$$\text{Primeros}(\alpha_i)\cap\text{Primeros}(\alpha_j)=\emptyset \quad\text{y si } X\overset{*}{\Rightarrow}\lambda:\ \text{Siguientes}(X)\cap\text{Primeros}(X)=\emptyset$$
- LL(1) es análisis descendente **sin backtracking**.

### 5.2 Ascendente — LR
- **Item LR(1):** $[A\to\alpha_1\cdot\alpha_2,\,t]$ con $\alpha_1,\alpha_2\in(N\cup\Sigma)^*$ y **lookahead** $t\in\Sigma\cup\{\$\}$. (El punto puede ir al principio: $\alpha_1$ puede ser vacío.)
- **Acciones de la tabla:**
  - **Desplazar $t$:** se mete el símbolo $t$ de la entrada en la pila **y se consume** el símbolo.
  - **Reducir $A\to\beta$:** se sacan $|\beta|$ símbolos de la pila y, con $t$ en el tope, se apila $\text{ir}\_A[t,A]=q$ (no se consume entrada).
- **Conflicto desplazamiento-reducción:** pertenece al **análisis ascendente**; ocurre cuando, conociendo el contenido de la pila y el siguiente símbolo de la entrada, el analizador **no puede decidir** si desplazar o reducir. El "problema de cuándo reducir / qué producción aplicar" es el problema central del **ascendente** (no del descendente).

→ [[TLA -Análisis Sintáctico]], [[TLA -Análisis Ascendente]]

---

## 6. Gramáticas con atributos (análisis semántico)

- **Definición Dirigida por la Sintaxis (DDS):** una GLC que asocia a **cada símbolo** un conjunto de **atributos** y a **cada producción** un conjunto de **reglas semánticas** para calcular los valores de esos atributos.
- **Atributo sintetizado:** su valor depende de los atributos de los **hijos** en el árbol (se calcula de **hijos → padre**, hacia arriba).
- **Atributo heredado:** su valor depende del **nodo padre** o de los **hermanos** (se calcula de arriba/costado → **hacia el hijo**).
- **S-atribuida:** DDS que usa **sólo atributos sintetizados**.
- **Esquema de Traducción Postfijo (ETDS postfijo):** si la DDS es **S-atribuida**, cada acción semántica se coloca **al final** de la producción y se ejecuta **al hacer la reducción** en el análisis ascendente **LR**.
- **Reducción:** paso del análisis ascendente en el que se reemplaza el lado derecho $\beta$ de una producción $A\to\beta$ (que está en el tope de la pila) por el no terminal $A$; es el momento en que se disparan las acciones del ETDS postfijo.

> [!example] Frase para el Ej 7 (relacionar)
> "Cuando una DDS es **S-atribuida** (sus atributos son **sintetizados**), se puede construir un **esquema de traducción postfijo** en el que cada acción va al final de la producción y se ejecuta al realizar una **reducción** en el **análisis ascendente (LR)**."

→ [[TLA -Análisis Semántico]]

---

## 7. Máquinas de Turing y computabilidad

- **Máquina de Turing** $=\langle Q,\Sigma,\Gamma,\delta,q_0,B,F\rangle$: $\Gamma$ es el alfabeto de cinta ($\Sigma\subset\Gamma$), $B\in\Gamma$ el blanco, $\delta:Q\times\Gamma\to Q\times\Gamma\times\{L,R\}$. Tiene una **cinta infinita** y un cabezal que lee/escribe y se mueve a izquierda o derecha.
- **Qué lenguaje aceptan y por qué se llaman así:** aceptan los **lenguajes recursivamente enumerables (Tipo 0)**. Se llaman así porque un lenguaje es RE sii **existe una MT que lo acepta** (enumera/reconoce sus palabras); si además la MT **para siempre** (decide), el lenguaje es **recursivo/decidible**.
- **Autómata Linealmente Acotado (AAL):** MT **no determinista** cuya cinta tiene **longitud finita**, **función lineal** de la longitud de la cadena de entrada. Reconoce los **lenguajes sensibles al contexto (Tipo 1)**.
- **Decidible vs Tratable:** un problema es **decidible** si existe una MT que lo resuelve (siempre para); es **tratable** si además existe un algoritmo que lo resuelve en **tiempo razonable** (polinomial).

> [!example] Frase para el Ej 7 (relacionar)
> "Los **lenguajes recursivamente enumerables** son los aceptados por una **Máquina de Turing**; los **sensibles al contexto** son los aceptados por un **Autómata Linealmente Acotado**, que es una MT no determinista con cinta finita de longitud lineal en la entrada."

→ [[TLA -Máquina de Turing]], [[TLA -Máquina de Turing (Parte 2)]]

---

## 8. Demostraciones completas (Ejercicio 2)

Son "de librito": las escribís casi textual. Van las cinco que rotan.

### 8.1 AFD $\Rightarrow$ AFND-$\lambda$
**Teorema:** si $L$ es aceptado por un AFD, entonces es aceptado por un AFND-$\lambda$.

**Construcción.** Sea $A_D=\langle Q_D,\Sigma,\delta_D,q_{0D},F_D\rangle$ un AFD. Definimos el AFND-$\lambda$
$$A_N=\langle Q_D,\ \Sigma,\ \delta_N,\ q_{0D},\ F_D\rangle$$
con las mismas componentes salvo la transición:
- $\forall q\in Q_D:\ \delta_N(q,\lambda)=\emptyset$ (no agregamos transiciones $\lambda$),
- $\forall q\in Q_D,\forall a\in\Sigma:\ $ si $\delta_D(q,a)=p$ entonces $\delta_N(q,a)=\{p\}$.

**Prueba ($L(A_N)=L(A_D)$).** Como no hay transiciones $\lambda$ y cada $\delta_N(q,a)$ es un **conjunto unitario**, el AFND-$\lambda$ se comporta de forma determinista. Por inducción en $|\omega|$ se ve que $\hat{\delta}_N(q_{0},\omega)=\{\hat{\delta}_D(q_{0},\omega)\}$:
- **Base** $\omega=\lambda$: $\hat{\delta}_N(q_0,\lambda)=\{q_0\}=\{\hat{\delta}_D(q_0,\lambda)\}$.
- **Paso** $\omega=xa$: $\hat{\delta}_N(q_0,xa)=\delta_N(\hat{\delta}_N(q_0,x),a)=\delta_N(\{\hat{\delta}_D(q_0,x)\},a)=\{\delta_D(\hat{\delta}_D(q_0,x),a)\}=\{\hat{\delta}_D(q_0,xa)\}$.

Entonces $\omega\in L(A_N)\iff \hat{\delta}_N(q_0,\omega)\cap F_D\neq\emptyset \iff \hat{\delta}_D(q_0,\omega)\in F_D \iff \omega\in L(A_D)$. $\blacksquare$

### 8.2 El complemento de un regular es regular
**Teorema:** $L$ (reconocido por el AFD $A=\langle Q,\Sigma,\delta,q_0,F\rangle$) es regular $\iff$ $L^c$ es reconocido por $A'=\langle Q,\Sigma,\delta,q_0,\,Q-F\rangle$.

> [!warning] Requisito: $A$ debe ser un **AFD total** (con $\delta$ definida para todo estado y símbolo, usando estado trampa si hace falta). Sólo así $A'$ acepta exactamente lo que $A$ rechaza.

**($\Rightarrow$)** Sabemos $L^c=\Sigma^*-L$ y que $\omega\in L\iff\hat{\delta}(q_0,\omega)\in F$. Tomando $A'$:
$$\omega\in L(A') \iff \hat{\delta}(q_0,\omega)\in Q-F \iff \hat{\delta}(q_0,\omega)\notin F \iff \omega\notin L \iff \omega\in L^c$$
$\therefore L(A')=L^c$, y como $A'$ es un AFD, $L^c$ es regular.

**($\Leftarrow$)** Si $L(A')=L^c$, con el mismo razonamiento las palabras que **no** están en $L(A')$ son $\Sigma^*-L(A')=\{\omega:\hat{\delta}(q_0,\omega)\in F\}=L$. $\therefore L(A)=(L^c)^c=L$. $\blacksquare$

### 8.3 La intersección de dos regulares es regular (autómata producto)
**Teorema:** si $L=L(A)$ y $M=L(B)$ son regulares, el AFD **producto** $C$ reconoce $L\cap M$.

**Construcción.** $A=\langle Q_A,\Sigma,\delta_A,q_{0A},F_A\rangle$, $B=\langle Q_B,\Sigma,\delta_B,q_{0B},F_B\rangle$.
$$C=\langle Q_A\times Q_B,\ \Sigma,\ \delta_C,\ (q_{0A},q_{0B}),\ F_A\times F_B\rangle,\qquad \delta_C((p,q),a)=(\delta_A(p,a),\ \delta_B(q,a))$$

**Prueba (inducción estructural en $\omega$).** Se prueba que $\hat{\delta}_C((q_{0A},q_{0B}),\omega)=(\hat{\delta}_A(q_{0A},\omega),\ \hat{\delta}_B(q_{0B},\omega))$:
- **Base** $\omega=\lambda$: $\hat{\delta}_C((q_{0A},q_{0B}),\lambda)=(q_{0A},q_{0B})=(\hat{\delta}_A(q_{0A},\lambda),\hat{\delta}_B(q_{0B},\lambda))$.
- **Paso** $\omega=xa$:
$$\hat{\delta}_C((q_{0A},q_{0B}),xa)=\delta_C\big(\hat{\delta}_C((q_{0A},q_{0B}),x),a\big)=\delta_C\big((\hat{\delta}_A(q_{0A},x),\hat{\delta}_B(q_{0B},x)),a\big)=(\hat{\delta}_A(q_{0A},xa),\hat{\delta}_B(q_{0B},xa))$$

Entonces $\omega\in L(C)\iff \hat{\delta}_C(\dots)\in F_A\times F_B \iff \hat{\delta}_A(q_{0A},\omega)\in F_A \land \hat{\delta}_B(q_{0B},\omega)\in F_B \iff \omega\in L\land\omega\in M \iff \omega\in L\cap M$. $\blacksquare$

### 8.4 El AFD del conjunto cociente es equivalente y mínimo
**Teorema:** el AFD $A'$ obtenido de $A$ por el algoritmo del **conjunto cociente** (una clase por cada grupo de estados indistinguibles) es **equivalente** a $A$ y **mínimo**.

**(1) $L(A')=L(A)$.** Por el algoritmo, $\forall\omega\in\Sigma^*:\ \hat{\delta}'([q_0],\omega)=[\hat{\delta}(q_0,\omega)]$ (la clase del estado al que llega $A$). Por definición de indistinguibilidad, todos los estados de una clase **coinciden** en aceptar o no cada $\omega$. Luego $[q_0]$ llega a una clase de aceptación $\iff$ $q_0$ llega a un estado de $F$. $\therefore L(A')=L(A)$.

**(2) $A'$ es mínimo.** Supongamos que **no** lo es $\Rightarrow$ existe $A''=\langle Q'',\Sigma,\delta'',q_0'',F''\rangle$ equivalente a $A'$ con $|Q''|=n<|Q'|$. Como $A'$ salió del algoritmo, sus estados son **distinguibles de a pares**: existen $\alpha,\beta\in\Sigma^*$ con $\hat{\delta}'(q_0,\alpha)=p$, $\hat{\delta}'(q_0,\beta)=q$, $p\neq q$ (clases distintas), luego $\exists\omega:\ \hat{\delta}'(p,\omega)\in F \land \hat{\delta}'(q,\omega)\notin F$ (o viceversa) $\Rightarrow$ $\alpha\omega$ y $\beta\omega$ **difieren** en pertenencia a $L$. Pero en $A''$, con menos estados, por **palomar** $\alpha$ y $\beta$ caen en el mismo estado: $\hat{\delta}''(q_0'',\alpha)=\hat{\delta}''(q_0'',\beta)=r$ $\Rightarrow$ $\alpha\omega$ es aceptada $\iff$ $\beta\omega$ es aceptada $\Rightarrow$ $A''$ no es equivalente a $A'$. **Absurdo.** $\therefore A'$ es mínimo. $\blacksquare$

### 8.5 Equivalencia de criterios de aceptación en AP
#### 8.5.a Por pila vacía $\Rightarrow$ por estado final ($L_{pv}\Rightarrow L_{pf}$)
**Teorema:** si $L=L_{pv}(P_v)$ con $P_v=\langle Q,\Sigma,\Gamma,\delta_v,q_0,S\rangle$, existe $P_f$ con $L=L_{pf}(P_f)$.

**Construcción.** $P_f=\langle Q\cup\{q_{0f},q_{ff}\},\ \Sigma,\ \Gamma\cup\{Z_0\},\ \delta_f,\ q_{0f},\ Z_0,\ \{q_{ff}\}\rangle$ con:
1. $\delta_f(q_{0f},\lambda,Z_0)=\{(q_0,SZ_0)\}$ — apila $S$ sobre un **fondo nuevo** $Z_0$ y pasa a $q_0$.
2. $\forall p\in Q,\ a\in\Sigma\cup\{\lambda\},\ Y\in\Gamma:\ \delta_f(p,a,Y)=\delta_v(p,a,Y)$ — **replica** $P_v$.
3. $\forall p\in Q:\ \delta_f(p,\lambda,Z_0)=\{(q_{ff},\lambda)\}$ — cuando $P_v$ **vaciaría** su pila, queda expuesto $Z_0$ y se pasa al estado final.

**Prueba.** $P_f$ replica a $P_v$ con un estado extra al principio y otro final.
$$\omega\in L_{pv}(P_v)\iff (q_0,\omega,S)\vdash^*(p,\lambda,\lambda) \text{ para algún } p\in Q$$
En $P_f$: $(q_{0f},\omega,Z_0)\vdash(q_0,\omega,SZ_0)\vdash^*(p,\lambda,Z_0)\vdash(q_{ff},\lambda,\lambda)$. El fondo $Z_0$ evita que $P_f$ se detenga por pila vacía "de mentira" mientras simula $P_v$; sólo cuando $P_v$ vacía **legítimamente** aparece $Z_0$ y $P_f$ acepta. $\therefore L_{pf}(P_f)=L_{pv}(P_v)=L$. $\blacksquare$

#### 8.5.b Por estado final $\Rightarrow$ por pila vacía ($L_{pf}\Rightarrow L_{pv}$)
**Teorema:** si $L=L_{pf}(P_f)$ con $P_f=\langle Q,\Sigma,\Gamma,\delta_f,q_0,S,F\rangle$, existe $P_v$ con $L=L_{pv}(P_v)$.

**Construcción.** $P_v=\langle Q\cup\{q_{0v},q_e\},\ \Sigma,\ \Gamma\cup\{Z_0\},\ \delta_v,\ q_{0v},\ Z_0\rangle$ con:
1. $\delta_v(q_{0v},\lambda,Z_0)=\{(q_0,SZ_0)\}$ — arranca con fondo $Z_0$.
2. Replica $\delta_f$ (mismas transiciones de $P_f$).
3. $\forall p\in F,\ Y\in\Gamma\cup\{Z_0\}:\ \delta_v(p,\lambda,Y)=\{(q_e,\lambda)\}$ — desde un estado final, pasa a **vaciar**.
4. $\forall Y\in\Gamma\cup\{Z_0\}:\ \delta_v(q_e,\lambda,Y)=\{(q_e,\lambda)\}$ — $q_e$ **vacía** toda la pila.

**Prueba.** $\omega\in L_{pf}(P_f)\iff P_f$ llega a un estado de $F$ con $\omega$ consumida $\iff$ en $P_v$ se llega a ese estado y luego $q_e$ vacía la pila $\iff \omega\in L_{pv}(P_v)$. El fondo $Z_0$ impide que $P_v$ acepte por vaciado "accidental" durante la simulación (sólo $q_e$ puede sacar $Z_0$). $\therefore L_{pv}(P_v)=L$. $\blacksquare$

---

## 9. Multiple choice: las formas exactas que se repiten (Ej 3)

Los distractores cambian **un detalle** de la definición. Estas son las **versiones correctas** (confirmadas contra las claves de los finales):

| Concepto | Forma correcta |
|----------|----------------|
| **Lema bombeo regular (demo)** | AFD con **$N$ estados** ($N$=cte del lema) y cadena $\omega\in L$ con $|\omega|\ge N$ |
| **Lema bombeo LLC (forma)** | $\alpha=rxyzs$: $|xyz|\le P \land |xz|\ge1 \land \forall i\ge0:\ rx^iyz^is\in L$ |
| **Lema bombeo LLC (demo)** | Gramática en **FNC** que genere $L(G)=L-\{\lambda\}$ |
| **$\hat{\delta}$ extendida (AFD)** | $\hat{\delta}:Q\times\Sigma^*\to Q$, $\hat{\delta}(q,\omega)=\delta(\hat{\delta}(q,\omega'),a)$ si $\omega=\omega'a$ |
| **Clausura-$\lambda$ (recursiva)** | $\{q\}$ (base) $\land$ $(p\in\text{claus}(q)\land r\in\delta(p,\lambda)\Rightarrow r\in\text{claus}(q))$ (paso) |
| **Indistinguibilidad orden $k{+}1$** | $\forall a\in\Sigma:\ \delta(p,a)\,E_k\,\delta(q,a)\iff p\,E_{k+1}\,q$ |
| **Lema de Arden** | $X=\alpha X+\beta\iff X=\alpha^*\beta$, con $\lambda\notin L(\alpha)$ |
| **Primeros($A$)** | $\{t\in\Sigma : A\overset{*}{\Rightarrow}t\beta\}\cup\{\lambda : A\overset{*}{\Rightarrow}\lambda\}$ |
| **Siguientes($A$)** | $\{t\in\Sigma : S\overset{*}{\Rightarrow}\alpha A t\beta\}\cup\{\$ : S\overset{*}{\Rightarrow}\alpha A\}$ |
| **Item LR(1)** | $[A\to\alpha_1\cdot\alpha_2,\,t]$ con $\alpha_1,\alpha_2\in(N\cup\Sigma)^*$, $t\in\Sigma\cup\{\$\}$ |
| **LL(1)** | No ambigua + sin recursividad izq. + factorizada izq.; $\text{Prim}(\alpha_i)\cap\text{Prim}(\alpha_j)=\emptyset$ |
| **ACCIÓN = desplazar $t$** | Se mete $t$ de la entrada en la pila **y se consume** el símbolo |
| **ACCIÓN = reducir $A\to\beta$** | Se sacan $|\beta|$ símbolos; con $t$ en el tope se apila $\text{ir}\_A[t,A]=q$ |
| **FNC (genera LLC)** | Sin $\lambda$; producciones $A\to t$ o $A\to BC$ ($A,B,C\in N$, $t\in\Sigma$) |
| **GLC en forma normal** | Existe un AP que reconoce el mismo lenguaje que genera la gramática |
| **Lenguaje aceptado por AFND** | $L=\{\omega : \hat{\delta}(q_0,\omega)\cap F\neq\emptyset\}$ |
| **Gramática ambigua** | Existe **alguna** cadena con dos árboles de derivación distintos |

**V/F clásicos:** $\emptyset^*=\{\lambda\}$ (no $\emptyset$) · $\alpha+\emptyset=\alpha$ (no $\emptyset$) · intersección de regulares **sí** es regular · LL(1) es descendente **sin** backtracking · el estado trampa/vacío **sí** va en su clase en el conjunto cociente · hay lenguajes de gramáticas **no ambiguas** que **ningún AP determinista** reconoce (verdadero) · en el lema LLC debe cumplirse $|xz|\ge1$ (por eso "$\forall\omega,|\omega|\ge P\Rightarrow\dots$ con $|xz|=0$" es falso).

---

## 10. Práctico (Ej 5): corregir AP / completar MT

> [!tip] El único ejercicio de "hacer". Vale 1 punto. Patrón fijo: *"este autómata NO reconoce $L$, ¿por qué? ¿qué transiciones están mal/faltan?"* — **no** se agregan estados ni se cambian símbolos, sólo se corrige/completa $\delta$.

### 10.1 Método para corregir un AP
1. **Leé bien $L$** e identificá la relación que hay que forzar (p. ej. igualdad/doble de conteos, orden de los bloques, un único separador).
2. **Trazá** el AP dado sobre un par de cadenas de $L$ y de $L^c$ y detectá el modo de falla típico:
   - **Cuenta lo que no debe** (apila/desapila el símbolo equivocado): corregí qué se apila/saca en cada transición.
   - **No fuerza el orden** de los bloques (deja mezclar): falta un estado/transición que separe fases.
   - **No puede aceptar** (falta llegar a $q_f$ / falta vaciar $Z_0$): agregá la transición $\lambda$ final que vacía la pila o va al estado final.
3. **Justificá** cada cambio con la condición de $L$ que restaura.

**Casos que ya cayeron:**
- $L=\{\omega=\alpha c\beta : \#_c(\omega)=1 \land 2\,\#_a(\alpha)=\#_b(\beta)\}$ — hay que apilar **2 marcas por cada $a$** en $\alpha$ y desapilar una por cada $b$ en $\beta$; corregir las transiciones que contaban mal y el vaciado con $Z_0$.
- $L=\{\omega=\alpha 1^n 2^n \alpha^R \dots\}$ (2C-2022) — el AP **no garantiza** que los $1$ vayan antes que los $2$ y **nunca reconstruye** $\alpha^R$; faltan transiciones en $q_2$.
- $L=\{a,b\}^+$ con $\omega=\alpha\delta 0^r\beta$ y $|\alpha|_0=|\beta|_0$ (1C-2023) — estaba **contando los $1$** en lugar de los $0$; se corrigen qué símbolos se apilan.

### 10.2 Completar una MT — ejemplo $L=\{a^{2^n} : n\ge0\}$
Idea del algoritmo (longitudes $1,2,4,8,\dots$): en cada pasada **tachá una $a$ de cada dos** (reemplazá por $X$); si en una pasada la cantidad de $a$ es **impar y $>1$**, **rechazá**; si terminás con **una sola $a$**, **aceptá** (fue potencia de 2). Repetir el "dividir por 2" verifica la potencia.

**La corrección típica del final:** faltaba la **transición de aceptación** (al leer blanco $B$ tras la última pasada, ir a $q_f$) y/o el **retorno al inicio** para la siguiente pasada. Se completa $\delta$ con esos arcos sin tocar los símbolos.

---

## 11. Cheat-sheet final (lo que se pide textual)

- **$\hat{\delta}$ (AFD):** base $\hat{\delta}(p,\lambda)=p$; paso $\hat{\delta}(p,\omega'a)=\delta(\hat{\delta}(p,\omega'),a)$.
- **Clausura-$\lambda$:** base $q\in\text{claus}(q)$; paso $p\in\text{claus}(q)\land r\in\delta(p,\lambda)\Rightarrow r\in\text{claus}(q)$.
- **Forma sentencial:** base $S$; paso $\alpha\beta\gamma$ FS $\land\ \beta\to\delta\Rightarrow\alpha\delta\gamma$ FS.
- **Arden:** $X=\alpha X+\beta\Rightarrow X=\alpha^*\beta$ ($\lambda\notin\alpha$).
- **Bombeo regular:** $\omega=xyz$, $|xy|\le N$, $|y|\ge1$, $\forall i\ge0:\ xy^iz\in L$.
- **Bombeo LLC:** $\alpha=rxyzs$, $|xyz|\le P$, $|xz|\ge1$, $\forall i\ge0:\ rx^iyz^is\in L$.
- **Chomsky:** Tipo 0 RE (MT) · Tipo 1 Sensible al contexto (AAL) · Tipo 2 LLC (AP) · Tipo 3 Regular (AF).
- **FNC:** $A\to BC \mid t$ (sin $\lambda$). **FNG:** $A\to t\beta$, $t\in\Sigma$, $\beta\in N^*$.
- **LL(1):** no ambigua + sin recursividad izq. + factorizada izq.
- **Item LR(1):** $[A\to\alpha_1\cdot\alpha_2,\ t]$, $t\in\Sigma\cup\{\$\}$.
- **Sintetizado:** hijos → padre. **Heredado:** padre/hermanos → hijo. **S-atribuida** = sólo sintetizados → **ETDS postfijo** (acción al reducir en LR).
- **RE = MT; Sensible al contexto = AAL (MT no determinista, cinta lineal). Decidible = para siempre; Tratable = decidible en tiempo polinomial.**

---

*Fuentes: compilado "TLA - Finales" (2C-2022, 1C-2023, 2C-2023, 1C-2024 1ª y 2ª fecha, 2C-2024) + fotos de finales resueltos (72.39, Lic. Ana María Arias Roig). Claves verificadas contra las notas del vault de TLA.*

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -intro resumen]] — definiciones básicas
- [[TLA -Lenguajes Regulares]] — lenguajes regulares
- [[TLA -Autómatas de Pila]] — autómatas de pila
- [[TLA -Análisis Sintáctico]] — análisis sintáctico
- [[TLA -Máquina de Turing]] — máquinas de Turing
- [[TLA -Guía Repaso Final (poco tiempo)]] — repaso express

<!-- notas-relacionadas:fin -->
