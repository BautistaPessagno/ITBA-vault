---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-08-0516:05"
Materia: "[[Machine Learning.base|Machine Learning]]"
temas:
  - Aprendizaje automático
  - Programación tradicional vs AA
  - Aprendizaje supervisado
  - Aprendizaje no supervisado
  - Aprendizaje por refuerzo
  - Batch vs Online learning
  - Out-of-core learning
  - Instance-based vs Model-based
  - IA vs AA vs Deep Learning
  - Estructura de un proyecto ML
  - Sampling bias
  - Survival bias
  - Overfitting
  - Underfitting
---
# Machine Learning — Clase 1: Introducción al Aprendizaje Automático

## Resumen

El AA invierte el esquema de la programación tradicional: en vez de darle al ordenador **datos + programa** para obtener un output, se le dan **datos + outputs** y él produce el **programa**. La clase recorre tres criterios para clasificar tipos de AA (supervisión humana, aprendizaje en tiempo real, estrategia de generalización), ubica al AA dentro de la IA y el DL, y define los 4 pasos de un proyecto de ML supervisado junto con sus principales retos.

## ¿Qué es aprendizaje?

> Es el proceso mediante el cual un sistema (humano, animal o máquina) **adquiere, modifica o mejora su comportamiento o conocimiento** a partir de la experiencia o la información del entorno, permitiéndole adaptarse y responder mejor a situaciones futuras.

![[Pasted image 20260805164412.png]]

## ¿Sin ser programados explícitamente?

> "Campo de estudio que confiere a los ordenadores la capacidad de aprender sin ser programados explícitamente" — **Arthur Samuel (1959)**

En la forma **explícita** se le da al ordenador el algoritmo con las instrucciones.

**Programación "tradicional"**: `Datos + Programa → Ordenador → Output`

![[Pasted image 20260805164507.png]]

Ejemplos: ordenar una lista de números, verificador de contraseña, contador de palabras. El programador conoce las reglas y las escribe.

**Aprendizaje automático**: `Datos + Outputs → Ordenador → Programa`

![[Pasted image 20260805164617.png]]

Se **etiquetan** los datos y el algoritmo busca patrones; el resultado es el programa, que después se aplica a datos nuevos. Ejemplos: clasificador de spam, predictor del precio de una casa, identificación de pájaros.

> [!tip] La idea central
> En AA el **programa es la salida del entrenamiento**, no la entrada. Por eso sirve para problemas donde nadie sabe escribir las reglas a mano.

## Flujo de un programa de AA

Dos fases distintas:

1. **Entrenamiento** — `Datos + Outputs (etiquetas) → Ordenador → Modelo`
2. **Inferencia** — `Datos nuevos + Modelo → Ordenador → Output`

![[Pasted image 20260805164801.png]]

Hace falta **etiquetar los datos** (label). En el ejemplo del spam, las etiquetas salen de los usuarios que marcan correos como spam. Este trabajo de etiquetado muchas veces lo hacen personas:

![[Pasted image 20260805164824.png|337]] ![[Pasted image 20260805164836.png|331]]

### Ventajas frente a la programación tradicional

¿Cómo haríamos un detector de spam a mano?

1. Observar decenas/cientos de correos spam.
2. Identificar patrones: palabras como "gratis", "promoción", uso de mayúsculas…
3. Programar reglas específicas para esas palabras.
4. Evaluar la eficiencia del programa.
5. Volver al punto 2 si el resultado no alcanza.

Es un ciclo manual e interminable: cada vez que los spammers cambian de vocabulario hay que reescribir reglas. El enfoque de AA reaprende de los datos nuevos sin tocar el código.

## ¿Qué tipos de AA hay?

Tres criterios, no excluyentes entre sí:

1. **¿Es el aprendizaje supervisado por el humano?** (el más usado)
2. **¿Se aprende en tiempo real de un flujo de datos continuo?**
3. **¿Qué estrategia usa para generalizar a nuevos datos?**

### Criterio 1 — Supervisión humana

Analogía con cómo aprendemos:

| Ejemplo humano | Cómo se aprende | Tipo de AA |
|---|---|---|
| Identificar un perro | Viendo varios perros, extraemos rasgos característicos | **Supervisado** |
| Agrupar juguetes por colores | Observamos patrones (forma, color) sin que nadie nos diga que son categorías distintas | **No supervisado** |
| Aprender a caminar | Probamos movimientos, recibimos feedback "caída / no caída" y ajustamos | **Por refuerzo** |

![[ML C1 - tipos de aprendizaje.png]]

#### Aprendizaje supervisado

Aprender una función que mapea **entradas → salidas** a partir de **ejemplos etiquetados**.

**¿Qué tenemos?** $n$ datos, cada dato $i \in \{1, \dots, n\}$:

$$D_n = \{(x^{(1)}, y^{(1)}), \dots, (x^{(n)}, y^{(n)})\}$$

- $x^i = (x^i_1, \dots, x^i_d)$ — **vector de características** (*features*): número de patas, ¿orejas de perro?, …
- $y^i \in \{0, 1\}$ — **vector de etiquetas** (*labels*): perro (1), no perro (0)

**¿Qué queremos?** Una buena estrategia para etiquetar (clasificar) datos nuevos.

En el ejemplo del perro, cada dato se ubica en un espacio de características (*orejas de perro* × *cola de perro*) y buscamos la frontera que separa las clases. Se ve en el **módulo 1**, y en profundidad en los **módulos 2 y 3**.

![[ML C1 - supervisado.png]]

#### Aprendizaje no supervisado

Algoritmos diseñados para **buscar patrones en los datos** sin etiquetas.

- **Qué queremos**: encontrar patrones que agrupen los datos de la mejor forma posible.
- En el ejemplo de los juguetes, cada uno se representa por sus componentes RGB y los grupos emergen solos.
- Se ve en el **módulo 4**.

![[ML C1 - no supervisado.png]]

#### Aprendizaje por refuerzo

Aprende a tomar decisiones óptimas **interactuando con un entorno**, recibiendo feedback indirecto (incluso diferido).

- No se le dice cuál es la acción correcta: la descubre por **prueba y error**, con **recompensas** cuando acierta y **castigos** cuando se equivoca.
- **No hay un mapeo directo entrada → salida** como en supervisado.
- El agente aprende una **política** que maximiza recompensas.

*Ejemplo — robot que sale de un laberinto*: pequeña recompensa cada vez que avanza hacia la salida, penalización al chocar contra una pared, gran recompensa al salir. Tras muchas repeticiones aprende el camino más rápido sin que nadie se lo haya explicado.

#### Ejemplos de aplicación

| Aplicación | Tipo |
|---|---|
| Diagnóstico de imágenes médicas | Supervisado |
| Detección de fraude financiero | Supervisado |
| Control de prótesis robóticas | Por refuerzo |
| Análisis genómico (detectar patrones en datos genéticos) | No supervisado |
| Segmentación de clientes para recomendación personalizada | No supervisado |

### Criterio 2 — Batch vs Online Learning

| | **Batch / offline learning** | **Online learning** |
|---|---|---|
| Cómo aprende | Una vez, a partir de **todos** los datos | De forma **continua**, con datos individuales o mini-batches |
| Costo del entrenamiento | Alto: mucha CPU y memoria | "Ligero": debe ocurrir en tiempo real |
| Adaptarse a datos nuevos | Hay que **reentrenar todo** (coste alto) | Se adapta de forma continua |
| Ruido | Robusto si una parte de los datos es ruidosa | Una secuencia ruidosa puede arruinar el funcionamiento un tiempo → importa definir el **learning rate** (qué tan rápido se adapta) |

![[ML C1 - batch vs online.png]]

> [!note] Caso especial: out-of-core learning
> En entornos con poca memoria y/o muchos datos, las estrategias de online learning son muy útiles:
> - Permiten aprender en **mini-batches** sin guardar todos los datos en memoria/disco.
> - Entrenar offline pero con métodos de online learning equivale a un **aprendizaje incremental**.

### Criterio 3 — Cómo generaliza a datos nuevos

| | **Instance-Based Learning** | **Model-Based Learning** |
|---|---|---|
| Idea | Literalmente "aprender de memoria" | A partir de los datos construye un **modelo** que los representa |
| Qué se usa para predecir | Los **datos** mismos: cuál y cómo es el dato más parecido | El **modelo** |
| Qué hay que definir | Cómo se evalúa la **similitud** entre datos | Cómo **construir el modelo** a partir de los datos |

![[ML C1 - instance vs model based.png]]

## IA vs AA vs DL

Son conjuntos anidados: **IA ⊃ ML ⊃ ANN ⊃ DL**.

| Nivel | Qué es |
|---|---|
| **IA** | Cualquier simulación/imitación de la inteligencia humana en máquinas: máquinas programadas para imitar el razonamiento humano, aprender o resolver problemas |
| **AA / ML** | Metodología de entrenar sistemas para que aprendan a partir de datos pasados y hagan predicciones |
| **Redes neuronales (ANN)** | Sistemas de AA inspirados en las redes neuronales humanas: neuronas conectadas que aprenden las relaciones entre inputs y outputs |
| **Deep Learning** | Redes neuronales grandes, con múltiples capas (>3) |

![[ML C1 - IA vs AA vs DL.png]]

Esta materia se sitúa en el nivel de **AA / ML** (las redes neuronales y los LLM corresponden a *Sistemas de Inteligencia Artificial*, SIA).

## Contenido y objetivos de la materia

**Módulo 1 – Fundamentos y supervisado clásico**
Principios teóricos del aprendizaje supervisado y estructura de un proyecto de ML supervisado. Métricas y procesos de validación para evaluar modelos básicos.

**Módulo 2 – Clasificadores supervisados**
Funcionamiento interno de clasificadores clásicos (**KNN, SVM, árboles**). Comparación de su comportamiento y supuestos en distintos contextos de datos.

**Módulo 3 – Teoría del aprendizaje y mejora de modelos**
Criterios de diagnóstico y optimización (**bias-variance, regularización, tuning**). Selección de modelos y estrategias para evitar errores comunes; buenas prácticas.

**Módulo 4 – No supervisado y feature engineering**
Métodos de **clustering** y **reducción de dimensionalidad**. Técnicas de preprocesamiento y representación para mejorar el rendimiento del modelo.

## Estructura de un proyecto de ML supervisado

1. **Obtener y preparar los datos**
2. **Elegir una estrategia de aprendizaje** (algoritmo)
3. **Definir cómo evaluar el aprendizaje**
4. **Evaluar cómo funcionará en datos nuevos**

### Ejemplo: diagnóstico precoz de Alzheimer

| Paso | Concreción |
|---|---|
| 1. Obtener y preparar los datos | 1.1 Hablar con neurólogos / leer para entender cómo se puede hacer. 1.2 Obtener imágenes de MRI y **etiquetarlas** (diagnóstico positivo/negativo). 1.3 Extraer información de las imágenes: descriptores de forma, contraste, color, textura… |
| 2. Elegir el algoritmo | 2.1 Viendo los datos y el estado del arte, elegimos **SVM** (por ejemplo) |
| 3. Definir cómo evaluar | 3.1 Elegir una métrica que **penalice más el falso negativo**, para no dejar sin detectar un caso positivo |
| 4. Evaluar en datos nuevos | 4.1 Probar el sistema completo (pasos 1 a 3) sobre datos nunca vistos |

![[ML C1 - estructura proyecto alzheimer.png]]

## Principales retos del ML

- **No tener suficientes datos.**
- **Datos no representativos del problema (sampling bias).** Caso clásico: la portada del *Chicago Tribune* de 1948 ("Dewey defeats Truman") — las encuestas se hacían por teléfono y en esa época la gente de bajos ingresos no tenía teléfono, pero obviamente sí votaba. Relacionado: **survival bias** — la diapositiva lo ilustra con el avión de la Segunda Guerra lleno de impactos: solo se observan los aviones que **volvieron**, así que hay que blindar las zonas *sin* agujeros, no las que los tienen.
- **Mala calidad de los datos.** Buena parte del tiempo de un proyecto se va acá: outliers, datos faltantes, errores en la obtención, ruido.
- **Variables/características irrelevantes.** Seleccionar o crear nuevas características es una parte crítica del proyecto, tanto en el diseño (antes de obtener los datos) como después, al seleccionar o combinar features.
- **Underfitting / overfitting.** Pocos datos + modelo muy complejo → overfitting. Se reduce con más datos, un algoritmo más simple y buenas prácticas.

![[ML C1 - sampling bias.png]]

> [!info] Dónde caen los retos
> Los cuatro primeros son problemas del **paso 1** (obtener y preparar los datos); underfitting/overfitting aparecen en los **pasos 2 a 4**.

## Próxima clase

Todo el proceso desde que definimos el problema hasta que tenemos variables relevantes para resolverlo, es decir el **paso 1: obtener y preparar los datos**.

## Preguntas

- ¿Qué diferencia concreta hay entre online learning y out-of-core learning, si ambos usan mini-batches?
- ¿KNN es instance-based y SVM model-based? ¿Hay algoritmos que combinen las dos estrategias?
- ¿Cómo se elige el learning rate en online learning para balancear adaptación vs. robustez al ruido?
- ¿Por qué en el ejemplo de Alzheimer se penaliza más el falso negativo? ¿Qué métrica concreta se usaría?

[[Machine Learning.base|Machine Learning]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Machine Learning)**

- [[Materia - Machine Learning]] — índice de la materia y sus módulos

**Otras materias**

- **MNA**  [[Resumen MNA]] — la SVD y los cuadrados mínimos son la base matemática de la reducción de dimensionalidad (módulo 4) y de los modelos lineales del módulo 1

<!-- notas-relacionadas:fin -->
