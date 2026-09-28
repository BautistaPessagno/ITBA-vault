---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-09-22
Materia: "[[Economia.base|Economia]]"
Cuatri: "2C"
temas:
  - macroeconomía
  - ciclo y tendencia
  - producto potencial
  - demanda agregada
  - oferta agregada
  - oferta agregada de corto plazo
  - oferta agregada de largo plazo
  - rigidez salarial
  - modelo OA-DA
  - equilibrio macroeconómico
  - shock de demanda
  - shock de oferta
  - estanflación
  - brecha recesiva
  - brecha inflacionaria
  - autorregulación
  - política de estabilización
  - política fiscal
  - política monetaria
  - multiplicador del gasto
  - sector externo
  - ventaja comparativa
  - Heckscher-Ohlin
  - comercio internacional
  - importaciones y exportaciones
  - excedente del consumidor y del productor
  - aranceles
  - proteccionismo
  - balanza de pagos
  - cuenta corriente
  - cuenta financiera
  - tipo de cambio
  - apreciación y depreciación
  - tipo de cambio real
  - tipo de cambio multilateral
  - bienes transables
  - paridad de tasas de interés
  - carry trade
  - restricción externa
  - trilema
  - regímenes cambiarios
---
# Economia - Macro Clase 4 - Oferta y Demanda Agregada y Sector Externo

> Clase 4 de Macroeconomía — *Economía para Ingenieros* (ITBA 2026). El deck son **dos clases pegadas**: primero el **modelo de Oferta y Demanda Agregada** (el que la Clase 3 prometió y no desarrolló), y después el **Sector Externo** (comercio, balanza de pagos, tipo de cambio). El hilo común: en la primera mitad la economía es cerrada y el equilibrio es *interno* (pleno empleo, precios estables); en la segunda se abre y aparece el equilibrio *externo* — y los dos no siempre son compatibles.

## Resumen

Lo que promete la clase:

**Parte 1 — OA–DA**
- ¿Por qué la curva de oferta agregada es distinta en el corto y en el largo plazo?
- ¿Cómo se usa el modelo OA–DA para estudiar fluctuaciones económicas?
- ¿Cómo pueden estabilizar la economía la política fiscal y la monetaria?

**Parte 2 — Sector externo**
- ¿Cuáles son las fuentes de las ventajas comparativas?
- ¿Qué es la balanza de pagos?
- ¿Qué determina los flujos de capital?
- ¿Qué rol juega el tipo de cambio?

### Mapa de la clase

| Bloque | Temas | Sección |
|---|---|---|
| **1. Introducción** | Ciclo vs tendencia, de qué depende la producción, multiplicador | §1 |
| **2. Demanda agregada** | Definición, pendiente negativa, desplazamientos | §2 |
| **3. Oferta agregada** | OA de corto plazo (rigidez salarial), OA de largo plazo, desplazamientos | §3 |
| **4. Modelo OA–DA** | Equilibrio de CP, shocks de demanda y de oferta, equilibrio de LP | §4 |
| **5. Brechas y autocorrección** | Brecha recesiva, brecha inflacionaria, ajuste de salarios | §5 |
| **6. Política de estabilización** | Qué hacer ante cada tipo de shock, aprendizajes | §6 |
| **7. Comercio internacional** | Ventaja comparativa, importaciones, exportaciones, aranceles | §7–§8 |
| **8. Balanza de pagos** | Cuenta corriente, cuenta financiera, dos formatos de tabla | §9 |
| **9. Tipo de cambio** | Nominal, real, multilateral, transables | §10 |
| **10. Movilidad de capitales** | Paridad de tasas de interés | §11 |
| **11. Equilibrio interno y externo** | Tabla de efectos, trilema, regímenes cambiarios | §12–§13 |

---

# Parte 1 — Oferta y Demanda Agregada

## 1. Introducción: ciclo y tendencia

### 1.1 Por qué importa estabilizar

La economía se puede mirar en dos bloques: el **mercado interno** (depende del nivel de actividad y del ingreso local → determina la absorción interna, consumo e inversión → afecta la **demanda** de divisas) y el **comercio exterior** (depende de la actividad y el ingreso externos, de políticas externas y, en economías agro-dependientes, del clima → afecta la **oferta** de divisas).

El **Banco Central** entra en los dos lados:

- La **estabilidad de precios y de crecimiento** permite mejores decisiones de inversión: se pueden proyectar variables y enfrentar la incertidumbre.
- Hace **política monetaria para alinear el producto a su tendencia**.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Tendencia%20y%20ciclo%20del%20PBI%20argentino.png)

*Izquierda: PBI real per cápita de Argentina (log) y su tendencia (punteada). Derecha: el ciclo, como desvío porcentual respecto de la tendencia — llega a ±20%. Fuente: Uribe y Schmitt-Grohé, Open Economy Macroeconomics.*

> [!important] La idea de la slide
> **Mayores desvíos de la tendencia → mayor incertidumbre → menor probabilidad de inversión y crecimiento.** El ciclo no es solo "ruido" alrededor de la tendencia: si es muy volátil, termina afectando la tendencia misma.

### 1.2 Ciclo y tendencia

| Concepto | Qué estudia |
|---|---|
| **Tendencia** | El nivel de largo plazo. Es **estocástica**: no hay certeza de qué pasará. La estudian las **teorías del crecimiento**. |
| **Ciclo** | Las **fluctuaciones en torno a la tendencia**. Las estudian las **teorías del ciclo**. |
| **Política económica** | Su rol es reducir los desvíos del ciclo. |

### 1.3 ¿De qué depende el nivel de producción agregada?

1. **Variaciones de la demanda de bienes** → explica el **ciclo** (corto plazo). Es lo que modela la DA.
2. **La cantidad que puede producir la economía** → el **producto potencial** (la capacidad). Es lo que modela la OA.
3. **Sistema educativo, tasa de ahorro y calidad del Estado** → los determinantes de largo plazo de la **tendencia** (crecimiento).

La pregunta de la clase es: **¿por qué la producción fluctúa alrededor de su nivel potencial?**

### 1.4 Modelo básico de la demanda agregada: el multiplicador

- El **gasto determina la producción/ingreso, y el ingreso determina el gasto** (retroalimentación).
- Un aumento del **gasto autónomo** aumenta la producción **más que 1 a 1** → **efecto multiplicador**.
- El tamaño del multiplicador depende de la **propensión marginal a consumir** y de las **tasas impositivas**.
- Implicancia: la política económica puede **manejar la DA**.

> [!note] Complemento (no está en las slides): la fórmula del multiplicador
> Con propensión marginal a consumir $c$ (fracción de cada peso extra de ingreso que se consume):
> $$k = \frac{1}{1-c}$$
> Si además hay un impuesto proporcional al ingreso con tasa $t$, el ingreso disponible es $(1-t)Y$ y
> $$k = \frac{1}{1-c(1-t)}$$
> Por eso la slide dice que el multiplicador depende de **ambas** cosas: más $c$ → mayor $k$; más $t$ → menor $k$. Ej.: $c=0{,}8$, $t=0$ → $k=5$; con $t=0{,}25$ → $k = 1/(1-0{,}6) = 2{,}5$.

---

## 2. Demanda agregada (DA)

La **curva de demanda agregada** refleja la relación entre el **nivel de precios agregado** y la **cantidad de producción agregada** que demandan hogares, empresas, Estado y resto del mundo. Muestra la producción total demandada para cada nivel de precios.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Curva%20de%20demanda%20agregada.png)

*Un movimiento hacia abajo a lo largo de la DA: menor nivel de precios ↔ mayor producción demandada. Ejes: nivel de precios (deflactor del PIB) y PIB real.*

### 2.1 ¿Por qué tiene pendiente negativa?

Se parte de la identidad macroeconómica básica:

$$\text{PBI} = C + I + G + X - M$$

($C$ consumo, $I$ inversión, $G$ gasto público, $X$ exportaciones, $M$ importaciones.) La suma es la cantidad de bienes y servicios finales producidos que se demandan en un período.

La DA tiene pendiente negativa porque **un aumento del nivel de precios reduce $C$, $I$ y $X$** — lo último porque la suba de precios hace a la economía **más cara respecto al resto del mundo**.

> [!tip] Los tres canales, con nombre
> La slide los enumera; en la bibliografía tienen nombre propio:
> - **Efecto riqueza** ($C$): con precios más altos, el poder adquisitivo de los activos de los hogares cae → consumen menos.
> - **Efecto tasa de interés** ($I$): con precios más altos la gente necesita más dinero para transacciones — la demanda de dinero es proporcional al nivel de precios (ver [Economia - Macro Clase 3 - Política Monetaria](Economia%20-%20Macro%20Clase%203%20-%20Política%20Monetaria.md) §9.4) → sube la tasa de interés → cae la inversión.
> - **Efecto tipo de cambio real** ($X - M$): precios internos más altos con tipo de cambio dado = economía más cara (ver §10) → caen las exportaciones netas.

> [!warning] No es la demanda de micro
> En micro la demanda tiene pendiente negativa porque el bien se encarece **relativo a otros**. Acá suben **todos** los precios a la vez, así que ese argumento no sirve: la pendiente sale de los tres efectos de arriba.

### 2.2 Movimiento *sobre* la curva vs desplazamiento *de* la curva

Igual que en micro:

- **Movimiento a lo largo de la DA** → cambia la cantidad agregada demandada porque **cambió el nivel de precios**.
- **Desplazamiento de la DA** → cambia la cantidad demandada **para cada nivel de precios**, por un factor **distinto del precio**.

### 2.3 Factores que desplazan la DA

| Factor | Mecanismo |
|---|---|
| **Expectativas** | $C$ e $I$ dependen no solo del ingreso/fondos actuales sino de lo que se espera a futuro |
| **Riqueza** | El consumo depende del valor de los activos de los hogares (bolsa, propiedades) |
| **Stock de capital físico** | Si las empresas ya tienen mucho capital ocioso, tienen menos incentivo a invertir |
| **Política fiscal** | $G$ directamente; impuestos y transferencias vía el **ingreso disponible** → consumo |
| **Política monetaria** | $\uparrow M \Rightarrow$ más capacidad prestable $\Rightarrow \downarrow r \Rightarrow \uparrow I, \uparrow C \Rightarrow$ DA a la derecha |

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Desplazamientos%20de%20la%20DA.png)

*(a) Aumento de la DA: $AD_1 \to AD_2$ a la derecha. (b) Disminución: a la izquierda.*

---

## 3. Oferta agregada (OA)

La **curva de oferta agregada** muestra la relación entre el **nivel de precios** y la **producción agregada**.

### 3.1 OA de corto plazo: pendiente positiva

A corto plazo hay una **relación positiva**: céteris paribus, un aumento del nivel de precios provoca un aumento de la producción agregada, y viceversa.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Curva%20de%20oferta%20agregada%20de%20corto%20plazo.png)

*EEUU 1929 → 1933: al caer el nivel de precios (11,9 → 8,9) la economía se mueve hacia abajo a lo largo de la SRAS y la producción cae (865 → 636 miles de millones de dólares de 2000).*

**Por qué.** Cada productor hace un análisis costo-beneficio. Si sube el nivel de precios, recibe un precio mayor pero **muchos de sus costos son fijos o rígidos**, así que el costo no sube en la misma proporción:

$$\text{Beneficio por unidad} = \text{Precio} - \text{Costo de producción medio}$$

→ sube el beneficio por unidad → produce más. Al revés si los precios bajan.

**La rigidez clave es el salario.** El salario nominal se fija por contrato y es inflexible: **rigidez salarial**.

> [!important] Qué distingue el corto del largo plazo
> **El tiempo que tardan los salarios nominales en volverse flexibles.** No es un plazo calendario fijo: es "hasta que se renegocian los salarios".

### 3.2 El mecanismo, paso a paso

La slide lo escribe así ($\bar w$ = salario fijo):

**Corto plazo** (salario fijo):
$$\uparrow \text{Beneficio} = \uparrow P - \bar w \cdot L \quad\Longrightarrow\quad \text{mayor producción, se contrata más: } \uparrow \text{Beneficio} = \uparrow P - \bar w \cdot \uparrow L$$

**Largo plazo** (el exceso de demanda de trabajo hace subir los salarios):
$$\downarrow \text{Beneficio} = \uparrow P - \uparrow w \cdot L \quad\Longrightarrow\quad \text{menor producción (vuelve al potencial): } \downarrow \text{Beneficio} = \uparrow P - \uparrow w \cdot \downarrow L$$

> [!warning] Notación de la slide
> La slide llama "beneficio **por unidad**" a $P - w \cdot L$, pero $w \cdot L$ es el costo laboral **total**, no por unidad. Escrito prolijo: beneficio total $= P \cdot Y - w \cdot L$, o por unidad $= P - \dfrac{w L}{Y}$. La **lógica** de la slide está bien (lo que importa es que con $w$ fijo el precio sube y el costo no); solo mezcla las dos formas.

### 3.3 OA de largo plazo (LRAS)

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20OA%20de%20corto%20y%20largo%20plazo.png)

- La **LRAS es vertical** en el **producto potencial** $Y_P$: a largo plazo, con salarios flexibles, la producción **no depende del nivel de precios**.
- $Y_P$ es el **nivel de producto que no acelera la inflación**.
- En el gráfico la economía está en $A_1$, con $Y_1 > Y_P$: está produciendo por encima del potencial *sobre la SRAS de corto plazo*. Eso no se sostiene: los salarios van a subir.

### 3.4 Desplazamientos de la OA de corto plazo

Misma distinción: **movimiento a lo largo** (cambia el nivel de precios) vs **desplazamiento** (cambia otro factor). Derecha = más OA para cada nivel de precios; izquierda = menos.

| Factor | Efecto sobre la SRAS |
|---|---|
| **Precios de insumos** (materias primas, petróleo) | ↑ precio del insumo → ↑ costos en toda la economía → SRAS a la **izquierda** |
| **Salarios nominales** | ↑ salario → ↑ costos → SRAS a la **izquierda** |
| **Productividad** | ↑ productividad → ↓ costos, ↑ beneficios → SRAS a la **derecha** |

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Desplazamientos%20de%20la%20OA%20de%20corto%20plazo.png)

### 3.5 Del corto al largo plazo

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20PBI%20efectivo%20vs%20potencial%20EEUU.png)

*PBI real efectivo (azul) vs potencial (naranja), EEUU 1990–2018. Casi nunca coinciden: hay tramos con el efectivo por encima y tramos por debajo (p. ej. la caída post-2008). Krugman/Wells, Essentials of Economics 5e.*

El PBI casi siempre está **por encima o por debajo** del potencial. Mientras la economía está solo sobre la SRAS (fuera del potencial), el **nivel de desempleo** fuerza un **ajuste de los salarios nominales** que la devuelve al potencial:

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Ajuste%20de%20salarios%20del%20corto%20al%20largo%20plazo.png)

| Situación | Mercado de trabajo | Salarios nominales | SRAS |
|---|---|---|---|
| (a) $Y_1 > Y_P$ | Desempleo bajo, exceso de demanda de trabajo | **Suben** | Se desplaza a la **izquierda** ($SRAS_1 \to SRAS_2$) |
| (b) $Y_1 < Y_P$ | Desempleo alto | **Bajan** | Se desplaza a la **derecha** |

---

## 4. El modelo OA–DA

Analiza las fluctuaciones estudiando **las dos curvas juntas**. Tiene:

- **Equilibrio macroeconómico de corto plazo**, con dos tipos de perturbación:
  - desplazamientos de la DA → **shocks de demanda**
  - desplazamientos de la OA → **shocks de oferta**
- **Equilibrio macroeconómico de largo plazo**
- Permite ilustrar el **impacto de las políticas macroeconómicas**.

### 4.1 Equilibrio macro de corto plazo

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Equilibrio%20macro%20de%20corto%20plazo.png)

La intersección de **DA** y **SRAS** determina el equilibrio de corto plazo $E_{CP}$, con nivel de precios $P_E$ y producción $Y_E$.

- **Exceso de demanda** → **sube** el nivel de precios.
- **Exceso de oferta** → **baja** el nivel de precios.

### 4.2 Shocks de demanda

**Shock de demanda** = cualquier acontecimiento que desplace la DA (expectativas, riqueza, políticas…).

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Shocks%20de%20demanda.png)

| Shock | DA | La economía se mueve… | $P$ | $Y$ |
|---|---|---|---|---|
| **Positivo** | → derecha | hacia **arriba** sobre la SRAS | ↑ | ↑ |
| **Negativo** | ← izquierda | hacia **abajo** sobre la SRAS | ↓ | ↓ |

> [!tip] La firma de un shock de demanda
> **Precios y producción se mueven en la misma dirección.**

### 4.3 Shocks de oferta

**Shock de oferta** = cualquier acontecimiento que desplace la SRAS: precio de materias primas, salarios nominales, productividad.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Shocks%20de%20oferta.png)

| Shock | SRAS | La economía se mueve… | $P$ | $Y$ |
|---|---|---|---|---|
| **Positivo** | → derecha | hacia **abajo** sobre la DA | ↓ | ↑ |
| **Negativo** | ← izquierda | hacia **arriba** sobre la DA | ↑ | ↓ |

> [!important] Estanflación
> La combinación de **caída de la producción + inflación** que produce un shock de oferta negativo se llama **estanflación**. Firma de un shock de oferta: **precios y producción se mueven en direcciones opuestas.**

### 4.4 Equilibrio macro de largo plazo

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Equilibrio%20macro%20de%20largo%20plazo.png)

Cuando **DA, SRAS y LRAS se cortan en el mismo punto** $E_{LP}$, la producción de equilibrio es igual a la **producción potencial** $Y_P$: **equilibrio macroeconómico de largo plazo**.

---

## 5. Brechas y autocorrección

¿Qué pasa si un shock saca a la economía de su equilibrio de largo plazo?

### 5.1 Brecha recesiva (shock de demanda negativo)

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Brecha%20recesiva%20y%20autocorreccion.png)

1. Se parte de $E_1$ (equilibrio de CP y de LP). Cae la DA: $AD_1 \to AD_2$.
2. Precios y producción caen a $P_2$, $Y_2$ ($E_2$). Como $Y_2 < Y_P$ hay una **brecha recesiva** (*output gap*).
3. El **alto desempleo** hace **bajar los salarios nominales** → la SRAS se desplaza a la **derecha** ($SRAS_1 \to SRAS_2$) hasta $E_3$: la economía vuelve al **producto potencial**, pero con un **nivel de precios menor** ($P_3$).

### 5.2 Brecha inflacionaria (shock de demanda positivo)

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Brecha%20inflacionaria%20y%20autocorreccion.png)

1. Sube la DA: $AD_1 \to AD_2$.
2. Precios y producción suben a $P_2$, $Y_2$. Como $Y_2 > Y_P$ hay una **brecha inflacionaria**.
3. El **bajo desempleo** hace **subir los salarios** → la SRAS se desplaza a la **izquierda** hasta $E_3$: vuelve al potencial con un **nivel de precios mayor** ($P_3$).

| | Brecha recesiva | Brecha inflacionaria |
|---|---|---|
| Producción | $Y < Y_P$ | $Y > Y_P$ |
| Desempleo | Alto | Bajo |
| Salarios nominales | Bajan | Suben |
| SRAS | → derecha | ← izquierda |
| Punto final | $Y_P$ con **menor** $P$ | $Y_P$ con **mayor** $P$ |

> [!question] "¿En qué momento intervino el gobierno?"
> La slide lo pregunta a propósito: **en ningún momento**. Todo el ajuste de §5 lo hacen los salarios solos. La economía se **autorregula** — el debate (§6) es cuánto tarda y si conviene acelerarlo.

> [!tip] Relación con la Clase 3
> Es el mismo esquema que la **neutralidad del dinero**: a largo plazo, un cambio en la DA solo termina moviendo el nivel de precios, no el producto. Ver [Economia - Macro Clase 3 - Política Monetaria](Economia%20-%20Macro%20Clase%203%20-%20Política%20Monetaria.md) §14.

---

## 6. Políticas macroeconómicas de estabilización

A largo plazo la economía se autorregula. Pero en vez de esperar, las autoridades pueden aplicar una **política de estabilización**: usar la **política monetaria y la fiscal** para devolver la economía a su senda de largo plazo.

- Estabilizar = **activar un shock de demanda para contrarrestar otro** (reducir la severidad de las recesiones y refrenar las expansiones).
- Ante **shocks de oferta** **no hay políticas unívocas** para mover la SRAS en el corto plazo.

### 6.1 ¿Qué hacer ante un shock de demanda?

- La política monetaria o fiscal puede reaccionar a la caída de la DA, **refrenando la presión recesiva**.
- ¿Siempre? **No necesariamente**: algunas políticas aumentan el **déficit**, y querer estabilizar puede generar **más inestabilidad** — *el cómo importa*.
- La mayoría de los economistas cree que lo correcto es **balancear de modo contracíclico** estos shocks.

### 6.2 ¿Qué hacer ante un shock de oferta? El dilema

Un shock de oferta negativo sube **a la vez** precios y desempleo → **dilema político**:

| Objetivo | Qué hay que hacer con la DA | Costo |
|---|---|---|
| Estabilizar el **desempleo** | Aumentar la DA | **Más inflación** |
| Estabilizar los **precios** | Reducir la DA | **Más desempleo** |

> [!important] Por qué es un dilema y el shock de demanda no
> Con un shock de demanda, compensar la DA arregla precios **y** producción al mismo tiempo (se mueven juntos). Con uno de oferta, la política de demanda solo puede elegir **cuál** de los dos problemas empeorar.

### 6.3 Shocks de oferta vs shocks de demanda

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Desempleo%20EEUU%20y%20shocks%20de%20oferta.png)

*Desempleo en EEUU 1948–2017. Los picos después de 1973 (guerra árabe-israelí) y 1979 (revolución iraní) son shocks de oferta petroleros. Krugman/Wells 5e.*

- Los **shocks de demanda son más comunes**.
- Los **shocks de oferta negativos son poco frecuentes, pero más costosos en promedio**.

### 6.4 Aprendizaje macro

- Es cierto que la economía **se autorregula en el largo plazo**…
- …pero la mayoría de los economistas piensa que **puede tardar una década o más**.
- Rol de la política de estabilización: **reducir la severidad de las recesiones y refrenar las expansiones fuertes**.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Gran%20Depresion%20vs%20Gran%20Recesion.png)

*Producción industrial mundial desde el pico: Gran Depresión (desde junio 1929) vs Gran Recesión (desde febrero 2008). En 2008 hubo política de estabilización agresiva y la caída fue mucho menor y más corta. Krugman/Wells 5e.*

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20DA%20en%20la%20Gran%20Depresion.png)

*EEUU 1929–1942: de 1929 a 1933 caen **juntos** precios y producción → firma de un **shock de demanda negativo** (§4.2). La recuperación posterior es lenta. (La slide acompaña el gráfico con una foto de Ben Bernanke, estudioso de la Gran Depresión y presidente de la Fed en 2008.)*

### 6.5 Conclusiones de la Parte 1

- La **SRAS tiene pendiente positiva porque los salarios son rígidos**. La **LRAS es vertical** porque la producción potencial no depende de los precios.
- Los **shocks de demanda** (cambios en el gasto) o **de oferta** (cambios en los costos) desplazan las curvas y alteran el equilibrio de corto plazo.
- Después de un shock, la política macro **puede reducir la volatilidad**, *si no se cometen errores de política económica*.

---

# Parte 2 — Sector externo

## 7. Ventaja comparativa y comercio internacional

¿Por qué hay comercio internacional y por qué se cree que es beneficioso?

> [!important] Ventaja comparativa
> Un país tiene **ventaja comparativa** en la producción de un bien si el **costo de oportunidad** de producirlo es **menor** que el de otro país.

Ojo: se compara **costo de oportunidad**, no productividad absoluta (eso sería ventaja *absoluta*). Un país puede ser peor produciendo todo y aun así tener ventaja comparativa en algo. (Costo de oportunidad y FPP: [Economia Intro](Economia%20Intro.md).)

### 7.1 Fuentes de la ventaja comparativa

| Fuente | Idea |
|---|---|
| **Clima** | Determina qué se puede producir (muy relevante para economías agro-exportadoras). |
| **Dotación de factores** — modelo de **Heckscher-Ohlin** | Un país tiene ventaja comparativa en los bienes cuya producción es **intensiva en el factor que le abunda**. |
| **Tecnología** | Diferencias en técnicas de producción y en capital humano. |

---

## 8. Efectos del comercio sobre un mercado

Todo este bloque es **análisis de excedentes de micro** (ver [Economia - Oferta, Demanda y Mercado](Economia%20-%20Oferta,%20Demanda%20y%20Mercado.md) y [Economia - Resumen Microeconomía](Economia%20-%20Resumen%20Microeconomía.md) §4.6–4.8) aplicado a un mercado que se abre. El supuesto clave: el país es **pequeño** y toma el **precio internacional $P_I$ como dado** — puede comprar o vender todo lo que quiera a ese precio.

### 8.1 Importaciones ($P_I < P_A$)

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Efecto%20de%20las%20importaciones.png)

Si el precio internacional es **menor** que el de autarquía $P_A$, a los importadores les conviene traer y vender en el mercado interno. El precio interno baja a $P_I$:

- La **cantidad demandada** interna **sube** de $Q_A$ a $Q_D$.
- La **cantidad ofrecida** interna **baja** de $Q_A$ a $Q_S$.
- **Importaciones** $= Q_D - Q_S$.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Excedentes%20con%20importaciones.png)

| | Ganancias | Pérdidas |
|---|---|---|
| Excedente del consumidor | $X + Z$ | |
| Excedente del productor | | $-X$ |
| **Cambio del excedente total** | $+Z$ | |

Cae el precio → **consumidores ganan**, **productores pierden**. $X$ es una transferencia de productores a consumidores; $Z$ es la **ganancia neta** de abrir el mercado.

### 8.2 Exportaciones ($P_I > P_A$)

Si el precio internacional es **mayor** que el de autarquía, los productores venden afuera y el precio interno **sube** a $P_I$: la cantidad demandada interna **baja** a $Q_D$, la ofrecida **sube** a $Q_S$, y **exportaciones** $= Q_S - Q_D$.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Excedentes%20con%20exportaciones.png)

| | Ganancias | Pérdidas |
|---|---|---|
| Excedente del consumidor | | $-X$ |
| Excedente del productor | $X + Z$ | |
| **Cambio del excedente total** | $+Z$ | |

> [!tip] La simetría
> En los dos casos la apertura **aumenta el excedente total** ($+Z$), pero **genera ganadores y perdedores**: con importaciones ganan los consumidores; con exportaciones ganan los productores. Esa es la raíz política del proteccionismo (§8.3) y, en Argentina, de las retenciones: la exportación sube el precio interno de los alimentos.

### 8.3 Libre comercio vs proteccionismo

- **Libre comercio**: el gobierno no intenta reducir ni aumentar las exportaciones e importaciones que surgen de la oferta y la demanda.
- **Proteccionismo**: **barreras al comercio** (aranceles, cuotas, retenciones) para proteger a los productores nacionales. También hay políticas para **fomentar** exportaciones (subsidios a la exportación).

### 8.4 Efecto de un arancel

Un **arancel** es un **impuesto a las importaciones**. Recauda, pero su objetivo habitual es desincentivar importaciones y proteger a los productores locales.

Con el arancel el precio interno sube de $P_I$ a $P_T = P_I + \text{arancel}$:

- La cantidad **ofrecida** interna **aumenta** ($Q_S \to Q_{ST}$).
- La cantidad **demandada** interna **disminuye** ($Q_D \to Q_{DT}$).
- Las **importaciones caen**: de $Q_D - Q_S$ a $Q_{DT} - Q_{ST}$.

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Efecto%20de%20un%20arancel.png)

| Área | Qué representa |
|---|---|
| $A$ | **Ganancia de los productores** por el precio interno más alto |
| $A + B + C + D$ | **Pérdida de los consumidores** |
| $C$ | **Recaudación del Estado** (arancel × importaciones que quedan) |
| $B + D$ | **Pérdida neta de excedente total** (pérdida irrecuperable) |

$$\Delta \text{Excedente total} = \underbrace{A}_{\text{productores}} + \underbrace{C}_{\text{Estado}} - \underbrace{(A+B+C+D)}_{\text{consumidores}} = -(B + D)$$

> [!tip] Qué es cada triángulo
> $B$ = **ineficiencia en la producción**: se producen localmente unidades que costaban más que $P_I$ (se podían importar más baratas). $D$ = **ineficiencia en el consumo**: consumidores que valoraban el bien por encima de $P_I$ dejan de comprarlo. Es la misma lógica que la pérdida irrecuperable de un impuesto en micro.

---

## 9. La balanza de pagos

La **balanza de pagos (BP)** es un **resumen contable** de las transacciones de un país con el resto del mundo: ingresos y egresos del/al extranjero.

| Cuenta | Qué registra |
|---|---|
| **Cuenta corriente** | **Balanza comercial** ($X - M$ de bienes y servicios) + **rentas de los factores** (intereses, utilidades) + **transferencias** internacionales |
| **Cuenta financiera** | **Compra y venta de activos financieros** con el exterior |

- **Déficit de cuenta corriente**: se pagó al exterior (bienes, servicios, rentas, transferencias) más de lo que se recibió.
- **Superávit de cuenta financiera**: se **vendieron** más activos al extranjero de los que se compraron (entró financiamiento).

La identidad:

$$\text{Cuenta corriente} + \text{Cuenta financiera} = 0 \quad\Longleftrightarrow\quad \text{Cuenta corriente} = -\,\text{Cuenta financiera}$$

En la práctica no se cumple a la perfección por **discrepancias estadísticas**.

> [!important] La lógica de la identidad
> Si un país gasta en el exterior más de lo que gana (déficit de CC), la diferencia **tiene que financiarse**: vendiendo activos o endeudándose (superávit de CF). No hay otra forma de pagar los dólares que faltan.

### 9.1 Ejemplo 1 — formato "pagos del / al extranjero"

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Balanza%20de%20pagos%20EEUU.png)

| | Saldo neto |
|---|---|
| (1) Bienes y servicios | $1827 - 2522 = -695$ |
| (2) Rentas de factores | $765 - 646 = +119$ |
| (3) Transferencias | $-128$ |
| **Cuenta corriente** $(1+2+3)$ | $\mathbf{-704}$ |
| (4) Activos públicos | $487 - 534 = -47$ |
| (5) Activos privados | $47 - (-534) = +581$ |
| **Cuenta financiera** $(4+5)$ | $\mathbf{+534}$ |
| **Total** | $\mathbf{-170}$ |

> [!check] Verificado
> Todas las sumas de la tabla cierran. El total $-704 + 534 = -170 \neq 0$ es justamente la **discrepancia estadística** que menciona la slide.

### 9.2 Ejemplo 2 — formato BCRA / FMI (débito, crédito y variación de activos/pasivos)

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Balanza%20de%20pagos%20formato%20BCRA.png)

| Concepto | Débito | Crédito | Saldo |
|---|---|---|---|
| A. Exportaciones de bienes y servicios | | 1.000 | 1.000 |
| B. Importaciones de bienes y servicios | 700 | | −700 |
| C. Giro de utilidades a casas matrices *(ingreso primario)* | 50 | | −50 |
| D. Intereses de deuda externa *(ingreso primario)* | 150 | | −150 |
| E. Remesas de argentinos desde el exterior *(ingreso secundario)* | | 20 | 20 |
| **Cuenta corriente** | **900** | **1.020** | **120** |
| F. Venta del pase de un futbolista *(cuenta capital)* | | 10 | 10 |
| **Necesidad / capacidad de financiamiento (CC + CK)** | | | **130** |

| Cuenta financiera | Var. activos | Var. pasivos | Saldo (A − P) |
|---|---|---|---|
| Inversión directa: empresa extranjera invierte en filial local | | 30 | −30 |
| Inversión de cartera: fondo extranjero compra acciones locales | | 15 | −15 |
| Otra inversión: hogares compran divisas | 20 | | 20 |
| Otra inversión: vencimiento de deuda externa del gobierno | | −50 | 50 |
| Activos de reserva: el BCRA compra reservas | 105 | | 105 |
| **Total cuenta financiera** | **125** | **−5** | **130** |

> [!bug] La slide pone saldo de cuenta corriente = 130; es **120**
> Créditos − débitos $= 1.020 - 900 = 120$, y la suma de los saldos de las filas da lo mismo: $1.000 - 700 - 50 - 150 + 20 = 120$. El **130** es la **necesidad/capacidad de financiamiento**, que suma además la cuenta capital: $120 + 10 = 130$. La slide copió ese 130 en la fila de cuenta corriente.

> [!warning] Los dos ejemplos usan convenciones de signo opuestas para la cuenta financiera
> - **Ejemplo 1**: la CF se mide como **ventas netas** de activos (entrada de fondos = +). Por eso $CC + CF = 0$.
> - **Ejemplo 2** (manual del FMI, el que usa el INDEC/BCRA): la CF se mide como **adquisición neta de activos − pasivos netos incurridos**. Ahí la identidad es
> $$CC + CK = CF$$
> ($130 = 130$ en el ejemplo). Es la misma economía contada al revés: un superávit de CC (+120) significa que el país **acumula** activos externos (reservas, dólares de los hogares) o cancela deuda. Si en un ejercicio te dan una tabla, mirá qué convención usa antes de aplicar "CC = −CF".

> [!check] Verificado
> Cuenta financiera: activos $20 + 105 = 125$; pasivos $30 + 15 - 50 = -5$; saldo $125 - (-5) = 130$; suma de saldos por fila $-30 - 15 + 20 + 50 + 105 = 130$. Cierra con la necesidad de financiamiento (130), sin errores y omisiones.

> [!tip] Cómo leer el ejemplo argentino
> Con superávit de CC, los dólares que entran terminan en el BCRA (compra 105 de reservas), en los hogares que compran divisas (20) y en cancelar deuda del gobierno (50). La slide de síntesis lo resume: **el resultado de la cuenta corriente muestra la capacidad de una economía de generar dólares**.

---

## 10. El tipo de cambio

Los bienes, servicios y activos de un país se pagan en su moneda, así que el comercio internacional necesita un **mercado de divisas**, donde se intercambian monedas.

**Tipo de cambio** = precio de una moneda en términos de otra; acá, **cantidad de moneda local por unidad de moneda extranjera**:

$$TC = \frac{X \text{ pesos}}{\text{dólar}}$$

| Movimiento | $TC$ | Nombre |
|---|---|---|
| La moneda local **gana** valor | **baja** | **Apreciación** |
| La moneda local **pierde** valor | **sube** | **Depreciación** |

> [!warning] Contraintuitivo
> Con esta definición (pesos por dólar), **TC más alto = peso más débil**. "Sube el dólar" = el peso se deprecia.

Los movimientos del tipo de cambio importan porque, céteris paribus, cambian los **precios relativos** de bienes y servicios entre países.

### 10.1 Tipo de cambio real (TCR)

Para tener en cuenta las **diferencias de inflación** entre países, se usa el **tipo de cambio real**: el nominal ajustado por la relación entre niveles de precios.

$$TCR = TC \cdot \frac{IPC^*}{IPC}$$

($IPC^*$ = índice de precios externo, $IPC$ = índice de precios local.)

El TCR es el **precio relativo de los bienes argentinos frente a los del resto del mundo**: dice qué tan **barata o cara** es nuestra economía. Es la medida de **competitividad**.

| | Significado |
|---|---|
| $\Delta TCR > 0$ | Los bienes **domésticos** se abaratan relativo a los externos → ganan competitividad |
| $\Delta TCR < 0$ | Los bienes **internacionales** se abaratan relativo a los locales |

> [!example] Por qué el nominal no alcanza
> Si el peso se deprecia 30% ($TC$ ×1,3) pero la inflación local es 30% y la externa 0%, entonces $TCR$ queda igual ($1{,}3 \cdot 1/1{,}3 = 1$): la economía **no** se abarató nada. Solo una depreciación **mayor** que el diferencial de inflación mejora la competitividad.

El TCR importa porque guía cómo productores y consumidores **asignan recursos y presupuesto**, en particular entre:

- **Bienes transables**: compiten con los productos que se producen internacionalmente.
- **Bienes no transables**: solo compiten en el ámbito local (servicios, construcción…).

### 10.2 Tipo de cambio multilateral

Un país comercia con muchos socios; el multilateral pondera los bilaterales (típicamente por peso de cada socio en el comercio):

$$TC_{\text{multilateral}} = \sum_j TC_{\text{bilateral},\,j} \cdot w_j \qquad TCR_{\text{multilateral}} = \sum_j TCR_{\text{bilateral},\,j} \cdot w_j$$

---

## 11. Movilidad de capitales: paridad de tasas de interés

Con **libre movilidad de capitales**, la condición de equilibrio que rige las decisiones de inversión es la **paridad de interés descubierta**:

$$i = i^* + \Delta TC^e + RP$$

| Símbolo | Significado |
|---|---|
| $i$ | Tasa de interés local |
| $i^*$ | Tasa de interés internacional |
| $\Delta TC^e$ | **Depreciación esperada** de la moneda local |
| $RP$ | **Riesgo país** (prima de riesgo — ver [Economia - Macro Clase 3 - Política Monetaria](Economia%20-%20Macro%20Clase%203%20-%20Política%20Monetaria.md) §4.4–4.5) |

Lectura: invertir en pesos tiene que rendir lo mismo que invertir afuera **más** lo que se espera que se deprecie el peso **más** una compensación por el riesgo. Si no se cumple, el **arbitraje** la hace cumplir:

- $i > i^* + \Delta TC^e + RP$ → conviene pasarse a pesos → **carry trade** (entran capitales, se aprecia el peso).
- $i < i^* + \Delta TC^e + RP$ → conviene salir a dólares / **comprar dólar futuro**.

> [!note] Es una aproximación
> La versión exacta es multiplicativa, $(1+i) = (1+i^*)(1+\Delta TC^e)(1+RP)$; la suma es la aproximación de primer orden. Es el mismo problema que la aproximación de Fisher en la Clase 3: con tasas altas (Argentina) el error deja de ser despreciable.

> [!tip] Por qué dice "descubierta"
> Porque el inversor **no se cubre** del riesgo cambiario: apuesta a la depreciación *esperada*. En la versión cubierta, $\Delta TC^e$ se reemplaza por la prima del dólar futuro.

---

## 12. Equilibrio interno y externo

### 12.1 Cómo afectan las perturbaciones al ingreso y a las exportaciones netas

| | ↑ Gasto nacional | ↑ Ingreso foráneo | Depreciación real (↑ TCR) |
|---|---|---|---|
| **Ingreso** | + | + | + |
| **Exportaciones netas** | − | + | + |

*(Tabla 12-2 de la slide.)*

> [!tip] La columna que genera el conflicto
> Un aumento del **gasto nacional** sube el ingreso pero **empeora** las exportaciones netas (parte del gasto se va en importaciones). Es la versión formal de la síntesis: *"una política puede solucionar un desequilibrio y agravar otro"*. En cambio, la depreciación real mejora las dos cosas… pero a costa de algo que no está en la tabla: el salario real (§12.2).

### 12.2 El trilema de la restricción externa

Con la **política fiscal** afectando el **nivel** del gasto y el **TCR** su **composición** (bienes locales vs importados), los hacedores de política enfrentan un **trilema** ("restricción externa al crecimiento"). Tres objetivos, que no se pueden tener los tres a la vez:

1. **Pleno empleo**
2. **Nivel de salarios reales** (alto)
3. **Equilibrio externo**

> [!important] Por qué no entran los tres
> - **Pleno empleo + salarios reales altos** → mucho gasto y TCR bajo (salarios altos en dólares) → suben importaciones, caen exportaciones → **déficit externo**.
> - **Pleno empleo + equilibrio externo** → hace falta TCR alto (competitividad) → **salario real bajo** (lo importado y lo exportable, como alimentos, se encarece relativo al salario).
> - **Salarios reales altos + equilibrio externo** → la única forma de no importar de más es con **menos actividad** → **desempleo**.
>
> Es la mecánica detrás de los ciclos de "stop and go" argentinos: crecimiento → falta de dólares → devaluación → caída del salario real y de la actividad.

---

## 13. Sistemas y regímenes de tipo de cambio

Los sistemas cambiarios dependen de **si el Banco Central interviene o no** en el mercado de divisas:

| Tipo | Variante | Qué hace el BC |
|---|---|---|
| **Fijo** | | Se compromete a un valor y compra/vende divisas para sostenerlo |
| **Flexible** | **Flotación libre** (pura) | No interviene: el mercado define el TC |
| | **Flotación administrada** (sucia) | Deja flotar pero interviene para suavizar movimientos |

![](Attachments/Economia%20-%20Macro%20Clase4%20-%20Regimenes%20cambiarios.png)

El espectro, de menos a más flexible: unión monetaria → adopción de moneda → junta monetaria → tipo de cambio fijo → zona objetivo → *crawling peg* → flotación sucia → flotación pura.

| A medida que el régimen es más flexible… | |
|---|---|
| Flexibilidad del tipo de cambio | ↑ |
| Pérdida de autonomía de la política monetaria | ↓ (con flotación el BC recupera la política monetaria) |
| Impacto anti-inflacionario | ↓ (máximo en los regímenes duros) |
| Credibilidad del compromiso cambiario | ↓ (máxima en los regímenes duros) |

> [!question] Actividad: ¿dónde van estos casos?
> - **Dolarización** → **adopción de moneda** (el país usa directamente la moneda de otro, p. ej. Ecuador). Máxima pérdida de autonomía monetaria.
> - **Convertibilidad** (Argentina 1991–2001, 1 peso = 1 dólar, con la base monetaria respaldada por reservas) → **junta monetaria** (*currency board*): moneda propia, pero el BC no puede emitir sin respaldo en divisas.

> [!tip] Conexión con la Clase 3
> Elegir un régimen cambiario es elegir un **ancla nominal**: la convertibilidad usaba el tipo de cambio como ancla; un régimen de flotación necesita otra (metas de inflación). Ver [Economia - Macro Clase 3 - Política Monetaria](Economia%20-%20Macro%20Clase%203%20-%20Política%20Monetaria.md) §12–13.

---

## 14. Síntesis de la Parte 2

- El comercio internacional surge de las **ventajas comparativas**.
- La economía interna se ve alterada por efectos internos y también por **shocks externos**.
- Hay una disyuntiva entre **equilibrio interno** (pleno empleo) y **externo** (balanza de pagos): una política puede solucionar un desequilibrio y agravar otro.
- La **balanza de pagos** resume todas las transacciones (flujos) con el resto del mundo.
- Los **flujos de capital** responden a las diferencias de **tasas de interés**, de **devaluación esperada** y de **riesgo** (paridad de tasas).
- El resultado de la **cuenta corriente** muestra la **capacidad de generar dólares** de una economía.
- Hay distintos **sistemas cambiarios** según la intervención (o no) del BC.
- El **tipo de cambio real** es la medida de **competitividad** de la producción del país.

---

## Tabla de errores y observaciones de las slides

| Slide | Qué dice | Qué debería decir |
|---|---|---|
| "La balanza de pagos" (formato BCRA) | Saldo de **cuenta corriente = 130** | **120** ($1.020 - 900$). 130 es CC + cuenta capital (necesidad de financiamiento) |
| "¿Por qué tiene pendiente positiva la OA?" | "Beneficio **por unidad** $= P - w \cdot L$" | $w \cdot L$ es costo total; por unidad sería $P - wL/Y$ (la lógica igual vale) |
| "La balanza de pagos" (las dos tablas) | Usa "CC + CF = 0" y después una tabla con la convención FMI | Con la convención FMI la identidad es $CC + CK = CF$; son signos opuestos para la CF, no una contradicción |

---

## Preguntas de repaso

- ¿Por qué la SRAS tiene pendiente positiva y la LRAS es vertical? ¿Qué variable marca la diferencia entre corto y largo plazo?
- Nombrá los tres canales por los que la DA tiene pendiente negativa. ¿Por qué no sirve el argumento de la demanda de micro?
- Con $c = 0{,}75$ y $t = 0{,}2$, ¿cuánto vale el multiplicador? ¿Y sin impuestos?
- Clasificá: suba del petróleo, caída de la bolsa, aumento del gasto público, mejora tecnológica. ¿Shock de oferta o de demanda, positivo o negativo? ¿Qué pasa con $P$ e $Y$?
- Dibujá una brecha recesiva y explicá cómo se cierra **sin** intervención. ¿Con qué nivel de precios termina?
- ¿Por qué un shock de oferta negativo pone a la política en un dilema y uno de demanda no?
- En el gráfico de un arancel, ¿qué área gana el Estado, cuál los productores y cuál se pierde? ¿Qué representan $B$ y $D$?
- Abrir un mercado a las importaciones aumenta el excedente total. ¿Entonces por qué hay proteccionismo?
- Un país tiene déficit de cuenta corriente de 50. ¿Qué tiene que pasar con la cuenta financiera (convención "ventas netas")? ¿Y con la convención del FMI?
- El peso se deprecia 20% en un año con inflación local 35% e internacional 3%. ¿El TCR sube o baja? ¿La economía se abarató o se encareció?
- Escribí la paridad de interés descubierta. Si $i^* = 5\%$, $RP = 8\%$ y se espera una depreciación de 25%, ¿qué tasa en pesos hace indiferente al inversor? ¿Qué pasa si el BC fija $i$ por debajo?
- Explicá el trilema pleno empleo / salario real / equilibrio externo.
- Ubicá en el espectro de regímenes: dolarización, convertibilidad, crawling peg, flotación con intervenciones.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Economia)**

- [Economia - Macro Clase 3 - Política Monetaria](Economia%20-%20Macro%20Clase%203%20-%20Política%20Monetaria.md) — clase anterior: prometía el modelo OA–DA y solo mostraba cómo la política monetaria **desplaza** la DA; acá se desarrolla el modelo completo. También comparte el riesgo país (que reaparece en la paridad de tasas), la neutralidad del dinero (mismo esquema que la autocorrección de §5) y el ancla nominal de los regímenes cambiarios
- [Economia - Oferta, Demanda y Mercado](Economia%20-%20Oferta,%20Demanda%20y%20Mercado.md) — excedente del consumidor y del productor y la distinción movimiento vs desplazamiento de la curva: las mismas herramientas se usan en §2–3 (a nivel agregado) y en §8 (efectos del comercio)
- [Economia - Resumen Microeconomía](Economia%20-%20Resumen%20Microeconomía.md) — §4.6 (excedente del productor) y §4.8 (impuesto sobre la producción) son la base del análisis del arancel: un arancel es un impuesto y la pérdida $B + D$ es su pérdida irrecuperable
- [Economia Intro](Economia%20Intro.md) — costo de oportunidad y FPP, que son la definición misma de **ventaja comparativa** (§7)
- [Economia - La cadena de distribución dejó de funcionar](Economia%20-%20La%20cadena%20de%20distribución%20dejó%20de%20funcionar.md) — artículo sobre el crédito en Argentina; contexto para la restricción externa y la capacidad de generar dólares
- [Materia - Economia](Materia%20-%20Economia.md) — nota índice de la materia

<!-- notas-relacionadas:fin -->
