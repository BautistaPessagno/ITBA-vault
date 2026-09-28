---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-08-1316:07"
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
temas:
  - One Time Pad
  - Secreto perfecto
  - Seguridad computacional
  - Generadores pseudoaleatorios
  - Criptosistemas de flujo
  - Prueba de indistinguibilidad (EAV)
  - Múltiples cifrados (MUL)
  - Ataque de texto plano escogido (CPA)
  - Cifrado por bloques
  - Modos de encadenamiento (ECB, CBC, CFB, OFB, CTR)
  - Padding
  - DES y 3DES
  - AES
  - Función despreciable
---
floflfl# Cifrado

## Repaso: criptosistema

![imagen|480](Attachments/Pasted%20image%2020260813160940.png)

Un criptosistema es una terna de algoritmos:

- $Gen: () \rightarrow K$ genera una clave.
- $Enc: K \times P \rightarrow C$ cifra un mensaje.
- $Dec: K \times C \rightarrow P$ descifra un mensaje.

Debe cumplir la propiedad de corrección: para todo mensaje y clave válidos, $d_k(e_k(m))=m$.

La seguridad no debe depender de ocultar el algoritmo, sino de mantener secreta la clave. Esto permite publicar y analizar el diseño: un algoritmo que solo funciona mientras nadie conoce su implementación ofrece **seguridad por oscuridad**, no una garantía criptográfica.

## Secreto perfecto

![imagen|488](Attachments/Pasted%20image%2020260814141315.png)

Un criptosistema posee secreto perfecto si observar el texto cifrado no modifica la distribución de probabilidad del mensaje:

$$
Pr[M=m\mid C=c]=Pr[M=m].
$$

Por lo tanto, las variables aleatorias $M$ y $C$ son independientes: conocer $c$ no aporta información sobre $m$. Esto es más fuerte que decir que el mensaje es “difícil de recuperar”; incluso un adversario con recursos ilimitados no aprende nada nuevo.

## One Time Pad (OTP)

![](Attachments/Pasted%20image%2020260814141724.png)

El One Time Pad, atribuido a Vernam (1917), trabaja con una clave uniforme y verdaderamente aleatoria de la misma longitud que el mensaje:

$$
e_k(m)=m\oplus k
\qquad
d_k(c)=c\oplus k.
$$

El descifrado funciona porque $m\oplus k\oplus k=m$. Para cada par $(m,c)$ existe exactamente una clave compatible, $k=m\oplus c$; como todas las claves son equiprobables, el cifrado no favorece ningún mensaje y alcanza secreto perfecto.

La garantía exige simultáneamente que la clave:

- sea realmente aleatoria;
- tenga la misma longitud que el mensaje;
- permanezca secreta;
- se utilice **una sola vez**.

Si se reutiliza la clave,

$$
c_1\oplus c_2=(m_1\oplus k)\oplus(m_2\oplus k)=m_1\oplus m_2,
$$

y la clave desaparece de la ecuación. Aunque el atacante todavía no vea directamente los mensajes, obtiene una relación entre ambos que suele ser explotable por su estructura o redundancia.

Una clave sesgada tampoco sirve: si algunos valores de $k$ son más probables, observar $c$ cambia las probabilidades a posteriori de los mensajes. La aleatoriedad de la clave no es un detalle de implementación, sino parte de la demostración.

## Más allá del OTP

![](Attachments/Pasted%20image%2020260814142902.png)

Según el resultado visto en clase, cualquier criptosistema con secreto perfecto es reducible al OTP. Además, el secreto perfecto requiere un espacio de claves al menos tan grande como el espacio de mensajes cifrables.

El costo aparece en la distribución y el almacenamiento de claves: para proteger $n$ bits hay que compartir previamente $n$ bits secretos y luego descartarlos. Por eso el OTP es una referencia teórica muy útil, pero resulta impráctico para la mayoría de los sistemas.

## Seguridad computacional

El secreto perfecto brinda **seguridad incondicional**. La seguridad computacional relaja esa meta de dos maneras:

- limita los recursos del adversario, sobre todo su tiempo de cómputo;
- acepta una probabilidad de éxito pequeña, en lugar de exigir que sea exactamente cero.

![imagen|392](Attachments/Pasted%20image%2020260814143612.png)

La pregunta deja de ser “¿existe algún ataque?” y pasa a ser “¿existe un ataque factible dentro del modelo considerado?”. Toda afirmación de seguridad computacional depende entonces de tres elementos: qué puede hacer el adversario, cuánto puede computar y qué ventaja se considera tolerable.

## Criptosistemas de flujo

![](Attachments/Pasted%20image%2020260814143644.png)

Un criptosistema de flujo reemplaza la clave larga del OTP por la salida de un generador pseudoaleatorio:

$$
e_k(m)=G(k)\oplus m
\qquad
d_k(c)=G(k)\oplus c.
$$

Así, una semilla corta produce una secuencia del largo necesario y $|K|\ll|M|$. La construcción imita al OTP, pero pierde el secreto perfecto porque la secuencia proviene de un conjunto mucho menor de posibilidades. Su objetivo pasa a ser que ningún adversario eficiente pueda distinguirla de una secuencia realmente aleatoria.

### Generadores pseudoaleatorios

Un generador pseudoaleatorio (PRG) es un algoritmo determinístico que expande una semilla corta $s$ en una salida más larga $G(s)$. La salida no es aleatoria en sentido estricto: una misma semilla siempre produce la misma secuencia.

![imagen|600](Attachments/Pasted%20image%2020260814144020.png)
![imagen|595](Attachments/Pasted%20image%2020260814144200.png)

La propiedad relevante es la **indistinguibilidad computacional**. Para todo distinguidor eficiente $D$, la diferencia

$$
\left|Pr[D(G(U_s))=1]-Pr[D(U_n)=1]\right|
$$

debe ser despreciable, donde $U_s$ y $U_n$ representan elecciones uniformes de $s$ y $n$ bits. Que una secuencia “se vea desordenada” o pase algunas pruebas estadísticas no alcanza: debe resistir a toda familia de ataques eficientes contemplada por el modelo.

El generador congruencial mostrado como ejemplo permite entender la expansión y el período, pero no debe confundirse con un PRG criptográficamente seguro: una recurrencia simple puede ser predecible aunque su salida parezca variada.

## Pruebas de seguridad

![imagen|547](Attachments/Pasted%20image%2020260814144520.png)

Una prueba de seguridad formaliza un juego entre un retador y un adversario. El juego fija qué información recibe el adversario, qué consultas puede realizar y en qué condición gana. Repetirlo permite medir su probabilidad de éxito.

Cada prueba modela un escenario distinto. Superar una prueba no significa ser “seguro para todo”, sino ser seguro frente a las capacidades específicas que esa prueba concede.

### Prueba de indistinguibilidad EAV

![](Attachments/Pasted%20image%2020260814144739.png)

En la prueba de escucha pasiva (EAV):

1. El adversario elige dos mensajes $m_0$ y $m_1$ de igual longitud.
2. El retador genera una clave $k$ y un bit uniforme $b$.
3. El adversario recibe $c=e_k(m_b)$.
4. Emite una conjetura $b'$.

Si no puede hacer algo mejor que adivinar, su probabilidad de éxito es $1/2$ más una ventaja despreciable. Exigir igual longitud evita que el tamaño del cifrado revele trivialmente cuál de los dos mensajes fue elegido.

### Nivel de seguridad y función despreciable

![](Attachments/Pasted%20image%2020260814145229.png)

El parámetro de seguridad $n$ relaciona el poder permitido al adversario con su ventaja. Se consideran adversarios de tiempo probabilístico polinomial, $PPT(n)$, y se exige que su ventaja $\varepsilon(n)$ sea despreciable.

Formalmente, $\varepsilon$ es despreciable si, para todo $d>0$, existe $n_0$ tal que para todo $n>n_0$:

$$
\varepsilon(n)<\frac{1}{n^d}.
$$

No significa simplemente “un número chico”: una constante como $10^{-20}$ sigue siendo constante y, asintóticamente, no es despreciable. La propiedad describe cómo cae la ventaja al aumentar el parámetro de seguridad.

### Teorema para criptosistemas de flujo

![](Attachments/Pasted%20image%2020260814145622.png)

Si $G$ es un generador pseudoaleatorio seguro, el criptosistema de flujo construido como $G(k)\oplus m$ es indistinguible ante escucha pasiva para **un solo cifrado**.

La demostración es por reducción: si un adversario distinguiera los cifrados, se lo podría usar para distinguir la salida de $G$ de una cadena uniforme. Esto contradice la hipótesis de seguridad del generador.

## Múltiples cifrados

![](Attachments/Pasted%20image%2020260814145653.png)

La prueba MUL extiende EAV a dos secuencias de mensajes. El retador cifra todos los mensajes de una de ellas con la misma clave y el adversario intenta identificar cuál fue elegida.

Los criptosistemas de flujo anteriores **no son seguros** bajo múltiples cifrados si reutilizan exactamente $G(k)$:

![](Attachments/Pasted%20image%2020260814145802.png)

$$
c_1\oplus c_2=m_1\oplus m_2.
$$

Es el mismo fenómeno que reutilizar la clave de un OTP. La seguridad para un único desafío no se transfiere automáticamente al caso de varios mensajes.

### Necesidad de cifrado probabilístico

Un cifrado determinístico siempre produce el mismo resultado para el mismo par $(k,m)$. El adversario puede reconocer repeticiones y construir desafíos que se distinguen comparando cifrados; por eso no puede alcanzar seguridad CPA ni seguridad frente a múltiples cifrados en el modelo general.

La aleatorización no necesita ocultarse. Su función es lograr que dos cifrados del mismo mensaje con la misma clave sean diferentes sin impedir el descifrado.

### Evitar la reutilización

![](Attachments/Pasted%20image%2020260814150408.png)

Se incorpora a la generación de la secuencia un valor que no se repite para una misma clave. La clase presenta dos alternativas:

- **Modo sincronizado:** un IV inicial y evolución coordinada del estado.
- **Modo no sincronizado:** un IV o nonce por mensaje.

Aunque en explicaciones introductorias se usan casi como sinónimos, la condición exacta depende del modo: algunos requieren un IV impredecible y aleatorio; otros, como CTR, requieren principalmente que el nonce sea único para cada clave. Repetir el par $(k,nonce)$ repite la secuencia de clave y recrea el problema del OTP reutilizado.

## Ataque de texto plano escogido

![imagen|625](Attachments/Pasted%20image%2020260814150713.png)

En la prueba CPA el adversario tiene acceso a un oráculo de cifrado $e_k(\cdot)$ antes —y, en la formulación habitual, también después— de recibir el desafío. Puede elegir entradas y observar sus cifrados, pero no consultar directamente el mensaje desafío de una forma que trivialice el juego.

Este modelo es más fuerte y realista que EAV: en protocolos y aplicaciones, un atacante puede provocar que el sistema cifre datos parcialmente controlados por él.

### Propiedades CPA

- Un criptosistema determinístico no puede ser CPA-Secure.
- La seguridad CPA para un mensaje implica seguridad CPA para múltiples mensajes mediante un argumento híbrido.
- Una construcción segura para bloques de tamaño limitado puede extenderse a mensajes mayores si cada bloque usa correctamente la aleatoriedad o el encadenamiento requerido.

![](Attachments/Pasted%20image%2020260814151003.png)

La confidencialidad CPA no brinda integridad. Un modo puede impedir que el atacante conozca el mensaje y aun así permitirle modificar el cifrado de manera controlada. En un sistema real suele requerirse cifrado autenticado.

## Primitivas de cifrado en bloque

![](Attachments/Pasted%20image%2020260814151714.png)

Una primitiva de bloque transforma bloques de tamaño fijo mediante una clave:

$$
E_k:\{0,1\}^b\rightarrow\{0,1\}^b.
$$

Para cada clave, $E_k$ debe ser una permutación para que exista $D_k$. Se busca que se comporte como una permutación pseudoaleatoria: sin conocer $k$, distinguirla de una permutación elegida al azar debe ser computacionalmente inviable.

La primitiva por sí sola es determinística y no constituye un esquema seguro para mensajes generales. Necesita un modo de operación que resuelva la división en bloques, la aleatorización y el encadenamiento.

## Extensión, padding y encadenamiento

![](Attachments/Pasted%20image%2020260814152001.png)

Si el mensaje supera el tamaño de bloque, se divide en $m_0\|m_1\|\dots\|m_i$. Si el último bloque queda incompleto, se aplica **padding**:

- **Simple pad:** completa con ceros, pero exige conocer por otra vía la longitud original porque los ceros finales serían ambiguos.
- **Bit padding (presentado como DES pad):** agrega un bit `1` seguido de ceros. Si el mensaje ya ocupa bloques completos, agrega un bloque entero para que el padding siga siendo reconocible.

El padding debe validarse con cuidado. En protocolos mal diseñados, informar si el padding es válido puede convertirse en un *padding oracle* y filtrar información sobre el texto plano.

### ECB

![](Attachments/Pasted%20image%2020260814152113.png)

ECB cifra cada bloque de forma independiente: $c_i=E_k(m_i)$.

Bloques iguales producen cifrados iguales, por lo que conserva patrones y no es CPA-Secure. La invertibilidad de $E_k$ garantiza que se pueda descifrar, pero **corrección no implica seguridad**.

> [!warning] No utilizar ECB para cifrar mensajes estructurados
> Puede ocultar los valores concretos y, al mismo tiempo, revelar repeticiones, posiciones y forma general del contenido.

### CBC

![imagen|640](Attachments/Clase%202%20-%20Encadenamiento%20CBC.png)

CBC combina cada bloque plano con el cifrado anterior antes de aplicar la primitiva:

$$
c_0=E_k(m_0\oplus IV),
\qquad
c_i=E_k(m_i\oplus c_{i-1}).
$$

Con una primitiva segura y un IV aleatorio e impredecible, CBC alcanza seguridad CPA. El cifrado es secuencial porque cada bloque depende del anterior. Un error en un bloque cifrado destruye el bloque plano correspondiente y altera bits puntuales del siguiente.

### CFB

![](Attachments/Pasted%20image%2020260814152541.png)

CFB usa únicamente la operación de cifrado de la primitiva y convierte el cifrador de bloque en uno de flujo. Realimenta el cifrado previo, por lo que puede trabajar en unidades menores que un bloque y autosincronizarse.

Un error de transmisión afecta la porción correspondiente del texto plano y también una cantidad limitada de datos posteriores mientras el valor errado permanezca en el registro de realimentación; no se propaga indefinidamente.

### OFB

![](Attachments/Pasted%20image%2020260814152749.png)

OFB realimenta la **salida interna** de la primitiva para generar una secuencia de clave independiente del mensaje y del cifrado. Esa secuencia puede precalcularse.

A diferencia de lo que sugería el apunte original, un cambio de bit en el texto cifrado altera solamente el bit correspondiente del texto plano: los errores de bits **no se propagan entre bloques**. Sin embargo, una inserción o pérdida de bits rompe la sincronización, y reutilizar el IV con la misma clave reutiliza la secuencia.

### Counter (CTR)

![](Attachments/Pasted%20image%2020260814152814.png)

CTR cifra valores formados por un nonce y un contador para producir la secuencia de clave:

$$
c_i=m_i\oplus E_k(nonce\|counter_i).
$$

Permite paralelizar, precalcular, acceder a un bloque sin procesar los anteriores y limita un error de bit a la misma posición del texto plano. Su condición crítica es no repetir nunca el mismo par $(k,nonce)$.

En OFB, CFB y CTR, modificar un bit del cifrado puede modificar el texto plano de manera predecible. Por eso estos modos aportan confidencialidad, pero no autenticidad por sí solos.

### Seguridad de los modos de bloque

![imagen|532](Attachments/Pasted%20image%2020260814153402.png)
![imagen|535](Attachments/Pasted%20image%2020260814153430.png)

Las pruebas se hacen por reducción a la seguridad de la primitiva subyacente. Si se comporta como una función o permutación pseudoaleatoria, entonces, bajo las condiciones indicadas en clase:

- CBC, CFB y OFB son CPA-Secure con IV adecuados y aleatorios;
- CTR es CPA-Secure si nunca se repite $(k,nonce)$;
- ECB no es CPA-Secure.

La conclusión siempre incluye sus hipótesis: un modo demostrado seguro deja de estar cubierto por la prueba si se repite el nonce, se genera mal el IV o se filtran errores de padding.

# DES — Data Encryption Standard

DES fue desarrollado por IBM y adoptado como estándar por el gobierno de Estados Unidos. Trabaja con bloques de 64 bits y claves de 64 bits, de los cuales solo 56 son efectivos; los otros 8 se destinaban a paridad.

Es una red de Feistel de 16 rondas. En cada ronda se divide el estado en dos mitades, se transforma una de ellas con una subclave y se intercambian las mitades. La estructura de Feistel permite descifrar aplicando la misma arquitectura con las subclaves en orden inverso.

![imagen|398](Attachments/Pasted%20image%2020260821180927.png)

La función de ronda expande 32 bits a 48, aplica cajas de sustitución de 6 a 4 bits y luego una permutación. Las sustituciones aportan no linealidad; las permutaciones y expansiones difunden la influencia de cada bit.

> [!question]- Pregunta de examen: ¿Base64 es un algoritmo de cifrado?
> No. Base64 es una **codificación** reversible y no utiliza clave. Cualquiera que conozca el formato puede recuperar los bytes originales.

> [!question]- ¿Qué significa ofuscar código?
> Significa transformar o presentar el código para dificultar su lectura y la ingeniería inversa. Puede aumentar el esfuerzo de análisis, pero no reemplaza al cifrado ni convierte en secreto un algoritmo embebido en el programa.

## Generación de subclaves

![](Attachments/Pasted%20image%2020260821183813.png)

La permutación PC-1 descarta los 8 bits de paridad y divide los 56 restantes en dos mitades. Estas se rotan en cada ronda y PC-2 selecciona 48 bits para formar cada subclave.

La clave efectiva de 56 bits hace que DES sea vulnerable a fuerza bruta. Los ataques diferencial y lineal también reducen su margen teórico, aunque parte del diseño de sus S-boxes resultó estar preparado para resistir mejor el criptoanálisis diferencial.

## 3DES

![](Attachments/Pasted%20image%2020260821184605.png)

3DES aplica DES tres veces con el esquema cifrar–descifrar–cifrar (EDE):

$$
c=E_{k_1}(D_{k_2}(E_{k_3}(m))).
$$

El paso central de descifrado conserva compatibilidad con DES cuando las claves coinciden. Aunque podría sugerir 168 bits de clave, los ataques *meet-in-the-middle* reducen su seguridad efectiva a aproximadamente 112 bits. También triplica aproximadamente el costo de DES y mantiene el pequeño tamaño de bloque de 64 bits.

DES y 3DES deben entenderse hoy como algoritmos históricos o de compatibilidad, no como opciones para proyectos nuevos. NIST considera deshabilitado 3DES/TDEA para aplicar nueva protección desde 2024, mientras mantiene AES como aceptable ([NIST SP 800-131A Rev. 2](https://csrc.nist.gov/pubs/sp/800/131/a/r2/final)).

# AES — Advanced Encryption Standard

AES reemplazó a DES tras un concurso internacional abierto. Opera sobre bloques de 128 bits y admite claves de 128, 192 o 256 bits. La longitud de clave determina además 10, 12 o 14 rondas, respectivamente.

Representa el bloque como una matriz de bytes llamada **estado** y aplica cuatro transformaciones:

1. **SubBytes:** sustitución no lineal de cada byte mediante una S-box construida sobre $GF(2^8)$.
2. **ShiftRows:** desplazamiento cíclico de los **bytes** de cada fila. La diapositiva lo resume como permutación de bits, pero describirlo por bytes es más preciso.
3. **MixColumns:** transformación lineal invertible que mezcla los cuatro bytes de cada columna.
4. **AddRoundKey:** XOR del estado con la subclave de la ronda.

![imagen|640](Attachments/Clase%202%20-%20Esquema%20AES.png)

Antes de las rondas normales hay un AddRoundKey inicial; la última ronda omite MixColumns. Es más preciso hablar de una **etapa inicial** que decir que la primera ronda “solo” contiene AddRoundKey.

SubBytes aporta confusión y ShiftRows junto con MixColumns aporta difusión. AddRoundKey es la única etapa que incorpora el secreto; las demás transformaciones son públicas e invertibles.

## Expansión de clave

La clave original se expande en una subclave por ronda. Para AES-128, los primeros 16 bytes son la clave original; los siguientes se derivan rotando palabras, aplicando SubBytes, incorporando una constante de ronda y combinando mediante XOR con palabras anteriores.

La expansión no agrega entropía: todas las subclaves dependen de la clave original. Su objetivo es distribuir su influencia y evitar que las rondas utilicen exactamente el mismo material.

# Criptosistemas en proyectos

Se considera mala práctica diseñar un criptosistema nuevo para un proyecto. Los algoritmos aceptados fueron analizados durante años y aun así pueden aparecer ataques que obliguen a cambiar parámetros o retirarlos. En una implementación conviene usar bibliotecas mantenidas y construcciones completas, no ensamblar primitivas manualmente.

## Estado de un criptosistema

- **Seguro:** cumple las expectativas de un modelo de seguridad definido.
- **Debilitado:** existe un ataque con ventaja no despreciable, pero requiere un esfuerzo enorme o condiciones difíciles de reunir.
- **Quebrado:** existe un ataque con ventaja no despreciable y costo practicable para el escenario considerado.

Estas categorías no son absolutas. Un criptosistema puede estar quebrado bajo una prueba o uso particular y seguir satisfaciendo otro modelo. También puede ser teóricamente seguro y fallar en la práctica por generación deficiente de claves, reutilización de nonces, filtraciones laterales o errores del protocolo.

## Algoritmos mencionados en la clase

Las diapositivas enumeran como generadores o cifradores de flujo a RC4, CSS, A5/1, A5/2, E0, Salsa20 y Rabbit, y como primitivas de bloque a DES, IDEA, 3DES y AES. La lista sirve para ubicar algoritmos y aplicaciones históricas; no todos conservan hoy el estado de “recomendados”.

En particular, RC4 está prohibido en TLS por sus sesgos y ataques prácticos ([RFC 7465](https://www.rfc-editor.org/rfc/rfc7465.html)); DES tiene una clave demasiado corta y 3DES fue retirado para nueva protección. Dentro del alcance de esta clase, **AES con una construcción moderna y autenticada** es el punto de partida razonable para sistemas nuevos.

## Tamaños y margen de seguridad

- El bloque de AES tiene siempre 128 bits; una clave AES-256 no implica bloques de 256 bits.
- La longitud de clave determina el costo de fuerza bruta, mientras que el tamaño de bloque condiciona fenómenos como colisiones por cumpleaños al procesar grandes volúmenes.
- Una clave grande no compensa un modo mal usado: AES-256 con nonce repetido en CTR puede fallar de forma inmediata.
- El cifrado protege confidencialidad, no disponibilidad, identidad ni integridad salvo que la construcción lo incluya explícitamente.

La elección práctica no termina en “AES”: también hay que definir modo, manejo de nonces, autenticación, derivación y rotación de claves, almacenamiento y tratamiento de errores.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Criptografía y Seguridad)**

- [Criptografia y seguridad intro](Criptografia%20y%20seguridad%20intro.md) — clase anterior: define criptosistema y secreto perfecto, que acá se retoman como repaso
- [Materia - Criptografía y Seguridad](Materia%20-%20Criptografía%20y%20Seguridad.md) — índice de la materia
- [Guia 1 - criptografia y seguridad](Guia%201%20-%20criptografia%20y%20seguridad.md) — ejercicios sobre cifrados clásicos, previos al OTP
- [Practica 1 - criptografia y seguridad](Practica%201%20-%20criptografia%20y%20seguridad.md) — práctica asociada

**Otras materias**

- **Protos** — [9. Protos - SSH](9.%20Protos%20-%20SSH.md) — SSH negocia justamente estas primitivas (AES-CTR, AES-CBC) y usa el intercambio de claves para acordar la `k`
- **Protos** — [2. Protos - HTTP](2.%20Protos%20-%20HTTP.md) — HTTPS/TLS es el caso de uso masivo de los criptosistemas de flujo y bloque vistos acá (y donde RC4 quedó deprecado)
- **TLA** — [TLA -Autómatas Finitos Determinísticos](TLA%20-Autómatas%20Finitos%20Determinísticos.md) — el LFSR de §6.1 es literalmente un autómata finito determinístico: estado = contenido del registro, y su **período** es el largo del ciclo en el grafo de transiciones

<!-- notas-relacionadas:fin -->
