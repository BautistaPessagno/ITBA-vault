---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[Quimica.base|Quimica]]"
temas:
  - hidrolisis
  - soluciones reguladoras
  - titulacion
  - equilibrio de precipitacion
  - electroquimica
  - electrolisis
Created: 2026-06-04T17:43:00
---
# Mega Resumen — Segundo Parcial de Química

Resumen integrador para el **segundo parcial**. Cubre los 6 temas que aparecen siempre:
[[Hidrólisis]], soluciones reguladoras ([[Buffer]]), curvas de titulación ([[Curva de Titulacion]]),
[[Equilibrio de precipitacion]], [[Electroquímica]] (celdas galvánicas + Nernst) y electrólisis.

Vista del curso: [[Quimica.base]]

> [!abstract] Cómo está armado
> Cada tema tiene **(1) teoría y fórmulas**, **(2) la receta de resolución** y **(3) ejemplos de
> parcial resueltos paso a paso**. Al final hay una **tabla "¿qué fórmula uso?"** para repaso rápido.

---

## 0. Datos y constantes que hay que tener a mano

| Constante | Valor |
|---|---|
| $K_w$ (producto iónico del agua, 25 °C) | $10^{-14}$ |
| Relación pH | $pH + pOH = 14$ ; $pH = -\log[H_3O^+]$ |
| $F$ (Faraday) | $96500\ \text{C/mol } e^-$ |
| $R$ | $8{,}31\ \text{J/K·mol} = 0{,}082\ \text{L·atm/K·mol}$ |
| Factor de Nernst a 25 °C | $\dfrac{RT}{F}\ln \to \dfrac{0{,}059}{n}\log$ |

**Relación entre constantes de un par conjugado:** $\boxed{K_a \cdot K_b = K_w = 10^{-14}}$
- $K_{h,a} = \dfrac{K_w}{K_b}$ (hidrólisis ácida del catión)  · $K_{h,b} = \dfrac{K_w}{K_a}$ (hidrólisis básica del anión)

**Potenciales de reducción estándar frecuentes** ($E^\circ$ en V, los más usados en parciales):

| Hemirreacción (reducción) | $E^\circ$ (V) |
|---|---|
| $MnO_4^- + 8H^+ + 5e^- \to Mn^{2+} + 4H_2O$ | **+1,51** |
| $Cl_2 + 2e^- \to 2Cl^-$ | +1,36 |
| $O_2 + 4H^+ + 4e^- \to 2H_2O$ | **+1,23** |
| $IO_3^- /\ I_2$ | +1,20 |
| $Ag^+ + e^- \to Ag$ | **+0,80** |
| $Fe^{3+} + e^- \to Fe^{2+}$ | +0,77 |
| $O_2 + 2H_2O + 4e^- \to 4OH^-$ | +0,40 |
| $Cu^{2+} + 2e^- \to Cu$ | **+0,34** |
| $Sn^{4+} + 2e^- \to Sn^{2+}$ | +0,15 |
| $2H^+ + 2e^- \to H_2$ | **0,00** (ENH) |
| $I_2 + 2e^- \to 2I^-$ | +0,54 |
| $Ni^{2+} + 2e^- \to Ni$ | −0,25 |
| $Fe^{2+} + 2e^- \to Fe$ | −0,44 |
| $2H_2O + 2e^- \to H_2 + 2OH^-$ | **−0,83** |
| $Zn^{2+} + 2e^- \to Zn$ | −0,76 |
| $Mg^{2+} + 2e^- \to Mg$ | **−2,38** |
| $K^+ + e^- \to K$ | −2,92 |

> [!tip] Convención de Nernst de la cátedra
> Siempre se escribe $\Delta E = \Delta E^\circ - \dfrac{0{,}059}{n}\log Q$ con $Q = \dfrac{[\text{productos}]}{[\text{reactivos}]}$.
> **Los sólidos y el solvente (agua) valen 1** y no aparecen en $Q$. Los gases entran como su presión parcial.

---

## 1. Hidrólisis

### Teoría
Una **sal** proviene de un ácido y una base. Sus iones pueden reaccionar con el agua (**hidrolizar**)
regenerando el ácido/base débil de origen y liberando $H_3O^+$ o $OH^-$:

> [!important] ¿Qué ion hidroliza? (regla clave)
> - **Conjugado de un fuerte → NO hidroliza** (es espectador). Ej.: $Na^+$ (de NaOH), $Cl^-$ (de HCl), $K^+$, $NO_3^-$.
> - **Catión de base débil** (ej. $NH_4^+$) → hidroliza dando solución **ácida** ($pH<7$).
> - **Anión de ácido débil** (ej. $CH_3COO^-$, $CN^-$, $CO_3^{2-}$, $S^{2-}$) → hidroliza dando solución **básica** ($pH>7$).
> - **Sal de fuerte + fuerte** (ej. $NaCl$) → ninguno hidroliza → **neutra** ($pH=7$).
> - **Sal de débil + débil** → hidrolizan los dos; el pH lo decide la constante más grande.

### Fórmulas
Hidrólisis de un **anión** (básica):
$$A^- + H_2O \rightleftharpoons HA + OH^- \qquad K_{h,b} = \frac{[HA][OH^-]}{[A^-]} = \frac{K_w}{K_a}$$
Hidrólisis de un **catión** (ácida):
$$B^+ + H_2O \rightleftharpoons BOH + H^+ \qquad K_{h,a} = \frac{[BOH][H^+]}{[B^+]} = \frac{K_w}{K_b}$$

Con concentración inicial de sal $C$ y $x$ lo que hidroliza:
$$K_h = \frac{x^2}{C-x} \;\;(\approx \tfrac{x^2}{C}\text{ si } x \ll C)$$
- Anión: $x=[OH^-] \Rightarrow pOH=-\log x \Rightarrow pH = 14 - pOH$.
- Catión: $x=[H^+] \Rightarrow pH=-\log x$.
- **Grado de hidrólisis:** $\alpha = \dfrac{x}{C}$ (fracción de sal hidrolizada; se suele dar en %).

### Receta
1. Escribir la disociación total de la sal → identificar qué ion hidroliza.
2. Plantear el equilibrio de hidrólisis de ese ion.
3. Calcular $K_h = K_w/K_{a\,\text{ó}\,b}$ del **conjugado débil**.
4. Resolver $K_h = x^2/(C-x)$, sacar $[H^+]$ ó $[OH^-]$, y de ahí el pH.

### Ejemplo 1 — pH de sales 0,1 M (parcial 1C2018, adic. 4)
**(ii) $NH_4Cl$ 0,1 M** ($K_b$ del $NH_3 = 1{,}8\times10^{-5}$):
$$NH_4^+ + H_2O \rightleftharpoons NH_3 + H_3O^+ \qquad K_{h,a}=\frac{10^{-14}}{1{,}8\times10^{-5}}=5{,}56\times10^{-10}$$
$$5{,}56\times10^{-10}=\frac{x^2}{0{,}1}\Rightarrow x=[H_3O^+]=7{,}5\times10^{-6}\Rightarrow \boxed{pH=5{,}1}\ (\text{ácida, ok})$$
**(iii) $CH_3COONa$ 0,1 M** ($K_a$ del acético $=1{,}8\times10^{-5}$): mismo $K_h=5{,}56\times10^{-10}$, pero ahora $x=[OH^-]=7{,}5\times10^{-6}$ → $\boxed{pH=8{,}9}$ (básica).
**(iv) $NaCl$:** ni $Na^+$ ni $Cl^-$ hidrolizan → $\boxed{pH=7}$.

### Ejemplo 2 — Grado de hidrólisis de NaCN 0,2 M (adic. 20)
$K_a$(HCN)$=4\times10^{-10}$ → $K_{h,b}=\dfrac{10^{-14}}{4\times10^{-10}}=2{,}5\times10^{-5}$.
$$2{,}5\times10^{-5}=\frac{x^2}{0{,}2}\Rightarrow x=2{,}24\times10^{-3}\Rightarrow \alpha=\frac{x}{0{,}2}=1{,}12\times10^{-2}\Rightarrow \boxed{\alpha \approx 1{,}12\%}$$

---

## 2. Soluciones reguladoras (Buffer)

Ver también [[Buffer]].

### Teoría
Un **buffer** resiste cambios bruscos de pH. Está formado por un **par conjugado ácido-base débil**
en **concentraciones similares y apreciables** ($HA + A^-$, ó $B + BH^+$).
- Si se agrega $H^+$: lo neutraliza la base conjugada → $A^- + H^+ \to HA$.
- Si se agrega $OH^-$: lo neutraliza el ácido → $HA + OH^- \to A^- + H_2O$.

### Fórmula — Henderson-Hasselbalch
Partiendo de $K_a=\dfrac{[H_3O^+]\,C_A}{C_{HA}}$ (las variaciones $x$ son despreciables porque cada
componente inhibe al otro):
$$\boxed{[H_3O^+] = K_a\,\frac{C_{HA}}{C_{A^-}}} \qquad pH = pK_a + \log\frac{[A^-]}{[HA]}$$

> [!note] Da igual plantear desde el ácido o desde la base
> Como tenés ácido **y** base conjugada desde el inicio, $K_a$ y $K_{h,b}=K_w/K_a$ llevan al mismo
> resultado. **Conviene usar $K_a$** porque despejás $[H_3O^+]$ directo.

**Rango útil / capacidad buffer:** funciona bien si $\frac{[A^-]}{[HA]}\in[0{,}1;\ 10]$, es decir
$pH = pK_a \pm 1$. Fuera de ahí pierde capacidad. Si se agrega tanto ácido/base que **se agota un
componente**, deja de ser buffer (pasa a ser mezcla de ácidos / exceso de base fuerte).

> [!tip] Un buffer aparece solo en la titulación
> En una titulación de ácido débil con base fuerte, en la **zona "durante"** coexisten el ácido sin
> neutralizar y la base conjugada formada → **es un buffer**. Por eso el pH cambia poco ahí (zona plana).

### Receta
1. Identificar el par conjugado y sus moles/concentraciones (¡ojo si hay reacción previa que los genera!).
2. Aplicar $[H_3O^+]=K_a\,\frac{C_{HA}}{C_{A^-}}$ → pH.
3. Si agregan ácido/base fuerte: actualizar moles ($A^-+H^+\to HA$ ó $HA+OH^-\to A^-$) y recalcular.

### Ejemplo 1 — Relación para un pH dado (adic. 19)
Buffer de pH = 4,65 con ácido acético ($pK_a=-\log(1{,}8\times10^{-5})=4{,}74$):
$$4{,}65=4{,}74+\log\frac{[A^-]}{[HA]}\Rightarrow \log\frac{[A^-]}{[HA]}=-0{,}09\Rightarrow \frac{[HA]}{[A^-]}=\boxed{1{,}24}$$

### Ejemplo 2 — pH de una mezcla buffer (adic. 22)
0,6 L con 0,3 mol acético y 0,225 mol acetato, $K_a=1{,}8\times10^{-5}$:
$$[H_3O^+]=1{,}8\times10^{-5}\cdot\frac{0{,}3}{0{,}225}=2{,}4\times10^{-5}\Rightarrow \boxed{pH=4{,}62}$$
(El volumen se cancela porque es la **razón** de moles.)

### Ejemplo 3 — Poder regulador (teórica)
Buffer pH=4 ($C_{HA}=C_{A^-}=0{,}1$ M). Agregás 0,01 mol HCl a 1 L:
$$HCl+A^-\to HA+Cl^- \Rightarrow C_{HA}=0{,}11,\ C_{A^-}=0{,}09$$
$$[H_3O^+]=10^{-4}\cdot\tfrac{0{,}11}{0{,}09}=1{,}22\times10^{-4}\Rightarrow pH=3{,}91$$
El buffer pasó de 4 a 3,91 (Δ0,09). Una solución de HCl al mismo pH inicial habría pasado a **pH=2** (Δ2,0). **El buffer resistió.**

---

## 3. Curvas de titulación

Ver [[Curva de Titulacion]].

### Teoría
Se agrega lentamente un reactivo (titulante, concentración conocida) sobre el **analito** hasta el
**punto de equivalencia**: el momento en que los moles se igualan según la estequiometría
($n_{\text{ácido}}=n_{\text{base}}$ para mono-próticos 1:1). La reacción de **neutralización** se
considera completa (flecha única →). El gráfico es **pH (y) vs. volumen de titulante (x)**.
Los **indicadores** ácido-base cambian de color en un rango de pH; se elige uno cuyo viraje coincida
con el salto de la curva en el punto de equivalencia.

### Las 4 regiones a calcular

| Región | Qué hay en solución | Cómo calcular el pH |
|---|---|---|
| **Antes** (solo analito) | ácido (o base) puro | Equilibrio del ácido/base débil ($K_a$/$K_b$) o disociación total si es fuerte |
| **Durante** (antes del p. eq.) | ácido sin reaccionar + base conjugada formada | **Buffer** → Henderson |
| **Punto de equivalencia** | solo la sal (productos) | **Hidrólisis** de la sal ($K_h$). ¡No da pH=7 si es débil/fuerte! |
| **Exceso** (pasado el p. eq.) | exceso de titulante fuerte | El pH lo fija el fuerte en exceso |

**Forma de las curvas:**
- **Fuerte/fuerte:** p. eq. en **pH = 7**, salto muy grande y vertical.
- **Débil/fuerte** (ác. débil + base fuerte): empieza más arriba, hay **zona buffer plana**, y el p. eq. está en **pH > 7** (la sal hidroliza dando base). En la mitad de la titulación ($V=\tfrac12 V_{eq}$) → $pH = pK_a$.
- **Débil con base débil:** salto poco marcado, difícil de detectar el p. eq.

### Receta general
1. $n_{\text{ácido}} = n_{\text{base}}$ en el punto de equivalencia → sirve para hallar concentraciones desconocidas.
2. Identificar en qué región estoy (comparar moles agregados vs. moles necesarios).
3. Aplicar el cálculo de pH de esa región (tabla de arriba).
4. ⚠️ Recalcular **concentraciones con el volumen total** (volúmenes aditivos).

### Ejemplo 1 — CH₃COOH 0,1 M titulado con NaOH 0,1 M, 50 mL (adic. 16)
$K_a=1{,}8\times10^{-5}$. Resultados de referencia:
- **a) Antes:** ácido débil puro → $pH=2{,}9$.
- **b) 50% neutralizado:** $C_{HA}=C_{A^-}$ → buffer → $pH=pK_a=4{,}7$.
- **c) Punto de equivalencia:** todo es acetato → hidrólisis básica → $pH=8{,}72$.
- **d) Agregados 100 mL NaOH (exceso):** lo fija el $OH^-$ en exceso → $pH=12{,}52$.

### Ejemplo 2 — Ácido débil HA, hallar pH lejos del p. eq. (parcial 1C2023-Ej2)
*"Se titulan 10 mL de HA con NaOH 0,1 M, gastándose 23 mL; en el p. eq. pH=9,27. Hallar el pH al
mezclar 10 mL de ese ácido con 10 mL de NaOH 0,1 M."*

**Paso A — sacar $K_a$ desde el dato del p. eq.:**
En el p. eq.: $n_{HA}=n_{NaOH}=0{,}1\cdot0{,}023=2{,}3\times10^{-3}$ mol, en $V=33$ mL → $[A^-]=0{,}07$ M.
$$pH=9{,}27\Rightarrow pOH=4{,}73\Rightarrow[OH^-]=1{,}9\times10^{-5}$$
$$K_{h,b}=\frac{[OH^-]^2}{C_{A^-}}=\frac{(1{,}9\times10^{-5})^2}{0{,}07}=4{,}95\times10^{-9}$$
**Paso B — la mezcla pedida** (10 mL HA con $n_{HA}=2{,}3\times10^{-3}$ + 10 mL NaOH con $n=10^{-3}$, $V=0{,}02$ L):
$$HA+NaOH\to A^-+H_2O:\quad n_{HA}=1{,}3\times10^{-3},\ n_{A^-}=10^{-3}$$
Queda ácido **y** base conjugada → es un **buffer/hidrólisis parcial**. Con
$[A^-]=0{,}05$ M y $[HA]=0{,}065$ M y $K_{h,b}=4{,}95\times10^{-9}$:
$$4{,}95\times10^{-9}=\frac{x(x+0{,}05)}{0{,}065-x}\Rightarrow x=[OH^-]=3{,}8\times10^{-9}\Rightarrow \boxed{pH=5{,}58}$$

---

## 4. Equilibrio de precipitación

Ver [[Equilibrio de precipitacion]].

### Teoría
Una sal poco soluble en equilibrio con sus iones tiene una **constante de producto de solubilidad $K_{ps}$**.
El sólido (reactivo) **no aparece** en la constante.
$$A_aB_b\,(s) \rightleftharpoons a\,A^{+}+b\,B^{-}\qquad K_{ps}=[A^+]^a[B^-]^b$$

**Solubilidad $s$** = concentración de la solución saturada. Relación con $K_{ps}$ según estequiometría:
- $AB$ (ej. AgCl): $K_{ps}=s^2$
- $A_2B$ ó $AB_2$ (ej. Ag₂CrO₄, Mg(OH)₂): $K_{ps}=4s^3$
- $A_aB_b$: $K_{ps}=a^a b^b\, s^{a+b}$

**Efecto ión común:** la presencia de otra sal con un ion común **disminuye la solubilidad** (Le Chatelier).

### ¿Precipitará? Criterio $Q_{ps}$ vs $K_{ps}$
Se calcula el **producto iónico $Q_{ps}$** con las concentraciones **reales** del momento (¡con el
volumen total de la mezcla!):

| Comparación | Estado |
|---|---|
| $Q_{ps} < K_{ps}$ | No saturada, **no precipita** |
| $Q_{ps} = K_{ps}$ | Justo saturada (equilibrio) |
| $Q_{ps} > K_{ps}$ | Sobresaturada, **precipita** |

### Sulfuros y dependencia con el pH
La precipitación de sulfuros se controla con el pH porque el $S^{2-}$ viene del $H_2S$:
$$H_2S \rightleftharpoons 2H^+ + S^{2-}\qquad P_i=[H^+]^2[S^{2-}]$$
Bajando el pH (más $H^+$) se desplaza el equilibrio y **baja $[S^{2-}]$** → cuesta más precipitar el sulfuro. Esto permite separar cationes selectivamente.

### Disolución de precipitados
Para disolver un sólido hay que lograr $Q_{ps}<K_{ps}$ (bajar la concentración de algún ion):
- **Modificar el pH** (si un ion es básico/ácido, ej. hidróxidos, carbonatos, sulfuros).
- **Reacciones redox** (cambiar el estado de oxidación de un ion).
- **Formación de iones complejos:** un ligando "captura" al catión bajando su concentración libre.
  La estabilidad del complejo se mide con la **constante de inestabilidad $K_i$** (cuanto menor $K_i$, más estable):
  $$[ML_n]^{x} \rightleftharpoons M^{x+} + n\,L \qquad K_i=\frac{[M^{x+}][L]^n}{[ML_n]}$$

### Receta "¿precipita?"
1. Disociar todas las sales; calcular **moles** de cada ion.
2. Pasar a **concentración con el volumen total** de la mezcla.
3. Armar $Q_{ps}$ con la estequiometría correcta y comparar con $K_{ps}$.

### Ejemplo 1 — ¿Precipita Ag₂CrO₄? (parcial 2C2022-Ej3)
250 mL AgNO₃ 0,01 M + 500 mL K₂CrO₄ 0,005 M; $K_{ps}=1{,}1\times10^{-12}$. $V_{tot}=0{,}75$ L:
$$[Ag^+]=\frac{0{,}01\cdot0{,}25}{0{,}75}=0{,}003\ \text{M};\quad [CrO_4^{2-}]=\frac{0{,}005\cdot0{,}5}{0{,}75}=0{,}0033\ \text{M}$$
$$Q_{ps}=[Ag^+]^2[CrO_4^{2-}]=(0{,}003)^2(0{,}0033)=3{,}0\times10^{-8}$$
$Q_{ps}=3{,}0\times10^{-8} > K_{ps}=1{,}1\times10^{-12}$ → **precipita**.

### Ejemplo 2 — Precipita Fe(OH)₂ y luego se disuelve con NH₄Cl (parcial 1C2023-Ej3)
100 mL FeCl₂ $10^{-3}$ M + 300 mL NH₃ 0,4 M; $K_{ps}\,\text{Fe(OH)}_2=8\times10^{-16}$, $K_b\,\text{NH}_3=1{,}8\times10^{-5}$.
**a)** El $NH_3$ genera $OH^-$: $K_b=\frac{x^2}{0{,}4-x}\Rightarrow [OH^-]=2{,}7\times10^{-3}$ M (en 0,3 L; al diluir a $V_{tot}=0{,}4$ L → $[OH^-]\approx0{,}002$ M). $[Fe^{2+}]=\frac{10^{-3}\cdot0{,}1}{0{,}4}=2{,}5\times10^{-4}$.
$$Q_{ps}=(2{,}5\times10^{-4})(0{,}002)^2=10^{-9} > K_{ps}\Rightarrow \textbf{precipita}$$
**b)** Para **disolverlo** se busca $Q_{ps}=K_{ps}$ (límite): con $[Fe^{2+}]$ fijo,
$[OH^-]=\sqrt{K_{ps}/[Fe^{2+}]}=1{,}8\times10^{-6}$ M. Hay que bajar el $OH^-$ agregando $NH_4Cl$ (ion común
$NH_4^+$ que desplaza el equilibrio del $NH_3$). Resolviendo $K_b=\frac{C_i\cdot1{,}8\times10^{-6}}{0{,}3}$ → $C_i\approx3$ M de $NH_4Cl$ → $n=1{,}2$ mol → con $M_r=53{,}5$ → $\boxed{\approx 64{,}2\ \text{g}}$.

### Ejemplo 3 — Mg(OH)₂ (adic. 26)
Agregar $NH_4Cl$ a NH₃ para que el pH baje 3 unidades y ver si precipita Mg(OH)₂ ($K_{ps}=1{,}1\times10^{-11}$): se necesitan **0,19 mol** de $NH_4Cl$ y con ese $[OH^-]$ rebajado, $Q_{ps}<K_{ps}$ → **no precipita**.

---

## 5. Electroquímica — celdas galvánicas y Nernst

Ver [[Electroquímica]].

### Repaso REDOX y balanceo (método ión-electrón)
En una reacción **redox** una especie se **oxida** (pierde $e^-$, sube su número de oxidación) y otra
se **reduce** (gana $e^-$). Para balancear:
1. Separar en dos **hemirreacciones** (oxidación y reducción).
2. Balancear el átomo que cambia, luego **O con $H_2O$**, **H con $H^+$** (medio ácido). En medio
   básico, agregar $OH^-$ a ambos lados para neutralizar los $H^+$.
3. Balancear carga con $e^-$.
4. Multiplicar cada hemirreacción para **igualar los $e^-$** y sumar.

### Celda galvánica (pila)
Aprovecha una reacción **espontánea** para producir electricidad. Componentes: dos electrodos, dos
electrolitos y un **puente salino**.

> [!important] Ánodo vs Cátodo
> - **Ánodo** → ocurre la **oxidación** → polo **negativo** (–).
> - **Cátodo** → ocurre la **reducción** → polo **positivo** (+).
> - Los electrones van del ánodo al cátodo por el cable.

**Notación de celda:** `Ánodo / ion (c) // ion (c) / Cátodo`. La doble barra `//` es el puente salino;
la simple `/` una interfaz. El **ánodo va a la izquierda**.

### Potencial estándar y espontaneidad
$$\Delta E^\circ = E^\circ_{\text{cátodo}} - E^\circ_{\text{ánodo}}\quad(\text{ambos como potenciales de reducción})$$
$$\Delta G = -n F\,\Delta E$$

| $\Delta E$ | $\Delta G$ | Reacción |
|---|---|---|
| $> 0$ | $< 0$ | **espontánea** (pila funciona) |
| $= 0$ | $= 0$ | equilibrio |
| $< 0$ | $> 0$ | no espontánea (necesita electrólisis) |

### Ecuación de Nernst (fuera del estándar)
$$\boxed{\Delta E = \Delta E^\circ - \frac{0{,}059}{n}\log Q}\qquad Q=\frac{[\text{productos}]}{[\text{reactivos}]}$$
- **Constante de equilibrio:** en el equilibrio $\Delta E=0$ y $Q=K$:
  $$\log K = \frac{n\,\Delta E^\circ}{0{,}059}$$
- **Dependencia con el pH:** si en la reacción aparecen $H^+$/$OH^-$, $\Delta E$ depende del pH (relación lineal con el pH). Sirve para hallar el pH al que la reacción se invierte ($\Delta E=0$).
- **Celdas de concentración:** $\Delta E^\circ=0$ (mismo electrodo en ambos lados); el potencial nace solo de la diferencia de concentraciones.

> [!tip] Truco para hallar $n$
> Planteá el balance redox completo: la cantidad de electrones intercambiados es el $n$ de Nernst.

### Receta — celda galvánica
1. Identificar quién se oxida y quién se reduce (mayor $E^\circ$ de reducción → es el cátodo).
2. $\Delta E^\circ = E^\circ_{cát}-E^\circ_{án}$. Si $>0$, espontánea como está escrita.
3. Si las concentraciones ≠ 1 M, aplicar Nernst con el $Q$ y el $n$ del balance.

### Ejemplo 1 — fem de una pila Zn / H⁺ (parcial 1C2018-Ej3)
`Zn / Zn²⁺(0,1 M) // H⁺(0,05 M) / H₂(0,2 atm) / Pt`. Ánodo: $Zn\to Zn^{2+}+2e^-$; cátodo: $2H^++2e^-\to H_2$.
Global: $Zn + 2H^+ \to Zn^{2+}+H_2$, $n=2$, $\Delta E^\circ=0-(-0{,}76)=0{,}76$ V:
$$\Delta E = 0{,}76-\frac{0{,}059}{2}\log\frac{P_{H_2}\,[Zn^{2+}]}{[H^+]^2}=0{,}76-\frac{0{,}059}{2}\log\frac{0{,}2\cdot0{,}1}{(0{,}05)^2}=\boxed{0{,}73\ \text{V}}$$

### Ejemplo 2 — K y pH de inversión (parcial 2C2022-Ej1)
`Ag/Ag⁺(0,01) // MnO₄⁻(0,01); H⁺(x); Mn²⁺(0,01)/Pt`. Reducción $MnO_4^-$ ($n=5$), oxidación Ag.
$E^\circ=E^\circ_{MnO_4^-/Mn^{2+}}-E^\circ_{Ag^+/Ag}=1{,}51-0{,}80=0{,}71$ V.
$$\log K=\frac{n\,E^\circ}{0{,}059}=\frac{5\cdot0{,}71}{0{,}059}=60{,}17\Rightarrow K=10^{60{,}17}$$
**b)** Se invierte cuando $\Delta E=0$ (equilibrio aparente con las concentraciones dadas salvo $H^+$):
despejando $[H^+]$ de $Q=K$ → $[H^+]=10^{-8{,}65}$ → $\boxed{pH=8{,}65}$.

### Ejemplo 3 — Espontaneidad y pH de equilibrio (parcial 1C2023-Ej4)
$Br^- + MnO_4^- + H_2O \rightleftharpoons BrO_3^- + MnO_2(s) + OH^-$, con todas las [especies]=1 M y $[OH^-]=1{,}7\times10^{-7}$.
Balanceada ($n=6$), $\Delta E^\circ=0{,}59-0{,}61=-0{,}02$… se calcula
$\Delta E=-0{,}02-\frac{0{,}059}{6}\log[OH^-]^2 = 0{,}11$ V $>0$ → **espontánea** en esas condiciones.
**b)** Equilibrio ($\Delta E=0$): $[OH^-]=0{,}096$ M. **c)** $\log K=\frac{6(-0{,}02)}{0{,}059}\Rightarrow K=9{,}2\times10^{-3}$.

---

## 6. Electrólisis

### Teoría — celda electrolítica
Es lo **inverso** de la pila: se usa una **fuente externa** para forzar una reacción **no espontánea**
($\Delta E<0$). Se invierten los signos respecto de la galvánica:

> [!important] Signos invertidos en electrólisis
> - **Cátodo** = reducción = polo **negativo** (–) (conectado al – de la fuente). Hacia él van los cationes.
> - **Ánodo** = oxidación = polo **positivo** (+). Hacia él van los aniones.

### Leyes de Faraday
> *"La cantidad de sustancia oxidada/reducida en un electrodo es proporcional a la carga que circula."*
$$Q = i\cdot t \qquad n_{e^-}=\frac{Q}{F}=\frac{i\,t}{96500}$$
Luego, por estequiometría de la hemirreacción, se obtienen los moles/masa de producto. Si hay
**rendimiento de corriente** $<100\%$, se multiplica $Q$ útil por ese factor. Para gases se usa $PV=nRT$.

### Criterio: ¿sal fundida o solución acuosa? (la trampa clásica)
En solución acuosa, **el agua compite** con los iones por reducirse/oxidarse:
- Reducción del agua: $2H_2O+2e^-\to H_2+2OH^-$ ($E^\circ=-0{,}83$ V)
- Oxidación del agua: $2H_2O\to O_2+4H^++4e^-$ ($E^\circ=+1{,}23$ V; algunos enunciados dan $0{,}40$ con $O_2+2H_2O+4e^-\to4OH^-$)

> [!warning] Cómo decidir qué especie reacciona
> En cada electrodo calculá el $\Delta E$ (con Nernst, usando las concentraciones reales) de **la especie**
> y **del agua**, y elegí la que tenga el **potencial de reducción más alto** para reducirse (cátodo) /
> el más alto para oxidarse en el caso del ánodo. Si gana el agua, **no podés obtener el metal en
> acuoso** → hay que usar la **sal fundida**.
> Caso típico: $Mg^{2+}$ ($E=-2{,}38$) pierde contra el agua ($-0{,}83$) → el Mg metálico **solo se obtiene de MgCl₂ fundido**.

### Receta — electrólisis
1. Disociar la sal; ubicar catión → cátodo (red.), anión → ánodo (ox.).
2. En **acuoso**, comparar cada hemirreacción con la del agua vía Nernst → elegir la real.
3. Escribir hemirreacciones + global.
4. $Q=i\,t$ (× rendimiento si lo hay) → $n_{e^-}=Q/F$ → moles/masa/volumen por estequiometría.
5. Para el pH final: contar los $H^+$ ó $OH^-$ generados en los electrodos.

### Ejemplo 1 — Obtener Mg metálico (parcial 1C2023-Ej1)
Se quieren 10 g de Mg de MgCl₂; $i=5$ A.
**a)** Para reducir $Mg^{2+}$ ($\Delta E=-2{,}41$ V con Nernst) vs. reducir agua ($\Delta E=-0{,}3$ V): gana el agua → **hay que usar la sal fundida**.
**b)** Cátodo: $Mg^{2+}+2e^-\to Mg$; ánodo: $2Cl^-\to Cl_2+2e^-$; global $Mg^{2+}+2Cl^-\to Mg+Cl_2$.
**c)** $n_{Mg}=\frac{10}{24{,}31}=0{,}41$ mol → $Q=0{,}41\cdot2\cdot96500=79130$ C → $t=\frac{79130}{5}=\boxed{15826\ \text{s}}$.

### Ejemplo 2 — Recubrir con Ni; ¿se puede con Mg? (parcial 2C2022-Ej4)
Electrólisis de NiSO₄ para depositar 10 g de Ni con $i=5$ A.
**a)** $Ni^{2+}+2e^-\to Ni$ (reducción, ocurre en el **cátodo, polo –**) → el objeto se conecta al **polo negativo**.
**b)** $n_{Ni}=\frac{10}{58{,}7}=0{,}17$ mol → $Q=0{,}17\cdot2\cdot96500=32810$ C → $t=\frac{32810}{5}=\boxed{6562\ \text{s}}$.
**c)** Con MgCl₂ acuoso **no** se puede (gana la reducción del agua, igual que el Ejemplo 1).

### Ejemplo 3 — Electrólisis de CuSO₄ acuoso (adic. 35)
1 L CuSO₄ 1 M, 2,3 A, 2 h, rendimiento 80%, electrodos inertes:
- **a)** Cátodo: $Cu^{2+}+2e^-\to Cu$ (gana al agua). Ánodo: $2H_2O\to O_2+4H^++4e^-$ (el $SO_4^{2-}$ ya está en máxima oxidación).
- **b)** Volumen de O₂ medido a 25 °C, 1,2 atm → $V=0{,}699$ L.
- **c)** pH final por los $H^+$ generados → $pH=0{,}86$. **d)** Si se diluye a 2 L → $pH=1{,}16$.

> [!tip] Si el ánodo es del mismo metal (no inerte)
> Con **ánodo de cobre** en CuSO₄, en vez de oxidarse el agua se **oxida el cobre del ánodo**
> ($Cu\to Cu^{2+}+2e^-$): el metal se disuelve en un electrodo y se deposita en el otro
> (principio de la **refinación electrolítica del cobre**).

---

## 7. Tabla final — "¿Qué fórmula uso?"

| Si el problema dice… | Tema | Ecuación clave | Trampa frecuente |
|---|---|---|---|
| "sal X M, calcular pH" | Hidrólisis | $K_h=\frac{K_w}{K_{a/b}}=\frac{x^2}{C}$ | Ver qué ion hidroliza; conjugado de fuerte = espectador |
| "grado de hidrólisis" | Hidrólisis | $\alpha=x/C$ | Darlo en % |
| "ácido débil + su sal", "resiste pH" | Buffer | $[H_3O^+]=K_a\frac{C_{HA}}{C_{A^-}}$ | El volumen se cancela (razón de moles) |
| "se titula… p. eq. pH=…" | Titulación | $n_{ác}=n_{base}$ + región | p. eq. NO es pH 7 si es débil/fuerte |
| "50% neutralizado" | Titulación | $pH=pK_a$ | — |
| "¿se forma precipitado?" | Precipitación | $Q_{ps}$ vs $K_{ps}$ | Concentraciones con **volumen total** |
| "solubilidad" | Precipitación | $K_{ps}=a^a b^b s^{a+b}$ | Estequiometría ($4s^3$ para AB₂) |
| "¿a qué pH precipita el sulfuro?" | Precipitación | $P_i=[H^+]^2[S^{2-}]$ | Bajar pH baja $[S^{2-}]$ |
| "disolver el precipitado" | Precipitación | buscar $Q_{ps}=K_{ps}$ | Ion común / pH / complejo / redox |
| "celda galvánica, calcular fem" | Electroquímica | $\Delta E=\Delta E^\circ-\frac{0{,}059}{n}\log Q$ | Sólidos y H₂O = 1; hallar $n$ con el balance |
| "constante de equilibrio de la reacción" | Electroquímica | $\log K=\frac{n\,\Delta E^\circ}{0{,}059}$ | — |
| "¿a qué pH se invierte / es espontánea?" | Electroquímica | $\Delta E=0$ y despejar $[H^+]$ | — |
| "¿espontánea?" | Electroquímica | $\Delta E>0 \Leftrightarrow \Delta G<0$ | $\Delta E^\circ=E^\circ_{cát}-E^\circ_{án}$ |
| "¿cuánto tiempo / qué masa deposita?" | Electrólisis | $Q=i\,t$, $n_{e^-}=Q/F$ | Rendimiento <100%; estequiometría de la hemirreacción |
| "¿sal fundida o acuosa?" / "obtener metal" | Electrólisis | comparar con Nernst del agua | Mg, Na, K, Al → **fundida** (gana el agua) |
| "pH después de la electrólisis" | Electrólisis | contar $H^+$/$OH^-$ generados | Usar volumen total |

---

> [!success] Fuentes integradas
> Notas de clase ([[Equilibrio de precipitacion]], [[Hidrólisis]], [[Curva de Titulacion]],
> [[Electroquímica]], [[Buffer]]), teóricas de *Soluciones reguladoras* y *Electrólisis*, compilación
> de segundos parciales resueltos (1C2023, 2C2022, Recu2C2022, 1C2018) y guía de *Problemas
> adicionales* (ej. 11–42 + parciales).
