---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-09-17
Materia: "[[Economia.base|Economia]]"
Cuatri: "2C"
temas:
  - política monetaria
  - macroeconomía
  - tasa de interés
  - interés simple y compuesto
  - TNA y TEA
  - tasa de interés real
  - paridad de Fisher
  - sistema financiero
  - instrumentos de renta fija
  - bonos
  - TIR
  - valor presente neto
  - estructura de riesgo de las tasas
  - rating crediticio
  - riesgo país
  - instrumentos de renta variable
  - perpetuidad
  - dinero
  - funciones del dinero
  - agregados monetarios
  - base monetaria
  - oferta monetaria
  - multiplicador monetario
  - encajes
  - expansión primaria y secundaria
  - Banco Central
  - BCRA
  - demanda de dinero
  - motivos keynesianos
  - mercado de dinero
  - operaciones de mercado abierto
  - demanda agregada
  - metas de inflación
  - neutralidad del dinero
  - zero lower bound
  - quantitative easing
  - forward guidance
---
# Economia - Macro Clase 3 - Política Monetaria

> Clase 3 de Macroeconomía — *Economía para Ingenieros* (ITBA 2026, Mg. Pablo Añes). La clase tiene dos mitades que parecen separadas pero no lo son: primero las **finanzas básicas** (tasa de interés, interés real, bonos, riesgo), y después **el dinero y la política monetaria**. El puente entre las dos es la tasa de interés: en la primera mitad es el precio al que se descuentan flujos, en la segunda es la variable que el Banco Central mueve.

## Resumen

Lo que promete la clase:

- Conceptos financieros básicos relacionados con la macroeconomía
- Características del dinero
- La política monetaria: implementación y funcionamiento
- Modelo de Oferta y Demanda Agregada

> [!info] Alcance real del deck
> El cuarto punto **no se desarrolla como modelo**. Lo que sí está es cómo la política monetaria **desplaza** la curva de DA (§11). El modelo OA–DA en sí (pendientes, corto vs largo plazo, brechas) queda para otra clase.

### Mapa de la clase

| Bloque | Temas | Sección |
|---|---|---|
| **1. Finanzas** | Tasa de interés, interés simple vs compuesto, tasa real, Fisher | §1–§2 |
| **2. Sistema financiero** | Funciones, indicadores, componentes | §3 |
| **3. Renta fija** | Bonos, VPN, TIR, cotización, riesgo, rating | §4 |
| **4. Renta variable** | Acciones, perpetuidad | §5 |
| **5. El dinero** | Características, tipos, funciones, agregados | §6 |
| **6. Variables monetarias** | BM, M, multiplicador, expansión primaria/secundaria | §7 |
| **7. Banco Central** | Funciones, balance | §8 |
| **8. Demanda de dinero** | Motivos keynesianos, curva MD, desplazamientos | §9 |
| **9. Mercado de dinero** | Equilibrio, política monetaria y tasa | §10 |
| **10. Política monetaria** | DA, regímenes, metas de inflación, largo plazo, no convencionales | §11–§15 |

---

## 1. Los dos conceptos fundamentales de finanzas

Toda la primera mitad de la clase se apoya en dos ideas:

| Concepto | Qué refleja |
|---|---|
| $i$ | El **"valor tiempo" del dinero** (rendimiento). Es el **costo de oportunidad del dinero** y lo que permite comparar flujos en **distintos momentos del tiempo**. |
| $E(R)$ vs $\sigma$ | El **trade-off fundamental** del sistema financiero: más rendimiento esperado exige más riesgo. |

Por qué importa el primero: el sistema financiero **canaliza el ahorro hacia la inversión transformando plazos**. Sin una tasa que permita comparar un peso hoy contra un peso dentro de dos años, no hay forma de hacer esa transformación.

### 1.1 Interés simple vs compuesto

| | Qué hace con los pagos intermedios | Tasa asociada |
|---|---|---|
| **Interés simple** | **No** se capitalizan / reinvierten | **Tasa Nominal Anual (TNA)** |
| **Interés compuesto** | **Sí** se capitalizan / reinvierten | **Tasa Efectiva Anual (TEA)** |

![[Economia - Macro Clase3 - Interes simple vs compuesto.png]]

*Capital inicial 100 al 2% mensual, 101 períodos. La brecha entre las dos curvas no es un detalle: a horizonte largo es todo.*

> [!important] En macro casi siempre se piensa en tasas compuestas
> Crecimiento económico, inflación y cantidad de dinero son todos procesos que se **capitalizan** período a período.

**Por qué importa en el largo plazo.** PIB per cápita en $t=100$:

$$\text{PIB}_{t+50} = 100\left(1+\frac{g}{100}\right)^{50}$$

| Crecimiento anual $g$ | PIB per cápita en $t+50$ |
|---|---|
| 1,5% | $100(1{,}015)^{50} = 210{,}52$ |
| 2,0% | $100(1{,}02)^{50} = 269{,}16$ |

Medio punto porcentual de crecimiento anual → un país **28% más rico** a 50 años.

> [!check] Verificado
> $100 \cdot 1{,}015^{50} = 210{,}52$ y $100 \cdot 1{,}02^{50} = 269{,}16$. El cociente es $269{,}16/210{,}52 = 1{,}2785$, o sea **+27,85%** — el "28%" de la slide está bien.

---

## 2. Tasa de interés real

La tasa nominal $i$ no dice cuánto poder adquisitivo se gana. Ajustada por inflación $\pi$:

$$1+r = \frac{1+i}{1+\pi} \qquad\Longleftrightarrow\qquad r = \frac{1+i}{1+\pi}-1$$

> La tasa de interés real es la tasa nominal **ajustada por los efectos de la inflación**, lo que permite medir el **poder adquisitivo real** que el inversor o prestamista gana (o pierde) con el paso del tiempo.

### 2.1 La aproximación de Taylor

Para números chicos vale la aproximación de primer orden:

$$r \approx i - \pi$$

> [!warning] NO para Argentina
> La cátedra lo marca en rojo: con tasas de inflación e interés altas la aproximación se rompe.

| Caso | $i$ | $\pi$ | $r$ exacta | $r \approx i-\pi$ | Error |
|---|---|---|---|---|---|
| Economía estable | 3% | 2% | **0,98%** | 1,00% | 0,02 p.p. |
| Argentina | 60% | 50% | **6,67%** | 10,00% | **3,33 p.p.** |

$$\frac{1+0{,}03}{1+0{,}02}-1 = 0{,}98\% \qquad\qquad \frac{1+0{,}60}{1+0{,}50}-1 = 6{,}66\%$$

> [!check] Verificado
> Exacto: $0{,}9804\%$ y $6{,}6667\%$. En el caso argentino la aproximación **sobreestima la tasa real en un 50%** (10% vs 6,67%) — no es un redondeo, es otro número.

> [!tip] Por qué falla
> $\dfrac{1+i}{1+\pi}-1 = \dfrac{i-\pi}{1+\pi}$. La aproximación tira el denominador $(1+\pi)$. Con $\pi = 2\%$ dividir por $1{,}02$ casi no cambia nada; con $\pi = 50\%$ estás dividiendo por $1{,}5$.

### 2.2 Ex-ante vs ex-post

| | Fórmula | Qué es |
|---|---|---|
| **Ex-ante** | $1+r_t = \dfrac{1+i_t}{1+\pi^{e}_{t+1}}$ | Tasa real **esperada** — usa inflación **esperada**. Es la **Paridad de Fisher**. Es la que mira el que decide hoy. |
| **Ex-post** | $1+r_t = \dfrac{1+i_t}{1+\pi_{t+1}}$ | Tasa real **realizada** — usa la inflación que efectivamente ocurrió. Solo se conoce después. |

> [!tip] Observación
> La diferencia entre las dos es exactamente el **error de expectativas**. Cuando la inflación sorprende para arriba, el deudor gana y el acreedor pierde: la tasa real ex-post termina siendo menor que la ex-ante pactada.

---

## 3. El sistema financiero

### 3.1 Funciones

Un **sistema financiero conecta agentes con necesidades de capital con agentes con exceso de capital**, permitiendo el intercambio de fondos entre ellos bajo condiciones concretas.

Sus funciones, y por qué cada una empuja el **crecimiento económico**:

- **Canalizar el ahorro hacia la inversión**
- **Agregar capital** (volumen) — junta muchos ahorros chicos para financiar proyectos grandes
- **Selección de proyectos** con mejor relación riesgo-retorno
- **Diversificación de riesgos**
- **Monitoreo de riesgos**

La relación sistema financiero ↔ crecimiento económico es de **doble vía**.

### 3.2 Componentes

![[Economia - Macro Clase3 - Componentes del sistema financiero.png]]

| Componente | Rol |
|---|---|
| **Prestamistas / inversores** | Agentes con exceso de capital |
| **Prestatarios** | Agentes con necesidad de capital |
| **Instrumentos financieros** | El vehículo del intercambio (bonos, acciones, ON, etc.) |
| **Intermediarios financieros** | Bancos, fondos — se interponen entre las dos puntas |
| **Mercados financieros** | Donde se negocian los instrumentos |
| **Organismos supervisores** | Envuelven todo (BCRA, CNV) |

> [!tip] Las dos vías
> La flecha de arriba (directa) es el mercado de capitales: el inversor le compra el instrumento al emisor. Las flechas de abajo son la intermediación: el banco toma depósitos de uno y presta al otro. Esta distinción es la que reaparece en §7 con la **expansión secundaria del dinero** — solo los intermediarios crean dinero.

### 3.3 Indicadores: Argentina en contexto

![[Economia - Macro Clase3 - Depositos del sector privado sobre PIB.png]]

![[Economia - Macro Clase3 - Credito al sector privado sobre PIB.png]]

| Indicador (% del PIB) | América Latina | Otros emergentes | Desarrollados | **Argentina** |
|---|---|---|---|---|
| Depósitos del sector privado | 41,0 | 78,4 | 130,7 | **15,4** |
| Crédito al sector privado | 41 | 89 | 131 | **12,8** |

> [!important] El dato que hay que retener
> Argentina está **última en ambos rankings**, y no por poco: el crédito al sector privado es **un tercio** del promedio regional y **un décimo** del de los países desarrollados. Un sistema financiero de ese tamaño no puede cumplir bien ninguna de las funciones de §3.1.

---

## 4. Instrumentos de renta fija

### 4.1 Qué son

- En su mayoría son **instrumentos de deuda** que emite el deudor y "compra" el acreedor.
- Tienen un flujo de pagos **prefijado por contrato** desde la emisión del título.
- El **prospecto** incluye:
	- Estructura de **cupones** (pago de intereses)
	- Amortización del **capital**
	- Cláusulas de rescate, cross-default, **Cláusulas de Acción Colectivas (CACs)**, legislación aplicable, garantías, etc.

**Clasificación según el deudor:**

| Sector | Instrumento |
|---|---|
| Público | Bonos o títulos públicos |
| Privado | **Obligaciones Negociables (ON)** — a veces también se les dice "bonos" genéricamente |

**Mercado primario vs secundario:**

| | Quién participa | Qué precio se define |
|---|---|---|
| **Primario** | El deudor emite, los acreedores compran | Precio de **emisión** |
| **Secundario** | Compraventa posterior entre inversores — **el deudor no participa** | **El precio del bono** |

> [!important] El precio de los bonos se define en el mercado secundario
> Esto es clave para §4.4: el emisor no elige el precio al que cotiza su deuda. El mercado sí.

### 4.2 Valuación: valor presente neto de un flujo de pagos

$$P^{B} = \frac{CF_1}{1+i} + \frac{CF_2}{(1+i)^2} + \cdots + \frac{CF_N}{(1+i)^N}$$

| Elemento | Qué es |
|---|---|
| $CF_1 \ldots CF_N$ | **Conocidos y definidos por contrato/prospecto** |
| $i$ | La **tasa a la que descuento** los pagos — mi **costo de oportunidad** |

### 4.3 Ejemplo práctico: bono bullet

**Bono bullet** (amortiza todo el capital al final), cupones semestrales de 10% anual, a dos años. Flujo: 5, 5, 5 y 105.

![[Economia - Macro Clase3 - Bono bullet a la par bajo y sobre la par.png]]

La **TIR** es la tasa que iguala el valor presente del flujo al precio de mercado — la tasa que refleja el **rendimiento esperado**:

$$i = \frac{FP - P_b}{P_b} \qquad\qquad P_b = \frac{FP}{1+i}$$

> **El valor de un bono es su flujo de pagos descontado a su TIR.**

| Precio de mercado | TIR | Cómo cotiza | Lectura |
|---|---|---|---|
| **100** | 10% | **a la par** (TIR = cupones) | El inversor obtiene exactamente la rentabilidad pactada en el cupón |
| **80** | 25% | **bajo la par** (TIR > cupones) | El mercado exige más rentabilidad de la que paga el cupón |
| **110** | 5% | **sobre la par** (TIR < cupones) | El mercado se conforma con menos |

> [!bug] El exponente de la fórmula de la slide está mal
> La slide escribe el factor de descuento como $\left(1+TIR\cdot\frac{180}{360}\right)^{d/360}$ con $d = 180, 360, 540, 720$, o sea exponentes $0{,}5;\ 1;\ 1{,}5;\ 2$.
>
> Con esos exponentes, descontando al 10% el bono **no** da 100 sino **109,53**. El exponente tiene que ser el **número de períodos de capitalización** (semestres), es decir $d/180 = 1, 2, 3, 4$:
>
> $$P^{B}=\sum_{k=1}^{4}\frac{CF_k}{\left(1+TIR\cdot\frac{180}{360}\right)^{k}}$$
>
> Así sí: $\frac{5}{1{,}05}+\frac{5}{1{,}05^{2}}+\frac{5}{1{,}05^{3}}+\frac{105}{1{,}05^{4}} = 100{,}00$ exacto.
>
> El resultado conceptual (100 ↔ TIR = cupón) es correcto; lo que está mal escrito es el exponente.

> [!check] Verificado — las TIR de 80 y 110 son redondeos
> Resolviendo con la fórmula corregida:
>
> | Precio | TIR exacta (TNA) | TEA | La slide dice |
> |---|---|---|---|
> | 100 | **10,000%** | 10,25% | 10% ✓ exacto |
> | 80 | **23,04%** | 24,36% | 25% (aprox.) |
> | 110 | **4,70%** | 4,76% | 5% (aprox.) |
>
> Los valores de 80 y 110 son ilustrativos, no exactos. Para el parcial lo que importa es la **dirección**: precio abajo ⇒ TIR arriba.

> [!tip] La relación que hay que tener automatizada
> **Precio y TIR se mueven siempre en sentido opuesto.** El flujo de pagos está fijo por contrato; lo único que puede moverse es el precio, así que todo cambio de expectativas se traduce en TIR.

### 4.4 Estructura de riesgo de las tasas de interés

$$\text{TIR}_{\text{bono}} = \text{Tasa libre de riesgo} + \text{Prima de riesgo}$$

- La **tasa libre de riesgo** es el bono de USA (T-Bill / T-Bond).
- La **prima de riesgo** depende de:
	- Tipo de emisor del bono
	- Liquidez del instrumento
	- Confianza en el emisor
	- Tratamiento impositivo
	- Plazo

$$\text{Más riesgo} \;\Longrightarrow\; \downarrow \text{Precio del bono} \;\Longleftrightarrow\; \uparrow \text{TIR implícita}$$

### 4.5 Rating crediticio → clave para la prima de riesgo

| Moody's | S&P | Fitch | Definición | Categoría |
|---|---|---|---|---|
| Aaa | AAA | AAA | Prime Maximum Safety | **Investment grade** |
| Aa1–Aa3 | AA+ / AA / AA− | AA+ / AA / AA− | High Grade High Quality | Investment grade |
| A1–A3 | A+ / A / A− | A+ / A / A− | Upper Medium Grade | Investment grade |
| Baa1–Baa3 | BBB+ / BBB / BBB− | BBB+ / BBB / BBB− | Lower Medium Grade | Investment grade |
| Ba1–Ba3 | BB+ / BB / BB− | BB+ / BB / BB− | Noninvestment Grade / Speculative | **Junk** |
| B1–B3 | B+ / B / B− | B+ / B / B− | Highly Speculative | Junk |
| Caa1–Caa3 | CCC+ / CCC / CCC− | CCC | Substantial Risk / In Poor Standing | Junk |
| Ca | — | — | Extremely Speculative | Junk |
| C | — | — | May Be in Default | Junk |
| — | — | D | Default | Junk |

> [!important] La línea roja está entre BBB− y BB+
> Esa es la frontera **investment grade / junk**. No es cosmética: muchos fondos institucionales tienen prohibido por mandato tener papeles debajo de esa línea, así que cruzarla dispara ventas forzadas.

![[Economia - Macro Clase3 - Probabilidad historica de default por rating.png]]

*Probabilidad histórica de default acumulada a 5 años. De A+ a BBB apenas se mueve (1,6% → 2,0%); de BBB para abajo se dispara hasta 14,6% en B−. Fuente: BofA Global Research sobre datos de Moody's, S&P y Fitch.*

![[Economia - Macro Clase3 - Riesgo pais EMBI y rating 2021.png]]

*Riesgo país (EMBI - JP Morgan) contra rating crediticio, 2021. La nube sube de izquierda a derecha: peor rating ⇒ mayor spread exigido.*

---

## 5. Instrumentos de renta variable

- El ejemplo más básico son las **acciones**: participaciones directas en una empresa — no hay acreedores sino **"dueños"**.
- Son una **"perpetuidad"**: no tienen vencimiento.
- El valor de una acción se suele definir como el **valor presente descontado del flujo "infinito" de dividendos esperados**.

**Valor presente de una perpetuidad constante:**

$$P^{A} = \frac{D}{1+i} + \frac{D}{(1+i)^2} + \cdots = \frac{D}{i}$$

| Variable | De qué depende |
|---|---|
| $D$ | De los **dividendos proyectados** |
| $i$ | De la **tasa esperada futura de descuento** relevante |

> [!tip] Por qué las acciones caen cuando suben las tasas
> $P^A = D/i$ es una hipérbola: si $i$ pasa de 5% a 10% con los mismos dividendos, el precio se parte **al medio**. Esta es la conexión directa entre §10 (el BC mueve la tasa) y el mercado de acciones.

### 5.1 En Argentina el mayor volumen lo concentra la renta fija

Volumen negociado 2024 (millones de ARS, prioridad precio-tiempo + SENEBI):

| Instrumento | 2024 (total) |
|---|---|
| **Títulos Públicos** (PPT) | 314.470.575 |
| **Títulos Públicos** (SENEBI) | 572.309.183 |
| Obligaciones Negociables (PPT + SENEBI) | 108.963.443 |
| Acciones | 13.031.822 |
| Cedears | 11.162.634 |
| Cauciones | 555.821.649 |
| **Volumen total** | **1.629.184.654** |

> [!tip] Observación
> Los títulos públicos solos (≈ 886.000 millones entre los dos segmentos) mueven **68 veces** lo que mueven las acciones. El mercado de capitales argentino es, esencialmente, un mercado de deuda soberana — coherente con el crédito al sector privado de 12,8% del PIB de §3.3.

*(La slide tiene una errata en el título: "EN ARGENTINA EL MAYOR **LO** VOLUMEN LO CONCENTRA LA RENTA FIJA".)*

---

## 6. El dinero

### 6.1 Actividad: características del dinero

La cátedra hace votar cuáles de estas son características del dinero:

| Característica | ¿Es del dinero? |
|---|---|
| Portabilidad | Sí |
| Escasez | Sí |
| Tangibilidad | **Discutible** |
| Aceptabilidad | Sí — es *la* característica |
| Rentabilidad | **No** |
| Durabilidad | Sí |
| Uniformidad | Sí |
| Divisibilidad | Sí |

> [!tip] Las dos trampas de la actividad
> - **Tangibilidad**: el dinero electrónico (que la propia clase lista como tipo de dinero en §6.2) no es tangible. Era deseable cuando el dinero era mercancía; hoy no es necesaria.
> - **Rentabilidad**: es directamente **lo contrario**. Que el dinero *no* rinda es lo que genera su **costo de oportunidad**, y ese costo de oportunidad es lo que hace que la curva de demanda de dinero tenga pendiente negativa (§9). Si el dinero rindiera como los demás activos, no habría demanda de dinero que modelar.
>
> Por eso la conclusión de la clase dice "algunas **necesarias**, otras **deseables**".

### 6.2 Tipos y funciones

> **Dinero es aquello que se utiliza como medio de cambio, aquello que se puede utilizar para intercambiarlo por bienes y servicios.**

![[Economia - Macro Clase3 - Funciones del dinero.png]]

**Tipos de dinero:** mercancía · con respaldo en mercancías · fiduciario · electrónico

| Función | Qué significa |
|---|---|
| **Medio de cambio** | Compra de bienes y servicios |
| **Unidad de cuenta** | Precios y balances |
| **Depósito de valor** | Acumular riqueza |
| **Patrón de pago diferido** | Pagos a futuro |

> [!tip] Cuál se rompe primero con inflación alta
> **Depósito de valor** y **patrón de pago diferido** — por eso en Argentina se ahorra en dólares y los contratos largos se indexan o se dolarizan. **Medio de cambio** es la última que se pierde (y cuando se pierde, hay dolarización de facto).

### 6.3 Agregados monetarios

> La **oferta monetaria** es todo el dinero que hay en una economía para hacer transacciones.
> Los **agregados monetarios** son los distintos elementos o formas de dinero que integran la oferta monetaria.

| Agregado | Composición |
|---|---|
| **M1** | Efectivo o circulante, depósitos en cuenta corriente transferibles mediante cheque y tipos de depósito de fácil liquidez |
| **M2** | M1 + depósitos de corto plazo o caja de ahorro que devengan intereses |
| **M3** | M2 + depósitos a largo plazo que devengan intereses, depósitos en divisa, bonos, letras del tesoro |

> [!tip] El criterio de ordenamiento es la liquidez
> M1 ⊂ M2 ⊂ M3. A medida que subís de agregado ganás rendimiento y perdés liquidez — el mismo trade-off $E(R)$ vs $\sigma$ de §1, pero en la dimensión plazo.

---

## 7. Variables monetarias: base, oferta y multiplicador

### 7.1 Las ecuaciones

| # | Ecuación | Lectura |
|---|---|---|
| 1 | $M = E + D$ | **Oferta monetaria** = efectivo en poder del público + depósitos |
| 2 | $BM = E + R$ | **Base monetaria** (dinero de alta potencia) = efectivo + reservas |
| 3 | $e = E/D$ | Preferencia del **público** por efectivo |
| 4 | $r = R/D$ | Coeficiente de **encajes** / reservas de los bancos |
| 5 | $m = \dfrac{M}{BM} = \dfrac{e+1}{e+r}$ | **Multiplicador del dinero** |
| 6 | $M = m \cdot BM$ | |
| 7 | $R = r \cdot D$ | |
| 8 | $\text{Préstamos} = D - R$ | |

![[Economia - Macro Clase3 - Efectivo depositos y base monetaria.png]]

*Las existencias de dinero (M) son efectivo + depósitos; el dinero de alta potencia (BM) es efectivo + reservas. La diferencia entre las dos barras es todo lo que crearon los bancos. Fuente: elaboración propia sobre Dornbusch.*

> [!check] Derivación del multiplicador (verificada)
> $$\frac{M}{BM}=\frac{E+D}{E+R}=\frac{eD+D}{eD+rD}=\frac{e+1}{e+r}$$
>
> Estática comparada, derivando:
> $$\frac{\partial m}{\partial r}=\frac{-(e+1)}{(e+r)^{2}}<0 \qquad\qquad \frac{\partial m}{\partial e}=\frac{r-1}{(e+r)^{2}}<0 \ \ (\text{porque } r<1)$$
>
> Coincide con lo que dice la slide:
>
> | | |
> |---|---|
> | $\uparrow r \Rightarrow \downarrow m$ | $\uparrow e \Rightarrow \downarrow m$ |
> | $\downarrow r \Rightarrow \uparrow m$ | $\downarrow e \Rightarrow \uparrow m$ |
>
> Casos límite: si $e=0$ (nadie tiene efectivo) ⇒ $m = 1/r$. Si $r=1$ (encaje del 100%) ⇒ $m=1$ y no hay creación secundaria.

### 7.2 El multiplicador en acción: balance del sistema financiero

**El comportamiento de los bancos y del público también define la cantidad de dinero total en una economía.**

![[Economia - Macro Clase3 - Multiplicador monetario balance agregado.png]]

Base monetaria \$100, coeficiente de encajes 10%:

| Ronda | Depósito | Encaje (10%) | Presta |
|---|---|---|---|
| 1 (Fravega) | 100 | 10 | 90 → Juan → Fiat |
| 2 (Fiat) | 90 | 9 | 81 → José → Peugeot |
| 3 (Peugeot) | 81 | 8,1 | 72,9 → Ruben → … |
| **Suma parcial** | **271** | | |

> [!check] Verificado
> Los \$271 de la slide son la suma de las **tres primeras rondas** (100 + 90 + 81), no el total. La serie geométrica completa converge a $\sum_{k\ge0} 100\cdot0{,}9^{k} = 100/0{,}10 = 1000$, o sea $m = 1/r = 10$ — consistente con la fórmula de §7.1 con $e=0$.
>
> La conclusión de la slide (**Oferta Monetaria Total > Base Monetaria**) es correcta; el número 271 es solo el arranque.

### 7.3 Expansión primaria y secundaria del dinero

| | Quién la hace | Qué afecta |
|---|---|---|
| **Expansión primaria** | Solo el **Banco Central** | La **base monetaria** |
| **Expansión secundaria** | Los **bancos comerciales** | Crea **dinero secundario** |

![[Economia - Macro Clase3 - Expansion primaria y secundaria.png]]

Ejemplo del deck, ahora con público que sí tiene efectivo (\$100 iniciales):

$$E = \$20 + \$70 \text{ (préstamo)} = \$90 \qquad D = \$80 \qquad R = \$10$$

$$BM = 90 + 10 = \$100 \qquad M = 90 + 80 = \$170 \qquad m = \frac{170}{100} = 1{,}7$$

> [!check] Verificado contra la fórmula
> $e = 90/80 = 1{,}125$, $r = 10/80 = 0{,}125$.
> $$m=\frac{e+1}{e+r}=\frac{2{,}125}{1{,}25}=1{,}7 \ \checkmark$$
> Comparar con el caso anterior ($e=0$, $r=0{,}1$ ⇒ $m=10$): que el público retenga efectivo **desarma** el multiplicador, porque el efectivo en el bolsillo no vuelve al circuito de préstamos.

### 7.4 El multiplicador del dinero en Argentina

![[Economia - Macro Clase3 - Multiplicador del dinero en Argentina.png]]

A fecha del **16/09/2025** (Informe monetario diario del BCRA):

$$m = \frac{M2}{BM} = \frac{\$72.633.173}{\$41.347.862} = 1{,}756$$

| Agregado (millones de $) | 16-sept-25 |
|---|---|
| Base monetaria | 41.347.862 |
| M1 | 48.254.258 |
| M2 | 72.633.173 |
| M3 | 144.171.226 |

> [!check] Verificado
> $72.633.173 / 41.347.862 = 1{,}7566$ ✓. La inversa es $BM/M2 = 0{,}569$: **el 57% de M2 es emisión del Banco Central** y el 43% restante lo crearon los bancos. La frase "aproximadamente sólo uno de cada dos pesos" es razonable (más precisamente, 57 de cada 100).
>
> Con M3: $m = 144.171.226/41.347.862 = 3{,}49$.

> [!tip] Un multiplicador de 1,76 es bajísimo
> Compará contra el ejemplo teórico con encaje 10% ($m=10$). Un $m$ chico refleja encajes altos y mucha preferencia del público por efectivo — exactamente lo que predice $m=(e+1)/(e+r)$, y otra cara del mismo dato de §3.3.

---

## 8. Banco Central

> El BC es la institución que actúa como **autoridad monetaria** y suele ser el encargado de la **emisión del dinero legal**, además de **diseñar y ejecutar la política monetaria** del país al que pertenece.

### 8.1 Funciones

- Preservar el valor de la moneda
- Custodio y administrador de las reservas de oro y divisas
- Proveedor del dinero de curso legal
- Ejecutor de la política cambiaria
- Responsable de la política monetaria
- Prestamista y agente financiero del gobierno nacional
- Prestamista de última instancia (**banco de bancos**)

### 8.2 Balance del Banco Central

| Activos | Pasivos |
|---|---|
| Reservas Internacionales | Efectivo o Circulante |
| Préstamos al tesoro (Gob. / Bonos) | Reservas Legales (Encajes) |
| Préstamos al sector bancario (Redescuentos) | Letras del Banco Central |

> [!tip] Cómo leer este balance
> - **Base monetaria = Efectivo + Encajes**, o sea las **dos primeras líneas del pasivo**. Las **Letras del BC no son base monetaria**: son pasivo remunerado no monetario (es justamente lo que el BC usa para *retirar* pesos).
> - Toda operación del BC toca las dos columnas a la vez. Comprar divisas ⇒ ↑Reservas Internacionales (activo) y ↑Circulante (pasivo) ⇒ **expande la base**. Vender bonos ⇒ ↓Préstamos al tesoro y ↓Circulante ⇒ **contrae la base**.
> - Esa mecánica es exactamente la tabla de §10.4.

---

## 9. La demanda de dinero

### 9.1 Motivos keynesianos

Keynes clasificaba en tres los motivos por los que los agentes económicos desean mantener **saldos líquidos**:

| Motivo | Para qué |
|---|---|
| **Transacción** | Compras cotidianas |
| **Precaución** | Gastos futuros inesperados |
| **Especulación** | Oportunidad de negocio |

### 9.2 La demanda de dinero como decisión marginal

Los individuos y empresas **comparan beneficios y costos** de mantener dinero al decidir qué proporción de sus activos mantienen líquida:

| | |
|---|---|
| **Beneficio** de mantener dinero | Poder **usarlo para transacciones** |
| **Costo** de mantener dinero | El dinero tiene una **rentabilidad más baja que otros activos no monetarios** ⇒ hay un **costo de oportunidad** |

> ¿Cuál es ese costo de oportunidad? Las **ganancias que conseguirían a través del activo financiero que puedan elegir**.
>
> Cuanto **mayor** sea la **tasa de interés** de los activos no monetarios, **mayor** será el **costo de oportunidad de mantener dinero**, y viceversa.

### 9.3 La curva de demanda de dinero

Como el nivel general de tasas afecta el costo de oportunidad de mantener dinero, la **demanda de dinero** está **relacionada inversamente con la tasa de interés** ⇒ la curva MD tiene **pendiente negativa**.

![[Economia - Macro Clase3 - Curva de demanda de dinero.png]]

### 9.4 Desplazamientos de la curva

![[Economia - Macro Clase3 - Desplazamientos de la demanda de dinero.png]]

| Movimiento | Qué significa |
|---|---|
| **A la derecha** (MD₁ → MD₂) | **Aumento** de la cantidad demandada de dinero **para cada nivel de tasa de interés** |
| **A la izquierda** (MD₁ → MD₃) | **Disminución**: cae la cantidad demandada para cada nivel de tasa |

**Factores que desplazan la curva:**

| Factor | Efecto |
|---|---|
| **Nivel de precios agregado** | La demanda de dinero es **proporcional** al nivel de precios, *ceteris paribus* |
| **PBI real** | A mayor cantidad de bienes y servicios que se compren, mayor demanda de dinero, y viceversa |
| **Tecnología** | Los avances tecnológicos permiten que la demanda de dinero sea **menor** (no hace falta llevar encima gran cantidad de dinero) |
| **Instituciones** | Regulaciones bancarias (encajes, tasas, etc.) |

> [!important] Distinguir movimiento *sobre* la curva de desplazamiento *de* la curva
> Un cambio en $r$ es un movimiento **a lo largo** de MD. Los cuatro factores de arriba mueven **toda la curva**. El primero (nivel de precios) es el que hace funcionar la **neutralidad del dinero** de §14.

---

## 10. El dinero y la tasa de interés

Para entender cómo se determina la tasa de interés (suponiendo por simplicidad **una única tasa** para todos los activos financieros) se analiza el **mercado de dinero**, donde la tasa está determinada por la **intersección de oferta y demanda de dinero**:

- La **demanda de dinero** está representada por la curva **MD** (pendiente negativa).
- La **oferta de dinero** la determina la **autoridad monetaria**: el BC elige el nivel de oferta monetaria que espera que le permita alcanzar la tasa deseada. Se refleja en la curva **MS**, una **recta vertical** en el nivel elegido.

### 10.1 Equilibrio en el mercado de dinero

![[Economia - Macro Clase3 - Equilibrio en el mercado de dinero.png]]

| Situación | Qué pasa |
|---|---|
| $r < r_E$ | La cantidad demandada de dinero **supera** a la ofrecida; a su vez la cantidad demandada de activos financieros es menor a la ofrecida ⇒ **sube la tasa de interés** |
| $r > r_E$ | La cantidad demandada de dinero es **inferior** a la ofrecida ⇒ **baja la tasa de interés** |
| | **Tendencia al equilibrio $r_E$** |

> [!tip] El mecanismo de ajuste pasa por el precio de los bonos
> Si querés más dinero del que hay, vendés bonos ⇒ cae el precio del bono ⇒ **sube la TIR** (§4.3). El mercado de dinero y el de bonos son la misma moneda: exceso de demanda de dinero = exceso de oferta de bonos.

### 10.2 La política monetaria y la tasa de interés

![[Economia - Macro Clase3 - Aumento de la oferta monetaria y tasa de interes.png]]

- Si **aumenta la oferta de dinero** de $M_1$ a $M_2$, la curva MS se desplaza a la derecha y la tasa de equilibrio **baja** de $r_1$ a $r_2$, porque **solo a una tasa más baja los agentes están dispuestos a mantener más dinero**.
- Si el BC **reduce la oferta de dinero**, MS se desplaza a la izquierda y la **tasa aumenta**.

### 10.3 Operaciones de mercado abierto

Los BC hacen **compra/venta de bonos** para inyectar/sacar dinero de la economía:

![[Economia - Macro Clase3 - Operaciones de mercado abierto y tasa objetivo.png]]

| Operación | Efecto sobre MS | Tasa |
|---|---|---|
| **Compra** de títulos (open-market purchase) | MS₁ → MS₂ (derecha) | Baja hasta $r_T$ |
| **Venta** de títulos (open-market sale) | MS₁ → MS₂ (izquierda) | Sube hasta $r_T$ |

### 10.4 Factores de expansión y contracción de la oferta monetaria

Los bancos comerciales y las personas **no pueden modificar la cantidad de dinero primario (BM)**, pero **sí pueden influir en el dinero secundario**, principalmente a través de sus decisiones de depósitos y préstamos.

| Qué modifica | Herramienta | Expansiva | Contractiva |
|---|---|---|---|
| **Base monetaria** | Operaciones cambiarias | Comprar divisa | Vender divisa |
| **Base monetaria** | Operaciones de mercado abierto (OMA) | Comprar bonos; rescatar pases | Vender bonos; licitar pases |
| **Base monetaria** | Adelantos al tesoro | Financiamiento de déficit presupuestarios | — |
| **Base monetaria** | Política de redescuento | Bajar tasa de redescuento | — |
| **Dinero secundario** | Variación de encajes | Bajar tasa de encaje | Subir tasa de encaje |
| **Tasa de interés** | Tasa de política | Bajar tasa de interés | Subir tasa de interés |

| | |
|---|---|
| **Políticas expansivas** | **Aumentan** la cantidad de dinero en la economía |
| **Políticas contractivas** | **Reducen** la cantidad de dinero en la economía |

> [!tip] Dónde pega cada herramienta
> Las primeras cuatro filas mueven $BM$; los encajes mueven $m$ (vía $r$ en la fórmula de §7.1). Como $M = m \cdot BM$, el BC tiene dos palancas independientes sobre la misma cantidad.

> **Definición de política monetaria:** acción de las autoridades monetarias dirigida a **controlar las variaciones en la cantidad total del dinero** con el fin de colaborar con los demás instrumentos de la política económica al control de sus objetivos.

---

## 11. Política monetaria y demanda agregada

![[Economia - Macro Clase3 - Politica monetaria y demanda agregada.png]]

La política monetaria es uno de los factores que puede mover la curva de **demanda agregada**:

**Expansiva:**
$$\uparrow M \Rightarrow \downarrow r \Rightarrow \uparrow I \text{ y } \uparrow C \text{ (vía multiplicador)} \Rightarrow \text{DA se desplaza a la derecha}$$

**Contractiva:**
$$\downarrow M \Rightarrow \uparrow r \Rightarrow \downarrow I \text{ y } \downarrow C \Rightarrow \text{DA se desplaza a la izquierda}$$

### 11.1 Política monetaria en la práctica

En la práctica la política monetaria se lleva a cabo teniendo en cuenta **dos objetivos**: la **estabilidad del nivel de precios** y la **estabilidad del producto**.

> [!bug] Error en la slide "Política monetaria en la práctica"
> La slide dice: *"los bancos centrales suelen implementar **políticas fiscales** expansivas cuando la economía está en una brecha recesiva y **políticas fiscales** contractivas cuando está en una brecha inflacionaria"*.
>
> Debería decir **políticas monetarias**. Los bancos centrales no hacen política fiscal — esa es del Tesoro / Ministerio de Economía. El contenido conceptual (expansiva en brecha recesiva, contractiva en brecha inflacionaria) es correcto.

![[Economia - Macro Clase3 - Fed funds rate y desempleo.png]]

*Fed funds rate y tasa de desempleo, 2007–2019. La Fed llevó la tasa a cero en la crisis de 2008 y la mantuvo ahí hasta ~2016, recién subiéndola cuando el desempleo ya había bajado de 10% a ~5%. Krugman/Wells, Essentials of Economics 5e.*

---

## 12. Régimen de política monetaria

![[Economia - Macro Clase3 - Regimen de politica monetaria.png]]

La cadena va de izquierda a derecha: el BC maneja **instrumentos**, que le permiten alcanzar un **objetivo operativo**, que sirve de **meta intermedia**, que finalmente persigue el **objetivo** último.

| Eslabón | Opciones |
|---|---|
| **Instrumentos / herramientas** | a) Intervención cambiaria · b) y c): Encajes y OMAs (**el BC define montos y frecuencia**) · Facilidades de depósitos/crédito, o pases pasivos y activos (**los bancos definen montos y frecuencia**) |
| **Objetivo operativo** | a) Fijo o bandas cambiarias · b) Base monetaria (esquema híbrido: tasas de interés de corto plazo) · c) Tasas de interés de corto plazo |
| **Meta intermedia** | a) Tipo de cambio · b) Agregados monetarios · c) Inflación (y expectativas) → **define el ancla nominal de la economía** |
| **Objetivo** | Estabilidad de precios (baja inflación) · Estabilidad financiera · Crecimiento económico de largo plazo (y pleno empleo) |

> [!important] El concepto que se evalúa acá es el **ancla nominal**
> Es la variable que el BC se compromete a controlar y que fija las expectativas de precios. Cada régimen elige un ancla distinta: tipo de cambio (convertibilidad), agregados monetarios (metas de emisión) o inflación (inflation targeting).

---

## 13. Fijación de objetivos de inflación (*inflation targeting*)

El tipo de política monetaria más usado en la práctica por los bancos centrales es la **fijación de metas de inflación**: la determinación y comunicación de un **objetivo de inflación explícito** y la elaboración de la política monetaria en torno a ese objetivo.

| Elemento | Detalle |
|---|---|
| **Objetivo** | Generalmente el IPC (países desarrollados: 2,3% anual; países emergentes: 3,6% anual) |
| **Instrumento** | Generalmente la tasa de corto plazo |
| **Comunicación** | Estrategia clara y transparente |
| **Evaluación** | Prospectiva de la dinámica macro, para alterar la tasa de largo plazo |

**Ventajas:** transparencia, reducción de la incertidumbre y **mayor responsabilidad (accountability)** del BC al ser explícito el objetivo.

![[Economia - Macro Clase3 - Metas de inflacion por pais.png]]

*Metas de inflación: Nueva Zelanda, Canadá y Suecia usan bandas (1–3%); Gran Bretaña y EEUU un punto (2%); Noruega 2,5%. Krugman/Wells 5e.*

### 13.1 El prerrequisito más controversial: ¿inflación de partida?

La pregunta es si se puede adoptar un régimen de metas de inflación **desde una inflación alta**. En la muestra de países con régimen IT pleno (1990–2016), casi todos adoptaron con inflación de un dígito; **Argentina adoptó con 35,5%** — el valor más alto de la muestra.

> [!tip] Por qué es controversial
> El régimen funciona anclando expectativas. Si la inflación de partida es muy alta, la meta anunciada no es creíble, las expectativas no se anclan y la política pierde su mecanismo principal. La evidencia del gráfico de tasas antes/después muestra que **casi todos los países bajaron la inflación tras adoptar el régimen** (los puntos caen debajo de la diagonal), pero la mayoría partía de niveles mucho más bajos.

---

## 14. Largo plazo: la neutralidad del dinero

![[Economia - Macro Clase3 - Neutralidad del dinero en el largo plazo.png]]

La secuencia completa:

1. **Equilibrio inicial de largo plazo** $E_1$, con oferta de dinero $M_1$ y tasa $r_1$.
2. El BC **aumenta la oferta monetaria** a $M_2$. **A corto plazo** la economía se mueve a $E_2$ y la **tasa baja** a $r_2$.
3. **A largo plazo**, cuando el **nivel de precios aumenta**, la **demanda de dinero también** lo hace (es proporcional al nivel de precios — §9.4) y la curva se desplaza de $MD_1$ a $MD_2$, llegando a $E_3$, donde la **tasa vuelve a ser $r_1$**.

> [!important] Neutralidad del dinero
> **A largo plazo los cambios en la oferta monetaria no afectan a la tasa de interés.** Solo cambian el nivel de precios. El dinero es neutral en el largo plazo pero **no** en el corto — y ese hueco de corto plazo es justamente el espacio en el que la política monetaria puede hacer algo.

---

## 15. Instrumentos no convencionales de política monetaria

Tras la crisis de **2008**, los instrumentos convencionales no fueron suficientes para garantizar una respuesta anticíclica adecuada.

### 15.1 El problema del piso límite cero (*zero lower bound*)

$$r \approx i - \pi$$

Si $i$ no puede bajar de 0 (nadie presta a tasa negativa teniendo la opción de guardar efectivo), entonces el piso de la tasa **real** es $r \ge -\pi$. Con inflación baja o negativa, el BC se queda sin margen para estimular.

![[Economia - Macro Clase3 - Zero Lower Bound.png]]

*Tasa efectiva de fondos federales y objetivo de política (% anual), jun-98 a jun-22. Se ve el piso cero de 2009–2015 y otra vez en 2020–2021. Fuente: FRED.*

### 15.2 Los cuatro instrumentos

| # | Instrumento | En qué consiste |
|---|---|---|
| 1 | **Inyecciones de liquidez** | Redescuentos, subastas de liquidez (TAF), programas de préstamos |
| 2 | **Compras de activos a gran escala** | **Quantitative Easing (QE)** |
| 3 | **Forward Guidance** | Manejo de expectativas pre-anunciando el comportamiento futuro de la política monetaria (condicionales / no-condicionales) |
| 4 | **Tasas nominales negativas** | Sobre las reservas de los bancos en el BC (UE y Japón) |

![[Economia - Macro Clase3 - Base monetaria de EEUU y QE.png]]

*Base monetaria total de EEUU (BOGMBASE, FRED). Se identifican QE1 (2008), QE2 (2010), QE3 (2012), el salto del Covid (2020) y el QT posterior. De ~\$800.000 millones pre-2008 a más de \$6 billones en 2021.*

> [!tip] Por qué QE no multiplicó los precios
> El QE multiplicó la **base monetaria** por ~8, pero M no creció ni cerca en esa proporción: el multiplicador $m$ se derrumbó porque los bancos dejaron el dinero como **reservas excedentes** en el BC en vez de prestarlo (↑$r$ en la fórmula de §7.1 ⇒ ↓$m$). Es el mismo mecanismo del §7.3, en escala macro.

---

## 16. Conclusiones de la clase

- El dinero presenta características, **algunas necesarias otras deseables**, para cumplir con sus funciones centrales.
- Si bien es un agente central en el control de la cantidad de dinero (oferta), **el Gobierno no define totalmente la misma** — el multiplicador depende de bancos y público.
- Existen **diferentes regímenes** de política monetaria, distintas formas de aplicarla.
- La política monetaria es una **herramienta esencial para estabilizar el ciclo económico**.
- **Presenta limitaciones**, al igual que la política fiscal.

---

## 17. Errores y cosas a chequear de las slides

| Slide | Qué dice | Qué corresponde |
|---|---|---|
| Bono bullet (ej. práctico) | Exponente $\left(1+TIR\frac{180}{360}\right)^{d/360}$ con $d=180,360,540,720$ | El exponente debe ser el **número de semestres** ($d/180 = 1,2,3,4$). Con el exponente de la slide, el bono a la par da **109,53**, no 100 |
| Bono bullet (80 y 110) | TIR = 25% y TIR = 5% | Son redondeos: las TIR exactas son **23,04%** y **4,70%**. La del bono a la par (10%) sí es exacta |
| Multiplicador monetario (balance) | "\$271" | Es la suma de las **tres primeras rondas**, no el total. La serie converge a **1000** ($m=1/r=10$) |
| Volumen Argentina (título) | "EN ARGENTINA EL MAYOR **LO** VOLUMEN LO CONCENTRA…" | Errata de tipeo |
| Política monetaria en la práctica | "los bancos centrales suelen implementar **políticas fiscales** expansivas/contractivas" | **Políticas monetarias**. Los BC no hacen política fiscal |
| Fijación de objetivos de inflación | "Las principales ventajas del inflation targeting suele ser la transparencia que aporta este método pasan por reducir incertidumbre…" | Frase rota (mezcla dos redacciones). Se entiende: las ventajas son transparencia, menor incertidumbre y mayor accountability |
| Actividad "características del dinero" | Lista **rentabilidad** como característica | El dinero se caracteriza precisamente por **no** rendir — de ahí su costo de oportunidad y la pendiente negativa de MD |
| Multiplicador Argentina | "uno de cada dos pesos" | Más exacto: **57 de cada 100** ($BM/M2 = 0{,}569$) |
| Agenda ("¿Qué aprenderemos?") | Promete "Modelo de Oferta y Demanda Agregada" | El deck solo cubre cómo la política monetaria **desplaza** DA; el modelo OA–DA no se desarrolla |

---

## Preguntas de repaso

- ¿Por qué la aproximación $r \approx i - \pi$ no sirve en Argentina? Calculá el error con $i=60\%$ y $\pi=50\%$.
- Un bono cotiza a 80 con cupones de 10% anual. ¿La TIR es mayor o menor al 10%? ¿Por qué?
- ¿Qué diferencia hay entre la tasa real ex-ante y la ex-post, y quién gana cuando la inflación sorprende para arriba?
- Escribí $m$ en función de $e$ y $r$, y explicá en una línea por qué $m$ cae cuando el público retiene más efectivo.
- Base monetaria \$200, con $e=0{,}5$, $r=0{,}2$. Calculá $m$, $M$, $E$, $D$, $R$ y los préstamos.
- ¿Por qué las Letras del Banco Central son pasivo del BC pero **no** son base monetaria?
- ¿Por qué la curva de oferta de dinero es vertical y la de demanda tiene pendiente negativa?
- El BC quiere bajar la tasa. ¿Compra o vende bonos? Dibujá qué pasa con MS y con el precio de los bonos.
- Explicá la neutralidad del dinero en tres pasos ($E_1 \to E_2 \to E_3$). ¿En cuál de los tres puntos hay efecto real?
- ¿Qué es el ancla nominal y qué ancla usa cada uno de los tres regímenes de §12?
- ¿Por qué el zero lower bound obliga a instrumentos no convencionales? ¿Cuáles son los cuatro?
- Si el QE multiplicó la base monetaria por 8, ¿por qué no hubo hiperinflación en EEUU?

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Economia)**

- [[Economia Intro]] — la clase introductoria ya anticipa el bloque "Dinero y Política Monetaria" en el contenido de la materia, y define el **costo de oportunidad** y el **análisis marginal** que acá reaparecen como base de la demanda de dinero
- [[Economia - Oferta, Demanda y Mercado]] — el mercado de dinero de §10 es un mercado más: oferta, demanda y equilibrio. La diferencia es que la oferta la fija una autoridad (curva vertical) en vez de surgir de decisiones descentralizadas
- [[Economia - Resumen Microeconomía]] — la primera mitad de la materia; el trade-off riesgo-retorno y el costo de oportunidad de §1 son la versión financiera de conceptos micro
- [[Economia - La cadena de distribución dejó de funcionar]] — artículo sobre por qué no fluye el crédito en Argentina: es este marco teórico aplicado, con los mismos indicadores de §3.3
- [[Materia - Economia]] — nota índice de la materia: cuatrimestre, temas y punto de entrada al resto de las clases

**Otras materias**

- **Derecho** [[Derecho - U4 Fondo de Comercio, Seguros, Bolsa y Concursos]] — cubre bolsa y mercado de valores, títulos de crédito y **obligaciones negociables** desde el lado jurídico; acá se ven los mismos instrumentos desde la valuación (prospecto, cupones, CACs)

<!-- notas-relacionadas:fin -->
