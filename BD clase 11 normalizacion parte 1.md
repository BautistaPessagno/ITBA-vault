---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - normalizacion
Created: 2026-03-03 14:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - dependencias funcionales
  - axiomas de Armstrong
  - cierre de atributos
  - claves candidatas
  - formas normales 1NF 2NF 3NF
  - descomposicion sin perdida
  - preservacion de dependencias
  - cubrimiento minimal
---
# BD clase 11 — Normalización (parte 1)

## Resumen

### Dependencias funcionales

Una **dependencia funcional** (FD) es una restricción entre dos conjuntos de atributos de un esquema de relación $R$. Expresa que el valor de un conjunto de atributos determina unívocamente el valor de otro. Son una propiedad de la **semántica** (intensión) de los datos, no de una instancia particular.

$$\forall\; t_1, t_2 \in r:\; t_1[X] = t_2[X] \Rightarrow t_1[Y] = t_2[Y]$$

Se denota $X \to Y$ y se lee *"X determina funcionalmente a Y"* o *"Y depende funcionalmente de X"*.

| Notación | Significado |
|---|---|
| $X \to Y$ | X determina funcionalmente a Y |
| $X \not\to Y$ | X **no** determina funcionalmente a Y |
| $F$ | Conjunto de dependencias funcionales de un esquema |
| $F^+$ | **Clausura** de $F$: todas las FDs inferibles lógicamente desde $F$ |

> [!important]
> Las FDs dependen de la **intensión** de la base de datos (reglas del negocio), no de la extensión (datos actuales). Si cambian las reglas del mini-universo, pueden cambiar las FDs.

**Dependencia funcional trivial:** $X \to Y$ es trivial $\iff Y \subseteq X$.

> *Si el lado derecho ya está contenido en el lado izquierdo, la dependencia no aporta información nueva.*

---

### Superclaves y claves (definición vía FDs)

Usando dependencias funcionales se redefinen formalmente superclave y clave:

| Concepto              | Definición formal                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Superclave**        | $X \subseteq R$ es superclave de $R$ si $X \to R$                                                                          |
| **Clave (candidata)** | $X \subseteq R$ es clave si: 1) $X \to R$ (es superclave) y 2) $\nexists\; Y \subset X$ tal que $Y \to R$ (es **minimal**) |
| **Atributo primo**    | Un atributo es **primo** si forma parte de alguna clave candidata                                                          |
| **Atributo no primo** | Un atributo que no pertenece a ninguna clave candidata                                                                     |

> [!tip]
> En SQL, de todas las claves candidatas se elige una como `PRIMARY KEY` y las restantes se declaran con `UNIQUE`. Todas tienen igual importancia teórica.

---

### Axiomas de Armstrong

Sean $X$, $Y$, $Z$ conjuntos de atributos de $R$. Los axiomas de Armstrong son un sistema **correcto** (sound) y **completo** (complete) para inferir FDs:

| Axioma | Enunciado | Fórmula |
|---|---|---|
| **A1 — Reflexividad** | Si $Y$ es subconjunto de $X$, entonces $X$ determina a $Y$ | $Y \subseteq X \Rightarrow X \to Y$ |
| **A2 — Aumentación** | Se puede agregar atributos a ambos lados | $X \to Y \Rightarrow XZ \to YZ$ |
| **A3 — Transitividad** | FDs se encadenan | $X \to Y,\; Y \to Z \Rightarrow X \to Z$ |

> [!important]
> Por ser **correctos**, sólo derivan FDs que pertenecen a $F^+$. Por ser **completos**, pueden generar **todo** $F^+$ a partir de $F$.

**Reglas derivadas** (demostrables con A1-A3):

| Regla | Enunciado | Fórmula |
|---|---|---|
| **R1 — Unión** | Dos FDs con mismo determinante se combinan | $X \to Y,\; X \to Z \Rightarrow X \to YZ$ |
| **R2 — Pseudotransitividad** | Variante de transitividad con atributos extra | $X \to Y,\; UY \to Z \Rightarrow XU \to Z$ |
| **R3 — Descomposición** | Una FD con lado derecho compuesto se separa | $X \to YZ \Rightarrow X \to Y,\; X \to Z$ |

> *Unión y Descomposición son inversas: permiten pasar entre forma compacta y forma atómica del lado derecho.*

---

### Clausura de dependencias funcionales ($F^+$)

La **clausura** de $F$ es el conjunto de **todas** las dependencias funcionales inferibles lógicamente desde $F$:

$$F^+ = \{\; X \to Y \;/\; F \models X \to Y \;\}$$

Calcular $F^+$ tiene complejidad **exponencial** respecto al número de atributos del esquema. Por eso, en la práctica se evita calcularlo directamente y se usa el **cierre de atributos**.

> [!tip]
> Para decidir si $X \to Z \in F^+$ no hace falta calcular todo $F^+$. Basta calcular $X^+$ y verificar si $Z \subseteq X^+$.

---

### Cierre de atributos ($X^+$)

Dado un conjunto de atributos $X \subseteq R$ y un conjunto de FDs $F$, la **clausura de atributos** $X^+$ es el conjunto de todos los atributos determinados funcionalmente por $X$:

$$X^+ = \{\; A \;/\; X \to A \in F^+ \;\}$$

**Propiedad clave:** $X \to A \in F^+ \iff A \in X^+$.

#### Algoritmo para calcular $X^+$

1. Inicializar $X^+ = X$
2. **Mientras** $X^+$ sufra cambios:
   1. **Para cada** FD $Y \to Z \in F$:
      1. **Si** $Y \subseteq X^+$, entonces $X^+ = X^+ \cup Z$
3. **Devolver** $X^+$

> [!important]
> Este algoritmo tiene complejidad **lineal** respecto al tamaño de $F$. Es la herramienta fundamental para verificar pertenencia a $F^+$ y para encontrar claves.

**Ejemplo:** Sea $R(A,B,C,G,H,I)$ con $F = \{A \to B,\; A \to C,\; CG \to H,\; CG \to I,\; B \to H\}$.

Calculamos $(AG)^+$:

| Paso | FD aplicada | $X^+$ |
|---|---|---|
| Inicio | — | $\{A, G\}$ |
| 1 | $A \to B$ | $\{A, B, G\}$ |
| 2 | $A \to C$ | $\{A, B, C, G\}$ |
| 3 | $CG \to H$ | $\{A, B, C, G, H\}$ |
| 4 | $CG \to I$ | $\{A, B, C, G, H, I\}$ |
| 5 | $B \to H$ | sin cambio |

$(AG)^+ = R$, por lo tanto $AG$ es **superclave**. Para verificar si es **clave** hay que comprobar que $A^+ \neq R$ y $G^+ \neq R$ (minimalidad).

> *Una vez calculada $X^+$, toda combinación de atributos de $X^+$ es determinada funcionalmente por $X$.*

---

### Algoritmo para encontrar claves candidatas

1. Identificar los atributos que **nunca aparecen a la derecha** de ninguna FD en $F$. Estos atributos deben estar en **toda** clave candidata.
2. Sea $N$ ese conjunto. Calcular $N^+$.
   - Si $N^+ = R$, entonces $N$ es la **única clave**.
   - Si $N^+ \neq R$, combinar $N$ con cada subconjunto de los atributos restantes (empezando de menor a mayor cardinalidad).
3. Para cada candidato $C = N \cup S$:
   - Calcular $C^+$. Si $C^+ = R$, verificar **minimalidad**: ningún subconjunto propio de $C$ debe tener clausura igual a $R$.
4. Toda $C$ que pase ambas pruebas es **clave candidata**.

> [!tip]
> **Heurística:** los atributos que aparecen **sólo a la izquierda** en $F$ siempre forman parte de toda clave. Los que aparecen **sólo a la derecha** nunca forman parte de ninguna clave. Esto reduce mucho el espacio de búsqueda.

**Ejemplo:** $R(E,H,L,N,P,S)$ con $F = \{S \to E,\; P \to NL,\; SP \to H\}$.

- Atributos que nunca están a la derecha: $S$, $P$.
- $S^+ = \{S, E\} \neq R$
- $P^+ = \{P, N, L\} \neq R$
- $SP^+ = \{S, P, E, N, L, H\} = R$

Como ningún subconjunto propio de $SP$ determina $R$, la clave es $SP$.

> *Los atributos $S$ y $P$ no aparecen a la derecha de ninguna FD, así que obligatoriamente integran toda clave.*

---

### Primera Forma Normal (1NF)

Un esquema de relación $R$ está en **1NF** si todos sus atributos son **atómicos** (no contienen conjuntos, listas ni estructuras anidadas).

| Condición | Detalle |
|---|---|
| Todos los dominios son atómicos | No se admiten atributos multivaluados ni compuestos |
| Es la base del modelo relacional | Sin 1NF, el álgebra relacional no es aplicable |

> [!important]
> La **1NF** es un requisito implícito del modelo relacional. Toda tabla en un RDBMS la cumple por definición del modelo.

> *1NF se viola cuando un atributo almacena múltiples valores (e.g., una lista de teléfonos en un solo campo).*

---

### Segunda Forma Normal (2NF)

Un esquema $R$ está en **2NF** si:

1. Está en **1NF**, y
2. **Ningún atributo no primo** depende funcionalmente de un **subconjunto propio** de alguna clave candidata (no hay **dependencia parcial**).

$$\text{Violación de 2NF:}\quad \exists\; X \subset K,\; A \notin K:\; X \to A \in F^+ \quad \text{(donde } K \text{ es clave)}$$

| Término | Significado |
|---|---|
| **Dependencia parcial** | Un atributo no primo depende de **parte** de una clave compuesta |
| **Dependencia completa** | El atributo depende de **toda** la clave, no de un subconjunto |

> [!important]
> Si **todas** las claves candidatas son **simples** (un solo atributo), el esquema automáticamente cumple 2NF. Las violaciones de 2NF sólo pueden ocurrir con claves compuestas.

**Criterio de violación:**
1. Encontrar una clave candidata compuesta $K = A_1 A_2 \cdots A_n$.
2. Verificar si algún subconjunto propio $X \subset K$ determina algún atributo no primo $B$.
3. Si existe tal $X \to B$, hay **dependencia parcial** y se viola 2NF.

> *Para corregir: separar en un nuevo esquema los atributos parcialmente dependientes junto con el subconjunto de clave que los determina.*

---

### Tercera Forma Normal (3NF)

Un esquema $R$ está en **3NF** si para toda FD no trivial $X \to A \in F^+$ se cumple **al menos una** de:

1. $X$ es **superclave** de $R$, o
2. $A$ es **atributo primo** (pertenece a alguna clave candidata).

$$\text{Violación de 3NF:}\quad X \to A \in F^+,\;\; X \text{ no es superclave},\;\; A \text{ no es primo}$$

| Forma normal | Qué prohíbe |
|---|---|
| **2NF** | Dependencias **parciales** de atributos no primos respecto de claves |
| **3NF** | Dependencias **parciales** y **transitivas** de atributos no primos |

> [!important]
> 3NF implica 2NF. Toda violación de 2NF es también violación de 3NF (dependencia parcial es un caso particular donde $X$ es subconjunto propio de una clave). Pero 3NF agrega la prohibición de **dependencias transitivas**: $K \to X \to A$ donde $X$ no es superclave y $A$ no es primo.

**Criterio de violación (para cada FD $X \to A$ no trivial en $F$):**
1. Calcular $X^+$. Si $X^+ \neq R$ (no es superclave):
2. Verificar si $A$ es **primo** (pertenece a alguna clave). Si $A$ no es primo: **violación de 3NF**.

> *La 3NF es el objetivo habitual de normalización en diseño transaccional. Elimina redundancias causadas por dependencias parciales y transitivas.*

---

### Descomposición sin pérdida de información (Lossless Join)

Al descomponer un esquema $R$ en sub-esquemas $R_1, R_2, \ldots, R_k$, se exige que la junta natural de las proyecciones reconstruya exactamente la relación original:

$$R = \bigcup_{i=1}^{k} R_i \qquad\text{y}\qquad r_1 \bowtie r_2 \bowtie \cdots \bowtie r_k = r \quad\text{donde } r_i = \pi_{R_i}(r)$$

#### Condición para descomposición en dos esquemas

Si $R$ se descompone en $R_1$ y $R_2$, la descomposición es sin pérdida si **al menos una** de estas FDs pertenece a $F^+$:

$$R_1 \cap R_2 \to R_1 - R_2 \qquad\text{o}\qquad R_1 \cap R_2 \to R_2 - R_1$$

> [!tip]
> Equivale a decir que la intersección de los dos subesquemas debe ser **superclave** de al menos uno de ellos.

#### Condición para descomposición en N esquemas (Tableau)

Se construye un **tableau** (matriz de Aho-Sagiv-Ullman) con una fila por subesquema y una columna por atributo:

$$T(i,j) = \begin{cases} a_j & \text{si } R_i \text{ contiene } A_j \\ b_{ij} & \text{si } R_i \text{ no contiene } A_j \end{cases}$$

1. Armar el tableau inicial.
2. Aplicar iterativamente las FDs de $F$: si dos filas coinciden en los atributos del lado izquierdo, igualar los valores del lado derecho (preferir reemplazar $b_{kn}$ por $a_n$).
3. Si alguna fila queda compuesta **sólo por variables distinguidas** ($a_i$), la descomposición es **sin pérdida**.

> *Si no se logra una fila completa de variables distinguidas tras agotar todas las FDs, hay pérdida de información.*

---

### Preservación de dependencias

Una descomposición preserva dependencias si las restricciones originales se pueden verificar **localmente** en cada subesquema, sin necesidad de hacer juntas:

$$F'^+ = F^+ \qquad\text{donde}\quad F' = \bigcup_{i=1}^{N} \pi_{R_i}(F^+)$$

> [!important]
> - La proyección debe hacerse sobre $F^+$, **no** sobre $F$.
> - Descomposición sin pérdida de información $\not\Rightarrow$ preservación de dependencias.
> - Preservación de dependencias $\not\Rightarrow$ descomposición sin pérdida de información.
> - Son propiedades **independientes**. Una buena normalización busca **ambas**.

#### Algoritmo de Gottlob para proyectar $F^+$ sobre un subesquema

Dado $R$ con FDs $F$ y subesquema $R_1$, para obtener $F_1 = \pi_{R_1}(F^+)$ sin calcular $F^+$:

1. $F_1 = F$ (con cada FD descompuesta a lado derecho atómico vía R3)
2. $X = R - R_1$
3. **Mientras** $X \neq \emptyset$:
   1. Tomar un atributo $A \in X$; hacer $X = X - \{A\}$
   2. $\text{RES} = \emptyset$
   3. **Para cada** FD $Y \to A \in F_1$:
      1. **Para cada** FD $AZ \to B \in F_1$:
         1. $h = YZ \to B$
         2. Si $h$ no es trivial: $\text{RES} = \text{RES} \cup \{h\}$
   4. Eliminar de $F_1$ toda FD que contenga $A$
   5. $F_1 = F_1 \cup \text{RES}$
4. Devolver $F_1$

> [!tip]
> **Estrategia rápida para verificar preservación:** si toda FD original de $F$ ya aparece en $\bigcup F_i$, hay preservación directa. Si alguna FD $X \to Y$ de $F$ falta, calcular $X^+$ usando las FDs de $\bigcup F_i$: si $Y \subseteq X^+$, la FD se infiere y no se perdió.

---

### Algoritmo de descomposición en 3NF

El siguiente algoritmo produce una descomposición que cumple **3NF**, **preserva dependencias** y es **sin pérdida de información**:

1. Obtener un **cubrimiento minimal** $F_c$ de $F$ (ver sección siguiente).
2. **Para cada** FD $X \to A$ en $F_c$:
   - Crear un subesquema $R_i = X \cup \{A\}$.
   - (Si varias FDs comparten el mismo determinante $X$, agruparlas en un solo esquema $R_i = X \cup A_1 \cup A_2 \cup \cdots$).
3. Si **ninguno** de los subesquemas generados contiene alguna **clave candidata** de $R$:
   - Agregar un subesquema adicional formado por una clave candidata de $R$.
4. Eliminar subesquemas **redundantes** (aquellos cuyo conjunto de atributos esté contenido en otro subesquema).

> [!important]
> Este algoritmo garantiza:
> - **Preservación de dependencias** (cada FD del cubrimiento minimal queda en algún subesquema).
> - **Sin pérdida de información** (el paso 3 garantiza lossless join).
> - **3NF** en cada subesquema resultante.

> *Es el algoritmo de síntesis de Bernstein (1976). Es el método estándar para normalizar a 3NF.*

---

### Cubrimiento minimal (canonical cover)

Un **cubrimiento minimal** $F_c$ de $F$ es un conjunto equivalente de FDs ($F_c^+ = F^+$) que cumple:

| Propiedad | Descripción |
|---|---|
| **Lado derecho unitario** | Toda FD en $F_c$ tiene la forma $X \to A$ (un solo atributo a la derecha) |
| **Sin atributos extraños a la izquierda** | No se puede eliminar ningún atributo del lado izquierdo de ninguna FD sin cambiar $F_c^+$ |
| **Sin FDs redundantes** | No se puede eliminar ninguna FD de $F_c$ sin cambiar $F_c^+$ |

#### Algoritmo para obtener $F_c$

1. **Descomponer** el lado derecho: aplicar R3 (descomposición) para que toda FD tenga un solo atributo a la derecha.
2. **Eliminar atributos extraños** del lado izquierdo: para cada FD $X \to A$ y cada atributo $B \in X$:
   1. Calcular $(X - \{B\})^+$ usando $F$ actual.
   2. Si $A \in (X - \{B\})^+$, entonces $B$ es **extraño**: reemplazar $X \to A$ por $(X - \{B\}) \to A$.
3. **Eliminar FDs redundantes**: para cada FD $f = X \to A$ en $F_c$:
   1. Calcular $X^+$ usando $F_c - \{f\}$.
   2. Si $A \in X^+$, entonces $f$ es **redundante**: eliminarla de $F_c$.

> [!important]
> El orden importa: primero se descompone el lado derecho, luego se simplifican los lados izquierdos, y finalmente se eliminan FDs redundantes. Alterar el orden puede dar resultados incorrectos.

> [!tip]
> El cubrimiento minimal **no es único**: puede haber varios cubrimientos minimales equivalentes para un mismo $F$, dependiendo del orden en que se procesan las FDs.

> *El cubrimiento minimal es el paso previo obligatorio al algoritmo de descomposición en 3NF.*

---

### Anomalías de una base mal diseñada

Cuando un esquema tiene **redundancia de datos**, surgen tres tipos de anomalías:

| Anomalía | Descripción |
|---|---|
| **De inserción** | No se puede insertar cierta información sin completar datos no relacionados con NULL o ficticios |
| **De modificación** | Un cambio en un dato debe replicarse en múltiples tuplas; si se omite alguna, hay inconsistencia |
| **De borrado** | Al eliminar tuplas se pierde involuntariamente información ajena a lo que se quería borrar |

> [!important]
> El objetivo de la normalización es **eliminar la redundancia** y, con ella, las anomalías de inserción, modificación y borrado.

> *Intuitivamente, las anomalías aparecen cuando atributos que dependen de diferentes determinantes se mezclan en un mismo esquema.*

---

## Notas

- La **Teoría de Normalización** (Codd, 1970-1972) es el mecanismo formal para evaluar y corregir diseños de BD transaccionales (OLTP).
- Dos enfoques de normalización:
  - **Sintético** (bottom-up): partir de todos los atributos y armar subesquemas. En desuso.
  - **Analítico** (top-down): evaluar esquemas obtenidos del mapeo ER. Es el enfoque actual.
- Las formas normales basadas en FDs son: **1NF, 2NF, 3NF, BCNF**. Las formas 4NF y 5NF se basan en dependencias multivaluadas y de junta (cubiertas en clases posteriores).
- PostgreSQL reconoce la FD $PK \to R$ en el `GROUP BY`: si la PK está en el `GROUP BY`, permite omitir otros atributos del `SELECT`. No reconoce lo mismo para claves definidas con `UNIQUE`.
- Convención de notación: esquemas $R, S, T$; atributos $A, B, C$; instancias $r, s$; tuplas $t, u, v, w$; conjuntos de atributos $X, Y, Z$; restricción de tupla $t[X]$.

---

## Preguntas

1. Dado un esquema con múltiples claves candidatas, todas compuestas, cual es la estrategia mas eficiente para verificar 2NF sin explorar todas las combinaciones de subconjuntos de cada clave?
2. El algoritmo de Gottlob para proyectar $F^+$ sobre un subesquema, puede producir FDs redundantes en el resultado? Si es así, conviene aplicar cubrimiento minimal al resultado?
3. Si una descomposición tiene pérdida de dependencias pero no de información, cual es el costo práctico? Se puede compensar con triggers o constraints adicionales?
4. El algoritmo de descomposición en 3NF siempre produce la cantidad mínima de subesquemas posible, o puede haber descomposiciones con menos tablas que también cumplan 3NF + lossless join + preservación de FDs?

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (BD)**

- [[BD clase 9 SQL avanzado consultas]] — clase anterior
- [[BD clase 12 normalizacion parte 2]] — continuación
- [[BD clase 3 modelo relacional]] — claves y restricciones

**Otras materias**

- **EDA**  [[EDA - Algoritmos y Complejidad]] — el cierre de atributos es lineal en |F|, mientras que calcular F⁺ es exponencial en la cantidad de atributos

<!-- notas-relacionadas:fin -->
