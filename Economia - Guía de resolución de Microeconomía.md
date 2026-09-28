---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[Economia.base|Economia]]"
Created: 2026-08-27
temas:
  - Guías prácticas de Microeconomía
  - Oferta y demanda
  - Equilibrio de mercado
  - Elasticidades
  - Excedente del consumidor y del productor
  - Controles de precios
  - Impuestos y subsidios
  - Comercio internacional y aranceles
  - Producción total, media y marginal
  - Costos de corto y largo plazo
  - Competencia perfecta
  - Monopolio
  - Regulación
  - Discriminación de precios
---
# Economia - Guía de resolución de Microeconomía

> Resumen operativo para resolver las GP1, GP2 y GP3. Reúne las fórmulas, los criterios de decisión y el orden de resolución que se repite en los ejercicios.

[Economia](Categories/Economia.base)

> [!note] Alcance de las fuentes
> La notación y los criterios económicos siguen las clases 001 a 005 y las tres guías prácticas. El ajuste por IPC, los subsidios, los aranceles y parte del comercio exterior aparecen como ejercicios, pero no están desarrollados en las filminas disponibles. En esos puntos se completa el procedimiento algebraico estándar.

## Mapa rápido

| Si el ejercicio pide... | Herramienta principal |
|---|---|
| Comparar precios de distintos años | Deflactar con el IPC |
| Equilibrio | Igualar $Q_D(P)=Q_S(P)$ |
| Sensibilidad en un punto | Elasticidad $\frac{dQ}{dX}\frac{X}{Q}$ |
| Sumar consumidores o empresas | Suma horizontal, cuidando los tramos activos |
| Precio máximo o mínimo | Comparar primero con $P^*$; después calcular escasez o excedente |
| Excedentes | Áreas entre precio, demanda y oferta |
| Producción | $PMe=Q/L$ y $PMg=dQ/dL$ |
| Completar costos | $CT=CF+CV$ y sus versiones medias y marginales |
| Producción óptima | $IM=CMg$ |
| Empresa competitiva | $P=IM=CMg$, sujeto a la condición de cierre |
| Monopolio | Hallar $IM$ desde la demanda, igualar $IM=CMg$ y volver a la demanda para obtener $P$ |
| Regulación de monopolio natural | Comparar $P=CMg$ con $P=CMe$ |
| Discriminación de tercer grado | $IM_1=IM_2=CMg$ |

## Método general

1. Identificar quién decide: mercado, consumidor, empresa competitiva o monopolista.
2. Escribir todas las ecuaciones en la misma variable. Conviene usar cantidades como función del precio para agregar y precio como función de la cantidad para calcular ingresos.
3. Resolver el punto candidato.
4. Verificar restricciones: $Q\ge 0$, tramo activo, precio regulado vinculante, condición de cierre y máximo en vez de mínimo.
5. Recién después calcular beneficios, excedentes, recaudación o pérdida social.
6. En el gráfico, marcar ambos ejes, el equilibrio inicial, la intervención y la distancia vertical entre los precios que paga y recibe cada lado.

## 1. Precios nominales y reales

Para expresar un precio nominal del período $t$ en pesos del período base $b$:

$$
P_t^{\text{real, base }b}=P_t^{\text{nominal}}\frac{IPC_b}{IPC_t}
$$

Algoritmo:

1. Elegir el IPC del año cuyos pesos pide el enunciado.
2. Multiplicar el precio nominal por $IPC_b/IPC_t$.
3. Comparar precios reales, no nominales.

Chequeo: si hubo inflación entre $b$ y $t$, entonces $IPC_t>IPC_b$ y el precio expresado en pesos antiguos debe ser menor que el nominal de $t$.

>[!important] Aclaracion
>base b: el año sobre donde nos queremos parar a hacer la comparacion (precios del año b)
>t: el ipc con el que comparamos

## 2. Oferta, demanda y equilibrio

### Equilibrio algebraico

Con funciones directas $Q_D(P)$ y $Q_S(P)$:

$$
Q_D(P^*)=Q_S(P^*)
$$

Primero se obtiene $P^*$ y luego:

$$
Q^*=Q_D(P^*)=Q_S(P^*)
$$

Si las funciones vienen en forma inversa, $P_D(Q)$ y $P_S(Q)$, se igualan directamente en $Q$.

### Movimiento o desplazamiento

| Cambio | Qué ocurre |
|---|---|
| Cambia el precio del propio bien | Movimiento sobre la curva |
| Cambia ingreso, gustos, población, expectativas o precio de otro bien | Se desplaza la demanda |
| Cambia tecnología, costo de factores, clima o expectativas del productor | Se desplaza la oferta |

Reglas de signo:

- Sube la demanda: suben $P^*$ y $Q^*$.
- Baja la demanda: bajan $P^*$ y $Q^*$.
- Sube la oferta: baja $P^*$ y sube $Q^*$.
- Baja la oferta: sube $P^*$ y baja $Q^*$.

Cuando cambian las dos curvas, el efecto común es seguro y el otro puede quedar indeterminado. Por ejemplo, si suben oferta y demanda, $Q^*$ aumenta, pero el efecto sobre $P^*$ depende de la magnitud de ambos desplazamientos.

![](Attachments/Economia%20-%20Equilibrio%20y%20controles%20de%20precios.svg)

### Complementarios y sustitutos

- Si $X$ e $Y$ son sustitutos, una suba de $P_Y$ aumenta la demanda de $X$.
- Si son complementarios, una suba de $P_Y$ reduce la demanda de $X$.
- Un cambio en el costo de producir $X$ desplaza la oferta de $X$, no su demanda.

Para compensar un shock de oferta negativo:

- Mantener el precio inicial requiere una contracción de la demanda.
- Mantener la cantidad inicial requiere una expansión de la demanda.

En pasajes y valijas, las valijas son complementarias del viaje. Subir el precio de las valijas contrae la demanda de pasajes; bajarlo la expande.

## 3. Elasticidades

### Fórmula única

La elasticidad de $Q$ respecto de cualquier variable $X$ en un punto es:

$$
\varepsilon_{Q,X}=\frac{dQ}{dX}\frac{X}{Q}
$$

Si el cambio es discreto:

$$
\varepsilon_{Q,X}\approx\frac{\Delta Q/Q}{\Delta X/X}
$$

Para cambios grandes entre dos puntos conviene usar el método del punto medio:

$$
\varepsilon\approx
\frac{\Delta Q/\left(\frac{Q_1+Q_2}{2}\right)}
{\Delta X/\left(\frac{X_1+X_2}{2}\right)}
$$

### Elasticidad precio de la demanda

$$
\varepsilon_D=\frac{dQ_D}{dP}\frac{P}{Q_D}<0
$$

La cátedra suele clasificar con el valor absoluto $|\varepsilon_D|$:

| Valor | Tipo | Si baja el precio, el ingreso total $IT=P\cdot Q$... |
|---|---|---|
| $|\varepsilon_D|>1$ | Elástica | Aumenta |
| $|\varepsilon_D|=1$ | Unitaria | Es máximo local |
| $|\varepsilon_D|<1$ | Inelástica | Disminuye |

En una demanda lineal, la pendiente es constante pero la elasticidad cambia porque cambia $P/Q$.

![](Attachments/Economia%20-%20Elasticidad%20e%20ingreso%20total.svg)

### Elasticidad ingreso

$$
\varepsilon_Y=\frac{\partial Q}{\partial Y}\frac{Y}{Q}
$$

| Signo o valor | Tipo de bien |
|---|---|
| $\varepsilon_Y<0$ | Inferior |
| $0<\varepsilon_Y<1$ | Normal necesario o básico |
| $\varepsilon_Y>1$ | Normal de lujo |

### Elasticidad cruzada

$$
\varepsilon_{X,Y}=\frac{\partial Q_X}{\partial P_Y}\frac{P_Y}{Q_X}
$$

| Signo | Relación entre bienes |
|---|---|
| $\varepsilon_{X,Y}>0$ | Sustitutos |
| $\varepsilon_{X,Y}<0$ | Complementarios |
| $\varepsilon_{X,Y}=0$ | No relacionados localmente |

### Elasticidad de la oferta

$$
\varepsilon_S=\frac{dQ_S}{dP}\frac{P}{Q_S}>0
$$

La oferta suele ser más elástica a largo plazo porque la empresa puede variar todos los factores.

### Reconstruir una demanda lineal

Si $Q=a-bP$ y se conoce el punto $(Q_0,P_0)$ y la elasticidad con signo $\varepsilon_0$:

$$
\varepsilon_0=-b\frac{P_0}{Q_0}
\qquad\Rightarrow\qquad
b=-\varepsilon_0\frac{Q_0}{P_0}
$$

Después:

$$
a=Q_0+bP_0
$$

Chequeo: en una demanda decreciente debe resultar $b>0$.

## 4. Agregación horizontal

### Demanda de mercado

Para cada precio se suman cantidades, no precios:

$$
Q_D^{M}(P)=\sum_i \max\{Q_{D,i}(P),0\}
$$

El `max` importa. Si un consumidor tiene cantidad negativa a cierto precio, en realidad demanda cero. Por eso la demanda de mercado puede quedar definida por tramos.

Procedimiento:

1. Hallar el precio de reserva de cada consumidor, donde $Q_i=0$.
2. Ordenar esos precios.
3. En cada tramo, sumar sólo las demandas positivas.
4. Verificar continuidad en los puntos donde entra otro consumidor.

### Oferta de mercado

También se suman cantidades a cada precio:

$$
Q_S^{M}(P)=\sum_i Q_{S,i}(P)
$$

En competencia perfecta de corto plazo cada empresa ofrece sólo donde:

$$
P=CMg_i(q_i)
\quad\text{y}\quad
P\ge \min CVMe_i
$$

Si hay grupos de empresas con distintos costos, la oferta queda por tramos. En cada tramo se incluyen sólo los grupos cuyo precio cubre el mínimo del $CVMe$.

## 5. Controles de precios

### Precio máximo

Un techo $P_{max}$ es vinculante sólo si:

$$
P_{max}<P^*
$$

Si es vinculante:

$$
\text{escasez}=Q_D(P_{max})-Q_S(P_{max})
$$

La cantidad efectivamente transada no puede superar el lado corto:

$$
Q_{transada}=\min\{Q_D,Q_S\}=Q_S
$$

### Precio mínimo

Un piso $P_{min}$ es vinculante sólo si:

$$
P_{min}>P^*
$$

Entonces:

$$
\text{exceso de oferta}=Q_S(P_{min})-Q_D(P_{min})
$$

Error típico: aplicar el precio regulado sin comprobar antes si altera el equilibrio.

## 6. Excedentes, bienestar e ingreso

### Excedente del consumidor

En datos discretos:

$$
EC=\sum_{j\text{ comprado}}(P_j^{reserva}-P_{mercado})
$$

En una demanda continua:

$$
EC=\int_0^{Q^*}[P_D(Q)-P^*]dQ
$$

Con demanda lineal:

$$
EC=\frac{1}{2}(P_{intercepto}-P^*)Q^*
$$

Si se cobra una tarifa fija y el precio por unidad es cero, la tarifa máxima que acepta el consumidor es su excedente a precio cero, equivalente a su disposición total a pagar por las unidades consumidas.

### Excedente del productor

$$
EP=\int_0^{Q^*}[P^*-P_S(Q)]dQ
$$

En una oferta lineal es el área triangular sobre la oferta y debajo del precio. Para una empresa:

$$
EP=IT-CV
\qquad\text{y}\qquad
\pi=EP-CF
$$

### Bienestar y pérdida social

$$
BT=EC+EP
$$

La pérdida irrecuperable de eficiencia entre una cantidad distorsionada $Q_d$ y la competitiva $Q_c$ es:

$$
DWL=\int_{Q_d}^{Q_c}[P_D(Q)-CMg(Q)]dQ
$$

Si las curvas son lineales, suele ser un triángulo.

## 7. Impuestos, subsidios y aranceles

### Regla de la cuña

Conviene distinguir:

- $P_d$: precio pagado por el demandante.
- $P_o$: precio recibido por el oferente.

Con un impuesto unitario $t$:

$$
P_d-P_o=t
$$

Con un subsidio unitario $s$:

$$
P_o-P_d=s
$$

Se resuelve el sistema formado por demanda, oferta y cuña. La incidencia económica no depende de a quién le entregue o cobre el gobierno el monto en forma administrativa. Para el mismo subsidio por unidad, subsidiar al productor o al consumidor genera la misma cantidad de equilibrio. Cambia la forma de escribir la cuña.

Recaudación o costo fiscal:

$$
R=tQ
\qquad
G=sQ
$$

La parte menos elástica del mercado absorbe una mayor parte de la carga del impuesto o del beneficio del subsidio.

### Impuesto sobre la producción de una empresa

Un impuesto unitario se agrega al costo variable:

$$
CT_t(q)=CT(q)+tq
$$

Por lo tanto:

$$
CMg_t(q)=CMg(q)+t
$$

$$
CVMe_t(q)=CVMe(q)+t
\qquad
CTMe_t(q)=CTMe(q)+t
$$

Un impuesto fijo, en cambio, aumenta $CF$ y $CTMe$, pero no cambia $CMg$ ni $CVMe$.

### Comercio exterior con precio mundial

Si la oferta internacional es perfectamente elástica a $P_w$ y hay importaciones:

$$
P_d=P_o=P_w
$$

$$
M=Q_D(P_w)-Q_S(P_w)
$$

Sin importaciones, el país vuelve al equilibrio de autarquía:

$$
Q_D(P)=Q_S(P)
$$

Con un impuesto general al consumo $t$:

$$
P_d=P_o+t
$$

Si siguen entrando importaciones, el precio recibido por el oferente queda anclado en el precio mundial.

Con un arancel $a$ sólo a las importaciones y si todavía se importa:

$$
P_d=P_o=P_w+a
$$

Después se recalculan demanda nacional, oferta nacional e importaciones. Si el precio con arancel supera el precio de autarquía, las importaciones caen a cero y el precio relevante es el de autarquía.

Magnitudes pedidas con frecuencia:

$$
\text{Gasto demandantes}=P_dQ_D
$$

$$
\text{Ingreso productores nacionales}=P_oQ_S
$$

$$
\text{Recaudación arancelaria}=aM
$$

## 8. Producción de corto plazo

Con capital fijo $\bar K$ y trabajo variable $L$:

$$
Q=f(\bar K,L)
$$

### Producto total, medio y marginal

$$
PT=Q
$$

$$
PMe_L=\frac{Q}{L}
$$

$$
PMg_L=\frac{dQ}{dL}
\quad\text{o, en tabla,}\quad
PMg_L=\frac{\Delta Q}{\Delta L}
$$

Relaciones para completar tablas y gráficos:

- Si $PMg>PMe$, el $PMe$ sube.
- Si $PMg<PMe$, el $PMe$ baja.
- El $PMe$ alcanza su máximo cuando $PMg=PMe$.
- El producto total alcanza su máximo cuando $PMg=0$.
- Un productor racional opera en la etapa II, donde $PMg>0$ y ya actúan los rendimientos marginales decrecientes.

Para maximizar $PMe(L)=Q(L)/L$, se puede derivar e igualar a cero. El resultado interior satisface $PMg=PMe$.

## 9. Costos

### Identidades básicas

$$
CT=CF+CV
$$

$$
CFMe=\frac{CF}{Q}
\qquad
CVMe=\frac{CV}{Q}
\qquad
CTMe=\frac{CT}{Q}=CFMe+CVMe
$$

$$
CMg=\frac{dCT}{dQ}=\frac{dCV}{dQ}
$$

En una tabla discreta:

$$
CMg(Q)=\frac{\Delta CT}{\Delta Q}=\frac{\Delta CV}{\Delta Q}
$$

Si $\Delta Q=1$, el costo marginal es simplemente la diferencia entre costos totales consecutivos.

### Cómo completar una tabla

1. Copiar el mismo $CF$ en todas las filas.
2. Usar $CT=CF+CV$.
3. Dividir por $Q$ para obtener los costos medios.
4. Usar diferencias entre filas para obtener $CMg$.
5. Si falta un $CV$ pero se conoce $CVMe$, usar $CV=Q\cdot CVMe$.
6. Revisar que $CTMe=CFMe+CVMe$ en cada fila.

### Relación entre producto y costo marginal

Si el único factor variable es trabajo y su salario es $w$:

$$
CV=wL
$$

$$
CMg=\frac{w}{PMg_L}
$$

Cuando cae el producto marginal del trabajo, sube el costo marginal.

### Derivar costos desde una función de producción

Con $Q=f(\bar K,L)$, alquiler del capital $r$ y salario $w$:

1. Fijar $K=\bar K$.
2. Despejar la demanda condicionada de trabajo $L(Q)$.
3. Escribir:

$$
CT(Q)=r\bar K+wL(Q)
$$

4. Identificar $CF=r\bar K$ y $CV(Q)=wL(Q)$.
5. Calcular $CTMe=CT/Q$ y $CMg=dCT/dQ$.

En el mínimo interior del costo medio:

$$
CMg=CTMe
$$

## 10. Beneficio y condición marginal

$$
IT(q)=P(q)q
$$

$$
IM(q)=\frac{dIT}{dq}
$$

$$
\pi(q)=IT(q)-CT(q)
$$

La condición de primer orden es:

$$
\frac{d\pi}{dq}=IM-CMg=0
\qquad\Rightarrow\qquad
IM=CMg
$$

Interpretación alrededor del óptimo:

- Si $IM>CMg$, conviene aumentar $q$.
- Si $IM<CMg$, conviene reducir $q$.
- Para que sea un máximo, al cruzarse las curvas el $CMg$ debe quedar por encima del $IM$.

## 11. Competencia perfecta

La empresa es precio-aceptante:

$$
P=IM=IMe
$$

Por eso el nivel óptimo cumple:

$$
P=CMg(q^*)
$$

Se usa el tramo creciente del costo marginal.

### Decisión de producir o cerrar

| Condición en $q^*$ | Decisión de corto plazo |
|---|---|
| $P>CTMe$ | Produce con beneficio positivo |
| $CVMe\le P<CTMe$ | Produce con pérdida; cubre los costos variables y parte de los fijos |
| $P<CVMe$ | Cierra temporalmente |

La condición exacta de cierre compara el precio con el mínimo del costo variable medio:

$$
q(P)=
\begin{cases}
0, & P<\min CVMe\\
CMg^{-1}(P), & P\ge\min CVMe
\end{cases}
$$

Error típico: cerrar porque el precio está debajo del costo total medio. A corto plazo se produce con pérdida si todavía se cubren los costos variables.

![](Attachments/Economia%20-%20Costos%20y%20cierre%20competitivo.svg)

### Beneficio

$$
\pi=(P-CTMe(q^*))q^*
$$

### Equilibrio competitivo de largo plazo

Con libre entrada y salida, empresas idénticas e industria de costos constantes:

$$
P^{LP}=\min CMe_{LP}=CMg(q_e)
$$

$$
\pi^{LP}=0
$$

Procedimiento:

1. Minimizar el costo medio de la empresa para hallar $q_e$ y $P^{LP}$.
2. Evaluar la demanda de mercado a ese precio para hallar $Q^{LP}$.
3. Calcular el número de empresas:

$$
N=\frac{Q^{LP}}{q_e}
$$

Ante un aumento de demanda:

- Corto plazo: suben precio, producción y beneficio.
- Largo plazo: entran empresas, el precio vuelve al mínimo del costo medio, el beneficio vuelve a cero y aumenta la producción total.

## 12. Monopolio

El monopolista enfrenta la demanda de mercado. No tiene una curva de oferta independiente de la demanda.

### Demanda lineal

Si la demanda inversa es:

$$
P(Q)=a-bQ
$$

entonces:

$$
IT(Q)=aQ-bQ^2
$$

$$
IM(Q)=a-2bQ
$$

El $IM$ tiene el mismo intercepto vertical que la demanda y el doble de pendiente.

### Algoritmo del monopolista

1. Pasar la demanda a forma inversa $P(Q)$.
2. Calcular $IT=P(Q)Q$.
3. Derivar $IM=dIT/dQ$.
4. Calcular $CMg=dCT/dQ$.
5. Resolver $IM=CMg$ para hallar $Q_m$.
6. Reemplazar $Q_m$ en la demanda, no en el $IM$, para hallar $P_m$.
7. Calcular:

$$
\pi_m=P_mQ_m-CT(Q_m)
$$

En el óptimo interior con demanda decreciente:

$$
P_m>CMg
$$

El margen de Lerner relaciona poder de mercado y elasticidad:

$$
\frac{P-CMg}{P}=\frac{1}{|\varepsilon_D|}
$$

El monopolista maximizador de beneficios nunca elige el tramo inelástico de la demanda, porque allí podría subir el ingreso y reducir costos bajando la cantidad.

### Costo a partir de una función de producción

Si el capital está fijo en $\bar K$:

1. Despejar $L(q)$ desde $q=f(\bar K,L)$.
2. Escribir $CT(q)=wL(q)+r\bar K$.
3. Derivar $CMg(q)$.
4. Seguir el algoritmo $IM=CMg$.

La demanda de trabajo que maximiza beneficio es $L^*=L(q_m)$.

### Competencia frente a monopolio

- Competencia: $P_c=CMg(Q_c)$.
- Monopolio: $IM(Q_m)=CMg(Q_m)$.
- Normalmente $Q_m<Q_c$ y $P_m>P_c$.
- La pérdida social es el área entre demanda y costo marginal desde $Q_m$ hasta $Q_c$.

![](Attachments/Economia%20-%20Monopolio%20y%20pérdida%20social.svg)

### Regulación

Si se busca eficiencia asignativa:

$$
P=CMg
$$

En un monopolio natural puede ocurrir $CMe>CMg$, por lo que ese precio genera pérdidas. Si además se exige equilibrio económico sin subsidio, se usa:

$$
P=CMe
$$

Este es el precio más bajo compatible con beneficio económico nulo. El orden práctico es:

1. Resolver demanda $P(Q)$ igual a $CMe(Q)$.
2. Elegir la intersección económicamente válida.
3. Verificar que el ingreso total cubra el costo total.

### Impuesto unitario al monopolio

Agregar el impuesto al costo marginal:

$$
CMg_t=CMg+t
$$

Después se vuelve a resolver $IM=CMg_t$. El aumento del precio no tiene por qué ser igual al impuesto.

### Maximización de ventas

Si "ventas" significa ingreso total, el punto se obtiene con:

$$
IM=0
$$

En una demanda lineal corresponde a $|\varepsilon_D|=1$. Si el enunciado usa "ventas" como cantidad física, el máximo está en la mayor cantidad admisible, normalmente el intercepto donde $P=0$. Conviene aclarar la interpretación.

### Discriminación de precios de tercer grado

Cada segmento tiene su propia demanda e ingreso marginal. Si el costo depende de la producción total $Q=q_1+q_2$:

$$
IM_1(q_1)=IM_2(q_2)=CMg(q_1+q_2)
$$

Procedimiento:

1. Invertir cada demanda y obtener $IM_i$.
2. Igualar los ingresos marginales entre sí y con el costo marginal total.
3. Resolver $q_1$, $q_2$ y $Q$.
4. Volver a cada demanda para hallar $P_1$ y $P_2$.
5. Comparar el beneficio discriminando con el beneficio de precio único.

El segmento menos elástico paga el precio más alto.

## 13. Errores que cuestan puntos

- Usar la pendiente como si fuera la elasticidad. Falta multiplicar por $X/Q$.
- Sumar precios en vez de cantidades al construir una curva de mercado.
- Permitir cantidades negativas al agregar demandas u ofertas.
- Confundir movimiento sobre la curva con desplazamiento.
- Aplicar un precio máximo o mínimo sin revisar si es vinculante.
- Igualar demanda con $IM$ en monopolio. La cantidad sale de $IM=CMg$ y el precio sale de la demanda.
- Usar $P=CMg$ para un monopolio cuando el ejercicio exige equilibrio económico sin subsidio. En ese caso se busca $P=CMe$.
- Cerrar una empresa competitiva sólo porque tiene pérdidas. El cierre de corto plazo depende de $CVMe$.
- Agregar un impuesto unitario al costo fijo. Se agrega a $CV$, $CVMe$, $CTMe$ y $CMg$.
- Calcular áreas antes de ordenar bien los precios y cantidades relevantes.

## Fuentes trabajadas

- Clases 001 a 005 de Economía para Ingenieros.
- `GP1_ePI_24.pdf`.
- `GP2_ePI_2024.pdf`.
- `GP3_ePI_2026.pdf`.
- [Economia Intro](Economia%20Intro.md) y [Economia - Oferta, Demanda y Mercado](Economia%20-%20Oferta,%20Demanda%20y%20Mercado.md), que conservan la notación y el recorte de la cátedra dentro del vault.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Economia)**

- [Economia - Oferta, Demanda y Mercado](Economia%20-%20Oferta,%20Demanda%20y%20Mercado.md) - desarrolla oferta, demanda, equilibrio, controles de precios, excedente del consumidor y elasticidades usados en GP1 y GP2
- [Economia Intro](Economia%20Intro.md) - aporta costo de oportunidad, análisis marginal y el mapa general que conecta consumidor, productor y mercados
- [Materia - Economia](Materia%20-%20Economia.md) - nota índice de la materia y acceso al resto de los apuntes

<!-- notas-relacionadas:fin -->
