---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - normalizacion
Created: 2026-03-0314:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Formas normales (1NF, 2NF, 3NF, BCNF, 4NF, 5NF)
  - Recubrimiento minimal
  - Algoritmo de descomposicion en BCNF
  - Algoritmo de descomposicion en 3NF
  - BCNF vs 3NF y preservacion de dependencias
  - Dependencias multivaluadas (MVDs) y 4NF
  - Dependencias de junta y 5NF
  - Tableau y chase algorithm
---
# BD clase 12 -- Normalizacion (parte 2)

## Resumen

### Primera y segunda forma normal (1NF, 2NF)

**1NF** exige que todos los atributos de un esquema sean **atomicos**. Hoy se considera parte de la definicion misma de relacion.

**2NF** se basa en el concepto de **dependencia funcional total**: un esquema esta en 2NF si ningun atributo no primo depende parcialmente de alguna clave. Operativamente **ya no se usa** porque la 3NF la subsume.

- **Dependencia total**: $\alpha \to \beta$ es total si $\forall A \in \alpha: (\alpha - \{A\}) \not\to \beta$
- **Dependencia parcial**: $\alpha \to \beta$ es parcial si $\exists A \in \alpha / (\alpha - \{A\}) \to \beta$

> [!tip] La 2NF solo puede violarse cuando la clave es **compuesta**. Si la clave es simple, el esquema ya esta automaticamente en 2NF.

### Recubrimiento minimal de F

El **recubrimiento minimal** $F_m$ de un conjunto de dependencias funcionales $F$ es un conjunto equivalente a $F$ que cumple cuatro condiciones:

| Condicion | Descripcion |
|---|---|
| a) Equivalencia | $F_m \equiv F$ (misma clausura) |
| b) Lado derecho simple | Cada DF tiene un unico atributo a la derecha |
| c) Sin atributos redundantes a izquierda | Eliminar cualquier atributo del lado izquierdo rompe la equivalencia |
| d) Sin DFs redundantes | Eliminar cualquier DF rompe la equivalencia |

**Algoritmo para calcular $F_m$:**

1. **Desdoblar** las DFs con lado derecho compuesto en DFs con atributo simple a la derecha
2. **Eliminar atributos redundantes a izquierda**: para cada DF $\alpha \to A$, probar si algun atributo de $\alpha$ puede removerse sin cambiar la clausura
3. **Eliminar DFs redundantes**: para cada DF, verificar si se puede inferir de las restantes

> [!important] El recubrimiento minimal es el punto de partida para el algoritmo de descomposicion en 3NF. Sin el, no se puede garantizar conservacion de dependencias.

### Tercera forma normal (3NF)

Un esquema $R$ esta en **tercera forma normal** si para toda dependencia $\alpha \to B$ no trivial de $F^+$ se cumple:

$$\alpha \text{ es superclave} \quad \lor \quad B \text{ es primo}$$

La definicion clasica equivalente: esta en 2NF y ningun atributo no primo depende **transitivamente** de una clave.

- **Dependencia transitiva**: $X \to Y$ es transitiva si existe un conjunto de atributos $Z$ no primos tal que $X \to Z$ y $Z \to Y$

**Algoritmo de descomposicion en 3NF:**

1. Hallar $F_m$ (recubrimiento minimal de $F$)
2. Juntar las DFs que coincidan en su lado izquierdo (aplicar regla de union)
3. Para cada $X \to Y$ de $F_m$ (luego del paso 2), crear una relacion con el esquema $XY$
4. Eliminar toda relacion cuyo esquema sea subconjunto de otro
5. Si ninguno de los esquemas contiene una **clave candidata** de $R$, agregar un esquema que la contenga

> [!important] Este algoritmo garantiza **dos propiedades simultaneamente**: junta sin perdida (lossless join) y conservacion de dependencias funcionales. Es el unico algoritmo de normalizacion que garantiza ambas.

> *Cada DF queda contenida en algun subesquema, y la inclusion de una clave candidata asegura la reconstruccion sin perdida.*

### Forma Normal de Boyce-Codd (BCNF)

Un esquema $R$ esta en **BCNF** si para toda dependencia $\alpha \to \beta$ no trivial de $F^+$ se cumple:

$$\alpha \text{ es superclave de } R$$

Equivalentemente: $\beta \subseteq \alpha$ (trivial) o $\alpha$ es superclave.

La diferencia con 3NF es que BCNF **no permite la excepcion** de que $\beta$ sea primo. Por eso elimina redundancias incluso entre atributos primos.

**Algoritmo de descomposicion en BCNF:**

1. Inicializar $\varphi = \{R\}$
2. Calcular $F^+$
3. Mientras exista un subesquema $R_i \in \varphi$ con una DF $\alpha \to \beta$ que viole BCNF:
   1. Generar el esquema $R_{B} = (\alpha, \beta)$ -- contiene la DF violadora
   2. Generar el esquema $R_{i}' = R_i - (\beta - \alpha)$ -- el resto sin los atributos determinados
   3. Proyectar $F^+$ sobre cada nuevo subesquema y calcular sus claves
   4. Reemplazar $R_i$ por $R_B$ y $R_i'$ en $\varphi$
   5. Repetir para cada subesquema que aun viole BCNF (incluidos los de la "rama derecha")
4. La descomposicion final es $\varphi$

> [!important] El orden en que se eligen las DFs violadoras **influye** en la descomposicion final. Ademas, este algoritmo **garantiza junta sin perdida** pero **NO garantiza conservacion de dependencias funcionales**.

> *En cada paso se cumple $R_{i+1B} \cap R_{i+1} = \alpha$ y $R_{i+1B} - R_{i+1} = \beta$, con lo cual la DF de la interseccion pertenece a $F^+$, asegurando lossless join.*

### BCNF vs. 3NF -- tabla comparativa

| Criterio | BCNF | 3NF |
|---|---|---|
| Condicion | Todo determinante es superclave | Todo determinante es superclave **o** el determinado es primo |
| Redundancia | Elimina **toda** redundancia por DFs | Puede dejar redundancia limitada (relaciones transitivas con primos) |
| Junta sin perdida | **Garantizada** por el algoritmo | **Garantizada** por el algoritmo |
| Conservacion de DFs | **NO garantizada** | **Garantizada** |
| Relacion | BCNF $\Rightarrow$ 3NF (BCNF es mas restrictiva) | 3NF $\not\Rightarrow$ BCNF |
| Existencia | No siempre se puede alcanzar sin perder DFs | Siempre alcanzable conservando DFs |

> [!tip] **Criterio de decision practica**: intentar primero BCNF. Si se pierden dependencias funcionales, evaluar el costo: si la perdida compromete la integridad de los datos, **quedarse con 3NF** y tolerar la redundancia limitada. En la mayoria de los casos 3NF es preferible cuando hay conflicto.

**Ejemplo clasico de conflicto** (ciudad, direccion, codPostal):
- Claves: `ciudad direccion` y `codPostal direccion`
- `codPostal -> ciudad` viola BCNF, pero todos los atributos son primos, asi que esta en 3NF
- Al descomponer en BCNF se pierde `ciudad direccion -> codPostal`
- Consecuencia: se pueden insertar datos inconsistentes en los subesquemas que, al juntarlos, violan la DF perdida
- **Conclusion**: no conviene normalizar a BCNF en este caso

> *Al perder una dependencia, las relaciones individuales no la violan, pero la base de datos como un todo si puede quedar inconsistente.*

### Preservacion de dependencias -- definicion y test

Una descomposicion $\{R_1, R_2, \ldots, R_k\}$ de $R$ **preserva las dependencias** si:

$$\left(\bigcup_{i=1}^{k} F_i\right)^+ = F^+$$

donde $F_i = \Pi_{R_i}(F)$ es la proyeccion de $F$ sobre $R_i$.

**Test de preservacion para una DF** $\alpha \to \beta$: calcular $\alpha^+$ usando solo las dependencias de $\bigcup F_i$. Si $\beta \subseteq \alpha^+$, la DF se conserva. Repetir para cada DF de $F$.

> [!important] Si una DF se pierde, la unica forma de garantizarla es con **restricciones adicionales a nivel de la base de datos** (triggers, checks entre tablas), lo que penaliza el rendimiento.

### Propiedades de descomposicion por forma normal

| Propiedad | 3NF | BCNF | 4NF |
|---|---|---|---|
| Junta sin perdida (lossless join) | Si | Si | Si |
| Conservacion de DFs | Si | **No siempre** | **No siempre** |
| Elimina redundancia por DFs | Parcial | Total | Total |
| Elimina redundancia por MVDs | No | No | Si |

### Dependencias multivaluadas (MVDs) y 4NF

Una **dependencia multivaluada** $\alpha \twoheadrightarrow \beta$ expresa que, para cada valor de $\alpha$, el conjunto de valores de $\beta$ es **independiente** del conjunto de valores de $R - \alpha - \beta$.

**Definicion formal**: $\alpha \twoheadrightarrow \beta$ si para todo par de tuplas $t_1, t_2$ en $r$ con $t_1[\alpha] = t_2[\alpha]$, existen tuplas $t_3, t_4$ en $r$ tales que:
- $t_3[\alpha] = t_4[\alpha] = t_1[\alpha]$
- $t_1[\beta] = t_3[\beta]$ y $t_2[\beta] = t_4[\beta]$
- $t_1[R - \alpha\beta] = t_4[R - \alpha\beta]$ y $t_2[R - \alpha\beta] = t_3[R - \alpha\beta]$

**Definicion intuitiva** (equivalente): al **intercambiar los valores de $\beta$** entre dos tuplas que coinciden en $\alpha$, las tuplas resultantes tambien pertenecen a la relacion.

- **MVD trivial**: $\alpha \twoheadrightarrow \beta$ es trivial si $\beta \subseteq \alpha$ o $\alpha \cup \beta = R$
- **Toda FD es MVD**: si $\alpha \to \beta$ entonces $\alpha \twoheadrightarrow \beta$ (por replicacion, axioma 4)

**Axiomas para MVDs** (sistema completo y correcto):

| Axioma | Nombre | Enunciado |
|---|---|---|
| Ax1 | Complementacion | $X \twoheadrightarrow Y \Rightarrow X \twoheadrightarrow R - XY$ |
| Ax2 | Aumentacion | $X \twoheadrightarrow Y, V \subseteq W \Rightarrow XW \twoheadrightarrow YV$ |
| Ax3 | Transitividad | $X \twoheadrightarrow Y, Y \twoheadrightarrow Z \Rightarrow X \twoheadrightarrow Z - Y$ |
| Ax4 | Replicacion (FD -> MVD) | $X \to Y \Rightarrow X \twoheadrightarrow Y$ |
| Ax5 | Coalescencia (MVD + FD -> FD) | $X \twoheadrightarrow Y, Z \subseteq Y, W \cap Y = \emptyset, W \to Z \Rightarrow X \to Z$ |

> [!important] Los axiomas 4 y 5 conectan FDs y MVDs. Ax4 permite inferir nuevas MVDs desde FDs. Ax5 permite inferir nuevas **FDs** desde una MVD y una FD combinadas -- necesario para encontrar claves cuando hay MVDs.

**Definicion de 4NF**: un esquema $R$ esta en **cuarta forma normal** si para toda $\alpha \twoheadrightarrow \beta$ no trivial de $F^+$, $\alpha$ es superclave de $R$.

$$4\text{NF} \Rightarrow \text{BCNF} \Rightarrow 3\text{NF}$$

**Algoritmo de descomposicion en 4NF:**

1. Inicializar $\varphi = \{R\}$
2. Calcular $F^+$ (incluyendo MVDs)
3. Mientras exista $R_i$ con $\alpha \twoheadrightarrow \beta$ que viole 4NF:
   1. Generar $R_B = (\alpha, \beta)$
   2. Generar $R_i' = R_i - \beta$
   3. Reemplazar $R_i$ por $R_B$ y $R_i'$
4. La descomposicion final es $\varphi$

**Propiedad de junta sin perdida para MVDs**: la descomposicion en $R_1$ y $R_2$ es sin perdida si:

$$(R_1 \cap R_2) \twoheadrightarrow (R_1 - R_2) \quad \text{o bien} \quad (R_1 \cap R_2) \twoheadrightarrow (R_2 - R_1) \quad \in D^+$$

> [!tip] 4NF formaliza la intuicion de que **datos independientes entre si no deben estar en la misma tabla**. Si un curso tiene una lista de profesores y una lista de textos que son independientes, mezclarlos en una sola tabla genera un producto cartesiano innecesario.

### Algoritmo de base de dependencias -- BaseDep(X)

Permite determinar todas las MVDs que dependen de un conjunto $X$. Genera una **particion** de los atributos de $R$ donde cada subconjunto es la parte derecha de una MVD.

1. $\text{BaseDep}(X) = \{R - X\}$
2. Mientras haya cambios en $\text{BaseDep}(X)$:
   - Para toda MVD $V \twoheadrightarrow W$ con $Y \in \text{BaseDep}(X)$ tal que $Y \cap V = \emptyset$ pero $Y \cap W \neq \emptyset$:
     - $\text{BaseDep}(X) = \text{BaseDep}(X) - \{Y\} \cup \{Y \cap W\} \cup \{Y - W\}$
3. Para todo atributo $A \in X$: $\text{BaseDep}(X) = \text{BaseDep}(X) \cup \{A\}$

**Uso**: si $Z \in \text{BaseDep}(X)$, entonces $X \twoheadrightarrow Z$. Ademas, $X$ multivalua cualquier **union** de elementos de $\text{BaseDep}(X)$ (por regla de union).

> *Resuelve el problema de pertenencia de MVD: para saber si $X \twoheadrightarrow Z \in F^+$, calcular BaseDep(X) y verificar si Z es combinacion de sus elementos.*

### Dependencias de junta y 5NF

Una **dependencia de junta** $J(R_1, R_2, \ldots, R_N)$ se satisface si $R$ es igual a la junta natural de sus proyecciones segun cada $R_i$.

- **JD trivial**: existe algun $R_i = R$
- **JD binaria** $J(S_1, S_2)$ equivale a una MVD: $(S_1 \cap S_2) \twoheadrightarrow (S_1 - S_2)$

**5NF**: un esquema esta en quinta forma normal si para toda JD $J(R_1, \ldots, R_n)$ no trivial de $F^+$, cada $R_i$ es superclave de $R$.

$$5\text{NF} \Rightarrow 4\text{NF} \Rightarrow \text{BCNF} \Rightarrow 3\text{NF}$$

> [!tip] En la practica, **generalmente se normaliza hasta BCNF o 4NF**. Detectar dependencias de junta no triviales con mas de dos componentes requiere gran intuicion y es poco frecuente en esquemas reales.

### Tableau y chase algorithm

Un **tableau** es una matriz con una columna por atributo. Usa:
- **Variables distinguidas** $a_j$: indican que el valor es conocido/relevante (columna $j$)
- **Variables ligadas** $b_{ij}$: indican que el valor es desconocido (fila $i$, columna $j$)

El **chase** es la aplicacion iterativa de las reglas de transformacion hasta llegar a un punto fijo. El resultado es unico independientemente del orden de aplicacion.

**Reglas de transformacion:**

- **Regla DF**: si dos filas coinciden en $X$ y la DF es $X \to A$, igualar los valores de la columna $A$ (preferir variables distinguidas)
- **Regla DJ**: si para una JD $J(S_1, \ldots, S_k)$ existen filas $w_1, \ldots, w_k$ que permiten construir una fila $w$ nueva con $w[S_i] = w_i[S_i]$ para todo $i$, agregar $w$

**Usos del tableau como plantilla:**

| Objetivo | Construccion | Criterio de exito |
|---|---|---|
| Clausura $X^+$ | $T_X$ con 2 filas: fila 1 toda distinguida, fila 2 distinguida solo en $X$ | Las columnas con solo variables distinguidas forman $X^+$ |
| MVDs de $X$ | Mismo $T_X$ | Cada fila con variables distinguidas en un conjunto $Z$ (con $Z \cap X^+ = \emptyset$) indica $X \twoheadrightarrow Z$ |
| Verificar JD | $T_J$ con una fila por subconjunto de $J$, distinguida en sus atributos | Si aparece una fila completamente distinguida, la JD se infiere |

> [!important] El chase siempre termina y el resultado es **unico** (independiente del orden). Es la herramienta formal para verificar si una descomposicion tiene junta sin perdida: construir el tableau de la descomposicion y aplicar las DFs; si alguna fila queda completamente distinguida, la descomposicion es lossless.

## Notas

- La jerarquia completa de formas normales es: $5\text{NF} \Rightarrow 4\text{NF} \Rightarrow \text{BCNF} \Rightarrow 3\text{NF} \Rightarrow 2\text{NF} \Rightarrow 1\text{NF}$
- En la practica, el **trade-off central** es entre BCNF (maxima eliminacion de redundancia por FDs) y 3NF (conservacion de dependencias). Si BCNF no pierde DFs, elegirla; si las pierde, evaluar el impacto antes de renunciar a 3NF
- El modelado de datos es "mas un arte que una ciencia": un buen DER produce esquemas normalizados sin necesidad de post-correccion. Los errores tipicos son omitir relaciones, confundir entidades, o no detectar atributos multivaluados
- Las herramientas CASE pueden generar hasta 3NF automaticamente, pero dependen de la calidad del DER de entrada

## Preguntas

- En el algoritmo de BCNF, si hay multiples DFs violadoras, como elegir la "mejor" para minimizar la perdida de dependencias? Existe heuristica?
- El chase algorithm puede tener complejidad exponencial en el peor caso. En la practica, cuantas iteraciones suelen necesitarse?
- Si la descomposicion en BCNF pierde una DF, se puede detectar automaticamente con el test de preservacion antes de comprometerse con la descomposicion?
- Para 4NF, como se descubren las MVDs en un esquema real? Provienen del DER o hay que inferirlas del dominio?
