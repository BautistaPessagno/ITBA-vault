---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-09-0218:10
Materia: "[[Machine Learning.base|Machine Learning]]"
temas:
  - Clasificación binaria
  - Regresión logística
  - Función sigmoidea
  - Odds y logit
  - Umbral de decisión
  - Accuracy y sus limitaciones
  - Matriz de confusión (TP, FP, FN, TN)
  - Precision, Recall (Sensitivity)
  - Specificity y Negative Predicted Value
  - Trade-off del umbral
  - Curva ROC (TPR vs FPR)
  - AUC
---

# Clase 4 - Regresión Logística y Métricas de Evaluación

> [!abstract] Plan de la sesión
> 1. Regresión Logística
> 2. Métricas de Evaluación
> 3. Métricas de Evaluación con clases desbalanceadas
> 4. Curva ROC
> 5. Multiclase
>
> Viene de [[ML Clase 3 - EDA, Feature selection, Regularización y Métricas|Clase 3]]: Problema-Datos-Features, EDA, feature selection, regularización y métricas de regresión.

## Repaso
si eliminamos una variable entre dos con mucha correlacion, eliminar la que menos correlacion tenga con la variable objetivo

hasta ahora fueron problemas de regression

lo que sigue es problemas de clasificacion

> [!info] Dónde estamos en el pipeline
> La Clase 3 cubrió los pasos de **preparación** (EDA, feature selection, regularización, evaluación en dev). Esta clase arranca el **paso 7: Modelado** — la regresión logística es el primer clasificador de la materia.
>
> Cambia el tipo de problema y por eso cambian las métricas: RMSE y R² son de regresión, acá no sirven.

---

## Regresión Logística

### Motivación
problemas de clasificacion binaria y basado en datos anteriores

el 0 es la clase negativa mientras que el 1 es la clase positiva

con la variable a predecir Y, se usan las features X

$$
y \in \{0,1\}
$$

Ejemplos de la cátedra, con qué features usarían:

| Problema | Clase 0 / Clase 1 | Features X |
| --- | --- | --- |
| Emails | no es spam / es spam | frecuencia de palabras: *address*, *free*, *order* |
| Tumor | benigno / maligno | radio, textura, perímetro, área, suavidad |
| Semiconductores | unidad OK / unidad defectuosa | información de sensores |

> [!tip] Quién es la clase positiva es una decisión tuya
> No hay nada en el problema que diga que "maligno" tiene que ser el 1. Pero **todas** las métricas que siguen (precision, recall, TPR, FPR) están definidas respecto de la clase positiva, así que si la das vuelta cambian todos los números. Por convención se pone como positiva la clase **rara y costosa de no detectar**.

### Por qué no alcanza con una regresión lineal
no conviene una regresion lineal porque puede afectar los valores, hay que achicar la curva

Caracteristicas:
- predice valores continuos
- resuelve problemas de regresion

Problemas:
- predice valores fuera del rango 0 y 1 y no tiene sentido interpretarlos

![[ML-C4-regresion-lineal-outlier.png]]

> [!observacion] Lo que muestra el gráfico
> Con los datos "lindos" la recta separaba bien: cortaba $y = 0.5$ justo entre los benignos y los malignos.
>
> Al agregar **un solo punto lejano** (un tumor de 20cm, que además está bien etiquetado como maligno) la recta se aplana para minimizar el error cuadrático, el corte con $0.5$ se corre a la derecha y un tumor que antes clasificaba bien ahora queda del lado benigno (la ✗ roja).
>
> El problema de fondo: **la regresión lineal minimiza el error cuadrático, no la cantidad de aciertos.** Un punto lejano pero *correcto* le sigue costando error y arrastra toda la recta.

### La función sigmoidea
la solucion es la funcion sigmoidea

$$
P(Y=1) = \frac{1}{1+e^{-(wx+b)}}
$$

![[ML-C4-funcion-sigmoidea.png]]

- Asíntotas en 0 y 1
- Transforma cualquier número real en una probabilidad
- Se utiliza para problemas de clasificación (**NO regresión**)

el entrenamiento cambian las variables **w** y **b** (peso y sesgo)

> [!observacion] Qué hace cada parámetro
> - **w** controla qué tan empinada es la curva: $w$ grande → transición casi vertical (el modelo está "seguro"); $w$ chico → curva suave (probabilidades tibias).
> - **b** desplaza la curva a izquierda/derecha: mueve el punto donde $P = 0.5$.
>
> El punto donde $P(Y=1) = 0.5$ es exactamente $wx + b = 0$. O sea que la **frontera de decisión de la regresión logística sigue siendo lineal** — lo que cambió es cómo se mide el error, no la forma de la frontera.

### De dónde sale la sigmoidea: odds y logit
El modelo logístico no es más que una regresión lineal común… pero aplicada sobre una **escala logarítmica**.

**Odds** (la "chance"):

$$
odds = \frac{p}{1-p}
$$

- Ejemplo: $odds = \dfrac{0.8}{1-0.8} = \dfrac{0.8}{0.2} = 4$ → es 4 veces más probable que compre a que no compre.
- Dominio $p \in (0,1)$, rango $(0, +\infty)$.

Odds va de 0 a infinito; para estirar el rango a **todos** los reales se le aplica el logaritmo:

$$
logit(p) = \ln\left(\frac{p}{1-p}\right) = \beta_0 + \beta_1 x
$$

- Dominio $(0, +\infty)$, rango $(-\infty, +\infty)$.
- El modelo es lineal, solo que **no en $p$ sino en el logit de $p$**.

Despejando $p$ de ahí sale la sigmoidea (llamando $z = \beta_0 + \beta_1 x$):

$$
\frac{p}{1-p} = e^{z} \;\Rightarrow\; p = e^{z}(1-p) \;\Rightarrow\; p(1+e^{z}) = e^{z} \;\Rightarrow\; p = \frac{e^{z}}{1+e^{z}} = \frac{1}{1+e^{-z}}
$$

![[ML-C4-despeje-sigmoidea.png]]

> [!observacion] Por qué importa el logit
> Es lo que explica de dónde sale la fórmula rara de la sigmoidea: **no es una función inventada para "aplastar"**, es la inversa de aplicar una regresión lineal sobre el log de las odds.
>
> También da la interpretación de los coeficientes: $\beta_1$ no dice cuánto sube la probabilidad, dice **cuánto sube el log-odds** por unidad de $x$. Equivalente: las odds se multiplican por $e^{\beta_1}$.

### Del número a la predicción: el umbral
la respuesta no va a ser binaria (ej: 0,7), para tener algo binario necesitamos un umbral

![[ML-C4-ejemplo-prediccion.png]]

Ejemplo de la clase: llega un paciente con un tumor de 4cm, la salida del modelo es **0.7** → el paciente tiene 70% de probabilidad de tener un tumor maligno. Para pasar de esa probabilidad a una predicción (0 o 1) hace falta **un umbral**.

> [!important] El umbral no lo aprende el modelo
> El entrenamiento ajusta $w$ y $b$. El umbral lo elegís **vos, después**, y se puede cambiar sin reentrenar nada. Por eso es una decisión de negocio/aplicación, no de optimización — y es de lo que trata toda la segunda mitad de la clase.

---

## Métricas de evaluación

Ya entrenamos el modelo. **¿Cómo sabemos si es bueno?**

- Accuracy (porcentaje de aciertos)
- Matriz de confusión
- Elección del umbral y curva ROC
- Otras métricas (precision, recall, F1)

### Accuracy

predicciones correctas / predicciones totales

$$
Accuracy = \frac{TP + TN}{TP + FP + TN + FN}
$$

Ejemplo: probamos el modelo con 10 pacientes nuevos y acertó el diagnóstico en 8 → $Accuracy = 8/10 = 80\%$.

limitaciones: hay distintos tipos de errores que me puede importar (no es lo mismo dos errores a dos falsos positvos)

![[ML-C4-limitaciones-accuracy.png]]

3 modelos, 3 resultados distintos, los mismos 10 pacientes, **la misma accuracy**:

| Escenario | Errores | Accuracy |
| --- | --- | --- |
| A | 2 falsos negativos | 80% |
| B | 2 falsos positivos | 80% |
| C | 1 falso negativo + 1 falso positivo | 80% |

> [!warning] La otra limitación: clases desbalanceadas
> Si el 99% de los mails no son spam, un modelo que dice "nunca es spam" tiene **99% de accuracy** y es completamente inútil. Cuanto más desbalanceadas las clases, más engañosa la accuracy.
>
> Por eso hacen falta las métricas que vienen: miran los errores **por tipo**, no en total.

### Matriz de confusión
metodo de visualizacion de los resultados de un algortimos
se deslglozan los errores

|                    | **Real: +**                 | **Real: −**                 |
| ------------------ | --------------------------- | --------------------------- |
| **Predicho: +**    | verdadero positivo (TP)     | falso positivo (FP)         |
| **Predicho: −**    | falso negativo (FN)         | verdadero negativo (TN)     |

![[ML-C4-matriz-confusion.png]]

Con el ejemplo del tumor (positivo = maligno):

- **TP (verdadero positivo):** tumor maligno, el modelo dijo maligno.
- **TN (verdadero negativo):** tumor benigno, el modelo dijo benigno.
- **FP (falso positivo):** tumor benigno, el modelo dijo maligno.
- **FN (falso negativo):** tumor maligno, el modelo dijo benigno — **el error más grave acá**.

esta bueno agregar porcentajes para ver realmente como predice

![[ML-C4-matriz-confusion-ejemplos.png]]

> [!observacion] Ojo con la orientación de la matriz
> No hay una convención única: acá las **filas son la predicción** y las **columnas el ground truth**, pero `sklearn.metrics.confusion_matrix` lo devuelve **al revés** (filas = real, columnas = predicho) y con el orden `[[TN, FP], [FN, TP]]`.
>
> Siempre mirá los labels de los ejes antes de leer los números, si no confundís FP con FN — que es justo el error que la matriz vino a evitar.

> [!note] Multiclase
> La matriz de confusión es lo único de esta clase que se generaliza directo a más de dos clases: pasa a ser $k \times k$, la **diagonal son los aciertos** y cada celda fuera de la diagonal dice *con qué otra clase* se confunde (ej: la matriz de Grado 0-4 de la derecha muestra que el modelo confunde sobre todo grados vecinos).
>
> Precision y recall en multiclase se calculan **una vs. el resto** para cada clase y después se promedian (macro / weighted).

### Precision y Recall (Sensitivity)

![[ML-C4-precision-recall.png]]

Recall = TP/(TP+FN)
Precision = TP/(TP+FP)
Accuracy = (TP + TN)/(TP + FP + TN + FN)

- **Precision:** qué tan exactas son las predicciones positivas.
- **Recall (Sensitivity):** capacidad del modelo de encontrar todos los casos positivos presentes.

> [!tip] Cómo no confundirlas
> Mirá el **denominador**, que es lo que dice sobre qué conjunto estás midiendo:
> - Precision divide por **lo que el modelo dijo que era positivo** → "de los que marqué, ¿cuántos eran?"
> - Recall divide por **lo que realmente es positivo** → "de los que había, ¿cuántos encontré?"

### Specificity y Negative Predicted Value
Las dos métricas espejo, mirando la clase **negativa**:

![[ML-C4-specificity.png]]

$$
Specificity = \frac{TN}{TN + FP} \qquad NPV = \frac{TN}{TN + FN}
$$

- **Specificity:** capacidad del modelo de encontrar los negativos (el recall de la clase negativa).
- **NPV (negative predicted value):** qué tan exactas son las predicciones negativas (la precision de la clase negativa).

Las cuatro juntas, para tenerlas de una:

| Métrica | Fórmula | Pregunta que responde |
| --- | --- | --- |
| Precision | $TP/(TP+FP)$ | de los que marqué positivos, ¿cuántos lo eran? |
| Recall / Sensitivity / TPR | $TP/(TP+FN)$ | de los positivos reales, ¿a cuántos detecté? |
| Specificity | $TN/(TN+FP)$ | de los negativos reales, ¿a cuántos detecté? |
| NPV | $TN/(TN+FN)$ | de los que marqué negativos, ¿cuántos lo eran? |

> [!note] F1
> El índice de la clase menciona **F1** pero no hay slide que la desarrolle. Es la media armónica de precision y recall, y sirve para tener **un solo número** cuando querés balancear las dos:
> $$F_1 = 2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$$
> Se usa media armónica y no aritmética justamente para que castigue los extremos: un modelo con precision 1.0 y recall 0.0 da F1 = 0, no 0.5.

---

## El trade-off del Umbral
Recordemos: la regresión logística devuelve una **probabilidad**. Nosotros elegimos el umbral para convertirla en 0 o 1.

bajamos el umbral -> mas falsos positivos entonces, sube el recall y baja la precision
subimos el umbral mas precision y baja el recall

![[ML-C4-umbral-bajo.png]]

**Si bajamos el umbral:** el modelo marca maligno más seguido → ↑ **Recall**, ↓ **Precision**. Más falsas alarmas pero no se escapa ningún maligno.

![[ML-C4-umbral-alto.png]]

**Si subimos el umbral:** el modelo marca benigno más seguido → ↑ **Precision**, ↓ **Recall**. No hay falsas alarmas pero se escapan algunos tumores malignos.

>[!important] es depende la aplicacion el umbral decidido

**¿Qué conviene? Depende del costo de cada error:**

| Aplicación | Umbral | Por qué |
| --- | --- | --- |
| Diagnóstico de cáncer | **bajo** (recall alto) | es peor dejar pasar un maligno que revisar de más un caso benigno |
| Filtro de spam | **alto** (precision alta) | es peor perder un mail importante que dejar pasar algún spam |

No existe un umbral "correcto" universal: depende del problema. ¿Cómo visualizamos este trade-off? → **curva ROC**.

---

## Curva ROC

En vez de fijar UN umbral y calcular UN (Precision, Recall), la curva ROC recorre **todos** los umbrales posibles. Para cada umbral se calculan dos cosas:

True Positive rate (TPR) = recall

$$
TPR = \frac{TP}{TP+FN}
$$

De los malignos reales, ¿a cuántos detectó?

False Positive Rate (FPR) = 1 − Specificity

$$
FPR = \frac{FP}{FP+TN}
$$

De los benignos reales, ¿a cuántos etiquetó mal como malignos?

Graficando cada $(FPR, TPR)$ a medida que bajamos el umbral de 1 a 0, obtenemos la curva ROC.

Los tres puntos que construyó la clase:

| Umbral | TP | FP | FN | TN | TPR | FPR | Punto |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1.0 (nada es positivo) | 0 | 0 | — | — | 0 | 0 | esquina (0,0) |
| 0.5 | 35 | 5 | 6 | 37 | 0.85 | 0.11 | (0.11, 0.85) |
| 0.0 (todo es positivo) | 45 | 45 | 0 | 0 | 1 | 1 | esquina (1,1) |

![[ML-C4-curva-roc.png]]

> [!observacion] Cómo leer la curva
> - La curva **siempre** arranca en (0,0) y termina en (1,1): son los dos umbrales extremos, y no dependen del modelo.
> - Bajar el umbral te mueve **hacia arriba y a la derecha**: ganás recall y pagás falsos positivos. La curva no puede bajar.
> - La **diagonal** es el clasificador aleatorio: para cada 1% de malignos que detectás, te comés 1% de benignos mal marcados. Estar por debajo de la diagonal significa que el modelo es peor que tirar una moneda (y que dando vuelta las predicciones sería bueno).
> - El clasificador **ideal** es el punto (0,1): detecta todo sin ninguna falsa alarma.
>
> Lo importante: la curva ROC es una propiedad **del modelo**, no de un umbral. Comparás modelos con ella *antes* de elegir el punto de operación.

> [!bug] Los números de las slides no cierran entre sí
> En el punto del umbral 0.5 hay $TP+FN = 41$ positivos y $FP+TN = 42$ negativos (83 casos). En el del umbral 0 hay 45 positivos y 45 negativos (90 casos).
>
> Sobre el mismo conjunto de test los totales **no pueden cambiar** al mover el umbral: lo único que se mueve es cómo se reparten entre TP/FN y FP/TN. Son slides distintas armadas con números de ejemplo, no un cálculo sobre un dataset común.
>
> (También: $FPR = 5/42 = 0.119$, que redondea a **0.12**, no a 0.11.)

---

## AUC
Manera equivalente de ver el desempeño del clasificador para diferentes umbrales: el **área bajo la curva ROC**.

![[ML-C4-auc-clasificadores.png]]

- **AUC = 1** → clasificador ideal
- **AUC = 0.5** → clasificador aleatorio (la diagonal)
- AUC > 0.5 → "buen" clasificador
- AUC < 0.5 → "mal" clasificador

> [!observacion] Qué mide realmente el AUC
> Tiene una interpretación probabilística linda: el AUC es **la probabilidad de que el modelo le asigne mayor score a un positivo elegido al azar que a un negativo elegido al azar**.
>
> Consecuencias prácticas:
> - Es **un solo número que no depende del umbral** → sirve para comparar modelos.
> - Solo le importa el **orden** de los scores, no su calibración: si a todas las probabilidades les aplicás la misma función creciente, el AUC no cambia.
> - Por eso mismo **no reemplaza** elegir bien el umbral, y con clases muy desbalanceadas puede verse optimista (ahí conviene mirar también la curva Precision-Recall).

## Elección del mejor umbral
No hay un umbral "correcto" único: depende de si priorizás positivos, negativos o ambos.

Un criterio práctico: elegir el umbral cuyo punto $(FPR, TPR)$ esté **más cerca de $(0,1)$**, el punto ideal en la curva ROC.

**Procedimiento:**

1. Definir un rango de umbrales según el clasificador
2. Para cada umbral: clasificar los datos, calcular FPR y TPR, y guardar la distancia al punto $(0,1)$
3. Elegir el umbral con distancia mínima

$$
d = \sqrt{FPR^2 + (1 - TPR)^2}
$$

> [!warning] El umbral se elige en validación, no en test
> Barrer umbrales y quedarse con el mejor es **ajustar un hiperparámetro**. Si lo hacés mirando el test, el número que reportás ya no es una estimación honesta — es el mismo problema de "overfitting a la validación" de la [[ML Clase 3 - EDA, Feature selection, Regularización y Métricas|Clase 3]].
>
> Y este criterio (distancia a (0,1)) trata a FP y FN como **igual de caros**, que es justo lo que la clase venía diciendo que casi nunca es cierto. Si un error cuesta más que el otro, el criterio correcto es minimizar el costo esperado, no la distancia geométrica.

---

> [!summary] Resumen de la clase en 6 líneas
> 1. Clasificación binaria: $y \in \{0,1\}$, y la **clase positiva la elegís vos** — todas las métricas dependen de esa elección.
> 2. La regresión lineal no sirve porque predice fuera de $[0,1]$ y un outlier correcto le corre la frontera → **sigmoidea**, que sale de hacer la regresión lineal sobre el **logit**.
> 3. El modelo devuelve una **probabilidad**; el **umbral** es una decisión posterior que no requiere reentrenar.
> 4. La **accuracy no distingue tipos de error** (ni sobrevive a clases desbalanceadas) → matriz de confusión y precision / recall / specificity / NPV.
> 5. **Trade-off del umbral:** bajarlo sube recall y baja precision; subirlo al revés. Cuál conviene depende del **costo de cada error** (cáncer → umbral bajo; spam → umbral alto).
> 6. La **curva ROC** (TPR vs FPR) recorre todos los umbrales y el **AUC** la resume en un número independiente del umbral: 1 = ideal, 0.5 = azar.

## Referencias de la cátedra

- [1] A. Géron, *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*, 3rd ed. O'Reilly Media, 2022.
- [2] Codificando Bits, "Clasificación: la curva ROC y el AUC", 2021. https://codificandobits.com/tutorial/clasificacion-curva-roc-auc/
- [3] "Video sobre regresión logística y clasificación", YouTube. https://www.youtube.com/watch?v=4u81xU7BIOc

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Machine Learning)**

- [[ML Clase 6 - GDA y Naive Bayes]] — clase siguiente: la regresión logística es el ejemplo de modelo **discriminativo**; LDA (generativo) llega a la misma frontera lineal y su posterior es exactamente esta sigmoide
- [[ML Clase 3 - EDA, Feature selection, Regularización y Métricas]] — clase anterior: cierra la etapa de preparación (EDA, feature selection, regularización) y deja planteado el paso de Modelado que arranca acá; además el "overfitting a la validación" es lo que hace que el umbral se elija en dev y no en test
- [[ML Clase 2 - Datos, variables, overfitting y métricas]] — de ahí vienen los data splits y las métricas de **regresión** (RMSE, R²); esta clase es el contraste: cambia el tipo de problema, cambian las métricas
- [[ML Clase 1 - Machine Learning Intro]] — encuadre supervisado/no supervisado: la clasificación binaria es aprendizaje supervisado con target categórico
- [[ML TP1 - Insurance]] — el TP1 es de regresión; la regresión logística es el modelo que faltaba para atacar un target binario
- [[Terminologia ML]] — el **umbral** es un hiperparámetro más: se elige en validación, no se aprende en el entrenamiento
- [[Materia - Machine Learning]] — índice de la materia

**Otras materias**

- **MNA** — [[Resumen MNA]] — la regresión logística no tiene solución cerrada como cuadrados mínimos: se ajusta con métodos iterativos (gradiente / Newton), que es la otra mitad de los métodos numéricos
- **Discrete Math** — [[Discrete Math - Grafos Fundamentos]] — el umbral de decisión $wx+b=0$ es un hiperplano separador: la misma idea de "partir el espacio en dos" que aparece en coloreo y bipartición

<!-- notas-relacionadas:fin -->
