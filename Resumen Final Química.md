---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[Quimica.base|Quimica]]"
temas:
  - final
  - uniones quimicas
  - geometria molecular
  - polaridad
  - fuerzas intermoleculares
  - solidos y liquidos
  - cambios de estado
  - gases
  - cinetica quimica
  - equilibrio quimico
  - acido base
  - hidrolisis
  - buffer
  - titulacion
  - equilibrio de precipitacion
  - electroquimica
  - electrolisis
Created: 2026-07-08T10:00:00
---
# Mega Resumen — FINAL de Química

Resumen integrador de **toda la materia** para el final. Cubre lo del **1er parcial**
(uniones, geometría, polaridad, fuerzas intermoleculares, sólidos/líquidos, cambios de estado,
gases, cinética y equilibrio) **y el 2do parcial** (ácido-base, hidrólisis, buffer, titulación,
precipitación, electroquímica, electrólisis).

Vista del curso: [[Quimica.base]] · Para el 2do parcial con **ejercicios resueltos paso a paso**: [[Resumen Segundo Parcial Química]]

> [!abstract] Cómo usar este resumen
> Cada tema tiene **teoría breve**, la **receta de resolución** y las **fórmulas clave**. Está pensado
> para leer rápido, entender el tema y mandarte a hacer ejercicios. Al final hay dos tablas: el
> **mapa del final** (qué tipo de ejercicio cae y a qué sección ir) y la **"¿qué fórmula uso?"**.

---

## 🗺️ Mapa del final (qué cae siempre)

El final son **4 ejercicios de 25 puntos**. Mirando finales viejos (14-07-23, 16-12-22, 22-12-22, 12-07-19, 19-07-19) el patrón es:

| Ejercicio típico | De qué trata | Sección |
|---|---|---|
| **Verdadero / Falso justificado** | Polaridad, geometría, fuerzas intermol., puntos de ebullición, cinética (orden/elemental/pseudo-orden/catalizador), equilibrio (sólidos, Le Chatelier), Kps/sulfuros, sólidos iónicos y conductividad, hidrólisis, Nernst vs pH | §1–§9 (ver callouts ⚠️ "trampa") |
| **Electroquímica**: celda galvánica o electrólisis | Hemirreacciones, balanceo ión-electrón, notación de celda, Nernst, ΔE, relación con pH, Faraday | §15, §16 |
| **Ácido-base / hidrólisis / buffer / titulación** | pH de sales, buffers, curva de titulación, punto de equivalencia | §10–§13 |
| **Precipitación / Kps** | ¿precipita?, solubilidad, efecto ión común, dependencia con pH, disolver precipitados | §14 |
| **Cinética** | órdenes, método integral, t½, Arrhenius/Ea, catalizador, gráficos | §8 |
| **Gases / equilibrio líquido-vapor** | gas ideal, presión de vapor, recipiente con gota de agua | §7 |

---

## 0. Constantes y datos a tener a mano

| Constante | Valor |
|---|---|
| $K_w$ (producto iónico del agua, 25 °C) | $10^{-14}$ |
| Relación pH | $pH+pOH=14$ ; $pH=-\log[H_3O^+]$ |
| $R$ | $8{,}314\ \text{J/K·mol}=0{,}082\ \text{L·atm/K·mol}$ |
| $F$ (Faraday) | $96500\ \text{C/mol }e^-$ |
| $N_A$ (Avogadro) | $6{,}022\times10^{23}$ |
| Factor de Nernst (25 °C) | $\dfrac{RT}{F}\ln \to \dfrac{0{,}059}{n}\log$ |
| $0\ °C$ | $273{,}15\ \text{K}$ ; $1\ \text{atm}=760\ \text{mmHg}$ |
| $K_a\cdot K_b$ de un par conjugado | $=K_w=10^{-14}$ |

**Potenciales de reducción estándar frecuentes** ($E^\circ$ en V):

| Hemirreacción (reducción) | $E^\circ$ (V) |
|---|---|
| $MnO_4^- + 8H^+ + 5e^- \to Mn^{2+}+4H_2O$ | **+1,51** |
| $Cl_2+2e^-\to 2Cl^-$ | +1,36 |
| $O_2+4H^++4e^-\to 2H_2O$ | **+1,23** |
| $Ag^++e^-\to Ag$ | **+0,80** |
| $Fe^{3+}+e^-\to Fe^{2+}$ | +0,77 |
| $I_2+2e^-\to 2I^-$ | +0,54 |
| $O_2+2H_2O+4e^-\to 4OH^-$ | +0,40 |
| $Cu^{2+}+2e^-\to Cu$ | **+0,34** |
| $2H^++2e^-\to H_2$ | **0,00** (ENH) |
| $Ni^{2+}+2e^-\to Ni$ | −0,25 |
| $Fe^{2+}+2e^-\to Fe$ | −0,44 |
| $Zn^{2+}+2e^-\to Zn$ | **−0,76** |
| $2H_2O+2e^-\to H_2+2OH^-$ | **−0,83** |
| $Mg^{2+}+2e^-\to Mg$ | **−2,38** |
| $Na^++e^-\to Na$ | −2,71 |
| $K^++e^-\to K$ | −2,92 |

---

# PARTE A — Primer parcial

## 1. Átomo, configuración electrónica y Lewis

- **Configuración electrónica:** orden de llenado $1s\,2s\,2p\,3s\,3p\,4s\,3d\dots$ Los **electrones de valencia** (última capa) son los que arman uniones.
  - Ej: $H=1s^1$ · $C=1s^2\,2s^2\,2p^2$ (4 val.) · $N=1s^2\,2s^2\,2p^3$ (5 val.) · $O$ (6 val.) · halógenos (7 val.).
- **Estructura de Lewis:** dibujar átomos con sus electrones de valencia, unir compartiendo pares y completar **octeto** (H con 2). El átomo central suele ser el menos electronegativo (nunca el H).
  - Contar: pares enlazantes (uniones) y **pares libres** (no enlazantes) del átomo central → definen la geometría (§3).

> [!receta] Para un V/F de "es polar / tiene tal geometría"
> 1. Config. electrónica → electrones de valencia. 2. Lewis. 3. Contar grupos alrededor del central (uniones + pares libres). 4. Geometría (§3). 5. Sumar vectores de momento dipolar → polar o no (§4).

---

## 2. Uniones químicas

La **unión química** es la fuerza que mantiene unidos a los átomos.

| Tipo | Qué es | Forma moléculas |
|---|---|---|
| **Iónica** | Atracción electrostática entre iones de carga opuesta (metal + no metal). Sales, óxidos e hidróxidos iónicos. | **No** (red cristalina) |
| **Covalente** | Comparten 1, 2 ó 3 pares de electrones (solapamiento de orbitales). No metales entre sí. | **Sí** |
| **Metálica** | Cationes fijos en una red + "mar" de electrones deslocalizados. | **No** |

**Tipos de unión covalente:** simple (1 par), doble (2 pares), triple (3 pares) y **dativa/coordinada** (el par lo aporta un solo átomo; físicamente igual a la simple).

### Uniones múltiples: σ y π ([[Uniones Multiples]])
- **σ (sigma):** unión sobre el **eje internuclear** (solapamiento frontal). Toda unión simple es σ.
- **π (pi):** solapamiento **lateral**, por fuera del eje; refuerza a la σ.
- **Doble** = 1σ + 1π · **Triple** = 1σ + 2π.

---

## 3. Geometría molecular — TRPECV y hibridación

**TRPECV** (repulsión de pares de electrones de valencia): los grupos electrónicos alrededor del átomo central se ubican **lo más separados posible**.
- **Geometría electrónica:** posición de *todos* los grupos (uniones + pares libres).
- **Geometría molecular:** posición solo de los **núcleos** (los pares libres "no se ven" pero sí empujan).
- Notación: **A** = átomo central, **B** = periféricos, **U** = pares libres.

| # grupos | Geom. electrónica | Geometría molecular | Hibridación | Ángulos |
|:--:|:--:|:--|:--:|:--:|
| 2 | Lineal | Lineal ($AB_2$) | $sp$ | 180° |
| 3 | Plana triangular | Plana triangular ($AB_3$) / Angular ($AB_2U$) | $sp^2$ | 120° |
| 4 | Tetraédrica | Tetraédrica ($AB_4$) / Piramidal ($AB_3U$) / Angular ($AB_2U_2$) | $sp^3$ | ≤109,5° |
| 5 | Bipiramidal trigonal | Bipiramidal ($AB_5$) / Sube-y-baja ($AB_4U$) / Forma T ($AB_3U_2$) / Lineal ($AB_2U_3$) | $sp^3d$ | 90/120/180° |
| 6 | Octaédrica | Octaédrica ($AB_6$) / Pirámide base cuadrada ($AB_5U$) / Cuadrado plano ($AB_4U_2$) | $sp^3d^2$ | 90/180° |

**Ejemplos clave:** $BeCl_2$ lineal · $BF_3$ plana triang. · $SO_2$ angular · $CH_4$ tetraédrica · $NH_3$ piramidal · $H_2O$ angular · $PCl_3$ piramidal · $HCN$ lineal ($H-C\equiv N$).

**Hibridación:** los orbitales atómicos "puros" no tienen los ángulos correctos, así que se combinan en **orbitales híbridos** ($sp$, $sp^2$, $sp^3$, $sp^3d$, $sp^3d^2$) — se lee directo de la fila de la tabla según el nº de grupos.

---

## 4. Polaridad y momento dipolar

En una unión covalente entre átomos **distintos**, el más **electronegativo** atrae más a los electrones → distribución de carga no homogénea. Esa unión es **polar** y tiene un **momento dipolar $\vec{\mu}$** (vector: magnitud, dirección y sentido, apunta hacia el más electronegativo).

$$\vec{\mu}=0 \Rightarrow \text{no polar} \qquad \vec{\mu}\neq 0 \Rightarrow \text{polar}$$

> [!important] Molécula polar ≠ tener uniones polares
> Para saber si la **molécula** es polar hay que **sumar vectorialmente** los momentos de cada unión, teniendo en cuenta la **geometría**.
> - Si por simetría se **cancelan** → molécula **no polar** aunque las uniones sean polares (ej: $CO_2$, $CH_4$, $CCl_4$, $BF_3$, $SF_6$).
> - Si **no se cancelan** → molécula **polar** (ej: $H_2O$, $NH_3$, $SO_2$, $PCl_3$).
> Trampa de final: *"una molécula con todas sus uniones covalentes polares siempre es polar"* → **FALSO**, depende de la geometría.

---

## 5. Fuerzas intermoleculares y puntos de ebullición

Mantienen unidas a las moléculas entre sí (**fases condensadas**: líquido y sólido). De más **débil** a más **fuerte**:

| Fuerza | Entre qué | Fuerza relativa |
|---|---|---|
| **London / dispersión** | *Todas* las moléculas (única en las **no polares**). Dipolos instantáneos; crece con la **masa molar / polarizabilidad** | Más débil |
| **Debye** (dipolo–dipolo inducido) | Molécula polar induce dipolo en una no polar | ↓ |
| **Keesom** (dipolo–dipolo) | Entre moléculas **polares** | ↑ |
| **Puente de hidrógeno** | H unido a **F, O o N** con otro F/O/N | Más fuerte (de las intermol.) |
| Ión–dipolo | Ión + molécula polar (soluciones) | Muy fuerte |

> [!warning] Trampa clásica de V/F: puntos de ebullición
> *"Las moléculas no polares tienen menor punto de ebullición que las polares"* → **cierto solo a igual masa molar**. Como London **crece con la masa molar**, una molécula no polar **grande** puede tener mayor $T_{eb}$ que una polar chica. Si el enunciado dice **"siempre"** → **FALSO** (depende de la masa).

**Relación:** más fuerza intermolecular → hay que dar más energía para separarlas → **mayor punto de ebullición** (y menor volatilidad).

**Regla de solubilidad:** *similar disuelve similar* (polar con polar, no polar con no polar).

---

## 6. Sólidos y líquidos

**Estados de agregación:** gaseoso (mucho vacío, sin interacción — fase no condensada) · líquido (partículas cerca, hay atracción — condensada) · sólido (movimiento restringido — condensada). Más fuerza de atracción → más sólido; más energía cinética (T) → más gaseoso.

**Tipos de sólidos** (según la fuerza que los une):

| Sólido | Unión | Propiedades |
|---|---|---|
| **Iónico** | Iónica (red de iones) | Duros, frágiles, alto punto de fusión |
| **Metálico** | Metálica | Conductores, maleables |
| **Covalente / red** | Covalente en toda la red (diamante, $SiO_2$) | Muy duros, altísimo p. de fusión |
| **Molecular** | London / dipolo / puente H | Blandos, bajo p. de fusión |

> [!warning] Conductividad de sólidos iónicos (trampa de V/F)
> Los sólidos iónicos **NO conducen en estado sólido** (los iones están fijos), pero **sí conducen fundidos (líquidos) o disueltos** en agua (iones libres). Si el enunciado dice *"conducen solo disueltos en agua"* → **FALSO** (también fundidos).

---

## 7. Cambios de estado, gases y equilibrio líquido–vapor

### Gas ideal
$$PV=nRT \qquad n=\frac{m}{M_r}$$
- **Presiones parciales (Dalton):** $P_{tot}=\sum P_i$, con $P_i=x_i\,P_{tot}$.

### Presión de vapor
El **vapor** es la fase gaseosa en equilibrio con su fase líquida. Su presión de equilibrio es la **presión de vapor $P_v$**.
> [!important] $P_v$ depende **solo de la temperatura** (no del volumen). Sustancias **volátiles** = bajas fuerzas de atracción → alta $P_v$.

- **Punto de ebullición normal:** T a la que $P_v = 1\ \text{atm}$ (en general, cuando $P_v$ = presión externa).

### Diagrama de fases (sustancia pura)
Curvas sólido–líquido, líquido–vapor y sólido–vapor. **Punto triple:** coexisten las 3 fases. **Punto crítico:** más allá no se distingue líquido de gas.

### Receta — recipiente con una gota de líquido (equilibrio L–V)
Recipiente cerrado de volumen $V$ a T, se mete una masa $m$ de líquido:
1. Calcular $P_f$ **suponiendo que todo se evapora**: $n=m/M_r$, $P_f=nRT/V$.
2. Comparar con $P_v(T)$:
   - Si $P_f \le P_v$ → **se evapora todo**, no hay equilibrio L–V, $P_{sistema}=P_f$.
   - Si $P_f > P_v$ → **queda líquido**, hay equilibrio L–V y $\boxed{P_{sistema}=P_v}$.
3. Masa en fase vapor: $n_{vap}=\dfrac{P_v\,V}{RT}$ → $m_{vap}=n_{vap}\,M_r$; **masa líquida** = $m-m_{vap}$.

---

## 8. Cinética química ([[Cinetica Quimica]])

### Velocidad de reacción
Para $aA+bB\to dD+eE$:
$$v=-\frac1a\frac{d[A]}{dt}=-\frac1b\frac{d[B]}{dt}=+\frac1d\frac{d[D]}{dt}=\dots$$
(a reactivos se les resta). Unidades: concentración/tiempo (o presión/tiempo en gases).

### Ley de velocidad y orden
$$v=k\,[A]^\alpha[B]^\beta$$
- $\alpha,\beta$ = **órdenes parciales** (se determinan **experimentalmente**, ¡no de los coeficientes!). Orden total $=\alpha+\beta$.
- $k$ depende solo de la **temperatura**.

> [!important] Elemental vs. no elemental
> El orden **coincide** con los coeficientes **solo si la reacción es elemental** (ocurre en una sola etapa). Trampa de V/F: *"$A+2B\to P$ es siempre de orden 3"* → **FALSO**, solo si es elemental.
> La **molecularidad** (nº de especies que chocan en una etapa elemental) sí sale de los coeficientes de esa etapa.

### Método integral (para 1 reactivo) — convención de la cátedra con $a\cdot k$
| Orden | Ley integrada | Recta ($y$ vs $t$) | $t_{1/2}$ |
|:--:|:--|:--|:--|
| **0** | $[A]=[A]_0-a k\,t$ | $[A]$ vs $t$ | $\dfrac{[A]_0}{2ak}$ |
| **1** | $\ln[A]=\ln[A]_0-a k\,t$ | $\ln[A]$ vs $t$ | $\dfrac{\ln 2}{ak}$ |
| **2** | $\dfrac{1}{[A]}=\dfrac{1}{[A]_0}+a k\,t$ | $\dfrac1{[A]}$ vs $t$ | $\dfrac{1}{ak\,[A]_0}$ |

> [!tip] ¿Qué orden es? Se prueba con cuál de las tres columnas de $y$ da una **recta**. La pendiente vale $\pm ak$ (para orden 2 es $+ak$; orden 0 y 1, $-ak$).

### Pseudo-orden
Si un reactivo está en **gran exceso**, su concentración casi no cambia → se lo considera **constante** y se absorbe en $k$. El orden observado baja (ej: una reacción de orden 2 real se ve como pseudo-orden 1).

### Efecto de la temperatura — Arrhenius
$$k=A\,e^{-E_a/RT}\qquad \ln\frac{k_2}{k_1}=\frac{E_a}{R}\left(\frac{1}{T_1}-\frac{1}{T_2}\right)$$
- $E_a$ = energía de activación; $A$ = factor de frecuencia; **T en Kelvin**.
- Sirve para: sabiendo cómo cambia $k$ con T, despejar $E_a$ (y viceversa).

### Catalizadores
Aceleran la reacción **bajando la $E_a$** (ofrecen un **mecanismo alternativo**); **no se consumen** y **no cambian el equilibrio ni el $\Delta H$**. Los negativos (inhibidores) la frenan.
> [!warning] Trampa: un catalizador **no** funciona "disminuyendo el número de etapas". Da un camino alternativo de menor $E_a$ (puede tener incluso otras/más etapas). En un gráfico [P] vs t, con catalizador se llega a **más producto en menos tiempo** (misma meseta de equilibrio, más rápido).

### Teorías y mecanismos
- **Teoría de colisiones:** para reaccionar, las moléculas deben chocar con suficiente energía ($\ge E_a$) y **orientación** correcta.
- **Complejo activado / estado de transición:** máximo de energía del camino de reacción.
- **Mecanismo:** suma de etapas elementales que da la reacción global; la **etapa lenta** determina la velocidad. Cada etapa elemental sí cumple la ley de velocidad con sus coeficientes.

---

## 9. Equilibrio químico ([[Equilibrio Quimico]])

Las reacciones son **reversibles**; en el **equilibrio dinámico** $v_{directa}=v_{inversa}$ (las concentraciones ya no cambian).

### Constante de equilibrio
$$aA+bB\rightleftharpoons cC+dD \qquad K_c=\frac{[C]^c[D]^d}{[A]^a[B]^b}$$
> [!important] **Sólidos y líquidos puros NO se incluyen** (actividad = 1). $K$ **no tiene unidades** y **depende solo de la temperatura**.

- **$K_p$** (gases, con presiones parciales): $K_p=K_c\,(RT)^{\Delta n}$, con $\Delta n=$ (moles gas productos − reactivos).
- **Relación con velocidades:** $K=\dfrac{k_{directa}}{k_{inversa}}$.
- Manipular reacciones: invertir → $1/K$; multiplicar por $n$ → $K^n$; sumar → se multiplican las $K$.

### Cociente de reacción $Q$
Mismo formato que $K$ pero con concentraciones **de cualquier momento**:
- $Q<K$ → avanza hacia **productos**. $Q=K$ → equilibrio. $Q>K$ → avanza hacia **reactivos**.

### Principio de Le Chatelier
Si se perturba un sistema en equilibrio, este responde **contrarrestando** la perturbación:

| Perturbación | Desplazamiento |
|---|---|
| ↑ reactivo (o ↓ producto) | hacia **productos** |
| ↑ producto (o ↓ reactivo) | hacia **reactivos** |
| ↑ presión (↓ volumen) | hacia el lado con **menos moles de gas** |
| ↑ temperatura | hacia donde **absorbe** calor (endo: →productos; exo: →reactivos) |

> [!important] Solo la **temperatura** cambia el valor de $K$. Concentración, presión y volumen desplazan el equilibrio pero **no** cambian $K$. Agregar/quitar un **sólido o líquido puro** no desplaza nada (actividad = 1).

---

# PARTE B — Segundo parcial

> [!note] Versión detallada con ejercicios resueltos
> Esta parte va condensada (teoría + receta). Para los **finales/parciales resueltos paso a paso** de cada tema, ver [[Resumen Segundo Parcial Química]].

## 10. Ácido-base (fundamentos)

- **Fuertes:** se disocian **totalmente** (HCl, HBr, HI, HNO₃, H₂SO₄, HClO₄; bases: NaOH, KOH, hidróxidos alcalinos). El pH sale directo de la concentración.
- **Débiles:** se disocian **parcialmente**, con constante $K_a$ (ácido) o $K_b$ (base):
$$HA\rightleftharpoons H^++A^- \quad K_a=\frac{[H^+][A^-]}{[HA]}=\frac{x^2}{C-x}\ \left(\approx\frac{x^2}{C}\ \text{si } x\ll C\right)$$
- $pH=-\log[H_3O^+]$ · $pH+pOH=14$ · $pK_a=-\log K_a$.
- Par conjugado: $\boxed{K_a\cdot K_b=K_w=10^{-14}}$.

## 11. Hidrólisis ([[Hidrólisis]])

Una **sal** proviene de un ácido y una base; sus iones pueden reaccionar con el agua.

> [!important] ¿Qué ion hidroliza?
> - **Conjugado de un fuerte → NO hidroliza** (espectador): $Na^+$, $K^+$, $Cl^-$, $NO_3^-$…
> - **Catión de base débil** (ej. $NH_4^+$) → solución **ácida** ($pH<7$).
> - **Anión de ácido débil** (ej. $CH_3COO^-$, $CN^-$, $S^{2-}$) → solución **básica** ($pH>7$).
> - **Fuerte + fuerte** ($NaCl$) → **neutra** ($pH=7$).

$$K_{h,b}=\frac{K_w}{K_a}\ (\text{anión})\qquad K_{h,a}=\frac{K_w}{K_b}\ (\text{catión})\qquad K_h=\frac{x^2}{C-x}$$
- Anión: $x=[OH^-]$; catión: $x=[H^+]$. **Grado de hidrólisis** $\alpha=x/C$.

**Receta:** disociar la sal → ver qué ion hidroliza → $K_h=K_w/K_{a\ ó\ b}$ del conjugado → resolver $K_h=x^2/C$ → pH.

## 12. Soluciones reguladoras / Buffer ([[Buffer]])

Par conjugado ácido-base débil en concentraciones **similares y apreciables**; resiste cambios de pH.
$$\boxed{[H_3O^+]=K_a\,\frac{C_{HA}}{C_{A^-}}}\qquad pH=pK_a+\log\frac{[A^-]}{[HA]}\ \text{(Henderson)}$$
- El **volumen se cancela** (es razón de moles). Rango útil: $pH=pK_a\pm1$.
- Si se agrega ácido/base fuerte: actualizar moles ($A^-+H^+\to HA$ ó $HA+OH^-\to A^-$) y recalcular.

## 13. Curva de titulación ([[Curva de Titulacion]])

Se agrega titulante hasta el **punto de equivalencia** ($n_{ácido}=n_{base}$). Gráfico pH vs volumen.

| Región | Qué hay | Cómo calcular pH |
|---|---|---|
| Antes | analito puro | equilibrio débil ($K_a/K_b$) o disociación total si fuerte |
| Durante | ácido sin reaccionar + conjugada | **Buffer** (Henderson) |
| Punto equivalencia | solo la sal | **Hidrólisis** ($K_h$) |
| Exceso | titulante fuerte en exceso | lo fija el fuerte |

- Fuerte/fuerte: p. eq. en **pH 7**. Débil/fuerte: p. eq. en **pH > 7**; a mitad de titulación $pH=pK_a$.
- ⚠️ Recalcular concentraciones con el **volumen total** (aditivo).

## 14. Equilibrio de precipitación / Kps ([[Equilibrio de precipitacion]])

$$A_aB_b(s)\rightleftharpoons a A^{+}+b B^{-}\qquad K_{ps}=[A^+]^a[B^-]^b\ (\text{el sólido no aparece})$$

**Solubilidad $s$:** $AB\Rightarrow K_{ps}=s^2$ · $AB_2/A_2B\Rightarrow K_{ps}=4s^3$ · general $K_{ps}=a^a b^b\,s^{a+b}$.

**¿Precipita?** Calcular $Q_{ps}$ con concentraciones reales (**volumen total**) y comparar:
- $Q_{ps}<K_{ps}$ → no precipita · $Q_{ps}=K_{ps}$ → saturada · $Q_{ps}>K_{ps}$ → **precipita**.

- **Efecto ión común:** una sal con ion común **baja la solubilidad** (Le Chatelier).
- **Sulfuros y pH:** $H_2S\rightleftharpoons 2H^++S^{2-}$; bajar el pH (más $H^+$) baja $[S^{2-}]$ → cuesta más precipitar. Por eso la precipitación de sulfuros **depende del pH**.
- **Disolver un precipitado:** lograr $Q_{ps}<K_{ps}$ (bajar [ión]) vía pH, redox o **ión complejo** (constante de inestabilidad $K_i$).

## 15. Electroquímica — celdas galvánicas y Nernst ([[Electroquímica]])

**Balanceo redox (ión-electrón):** separar en 2 hemirreacciones → balancear átomo que cambia, O con $H_2O$, H con $H^+$ (medio básico: neutralizar con $OH^-$) → balancear carga con $e^-$ → igualar $e^-$ y sumar.

**Celda galvánica** (pila): reacción **espontánea** produce electricidad.
> [!important] Ánodo vs Cátodo (pila)
> **Ánodo** = **oxidación** = polo **negativo** (−). **Cátodo** = **reducción** = polo **positivo** (+). Los $e^-$ van del ánodo al cátodo.
> Notación: `Ánodo / ion(c) // ion(c) / Cátodo` (ánodo a la izquierda; `//` = puente salino).

$$\Delta E^\circ=E^\circ_{cátodo}-E^\circ_{ánodo}\qquad \Delta G=-nF\,\Delta E$$
- $\Delta E>0 \Leftrightarrow \Delta G<0 \Rightarrow$ **espontánea**.

**Ecuación de Nernst:**
$$\boxed{\Delta E=\Delta E^\circ-\frac{0{,}059}{n}\log Q}\qquad Q=\frac{[\text{productos}]}{[\text{reactivos}]}$$
- Sólidos y agua = 1; gases entran como presión parcial. $n$ = electrones del balance.
- **Constante de equilibrio:** $\log K=\dfrac{n\,\Delta E^\circ}{0{,}059}$ (en el eq. $\Delta E=0$, $Q=K$).
- **Dependencia con pH:** si hay $H^+/OH^-$, $\Delta E$ es lineal con el pH → sirve para hallar el pH donde $\Delta E=0$.
- **Celda de concentración:** mismo electrodo en ambos lados → $\Delta E^\circ=0$; el potencial nace de la diferencia de concentraciones.


## 16. Electrólisis

Lo **inverso** de la pila: una **fuente externa** fuerza una reacción **no espontánea** ($\Delta E<0$).
> [!important] Signos invertidos (electrólisis)
> **Cátodo** = reducción = polo **negativo** (−). **Ánodo** = oxidación = polo **positivo** (+).

**Leyes de Faraday:** $\ Q=i\cdot t\quad;\quad n_{e^-}=\dfrac{Q}{F}=\dfrac{i\,t}{96500}$ → moles/masa por estequiometría de la hemirreacción (× rendimiento si <100 %; gases con $PV=nRT$).

> [!warning] ¿Sal fundida o solución acuosa? (trampa clásica)
> En **acuoso el agua compite**: reducción $2H_2O+2e^-\to H_2+2OH^-$ ($E^\circ=-0{,}83$); oxidación $2H_2O\to O_2+4H^++4e^-$ ($+1{,}23$). Comparar con Nernst la especie vs. el agua y gana el de **mayor potencial**. Si gana el agua **no se obtiene el metal en acuoso** → hay que usar la **sal fundida**. Caso típico: **Na, K, Mg, Al** → fundida.

**Receta:** disociar → catión al cátodo (red.), anión al ánodo (ox.) → en acuoso comparar con el agua vía Nernst → hemirreacciones + global → $Q=it$ → $n_{e^-}=Q/F$ → masa/volumen. pH final: contar $H^+/OH^-$ generados (volumen total).

---

## 17. Tabla maestra — "¿Qué fórmula uso?"

| Si el problema dice… | Tema | Ecuación clave | Trampa |
|---|---|---|---|
| "¿es polar? / qué geometría" | Geometría/Polaridad | Lewis → TRPECV → sumar $\vec\mu$ | Uniones polares ≠ molécula polar |
| "¿mayor punto de ebullición?" | Fuerzas intermol. | + fuerza → + $T_{eb}$ | No polar grande puede superar a polar chica (London) |
| "recipiente + gota de líquido" | Gases / L–V | $P_f=nRT/V$ vs $P_v$ | Si $P_f>P_v$: $P=P_v$ y queda líquido |
| "orden de reacción / t½" | Cinética | ley integrada (tabla §8) con $a k$ | Elemental para usar coeficientes; recta que ajusta |
| "$E_a$ / cambia k con T" | Cinética | $\ln\frac{k_2}{k_1}=\frac{E_a}{R}(\frac1{T_1}-\frac1{T_2})$ | T en Kelvin |
| "catalizador" | Cinética | baja $E_a$, no se consume | No cambia $K$ ni $\Delta H$; no "reduce etapas" |
| "¿hacia dónde se desplaza?" | Equilibrio | Le Chatelier | Solo T cambia $K$; sólidos/líq. no cuentan |
| "$K_p$ vs $K_c$" | Equilibrio | $K_p=K_c(RT)^{\Delta n}$ | $\Delta n$ solo de gases |
| "sal X M, pH" | Hidrólisis | $K_h=\frac{K_w}{K_{a/b}}=\frac{x^2}{C}$ | Ver qué ion hidroliza |
| "ácido débil + su sal" | Buffer | $[H_3O^+]=K_a\frac{C_{HA}}{C_{A^-}}$ | El volumen se cancela |
| "se titula… p. eq. pH=…" | Titulación | $n_{ác}=n_{base}$ + región | p. eq. ≠ 7 si débil/fuerte |
| "¿se forma precipitado?" | Precipitación | $Q_{ps}$ vs $K_{ps}$ | Concentración con **volumen total** |
| "solubilidad" | Precipitación | $K_{ps}=a^a b^b s^{a+b}$ | $4s^3$ para AB₂ |
| "celda galvánica, fem" | Electroquímica | $\Delta E=\Delta E^\circ-\frac{0{,}059}{n}\log Q$ | Sólidos y H₂O = 1; hallar $n$ del balance |
| "constante de equilibrio de la reacción" | Electroquímica | $\log K=\frac{n\Delta E^\circ}{0{,}059}$ | — |
| "¿espontánea? / ¿a qué pH se invierte?" | Electroquímica | $\Delta E>0$; $\Delta E=0$ y despejar $[H^+]$ | $\Delta E^\circ=E^\circ_{cát}-E^\circ_{án}$ |
| "¿tiempo / masa depositada?" | Electrólisis | $Q=it$, $n_{e^-}=Q/F$ | Rendimiento; estequiometría |
| "¿sal fundida o acuosa? / obtener metal" | Electrólisis | comparar con Nernst del agua | Na, K, Mg, Al → **fundida** |
| "sólidos iónicos conducen…" | Sólidos | — | Conducen fundidos o disueltos, NO sólidos |

---

> [!success] Fuentes
> Notas de clase del vault: [[Uniones Químicas]], [[Uniones Multiples]], [[Solidos y Liquidos]],
> [[Cambios de Estados]], [[Cinetica Quimica]], [[Equilibrio Quimico]], [[Hidrólisis]], [[Buffer]],
> [[Curva de Titulacion]], [[Equilibrio de precipitacion]], [[Electroquímica]] y
> [[Resumen Segundo Parcial Química]]. Patrón de examen tomado de la compilación **Química – Finales
> Viejos** (14-07-23, 16-12-22, 22-12-22, 12-07-19, 19-07-19).

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Química)**

- [[Uniones Químicas]] — uniones químicas
- [[Solidos y Liquidos]] — sólidos y líquidos
- [[Cambios de Estados]] — cambios de estado
- [[Cinetica Quimica]] — cinética
- [[Equilibrio Quimico]] — equilibrio
- [[Hidrólisis]] — hidrólisis
- [[Buffer]] — buffers
- [[Curva de Titulacion]] — titulación
- [[Equilibrio de precipitacion]] — precipitación
- [[Electroquímica]] — electroquímica
- [[Resumen Segundo Parcial Química]] — resumen del segundo parcial

<!-- notas-relacionadas:fin -->
