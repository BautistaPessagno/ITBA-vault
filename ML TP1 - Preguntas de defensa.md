---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-2318:37
Materia: "[[Machine Learning.base|Machine Learning]]"
temas:
  - TP
  - Preguntas de defensa
  - Train/dev/test
  - Cross validation (k-fold)
  - Data leakage
  - Regularización L1 y L2
  - Bias-variance
  - RMSE y R²
---

# ML TP1 - Preguntas de defensa

Preguntas que es razonable esperar en los **8 minutos de preguntas** del [TP1](ML%20TP1%20-%20Insurance.md), agrupadas por tema. Las respuestas están en versión corta — la idea es poder decirlas, no leerlas.

> [!tip] Estrategia
> Casi todas las preguntas de este TP se reducen a **tres**: ¿por qué separaste así los datos?, ¿por qué elegiste ese modelo?, ¿cómo sabés que va a andar en datos nuevos? Si esas tres están sólidas, el resto sale.

---

## Sobre la separación de datos

> [!question]- ¿Por qué hacen falta **tres** conjuntos y no dos?
> Porque **cada vez que usás un conjunto para decidir algo, lo contaminás**. El dev lo usamos para elegir grado y λ, así que su error queda sesgado hacia abajo: es el mínimo de muchas pruebas, y parte de ese mínimo es suerte. El test nunca participó de ninguna decisión, por eso es la única estimación honesta. **El dev elige, el test estima.**

> [!question]- ¿Por qué 80/20 y no otra proporción?
> Es un compromiso ligado al **tamaño del dataset**. Con 1337 filas, el 20 % son ~268 muestras de test: suficiente para estimar un error, y sin sacarle demasiado al entrenamiento. Si tuviéramos millones de filas usaríamos 98/1/1, porque el 1 % ya alcanza para estimar bien y conviene entrenar con el resto.

> [!question]- ¿Por qué estratificaron por `smoker` y no por otra variable?
> Porque es la más predictiva (correlación 0,79 con el target) y está **desbalanceada**: sólo 20,5 % de fumadores. Si el azar deja 15 % en train y 26 % en test, el modelo entrena y se evalúa sobre poblaciones distintas y el RMSE de test se vuelve mucho más ruidoso. Verificamos que quedó 20,5 % en ambos.

> [!question]- ¿Por qué k=5 y no k=10 o leave-one-out?
> Compromiso entre sesgo, varianza y costo. Con k chico (2–3) cada modelo entrena con pocos datos y el error queda **pesimista**. Con leave-one-out el costo se dispara (1069 entrenamientos por configuración × 15 configuraciones) y la estimación tiene **más varianza**, porque los modelos entre folds son casi idénticos y sus errores están muy correlacionados. Con k=5 cada fold valida sobre ~214 muestras.

> [!question]- ¿Por qué no usaron un único train/dev en vez de cross-validation?
> Con 1069 filas de train, un dev del 20 % son ~214 muestras: la estimación dependería mucho de **qué** 214 filas cayeron ahí. El k-fold usa **todos** los datos para validar (rotando) y además nos da el **desvío entre folds** (±647), que es información sobre cuán confiable es la estimación. Con un solo split no tenés esa barra de error.

---

## Sobre la limpieza

> [!question]- ¿Por qué no eliminaron los outliers de `charges`? Son el 10 % de los datos.
> **La pregunta clave es si son errores o casos reales.** Miramos quiénes son: casi todos **fumadores**. La media de un fumador es ~32.050 contra ~8.434 de un no fumador. No son errores de carga, son la señal que el modelo tiene que aprender. Eliminarlos sería entrenar sobre un mundo donde no existen los fumadores caros, y después el modelo fallaría justo en los casos que más plata cuestan.
>
> Un outlier se elimina cuando es **error de medición o de carga** (un BMI de 300, una edad de −5). Se conserva cuando es un **caso real y raro**.

> [!question]- ¿Por qué IQR y no z-score para detectar outliers?
> Porque `charges` tiene **skew 1,52** — cola larga a derecha. El z-score asume normalidad, y con una cola larga la media y el desvío ya están inflados por los propios outliers, así que el criterio se auto-sabotea. El IQR se basa en cuartiles, que son **robustos** a valores extremos.

> [!question]- ¿Qué habrían hecho si hubiera valores faltantes?
> Depende de cuántos y de por qué faltan. Si es un porcentaje muy chico y parece aleatorio, eliminar las filas. Si es una columna con muchos faltantes, imputar (media/mediana para numéricas, moda o una categoría "desconocido" para categóricas) y — importante — **imputar dentro del pipeline**, calculando la media sólo con el train de cada fold, no con el dataset completo. En este dataset no hizo falta: 0 nulos.

> [!question]- Eliminaron una fila duplicada. ¿Cambia algo?
> Numéricamente muy poco (1 de 1338), pero **conceptualmente sí**: si esa fila cae en el train de un fold y su gemela en el dev, el modelo está validando sobre un registro **idéntico** a uno que entrenó. Eso es data leakage, y ensucia la estimación aunque sea en una escala mínima.

---

## Sobre el preprocesamiento

> [!question]- ¿Por qué one-hot y no asignarle números a las categorías?
> Porque un label encoding (`northeast=0 … southwest=3`) le impone a la variable un **orden y una distancia que no existen**. El modelo lineal interpretaría que southwest está "3 veces más lejos" de northeast que northwest, y eso no significa nada — `region` es **nominal**. Con one-hot cada categoría tiene su propio coeficiente, independiente.

> [!question]- ¿Por qué `drop='first'`?
> Para evitar la ***dummy variable trap***: con las $k$ columnas, la suma de las dummies da siempre 1, que es exactamente la columna de bias. Eso es **colinealidad perfecta** y hace $X^TX$ singular (no invertible). Dropeando una categoría, ésta pasa a ser la **referencia** y los otros coeficientes se leen como diferencia respecto de ella.

> [!question]- ¿Por qué `children` queda como numérica si tiene sólo 6 valores?
> Porque es **numérica discreta**, no categórica: el orden significa algo (3 hijos es más que 1) y la relación con el costo es aproximadamente monótona. Codificarla one-hot gastaría 5 columnas para representar información que una sola ya captura, y perdería la monotonía.

> [!question]- Si para OLS el escalado no cambia el resultado, ¿por qué escalan?
> Por tres razones, y sólo la primera es opcional:
> 1. Para **OLS puro** efectivamente no cambia el RMSE — la solución es equivalente.
> 2. Para **Lasso/Ridge es imprescindible**: la penalización $\lambda\sum|w_j|$ castiga a todos los coeficientes por igual. Sin escalar, una variable en unidades grandes tiene coeficiente chico y se penaliza menos → la regularización dependería de las **unidades**, no de la relevancia.
> 3. Mejora el **condicionamiento numérico** con features polinómicas: en grado 3, $age^3 \approx 260.000$ conviviendo con dummies de 0/1 da una matriz muy mal condicionada.

> [!question]- ¿Dónde exactamente aplicaron el escalado? ¿No hay data leakage?
> Dentro de un `Pipeline` de sklearn, así que el `.fit()` del scaler se ejecuta **sólo sobre el train de cada fold**. Si calculáramos media y desvío sobre el dataset completo, información del dev (y del test) se filtraría al entrenamiento y todas las estimaciones quedarían optimistas. El orden del pipeline es: one-hot → polinomio → escalado → modelo.

---

## Sobre los modelos

> [!question]- ¿Por qué el grado 2 mejora tanto respecto del lineal?
> Por la **interacción `bmi × smoker`**. En el EDA se ve claro: el BMI encarece la póliza **sólo si la persona fuma** — en no fumadores la nube es casi plana. Un modelo lineal es **aditivo**: puede decir "el BMI suma X" y "fumar suma Y", pero no "el BMI suma X *sólo si* fuma". `PolynomialFeatures` genera productos cruzados, así que el término `bmi × smoker_yes` aparece explícitamente y el modelo puede usarlo.

> [!question]- ¿Cómo saben que el grado 3 overfittea y no es simplemente peor?
> Por la **combinación de train y validación**. El grado 3 tiene el **menor error de train de todos** (4.664, mejor que el grado 2) y a la vez el peor error de validación entre los polinómicos (5.226). Ese patrón — train baja, validación sube — es la definición operativa de overfitting. El **gap** lo resume: 133 en grado 2 contra 562 en grado 3.

> [!question]- ¿Por qué la regularización no mejora el modelo lineal?
> Porque el lineal **no está overfitteando**: su gap train/validación es de sólo 40 dólares. Está **underfitteando** — le falta capacidad, no le sobra. La regularización sirve para reducir varianza a costa de sumar bias; aplicarla a un modelo que ya tiene bias alto sólo lo empeora. Se ve en el gráfico: la curva del grado 1 es plana y sube con λ=500.

> [!question]- ¿Por qué L1 y no L2? ¿Cuál es la diferencia?
> **L1 (Lasso)** penaliza $\sum|w_j|$ y empuja coeficientes exactamente a **cero** → hace *feature selection* automática, deja un modelo **sparse**. **L2 (Ridge)** penaliza $\sum w_j^2$ y **encoge** los coeficientes hacia cero sin anularlos, lo que estabiliza el modelo frente a colinealidad.
>
> Elegimos L1 porque el grado 3 genera **164 features** de las que muchas son productos cruzados sin sentido, y queríamos que el modelo las descartara solo. La consigna además lo pedía así. Probar Ridge sería una extensión natural.

> [!question]- ¿Cómo eligieron los valores de λ?
> Barrido en escala aproximadamente logarítmica (1, 10, 100, 500) más el caso sin regularizar, evaluando cada uno por cross-validation. La escala log es lo correcto porque λ actúa multiplicativamente: la diferencia entre 1 y 10 importa mucho más que entre 100 y 110. El óptimo dio λ=10 para grado 2 y λ=100 para grado 3 — coherente: **cuanto más complejo el modelo, más regularización necesita**.

> [!question]- ¿Por qué RMSE y no MSE, MAE o R²?
> **RMSE sobre MSE**: queda en las mismas unidades que el target (dólares), así que se interpreta directo — "el modelo se equivoca en promedio 5.000 dólares". El MSE está en dólares al cuadrado, que no significa nada.
> **RMSE sobre MAE**: el RMSE penaliza más los errores grandes (los eleva al cuadrado). En seguros eso es deseable: equivocarse por 30.000 en un caso es mucho peor que por 3.000 en diez casos.
> **R²** lo reportamos como complemento (0,895 en test), pero es adimensional: dice qué proporción de la varianza explica el modelo, no cuánta plata errás.

---

## Sobre los resultados (las preguntas difíciles)

> [!question]- El RMSE de test (3.890) les dio **mejor** que el de validación (5.021). ¿No debería ser al revés?
> Sí, lo esperable es que el test sea **peor**, porque el error de validación está sesgado hacia abajo. Que salga mejor indica que **nos tocó una partición de test favorable**.
>
> Lo verificamos: repetimos todo el procedimiento con **10 particiones distintas**. El RMSE de test osciló entre **3.627 y 5.576**, mientras el de cross-validation se mantuvo entre **4.649 y 5.142**. Con sólo 268 muestras de test, un único número es **ruido de muestreo**. Por eso reportamos ~5.000 y no 3.890.

> [!question]- Entonces, ¿qué RMSE le prometerían a un cliente?
> **~5.000 dólares, presentado como rango (~4.600–5.600).** Es la estimación de cross-validation, que es la estable. Prometer el 3.890 sería sobrevender el modelo apoyándose en la suerte de un split.
>
> Y agregaría tres advertencias: el error **no es uniforme** (mucho mayor en fumadores), sólo vale para poblaciones **similares** a la de entrenamiento (EE.UU., 18–64), y una parte del error es **irreducible** por variables que no tenemos.

> [!question]- ¿Un RMSE de 5.000 es bueno o malo?
> Depende contra qué. El costo promedio es 13.270, así que es un **38 % de error relativo** — alto en términos absolutos. Pero contra el **baseline** de predecir siempre la media (RMSE 12.013) es una reducción del **68 %**, y el R² de 0,895 dice que explicamos casi el 90 % de la varianza. Para un dataset con sólo 6 variables demográficas y sin ninguna variable clínica, es razonable.

> [!question]- ¿Cómo mejorarían el modelo?
> Por orden de retorno esperado:
> 1. **Más variables**, no más datos: historia clínica, patologías previas, tipo de plan. Buena parte del error actual es **ruido por información faltante** — más filas del mismo tipo no lo arreglan.
> 2. **Modelos no lineales por partes** (árboles, gradient boosting): capturan el quiebre en BMI=30 sin necesidad de polinomios.
> 3. **Modelar el target en escala logarítmica**, dado el skew de 1,52.
> 4. Una curva de aprendizaje diría si más datos ayudarían — ver [Clase 2 §8](ML%20Clase%202%20-%20Datos,%20variables,%20overfitting%20y%20métricas.md#8.%20¿Cuántos%20datos%20hacen%20falta%3F).

> [!question]- ¿No estarían overfitteando a la validación por probar 15 configuraciones?
> Es un riesgo real: cuantas más veces mirás el dev, más se parece a un train. Con 15 configuraciones el efecto es chico, pero existe — y es exactamente la razón por la que el **test se reservó intacto**. Nuestro test confirma que el modelo generaliza. Si hubiéramos probado cientos de configuraciones, habría que agregar un nivel más (nested cross-validation).

> [!question]- Los residuos no parecen homocedásticos. ¿Es un problema?
> Se nota, sí: la dispersión de los residuos crece con el valor predicho, concentrada en fumadores. Rompe un supuesto de la inferencia clásica de OLS (los intervalos de confianza sobre los coeficientes dejarían de ser válidos), pero **no invalida el modelo como predictor** — el RMSE sigue siendo una medida honesta del error promedio. La forma de atacarlo sería transformar el target con log, o modelar fumadores y no fumadores por separado.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Machine Learning)**

- [ML TP1 - Insurance](ML%20TP1%20-%20Insurance.md) — el TP en sí: decisiones, resultados y respuestas de la consigna
- [ML Clase 2 - Datos, variables, overfitting y métricas](ML%20Clase%202%20-%20Datos,%20variables,%20overfitting%20y%20métricas.md) — de acá salen las respuestas sobre data splits, k-fold, outliers y fuentes de ruido
- [ML Clase 3 - EDA, Feature selection, Regularización y Métricas](ML%20Clase%203%20-%20EDA,%20Feature%20selection,%20Regularización%20y%20Métricas.md) — L1 vs L2, escalado y overfitting a la validación
- [Terminologia ML](Terminologia%20ML.md) — parámetros vs. hiperparámetros: útil para responder qué se aprende en train y qué se elige en dev

**Otras materias**

- **MNA** — [Resumen MNA](Resumen%20MNA.md) — por qué $X^TX$ singular rompe la solución de cuadrados mínimos: es el fundamento de la respuesta sobre la *dummy variable trap*

<!-- notas-relacionadas:fin -->
