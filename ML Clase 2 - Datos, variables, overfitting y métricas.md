---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-1216:14
Materia: "[[Machine Learning.base|Machine Learning]]"
temas:
---
# Datos, variables, overfitting y métricas

del la clase anterior

"Aprender si ser programado"
Datos y etiquetas, busca patrones y aprende

## como es un dataset*?
![](Attachments/Pasted%20image%2020260812162710.png)
### ejemplo
![](Attachments/Pasted%20image%2020260812162721.png)

## Tipos de variables
### Numericas
cantidades
Son las más comunes y pueden utilizarse directamente en la mayoría de los modelos.
### Numericas discretas
no existe continuidad entre ellos, generan relaciones no lineales o efecto umbral

## Categoricas
categorias o etiquetas
### Transformaciones
un enfoque es asignar un numero a cada categoria **(encoding ordinal)**
impone una jerarquia (0, 1, 2, 3...)
mas facil pero no mas conveniente

**one-hot encoding**: crea una variable nueva para cada categoria. funciona como un flag
si son muchas categorias son muchas opciones (malo)

**Frequency encoding**: cada categoria se la remplaza por la frecuencia de aparicion

**Target encoding**: Cada categoria toma el valor del promedio de Y para los datos de esa categoria
es basado en el objetivo (el usuario hace Y cosa en X categoria?)
$$
TE_c = \bar{y_c}
$$
$$
TE_c=\frac{n_c\bar{y_c}+\lambda \bar{y}}{n_c+\lambda}
$$
**lambda**: parámetro regularizador “añado lambda observaciones virtuales que tienen de media la media global”

**Modelos que "soportan este tipo de variables"**

## Limpieza de datos
antes de entrenar el modelo es necesario revisar y limpiar los datos para evitar errores oresultados engañosos

- valores faltantes erroneos
- outliers
- datos duplicados

### Fixes
- quitar todo el dato
- quitar variable
- imputar valor
Imputacion de valores en base a:
- estadisticas globales (media, mediana, moda): estoy inventando la informacion
- estadisticos por grupos: igual al global pero por grupo
- Valor constante: cuando creemos que es relevente que falte el dato
- Basado en modelos
### Outliers - impacto:
Pueden afectar gravemente al modelo:
- Distorsionan estadísticas (media, desviación estándar…)
- Sesgan el entrenamiento: algunos modelos intentan ajustarse a esos valores extremos.
- Algunos algoritmos son especialmente sensibles

 >[!important] obs
 >No tienen por que ser erróneos, pero sí pueden ser ejemplos no representativos en el dataset y por tanto poco útiles para el modelo y quizás confusos

### Outliers - Deteccion

**metodos visuales**
![](Attachments/Pasted%20image%2020260812170550.png)

**metodos estadisticos**
- IQR
- Z-score
![](Attachments/Pasted%20image%2020260812170635.png)

que hacer con ellos?
- eliminar dato
- transformacion estadistica pueden reducir su efecto

# Regresion 
![](Attachments/Pasted%20image%2020260812170928.png)![](Attachments/Pasted%20image%2020260812171029.png)

## overfitting y underfitting
![](Attachments/Pasted%20image%2020260812171243.png)

### como evitar overfitting?
añadiendo multiples caracteristicas se llega a overfitting

# Data Splits y learning Theory

## Data split
queremos que funcione bien con nuevos datos

![](Attachments/Pasted%20image%2020260812173744.png) 
## Train vs Test
![](Attachments/Pasted%20image%2020260812173823.png)
### Error de prediccion esperado
![](Attachments/Pasted%20image%2020260812174140.png)

## Train vs Dev vs Test
![](Attachments/Pasted%20image%2020260812174745.png)
### Cross validation
![](Attachments/Pasted%20image%2020260812175346.png) 
![](Attachments/Pasted%20image%2020260812180454.png)


# Resumen

Resumen extenso de la clase 2: qué forma tienen los datos, cómo se preparan, el primer
modelo supervisado (regresión), el problema central de generalización (overfitting) y la
maquinaria para medirlo (data splits + learning theory).

**Hilo conductor de la clase**: los datos entran crudos → se codifican y se limpian → se
entrena un modelo → el modelo puede memorizar en vez de aprender → se separan datos para
detectarlo → la teoría explica *por qué* pasa (ruido) y *cuántos* datos hacen falta.

---

## 1. Punto de partida: el dataset supervisado

Tenemos $n$ datos, cada uno un par (features, label):

$$
D_n = \{(x^{(1)}, y^{(1)}), \dots, (x^{(n)}, y^{(n)})\}
$$

- $x^i = (x^i_1, \dots, x^i_d)$ → **vector de características (features)**: las variables que
  describen al dato (noches, adultos, país, canal de reserva…).
- $y^i$ → **etiqueta (label / target)**: lo que queremos predecir (¿canceló la reserva?).

En **supervisado** tenemos $y$ y queremos una estrategia para etiquetar datos nuevos.
En **no supervisado** no hay $y$: buscamos patrones que agrupen los datos de la mejor
forma posible.

Según cómo sea el target, el problema supervisado se parte en dos:

| Target | Problema |
| --- | --- |
| discreto / categórico | **Clasificación** |
| continuo | **Regresión** |

**Ejemplo que atraviesa toda la clase**: una cadena hotelera quiere predecir si una reserva
va a ser cancelada (clasificación) y, más adelante, estimar el precio total de una reserva
en función de las noches (regresión).

---

## 2. Tipos de variables y cómo transformarlas

### 2.1 Numéricas

Representan **cantidades**. Son las más comunes y **pueden usarse directamente** en la
mayoría de los modelos (precio por noche, importe total, días de anticipación).

### 2.2 Numéricas discretas

Caso especial: sólo toman **valores específicos** (enteros), a diferencia de las continuas
que pueden tomar cualquier valor de un rango (noches, adultos, niños, cancelaciones previas).

- Se pueden usar directamente como numéricas porque representan cantidades reales.
- **Pero** no hay continuidad entre valores → pueden generar **relaciones no lineales o
  efectos umbral** que algunos modelos no capturan bien.
- Suelen tener **rangos chicos y distribuciones muy concentradas**, lo que limita la
  información que aportan.
- Recomendación de la cátedra: probar **modelos no lineales**, o pasarlas a **categóricas /
  binarias** (ej: `>3 habitaciones`).

### 2.3 Categóricas

Representan **categorías o etiquetas** (país, tipo de habitación, canal de reserva, tipo de
cliente). La mayoría de los algoritmos requieren entradas numéricas → **hay que
transformarlas**.

#### Encoding ordinal

Asignar un número a cada categoría: Web→1, Agencia→2, Teléfono→3.

- Es la primera aproximación y la más simple.
- **Impone una jerarquía** (0 < 1 < 2 < 3) → sólo es válido cuando esa relación de orden
  **existe de verdad**.

#### Categóricas ordinales

Son categóricas donde **sí existe relación jerárquica/de orden**: categoría del hotel
(3★ < 4★ < 5★), nivel de fidelidad (Bronze < Silver < Gold).
→ Su transformación natural es justamente el **encoding ordinal**.

> [!warning] La pregunta clave
> ¿Y si **no** existe esa relación jerárquica? (España vs Francia vs Italia).
> Ahí el encoding ordinal le miente al modelo: le dice que Italia "vale más" que España.
> Para esos casos están los tres encodings siguientes.

#### One-hot encoding

Crea **una variable nueva por cada categoría**, que funciona como flag 0/1.

| Cliente | Canal | Canal_Web | Canal_Agencia | Canal_Teléfono |
| --- | --- | --- | --- | --- |
| C1 | Web | 1 | 0 | 0 |
| C2 | Agencia | 0 | 1 | 0 |
| C5 | Teléfono | 0 | 0 | 1 |

- Muy utilizado, no impone orden falso.
- **Contra**: aumenta muchísimo la dimensionalidad. Está bien si **no** hay muchas variables
  categóricas ni muchas categorías por variable.

#### Frequency encoding

Cada categoría toma el valor de su **frecuencia de aparición** (Web→0.62, Agencia→0.11,
Teléfono→0.17).

- **Idea detrás**: puede ser útil que el algoritmo sepa que un dato pertenece a una
  **categoría muy rara**, porque las categorías raras suelen comportarse distinto.
- No aumenta la dimensionalidad (1 columna).
- Los métodos **basados en árboles** aprovechan muy bien este tipo de variable, porque
  "dividen" el dataset mirando variables individuales.

#### Target encoding

Cada categoría toma el **promedio de $y$** de los datos de esa categoría:

$$
TE_c = \bar{y}_c
$$

- Interpretación: *cómo se comportan en promedio los datos de esa categoría*.
- **Peligro**: si hay **pocos datos** de esa categoría (o uno solo), el promedio puede ser
  directamente la $y$ que queremos predecir → fuga de información / estimación inestable.

#### Regularized target encoding

Corrige el problema anterior **ponderando con la frecuencia de aparición**:

$$
TE_c = \frac{n_c \bar{y}_c + \lambda \bar{y}}{n_c + \lambda}
$$

- $n_c$: cantidad de datos de la categoría; $\bar{y}$: media global.
- **$\lambda$**: parámetro regularizador. Lectura intuitiva: *"añado $\lambda$ observaciones
  virtuales que tienen de media la media global"*.
- Comportamiento: si la categoría está **poco presente** → tiende a la **media global**;
  si está **muy presente** → tiende al **promedio de esa categoría**.
- En scikit-learn: `TargetEncoder(smooth="auto")`.

#### Modelos que soportan variables categóricas

Hay modelos **explícitamente diseñados** para usar categóricas sin encodearlas a mano:
**CatBoost, LightGBM, XGBoost**.

> [!tip] Cómo elegir encoding
> - ¿Hay orden real? → **ordinal**
> - ¿Pocas categorías, sin orden? → **one-hot**
> - ¿Muchas categorías y me importa lo raro? → **frequency**
> - ¿Muchas categorías y quiero señal del target? → **target (regularizado)**
> - ¿Uso boosting sobre árboles? → dejar que el modelo las maneje

---

## 3. Limpieza de datos

Antes de entrenar hay que **revisar y limpiar** los datos para evitar errores o resultados
engañosos. Los tres problemas típicos:

1. **Valores faltantes / erróneos**
2. **Outliers**
3. **Datos duplicados**

### 3.1 Valores faltantes y erróneos

Cuatro cosas distintas que aparecen en el ejemplo del hotel:

| Problema | Ejemplo | Cómo se detecta |
| --- | --- | --- |
| Valores faltantes | `NA` en días de anticipación, adultos, precio, país | conteo de nulos por columna |
| Valores imposibles | `noches = 0`, `adultos = 0`, `precio = -50` | rangos válidos por variable |
| Categorías erróneas | `"online"` vs `"Web"`, `"Wb"` | `df["canal"].unique()` |
| Valores atípicos | reserva con **200** días de anticipación | boxplots, IQR |

### 3.2 Qué hacer con los faltantes

- **Quitar todo el dato (fila)** → si el dataset **no es muy grande, mejor evitarlo**
  (se pierde información de las demás columnas).
- **Quitar la variable (columna)** → si el **% de faltantes en esa columna es alto (~20%)**.
- **Imputar un valor**.

> [!important] Orden correcto
> Antes de imputar o eliminar filas, mirar **primero** si la variable tiene un porcentaje
> alto de missing: si la columna está mayormente vacía, no tiene sentido imputar dato por dato.

### 3.3 Imputación de valores

| Método | Cómo | Cuándo / observaciones |
| --- | --- | --- |
| **Estadísticos globales** | media, mediana, moda de toda la columna | lo más simple; se está *inventando* información y se achica la varianza |
| **Estadísticos por grupo** | ej: precio medio **por tipo de habitación** | mejor que el global, **pero** debe existir relación real entre las dos variables/categorías |
| **Valor constante** | poner 0 / "Desconocido" | útil cuando **el hecho de que falte es relevante** para el modelo |
| **Basado en modelos** | modelar el valor más probable usando todo el dataset (ej: **KNN**) | el **más preciso**, pero **costoso computacionalmente** y con **posible data leakage** |

### 3.4 Outliers

**Impacto** — pueden afectar gravemente al modelo:

- **Distorsionan estadísticas** (media, desviación estándar…).
- **Sesgan el entrenamiento**: algunos modelos intentan ajustarse a esos valores extremos.
- **Algunos algoritmos son especialmente sensibles**.

> [!important] Observación
> **No tienen por qué ser erróneos**, pero sí pueden ser ejemplos **no representativos** del
> dataset y, por lo tanto, poco útiles para el modelo y quizás confusos.

**Detección — métodos visuales**: boxplot (ya marca outliers con estadística basada en IQR),
histograma, scatter plot.
*Nota de la cátedra*: los métodos visuales generalmente **necesitan un estadístico que
justifique** la eliminación del outlier en un paper o un trabajo importante.

**Detección — métodos estadísticos**:

- **Basado en IQR** (IQR = diferencia entre percentil 75 y percentil 25). Es outlier si:

$$
x < Q1 - 1.5 \cdot IQR \qquad \text{ó} \qquad x > Q3 + 1.5 \cdot IQR
$$

- **Basado en Z-score**. Es outlier si $|z| > 3$, con:

$$
z = \frac{x - \mu}{\sigma}
$$

donde $x$ = valor observado, $\mu$ = media, $\sigma$ = desviación estándar.

**Qué hacer con ellos**:

- **Eliminar el dato**: si no son errores sino un dato raro (ej: *un atleta olímpico en un
  dataset de una app de salud*).
- Algunas **transformaciones estadísticas** pueden **reducir su efecto**.
- Usar **modelos más robustos a outliers**, que se vean menos afectados.
- *Regla práctica*: si son outliers y se detectan de forma robusta, **lo más seguro es
  eliminar el dato**.

### 3.5 Checklist de limpieza (tabla de la clase)

| Paso | Qué revisar | Qué hacer | Ejemplo (hotel) |
| --- | --- | --- | --- |
| Tipos de variables | que cada columna tenga el tipo correcto | convertir tipos | precio numérico, país categórico |
| Duplicados | registros repetidos | eliminarlos | misma reserva cargada dos veces |
| Valores faltantes | columnas con NA | imputar o eliminar registros | precio, país, adultos con NA |
| Rangos válidos | valores imposibles | corregir o eliminar | precio < 0, noches = 0 |
| Outliers | valores extremos | eliminar, corregir o transformar | reserva con 200 días de anticipación |
| % de missing | gravedad del problema | si es alto, eliminar la variable | precio falta en muchas filas |
| Categorías existentes | categorías válidas | unificar/limpiar etiquetas | "web" vs "Web" |
| Frecuencias categóricas | distribución de categorías | detectar clases muy raras o errores | cuántas reservas por canal |

---

## 4. Primer modelo supervisado: Regresión

### 4.1 Idea básica

En regresión el objetivo es **predecir una variable numérica continua**. El modelo intenta
aprender una función que relacione las entradas con el target:

$$
y = f(x) \qquad \text{ej: } precio = f(noches)
$$

### 4.2 Regresión lineal

**Asume** que la relación entre las variables puede aproximarse por una **combinación lineal**
de las features:

$$
y = \beta_0 + \beta_1 x
$$

- $y$: valor a predecir (precio)
- $x$: variable de entrada (noches)
- $\beta_0$: **intercepto**
- $\beta_1$: **pendiente** (cuánto sube el precio cuando suben las noches)

### 4.3 Ajuste del modelo

Durante el entrenamiento el modelo ajusta $(\beta_0, \beta_1)$ para **minimizar el error**
entre predicciones y valores reales. Generalmente vía **MSE**:

$$
MSE = \frac{1}{n}\sum (y_i - \hat{y}_i)^2
$$

Las diferencias $(y_i - \hat{y}_i)$ son los **residuos**. Una vez ajustada la recta, predecir
es evaluar: dado $x_1$, se lee $y_1$ sobre la recta.

### 4.4 Regresión polinómica

En muchos casos la relación **no es lineal** y puede aproximarse con transformaciones como
**términos polinómicos**. La regresión polinómica **extiende** la lineal agregando términos
de mayor grado:

$$
y = w_1x + w_2x^2 + w_3x^3 + \dots + b
$$

**Aumentar el grado del polinomio incrementa la complejidad del modelo** y su capacidad de
ajustarse a los datos… lo cual lleva directo al problema siguiente.

---

## 5. Underfitting y overfitting

Un modelo **demasiado simple** puede **no capturar la relación real** (*underfitting*),
mientras que uno **demasiado complejo** puede **ajustarse demasiado a los datos de
entrenamiento** (*overfitting*).

![](Attachments/Pasted%20image%2020260812171243.png)

| | Underfitting | Buen ajuste | Overfitting |
| --- | --- | --- | --- |
| Modelo típico | $w_1x + b$ | $w_1x + w_2x^2 + b$ | $w_1x + w_2x^2 + w_3x^3 + w_4x^4\dots + b$ |
| Nombre alternativo | **alto sesgo** (*high bias*) | — | **alta varianza** (*high variance*) |
| Síntoma | error alto en train **y** en test | error bajo y parecido en ambos | error muy bajo en train, alto en test |

Lo mismo aplica a **clasificación**: la frontera de decisión pasa de ser una recta que no
separa bien (underfitting), a una curva razonable, a una frontera retorcida que rodea cada
punto (overfitting).

### 5.1 Por qué aparece

Si añadimos **suficientes features (x)**, eventualmente podemos reducir el error al máximo,
porque **cada $w$ se podría "usar" para modelar cada datapoint**. Además, al agregar
dimensiones los datos quedan cada vez **más dispersos** en el espacio (de una recta llena de
puntos, a un plano con huecos, a un cubo casi vacío) → hay más "lugar" para que el modelo
pase exactamente por cada punto.

![](Attachments/ML%20clase2%20-%20dispersion%20al%20agregar%20dimensiones.png)
*Los mismos ~15 puntos en 1D, 2D y 3D: al agregar features el espacio se vacía y sobra
lugar para que el modelo pase justo por cada dato.*

### 5.2 Cómo evitarlo (mapa de la clase)

| Estrategia | Detalle | Cuándo se ve |
| --- | --- | --- |
| **Aumentar la cantidad de datos de entrenamiento** | Caro y muchas veces imposible | (sección "¿Cuántos datos?") |
| **Reducir la cantidad de características (x)** | → **Regularización**, → **Selección de características** | **Clase siguiente** |
| **Evaluar el modelo en datos nuevos** (mide capacidad de generalización) | → **Data Splits** | **Esta clase** |

---

## 6. Data Splits

**Overfitting es que el modelo no generalice bien a datos nuevos.**
¿Cómo saber si generaliza? → **teniendo datos nuevos**: separar una parte de los datos para
evaluar. Se entrena el algoritmo en una parte y se deja el resto **sin usar** para evaluar.

### 6.1 Train vs Test — cómo separarlos

El conjunto de **train** debe:

- Ser **lo suficientemente grande** para poder entrenar el modelo.
- **Representar el problema**.

El conjunto de **test** debe:

- Ser **totalmente independiente**.
- **Representar los datos reales** en los que el modelo se va a usar (que deberían ser como
  los de train).
- No hace falta que sea tan grande como el train, pero **sí tener suficientes datos para
  representar la muestra**.

### 6.2 El problema del test: sesgo optimista

Si elegimos el modelo con **menor error en test**, evitamos overfittear al train… **pero**:

> El error en test **del modelo elegido en test** es **optimista**, ya que ha sido elegido
> por funcionar mejor **en esos datos**. Si modela mejor el **ruido del test**, no por eso
> funcionará mejor en datos nuevos.

Ejemplo de la clase: con 4 modelos evaluados en Datos1 y Datos2, elegimos el **Modelo 1**
(7.2% / 7.0%) porque es el que mejor va en Datos2 — pero ese 7.0% **ya no es una estimación
insesgada**, porque Datos2 dejó de ser "datos nuevos" en el momento en que se usó para elegir.
(Notar que el Modelo 3 tiene el mejor error en Datos1 (4.4%) y el peor en Datos2 (8.8%):
overfitting de manual.)

**Solución**: necesitamos **otra partición** — una para **elegir** el modelo con datos nuevos
y otra para **estimar el error** en otros datos nuevos → **Validation set (o Dev set)**.

### 6.3 Train vs Dev vs Test

| Partición | Para qué |
| --- | --- |
| **Train** | Entrenar los modelos |
| **Dev** (validation) | **Elegir** modelo / hiperparámetros (controlar overfitting) |
| **Test** | **Estimar de forma insesgada** el error en datos nuevos |

### 6.4 Proporciones

| Tamaño del dataset | Split sugerido | Comentario |
| --- | --- | --- |
| Pequeño-mediano (~200-500 datos) | **60-20-20** | buena regla general. **En esta materia nos vamos a mover en este rango** |
| Muy grande (DL, millones) | **90-5-5** o incluso **98-1-1** | depende de la **resolución** necesaria en dev/test: si necesito 0.05% de resolución, dev y test necesitan más datos |
| Muy pequeño (~100 datos) | — | 20 ejemplos para dev y para test puede ser **insuficiente** (a veces incluso los 60 de train) → **cross validation** |

### 6.5 Cross Validation (k-fold)

- **Recomendable para datasets pequeños** (por ejemplo, salud).
- **Computacionalmente más costoso**, pero con datasets pequeños no es un problema grande, y
  permite **usar los datos de manera más eficiente**.
- **k = 5 o 10** son las más usadas y son suficientes.
- Mecánica: se parte el train en k folds; en cada iteración uno hace de dev y el resto de
  train; se obtienen k errores (7.8, 7.6, 8.2, 6.9, 7.5) y se **promedia** (8.0) para decidir.
- El **test queda afuera** de todo el procedimiento.

![](Attachments/Pasted%20image%2020260812175346.png)

**Leave-one-out**: caso extremo donde cada fold es **un solo dato**.

- Útil para datasets **muy pequeños** (menos de ~80 ejemplos).
- **Aún más costoso computacionalmente** que k-fold.

### 6.6 ¿Qué modelo se entrega?

Ya elegido el mejor modelo: **ese modelo se re-entrena con TODO el train+dev**, y ese es el
que se entrega/implementa y **se evalúa en test**.

### 6.7 Reglas del Test set

- **Sólo se usa para estimar el error de predicción en datos nuevos, nunca para tomar
  decisiones.**
- Lo más seguro es usarlo **sólo al final del proyecto**, para reportar resultados al
  jefe/a, publicar un paper, etc.

**Excepciones (avanzado)**:

- Si sólo necesitamos **elegir** el mejor modelo pero no saber qué tan bien va a funcionar,
  se podría hacer sólo **train-dev** y así usar todos los datos.
  **Pero sabiendo que el error en dev del modelo ganador va a estar optimistamente sesgado.**
- Se puede usar durante el proyecto si únicamente se **reporta** el valor a otra
  persona/jefe/parte del proyecto. En ese caso, ser extra cuidadosos con **no elegir un
  modelo por el resultado de test**, y con **no dejar de probar modelos cuando el test sea
  suficientemente bueno**.

---

## 7. Learning Theory

### 7.1 Formulación

Queremos modelar la relación entre unas variables y un fenómeno concreto:

$$
y = f(x_{real}) + \epsilon
$$

- $x_{real}$: variables **reales** que explican $y$
- $f(\cdot)$: función **real** que relaciona $x$ e $y$
- $\epsilon$: **ruido** del dataset / todo lo demás → **todo lo que no se puede modelar**

Al entrenar, el modelo resuelve:

$$
\min_h \hat{E}_{train}(h) \qquad \text{con} \qquad \hat{E}_{train}(h) = E(h) + \varepsilon
$$

> [!warning] La clave de todo
> El modelo aprende a minimizar el error $E(h)$… **pero también el error en el ruido** $\varepsilon$.
> Eso *es* el overfitting: bajar el error de train explotando el ruido particular de esos datos.

### 7.2 Tres tipos/fuentes de ruido

1. **Aleatoriedad del mundo real**: errores de medición, eventos aleatorios… **irreducible**.
2. **Ruido de muestreo**: al trabajar con **subconjuntos** de los datos, estos tienen pequeños
   sesgos y varianzas — ruido producido al muestrear (**sería 0 para infinitos datos**).
3. **Información relevante no incluida** (variables): si el modelo no tiene toda la información
   necesaria para modelar la relación entre $x$ e $y$, **verá parte de los cambios de $y$ como
   ruido**, ya que nunca podrá explicarlos.

### 7.3 Ejemplo de falta de información como ruido

Queremos predecir $y$ = **índice de masa corporal (IMC)** a partir de
$x$ = (marca de celular, dinosaurio favorito). ¿Qué ocurre?

- $f(x)$ **no tiene relación con $y$**: será prácticamente una constante, casi imposible de
  minimizar.
- Al minimizar el error en train, **la única forma será minimizar el error en $\epsilon$**:
  el modelo intentará explicar la varianza de $y$ como ruido, **sin usar las $x$**.
- Cuando lleguen **datos nuevos**, los patrones del ruido cambiarán y **el error volverá a
  ser alto**.

### 7.4 Generalization gap

$$
\text{generalization gap} = E_{test}(h) - E_{train}(h)
$$

- Si $E_{test}(h) \approx E_{train}(h)$: el modelo redujo $\hat{E}_{train}(h)$ **principalmente
  reduciendo $E(h)$**, no aprovechando el ruido $\epsilon$ → **generaliza bien**.
  (El error en test y en train se deben a los $\epsilon$ y a lo que no se haya podido reducir de $E(h)$.)
- Si el modelo **aprende/modela ruido del train** → **el gap crece**.
- Si el gap es **cero**, el modelo generaliza bien.

---

## 8. ¿Cuántos datos hacen falta?

**Regla práctica: la "regla del 10×"** → **10 veces más ejemplos que parámetros**
(*degrees of freedom*).

- Ejemplo: 10 variables/features → se necesitan ~100 ejemplos.
- Funciona como aproximación **para modelos pequeños**.
- **Limitación**: en modelos grandes (deep learning) no basta con contar filas; también importa
  el **tamaño y complejidad de cada input**.

Referencias concretas:

- **Deep learning**: Goodfellow, Bengio et al. recomiendan **5000 ejemplos por clase** en un
  DL supervisado.
- **Visión por computadora**: **1000 imágenes por clase** puede ser un buen punto de partida.
- **Importante**: buscar **ejemplos similares de proyectos** para ver cuántos datos usaron.

### Curvas de aprendizaje: ¿serían útiles más datos?

Empezar con una cantidad razonable de datos y **evaluar la performance para distintas
cantidades** (ej: 50%, 60%, … 100% de los datos que tenemos) y ver si agregar datos
aumentaría el resultado.

- Curva que **hace plateau temprano** (se aplana): **más datos no aportan** → conviene
  cambiar de modelo/features.
- Curva que **sigue mejorando** en el último tramo: **grabar más datos sí sería útil**.
- Curva intermedia: mejora lenta, decisión de costo/beneficio.

![](Attachments/ML%20clase2%20-%20curvas%20de%20aprendizaje.png)
*La naranja ("sigue mejorando") es la que justifica salir a recolectar más datos; la azul
ya hizo plateau y con más datos no va a mejorar.*

---

## 9. Pipeline completo y data leakage

Recap del flujo de un proyecto y **dónde** se hacen las separaciones:

```
1. Recolección de datos
   ──────────────────────────── ← SEPARAR TEST ACÁ
2. Limpieza de datos (outliers, duplicados, imputar…)
3. Convertir a tipos de variables que sepamos usar (numéricas)
4. Evaluar features y elegir las mejores (o todas)
5. Elegir modelos que se van a evaluar
   ──────────────────────────── ← SEPARAR DEV ACÁ (en la práctica)
6. Entrenar modelos
7. Elegir el mejor modelo          → mirando error en Dev
8. Implementar el mejor modelo y estimar su error en datos nuevos → calculando error en Test
```

> [!danger] Data leakage
> Si quitamos outliers e imputamos valores usando **TODO el dataset**, información que luego
> irá al test **estará siendo usada para train**. El test deja de ser independiente y la
> estimación del error queda contaminada (optimista).

- **Separar Test justo después de recolectar**: idealmente **automatizar** toda la limpieza,
  imputación y conversión de tipos, para que a los datos de test **se les aplique el mismo
  procesamiento** (y también al dev), pero **ajustado sólo con train**.
- **Separar Dev**: idealmente se separaría **a la vez que el test**, pero en la práctica a
  veces se separa **justo antes de entrenar** por simplicidad.
- Recordar que la **imputación basada en modelos (KNN)** es la más propensa a leakage.

---

## 10. Puntos clave para el parcial

1. **Ordinal encoding sólo si hay orden real**; si no, one-hot / frequency / target.
2. **Target encoding sin regularizar es peligroso** con categorías poco frecuentes;
   $\lambda$ = observaciones virtuales con la media global.
3. **Quitar la columna** si el missing es alto (~20%); **quitar la fila** sólo si el dataset
   es grande.
4. **Outliers**: IQR ($Q1 - 1.5\,IQR$, $Q3 + 1.5\,IQR$) y Z-score ($|z|>3$). No son
   necesariamente errores, pero suelen ser no representativos.
5. **Underfitting = alto sesgo**; **overfitting = alta varianza**.
6. **Tres formas de atacar el overfitting**: más datos (caro), menos features
   (regularización/selección — clase siguiente), evaluar en datos nuevos (data splits — hoy).
7. **Dev elige, Test estima.** El error del modelo elegido *en el conjunto donde se lo eligió*
   es **optimistamente sesgado**.
8. **60-20-20** en datasets chicos-medianos; **90-5-5 / 98-1-1** en datasets enormes;
   **cross validation (k=5 o 10)** cuando hay pocos datos; **leave-one-out** con <80 ejemplos.
9. El modelo final se **re-entrena con train+dev** y se evalúa una sola vez en test.
10. $y = f(x_{real}) + \epsilon$ y $\hat{E}_{train}(h) = E(h) + \varepsilon$: el overfitting es
    minimizar $\varepsilon$ en lugar de $E(h)$. **Generalization gap** = $E_{test} - E_{train}$.
11. **Tres fuentes de ruido**: aleatoriedad real (irreducible), muestreo (→0 con infinitos
    datos), información relevante faltante.
12. **Regla del 10×**; DL ~5000 ejemplos/clase; visión ~1000 imágenes/clase.
13. **Separar el test lo antes posible** para evitar **data leakage** en limpieza e imputación.

---

> [!note] Pendiente
> El título de la clase menciona **métricas**, pero el material se detiene en data splits y
> learning theory. **Regularización y selección de características** quedan explícitamente
> para la clase siguiente.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Machine Learning)**

- [ML Clase 3 - EDA, Feature selection, Regularización y Métricas](ML%20Clase%203%20-%20EDA,%20Feature%20selection,%20Regularización%20y%20Métricas.md) — clase siguiente: retoma la regularización y la selección de características que esta clase deja explícitamente pendientes
- [ML Clase 1 - Machine Learning Intro](ML%20Clase%201%20-%20Machine%20Learning%20Intro.md) — clase anterior: el encuadre supervisado/no supervisado sobre el que se apoyan los data splits
- [ML TP1 - Insurance](ML%20TP1%20-%20Insurance.md) — el TP1 pone en código los data splits, el k-fold y el criterio de outliers de esta clase
- [Terminologia ML](Terminologia%20ML.md) — parámetros vs. hiperparámetros: el train ajusta los primeros, el dev elige los segundos
- [Materia - Machine Learning](Materia%20-%20Machine%20Learning.md) — índice de la materia

**Otras materias**

- **MNA** — [Resumen MNA](Resumen%20MNA.md) — la regresión lineal de §4.2 es el problema de cuadrados mínimos: las mismas ecuaciones normales $A^\top A x = A^\top b$, con otro nombre

<!-- notas-relacionadas:fin -->
