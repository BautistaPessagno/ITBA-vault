---
temas:
  - Números complejos
  - Vectores en Kⁿ
  - Matrices
  - Determinantes
  - Espacios vectoriales
  - Espacios euclidianos
  - Gram-Schmidt
  - Transformaciones lineales
  - Diagonalización
  - LU / PLU
  - QR
  - SVD
  - Cuadrados mínimos
  - Pseudoinversa de Moore-Penrose
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - cheatsheet
  - mna
Created: 2026-05-19
Materia: "[[MNA.base|MNA]]"
---
# Resumen MNA
---

## Conceptos

### Números complejos *(Unidad I)*

$z = a + bi$, $a = \text{Re}(z)$, $b = \text{Im}(z)$, $|z| = \sqrt{a^2+b^2}$, $\bar{z} = a - bi$.

**Forma polar:** $z = r e^{i\theta} = r(\cos\theta + i\sin\theta)$, $r = |z|$, $\theta = \arg(z)$.

**Propiedades clave:** $z\bar{z} = |z|^2$, $\overline{z_1 z_2} = \bar{z}_1\bar{z}_2$, $|z_1 z_2| = |z_1||z_2|$.

---

### Vectores en Kⁿ *(Clase 12/03)*

Sea $K = \mathbb{R}$ o $\mathbb{C}$, $u,v \in K^n$.

**Producto interno estándar:** $\langle u,v \rangle = \sum_{i=1}^n u_i \overline{v_i}$

**Propiedades:**
- Simetría conjugada: $\langle u,v \rangle = \overline{\langle v,u \rangle}$
- Sesquilinealidad: $\langle \alpha u + \beta w, v \rangle = \alpha \langle u,v \rangle + \beta \langle w,v \rangle$
- Positividad: $\langle u,u \rangle \geq 0$, con igualdad sii $u = 0$

**Normas:**

| Norma | Fórmula |
|---|---|
| Euclídea $\|\cdot\|_2$ | $\sqrt{\langle u,u \rangle} = \sqrt{\sum |u_i|^2}$ |
| Manhattan $\|\cdot\|_1$ | $\sum |u_i|$ |
| $p$-norma $\|\cdot\|_p$ | $\left(\sum |u_i|^p\right)^{1/p}$ |
| Máximo $\|\cdot\|_\infty$ | $\max_i |u_i|$ |

**Desigualdad de Cauchy-Bunyakovsky-Schwarz:** $|\langle u,v \rangle| \leq \|u\|\|v\|$

**Ángulo:** $\cos\alpha_{uv} = \dfrac{\langle u,v \rangle}{\|u\|\|v\|}$ (en $\mathbb{R}^n$)

**Ortogonalidad:** $u \perp v \iff \langle u,v \rangle = 0$

---

### Matrices *(Clases 12/03 y 19/03)*

$A \in K^{n \times m}$. Producto $C = AB$: $c_{ij} = \sum_k a_{ik} b_{kj}$. No conmutativo en general.

**Propiedades del producto:** $(AB)^\top = B^\top A^\top$, $(AB)^{-1} = B^{-1}A^{-1}$, $(A^\top)^{-1} = (A^{-1})^\top$.

**Tipos especiales:**

| Tipo | Condición |
|---|---|
| Simétrica | $A^\top = A$ |
| Antisimétrica | $A^\top = -A$ |
| Ortogonal | $AA^\top = A^\top A = I$ |
| Idempotente | $A^2 = A$ |
| Involutiva | $A^2 = I$ |
| Nilpotente | $A^k = 0$ para algún $k \geq 1$ |
| Triangular superior/inferior | ceros debajo/encima de la diagonal |
| Diagonal | $a_{ij} = 0$ si $i \neq j$ |

**Inversa:** $A$ regular (invertible) $\iff \det(A) \neq 0$. Fórmula para $2\times 2$:
$$\begin{pmatrix} a & b \\ c & d \end{pmatrix}^{-1} = \frac{1}{ad-bc}\begin{pmatrix} d & -b \\ -c & a \end{pmatrix}$$

---

### Determinantes *(Clase 19/03)*

**$2\times 2$:** $\det\begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc$

**Menor** $M_{ij}$: determinante de la submatriz al eliminar fila $i$ y columna $j$.

**Cofactor:** $A_{ij} = (-1)^{i+j}|M_{ij}|$

**Desarrollo de Laplace** (por fila $i$): $\det(A) = \displaystyle\sum_{j=1}^n a_{ij} A_{ij}$

**Propiedades:**
- $\det(A^\top) = \det(A)$
- $\det(AB) = \det(A)\det(B)$
- $\det(A^{-1}) = 1/\det(A)$
- Intercambio de columnas (o filas) → cambia signo
- Columna igual a combinación lineal de otras $\Rightarrow \det = 0$

**Adjunta:** $\text{adj}(A) = C(A)^\top$ (transpuesta de la matriz de cofactores).
$$A \cdot \text{adj}(A) = \det(A) \cdot I \implies A^{-1} = \frac{\text{adj}(A)}{\det(A)}$$

**Sistema $AX = B$:** solución única $\iff \det(A) \neq 0$. Sistema homogéneo $AX=0$: solo solución trivial $\iff \det(A) \neq 0$; infinitas soluciones $\iff \det(A) = 0$.

---

### Espacios vectoriales generales *(Clase 26/03)*

**Axiomas de $(V,+)$** (grupo abeliano): asociatividad, neutro $0_V$, opuesto $-v$, conmutatividad.

**Axiomas de $(V,K,\cdot)$**: distributividad escalar/vectorial, asociatividad mixta, unitariedad ($1 \cdot v = v$).

**Ejemplos:** $\mathbb{R}^n$, $\mathbb{C}^{n\times m}$, $\mathcal{C}[a,b]$, $P_n[x]$.

**Subespacio** $S \subseteq V$: $0_V \in S$ y cerrado bajo $+$ y $\cdot$. Equivalente: $\alpha u + \beta v \in S$ para todo $u,v\in S$, $\alpha,\beta\in K$.

> [!warning] Contraejemplos de subespacio
> Planos que no pasan por el origen; $\{A : \det(A) = 1\}$ (no cerrado bajo suma); $\{A : \det(A) \neq 0\}$ (el neutro $0$ no está).

**Combinación lineal:** $v = \sum_{i=1}^k \alpha_i v_i$. **Generado:** $\text{gen}(A)$.

**Independencia lineal (LI):** $\sum \alpha_i v_i = 0 \Rightarrow \alpha_i = 0\ \forall i$.

**Base:** conjunto LI que genera $V$. **Dimensión:** $\dim(V) = |B|$ (único para toda base).

---

### Espacios euclidianos *(Clase 09/04)*

Espacio vectorial con producto interno $\langle \cdot,\cdot \rangle: V\times V \to K$ (sesquilineal, conjugado-simétrico, positivo definido).

**Ejemplos generalizados:**

| Espacio | Producto interno |
|---|---|
| $\mathbb{R}^n$ ponderado | $\langle u,v \rangle = \sum \alpha_i u_i v_i$, $\alpha_i > 0$ |
| $\mathbb{R}^{n\times n}$ | $\langle A,B \rangle = \text{tr}(AB^\top)$ |
| $P_n$ | $\langle p,q \rangle = \int_a^b p(x)\overline{q(x)}\,dx$ |

**Proyección ortogonal** de $u$ sobre $v$: $P_v(u) = \dfrac{\langle u,v \rangle}{\|v\|^2}\,v$

**Base ortonormal (BON):** $\{u_1,\ldots,u_n\}$ con $\langle u_i,u_j \rangle = \delta_{ij}$.

**Coordenadas en BON:** $v = \displaystyle\sum_{i=1}^n \langle v,u_i \rangle\, u_i$

---

### Gram-Schmidt y BON *(Clase 09/04)*

Dado $\{v_1,\ldots,v_n\}$ LI en un espacio euclídeo, construir BON $\{u_1,\ldots,u_n\}$:

**Paso $k$:**
$$\tilde{u}_k = v_k - \sum_{j=1}^{k-1} \langle v_k, u_j \rangle\, u_j, \qquad u_k = \frac{\tilde{u}_k}{\|\tilde{u}_k\|}$$

> [!warning]
> Normalizar al final de cada paso. Omitir la normalización da vectores ortogonales pero no ortonormales.

> [!tip] L² como ejemplo
> Los espacios $L^1$, $L^2$ son euclidianos con producto integral. Gram-Schmidt funciona igual sobre funciones.

---

### Transformaciones lineales *(Clase 16/04)*

$T: V \to W$ es lineal sii $T(\alpha u + \beta v) = \alpha T(u) + \beta T(v)$ para todo $u,v \in V$, $\alpha,\beta \in K$.

**Núcleo (kernel):** $N(T) = \{v \in V : T(v) = 0_W\}$ — subespacio de $V$.

**Imagen (rango):** $R(T) = \{T(v) : v \in V\}$ — subespacio de $W$.

| Propiedad | Condición equivalente |
|---|---|
| Inyectiva | $N(T) = \{0_V\}$ |
| Sobreyectiva | $R(T) = W$ |
| Isomorfismo | inyectiva y sobreyectiva |

**Teorema de la dimensión:**
$$\dim(V) = \dim(N(T)) + \dim(R(T))$$

**Corolario:** Si $\dim V = \dim W$, entonces $T$ inyectiva $\iff$ $T$ sobreyectiva $\iff$ $T$ isomorfismo.

> [!tip] Con matriz asociada $A$
> $N(T)$: resolver $A\mathbf{x} = \mathbf{0}$ (escalonar). $R(T)$: columnas originales de $A$ correspondientes a los pivotes.

---

### Coordenadas y matriz asociada *(Clase 23/04)*

**Coordenadas** de $v$ en base $B = \{b_1,\ldots,b_n\}$: vector $[v]_B \in K^n$ tal que $v = \sum \alpha_i b_i$.

El mapa $\varphi: V \to K^n$, $v \mapsto [v]_B$ es un isomorfismo $\Rightarrow V \cong K^n$ (en particular $P_n \cong \mathbb{R}^{n+1}$, $\mathbb{R}^{2\times 2} \cong \mathbb{R}^4$).

**Matriz de la TL** $T: V \to W$ respecto a $B_V = \{v_1,\ldots,v_n\}$ y $B_W$:
$$M_{B_V B_W}(T) = \big[[Tv_1]_{B_W}\ \big|\ [Tv_2]_{B_W}\ \big|\ \cdots\ \big|\ [Tv_n]_{B_W}\big]$$

Columna $i$ = imagen de $v_i$ expresada en $B_W$.

**Matrices similares:** $B = P^{-1}AP$ representa la misma TL en distinta base.

**Cambio de base:** $[v]_{B_2} = P\,[v]_{B_1}$, $P$ = matriz de cambio de $B_1$ a $B_2$.

---

### Autovalores y diagonalización *(Clases 23/04 y 30/04)*

$Av = \lambda v$, $v \neq 0$: $\lambda$ autovalor, $v$ autovector.

**Polinomio característico:** $p_A(\lambda) = \det(\lambda I - A)$

**Autoespacio:** $S_\lambda = N(A - \lambda I)$

| Multiplicidad | Definición |
|---|---|
| Algebraica | Exponente de $(\lambda - \lambda_0)$ en $p_A$ |
| Geométrica | $\dim S_\lambda = \dim N(A - \lambda I)$ |

**Siempre:** mult. geométrica $\leq$ mult. algebraica.

**Criterio de diagonalización:** $A$ diagonalizable $\iff$ mult. geom. = mult. alg. para **todo** autovalor.

**Construcción:** $D = \text{diag}(\lambda_1,\ldots,\lambda_n)$, $P = [v_1|\cdots|v_n]$ (autovectores LI). Entonces $A = PDP^{-1}$.

> [!warning] Trampa clásica
> Si hay un autovalor con mult. algebraica $\geq 2$, verificar explícitamente $\dim N(A-\lambda I)$. Si no coincide con la mult. algebraica, **no es diagonalizable**.

> [!tip] Matrices triangulares
> Los autovalores son los elementos de la diagonal. El polinomio característico es el producto de los factores diagonales.

---

### Factorización LU / PLU *(Clase 07/05)*

**LU (Doolittle):** $A = LU$, $L$ triangular inferior con $1$s en diagonal, $U$ triangular superior.

**Multiplicadores:** $m_{ij} = a^{(k)}_{ij} / a^{(k)}_{ii}$ (fila $i$ $\leftarrow$ fila $i$ $-$ $m_{ij}$ · fila $j$). Los $m_{ij}$ se guardan en $L$.

**PLU:** si un pivote es cero, permutar filas con $E_i$ antes de continuar.

**Proposición:** $A$ regular $\Rightarrow$ $\exists P$ permutación tal que $PA = LU$.

**Uso para $AX = B$:**
1. Resolver $LY = PB$ (sustitución progresiva → $Y$).
2. Resolver $UX = Y$ (sustitución regresiva → $X$).

> [!warning]
> Intercambiar filas **antes** de calcular multiplicadores. Si no, los multiplicadores de $L$ quedan incorrectos.

---

### Factorización QR *(Clase 07/05)*

$A = QR$: $Q$ con columnas ortonormales (Gram-Schmidt sobre columnas de $A$), $R = Q^\top A$ triangular superior con entradas $r_{ij} = \langle a_j, q_i \rangle$.

**Algoritmo** para $A$ de $m\times n$ con columnas LI ($m \geq n$):
1. Aplicar Gram-Schmidt a $a_1,\ldots,a_n$ → $q_1,\ldots,q_n$ ortonormales.
2. $Q = [q_1|\cdots|q_n]$, $R_{ij} = \langle a_j, q_i \rangle$ para $i \leq j$ (0 para $i > j$).

**QR completo (cuadrado):** extender $Q$ a BON completa de $K^m$ → $Q$ cuadrada $m\times m$.

> [!tip] Relación con Gram-Schmidt
> $a_k = \sum_{i \leq k} r_{ik} q_i$: cada columna de $A$ se expresa como combinación de las $q_i$ ya construidas.

---

### SVD *(Clase 14/05)*

**Teorema:** toda $A \in \mathbb{R}^{m\times n}$ admite $A = USV^\top$ donde:
- $U$ ($m\times m$) ortogonal, $V$ ($n\times n$) ortogonal
- $S$ ($m\times n$) diagonal con $\sigma_1 \geq \sigma_2 \geq \cdots \geq 0$

**Valores singulares:** $\sigma_i = \sqrt{\lambda_i(A^\top A)}$

**Procedimiento:**
1. Calcular $A^\top A$ y sus autovalores $\lambda_1 \geq \cdots \geq \lambda_n \geq 0$.
2. $\sigma_i = \sqrt{\lambda_i}$; autovectores ortonormalizados de $A^\top A$ → columnas de $V$.
3. Para $\sigma_i > 0$: $u_i = Av_i / \sigma_i$.
4. Completar $\{u_i\}$ a BON de $\mathbb{R}^m$ (para los $\sigma_i = 0$, tomar vectores $\perp$ a los anteriores).
5. $U = [u_1|\cdots|u_m]$, $S$ con $\sigma_i$ en la diagonal.

**Rango:** $\text{rango}(A) = $ número de $\sigma_i > 0$.

**Positiva definida:** $X^\top AX > 0$ para todo $X \neq 0$ $\iff$ todos los autovalores de $A$ son positivos.

> [!warning] Errores frecuentes con SVD
> - $V$ son autovectores de $A^\top A$; $U$ son autovectores de $AA^\top$ (o se calculan con $u_i = Av_i/\sigma_i$). No mezclar.
> - Verificar $Av_i = \sigma_i u_i$ (signo positivo). Si $Av_i = -\sigma_i u_i$, tomar $-u_i$.
> - La descomposición es $A = USV^\top$, no $USV$.

---

### Cuadrados mínimos y pseudoinversa *(Clase 14/05)*

**Problema:** $A \in \mathbb{R}^{m\times n}$, $b \in \mathbb{R}^m$, $m > n$, sistema sobredeterminado. Minimizar $\|Ax - b\|^2$.

**Ecuaciones normales:** $A^\top A\, x = A^\top b$

**Pseudoinversa de Moore-Penrose** (cuando $A^\top A$ es invertible, i.e., columnas de $A$ LI):
$$A^+ = (A^\top A)^{-1} A^\top, \qquad x^* = A^+ b$$

**Via QR:** con $A = QR$, entonces $R x = Q^\top b$. Resolver por sustitución regresiva.

> [!tip] Cuadrados mínimos via QR (receta de parcial)
> 1. Construir $A$ según el modelo (e.g., para $y = a\cos x + b\sin x$: fila $i$ = $[\cos x_i,\ \sin x_i]$).
> 2. Factorizar $A = QR$.
> 3. Calcular $c = Q^\top b$.
> 4. Resolver $Rx = c$ (sustitución regresiva) → coeficientes $[a,b]^\top$.

---

## Práctico

### TL en $\mathbb{R}^3$ — Receta

Dada $T: \mathbb{R}^3 \to \mathbb{R}^3$ con matriz $A$:

**Núcleo $N(T)$:**
1. Escalonar $[A \mid 0]$.
2. Variables libres → expresar solución general.
3. Base de $N(T)$: vectores de los parámetros libres.
4. $\dim N(T) =$ número de variables libres.

**Imagen $R(T)$:**
- Por teorema de la dim: $\dim R(T) = 3 - \dim N(T)$.
- Base: columnas **originales** (sin escalonar) de $A$ que corresponden a los pivotes.

**Inyectiva:** $\dim N(T) = 0$. **Sobreyectiva:** $\dim R(T) = 3$.

**Preimagen de $b$:**
1. Resolver $A\mathbf{x} = b$.
2. Si compatible: $T^{-1}(b) = x_p + N(T)$ (solución particular + núcleo).
3. Si incompatible: $b \notin R(T)$.

---

### Diagonalización con parámetro $k$ — Receta

1. Calcular $p_A(\lambda) = \det(\lambda I - A)$ en función de $k$.
2. Hallar autovalores $\lambda_i(k)$ y sus multiplicidades algebraicas.
3. Para cada $\lambda_i$: calcular $\dim N(A - \lambda_i I)$ (escalonar $A - \lambda_i I$).
4. Comparar mult. geométrica vs algebraica:
   - Iguales para todo $\lambda_i$ → diagonalizable para ese $k$.
   - Determinar los valores de $k$ que lo permiten.
5. Construir $P = [v_1|\cdots|v_n]$ (autovectores), $D = \text{diag}(\lambda_1,\ldots,\lambda_n)$.

> [!warning]
> Si un $\lambda$ tiene mult. algebraica 2, calcular $\dim N(A-\lambda I)$: puede ser 1 (no diag.) o 2 (diag.).

---

### SVD de una matriz $2\times 2$ — Receta

Para $A \in \mathbb{R}^{2\times 2}$:

1. $B = A^\top A$ (simétrica $2\times 2$).
2. Autovalores $\lambda_1 \geq \lambda_2 \geq 0$ de $B$ → $\sigma_i = \sqrt{\lambda_i}$.
3. Autovectores ortonormalizados de $B$ → columnas de $V = [v_1, v_2]$.
4. $u_i = Av_i / \sigma_i$ para $\sigma_i > 0$.
5. Si $\sigma_2 = 0$: completar $u_2 \perp u_1$ (cualquier vector ortogonal a $u_1$, normalizado).
6. Verificar $A = U S V^\top$.

---

### QR — Receta

Para $A$ de $m\times n$ con columnas $a_1,\ldots,a_n$ LI:

1. $q_1 = a_1 / \|a_1\|$
2. Para $k = 2,\ldots,n$: $\tilde{q}_k = a_k - \sum_{j<k} \langle a_k, q_j \rangle q_j$, $q_k = \tilde{q}_k / \|\tilde{q}_k\|$
3. $Q = [q_1|\cdots|q_n]$
4. $R$: $r_{ij} = \langle a_j, q_i \rangle$ para $i \leq j$, $r_{ij} = 0$ para $i > j$

> [!tip]
> Verificar $A = QR$ y que $Q^\top Q = I_n$.

---

### PLU — Receta

Para $A$ de $3\times 3$ (o $3\times 2$):

1. Si $a_{11} \neq 0$: calcular $m_{i1} = a_{i1}/a_{11}$ para $i > 1$; eliminar columna 1; guardar $m_{i1}$ en $L_{i1}$.
2. Si $a_{11} = 0$: encontrar fila $k > 1$ con $a_{k1} \neq 0$, intercambiar filas 1 y $k$, registrar en $P$.
3. Repetir para columna 2 (en la submatriz inferior derecha).
4. $L$: triangular inferior con $1$s en diagonal y multiplicadores. $U$: resultado final del escalonamiento.
5. Verificar $PA = LU$.

---

### Cuadrados mínimos via QR — Receta

Datos $(x_i, y_i)$, modelo $y = f(x; a, b)$:

1. Construir $A \in \mathbb{R}^{m\times 2}$: fila $i$ = valores de las funciones base en $x_i$.
2. Construir $b = [y_1,\ldots,y_m]^\top$.
3. Factorizar $A = QR$ ($Q$ columnas ortonormales, $R$ triangular $2\times 2$).
4. Calcular $c = Q^\top b$.
5. Resolver $Rx = c$ (sustitución regresiva) → coeficientes del modelo.

---

### Cambio de base $M(T)_{B_V B_W}$ — Receta

$T: V \to W$, $B_V = \{v_1,\ldots,v_n\}$, $B_W = \{w_1,\ldots,w_m\}$:

1. Calcular $T(v_i)$ para cada $i$.
2. Expresar $T(v_i) = \alpha_{1i} w_1 + \cdots + \alpha_{mi} w_m$ (resolver sistema en la base $B_W$).
3. Columna $i$ de $M_{B_V B_W}(T)$ $= [\alpha_{1i},\ldots,\alpha_{mi}]^\top$.

---

### Coordenadas en $P_n$ y $\mathbb{R}^{n\times m}$

**En $P_n$** con base $B = \{p_1,\ldots,p_{n+1}\}$: escribir $v = \sum \alpha_i p_i$, comparar coeficientes por grado → sistema lineal en $\alpha_i$.

**En $\mathbb{R}^{n\times m}$** con base custom: vectorizar las matrices y resolver el sistema de coordenadas.

---

## Banco de preguntas

### TL — Núcleo, imagen e inyectividad

**Ej. — $T(x_1,x_2,x_3) = (x_1-x_2,\, x_2-x_3,\, x_1-x_3)$. Hallar $N(T)$, $R(T)$, inyectividad, sobreyectividad.**

> [!success]- Resolución
> $A = \begin{pmatrix} 1 & -1 & 0 \\ 0 & 1 & -1 \\ 1 & 0 & -1 \end{pmatrix}$. Fila 3 ← F3 - F1: $(0,1,-1)$ = fila 2. Rango 2.
>
> $\dim N(T) = 1$, $\dim R(T) = 2$.
>
> $N(T)$: $x_3$ libre, $x_2 = x_3$, $x_1 = x_3$ → base $\{(1,1,1)\}$.
>
> $R(T)$: columnas 1 y 2 de $A$ → base $\{(1,0,1),(-1,1,0)\}$.
>
> **No inyectiva** ($\dim N(T) = 1 \neq 0$). **No sobreyectiva** ($\dim R(T) = 2 \neq 3$).

---

### Diagonalización con parámetro

**Ej. — $M = \begin{pmatrix} a & 1 & 0 \\ 0 & a & 0 \\ 0 & 0 & 2 \end{pmatrix}$. ¿Para qué valores de $a$ es diagonalizable?**

> [!success]- Resolución
> $p_M(\lambda) = (a-\lambda)^2(2-\lambda)$. Autovalores: $\lambda_1 = a$ (mult. alg. 2), $\lambda_2 = 2$ (mult. alg. 1).
>
> Para $\lambda_2 = 2$: $\dim S_2 = 1$ ✓.
>
> Para $\lambda_1 = a$ (caso $a \neq 2$): $M - aI = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 2-a \end{pmatrix}$, rango 2, $\dim N = 1 \neq 2$. No diagonalizable.
>
> Para $a = 2$: $\lambda$ único = 2 con mult. alg. 3. $M - 2I = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$, $\dim N = 2 \neq 3$. No diagonalizable.
>
> **Conclusión:** $M$ no es diagonalizable para ningún valor de $a$.

---

**Ej. — Verificar si $(1,2,3)$ es autovector de $A = \begin{pmatrix} 2 & 0 & 1 \\ 0 & k & 0 \\ 1 & 0 & 2 \end{pmatrix}$. Hallar $k$.**

> [!success]- Resolución
> $A(1,2,3)^\top = (5, 2k, 7)^\top$. Para que sea autovector: $(5,2k,7) = \lambda(1,2,3)$.
>
> De la primera componente: $\lambda = 5$. De la tercera: $7 = 15$. Contradicción → $(1,2,3)$ **no puede ser autovector** de esta $A$ para ningún $k$.
>
> *(Si se pide solo $k$, el ejercicio típico usa un $A$ donde las tres ecuaciones son compatibles.)*

---

### SVD

**Ej. — Calcular SVD de $A = \begin{pmatrix} 1 & 1 \\ 0 & 1 \\ 1 & 0 \end{pmatrix}$.**

> [!success]- Resolución
> $A^\top A = \begin{pmatrix} 2 & 1 \\ 1 & 2 \end{pmatrix}$. Autovalores: $\lambda_1 = 3$, $\lambda_2 = 1$. Valores singulares: $\sigma_1 = \sqrt{3}$, $\sigma_2 = 1$.
>
> Autovectores de $A^\top A$: $v_1 = \tfrac{1}{\sqrt{2}}(1,1)^\top$, $v_2 = \tfrac{1}{\sqrt{2}}(1,-1)^\top$.
>
> $u_1 = Av_1/\sqrt{3} = \tfrac{1}{\sqrt{6}}(2,1,1)^\top$.
> $u_2 = Av_2/1 = \tfrac{1}{\sqrt{2}}(0,-1,1)^\top$.
> $u_3$: vector ortonormal a $u_1, u_2$ → $u_3 = \tfrac{1}{\sqrt{3}}(-1,1,1)^\top$ (verificar ortogonalidad).
>
> $S = \begin{pmatrix}\sqrt{3} & 0 \\ 0 & 1 \\ 0 & 0\end{pmatrix}$. Verificar $A = USV^\top$.

---

### QR

**Ej. — Factorizar $A = \begin{pmatrix} 1 & 1 \\ 1 & 0 \\ 0 & 1 \end{pmatrix}$.**

> [!success]- Resolución
> $a_1 = (1,1,0)^\top$: $\|a_1\| = \sqrt{2}$, $q_1 = \tfrac{1}{\sqrt{2}}(1,1,0)^\top$.
>
> $\langle a_2, q_1 \rangle = \tfrac{1}{\sqrt{2}}(1 + 0 + 0) = \tfrac{1}{\sqrt{2}}$.
> $\tilde{q}_2 = (1,0,1)^\top - \tfrac{1}{\sqrt{2}} \cdot \tfrac{1}{\sqrt{2}}(1,1,0)^\top = (\tfrac{1}{2},-\tfrac{1}{2},1)^\top$.
> $\|\tilde{q}_2\| = \sqrt{\tfrac{3}{2}}$, $q_2 = \tfrac{1}{\sqrt{3/2}}(\tfrac{1}{2},-\tfrac{1}{2},1)^\top$.
>
> $R = \begin{pmatrix} \sqrt{2} & \tfrac{1}{\sqrt{2}} \\ 0 & \sqrt{\tfrac{3}{2}} \end{pmatrix}$. Verificar $A = QR$.

---

### PLU

**Ej. — Descomponer $A = \begin{pmatrix} 0 & 2 & 1 \\ 1 & 0 & 3 \\ 2 & 1 & 0 \end{pmatrix}$.**

> [!success]- Resolución
> Pivote $a_{11} = 0$: intercambiar F1 ↔ F2. $P = E_{12}$.
>
> $A' = \begin{pmatrix} 1 & 0 & 3 \\ 0 & 2 & 1 \\ 2 & 1 & 0 \end{pmatrix}$. Multiplicador col. 1: $m_{31} = 2$. F3 ← F3 − 2·F1: $(0,1,-6)$.
>
> $A'' = \begin{pmatrix} 1 & 0 & 3 \\ 0 & 2 & 1 \\ 0 & 1 & -6 \end{pmatrix}$. Multiplicador col. 2: $m_{32} = \tfrac{1}{2}$. F3 ← F3 − $\tfrac{1}{2}$·F2: $(0,0,-\tfrac{13}{2})$.
>
> $L = \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 2 & \tfrac{1}{2} & 1 \end{pmatrix}$, $U = \begin{pmatrix} 1 & 0 & 3 \\ 0 & 2 & 1 \\ 0 & 0 & -\tfrac{13}{2} \end{pmatrix}$, $P = \begin{pmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{pmatrix}$.
>
> Verificar $PA = LU$.

---

### Cuadrados mínimos

**Ej. — Ajustar $y = a\cos x + b\sin x$ a los datos $(0,1)$, $(\pi/2, 0)$, $(\pi, -1)$.**

> [!success]- Resolución
> $A = \begin{pmatrix} \cos 0 & \sin 0 \\ \cos\frac{\pi}{2} & \sin\frac{\pi}{2} \\ \cos\pi & \sin\pi \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \\ -1 & 0 \end{pmatrix}$, $b = (1,0,-1)^\top$.
>
> $A^\top A = \begin{pmatrix} 2 & 0 \\ 0 & 1 \end{pmatrix}$, $A^\top b = \begin{pmatrix} 2 \\ 0 \end{pmatrix}$.
>
> Ecuaciones normales: $a = 1$, $b = 0$. Ajuste: $y = \cos x$. (En este caso el ajuste es exacto.)

---

## Trampas y Errores Recurrentes

> [!danger] Errores que aparecen en todos los parciales

1. **Mult. geométrica $\leq$ mult. algebraica siempre.** Para diagonalizar se necesita igualdad en **todos** los autovalores, no solo en uno.
2. **Columnas de $Q$ deben normalizarse.** Gram-Schmidt sin normalizar da vectores ortogonales, no ortonormales: $Q^\top Q \neq I$ y $R \neq Q^\top A$.
3. **$V$ en SVD = autovectores de $A^\top A$, no de $AA^\top$.** Usar el orden correcto al armar $V$.
4. **Verificar el signo de $u_i = Av_i/\sigma_i$.** Puede resultar $Av_i = -\sigma_i u_i$; en ese caso tomar $-u_i$ como vector de $U$.
5. **La descomposición SVD es $A = USV^\top$, no $A = USV$.** Transponer $V$ al multiplicar.
6. **Base de $R(T)$: columnas originales de $A$** (sin escalonar) que corresponden a los pivotes. Las columnas escalonadas no son la base.
7. **El núcleo es subespacio**: siempre contiene a $0_V$. Una preimagen de un vector no nulo no es el núcleo.
8. **Intercambiar filas ANTES de calcular multiplicadores** en PLU. Hacerlo después corrompe $L$.
9. **Verificar $PA = LU$**, no $A = LU$. En PLU hay una permutación.
10. **Ecuaciones normales**: son $A^\top Ax = A^\top b$, **no** $Ax = b$ (que es el sistema sobredeterminado original sin solución exacta).
11. **En $p_A(\lambda) = \det(\lambda I - A)$**: el signo importa para escribir el polinomio correctamente, aunque los autovalores son los mismos que con $\det(A - \lambda I)$.
12. **Para matrices triangulares**: autovalores = diagonal, pero los autovectores se calculan igual (escalonando $A - \lambda I$).
13. **QR rectangular**: si $A$ es $m \times n$ con $m > n$, $Q$ tiene $n$ columnas (no $m$) y $R$ es $n \times n$. El QR completo extiende $Q$ a $m \times m$.
14. **Cuadrados mínimos via QR**: resolver $Rx = Q^\top b$ (no $Rx = b$).

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (MNA)**

- [[GUIA_ESTUDIO_FINAL]] — guía de estudio del final

<!-- notas-relacionadas:fin -->
