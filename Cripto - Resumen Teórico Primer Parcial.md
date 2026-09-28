---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
Cuatri: "2C"
Created: 2026-09-20
temas:
  - primer parcial
  - repaso teórico
  - criptosistema
  - principio de Kerckhoffs
  - cifrados clásicos
  - índice de coincidencia
  - confusión y difusión
  - secreto perfecto
  - one-time pad
  - teorema de Shannon
  - seguridad computacional
  - función despreciable
  - reducción
  - generador pseudoaleatorio
  - función pseudoaleatoria
  - experimento EAV
  - experimento MUL
  - experimento CPA
  - experimento CCA
  - modos de encadenamiento
  - red de Feistel
  - DES
  - AES
  - maleabilidad
  - MAC
  - Mac-forge
  - CBC-MAC
  - funciones de hash
  - paradoja del cumpleaños
  - Merkle-Damgård
  - HMAC
  - Encrypt-then-MAC
  - cifrado autenticado
  - AEAD
  - distribución de claves
  - KDC
  - grupos cíclicos
  - teorema de Euler
  - Diffie-Hellman
  - logaritmo discreto
  - DDH
  - RSA
  - PKCS 1
  - ElGamal
  - firma digital
  - Sig-forge
  - no repudio
  - DSA
  - PKI
  - certificados X.509
  - revocación
  - Needham-Schroeder
  - Denning-Sacco
  - nonce
  - timestamp
  - TLS
  - forward secrecy
  - Shamir
  - secret sharing
---
# Cripto - Resumen Teórico Primer Parcial

> [!abstract] Qué es esto
> La **teoría** que entra en el primer parcial (jueves 24/09: teóricas 1 a 5, Guías 1 a 4), condensada y ordenada por clase: definiciones formales, experimentos de seguridad, teoremas con la idea de la prueba y tablas comparativas.
>
> Es el complemento de [Cripto - Resumen Primer Parcial](Cripto%20-%20Resumen%20Primer%20Parcial.md): aquella tiene las **recetas** para los ejercicios que se repiten; esta tiene el **por qué**. El desarrollo completo sigue en cada nota de clase, linkeada al principio de cada sección.
>
> Armada desde las notas de las Clases 1 a 5, los PDFs de las teóricas y las Guías 1 a 4 con sus soluciones. Los errores de las slides ya verificados en las notas de clase van como `[!bug]`, y las cuentas nuevas de esta nota se verificaron con Python.

> [!tip] Cómo usarla
> - **§0** es el mapa: qué resuelve cada clase y qué supuesto le deja a la siguiente.
> - **§1 a §5** siguen las clases.
> - **§6** son las tablas transversales (servicios, experimentos, supuestos, qué no se repite, implicaciones). Ahí se juega el V/F.
> - **§7** es para autoevaluarse: preguntas teóricas con la respuesta plegada.
> - **§8** junta los errores de las slides que pueden aparecer como pregunta.

---

## 0. El hilo de la materia

Cada clase resuelve un problema y deja abierto un supuesto, que es lo que ataca la siguiente:

| Clase | Pregunta | Respuesta | Supuesto que queda abierto |
|---|---|---|---|
| **1** | ¿Qué es un criptosistema y qué es "seguro"? | Terna $Gen/Enc/Dec$ + Kerckhoffs. Los clásicos caen porque filtran la estadística del idioma | "seguro" no tiene definición formal |
| **2** | ¿Se puede ser seguro de verdad? | Sí: secreto perfecto (OTP), pero con clave tan larga como el mensaje. Se relaja a **seguridad computacional**: PRG → flujo, PRF → bloque + modos. EAV → CPA | CPA no dice nada de un atacante que **modifica** el cifrado |
| **3** | ¿Y la integridad? | Maleabilidad → CCA. MAC (Mac-forge), CBC-MAC, hash, HMAC. Encrypt-then-MAC ⇒ CCA | las partes **ya comparten** la clave |
| **4** | ¿Cómo se consigue la clave? | KDC (centralizado) o Diffie-Hellman. Asimétrico: RSA, ElGamal. Firma: integridad **públicamente verificable** + no repudio | el atacante es **pasivo** y la clave pública que recibo es la **auténtica** |
| **5** | ¿Y contra un atacante activo? | PKI (certificados), Needham-Schroeder (nonce, timestamp, token, claves cortas), TLS = canal seguro. Shamir: un secreto repartido en $n$ partes | (segunda mitad de la materia) |

> [!important] El molde que se repite
> Toda definición de seguridad de la materia es un **juego** entre un retador y un adversario $A$ de recursos acotados: el retador genera la clave, $A$ recibe lo que el modelo le concede (un cifrado, oráculos, la clave pública, el transcript), produce algo, y se define cuándo gana. Hay dos familias:
> - **Indistinguibilidad** (EAV, MUL, CPA, CCA, KE): adivinar un bit $b$. Seguro si $\Pr[\text{gana}] \le \tfrac12 + negl(n)$.
> - **Falsificación** (Mac-forge, Sig-forge, Hash-coll): producir un objeto válido nuevo. Seguro si $\Pr[\text{gana}] \le negl(n)$.
>
> La tabla con todos está en §6.2. Saber **qué recibe** el adversario en cada uno es la mitad de cualquier pregunta teórica.

---

## 1. Clase 1 — Fundamentos y criptografía clásica

Detalle: [Criptografia y seguridad intro](Criptografia%20y%20seguridad%20intro.md) · ejercicios: [Guia 1 - criptografia y seguridad](Guia%201%20-%20criptografia%20y%20seguridad.md)

### 1.1 Criptosistema

Es una **terna de algoritmos**:

$$Gen: () \rightarrow K \qquad Enc: K \times P \rightarrow C \qquad Dec: K \times C \rightarrow P$$

con la **propiedad de corrección** $d_k(e_k(m)) = m$ para todo $m$ y $k$ válidos. $K$, $P$ y $C$ son los espacios de claves, de planos y de cifrados.

- **Consecuencia de la corrección**: para cada $k$, $e_k$ es **inyectiva** (si dos planos dieran el mismo cifrado no se podría descifrar), y entonces $\lvert C\rvert \ge \lvert P\rvert$. Es el V/F del 1C-2025.
- **$Gen$ es parte del diseño**: cómo se eligen las claves es parte de la seguridad (una clave sesgada rompe el OTP, §2.2).
- **Principio de Kerckhoffs**: el criptosistema tiene que ser seguro aunque **todo** sea público salvo la clave. Por eso *security through obscurity* no vale, **Base64 no es cifrado** (no tiene clave: es una codificación) y ofuscar código no reemplaza al cifrado.
- **Seguridad, versión informal**: ningún adversario puede calcular **ninguna función** del plano a partir del cifrado: ni el mensaje, ni una parte, ni el sentido. La versión formal es el secreto perfecto (§2.1) y después los experimentos de §2.6.
- **Criptoanálisis**: las técnicas para probar o romper la seguridad de un criptosistema.

**Modelos de ataque** (Katz, cap. 1): qué tiene el atacante, y con qué experimento de la materia se formaliza.

| El atacante tiene… | Nombre | Experimento |
|---|---|---|
| solo cifrados | *ciphertext-only* | EAV (MUL si son varios con la misma clave) |
| pares (plano, cifrado) que no eligió | *known-plaintext* | lo cubre CPA, que es más fuerte |
| cifrados de planos que **elige** | *chosen-plaintext* | CPA |
| además, descifrados de cifrados que **elige** | *chosen-ciphertext* | CCA |

### 1.2 Cifrados clásicos

Definición formal (Guía 1, ej. 1), sobre un alfabeto $\Sigma$ de $q$ símbolos numerados desde 0 ($q = 26$ en inglés, 27 en castellano con ñ) y $m = m_1\dots m_\ell$:

| Esquema | $Gen$ | $Enc$ | $Dec$ | Claves | Por qué cae |
|---|---|---|---|---|---|
| **Rotación** (César, ROT-X) | $k \leftarrow \{0,\dots,q-1\}$ | $c_i = m_i + k \bmod q$ | $m_i = c_i - k \bmod q$ | $q$ | espacio de claves chico: fuerza bruta |
| **Sustitución** monoalfabética | $\pi \leftarrow$ permutación de $\Sigma$ | $c_i = \pi(m_i)$ | $m_i = \pi^{-1}(c_i)$ | $q!$ ($26! \approx 4\cdot10^{26}$) | **preserva la estadística** del idioma: análisis de frecuencias |
| **Vigenère** (período $t$) | $k = k_0\dots k_{t-1} \leftarrow \Sigma^t$ | $c_i = m_i + k_{i \bmod t} \bmod q$ | $m_i = c_i - k_{i \bmod t} \bmod q$ | $q^t$ | **repite la clave**: Kasiski + IC; cada columna es un César |
| **Transposición** (bloques de $b$) | $\sigma \leftarrow$ permutación de $\{1,\dots,b\}$ | permuta las posiciones del bloque | $\sigma^{-1}$ | $b!$ | **no cambia las letras**: el histograma es el del idioma |

- **Polialfabético** = un mismo símbolo del plano se cifra distinto según la posición (Vigenère, Enigma). Enigma lleva la idea al extremo: rotores que avanzan con cada tecla (período enorme) + plugboard; cayó con la *Bombe* (exploración sistemática por fuerza bruta).
- **Homofónico** (1C-2018): cada letra frecuente se reparte en varios símbolos, en proporción a su frecuencia → **aplana** el histograma y el IC deja de servir tanto. No es Vigenère: el símbolo se elige al azar, no por posición.

> [!important] Tres lecciones de la Clase 1
> 1. **Espacio de claves grande ≠ seguro.** La sustitución tiene $26!$ claves y cae con frecuencias. Un espacio chico sí implica inseguridad: es condición **necesaria**, no suficiente.
> 2. **La fuerza bruta necesita distinguir un descifrado válido de uno inválido.** ROT-X sobre un mensaje de una letra: los 26 candidatos son todos válidos, no hay nada que elegir. Es la semilla del secreto perfecto (y el motivo de que un mensaje corto sea más difícil de romper que uno largo).
> 3. **Componer esquemas del mismo tipo no suma seguridad**: sustitución∘sustitución es otra sustitución; Vigenère de período $p$ ∘ Vigenère de período $r$ es un Vigenère de período $\operatorname{mcm}(p,r)$.

**Kasiski e índice de coincidencia**

- **Kasiski**: un n-grama se repite en el cifrado cuando el mismo n-grama del plano cae en la misma posición relativa de la clave. Entonces el largo $t$ **divide** a las distancias entre repeticiones, y por lo tanto a su **MCD** (salvo repeticiones casuales, por eso da un múltiplo/divisor y no el largo exacto).
- **Índice de coincidencia**: la probabilidad de que dos letras tomadas al azar del texto sean iguales,
$$IC = \frac{\sum_i F_i(F_i-1)}{N(N-1)} \;\approx\; \sum_i p_i^2 .$$
  Texto en castellano: $\approx 0{,}072$ con la tabla de frecuencias de la Clase 1 (otras tablas dan hasta $\approx 0{,}077$). Texto uniforme: $1/q$ ($0{,}038$ con 26 letras, $0{,}037$ con 27).
- **Por qué sirve para clasificar**: la sustitución **permuta las letras** y la transposición **permuta las posiciones**; ninguna de las dos cambia el multiconjunto de frecuencias, así que **no cambian el IC**. Vigenère mezcla varios alfabetos y lo baja hacia $1/q$ cuanto más largo es el período.

### 1.3 Confusión, difusión y linealidad (Shannon)

Salió como pregunta teórica en el 1C-2023 (a propósito de Base64):

| Concepto | Qué pide | En la práctica |
|---|---|---|
| **Confusión** | que la relación entre la **clave** y el cifrado sea compleja | etapas **no lineales**: S-boxes de DES, SubBytes de AES |
| **Difusión** | que cada bit del **plano** influya en muchos bits del cifrado, dispersando la estadística | permutaciones y mezclas: expansión y P de DES, ShiftRows + MixColumns de AES; **efecto avalancha** |
| **Linealidad** | el cifrado es combinación lineal o afín del plano y la clave | César, afín, Vigenère, Hill ($c = Km$), OTP, $c = (r,\,ar+b+m)$ |

Un cifrado lineal cae con pocos pares (plano, cifrado): se plantea el sistema y se despeja la clave. Por eso los cifrados modernos tienen al menos una etapa no lineal.

> [!note] El OTP también es lineal
> Y aun así tiene secreto perfecto. La linealidad es fatal cuando la clave **se reutiliza**: conocer un par (plano, cifrado) entrega la clave, que en el OTP no se vuelve a usar nunca.

**Línea de tiempo que conviene tener**: escítala (transposición) · César (rotación) · ~800 d.C. primeros tratados de análisis de frecuencias · 1553 Vigenère (en rigor, Bellaso) · 1863 Kasiski · 1939 Bombe (Turing, sobre el trabajo de Rejewski) · **1949 Shannon** → criptografía moderna · **1976 Diffie-Hellman** · 1978 Needham-Schroeder · 1979 Shamir · 1981 Denning-Sacco.

---
## 2. Clase 2 — Secreto perfecto, seguridad computacional y cifrado simétrico

Detalle: [Criptografia y seguridad Clase 2 - Cifrado](Criptografia%20y%20seguridad%20Clase%202%20-%20Cifrado.md) · Katz & Lindell, caps. 2 y 3

### 2.1 Secreto perfecto

> [!important] Definición
> Un criptosistema tiene **secreto perfecto** si para **toda** distribución de probabilidad sobre $M$, todo $m$ y todo $c$ con $\Pr[C=c] > 0$:
> $$\Pr[M=m \mid C=c] = \Pr[M=m]$$
> Ver el cifrado **no cambia** lo que el adversario cree sobre el mensaje, aunque tenga poder de cómputo **ilimitado**.

**La paradoja**: $c = e_k(m)$, así que $C$ y $M$ son dependientes; pero la definición dice que son independientes. Se resuelve porque la dependencia pasa por $k$, que es aleatoria y desconocida: promediando sobre todas las claves, la condicional queda igual a la marginal.

Formas equivalentes (la Guía 2, ej. 1 pide demostrar "de las cuatro maneras"):

| # | Condición | Comentario |
|---|---|---|
| 1 | $\Pr[M=m \mid C=c] = \Pr[M=m]$ | la definición (se calcula con Bayes) |
| 2 | $\Pr[C=c \mid M=m] = \Pr[C=c]$ | la misma, del otro lado |
| 3 | $\Pr[C=c \mid M=m_0] = \Pr[C=c \mid M=m_1]$ para todo $m_0, m_1, c$ | no depende de la distribución de $M$: la más cómoda |
| 4 | $\Pr[\text{PrivK}^{eav}_{A,\Pi} = 1] = \tfrac12$ **exacto** para todo $A$ (sin límite de cómputo) | la que sirve para **refutar**: un adversario que gane con $\neq \tfrac12$ |

> [!warning] Lo que el secreto perfecto **no** dice (Guía 2, ej. 2)
> No implica $\Pr[M=m \mid C=c] = \Pr[M=m' \mid C=c]$. Eso valdría solo si $M$ fuera uniforme. Contraejemplo: OTP de un bit con $\Pr[M=0] = 0{,}9$: tiene secreto perfecto y $\Pr[M=0 \mid C=c] = 0{,}9 \neq 0{,}1 = \Pr[M=1 \mid C=c]$. El cifrado no **iguala** los mensajes: deja las probabilidades **como estaban**.

**Casos de la Guía 2, ej. 3**:

- **Rotación de un solo símbolo** → sí: $\Pr[C=c \mid M=m] = \Pr[K = c-m] = \tfrac1{26}$ para todo $m$.
- **Sustitución** → solo con mensajes de **una** letra: cada par $(m,c)$ lo realizan $25!$ de las $26!$ claves. Con dos letras ya no (`aa` nunca se cifra como `ab`). La cota necesaria $\lvert K\rvert \ge \lvert M\rvert$ solo da $\lvert M\rvert \le 26!$.
- **Vigenère** con clave uniforme de largo $t$ para palabras de largo $t$ → sí: es un OTP módulo 26.

### 2.2 One-Time Pad (Vernam, 1917)

$$Gen: k \leftarrow \{0,1\}^n \text{ uniforme},\; n = \lvert m\rvert \qquad e_k(m) = m \oplus k \qquad d_k(c) = c \oplus k$$

> [!success] Teorema: el OTP tiene secreto perfecto
> Con $N = 2^n$ claves equiprobables y $K$ independiente de $M$:
> $$\Pr[C=c] = \sum_k \Pr[M = c\oplus k]\cdot\Pr[K=k] = \tfrac1N \sum_k \Pr[M=c\oplus k] = \tfrac1N$$
> (al recorrer todas las $k$, $c \oplus k$ recorre todos los mensajes). Y $\Pr[C=c \mid M=m] = \Pr[K = c\oplus m] = \tfrac1N$. Son iguales → forma 2 → secreto perfecto. $\blacksquare$

La garantía necesita **las cuatro** condiciones a la vez: clave **uniforme**, del **largo del mensaje**, **secreta** y usada **una sola vez**.

- **Reusar la clave**: $c_1 \oplus c_2 = m_1 \oplus m_2$. La clave desaparece y queda una relación entre planos, explotable por la redundancia del idioma.
- **Clave sesgada**: el ejemplo de la clase ($\Pr[K=00]=0{,}3$, …) da $\Pr[M=00 \mid C=01] \approx 0{,}32 \neq 0{,}6 = \Pr[M=00]$. La uniformidad es parte de la prueba, no un detalle.

### 2.3 El precio del secreto perfecto

> [!success] Teorema: secreto perfecto ⇒ $\lvert K\rvert \ge \lvert M\rvert$
> Idea de la prueba: tomar $M$ uniforme y un $c$ posible. Los mensajes de los que puede venir $c$ son $\{Dec_k(c) : k \in K\}$, a lo sumo $\lvert K\rvert$. Si $\lvert K\rvert < \lvert M\rvert$, hay un $m$ que **no** puede producir $c$: $\Pr[M=m \mid C=c] = 0 \neq \Pr[M=m]$. $\blacksquare$
>
> La slide lo escribe $\lvert K\rvert \ge \lvert C\rvert$: con $Enc$ determinístico también vale y es más fuerte, porque $\lvert C\rvert \ge \lvert M\rvert$.

**Teorema de Shannon** (Katz, cap. 2). Si $\lvert M\rvert = \lvert K\rvert = \lvert C\rvert$, hay secreto perfecto **si y solo si** (1) cada clave se elige con probabilidad $1/\lvert K\rvert$ y (2) para cada par $(m,c)$ existe **exactamente una** clave con $e_k(m) = c$. La slide lo dice así: *todo criptosistema con secreto perfecto es reducible al OTP, y el que no lo sea no tiene secreto perfecto*.

Consecuencia: para proteger $n$ bits hay que compartir antes $n$ bits secretos y tirarlos. **Impráctico** → hacen falta otras construcciones.

### 2.4 Seguridad computacional

El secreto perfecto es **seguridad incondicional**. La computacional relaja dos cosas:

1. **Limita al adversario**: solo algoritmos eficientes, probabilísticos de tiempo polinomial en el parámetro de seguridad $n$ ($PPT(n)$).
2. **Acepta un éxito chico**: se tolera una ventaja **despreciable**.

> [!important] Función despreciable
> $\varepsilon(n)$ es **despreciable** si para todo polinomio $p$ existe $N$ tal que para todo $n > N$: $\varepsilon(n) < \dfrac{1}{p(n)}$. O sea, cae más rápido que la inversa de **cualquier** polinomio.
> - Sí: $2^{-n}$, $2^{-\sqrt n}$, $n^{-\log n}$.
> - No: $\tfrac{1}{n^{10}}$ (es la inversa de un polinomio), ni una constante como $10^{-20}$ (no cae con $n$).
> - Cerrada por suma y por producto con un polinomio: repetir un ataque una cantidad polinomial de veces sigue dando éxito despreciable.

La pregunta cambia de "¿existe un ataque?" a "¿existe un ataque **factible** en este modelo?". Toda afirmación de seguridad computacional viene con tres cosas: qué puede hacer el adversario, cuánto puede computar y qué ventaja se tolera.

### 2.5 Generadores pseudoaleatorios y cifrado de flujo

**PRG**: algoritmo **determinístico** $G: \{0,1\}^s \rightarrow \{0,1\}^n$ con $s < n$, tal que para todo distinguidor $D$ eficiente
$$\big\lvert \Pr[D(G(U_s)) = 1] - \Pr[D(U_n) = 1] \big\rvert \le negl .$$
No alcanza con que la salida "parezca desordenada" o pase tests estadísticos: tiene que resistir a **todo** distinguidor eficiente.

> [!example] El generador congruencial de la slide no es un PRG
> $G_0 = s$, $G_i = 3G_{i-1}+1 \bmod 11$, salida $G_i \bmod 2$. Con $s=2$ los estados son $2, 7, 0, 1, 4, 2, 7,\dots$ (bits `01010`); con $s = 6$: $6, 8, 3, 10, 9, 6,\dots$ (bits `00101`). El período es **5** para toda semilla salvo $s=5$ (punto fijo), porque $3$ tiene orden 5 módulo 11: las 10 semillas caen en **dos** ciclos. Viendo unos pocos bits se predice el resto. Verificado con Python.

**Cifrado de flujo**: el OTP con la clave reemplazada por la salida de un PRG:
$$e_k(m) = G(k)\oplus m \qquad d_k(c) = G(k) \oplus c \qquad \lvert K\rvert \ll \lvert M\rvert$$
Por §2.3 **pierde el secreto perfecto**; lo que se busca es EAV.

> [!success] Teorema: si $G$ es un PRG, el cifrado de flujo es EAV-seguro (un mensaje)
> **Por reducción**: si existe $A$ que gana EAV con $\tfrac12 + \varepsilon$, se arma un distinguidor $D$ para $G$. $D$ recibe un string $w$ (que es $G(s)$ o uniforme), le pide a $A$ sus $m_0, m_1$, sortea $b$, le entrega $c = w \oplus m_b$ y devuelve 1 si $A$ acierta.
> - Si $w$ es uniforme, $A$ está jugando contra un **OTP**: acierta con $\tfrac12$ exacto.
> - Si $w = G(s)$, está jugando contra el cifrado de flujo: acierta con $\tfrac12 + \varepsilon$.
>
> $D$ distingue con ventaja $\varepsilon$. Como $G$ es PRG, $\varepsilon$ es despreciable. $\blacksquare$
>
> **La vuelta** (el ejercicio de la slide): si un $D$ distingue $G$, el flujo cae. $A$ elige $m_0 = 0^n$ y $m_1 = r$ al azar; si $b=0$ recibe $G(k)$, si $b=1$ recibe $G(k)\oplus r$, que es uniforme. Responde $b'=0$ si $D$ dice "es $G$": gana con $\tfrac12 + \tfrac12(\text{ventaja de } D)$.

> [!tip] Cómo se lee una prueba por reducción
> "Si $\Pi$ se rompe, entonces se rompe $G$" (contrarrecíproco de "si $G$ es seguro, $\Pi$ es seguro"). Se construye un algoritmo que **usa al atacante de $\Pi$ como subrutina** y le simula el experimento. Es el formato de todas las pruebas de la materia (también ElGamal ⇐ DDH, DH ⇐ DDH, cifrado autenticado ⇐ CPA + MAC).

### 2.6 Experimentos: EAV, MUL y CPA

| Experimento | Qué recibe $A$ | Modela |
|---|---|---|
| **EAV** ($\text{PrivK}^{eav}$) | elige $m_0, m_1$ de **igual largo**, recibe $c = e_k(m_b)$ | escucha pasiva de un mensaje |
| **MUL** | elige dos **vectores** de mensajes, recibe todos los $e_k(m_{b,j})$ con la misma $k$ | escucha de varios mensajes con la misma clave |
| **CPA** | además tiene un **oráculo** $e_k(\cdot)$ antes y después del desafío | puede **hacer cifrar** lo que quiera |

En los tres gana si $b' = b$; es seguro si $\Pr \le \tfrac12 + negl(n)$. El "igual largo" evita que el tamaño del cifrado delate cuál se eligió.

> [!important] Relaciones que hay que saber
> - **CPA ⇒ MUL ⇒ EAV**: más poder al adversario, definición más fuerte.
> - **EAV ⇏ MUL**: el flujo con la misma $G(k)$ es EAV-seguro pero cae en MUL. Ataque de la slide: vectores $(0^n, 0^n)$ y $(0^n, 1^n)$; si $c_1 \oplus c_2 = 0^n$ era $b = 0$. Es el OTP reusado, y no es casualidad.
> - **Determinístico ⇒ no MUL ⇒ no CPA**: cifrar dos veces lo mismo da lo mismo, y eso se detecta. Hace falta **cifrado probabilístico**; la aleatoriedad no tiene que ser secreta.
> - **CPA para un mensaje ⇒ CPA para muchos** (argumento híbrido). Por eso un esquema CPA de largo fijo se extiende cifrando cada bloque por separado, con aleatoriedad **nueva** en cada uno.
> - **CPA no da integridad**: el esquema puede ocultar el mensaje y dejar que el atacante lo modifique de forma controlada (§3.1).

**Cómo no reusar $G(k)$**: meter en el generador un valor que no se repite para la misma clave (**IV** o **nonce**). *Modo sincronizado*: un IV y el estado evoluciona coordinado. *No sincronizado*: un IV por mensaje, que viaja en claro. Según el modo, el IV tiene que ser **impredecible** (CBC) o alcanza con que sea **único** (CTR).

### 2.7 Primitivas de bloque: PRF y PRP

$$E: \{0,1\}^{\lvert k\rvert} \times \{0,1\}^b \rightarrow \{0,1\}^b$$

- **PRF** (función pseudoaleatoria): con $k$ secreta, $E_k(\cdot)$ es indistinguible (con acceso de oráculo) de una **función** elegida al azar entre todas las de $\{0,1\}^b \rightarrow \{0,1\}^b$.
- **PRP** (permutación pseudoaleatoria): además, cada $E_k$ es una **permutación** → existe $D_k$. Los cifradores de bloque (DES, AES) se modelan así.
- La primitiva sola es **determinística** → no pasa MUL. Hay que combinarla con un **modo de encadenamiento**.
- **No está demostrado que existan** PRGs ni PRFs (probarlo implicaría $P \neq NP$). Toda la criptografía simétrica moderna descansa en que AES "se comporta como" una PRP.

**Padding**: *simple pad* (ceros; hay que conocer el largo real por otra vía) o *bit padding* / *DES pad* (un `1` y ceros; si el mensaje ya ocupa bloques completos, agrega un bloque entero). Validarlo mal crea un *padding oracle* (§3.2).

### 2.8 Modos de encadenamiento

| Modo | Cifrado | ¿Necesita $D_k$? | CPA-secure si… | Error de 1 bit en $C_i$ | Paraleliza |
|---|---|---|---|---|---|
| **ECB** | $C_i = E_k(M_i)$ | sí | **nunca** (determinístico) | $M_i$ entero | todo |
| **CBC** | $C_i = E_k(M_i \oplus C_{i-1})$, $C_0 = IV$ | sí | IV **aleatorio e impredecible** | $M_i$ entero + el mismo bit de $M_{i+1}$ | solo descifrado |
| **CFB** | $C_i = M_i \oplus E_k(C_{i-1})$ | no | IV aleatorio | el mismo bit de $M_i$ + $M_{i+1}$ entero | solo descifrado |
| **OFB** | $S_i = E_k(S_{i-1})$, $C_i = M_i\oplus S_i$ | no | IV aleatorio y nunca repetido | **solo ese bit** | keystream precalculable |
| **CTR** | $C_i = M_i \oplus E_k(\text{nonce} \mathbin{\Vert} i)$ | no | $(k, \text{nonce})$ **nunca** repetido | **solo ese bit** | todo + acceso aleatorio |

> [!success] Teorema (slide *Seguridad de cifrado por bloques*)
> Si la primitiva es una PRF: **CBC, OFB y CFB son CPA-secure con IVs aleatorios**; **CTR es CPA-secure si no se repite $(k,\text{nonce})$**; ECB no lo es. La prueba es por reducción a la primitiva, y **vale solo con sus hipótesis**: un IV mal generado o un nonce repetido deja al modo fuera del teorema.

- **CFB, OFB y CTR convierten el bloque en flujo**: solo usan $E_k$, así que alcanza con una PRF (V/F de 1C-2023: "toda primitiva de bloque tiene que ser reversible" es **falso**).
- **Ningún modo da integridad**: en CTR/OFB/CFB cambiar un bit del cifrado cambia **ese** bit del plano; en CBC, cambiar un bit de $C_{i-1}$ cambia ese bit de $M_i$ (y arruina $M_{i-1}$).
- **Pérdida o inserción de un bloque**: ECB, CBC y CFB se resincronizan solos; OFB y CTR se desincronizan.
- ECB conserva los **patrones** del plano (bloques iguales → cifrados iguales): corrección no implica seguridad.

![imagen|480](Attachments/Clase%202%20-%20Encadenamiento%20CBC.png)

### 2.9 DES, 3DES y AES

| | DES | 3DES | AES |
|---|---|---|---|
| Bloque | 64 bits | 64 bits | **128 bits** (siempre) |
| Clave | 64 bits, **56 efectivos** (8 de paridad) | 3 claves: 168 bits, **~112 efectivos** | 128, 192 o 256 bits |
| Rondas | 16 | 3 × 16 | 10, 12 o 14 (según la clave) |
| Estructura | red de **Feistel** | $E_{k_1}(D_{k_2}(E_{k_3}(m)))$ (EDE) | red de sustitución-permutación sobre $GF(2^8)$ |
| Estado hoy | roto por fuerza bruta ($2^{56}$) | deshabilitado por NIST para cifrar desde 2024 | **el recomendado** |

- **Feistel**: se parte el estado en mitades $(L, R)$ y cada ronda hace $L' = R$, $R' = L \oplus f(R, k_i)$. Se descifra con **la misma estructura** y las subclaves en orden inverso, **aunque $f$ no sea invertible**. La $f$ de DES: expansión 32→48, XOR con la subclave, S-boxes 6→4 (lo no lineal) y permutación.
- **Ataques a DES**: fuerza bruta $2^{56}$; criptoanálisis diferencial $2^{47}$ (su diseño ya lo resistía mejor que otros); lineal $2^{43}$.
- **Claves débiles de DES** (Guía 2, ej. 8): con la clave de todos 0 o todos 1, las 16 subclaves son **iguales**, así que cifrar es lo mismo que descifrar y $E_k(E_k(x)) = x$. Las otras dos son las que dejan una mitad de la clave en 0 y la otra en 1: `E0E0E0E0F1F1F1F1` y `1F1F1F1F0E0E0E0E`. Verificado con `pycryptodome`: las cuatro cumplen $E_k(E_k(x)) = x$.
- **3DES**: el paso central **descifra** para que con $k_1 = k_2 = k_3$ sea compatible con DES. Da ~112 bits y no 168 por el ataque **meet-in-the-middle** (el mismo que hace que un "doble DES" apenas mejore a DES). Es 3 veces más lento y mantiene el bloque de 64 bits.
- **AES**, cada ronda: **SubBytes** (S-box no lineal: **confusión**), **ShiftRows** (corre los **bytes** de cada fila) y **MixColumns** (mezcla lineal invertible de cada columna): **difusión**; **AddRoundKey** (XOR con la subclave: el **único** paso que usa la clave). Antes de las rondas hay un AddRoundKey inicial y la última ronda no tiene MixColumns. La expansión de clave **no agrega entropía**.

> [!bug] "ShiftRows: permutación de bits"
> Así lo resume la slide; son desplazamientos cíclicos de **bytes** dentro de cada fila. Ya anotado en [la nota de la Clase 2](Criptografia%20y%20seguridad%20Clase%202%20-%20Cifrado.md#AES%20—%20Advanced%20Encryption%20Standard).

### 2.10 Estado de un criptosistema

- **Seguro**: cumple las expectativas de su modelo de seguridad.
- **Debilitado**: hay adversarios con probabilidad no despreciable, pero el esfuerzo es enorme (décadas) o las condiciones muy difíciles (por ejemplo, $2^{80}$ mensajes).
- **Quebrado**: hay adversarios con probabilidad no despreciable en tiempos practicables.

Un criptosistema puede ser **seguro y estar quebrado al mismo tiempo**, bajo pruebas distintas (el flujo con $G(k)$ fijo: EAV sí, MUL no). Y es **mala práctica** diseñar uno propio: se usan los estudiados durante años, en construcciones completas y con bibliotecas mantenidas.

---
## 3. Clase 3 — Integridad: MACs, hash y cifrado autenticado

Detalle: [Clase 3 - Criptografia - MACs y modo autenticado](Clase%203%20-%20Criptografia%20-%20MACs%20y%20modo%20autenticado.md) · Katz & Lindell, cap. 4

### 3.1 Maleabilidad: por qué CPA no alcanza

**Maleable** = modificar el cifrado produce un cambio **predecible** en el plano. No es un bug de implementación: sale de cómo está construido el cifrado. En flujo:
$$c' = c \oplus x \;\Rightarrow\; Dec_k(c') = m \oplus x$$

El ejemplo de la clase (sueldos cifrados en una base): el atacante conoce su sueldo $m$, quiere $m'$, y aplica $x = m \oplus m'$ sobre **su** celda. Nunca descifró nada, el esquema sigue siendo CPA-secure, y el sistema quedó comprometido. Le alcanza con tres cosas: el **formato** del mensaje, **un** plano conocido y poder **escribir** el cifrado.

### 3.2 CCA: el atacante que además ve descifrar

$\text{PrivK}^{cca}$: como CPA, pero $A$ tiene **también** un oráculo $d_k(\cdot)$, que puede usar con **cualquier** cifrado **salvo el desafío $c$**. Seguro si $\Pr \le \tfrac12 + negl(n)$. CCA ⇒ CPA.

- **No es ciencia ficción**: un error distinto según si el padding era válido, un tiempo de respuesta distinto o un log funcionan como un oráculo de descifrado parcial (*padding oracle*, Bleichenbacher sobre RSA).
- **Ningún esquema de las Clases 1 y 2 es CCA-secure**, y el motivo es siempre la maleabilidad. Ataque genérico al flujo: $m_0 = 0^n$, $m_1 = 1^n$; con el desafío $c$, pedir $d_k(c \oplus 0^{n-1}1)$ (está permitido porque es otro cifrado), y mirar un bit que no se tocó. Gana con probabilidad 1.

| Esquema | Cómo se toca $c$ | Efecto en $m$ |
|---|---|---|
| OTP / flujo | $c \oplus x$ | $m \oplus x$ |
| CTR / OFB | $c_i \oplus x$ | $m_i \oplus x$ |
| CBC | $c_{i-1} \oplus x$ | $m_i \oplus x$ (y $m_{i-1}$ ilegible) |
| ECB | permutar o repetir bloques | permuta o repite los $m_i$ |
| Textbook RSA | $c \cdot r^e$ | $m \cdot r$ |
| ElGamal | $(c_1,\ c_2 \cdot r)$ | $m \cdot r$ |

CCA no se arregla desde la confidencialidad: hace falta un **control de integridad**.

### 3.3 MAC (Message Authentication Code)

$$Gen: () \rightarrow K \qquad Mac: K \times P \rightarrow T \qquad Vrfy: K \times P \times T \rightarrow \{0,1\}$$

con $Vrfy_k(m, Mac_k(m)) = 1$. Misma forma que un criptosistema, objetivo opuesto: el MAC **no oculta** nada (la etiqueta viaja con el mensaje) y **no** necesita ser invertible; la etiqueta es corta y de tamaño fijo.

> [!important] Seguridad: $\text{Mac-forge}_{A,\Pi}$
> 1. $k \leftarrow Gen$.
> 2. $A$ tiene un oráculo $Mac_k(\cdot)$ y consulta lo que quiera; $Q$ = mensajes consultados.
> 3. $A$ emite $(m, t)$ con $m \notin Q$.
>
> Gana si $Vrfy_k(m,t) = 1$. El MAC es **infalsificable** si $\Pr \le negl(n)$.
>
> Es falsificación **existencial** bajo **mensajes elegidos**: cuenta cualquier $m$ nuevo, **aunque no tenga sentido**. La lección de 2004: las colisiones de MD5 "sin sentido" terminaron en 2008 en un certificado de CA falso.

**Qué da y qué no**:

- ✅ **Integridad** y **autenticación de origen** entre quienes comparten $k$.
- ❌ **Confidencialidad**: $m$ viaja en claro.
- ❌ **No repudio**: los dos tienen $k$, cualquiera de los dos pudo generar el tag.
- ❌ **Replay**: un mensaje repetido llega con un tag **válido**. Hace falta nonce, timestamp o número de secuencia.

**Las tres condiciones** que salen de los MACs rotos de la Guía 3, ej. 1 ($G(k)\oplus m$, $k \oplus \text{first}(m)$, $Enc_k(\lvert m\rvert)$): un MAC seguro tiene que (1) **depender de todos los bits** del mensaje, (2) **no filtrar la clave** a partir de pares $(m,t)$ y (3) tener etiqueta **corta y de tamaño fijo**. El tercer ejemplo muestra además que **no determinístico ≠ seguro**: como $Enc$ es CPA, $Vrfy$ no puede recalcular y comparar; tiene que descifrar $t$ y comparar con $\lvert m\rvert$, y aun así cualquier $m'$ del mismo largo verifica.

### 3.4 CBC-MAC

$$t_0 = 0^n \qquad t_i = F_k(t_{i-1} \oplus m_i) \qquad Mac_k(m) = t_\ell$$

| | Cifrado CBC | CBC-MAC |
|---|---|---|
| IV | aleatorio, viaja con el cifrado | **fijo en 0** |
| Salida | todos los bloques | **solo el último** |
| Invertible | sí | no (nunca se descifra) |

- **IV fijo**: con un IV aleatorio que viaja con el tag, el atacante cambia $IV' = IV\oplus\Delta$ y $m_1' = m_1 \oplus \Delta$ y el tag no cambia.
- **Seguro solo para mensajes de largo fijo.** Con largo variable: si $t = Mac_k(m)$ para $m$ de un bloque, entonces $(m \mathbin{\Vert} (m \oplus t),\ t)$ verifica, porque el segundo bloque vuelve a meter $F_k(m)$. El ataque de la clase es la versión de dos bloques: $(A \mathbin{\Vert} B \mathbin{\Vert} (A \oplus t_1),\ t_2)$.
- **Arreglos** (las opciones de Katz, Guía 3 ej. 4): (1) derivar $k' = F_k(\lvert m\rvert)$ y usarla en la cadena; (2) **anteponer** la longitud, $Mac_k(\lvert m\rvert \mathbin{\Vert} m)$; (3) recifrar el tag con otra clave, $t = F_{k_2}(\text{CBC-MAC}_{k_1}(m))$.
- **La longitud como sufijo no sirve**: el atacante ya controla lo que entra antes de que llegue la longitud. La intuición de los tres arreglos es que dos mensajes de distinto largo **nunca compartan estados intermedios**.
- **MAC por XOR de bloques** (Guía 3, ej. 2): $Mac$ aplicado a $\bigoplus_i m_i$ es invariante al **reordenar** bloques (*"el auto es azul y el lápiz rojo"* ↔ *"el auto es rojo y el lápiz azul"*).

### 3.5 Funciones de hash criptográficas

**Sin clave**: $Gen$ elige $s \leftarrow S$ (un **selector** de la familia, público; muchas veces $S = \{s_0\}$) y $H_s: \{0,1\}^* \rightarrow \{0,1\}^\ell$. Cualquiera, incluido el atacante, las puede calcular. Como el dominio es infinito y el codominio tiene $2^\ell$ elementos, **las colisiones siempre existen** (palomar): lo que se pide es que sean inviables de **encontrar**.

| Propiedad | Qué es inviable | Fuerza bruta |
|---|---|---|
| **Preimagen** (una vía) | dado $y$, hallar $x$ con $H(x) = y$ | $\sim 2^\ell$ |
| **Segunda preimagen** | dado $x$, hallar $x' \neq x$ con $H(x') = H(x)$ | $\sim 2^\ell$ |
| **Colisión** | hallar **cualquier** par $x \neq x'$ con $H(x) = H(x')$ | $\sim 2^{\ell/2}$ (**cumpleaños**) |

- **Implicaciones**: colisión ⇒ segunda preimagen (una segunda preimagen **es** una colisión) ⇒ preimagen (para funciones que comprimen). La más fuerte es la de colisiones. Por eso "la seguridad de un hash es la resistencia a preimágenes" es **falso** (V/F de 1C-2023/2018).
- **Experimento $\text{Hash-coll}_{A,H}$**: $s \leftarrow S$, $A$ recibe $s$ y emite $x, x'$; gana si $x \neq x'$ y $H_s(x) = H_s(x')$. Resistente si $\Pr \le negl(n)$.
- **Cumpleaños**: con $N = 2^\ell$ salidas posibles, $q \approx 1{,}18\sqrt N$ valores al azar ya dan probabilidad $\tfrac12$ de colisión (23 personas para 365 días). Por eso la salida mínima era **160 bits** ($2^{80}$ para colisiones) y hoy se usan **256**: un hash de 256 bits da los mismos 128 bits de seguridad que una clave AES-128.
- **Un hash no oculta entradas de baja entropía** (Guía 3, ej. 6): si $m$ es una nota del 1 al 10, se calculan los 10 hashes y se compara. Mismo hash ⇒ misma entrada. Resistencia a preimágenes es sobre $y$ al azar, no sobre un espacio chico.
- **Contraejemplo de la Guía 3, ej. 3**: "32 ceros si el largo es par, 32 unos si es impar" no cumple ninguna de las tres (y dos entradas al azar colisionan con probabilidad $\approx \tfrac12$).

**Merkle-Damgård** (MD5, SHA-1, SHA-2): padding + un bloque con la **longitud**, y una función de compresión $f$ iterada desde un IV fijo, $z_i = f(z_{i-1}, x_i)$.

> [!success] Teorema de Merkle-Damgård
> Si la función de compresión $f$ es resistente a colisiones, el hash completo $H$ también lo es. Se reduce la seguridad de mensajes de **cualquier** largo a la de bloques de tamaño fijo (para eso sirve la transformación, Guía 3 ej. 4d).

> [!warning] Length extension
> Es la misma cadena que CBC-MAC y sufre lo mismo: quien conoce $H(m)$ y $\lvert m\rvert$ calcula $H(m \mathbin{\Vert} pad \mathbin{\Vert} m_2)$ sin conocer $m$. Por eso **$H(k \mathbin{\Vert} m)$ no es un MAC seguro**. SHA-3 (esponja, no Merkle-Damgård) no tiene este problema.

| Función | Salida | Construcción | Estado |
|---|---|---|---|
| MD5 | 128 | Merkle-Damgård | **roto** (colisiones prácticas desde 2004) |
| SHA-1 | 160 | Merkle-Damgård | **roto** (SHAttered 2017, SHAmbles 2020) |
| SHA-2 | 224–512 | Merkle-Damgård | vigente |
| SHA-3 | 224–512 | **esponja** (Keccak) | vigente, FIPS 202 (2015) |

![imagen|420](Attachments/Clase%203%20-%20Modelo%20iterativo%20Merkle.png)

### 3.6 HMAC

$$t = H\big((k \oplus opad) \,\|\, H((k \oplus ipad) \,\|\, m)\big) \qquad ipad = \texttt{0x36}\cdots \quad opad = \texttt{0x5C}\cdots$$

- Las **dos pasadas anidadas** cortan el *length extension*: el atacante nunca ve el hash interno.
- Su seguridad real pide que la compresión se comporte como una **PRF**, no que $H$ sea resistente a colisiones. Por eso HMAC-MD5 no está roto en la práctica aunque MD5 sí. Para diseño nuevo: HMAC-SHA-256.

> [!bug] ipad y opad intercambiados en las slides
> La teórica de la Clase 3 (y la práctica de la Clase 4) dicen $opad = \texttt{0x36}$ e $ipad = \texttt{0x5C}$. El RFC 2104 dice **$ipad = \texttt{0x36}$** (hash interno) y **$opad = \texttt{0x5C}$** (externo). La fórmula anidada está bien. Ver [Clase 3](Clase%203%20-%20Criptografia%20-%20MACs%20y%20modo%20autenticado.md).

### 3.7 Confidencialidad + integridad: cómo combinar

| Orden | Qué se manda | Veredicto |
|---|---|---|
| Encrypt-and-MAC | $Enc_{k_1}(m)$ y $Mac_{k_2}(m)$ | ❌ el tag es sobre $m$ y puede filtrar información (un MAC determinístico delata mensajes repetidos) |
| MAC-then-Encrypt | $Enc_{k_1}(m \mathbin{\Vert} Mac_{k_2}(m))$ | ⚠️ puede ser seguro, **requiere prueba**; en TLS ≤ 1.2 dio padding oracles (POODLE, Lucky 13) |
| **Encrypt-then-MAC** | $c = Enc_{k_1}(m)$ y $t = Mac_{k_2}(c)$ | ✅ **siempre** seguro con las hipótesis del teorema |

> [!success] Teorema: cifrado autenticado ⇒ CCA
> Si $\Pi_E$ es **CPA-secure**, $\Pi_M$ es un MAC **infalsificable** y $k_1, k_2$ son **independientes**, Encrypt-then-MAC es **CCA-secure**. $Dec$ **verifica primero** y descifra solo si el tag es válido; si no, devuelve $\bot$.
>
> Idea: cualquier $c'$ que el adversario fabrique para el oráculo falla la verificación y recibe $\bot$, que no le dice nada; el oráculo de descifrado queda inútil y el juego vuelve a ser CPA.

> [!note] Un detalle fino: MAC **fuerte**
> Katz pide un MAC *fuertemente* infalsificable: que tampoco se pueda fabricar un tag **nuevo** para un mensaje ya consultado. Si se pudiera, el adversario CCA mandaría $(c, t')$ al oráculo, que no es el desafío literal, y recibiría $m_b$. Los MACs determinísticos que verifican recalculando (CBC-MAC, HMAC) son fuertes automáticamente, porque cada mensaje tiene un solo tag válido.

- **Claves independientes**: reusar la misma clave para cifrar y para el MAC rompe la prueba.
- **Verificar antes de descifrar**: descifrar primero es lo que habilita los padding oracles.

### 3.8 AEAD: CCM y GCM

**AEAD** (*Authenticated Encryption with Associated Data*): confidencialidad + integridad en una sola primitiva, más integridad (sin cifrado) de datos asociados que viajan en claro (headers, números de secuencia).

| | CCM | GCM |
|---|---|---|
| Cifrado | CTR | CTR |
| MAC | CBC-MAC (serie) | GHASH, multiplicación en $GF(2^{128})$ |
| Orden | MAC-then-encrypt, **con prueba propia** | encrypt-then-MAC |
| Claves | **una sola** (los bloques del MAC y del contador llevan flags distintos: nunca se evalúa $F_k$ en la misma entrada por los dos caminos) | una sola |
| Pasadas / paralelismo | 2 / no | 1 / **sí** |
| Uso | WPA2 | TLS 1.3, IPsec, QUIC |

Condición que no se negocia en los dos: **el nonce no se repite nunca con la misma clave** (repite el keystream y permite forjar tags). Regla práctica de la clase: en sistemas reales, **solo cifrado autenticado**, con una API AEAD (AES-GCM, AES-CCM, ChaCha20-Poly1305).

---
## 4. Clase 4 — Distribución de claves, criptografía asimétrica y firma digital

Detalle: [Clase 4 - Criptografía - Cifrado asimétrico y Firma digital](Clase%204%20-%20Criptografía%20-%20Cifrado%20asimétrico%20y%20Firma%20digital.md) · Katz & Lindell, caps. 9 a 12

### 4.1 El problema de distribuir claves

Todo lo anterior supone que las partes **ya comparten** $k$, y una prueba de seguridad solo puede apoyarse en que el atacante no conoce la clave. No se la puede mandar por el mismo canal.

| Estrategia | Claves en total | Por entidad | Problema |
|---|---|---|---|
| Una clave por par | $\frac{N(N-1)}{2}$ (aristas de $K_N$) | $N-1$ | crece **cuadráticamente** |
| **KDC** (tercero de confianza) | $N$ | 1 | **punto único de falla** (si cae, nadie arranca; si lo comprometen, lee todo) y **no elimina** el problema: $k_A, k_B, \dots$ se reparten antes |
| **Asimétrica** | $N$ pares $(pk, sk)$ | 1 par | la clave pública tiene que ser **auténtica** (Clase 5) |

El KDC genera una clave de **sesión** $k_S$ y la manda cifrada con la clave larga de cada uno ($Enc_{k_A}(k_S)$, $Enc_{k_C}(k_S)$). Implementaciones: Kerberos, Active Directory. La asimétrica cambia la exigencia: ya no hace falta un canal **secreto** previo, alcanza con uno **autenticado**, que es un requisito mucho más débil.

**Tres primitivas nuevas** (Diffie-Hellman 1976, *New Directions in Cryptography*), basadas en problemas fáciles de hacer y difíciles de deshacer:

| Primitiva | Qué permite | Análogo simétrico |
|---|---|---|
| Intercambio de claves | acordar una clave en línea sin secreto previo | no hay |
| Cifrado asimétrico | cifrar con $pk$, descifrar con $sk$ | cifrado simétrico |
| Firma digital | firmar con $sk$, verificar con $pk$ | MAC |

### 4.2 El álgebra que hace falta

| Estructura | Qué pide |
|---|---|
| **Grupo** $(G, \cdot)$ | clausura, asociatividad, neutro, inverso |
| Abeliano | además, conmutativo |
| **Cíclico** | existe un **generador** $g$ con $G = \{g^0, g^1, \dots\}$ |
| Anillo $(G,+,\cdot)$ | $(G,+)$ abeliano; $\cdot$ asociativa y distributiva sobre $+$ |
| **Cuerpo** (campo) | anillo con $\cdot$ conmutativa, con neutro e **inverso** para todo elemento salvo el 0: se puede **dividir** |

- **Orden** de $g$: el menor $r > 0$ con $g^r = 1$ = tamaño del subgrupo que genera. $g$ es **generador** (elemento primitivo) si su orden es $\lvert G\rvert$. Un grupo cíclico de orden $n$ tiene $\varphi(n)$ generadores.
- **Teorema de Lagrange**: el orden de cualquier elemento **divide** a $\lvert G\rvert$. De ahí el test de generador que se usa en los ejercicios: $g$ genera $\mathbb{Z}_p^*$ si y solo si $g^{(p-1)/r} \neq 1$ para **cada primo** $r$ que divide a $p-1$.
- **$\mathbb{Z}_n = \{0, \dots, n-1\}$**: con $n$ **primo** es un **cuerpo**; si no, es un anillo. **$\mathbb{Z}_n^*$** = los coprimos con $n$, grupo multiplicativo de orden $\varphi(n)$. Para $p$ primo, $\mathbb{Z}_p^*$ es cíclico de orden $p-1$.
- **Cuerpos finitos (Galois)**: todo cuerpo finito tiene $p^k$ elementos y dos del mismo tamaño son isomorfos. Los que aparecen: $\mathbb{Z}_p$ (DH, RSA, ElGamal) y $GF(2^8)$, $GF(2^{128})$ (AES, GCM).
- **Inverso**: $a$ tiene inverso módulo $n$ si y solo si $\gcd(a,n) = 1$; se calcula con **Euclides extendido**.
- **$\varphi$ de Euler**: $\varphi(p) = p-1$; $\varphi(p^a) = p^{a-1}(p-1)$; multiplicativa para coprimos. Para RSA: $\varphi(pq) = (p-1)(q-1)$.
- **Teorema de Euler**: si $\gcd(a,n)=1$, $a^{\varphi(n)} \equiv 1 \pmod n$. **Fermat**: $a^{p-1} \equiv 1 \pmod p$ para $p$ primo y $p \nmid a$.

> [!bug] Tres errores del repaso de álgebra (ya anotados en la [Clase 4](Clase%204%20-%20Criptografía%20-%20Cifrado%20asimétrico%20y%20Firma%20digital.md))
> - $\mathbb{Z}_n$ escrito como $\{1, \dots, n-1\}$: sin el 0 no hay neutro y no es grupo.
> - "Campo canónico" de tamaño $p-1$: eso es $\mathbb{Z}_p^*$, un **grupo** multiplicativo. El cuerpo es $\mathbb{Z}_p$ entero, de tamaño $p$.
> - Teorema de Euler sin la hipótesis $\gcd(a,n) = 1$: $2^{\varphi(4)} = 4 \equiv 0 \pmod 4$.

### 4.3 Intercambio de claves y Diffie-Hellman

Un protocolo $\Pi(n) \rightarrow (\textit{Trans}, k_A, k_B)$: **no tiene entrada** (la salida es la clave), el atacante ve todo el *transcript*, y la corrección es $k_A = k_B$.

> [!important] Experimento $KE_{A,\Pi}$ (adversario **pasivo**)
> Se ejecuta $\Pi$ y sale $k$. Se sortea $b$: si $b = 1$, $k' = k$; si $b = 0$, $k'$ uniforme. $A$ recibe **$\textit{Trans}$ y $k'$** y emite $b'$. Seguro si $\Pr[b' = b] \le \tfrac12 + negl(n)$.
> No alcanza con no poder **calcular** la clave: no tiene que poder **distinguirla** de una aleatoria (sacar un bit ya rompe el experimento).

**Diffie-Hellman**: $A$ fija $(G, q, g)$, elige $x \leftarrow \mathbb{Z}_q$ y manda $h_1 = g^x$; $B$ elige $y$ y manda $h_2 = g^y$; $k_A = h_2^{\,x} = g^{xy} = h_1^{\,y} = k_B$. El atacante ve $g^x$ y $g^y$ y no puede combinarlos: $g^x \cdot g^y = g^{x+y} \neq g^{xy}$. **DH acuerda** la clave: nadie la **envía** (V/F de 1C-2023/2018).

| Supuesto | Enunciado | |
|---|---|---|
| **DL** (logaritmo discreto) | dado $g^x$, no se puede hallar $x$ | necesario, no suficiente |
| **CDH** | dados $g^x, g^y$, no se puede calcular $g^{xy}$ | intermedio |
| **DDH** | dados $g^x, g^y$, no se puede **distinguir** $g^{xy}$ de un $g^z$ al azar | el que hace falta |

Resolver DL ⇒ resolver CDH ⇒ resolver DDH. Entonces, al revés: **DDH difícil ⇒ CDH difícil ⇒ DL difícil**. DDH es el supuesto más fuerte (se asume más).

> [!success] Teorema
> Si DDH es difícil en $G$, Diffie-Hellman es seguro en el experimento $KE$ (contra adversarios **pasivos**). Es prácticamente la definición: $KE$ le da al adversario $(g^x, g^y, k')$ con $k' = g^{xy}$ o uno al azar, que es exactamente el problema DDH.

> [!bug] "El logaritmo discreto es NP-hard" — falso
> Lo dice la slide *La seguridad de DH* y se repitió en clase. DL está en $NP \cap coNP$ y no se sabe (ni se cree) que sea NP-hard; si lo fuera, colapsaría la jerarquía polinomial. Además la criptografía necesita dureza en el **caso promedio**, no en el peor caso. Lo correcto: "se **conjetura** difícil". Lo mismo con la factorización.

> [!note]- Para ir más allá: por qué los grupos de DH son de orden primo
> En $\mathbb{Z}_p^*$ **entero** DDH es **falso**: el símbolo de Legendre (si un número es cuadrado módulo $p$) se calcula fácil y delata la **paridad** del exponente. De $g^x$ y $g^y$ se saca la paridad de $x$ y de $y$, y con eso la de $xy$, que se compara con la del candidato. Con $g^{xy}$ el test acierta siempre; con un valor al azar, la mitad de las veces (verificado con Python, $p = 1000003$). Por eso se trabaja en un **subgrupo de orden primo** $q$ (por ejemplo los cuadrados módulo un *safe prime* $p = 2q+1$), igual que DSA con su $g$ de orden $q$. Esto también responde "¿qué $q$ son válidos?" del 1C-2025.

**Contra un atacante activo, DH puro cae con un MITM**: Mallory le manda su $g^{x'}$ a Bob y su $g^{y'}$ a Alice y queda con dos claves, descifrando y recifrando todo. Nada ata $g^x$ a la identidad de $A$. DH necesita un canal **autenticado**, no secreto: se firma $(g^x, g^y)$ **junto con las identidades** (Guía 4, ej. 7), que es lo que hacen TLS y SSH. Si $x$ e $y$ son efímeros (se descartan), da **forward secrecy** (§5.4).

### 4.4 Criptosistema asimétrico

$$Gen: () \rightarrow PK \times SK \qquad Enc: PK \times P \rightarrow C \qquad Dec: SK \times C \rightarrow P \qquad d_{sk}(e_{pk}(m)) = m$$

> [!important] Consecuencias de que $pk$ sea pública
> - **EAV ⇒ CPA automáticamente**: el experimento le da $pk$ al adversario, y con $pk$ él mismo tiene el oráculo de cifrado. En simétrico había que dárselo aparte.
> - **Todo esquema asimétrico seguro es no determinístico**: si $Enc$ fuera determinístico, ante el desafío $c$ el adversario calcula $e_{pk}(m_0)$ y compara. Por eso el padding aleatorio de PKCS#1 y el $y$ de ElGamal.
> - **Es lento** (uno o dos órdenes de magnitud más que el simétrico): en la práctica solo se usa para transportar o acordar una clave simétrica, y el contenido va con cifrado simétrico (esquema **híbrido**, como TLS).

### 4.5 RSA

**Generación**: $p, q$ primos grandes, $n = pq$, $\varphi(n) = (p-1)(q-1)$; $e$ con $\gcd(e, \varphi(n)) = 1$; $d = e^{-1} \bmod \varphi(n)$ (Euclides extendido). $pk = (n, e)$, $sk = (n, d)$.
$$Enc_{pk}(m) = m^e \bmod n \qquad Dec_{sk}(c) = c^d \bmod n$$

- **Corrección**: $ed = 1 + k\varphi(n)$, así que $c^d = m^{ed} = m\cdot(m^{\varphi(n)})^k \equiv m$ por Euler. Esa cuenta supone $\gcd(m,n) = 1$; para el resto de los $m$ también vale (se prueba módulo $p$ y módulo $q$ con Fermat y se junta con el teorema chino del resto). Verificado para los 3233 valores de $\mathbb{Z}_{3233}$ con $p = 61$, $q = 53$.
- **Seguridad**: quien factoriza $n$ calcula $\varphi(n)$ y de ahí $d$. **Al revés también**: conocer $\varphi(n)$ equivale a factorizar, porque $p + q = n - \varphi(n) + 1$ y $pq = n$ dan una cuadrática. Lo que se asume en realidad es el **problema RSA** (calcular raíces $e$-ésimas módulo $n$ sin $d$), y **no está probado** que sea equivalente a factorizar.

**Textbook RSA es inseguro**:

1. **Determinístico** → no es CPA-secure.
2. **$e$ y $m$ chicos**: si $m^e < n$, no hay reducción modular y $m = \sqrt[e]{c}$ en los enteros.
3. **Módulo compartido**: con el mismo $n$ y distintos $e$, cada dueño puede sacar la privada del otro (con $e_1 d_1 - 1$, múltiplo de $\varphi(n)$, se factoriza $n$).
4. **Maleable**: $Enc(m_1)\cdot Enc(m_2) = Enc(m_1 m_2)$ → no es CCA ($c' = c\cdot r^e$ se descifra como $m\cdot r$, Guía 4 ej. 17).

**PKCS#1 v1.5**: $m' = \texttt{00}\,\|\,\texttt{02}\,\|\,r\,\|\,\texttt{00}\,\|\,m$, con $r$ aleatorio de bytes no nulos y al menos 8 bytes. Hace a RSA **no determinístico** (el mismo papel que el IV en CBC). Máximo $k - 11$ bytes de mensaje, con $k$ el largo de $n$ en bytes. Se **cree** CPA-secure, pero **no es CCA** (Bleichenbacher 1998: el servidor como oráculo de padding). Para CCA: **OAEP**. El padding es para CPA, no para CCA (V/F del 2C-2025).

> [!bug] Dos errores de las slides de RSA
> - *Problemas*: "se puede calcular el **logaritmo**" → es la **raíz $e$-ésima**; el logaritmo discreto es el problema de ElGamal. Y "comparten $n$ → se recupera $n$": $n$ es público, lo que se recupera es la factorización.
> - *PKCS#1*: "hasta **n**−11 bytes" → **$k$−11**.

### 4.6 ElGamal

Es Diffie-Hellman convertido en cifrado: el receptor publica su mitad una vez y el emisor hace la suya en cada mensaje.

$$pk = (G, q, g, h = g^x) \qquad sk = x \qquad Enc_{pk}(m) = (g^y,\ h^y \cdot m),\ y \leftarrow \mathbb{Z}_q \text{ nuevo} \qquad Dec_{sk}(c_1, c_2) = \frac{c_2}{c_1^{\,x}}$$

- **Corrección**: $\dfrac{h^y m}{(g^y)^x} = \dfrac{g^{xy} m}{g^{xy}} = m$.
- **CPA-secure si DDH es difícil** (demostrable). Idea: $h^y = g^{xy}$ es indistinguible de un elemento al azar, y multiplicar $m$ por un elemento al azar del grupo es un **OTP en el grupo**.
- **$y$ nunca se reusa**: con el mismo $y$, $c_2/c_2' = m_1/m_2$ y un plano conocido entrega el otro.
- **No es CCA**: es maleable, $(c_1,\ c_2\cdot r)$ se descifra como $m\cdot r$. Verificado con el ejemplo de la slide ($p = 2357$, $m = 2035$, $r = 5$ → $747 = 5\cdot2035 \bmod 2357$).

| | Textbook RSA | ElGamal |
|---|---|---|
| Problema difícil | factorización / raíz $e$-ésima | DL / **DDH** |
| Determinismo | determinístico | **probabilístico** por diseño |
| Prueba | no tiene; se parchea con padding | CPA-secure bajo DDH |
| Parámetros | $n$ propio de cada usuario | $(G, q, g)$ **compartibles** |
| Tamaño del cifrado | $\lvert n\rvert$ | $2\lvert q\rvert$ (el doble) |
| Grupo | $\mathbb{Z}_n$ | cualquiera donde DDH sea difícil, incluidas **curvas elípticas** |

**Tamaños**: RSA/DH/ElGamal sobre $\mathbb{Z}_p$: 2048 bits como piso (RSA-1024 prohibido por NIST desde 2013), 3072 para 128 bits de seguridad. Curvas elípticas: 256 bits ya dan 128 de seguridad, porque no se conocen ataques subexponenciales tipo *index calculus*. Las cifras de la slide (≥1024, ECC ≥320) están desactualizadas.

### 4.7 Firma digital

$$Gen: () \rightarrow PK\times SK \qquad Sign: SK \times P \rightarrow S \qquad Vrfy: PK \times P \times S \rightarrow \{0,1\}$$

| | Cifrado asimétrico | Firma digital |
|---|---|---|
| Clave pública | **cifra** | **verifica** |
| Clave privada | descifra | **firma** |
| Da | confidencialidad | integridad + autenticación + no repudio |

> [!important] $\text{Sig-forge}_{A,\Pi}$
> Mismo molde que Mac-forge, pero $A$ recibe **$pk$** y un oráculo $Sign_{sk}(\cdot)$. Gana si emite $(m, s)$ con $Vrfy_{pk}(m,s) = 1$ y $m \notin Q$. Seguro si $\Pr \le negl(n)$. Falsificación **existencial**: un $m$ sin sentido también cuenta.

| Propiedad | MAC | Firma |
|---|---|---|
| Integridad y autenticación | ✅ | ✅ |
| Verificable por **cualquiera** | ❌ solo quien tiene $k$ | ✅ cualquiera con $pk$ |
| **Transferible** | ❌ | ✅ |
| **No repudio** | ❌ la clave la tienen los dos | ✅ solo el firmante tiene $sk$ |
| Protege contra replay | ❌ | ❌ |
| Costo | barato | caro |

**RSA-Signature** (textbook): $s = m^d \bmod n$, se verifica $s^e \stackrel{?}{=} m$. Inseguro:

- **Falsificación sin mensaje**: elegir $s$ al azar y publicar $(s^e \bmod n,\ s)$. No hizo falta ver ninguna firma.
- **Multiplicativa**: con $(m_1, s_1)$ y $(m_2, s_2)$ válidos, $(m_1 m_2,\ s_1 s_2)$ también lo es.

**Hashed RSA**: $s = H(m)^d$, se verifica $s^e \stackrel{?}{=} H(m)$. Resuelve tres cosas: firma mensajes de cualquier largo; la falsificación sin mensaje ahora exige una **preimagen** de $s^e$; y $H(m_1)H(m_2) \neq H(m_1 m_2)$ rompe la multiplicativa. Tiene prueba solo en el modelo de oráculo aleatorio.

> [!important] Hash-and-sign exige resistencia a colisiones
> Si $H(m) = H(m')$, la firma de $m$ vale para $m'$. Es exactamente cómo en 2008 se usó una colisión de MD5 para fabricar un certificado de CA falso a partir de uno legítimo.

**DSA / DSS**: $p$ primo de $L$ bits, $q$ primo de $N$ bits con $q \mid p-1$, $g$ de orden $q$; $sk = x$, $pk = y = g^x \bmod p$. Firma con $k$ aleatorio **nuevo**: $r = (g^k \bmod p) \bmod q$, $s = k^{-1}(H(m) + xr) \bmod q$. Verifica con $u_1 = H(m)s^{-1}$, $u_2 = r s^{-1}$: $r \stackrel{?}{=} (g^{u_1} y^{u_2} \bmod p) \bmod q$. Seguridad basada en DL.

- **$k$ repetido ⇒ se despeja $x$** (dos ecuaciones, dos incógnitas): así se sacó la clave de firma de la PlayStation 3 en 2010.
- **FIPS 186-5 (2023) sacó a DSA** para generar firmas; lo aprobado hoy es RSA, ECDSA y EdDSA. Que "DSS es el estándar actual" ya no vale en esa forma.

> [!warning] No usar el mismo par para cifrar y firmar
> En textbook RSA, firmar $c$ es calcular $c^d$, que es **descifrar** $c$. Si la clave sirve para las dos cosas, pedir una "firma" es pedir un descifrado. Por eso los certificados declaran el **uso** de la clave (§5.2).

### 4.8 Post-cuántico y marco legal

- **Shor** (1994) resuelve en tiempo polinomial, en una computadora cuántica, **la factorización y el logaritmo discreto** (incluido el de curvas elípticas): cae **toda** la criptografía asimétrica de la clase. **Grover** solo da una aceleración cuadrática sobre la búsqueda de claves simétricas: se compensa duplicando el largo (AES-256). NIST estandarizó esquemas sobre retículos y hashes (ML-KEM, ML-DSA, SLH-DSA).
- **Ley 25.506** (Guía 4, ej. 14): la **firma digital** (asimétrica + certificado de un certificador licenciado) se **presume** válida y equivale a la manuscrita; la **firma electrónica** es la categoría residual, y quien la invoca tiene que probar su validez. Toda firma digital es electrónica; al revés, no.

> [!bug] "Shor amenaza a RSA, no directamente al logaritmo discreto"
> Se dijo en clase y es al revés: el paper se titula *"Algorithms for quantum computation: discrete logarithms and factoring"* y resuelve los dos.

---
## 5. Clase 5 — Protocolos: PKI, Needham-Schroeder, TLS y Shamir

Detalle: [Protocolos](Protocolos.md) · Bishop, *Computer Security*, cap. 11 · RFC 8446

Un **protocolo criptográfico** combina las primitivas de una forma concreta para obtener servicios que ninguna da sola. Lo que importa para el parcial no es memorizar mensajes sino poder decir, de cada pieza, **qué garantiza y contra qué**.

### 5.1 El atacante activo

El experimento $KE$ (y la seguridad de RSA/ElGamal "con $pk$ pública") suponían un atacante **pasivo**. Uno **activo** puede:

| Capacidad | Ejemplo |
|---|---|
| **Omitir** | el mensaje nunca llega |
| **Reescribir** | cambia contenido en tránsito (incluida una clave pública) |
| **Reordenar** | hace llegar los mensajes en otro orden |
| **Repetir** | reenvía un mensaje válido (*replay*) |

A diferencia de una red que pierde o duplica paquetes por error, acá el que lo hace es **inteligente** y elige cuándo: hay que diseñar para el peor caso.

**El problema de fondo**: para cifrar hacia $B$ hace falta $pk_B$, y $A$ se la **pide a alguien** por el mismo canal donde está el atacante. Si $E$ la reemplaza por $pk_E$, descifra, lee, modifica y recifra con la $pk_B$ auténtica (MITM). No se rompió ninguna primitiva: se rompió la **asociación identidad ↔ clave**, que es un problema de **integridad** (de la asociación) o, dicho en general, de **autenticación**.

### 5.2 PKI y certificados

**Certificado** = un documento **público** y estandarizado que dice "esta clave pública es de esta identidad", **firmado por una CA**:

$$\text{Cert}_B = \Big(\underbrace{\text{ID}_B,\ pk_B,\ \text{validez},\ \text{uso},\ \text{emisor},\ \text{nº de serie},\ \dots}_{\text{lo que se firma}},\ \ Sign_{sk_{CA}}\big(H(\cdot)\big)\Big)$$

- Contiene la **clave pública del titular** y la **firma** hecha con la **privada de la CA**. **Nunca** contiene una clave privada, ni la pública de la CA (V/F de 2C-2025 y 1C-2025).
- **Solo tiene sentido para clave pública**: es un documento que cualquiera puede ver y reenviar. Que el atacante reenvíe el certificado legítimo de $B$ no le sirve: el cifrado con $pk_B$ solo se abre con $sk_B$. Y si cambia la clave o el nombre, **rompe la firma de la CA**.
- **Todo certificado vence**: la privada puede haberse filtrado, copiado o quedado en manos de alguien que ya no está. El **uso** declarado evita mezclar cifrado y firma (§4.7).
- **Formato**: X.509 v3 (versión, serie, algoritmo de firma, emisor, validez, sujeto, clave pública del sujeto, extensiones como *Basic Constraints* `CA:TRUE`, *Key Usage*, SAN).

**Validación** (todo **offline**, salvo la revocación):

1. **Clave pública del emisor**: de la cadena que vino con el certificado; si no la conozco, validar **recursivamente** hasta una **raíz** del almacén local.
2. **Firma de la CA** → integridad: nada cambió desde la emisión (es la respuesta correcta del 1C-2018).
3. **Vigencia**.
4. **Identidad**: que sea el nombre esperado (hoy se compara contra el **SAN**, no el CN).
5. **Uso** permitido.
6. **Revocación** (CRL / OCSP).

**Cadena de confianza**: "confiar en una CA" es, en concreto, **tener su clave pública** donde corre la verificación. La cadena termina en una **raíz autofirmada** (emisor = sujeto), preinstalada en el SO, el navegador o el runtime. Las raíces se usan lo menos posible (firman intermedias y se guardan offline), y la validez **decrece** hacia abajo. **Certificación cruzada**: una CA firma el certificado de otra para que clientes que no tienen a una puedan llegar a la otra.

**Revocación** = invalidar un certificado **antes** de que venza. Choca con la validación offline: obliga a consultar algo.

| Mecanismo | Cómo | Problema |
|---|---|---|
| **CRL** | la CA publica la lista firmada de revocados; se baja y se busca (sale de la lista cuando vence) | una lista por CA, hay que mantenerlas al día |
| **OCSP** | el cliente pregunta online por un certificado | una consulta por cliente y conexión; privacidad |
| **OCSP Stapling** | el **dueño** consigue cada tanto una respuesta firmada "vigente ahora" y la abrocha a su cadena | configuración extra; el cliente vuelve a validar offline |

**Control del sistema**: *Certificate Transparency* (logs públicos de todo lo emitido) y la expulsión de las CAs que emiten mal (DigiNotar, 2011). Resumen de la clase: los certificados son **firmas digitales usadas para asociar identidades a claves**; lo que produce una PKI es **una clave pública con garantía de integridad**, que después se usa para lo que haga falta.

### 5.3 Needham-Schroeder: intercambio simétrico con KDC

Intercambio de claves usando **solo criptografía simétrica** y un KDC que comparte una clave **larga** con cada entidad ($k_a$, $k_b$). Notación: $\{M\}_k = e_k(M)$.

**Primera aproximación**: $A \rightarrow \text{KDC}$: pedido; $\text{KDC} \rightarrow A$: $\{k_s\}_{k_a} \,\|\, \{k_s\}_{k_b}$; $A \rightarrow B$: $\{k_s\}_{k_b}$. Tiene **replay** (para $B$ todo empieza en el mensaje 3: se puede repetir la sesión entera) y **key reuse** (reenviarle a $A$ un mensaje 2 viejo la obliga a usar una $k_s$ vieja).

**Needham-Schroeder (1978)**:

$$\begin{aligned}
&1.\ A \rightarrow \text{KDC}: && A \,\|\, B \,\|\, r_1\\
&2.\ \text{KDC} \rightarrow A: && \{A \,\|\, B \,\|\, r_1 \,\|\, k_s \,\|\, \{A \,\|\, k_s\}_{k_b}\}_{k_a}\\
&3.\ A \rightarrow B: && \{A \,\|\, k_s\}_{k_b}\\
&4.\ B \rightarrow A: && \{r_2\}_{k_s}\\
&5.\ A \rightarrow B: && \{r_2 - 1\}_{k_s}
\end{aligned}$$

| Pieza | Qué garantiza |
|---|---|
| $r_1$ que vuelve en 2 | que 2 **no es una repetición** (evita el key reuse) |
| $A$ y $B$ adentro de 2 | que no le reusan a $A$ una respuesta para **otras** entidades; en 1 viajan en claro y alguien podría cambiar $B$ por $E$ |
| $A$ adentro del token | que $B$ sepa **con quién** habla |
| 4 y 5 (*challenge-response*) | que del otro lado hay alguien que conoce $k_s$, y que 5 es fresco. $r_2 - 1$ y no $r_2$: si no, el atacante le **refleja** a $B$ su propio mensaje 4 |

**Ataque de Denning-Sacco (1981)**: $B$ no tiene cómo saber si el **token** es fresco. Si un atacante consigue, años después, la $k_s$ de una sesión grabada, reinyecta el mensaje 3 viejo, contesta el desafío y **se hace pasar por $A$**. Es un replay de largo plazo que explota que $k_b$ sigue vigente.

**Arreglo**: un **timestamp** $T$ dentro del token, $\{A \,\|\, T \,\|\, k_s\}_{k_b}$, y $B$ acepta solo si $\lvert \text{Clock} - T\rvert < \Delta t_1 + \Delta t_2$ (desfasaje de relojes + retardo de red). Es la base de Kerberos. La otra salida (Needham-Schroeder 1987, Guía 4 ej. 5): que $B$ genere un nonce **antes** y que viaje dentro del token.

| Recurso | Qué es | Contra qué |
|---|---|---|
| **Token** | dato cifrado que uno recibe **para pasárselo a otro** | entregar un secreto a través de un intermediario que no lo puede leer |
| **Claves cortas / largas** | la larga solo transporta; la corta se usa y se descarta | que una clave caída exponga muchas sesiones |
| **Nonce + challenge-response** | número de un solo uso que hay que devolver procesado | replay, key reuse, impersonación. Necesita **ida y vuelta** |
| **Timestamp** | hora de generación, con tolerancia | replay de largo plazo. Un solo mensaje, pero necesita **relojes sincronizados** |

> [!important] Por qué hacen falta estos trucos
> El replay viola la integridad del **sistema**, pero **ninguna primitiva lo detecta**: un mensaje repetido llega con su MAC o su firma **válidos**. La frescura se construye a nivel protocolo (nonces, timestamps, números de secuencia).

### 5.4 TLS: canal seguro

**Canal seguro** = sin problemas de confidencialidad ni de integridad contra ningún tipo de atacante. TLS lo construye entre la aplicación y el transporte (TCP; DTLS sobre datagramas), transparente para la aplicación una vez abierto.

| Da | No da |
|---|---|
| **Confidencialidad** (cifrado simétrico) | **no repudio**: los datos van con MAC/AEAD de clave **compartida** |
| **Integridad** (MAC o AEAD + números de secuencia) | no usa **KDC**: usa **PKI** |
| **Autenticación** del servidor (siempre) y del cliente (**opcional**: mTLS) | |

Estos tres renglones son el V/F que se repite en 1C-2018 y 1C-2023.

**Versiones**: SSL 2.0 y 3.0 prohibidos; TLS 1.0 y 1.1 deprecados (2021); **TLS 1.2** vigente como *fallback*; **TLS 1.3** (2018) el que se usa. TLS **desciende** de SSL 3.0 (en el cable, TLS 1.2 es la versión `3.3`).

**Record**: fragmenta ($\le 2^{14}$ bytes; la slide dice $2^{16}$), comprimía, agrega MAC, cifra y pone un header. **Tipos de mensaje**: Handshake, Application data, Alert, Change Cipher Spec.

**Cipher suite**: TLS no fija los algoritmos; los **negocia** (intercambio de claves, cifrado, integridad, autenticación), porque cualquier elección puede volverse obsoleta. **El cliente ofrece, el servidor elige** (versión y suite): es el que conoce su política.

**Sesión vs conexión**: la sesión guarda el *master secret* (48 bytes) y permite reanudar sin repetir el handshake pesado; cada conexión deriva **claves distintas por dirección** (evita reflejar mensajes) y lleva **números de secuencia** dentro del MAC (detectan replay, omisión y reordenamiento).

**Handshake (TLS 1.2), la idea**:

1. **Hello**: el cliente manda versiones, suites y un nonce $r_1$; el servidor elige y manda $r_2$.
2. **Servidor**: su **certificado**; si usa DH, un *ServerKeyExchange* con los parámetros **firmados con su privada** junto con $r_1, r_2$; opcionalmente pide certificado al cliente.
3. **Cliente**: el **pre-master secret** (con RSA: aleatorio, cifrado con la **pública** del servidor; con DH: su $g^b$, en claro). Si mandó certificado, *CertificateVerify* (firma de los mensajes: prueba que tiene la privada).
4. Los dos derivan el **master secret** de pre-master + $r_1$ + $r_2$ → *Change Cipher Spec* → **Finished**: MAC con el master de **todos** los mensajes del handshake, ya cifrado.

> [!important] Las preguntas de "¿para qué sirve…?"
> - **Nonces $r_1, r_2$**: frescura. Impiden reusar un handshake o un ServerKeyExchange viejo.
> - **Pre-master → master**: la clave de sesión depende de nonces **de los dos lados**, así que es **fresca** aunque se repita el pre-master (el *key reuse* de Needham-Schroeder), y se separan claves por uso y dirección.
> - **Finished**: **confirmación de clave** + integridad de **todo el handshake**, que viajó por un canal todavía inseguro. Si el atacante sacó suites o bajó la versión, los Finished no coinciden (mientras no pueda calcular el master).
> - **CertificateRequest**: autenticar al **cliente** (mTLS, típico servidor a servidor).
> - **Asimétrico solo en el handshake**: el contenido va con simétrico porque el asimétrico es órdenes de magnitud más lento (esquema híbrido).

> [!tip] Forward secrecy
> Con intercambio **RSA**, el pre-master viaja cifrado con la clave del certificado: si años después se roba esa privada, se descifra **todo** lo grabado. Con **DH efímero** (DHE/ECDHE), $a$ y $b$ se descartan al terminar y robar la clave del certificado no sirve para el pasado. Por eso **TLS 1.3 solo acepta (EC)DHE**.

| | TLS 1.2 (slides) | TLS 1.3 |
|---|---|---|
| *Round trips* | 2 | **1** |
| Intercambio | RSA, DH, DHE, ECDHE… | solo **(EC)DHE** o PSK |
| Cifrado | CBC + HMAC, RC4, AEAD… | solo **AEAD** (5 suites) |
| Compresión | sí | **no** (CRIME) |
| Firma del servidor | sobre $r_1, r_2$ y los parámetros | sobre **toda** la transcripción |
| Renegociación / Change Cipher Spec | sí | eliminados |

**Alert**: *fatal* (MAC incorrecto, mensaje inesperado: se corta) o *advertencia* (problemas de certificado: decide el receptor, por eso el navegador pregunta). **CloseNotify** evita que un corte del atacante haga pasar un mensaje **truncado** por completo.

> [!bug] Lo que las slides de TLS tienen mal (detalle en [Protocolos](Protocolos.md))
> - El ServerKeyExchange lleva una **firma** con la privada del servidor, no un MAC (todavía no hay clave compartida), y en TLS 1.2 **no cubre** la versión ni la suite (de ahí Logjam).
> - El ClientKeyExchange aparece "cifrado con $K_s$", que la slide define como la **privada** del servidor: con RSA es con la **pública**; con DH, $g^b$ va en claro.
> - La fórmula del master (`'A'`, `'BB'`, `'CCC'`) y el Finished con `ipad`/`opad` son de **SSL 3.0**; TLS 1.2 usa una PRF con HMAC-SHA256 y TLS 1.3, HKDF.

### 5.5 Criptografía de umbrales: Shamir

Un esquema **$(t, n)$**: se generan $n$ **sombras** y **$t$ cualesquiera** alcanzan para recuperar el secreto; con $t-1$, nada. Sirve para que una tarea crítica **no la pueda hacer una sola entidad** (las "dos llaves" del misil, pero con garantía criptográfica), para respaldos y para jerarquías (más sombras a quien tiene más poder).

**Construcción** (Shamir, 1979): primo $p > s$, $p > n$;
$$P(x) = s + a_1 x + \dots + a_{t-1}x^{t-1} \bmod p \qquad a_i \leftarrow \mathbb{Z}_p \qquad \text{sombras: } (i, P(i)),\ i = 1,\dots,n$$
El secreto es el **término independiente**: $P(0) = s$ (por eso nunca se reparte $i = 0$). Se basa en que un polinomio de **grado $d$ queda determinado por $d+1$ puntos**: umbral $t$ ⇒ grado $t-1$.

**Reconstrucción** con $t$ sombras $(x_a, y_a)$: interpolación de Lagrange evaluada en 0,
$$s = \sum_{a=1}^{t} y_a \prod_{b \neq a} \frac{-x_b}{x_a - x_b} \pmod p ,$$
donde dividir es multiplicar por el inverso módulo $p$ (por eso $p$ tiene que ser primo: $\mathbb{Z}_p$ es cuerpo).

> [!success] Por qué es seguro: secreto perfecto
> Con $t-1$ sombras, para **cada** valor posible $s' \in \mathbb{Z}_p$ existe **exactamente un** polinomio de grado $t-1$ que pasa por esas sombras y vale $s'$ en 0. Todos los secretos siguen igual de probables: seguridad **incondicional**, como el OTP, sin depender de ningún problema difícil. (Verificado en [Protocolos](Protocolos.md) para el ejemplo $(3,5)$ mod 11.)

> [!bug] Dos errores de la slide
> - "Un polinomio de grado $t$ se especifica con $t$ puntos" → hacen falta $t+1$; y para umbral $t$ el grado es $t-1$ (el propio ejemplo $(3,5)$ usa grado 2).
> - En el ejemplo $P(x) = 5x^2+3x+7 \bmod 11$, $P(4) = 99 \equiv \mathbf{0}$, no 2: toda terna con la sombra $(4,2)$ reconstruye mal.

**Shamir en sentido estricto es *secret sharing***: el que tiene el secreto no "cifra" con ninguna clave, arma el polinomio y las sombras son la salida. Los criptosistemas de umbral propiamente dichos (ElGamal o RSA con umbral) permiten que $t$ partes descifren juntas **sin reconstruir nunca la clave** en un lugar.

---
## 6. Tablas transversales

### 6.1 Qué servicio da cada herramienta

| Herramienta | Confidencialidad | Integridad | Autenticación de origen | No repudio | Frescura (anti-replay) |
|---|---|---|---|---|---|
| Cifrado CPA (flujo, CBC, CTR…) | ✅ | ❌ maleable | ❌ | ❌ | ❌ |
| MAC | ❌ | ✅ | ✅ entre quienes comparten $k$ | ❌ | ❌ |
| Hash solo | ❌ | solo si el digest llega por un canal íntegro | ❌ | ❌ | ❌ |
| Cifrado autenticado (EtM, GCM, CCM) | ✅ | ✅ | ✅ simétrica | ❌ | ❌ |
| Cifrado asimétrico | ✅ | ❌ cualquiera cifra con $pk$ | ❌ | ❌ | ❌ |
| Firma digital | ❌ | ✅ | ✅ verificable por cualquiera | ✅ | ❌ |
| Diffie-Hellman puro | acuerda una clave contra un **pasivo** | ❌ | ❌ | ❌ | — |
| Certificado | — | de la asociación identidad ↔ clave | de la **clave** | — | — |
| Nonce / timestamp / nº de secuencia | — | — | — | — | ✅ |
| TLS | ✅ | ✅ | ✅ servidor; cliente opcional | ❌ | ✅ |

> [!tip] La regla para el V/F
> **Confidencialidad** sale de cifrar; **integridad y autenticación**, de un MAC o una firma; **no repudio**, solo de una firma (clave que tiene uno solo); **frescura**, de ninguna primitiva: del protocolo.

### 6.2 Todos los experimentos

| Experimento | Qué recibe el adversario | Gana si | Seguro si |
|---|---|---|---|
| **EAV** | $c = e_k(m_b)$ | $b' = b$ | $\le \tfrac12 + negl$ |
| **MUL** | cifrados de un vector de mensajes, con la misma $k$ | $b' = b$ | $\le \tfrac12 + negl$ |
| **CPA** | oráculo $e_k(\cdot)$ + desafío | $b' = b$ | $\le \tfrac12 + negl$ |
| **CCA** | oráculos $e_k(\cdot)$ y $d_k(\cdot)$ (este último, salvo en $c$) + desafío | $b' = b$ | $\le \tfrac12 + negl$ |
| **KE** | *transcript* + $k'$ (la real o una al azar) | $b' = b$ | $\le \tfrac12 + negl$ |
| **EAV asimétrico** (= CPA) | $pk$ + desafío | $b' = b$ | $\le \tfrac12 + negl$ |
| **Mac-forge** | oráculo $Mac_k(\cdot)$ | $(m, t)$ válido con $m \notin Q$ | $\le negl$ |
| **Sig-forge** | $pk$ + oráculo $Sign_{sk}(\cdot)$ | $(m, s)$ válido con $m \notin Q$ | $\le negl$ |
| **Hash-coll** | el selector $s$ | $x \neq x'$ con $H_s(x) = H_s(x')$ | $\le negl$ |
| **Secreto perfecto** (forma 4) | $c = e_k(m_b)$, **sin límite de cómputo** | $b' = b$ | $= \tfrac12$ **exacto** |

### 6.3 Sobre qué supuesto descansa cada cosa

| Supuesto | Qué se asume | Qué depende de él |
|---|---|---|
| **Ninguno** (incondicional) | — | OTP, Shamir |
| Existen **PRG / PRF / PRP** (no demostrado) | AES y compañía se comportan como PRP | flujo, modos de bloque, CBC-MAC, HMAC (compresión como PRF) |
| **Resistencia a colisiones** | no se encuentran $x \neq x'$ con igual hash | Merkle-Damgård, hash-and-sign (Hashed RSA, DSA), certificados |
| **DDH** | $g^{xy}$ indistinguible de un $g^z$ | Diffie-Hellman, ElGamal |
| **DL** | no se calcula $x$ desde $g^x$ | DSA / ECDSA |
| **Problema RSA** / factorización | no se calculan raíces $e$-ésimas mod $n$ | RSA (cifrado y firma) |
| **Relojes sincronizados** | $\lvert\text{Clock} - T\rvert$ acotado | timestamps (Denning-Sacco, Kerberos) |
| **Raíces confiables** | el almacén local no fue alterado | toda la PKI, TLS |

### 6.4 Lo que no se repite nunca

| Qué | Dónde | Si se repite… |
|---|---|---|
| La clave del OTP | OTP | $c_1 \oplus c_2 = m_1 \oplus m_2$ |
| $G(k)$ sin IV | flujo | lo mismo: pierde MUL/CPA |
| El par $(k, \text{nonce})$ | CTR, OFB, GCM, CCM | keystream repetido (y en GCM, tags forjables) |
| Un IV **predecible** | CBC | pierde CPA |
| El $y$ efímero | ElGamal | $c_2 / c_2' = m_1 / m_2$ |
| El $k$ de firma | DSA / ECDSA | se despeja la **clave privada** |
| La misma clave para cifrar y para el MAC | Encrypt-then-MAC genérico | se cae la prueba |
| El mismo par para cifrar y firmar | RSA | firmar = descifrar |
| La misma clave en las dos direcciones | TLS | reflexión de mensajes |
| Una $k_s$ de vida corta | Needham-Schroeder | Denning-Sacco |
| Un nonce de desafío | *challenge-response* | replay |

### 6.5 Implicaciones que conviene tener a mano

| Implicación | ¿Vale la vuelta? |
|---|---|
| CCA ⇒ CPA ⇒ MUL ⇒ EAV | no: flujo con $G(k)$ fijo es EAV y no MUL; CBC es CPA y no CCA |
| Determinístico ⇒ no CPA (ni MUL) | no: no determinístico no implica seguro ($Enc_k(\lvert m\rvert)$ como MAC) |
| Maleable ⇒ no CCA | — |
| Secreto perfecto ⇒ $\lvert K\rvert \ge \lvert M\rvert$ | no: la sustitución sobre mensajes de 2 letras tiene $26! \ge 26^2$ claves y no tiene secreto perfecto |
| $G$ es PRG ⇔ el flujo con $G$ es EAV-seguro | sí (§2.5, las dos direcciones) |
| Resistencia a colisiones ⇒ a segundas preimágenes ⇒ a preimágenes | no |
| DDH difícil ⇒ CDH difícil ⇒ DL difícil | no en general: en $\mathbb{Z}_p^*$ entero DDH es fácil y CDH se cree difícil (§4.3) |
| Factorizar ⇒ romper RSA | no se sabe si romper RSA ⇒ factorizar |
| Conocer $\varphi(n)$ ⇔ factorizar $n$ | sí |
| CPA + MAC (fuerte) + claves independientes, EtM ⇒ CCA | — |
| Asimétrico: EAV ⇒ CPA | sí (el oráculo es la $pk$) |
| Firma ⇒ no repudio; MAC ⇏ no repudio | — |

---
## 7. Autoevaluación

Preguntas del tipo que toma la cátedra. Intentar responder en voz alta antes de abrir.

> [!question]- 1. ¿Por qué el OTP tiene secreto perfecto y qué pasa si se reusa la clave?
> Para cada par $(m, c)$ hay **una sola** clave compatible, $k = m \oplus c$, y todas las claves son equiprobables: $\Pr[C=c \mid M=m] = 2^{-n}$ para todo $m$, que es la forma 3. Si se reusa, $c_1 \oplus c_2 = m_1 \oplus m_2$: la clave desaparece y queda una relación entre planos (y un par conocido revela el otro). Además la clave tiene que ser uniforme, del largo del mensaje y secreta.

> [!question]- 2. ¿Por qué el secreto perfecto es impráctico?
> Porque exige $\lvert K\rvert \ge \lvert M\rvert$: si hubiera menos claves que mensajes, dado un $c$ algún $m$ sería imposible y la posterior de ese $m$ sería 0. Para proteger $n$ bits hay que haber compartido $n$ bits secretos antes, y usarlos una sola vez. Por eso se pasa a seguridad computacional.

> [!question]- 3. ¿Qué relaja la seguridad computacional? ¿$1/n^{100}$ es despreciable?
> Limita al adversario a tiempo polinomial probabilístico y acepta una ventaja despreciable. $1/n^{100}$ **no** es despreciable: es la inversa de un polinomio, y la definición pide ser menor que $1/p(n)$ para **todo** polinomio a partir de algún $n$. $2^{-n}$ sí lo es.

> [!question]- 4. Un cifrado de flujo con un PRG seguro, ¿es CPA-secure?
> Así como está ($G(k) \oplus m$), es EAV-seguro para **un** mensaje, pero no MUL ni CPA: es determinístico y $c_1 \oplus c_2 = m_1 \oplus m_2$. Se arregla metiendo un IV/nonce que no se repite para la misma clave (modo sincronizado o no sincronizado).

> [!question]- 5. ¿Por qué un esquema determinístico no puede ser CPA-secure? ¿Y en asimétrico?
> Con el oráculo de cifrado el adversario calcula $e_k(m_0)$ y lo compara con el desafío: gana con probabilidad 1. En asimétrico ni siquiera hace falta el oráculo: el adversario tiene $pk$ y cifra solo. Por eso todo asimétrico seguro es probabilístico (PKCS#1, el $y$ de ElGamal), y por eso en asimétrico EAV ⇒ CPA.

> [!question]- 6. ¿Toda primitiva de bloque tiene que ser reversible?
> No. ECB y CBC usan $D_k$ y necesitan una permutación. CFB, OFB y CTR solo usan $E_k$ para generar un keystream que se XORea: alcanza con una PRF.

> [!question]- 7. ¿Qué pide cada modo de su IV? ¿Y CBC-MAC?
> CBC: aleatorio e **impredecible**. OFB/CFB: aleatorio, no repetido. CTR: que $(k, \text{nonce})$ **no se repita** (puede ser un contador). CBC-MAC: al revés, IV **fijo** en 0; si fuera aleatorio y viajara con el tag, se cambia el IV y el primer bloque a la vez y el tag sigue valiendo.

> [!question]- 8. ¿Por qué CPA no alcanza? Definir CCA.
> Porque un esquema CPA puede ser **maleable**: en flujo, $c \oplus x$ se descifra como $m \oplus x$, y el atacante cambia el plano sin leerlo. CCA le da al adversario, además del oráculo de cifrado, uno de **descifrado** que puede usar con todo salvo el desafío. Ningún esquema de las Clases 1 y 2 es CCA-secure.

> [!question]- 9. ¿Qué da un MAC y qué no?
> Da integridad y autenticación de origen entre quienes comparten la clave. No da confidencialidad (el mensaje va en claro), ni no repudio (los dos tienen $k$), ni protección contra replay (un mensaje repetido tiene tag válido).

> [!question]- 10. ¿Por qué CBC-MAC con largo variable es inseguro? ¿Cómo se arregla?
> Porque el tag de un prefijo es el estado intermedio de la cadena: con $t = Mac(m)$, el mensaje $m \,\|\, (m \oplus t)$ tiene el mismo tag. Arreglos: anteponer la longitud, derivar $k' = F_k(\lvert m\rvert)$, o recifrar el tag con otra clave. La longitud como sufijo **no** sirve.

> [!question]- 11. Ordenar las propiedades de un hash. ¿Por qué 256 bits?
> Colisión ⇒ segunda preimagen ⇒ preimagen: la más fuerte es la de colisiones. Con $\ell$ bits de salida, una preimagen cuesta $\sim 2^\ell$ y una colisión $\sim 2^{\ell/2}$ (cumpleaños). Para 128 bits de seguridad contra colisiones hacen falta 256 bits de salida.

> [!question]- 12. ¿Por qué $H(k\,\|\,m)$ no es un buen MAC y HMAC sí?
> Con Merkle-Damgård, quien conoce $H(k \,\|\, m)$ puede extender la cadena y calcular $H(k \,\|\, m \,\|\, pad \,\|\, m')$ sin la clave (*length extension*). HMAC anida dos hashes con $k \oplus ipad$ y $k \oplus opad$: el atacante nunca ve el hash interno.

> [!question]- 13. ¿En qué orden se combinan cifrado y MAC?
> **Encrypt-then-MAC**: $c = Enc_{k_1}(m)$, $t = Mac_{k_2}(c)$, con claves **independientes**, verificando **antes** de descifrar. Con Enc CPA-secure y un MAC (fuertemente) infalsificable, el resultado es CCA-secure. Encrypt-and-MAC puede filtrar información de $m$ por el tag; MAC-then-Encrypt requiere prueba caso por caso.

> [!question]- 14. ¿Qué resuelve Diffie-Hellman, sobre qué supuesto y qué problema tiene?
> Permite **acordar** una clave sobre un canal público sin secreto previo. Es seguro contra atacantes **pasivos** si DDH es difícil. No autentica a nadie: cae con un MITM. Se arregla firmando $(g^x, g^y)$ con las identidades y certificados.

> [!question]- 15. ¿Por qué no alcanza con que el logaritmo discreto sea difícil?
> Porque el atacante no necesita $x$: le alcanza con sacar **información** de $g^{xy}$ (aunque sea un bit) para ganar el experimento $KE$. Lo que hace falta es que $g^{xy}$ sea **indistinguible** de un valor al azar (DDH). DL difícil es necesario pero no suficiente.

> [!question]- 16. Problemas de textbook RSA. ¿Para qué es el padding de PKCS#1 v1.5?
> Es determinístico (no CPA), $m^e < n$ se invierte con una raíz entera, el módulo compartido expone las privadas, y es maleable ($c \cdot r^e \mapsto m \cdot r$, no CCA). El padding aleatorio lo hace **no determinístico** → CPA. No lo hace CCA (Bleichenbacher); para eso está OAEP.

> [!question]- 17. ¿Por qué ElGamal es CPA-secure y por qué no es CCA-secure?
> CPA: $h^y = g^{xy}$ es indistinguible de un elemento al azar (DDH), así que $h^y \cdot m$ funciona como un OTP en el grupo, con $y$ nuevo en cada cifrado. No CCA: $(c_1, c_2 \cdot r)$ se descifra como $m \cdot r$; el oráculo lo descifra y se divide por $r$.

> [!question]- 18. Firma vs MAC. ¿Por qué solo la firma da no repudio?
> Los dos dan integridad y autenticación. La firma además es verificable por cualquiera y transferible, y da **no repudio** porque solo el firmante tiene $sk$; con un MAC la clave la tienen las dos partes y cualquiera pudo generar el tag. Ninguno frena el replay.

> [!question]- 19. ¿Por qué Hashed RSA y no textbook RSA para firmar?
> Textbook RSA permite forjar sin ver firmas ($s$ al azar, $m = s^e$) y combinar firmas ($s_1 s_2$ firma $m_1 m_2$). Con $H(m)^d$: la primera exige una preimagen de $s^e$ y la segunda se rompe porque $H(m_1)H(m_2) \neq H(m_1 m_2)$. Además permite firmar mensajes de cualquier largo. Requiere $H$ resistente a colisiones.

> [!question]- 20. ¿Qué es un certificado y qué se verifica al validarlo?
> La clave pública del titular + su identidad + validez + uso + emisor, firmado con la **privada de la CA**. Se verifica: la firma de la CA (subiendo por la cadena hasta una raíz local), la vigencia, la identidad esperada, el uso y que no esté revocado. Nunca contiene claves privadas.

> [!question]- 21. ¿Para qué sirve la revocación y qué problema trae?
> Para invalidar un certificado antes de que venza (privada comprometida, empleado que se fue). Rompe la validación offline: hay que consultar una CRL u OCSP. OCSP Stapling traslada el costo al servidor, que abrocha una respuesta firmada reciente.

> [!question]- 22. Needham-Schroeder: ¿para qué $r_1$, para qué los nombres en el mensaje 2, para qué 4 y 5, qué problema tiene?
> $r_1$: que el mensaje 2 sea fresco (evita key reuse). Los nombres adentro de 2: que no se cambie $B$ por $E$ en el mensaje 1, que viaja en claro. 4 y 5: *challenge-response*, prueba de que el otro conoce $k_s$ y de frescura. Problema (Denning-Sacco): $B$ no puede saber si el **token** es fresco; con una $k_s$ vieja se reinyecta el mensaje 3. Arreglo: timestamp en el token (o un nonce de $B$ que viaje al KDC).

> [!question]- 23. ¿Nonce o timestamp?
> Nonce: no necesita relojes, pero exige una ida y vuelta (quien verifica tiene que haberlo generado antes). Timestamp: sirve en un solo mensaje, pero necesita relojes sincronizados y una tolerancia.

> [!question]- 24. TLS: ¿qué da, por qué deriva el master y para qué sirve el Finished?
> Confidencialidad, integridad y autenticación del servidor (el cliente, opcional) con PKI; no da no repudio ni usa KDC. El master sale del pre-master **y de los nonces de los dos lados**: clave fresca en cada handshake y separada por uso. El Finished es un MAC de todo el handshake con el master: confirma que los dos derivaron la misma clave y detecta cualquier manipulación de los mensajes que viajaron en claro.

> [!question]- 25. ¿Qué es forward secrecy y cómo se logra?
> Que comprometer una clave de largo plazo **después** no permita descifrar sesiones **pasadas**. Se logra con DH **efímero**: los exponentes se descartan. Con intercambio RSA no hay forward secrecy, porque el pre-master viaja cifrado con la clave del certificado. TLS 1.3 solo acepta (EC)DHE.

> [!question]- 26. Shamir $(t, n)$: ¿por qué grado $t-1$ y por qué es seguro?
> Un polinomio de grado $d$ queda determinado por $d+1$ puntos: para que $t$ sombras alcancen, el grado es $t-1$. Con $t-1$ sombras, cada valor posible del secreto corresponde a exactamente un polinomio compatible: todos siguen igual de probables. Es secreto perfecto, incondicional.

---

## 8. Errores de las slides que pueden aparecer

Todos verificados en las notas de clase; acá solo la versión corta.

| Clase | La slide dice | Lo correcto |
|---|---|---|
| 1 | "1939: Bombe – Shannon" | la Bombe es de Turing (1939); Shannon publica en **1949** |
| 2 | OTP "atribuido a **Verman**" | **Vernam** (Gilbert Vernam, 1917) |
| 2 | Paradoja: "$C$ y **$E$** son dependientes" | $C$ y **$M$** |
| 2 | ShiftRows: "permutación de bits" | desplazamiento cíclico de **bytes** por fila |
| 3 | $opad = \texttt{0x36}$, $ipad = \texttt{0x5C}$ | al revés: **$ipad = \texttt{0x36}$**, **$opad = \texttt{0x5C}$** |
| 3 | SHA-3 estandarizado en 2013, entrada $< 2^{64}$ bits | FIPS 202 en **2015**; esponja, entrada de largo arbitrario |
| 3 | SHA-1 como vigente | **roto** (2017); NIST lo retira en 2030 |
| 4 | DL / DDH "es NP-hard" | **no se sabe** (y se cree que no); "se conjetura difícil" |
| 4 | $\mathbb{Z}_n = \{1, \dots, n-1\}$ | falta el **0** |
| 4 | "Campo canónico" de tamaño $p-1$ | eso es el **grupo** $\mathbb{Z}_p^*$; el cuerpo es $\mathbb{Z}_p$ |
| 4 | Teorema de Euler sin hipótesis | pide $\gcd(a, n) = 1$ |
| 4 | RSA: "se puede calcular el logaritmo" | la **raíz $e$-ésima** |
| 4 | PKCS#1: "hasta $n$−11 bytes" | **$k$−11**, con $k$ = bytes de $n$ |
| 4 | RSA-Signature: "$d \leftarrow$ … $\gcd(e, \varphi) = 1$" | el que se sortea es **$e$** |
| 4 | DSS como estándar vigente, con SHA-1 | FIPS 186-5 (2023) sacó DSA para firmar; SHA-1 prohibido para firmas |
| 4 | (clase) Shor amenaza la factorización, no el DL | resuelve **los dos** |
| 4 | RSA ≥ 1024 bits, ECC ≥ 320 | piso **2048** (3072 para 128 bits); ECC **256** |
| 5 | Shamir: grado $t$ con $t$ puntos | grado $d$ necesita $d+1$ puntos; umbral $t$ ⇒ grado **$t-1$** |
| 5 | Shamir: $P(4) = 2$ | $P(4) = 99 \bmod 11 = \mathbf{0}$ |
| 5 | Lagrange con $b \neq s$ | $b \neq a$ |
| 5 | TLS record: bloques de hasta $2^{16}$ | **$2^{14}$** bytes |
| 5 | ServerKeyExchange con "hash con clave" (MAC) | es una **firma** con la privada del servidor |
| 5 | ClientKeyExchange cifrado con $K_s$ (privada del servidor) | con la **pública** (RSA); con DH, $g^b$ va en claro |
| 5 | Master con `'A'`, `'BB'`, `'CCC'` como fórmula de TLS | es de **SSL 3.0**; TLS 1.2 usa una PRF, TLS 1.3 HKDF |
| 5 | "OCSP es un certificado" | es otra estructura firmada (`BasicOCSPResponse`) |
| 5 | Modificación "**Demming**-Sacco" | **Denning**-Sacco (1981) |

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Criptografía y Seguridad)**

- [Cripto - Resumen Primer Parcial](Cripto%20-%20Resumen%20Primer%20Parcial.md) — la contraparte práctica: recetas paso a paso, parciales resueltos y el banco de V/F; esta nota es la teoría que las justifica
- [Criptografia y seguridad intro](Criptografia%20y%20seguridad%20intro.md) — Clase 1: criptosistema, Kerckhoffs y los clásicos, desarrollados en §1
- [Criptografia y seguridad Clase 2 - Cifrado](Criptografia%20y%20seguridad%20Clase%202%20-%20Cifrado.md) — Clase 2: secreto perfecto, OTP, PRG, EAV/CPA y modos de bloque, condensados en §2
- [Clase 3 - Criptografia - MACs y modo autenticado](Clase%203%20-%20Criptografia%20-%20MACs%20y%20modo%20autenticado.md) — Clase 3: CCA, MACs, CBC-MAC, hash, HMAC y AEAD, con los ataques implementados; base de §3
- [Clase 4 - Criptografía - Cifrado asimétrico y Firma digital](Clase%204%20-%20Criptografía%20-%20Cifrado%20asimétrico%20y%20Firma%20digital.md) — Clase 4: álgebra, DH, RSA, ElGamal, firmas y el mini-resumen de la Guía 4; base de §4
- [Protocolos](Protocolos.md) — Clase 5: PKI, Needham-Schroeder, TLS y Shamir en detalle; base de §5
- [Guia 1 - criptografia y seguridad](Guia%201%20-%20criptografia%20y%20seguridad.md) — ejercicios de clásicos, la práctica de §1.2
- [Materia - Criptografía y Seguridad](Materia%20-%20Criptografía%20y%20Seguridad.md) — índice de la materia

**Otras materias**

- **Machine Learning** — [ML Clase 6 - GDA y Naive Bayes](ML%20Clase%206%20-%20GDA%20y%20Naive%20Bayes.md) — el mismo teorema de Bayes con prior y posterior: el secreto perfecto es exactamente pedir que la posterior $\Pr[M \mid C]$ sea igual al prior $\Pr[M]$
- **EDA** — [EDA - Hashing](EDA%20-%20Hashing.md) — misma palabra, otra exigencia: al hash de una tabla le alcanza con repartir uniforme; al criptográfico se le pide que encontrar colisiones sea inviable (§3.5)
- **Protos** — [2. Protos - HTTP](2.%20Protos%20-%20HTTP.md) — HTTPS es TLS: certificados, (EC)DHE, Finished y AEAD de §5.4 funcionando juntos
- **Protos** — [9. Protos - SSH](9.%20Protos%20-%20SSH.md) — SSH hace el DH autenticado de §4.3 (firma del intercambio con la host key) sin PKI jerárquica
- **Discrete Math** — [Discrete Math - Caminos y Conexidad](Discrete%20Math%20-%20Caminos%20y%20Conexidad.md) — las $\frac{N(N-1)}{2}$ claves de a pares de §4.1 son las aristas del grafo completo $K_N$

<!-- notas-relacionadas:fin -->
