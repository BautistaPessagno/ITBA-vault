---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
Cuatri: "2C"
Created: 2026-09-18
temas:
  - primer parcial
  - repaso
  - análisis de protocolos
  - modos de cifrado en bloque
  - propagación de errores
  - secreto perfecto
  - experimento CPA
  - experimento EAV
  - criptoanálisis clásico
  - Vigenère
  - test de Kasiski
  - índice de coincidencia
  - MAC
  - CBC-MAC
  - funciones de hash
  - HMAC
  - Encrypt-then-MAC
  - Diffie-Hellman
  - RSA
  - firma digital
  - certificados X.509
  - TLS
  - Needham-Schroeder
  - Shamir
  - verdadero o falso
  - reconocer protocolos
  - ataque de reflexión
  - masquerading
  - misbinding de identidad
  - Needham-Schroeder de clave pública
  - ataque de Lowe
  - PCBC
  - IV predecible (BEAST)
  - invariantes sin clave
---
# Cripto - Resumen Primer Parcial

> [!abstract] Qué es esto
> El recorte de parcial armado desde los **cuatro primeros parciales resueltos** (2C-2025, 1C-2025, 1C-2023 y 1C-2018, en *Cripto - Primeros Parciales.pdf*), las **Guías 1 a 4** con sus soluciones, las prácticas y las notas de las **Clases 1 a 5**.
>
> Está ordenado por **lo que más se repite**. Cuando un tipo de ejercicio vuelve siempre y tiene receta, está como **algoritmo paso a paso**, con el ejercicio del parcial resuelto y las cuentas verificadas con Python. La teoría completa sigue en cada nota de clase.

> [!info] Alcance — verificado contra el cronograma 2C 2026
> **Primer parcial: jueves 24/09.** Entran las teóricas 1 a 5 y las Guías 1 a 4.
>
> | Clase | Tema | Guía | Nota |
> |---|---|---|---|
> | 1 | Intro, criptografía clásica | 1 | [[Criptografia y seguridad intro]] · [[Guia 1 - criptografia y seguridad]] |
> | 2 | Secreto perfecto, OTP, EAV/CPA, cifrado en bloque y modos | 2 | [[Criptografia y seguridad Clase 2 - Cifrado]] |
> | 3 | MAC, hash, cifrado autenticado | 3 | [[Clase 3 - Criptografia - MACs y modo autenticado]] |
> | 4 | Diffie-Hellman, RSA, ElGamal, firma digital | 4 | [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital]] |
> | 5 | PKI, Needham-Schroeder, TLS, Shamir | 4 | [[Protocolos]] |
>
> **No entra**: políticas y control de acceso, autenticación, flujo de información, vulnerabilidades (Clase 6 en adelante). Las prácticas *Clase 8* y *Clase 9* de la carpeta son del 1C 2026 y son de esos temas.

---

## 1. Cómo es el parcial

Los cuatro parciales tienen la **misma estructura de 5 ejercicios**, y la cátedra **recicla**: el 1C-2023 reusó literal cuatro ejercicios del 1C-2018 (CBC, el esquema $c=(r,\,ar+b+m)$, el V/F y el de SSL/TLS).

| Tema | 2C-2025 | 1C-2025 | 1C-2023 | 1C-2018 | Frec. | Dónde |
|---|---|---|---|---|---|---|
| **Analizar un protocolo** | Ej 1 (MACs + nonces) | Ej 1 (Diffie-Hellman) | Ej 1.1 (handshake tipo TLS) | Ej 1 (Needham-Schroeder) | **4/4** | §2 |
| **Modo de bloque "inventado"** | Ej 3 | Ej 2 | Ej 1.2 (CTR) y Ej 2 (CBC) | Ej 3 (CBC) | **4/4** | §3 |
| **V/F con corrección** | Ej 5 | Ej 5 | Ej 1.3 y 5 | Ej 2 y 5 | **4/4** | §10 |
| Secreto perfecto | Ej 4 (Vigenère) | Ej 4 (XOR de 1 bit) | — | Ej 2.2 (homofónico) | 3/4 | §4 |
| CPA sobre un esquema dado | — | Ej 2b | Ej 4 | Ej 4 | 3/4 | §5 |
| Clásicos / criptoanálisis | Ej 2 (Vigenère) | Ej 5c (contar claves) | Ej 3 (Base64) | Ej 2.2 | 4/4 | §6 |
| MAC / hash / composición | Ej 5a-b | Ej 3 | Ej 5a-b | Ej 5a-b | 4/4 | §7 |
| Asimétrico / PKI / TLS | Ej 5c-d | Ej 5b, 5d | Ej 1.1, 1.3, 5c | Ej 2.1, 2.3, 5c | 4/4 | §8 |
| Shamir | — | — | — | — | nuevo | §9 |

> [!tip] Lectura estratégica
> - **Los ejercicios 1 y 2 son fijos**: un protocolo para analizar y un encadenamiento para romper. Los dos tienen receta (§2 y §3).
> - **El V/F se repite casi textual**: con las frases de §10 y §12 está cubierto.
> - Lo que rota es el 3/4: secreto perfecto, CPA, un clásico o un MAC.
> - Shamir no apareció nunca (en esos años era de la Guía 6), pero este cuatrimestre se dio en la Clase 5, antes del parcial.
> - Para **reconocer** de qué protocolo o modo se trata y demostrar fugas sin la clave, ver el catálogo de **§13**.

---

## 2. Algoritmo A — Analizar un protocolo (4/4)

Las preguntas son siempre las mismas: *¿qué tipo de protocolo es y qué intenta construir? ¿para qué sirve el mensaje X? ¿es vulnerable a MITM / replay? ¿qué problemas tiene?*

> [!example] Receta
> 1. **Clasificar.** ¿Qué construye? → una clave de sesión, autenticación (¿de uno o mutua?), o un canal seguro (las dos cosas). ¿Con qué? → secreto precompartido, un tercero de confianza (KDC/T) o claves públicas con certificados.
> 2. **Mensaje por mensaje**, anotar *quién puede generarlo* (qué clave hace falta), *qué le prueba al receptor* y *qué lleva adentro*. Un mensaje tiene que estar **atado** a (a) quién lo manda, (b) a quién va, (c) a qué sesión pertenece y (d) cuándo se generó. Lo que falte es el ataque.
> 3. **Frescura**: ¿el receptor ve algo que generó **él** (su nonce) o un timestamp? Si no → **replay**.
> 4. **Probar los cuatro ataques** de la Guía 4: *replay*, *key reuse*, *man in the middle*, *masquerading* (y reflexión, si el protocolo es simétrico y las dos partes hacen lo mismo).
> 5. **Propiedades**: confidencialidad, integridad, autenticación (¿de quién?), **no repudio (solo con firmas)**, *forward secrecy*.
> 6. **Arreglo**: agregar lo que faltaba — nonce propio, timestamp, identidades adentro de lo cifrado/MAC/firma, firmas sobre los valores de DH.

| Herramienta | Para qué sirve |
|---|---|
| Nonce propio que vuelve ($N$, $N-1$, $h_K(N,\dots)$) | frescura + prueba de que el otro tiene la clave (*challenge-response*) |
| Timestamp | frescura sin una ronda extra (requiere relojes sincronizados) |
| Identidades adentro de lo cifrado/firmado | que no se pueda redirigir ni reflejar el mensaje |
| Derivar la clave de sesión con nonces de las dos partes | clave nueva en cada sesión aunque el secreto de base se repita |
| Firmas / certificados | atar una clave pública a una identidad → frenan MITM |

Las fichas de cada protocolo, la tabla de señales y el guion de cada ataque están en **§13.1–13.5**. El detalle de cada ataque está en [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital#Mini-resumen para la Guía 4|el mini-resumen de la Guía 4]].

### 2C-2025 — autenticación mutua con MACs

$A$ y $B$ comparten $K$ y $K'$; $h'_{K'}$ es un MAC distinto de $h_K$.

$$\begin{aligned}
&(1.1)\; A\to B: && r_A\\
&(1.2)\; A\leftarrow B: && (B,A,r_A,r_B),\; h_K(B,A,r_A,r_B,K')\\
&(1.3)\; A\to B: && (A,r_B),\; h_K(A,r_B,K')\\
&(1.4)\text{–}(1.5)\; A, B: && W = h'_{K'}(r_B)
\end{aligned}$$

- **a)** Autenticación **mutua** + establecimiento de una clave de sesión $W$, con claves simétricas precompartidas (sin tercero).
- **b)** Son *challenge-response* cruzados. En 1.2, $B$ responde al desafío $r_A$: $A$ recalcula el MAC con $K$ y sabe que es $B$ y que el mensaje es fresco. En 1.3, $A$ responde a $r_B$ y $B$ hace lo mismo. Las identidades adentro del MAC evitan la reflexión.
- **c)** No es vulnerable a MITM mientras $K$ y $K'$ sean secretas: sin $K$ no se fabrica un MAC válido sobre nonces nuevos, y reenviar los mensajes sin tocarlos no da $W$ (hace falta $K'$).
- Suma decir el punto débil: **no hay forward secrecy**. Si mañana se filtra $K'$, todas las $W$ pasadas se recalculan con los $r_B$ que viajaron en claro.

### 1C-2025 — Diffie-Hellman

$A$ elige $(G,q,g)$, $x \leftarrow \mathbb{Z}_q$, manda $h_1=g^x$; $B$ elige $y$, manda $h_2=g^y$; $k_A = h_2^{\,x}$, $k_B = h_1^{\,y}$.

- **a)** Intercambio de claves de Diffie-Hellman: acordar un secreto $g^{xy}$ sobre un canal inseguro sin haber compartido nada antes.
- **b)** Ejemplo y valores válidos de $q$ → abajo.
- **c)** La seguridad está en que, viendo $g$, $g^x$ y $g^y$, no se pueda calcular $g^{xy}$ (CDH) **ni distinguirlo de un valor al azar (DDH)**, que es la hipótesis que realmente hace falta. Que el logaritmo discreto sea difícil es necesario pero **no suficiente** (ver [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital#La seguridad Diffie-Hellman|la tabla DL/CDH/DDH]]).
- **d)** Problemas: (1) **MITM** — no autentica a nadie, solo resiste atacantes pasivos; se arregla firmando $(g^x,g^y)$ con certificados (Guía 4, ej 7). (2) Los parámetros $(G,q,g)$ los elige $A$ y viajan **sin autenticar**: con $q$ chico o $g$ que no genera el grupo, el logaritmo discreto es fácil. (3) Nada ata $g^x$ a la identidad de $A$ → *masquerading*.

> [!example] Ejemplo numérico hecho a mano
> $p = 23$, $g = 5$. **¿Es generador?** $p-1 = 22 = 2\cdot 11$, así que alcanza con chequear los divisores primos: $5^{22/2} = 5^{11} \equiv 22 \neq 1$ y $5^{22/11} = 5^{2} \equiv 2 \neq 1$ ✓.
>
> **Cuadrados sucesivos**: $5^2 \equiv 2$, $5^4 \equiv 4$, $5^8 \equiv 16 \pmod{23}$.
> - $A$: $x = 6$ → $h_1 = 5^6 = 5^4\cdot 5^2 \equiv 4\cdot 2 = 8$.
> - $B$: $y = 15$ → $h_2 = 5^{15} = 5^8\cdot 5^4\cdot 5^2\cdot 5 \equiv 16\cdot4\cdot2\cdot5 = 640 \equiv 19$.
> - $k_A = 19^6 \bmod 23 = 2$ y $k_B = 8^{15} \bmod 23 = 2$ ✓.

**¿Qué $q$ son válidos?** Un **primo** (así $\mathbb{Z}_q^{*}$ es cíclico y existe una raíz primitiva $g$) y **grande** (hoy ≥ 2048 bits). Mejor todavía un *safe prime* $q = 2q'+1$ con $q'$ primo: si el orden del grupo $q-1$ tuviera solo factores chicos, el logaritmo discreto se resuelve por partes (Pohlig-Hellman). Los exponentes se eligen en $\{1,\dots,q-2\}$: $x=0$ y $x=q-1$ dan $g^x=1$.

> [!bug] El ejemplo de la resolución del PDF tiene mal una cuenta
> Con $\mathbb{Z}_5$, $g=2$, $x=3$ escribe $h_1 = 2^3 = 6 \equiv 1$. Es $2^3 = 8 \equiv \mathbf{3} \pmod 5$. Además elige $y = 4 = p-1$, con lo que $h_2 = 2^4 \equiv 1$ y la clave vale 1 para **cualquier** $x$: el ejemplo es degenerado y $k_A = k_B = 1$ coincide de casualidad. Verificado con Python.
>
> Y como "segundo problema" pone que la exponenciación modular es costosa: es un tema de rendimiento, no de seguridad del protocolo. Conviene responder con los parámetros sin autenticar o la falta de identidades.

### 1C-2023 — handshake tipo TLS y 1C-2018 — Needham-Schroeder

Están resueltos en detalle en [[Protocolos#Preguntas de parciales anteriores]]. Lo que hay que escribir:

| Ejercicio | Respuesta |
|---|---|
| **1C-2023 1.1a)** ¿Qué tipo de protocolo? | Intercambio de claves **autenticado** (el servidor, con certificado) para armar un **canal seguro**: la estructura del handshake de TLS |
| **1C-2023 1.1b)** ¿Para qué 1.4 y 1.5? | Son los *Finished*: **confirmación de clave** (cada lado prueba que derivó la misma $K_{cs}$ y conoce $k_1$), el MAC da integridad y el timestamp, frescura |
| **1C-2023 1.1c)** ¿Por qué derivar $K_{cs}=H(N,\,H(K_0,N_C,N_S))$ y no usar $K_0$? | Para que la clave de sesión sea **fresca**: depende de nonces de **las dos** partes, así que repetir un 1.3 viejo da otra clave (sin replay ni *key reuse*). Y para **separar claves** por uso ($k_1$ para el MAC, $K_{cs}$ para cifrar). $K_0$ es el *pre-master secret* |
| **1C-2018 a)** ¿Por qué el nombre de $B$ en 1.1 y 1.2? | En 1.1, para que $T$ sepa con quién armar la clave. Repetido **cifrado** en 1.2, para que $A$ detecte si alguien cambió $B$ por $E$ en 1.1 (que viaja en claro) |
| **1C-2018 b)** ¿Qué problema tiene? | **$B$ no puede saber si el ticket $E_{K_{BT}}(k,A)$ es fresco.** Con una $k$ vieja comprometida, el atacante reinyecta 1.3, contesta el desafío y se hace pasar por $A$ (replay) |
| **1C-2018 c)** ¿Para qué los timestamps de Denning-Sacco? | Para que $B$ rechace tickets viejos: acepta solo si $\lvert \text{reloj} - T\rvert < \Delta t_1 + \Delta t_2$ |

> [!warning] La resolución del PDF para 1C-2023 1.1c) no cierra
> Dice que no se usa $K_0$ *"porque es la clave pública del servidor"*. Si fuera pública, como $N_C$, $N_S$ y $N$ viajan en claro, **cualquiera** calcularía $K_{cs}$. $K_0$ es el secreto que elige el cliente; el enunciado escribe $E_{K_0}(kS)$, casi seguro con los subíndices invertidos: $K_0$ cifrado con la clave $kS$ del certificado.

---

## 3. Algoritmo B — ¿Es válido este modo de cifrado en bloque? (4/4)

El molde: *"$C_0 = IV$, $C_i = \dots$ ¿Es un esquema de cifrado en bloque válido? Comparar la confidencialidad y la tolerancia a errores contra CBC, CTR y OFB."*

> [!example] Receta
> 1. **¿Es válido? = ¿se puede descifrar?** Despejar $M_i$ en función de lo que tiene el receptor ($C_i$, $C_{i-1}$, $k$). Si hay que invertir $E_k$, la primitiva tiene que ser una **permutación** ($D_k$ tiene que existir). Si $E_k$ solo genera un *keystream* que se XORea (CTR, OFB, CFB), **no hace falta** $D_k$: alcanza con una PRF.
> 2. **¿Es CPA-secure?** Primero: ¿es determinístico? (IV fijo o sin IV → no). Si hay IV aleatorio, buscar una combinación de valores **públicos** que dé $E_k(\text{algo que depende solo de } M_i)$. Si existe, el ataque es: desafío con $m_0 = M\|M$ y $m_1 = M\|M'$, y comparar.
> 3. **Error de un bit en $C_i$**: marcar en qué fórmulas de descifrado aparece $C_i$. Si ahí $C_i$ **pasa por $D_k$** → ese bloque plano sale **entero** basura. Si solo entra en un **XOR** → sale invertido **el mismo bit**.
> 4. **Eficiencia**: ¿se paraleliza el cifrado, el descifrado? ¿se puede precalcular el keystream? ¿hay acceso aleatorio?

| Modo | Cifrado | Descifrado | CPA-secure si… | Error de 1 bit en $C_i$ | ¿Usa $D_k$? | Paraleliza |
|---|---|---|---|---|---|---|
| ECB | $C_i = E_k(M_i)$ | $M_i = D_k(C_i)$ | **nunca** (determinístico) | $M_i$ entero | sí | todo |
| CBC | $C_i = E_k(M_i\oplus C_{i-1})$ | $M_i = D_k(C_i)\oplus C_{i-1}$ | IV aleatorio e impredecible | $M_i$ entero + **el mismo bit** de $M_{i+1}$ | sí | solo descifrado |
| CFB | $C_i = M_i\oplus E_k(C_{i-1})$ | $M_i = C_i\oplus E_k(C_{i-1})$ | IV aleatorio | el mismo bit de $M_i$ + $M_{i+1}$ entero | no | solo descifrado |
| OFB | $S_i = E_k(S_{i-1})$, $C_i = M_i\oplus S_i$ | igual | IV aleatorio, nunca repetido | **solo el mismo bit** de $M_i$ | no | keystream precalculable |
| CTR | $C_i = M_i\oplus E_k(\text{nonce}\|i)$ | igual | $(k,\text{nonce})$ nunca repetido | **solo el mismo bit** de $M_i$ | no | todo + acceso aleatorio |

- **Bloque perdido o insertado**: ECB, CBC y CFB se resincronizan solos (CBC pierde 2 bloques); OFB y CTR se desincronizan.
- **Error en el plano** durante el cifrado CBC (Guía 2, ej 6a): cambia **todos** los $C$ siguientes, pero al descifrar solo sale mal ese $M_i$ (el que ya venía mal).
- **CFB de 8 bits con bloque de 64** (Guía 2, ej 6c): el error afecta ese byte y los 8 siguientes.

![[Clase 2 - Encadenamiento CBC.png|480]]

> [!tip] Doce formas de modos inventados, con veredicto, están en **§13.6**.

### 2C-2025 — $C_i = E_k(C_{i-1}\oplus M_i)$

Es **CBC tal cual** (el XOR conmuta). Válido: $M_i = D_k(C_i)\oplus C_{i-1}$. Confidencialidad: la de CBC, CPA-secure con IV aleatorio. Errores: los de CBC (tabla).

### 1C-2025 — $C_i = E_k(M_i)\oplus C_{i-1}$

- **a) Válido**: $C_i\oplus C_{i-1} = E_k(M_i)$ → $M_i = D_k(C_i\oplus C_{i-1})$.
- **b) No es CPA-secure**: el encadenamiento es cosmético. $C_i\oplus C_{i-1} = E_k(M_i)$ es **ECB disfrazado**, porque los dos $C$ son públicos. Ataque: $m_0 = M\|M$, $m_1 = M\|M'$; con el desafío $(C_0, C_1, C_2)$, responder $b'=0$ si $C_2\oplus C_1 = C_1\oplus C_0$. Gana con probabilidad 1. En cambio CBC y OFB son CPA con IV aleatorio, y CTR si no se repite $(k,\text{nonce})$.
- **Errores**: un bit mal en $C_i$ aparece en $C_i\oplus C_{i-1}$ **y** en $C_{i+1}\oplus C_i$, y los dos **pasan por $D_k$** → $M_i$ y $M_{i+1}$ salen **enteros** basura. Es peor que CBC, donde $M_{i+1}$ tiene un solo bit cambiado. En OFB y CTR cambia solo ese bit.

> [!success] Verificado
> Simulé los dos esquemas con una permutación aleatoria de 8 bits como $E_k$: el ataque CPA gana los 2000 juegos; un bit cambiado en $C_2$ deja $M_2$ y $M_3$ enteros mal en este esquema, contra $M_2$ entero + un solo bit de $M_3$ en CBC; en OFB cambia un solo bit.

> [!bug] La resolución del PDF dice "el bloque del error y el contiguo, al igual que en CBC y OFB"
> En OFB el error **no** pasa a otro bloque: cambia solo ese bit (la del 2C-2025 lo había corregido a mano; esta quedó mal). Y en este esquema el bloque contiguo no sale "afectado" como en CBC: sale ilegible.

### 1C-2023 ej 1.2 — CTR con una $E$ que no tiene inversa

- **a) Es válido**: $C_i = M_i\oplus E_k(\text{nonce}\|i)$ y $M_i = C_i\oplus E_k(\text{nonce}\|i)$. $D_k$ no aparece nunca: alcanza con una **PRF**. Es la misma idea del V/F "toda primitiva de bloque tiene que ser reversible" (§10).
- **b) Nonce**: se concatena con el contador en cada bloque; el par $(k,\text{nonce})$ **no se repite nunca**. Si se repite, $C\oplus C' = M\oplus M'$, como un OTP reusado.
- **c) Ventaja**: cifra y descifra en paralelo, precalcula el keystream, accede a cualquier bloque sin procesar los anteriores y no necesita padding.

### 1C-2023 ej 2 = 1C-2018 ej 3 — CBC: sacar y agregar bloques

$C_0 = IV$, $C_k = E_k(M_k\oplus C_{k-1})$.

- **a) Se borra $C_0$**: se pierde **solo $M_1$** ($M_1 = D_k(C_1)\oplus C_0$). $M_2,\dots,M_n$ salen bien, porque cada uno necesita solo su $C$ y el anterior.
- **b) Se borra $C_n$**: se pierde **solo $M_n$**.
- **c) Anteponer $M_0$ sin recifrar todo**: el viejo $C_0$ pasa a ser el cifrado de $M_0$ y el nuevo IV es
$$IV' = D_k(C_0)\oplus M_0 .$$
Se transmite $IV', C_0, C_1,\dots,C_n$. Chequeo: $D_k(C_0)\oplus IV' = M_0$ ✓. Hace falta $k$ (por eso dice "usuario legítimo").

> [!success] Verificado
> Los tres puntos, con la misma simulación. La resolución del PDF para (a) dice "no va a poder ser desencriptado": conviene aclarar que se pierde **solo el primer bloque**.

**Para practicar la mecánica** (Guía 2, ej 7): $E(K,M) = M\cdot K \bmod 32$, CBC con $IV = 19$, $K = 7$, $M = 24, 17, 26, 25, 12$ → $C = 13, 4, 18, 13, 7$. Para descifrar se multiplica por $K^{-1} = 23$ ($7\cdot 23 = 161 \equiv 1 \bmod 32$). Espacio efectivo de claves: $\varphi(32) = 16$ (solo las $K$ impares son invertibles). Verificado.

---

## 4. Algoritmo C — Secreto perfecto (3/4)

$$\Pr[M=m \mid C=c] = \Pr[M=m] \qquad \text{para toda distribución de } M,\ \forall m,\ \forall c \text{ con } \Pr[C=c]>0$$

Hay cuatro formas equivalentes (la Guía 2, ej 1, pide demostrarlo "de las cuatro maneras"):

| # | Condición | Para qué conviene |
|---|---|---|
| 1 | $\Pr[M=m \mid C=c] = \Pr[M=m]$ | la definición; se calcula con Bayes |
| 2 | $\Pr[C=c \mid M=m] = \Pr[C=c]$ | la misma, del otro lado |
| 3 | $\Pr[C=c \mid M=m_0] = \Pr[C=c \mid M=m_1]$ para todo $m_0, m_1, c$ | **la más rápida**: no depende de la distribución de $M$ |
| 4 | $\Pr[\text{PrivK}^{eav}_{A,\Pi}=1] = \tfrac12$ exacto, para todo $A$ | para **refutar**: mostrar un adversario que gana con $\neq \tfrac12$ |

**Condiciones necesarias** (Shannon): $\lvert K\rvert \ge \lvert M\rvert$, clave uniforme, **una clave por mensaje**. Si $\lvert M\rvert = \lvert K\rvert = \lvert C\rvert$: hay secreto perfecto si y solo si cada clave tiene probabilidad $1/\lvert K\rvert$ y para cada par $(m,c)$ hay **exactamente una** clave que lleva $m$ a $c$.

> [!example] Receta
> 1. Armar la **tabla** $(m,k)\mapsto c$, o escribir $c = f(m,k)$.
> 2. Para cada $c$ y cada $m$: $\Pr[C=c \mid M=m] = \sum_{k:\,Enc_k(m)=c}\Pr[K=k]$.
> 3. Si ese número **no depende de $m$** → secreto perfecto (forma 3). Si depende → no.
> 4. **Atajo para refutar**: buscar una relación entre partes de $c$ que **no dependa de la clave** (dos letras iguales, una resta que cancela $k$). Filtra información de $m$ → no hay secreto perfecto. Convertirlo en un adversario de $\text{PrivK}^{eav}$ que gana con más de $\tfrac12$.
> 4b. Cómo armar ese argumento paso a paso, con la tabla de invariantes por esquema: **§13.7**.
> 5. **Atajo para confirmar**: si para cada $(m,c)$ hay una única clave y las claves son uniformes, $\Pr[C=c \mid M=m] = \Pr[K=k_{m,c}]$ es constante → sí.

### 1C-2025 — $c = (m\oplus k_0)\oplus f(k_1)$, un bit

$f$ es la identidad, $k = k_0k_1 \leftarrow \{0,1\}^2$ uniforme, $\Pr[m=0] = 0{,}9$.

Queda $c = m\oplus(k_0\oplus k_1)$, y $k_0\oplus k_1$ es un bit uniforme (vale 0 para $k \in\{00, 11\}$, 1 para $k\in\{01, 10\}$): es un **OTP de un bit**.

$$\Pr[C=0\mid M=0] = \Pr[k\in\{00,11\}] = \tfrac12 = \Pr[k\in\{01,10\}] = \Pr[C=0\mid M=1]$$

e igual para $C=1$ → **secreto perfecto**. Con Bayes: $\Pr[C=0] = \tfrac12$ y $\Pr[M=0\mid C=0] = 0{,}9 = \Pr[M=0]$. La distribución sesgada de $M$ es un distractor. Verificado.

### 2C-2025 — Vigenère con clave de largo $l$ y mensaje de largo $n$

$c_j = \big(m_j + k_{((j-1)\bmod l)+1}\big) \bmod 26$.

- **$l < n$**: la clave se repite y $c_j - c_{j+l} \equiv m_j - m_{j+l}$ **no depende de la clave** → **no hay secreto perfecto**. Adversario: $m_0 = \texttt{a}^{l+1}$, $m_1 = \texttt{a}^{l}\texttt{b}$; dice 0 si $c_1 = c_{l+1}$. Gana siempre.
- **$l \ge n$**: cada letra usa una clave distinta, uniforme e independiente. Para cada $(m,c)$ hay una sola clave útil, $k_j = c_j - m_j$, así que $\Pr[C=c\mid M=m] = (1/26)^n$ para todo $m$ → **hay secreto perfecto** (es un OTP mod 26).

> [!warning] La resolución del PDF pide $l = n$
> Alcanza con $l \ge n$: si la clave es más larga, sobran letras que no se usan y el argumento es el mismo. Y hay que decir las **tres** condiciones: clave uniforme, $l \ge n$ y una sola vez.

### Otros para practicar

- **Guía 2, ej 1** (la tabla de $a,b$ y $k_1,k_2,k_3$): $\Pr[C=1,2,3,4] = \tfrac18, \tfrac{7}{16}, \tfrac14, \tfrac{3}{16}$. Como $\Pr[C=1\mid M=b] = 0 \neq \tfrac12 = \Pr[C=1\mid M=a]$, no hay secreto perfecto. El adversario "$m_0=a$, $m_1=b$, digo 0 si veo 1" gana con $\tfrac34$. Verificado.
- **Clase 2, OTP con clave sesgada**: $\Pr[M=00\mid C=01] = 0{,}06/0{,}185 \approx 0{,}32 \neq 0{,}6$. La clave tiene que ser uniforme. Verificado.
- **1C-2018, cifrado homofónico** (elegir la opción correcta): es la **(c)** — repartir cada vocal en varios símbolos, en proporción a su frecuencia, **aplana** el histograma y el índice de coincidencia deja de servir. La (b) es falsa: Vigenère depende de la **posición** con una clave periódica; el homofónico elige el símbolo al azar. La (a) acierta en que no hay secreto perfecto pero no por ese motivo: las consonantes siguen siendo monoalfabéticas y el espacio de claves es muchísimo más chico que el de mensajes.

---

## 5. Algoritmo D — Experimento CPA sobre un esquema dado (3/4)

$\text{PrivK}^{CPA}_{A,\Pi}(n)$:

1. $k \leftarrow Gen(1^n)$.
2. $A$ tiene un oráculo $Enc_k(\cdot)$ y emite $m_0$, $m_1$ de igual longitud.
3. Se sortea $b\in\{0,1\}$ y $A$ recibe $c \leftarrow Enc_k(m_b)$.
4. $A$ sigue usando el oráculo y emite $b'$.
5. Éxito si $b' = b$. El esquema es seguro si $\Pr[\text{éxito}] \le \tfrac12 + negl(n)$ para todo $A$ PPT.

EAV es lo mismo **sin** oráculo; CCA agrega un oráculo de $Dec_k$ que no se puede usar sobre el propio $c$.

> [!example] Receta para romperlo
> 1. **¿Es determinístico?** (mismo $m$ → mismo $c$). Si sí: pedirle al oráculo $Enc_k(m_0)$ y compararlo con el desafío. Gana con probabilidad 1. Así caen ECB, textbook RSA, sustitución y Vigenère.
> 2. **¿Es lineal o afín en la clave?** Pedir cifrados de mensajes conocidos, plantear el sistema y **despejar la clave**. Con la clave se descifra el desafío.
> 3. **¿Hay una relación visible entre bloques** (como el ECB disfrazado del §3)? Elegir $m_0$, $m_1$ que la activen o no.
> 4. Cerrar con la cuenta: $\Pr[\text{PrivK}^{CPA}_{A,\Pi}=1] = \tfrac12\cdot 1 + \tfrac12\cdot 1 = 1 > \tfrac12 + negl(n)$ → no es CPA-secure.

### 1C-2023 ej 4 = 1C-2018 ej 4

$k = (a,b)$, $Enc_k(m) = (r,\; ar+b+m) \bmod p$ con $r$ aleatorio. Variante: $r = (a+b) \bmod p$.

- **Con $r = a+b$**: $c = \big(a+b,\; a(a+b)+b+m\big)$. La primera componente es **constante** y la segunda es $m$ + constante → el esquema es **determinístico**. Se pide $Enc(m_0)$ al oráculo y se compara con el desafío: gana siempre → **no es CPA-secure**.
- **Extra, si preguntan por el original con $r$ aleatorio**: **tampoco** es CPA-secure. Dos consultas con $m = 0$ dan $(r_1, s_1 = ar_1+b)$ y $(r_2, s_2 = ar_2+b)$, y de ahí
$$a = \frac{s_1-s_2}{r_1-r_2}, \qquad b = s_1 - a\,r_1 \pmod p \qquad (r_1\neq r_2).$$
Con $(a,b)$ se descifra el desafío. Es lineal: dos pares alcanzan. Verificado.

> [!bug] La resolución del PDF dice "no es determinístico y por ende no va a ser CPA-Secure"
> Es al revés: **es** determinístico (el $r$ dejó de ser aleatorio) y **por eso** no es CPA-secure. La conclusión está bien, el motivo está invertido.

**Guía 2, ej 5** — los dos son determinísticos, así que cae la regla 1. Sustitución: con $m_0 = \texttt{aa}$ y $m_1 = \texttt{ab}$, si el cifrado tiene las dos letras iguales era $m_0$. Vigenère: cifrar $\texttt{aaa}\dots$ con el oráculo devuelve la clave, y con ella se descifra el desafío.

**CCA en una línea**: lo que rompe CCA es la **maleabilidad**. Se modifica el desafío a un $c' \neq c$, se pide $Dec(c')$ y se deshace el cambio: flujo/CTR/OFB con $c' = c\oplus\Delta$ (Guía 4, ej 15), textbook RSA con $c' = c\cdot r^e$ (Guía 4, ej 17). Lo arregla **Encrypt-then-MAC** con claves independientes (§7).

---

## 6. Algoritmo E — Criptoanálisis clásico (4/4 en alguna forma)

> [!example] Receta: ¿qué tipo de cifrado es? (Guía 1, ej 5)
> Contar las frecuencias del criptograma y compararlas con las del idioma:
> - **Mismas letras con las mismas frecuencias** (muchas E, A, O…) → **transposición**: solo cambió el orden.
> - **Otras letras, pero igual de "picudo"** (IC ≈ 0,077 en castellano) → **sustitución monoalfabética**; si los picos aparecen corridos en bloque → rotación (César).
> - **Histograma plano** (IC cerca de $1/26 \approx 0{,}038$) → **polialfabética** (Vigenère).
>
> $$IC = \frac{\sum_i F_i(F_i - 1)}{N(N-1)}$$

> [!example] Receta: romper Vigenère
> 1. **Kasiski**: buscar secuencias repetidas (de 3 letras o más si se puede) y medir distancias. El largo de la clave divide al **MCD** de las distancias.
> 2. **Confirmar con el IC**: partir el texto en $L$ columnas ($c_j, c_{j+L}, c_{j+2L},\dots$). Con el $L$ correcto cada columna tiene IC de idioma; con uno incorrecto, IC de texto al azar.
> 3. **Cada columna es un César**: la letra más frecuente probablemente es E o A. Mejor: probar los 26 corrimientos y quedarse con el que maximiza la suma de las frecuencias del idioma de las letras descifradas.
> 4. Leer la clave, descifrar y corregir con sentido común (palabras, digramas).

### 2C-2025 ej 2 — resuelto (la resolución del PDF lo salteó)

`GWAOESFENITLAGEUGEDRVPHJVCDFDR`, en castellano, alfabeto de 26 letras.

1. **Kasiski**: `DR` aparece en las posiciones 18 y 28 → distancia 10 → $L \in \{2, 5, 10\}$. Con 30 letras la evidencia es poca; el enunciado da la pista escondida ("encriptado **con clave**").
2. **Columnas con $L = 5$**: `GSTUVC` · `WFLGPD` · `AEAEHF` · `ONGDJD` · `EIERVR`.
3. **Corrimientos**: probando los 26 en cada columna contra la tabla de frecuencias del enunciado, el mejor de cada una da **C, L, A, V, E**.
4. Con la clave **CLAVE**: `ELATAQUESERAALASVEINTEHORASFIN` → *"El ataque será a las veinte horas. Fin."*

> [!success] Verificado con Python
> Con $L=5$ el corrimiento que maximiza la frecuencia en cada columna es exactamente CLAVE. Con $L=2$ o $L=10$ sale basura.

**César (Guía 1, ej 3)**: la K aparece en el 25 % del criptograma → K = E → corrimiento 6.

### Lo que más se pregunta de clásicos

- **Contar claves (1C-2025, 5c)**: con bloques de 3 bits hay $3! = 6$ transposiciones (permutar 3 posiciones) y $8! = 40320$ sustituciones (biyecciones de los $2^3 = 8$ valores). La afirmación "hay más transposiciones que sustituciones" es **falsa**. Con un alfabeto de $q$ símbolos: rotación $q$, sustitución $q!$, Vigenère de largo $t$: $q^t$.
- **Composición (Guía 1, ej 2 y 4c)**: sustitución∘sustitución es otra sustitución, no suma seguridad. Vigenère∘Vigenère es un Vigenère con clave suma, de largo $\operatorname{mcm}(p,q)$.
- **Transposición + rotación (Guía 1, ej 7)**: primero la rotación por frecuencias (la transposición no las altera), después se prueban anchos de columna. Fuerza bruta: $q\cdot m$ pruebas.
- **CPA contra los clásicos (Guía 1, ej 8)**: sustitución → cifrar `abc…y` (25 letras) revela toda la permutación. Vigenère → cifrar `aaaa…` devuelve la clave.

### 1C-2023 ej 3 — Base64 (la resolución del PDF dice "no lo vimos")

- **a)** **No** es un sistema de cifrado: no tiene clave. Es una **codificación** pública y reversible; por Kerckhoffs, conocer el algoritmo alcanza para "descifrar".
- **b)** **Confusión** (Shannon): que la relación entre la clave y el cifrado sea compleja — cada bit del cifrado depende de muchos bits de la clave (en AES, SubBytes). **Difusión**: que cada bit del plano influya en muchos bits del cifrado y se disperse la estadística del idioma (ShiftRows y MixColumns; efecto avalancha). Base64 no tiene ninguna: cada 6 bits van a un carácter, de forma local y sin clave.
- **c)** **Lineal**: el cifrado se escribe como combinación lineal (o afín) de los símbolos del plano y de la clave, como $c = K\cdot m$ o $c = m + k$. Con pocos pares (plano, cifrado) se arma un sistema lineal y sale la clave. Ejemplos: César y afín, Vigenère, **Hill** ($c = K\cdot m \bmod 26$), OTP, el esquema $c = (r,\,ar+b+m)$ del §5. AES es no lineal gracias a la S-box.

---

## 7. MAC, hash y cifrado autenticado

- Un **MAC** da **integridad + autenticación de origen** entre quienes comparten $k$. **No** da confidencialidad ($m$ va en claro), **ni no repudio** (los dos tienen $k$), **ni protege contra replay** (hace falta nonce, timestamp o número de secuencia).
- Seguridad: **Mac-forge**. Con un oráculo $Mac_k(\cdot)$, producir un $(m,t)$ válido con $m$ que no se haya consultado.
- **Para romper un MAC propuesto** (Guía 3, ej 1 y 2): pedir el MAC de $0^n$ y ver si sale la clave o algo reutilizable ($G(k)\oplus m$ entrega $G(k)$; $k\oplus \text{first}(m)$ entrega $k$), o buscar dos mensajes con el mismo tag (con el XOR de bloques alcanza permutar bloques: *"el auto es azul y el lápiz rojo"* ↔ *"el auto es rojo y el lápiz azul"*).
- **CBC-MAC**: $t_0 = 0^n$ (IV **fijo**), $t_i = F_k(t_{i-1}\oplus m_i)$, se emite solo $t_\ell$. Es seguro **solo con longitud fija**. Con longitud variable: si $t = Mac_k(m)$ para un $m$ de un bloque, entonces $(m\,\|\,(m\oplus t),\ t)$ verifica. Arreglos: **prefijar** la longitud (como sufijo no sirve), usar $k_\ell = F_k(\ell)$, o cifrar el tag con otra clave, $\hat t = F_{k_2}(t)$. Detalle en [[Clase 3 - Criptografia - MACs y modo autenticado#CBC-MAC]].

> [!warning] IV en CBC-MAC vs IV en CBC
> En CBC para **cifrar** el IV tiene que ser aleatorio. En CBC-MAC tiene que ser **fijo**: con un IV aleatorio que viaja con el tag, se cambia el primer bloque y el IV a la vez y el tag sigue verificando.

- **Hash**, tres niveles, del más fuerte al más débil: resistencia a **colisiones** ⇒ a **segundas preimágenes** ⇒ (en la práctica) a **preimágenes**. Costo genérico con salida de $\ell$ bits: preimagen $2^{\ell}$, colisión $2^{\ell/2}$ (cumpleaños). Por eso la práctica pide salidas de 160 bits o más ($2^{80}$ de trabajo para una colisión); hoy el piso razonable es 256. MD5 (128) y SHA-1 (160) tienen colisiones prácticas.
- **Merkle-Damgård**: $z_0$ fijo, $z_i = h(z_{i-1}\,\|\,x_i)$, y un último bloque con la longitud. Una colisión en $H$ implica una colisión en $h$. Por esta estructura, $H(k\,\|\,m)$ como MAC es vulnerable a **extensión de longitud** → por eso existe **HMAC**:
$$t = H\big((k\oplus opad)\,\|\,H((k\oplus ipad)\,\|\,m)\big), \qquad ipad = \texttt{0x36},\quad opad = \texttt{0x5C}$$

> [!bug] ipad y opad están intercambiados en las slides de teoría y de práctica
> Tanto la teórica de la Clase 3 como la práctica de la Clase 4 dicen opad = 0x36 e ipad = 0x5C. El RFC 2104 dice **ipad = 0x36** (el hash interno) y **opad = 0x5C** (el externo). La slide de TLS de la Clase 5 lo tiene bien.

| Orden | Qué se manda | Veredicto |
|---|---|---|
| Encrypt-and-MAC | $Enc_{k_1}(m)$ y $Mac_{k_2}(m)$ | ❌ el tag puede filtrar información de $m$ |
| MAC-then-Encrypt | $Enc_{k_1}(m\,\|\,Mac_{k_2}(m))$ | ⚠️ puede ser seguro, requiere prueba (padding oracles) |
| **Encrypt-then-MAC** | $c = Enc_{k_1}(m)$ y $t = Mac_{k_2}(c)$ | ✅ CCA-secure si Enc es CPA, el MAC es seguro y **$k_1$, $k_2$ son independientes**. El receptor **verifica antes de descifrar** |

### 1C-2025 ej 3 — $c = E_{k_1}\big(m\,\|\,H(k_2\,\|\,m)\big)$, con $m$ de tamaño fijo

- **a) Receptor**: (1) $d = D_{k_1}(c)$; (2) partir $d = m\,\|\,t$ (se sabe dónde cortar porque $\lvert m\rvert$ es fijo); (3) calcular $t' = H(k_2\,\|\,m)$; (4) aceptar $m$ solo si $t' = t$, y si no, rechazar.
- **b) Integridad**: sin $k_2$ el atacante no puede calcular el tag de otro $m'$. Aunque $E$ sea maleable (flujo o CTR) y pueda cambiar bits de $m$ adentro de $c$, el tag cifrado tendría que pasar a $H(k_2\,\|\,m')$, que no conoce → la verificación falla salvo con probabilidad despreciable. La extensión de longitud tampoco aplica: $m$ es de tamaño fijo y el tag viaja cifrado. Lo criticable es el **orden**: es MAC-then-Encrypt ("puede ser seguro, requiere prueba"); lo recomendado es Encrypt-then-MAC, con HMAC en vez de $H(k\,\|\,m)$.
- **c) Autenticación**: sí, **simétrica**: solo quien tiene $k_1$ y $k_2$ pudo armar $c$. Pero no distingue **cuál** de los dos lo mandó, y **no hay frescura**: un $c$ viejo reenviado verifica (replay).
- **d) No repudio**: **no**, las claves son compartidas.

> [!warning] La resolución del PDF para (b) no cierra
> Dice que el atacante puede armar $c' = (m'\,\|\,t')$ y pasar la validación. Para eso necesitaría $k_1$ (para cifrar) y $k_2$ (para el tag). La integridad se sostiene gracias a $k_2$; lo mejorable es el orden (MtE → EtM).

---

## 8. Asimétrico, firmas, PKI y TLS

| Esquema | Claves | Operación | Seguridad | Trampa |
|---|---|---|---|---|
| Diffie-Hellman | $x$ secreto, $g^x$ público | $k = g^{xy}$ | DDH | no autentica → MITM |
| Textbook RSA | $pk=(n,e)$, $sk=(n,d)$, $ed\equiv 1 \bmod \varphi(n)$ | $c = m^e$, $m = c^d \bmod n$ | factorizar $n$ (sin prueba) | **determinístico → no CPA**; maleable → no CCA |
| RSA + PKCS#1 v1.5 | ídem | padding aleatorio `00 02 r 00 m` | se cree CPA | **no CCA** (Bleichenbacher) |
| ElGamal | $pk = h = g^x$ | $(g^y,\ h^y\cdot m)$ con $y$ nuevo | CPA bajo DDH | no reusar $y$ |
| Firma RSA | las de RSA | firma $s = m^d$; verifica $s^e \stackrel{?}{=} m$ | factorizar $n$ | textbook: forja sin mensaje y multiplicativa → **Hashed RSA** $H(m)^d$ |
| DSA / ECDSA | $x$ secreto, $y = g^x$ | firma $(r,s)$ con un $k$ al azar | logaritmo discreto | no reusar $k$ (se despeja $x$) |

> [!example] RSA a mano, por si piden un ejemplo
> $p = 61$, $q = 53$ → $n = 3233$, $\varphi(n) = 60\cdot 52 = 3120$, $e = 17$.
> **Euclides extendido** para $d = 17^{-1} \bmod 3120$:
> $3120 = 17\cdot 183 + 9$, $\quad 17 = 9\cdot 1 + 8$, $\quad 9 = 8\cdot 1 + 1$
> $1 = 9 - 8 = 2\cdot 9 - 17 = 2\cdot 3120 - 367\cdot 17$ → $d = -367 \equiv 2753$.
> $m = 65$: $c = 65^{17} \bmod 3233 = 2790$ y $2790^{2753} \bmod 3233 = 65$ ✓.

- **Ataques de la Guía 4 que conviene tener a mano**: forja sin mensaje (elegir $s$ al azar y publicar $(s^e \bmod n,\ s)$); CCA a textbook RSA ($c' = c\cdot r^e$ → el oráculo devuelve $m\cdot r$ → multiplicar por $r^{-1}$). Verificados con este RSA de juguete.
- **Firma vs MAC**: los dos dan integridad y autenticación; solo la firma es **públicamente verificable**, **transferible** y da **no repudio**. Ninguno frena el replay por sí solo.
- **Direcciones de las claves**: cifrado → la **pública cifra** y la privada descifra. Firma → la **privada firma** y la pública verifica.
- **Certificado X.509**: identidad del titular + **clave pública del titular** + emisor, validez, uso y número de serie, todo **firmado con la privada de la CA**. **Nunca** contiene una clave privada.
- **Validar un certificado**: (1) conseguir la pública del emisor (subir por la cadena hasta una raíz confiable); (2) verificar la **firma de la CA**; (3) vigencia; (4) identidad (el nombre esperado); (5) uso permitido; (6) que no esté revocado (CRL / OCSP).
- **TLS**: confidencialidad + integridad + autenticación (del servidor siempre, del cliente opcional) sobre **PKI**. **Sin** KDC y **sin** no repudio (los datos van con MAC/AEAD de clave compartida). Handshake: nonces $r_1, r_2$ (contra replay) → certificado → *pre-master* (por RSA o DH) → *master* derivado del pre-master y los nonces → *Finished* (MAC de todo lo intercambiado: confirma las claves y detecta manipulación). Detalle en [[Protocolos]].

---

## 9. Algoritmo F — Shamir (Clase 5)

> [!example] Receta
> **Repartir un esquema $(t,n)$** — $n$ sombras, umbral $t$: primo $p > s$ y $p > n$;
> $$P(x) = s + a_1x + \dots + a_{t-1}x^{t-1} \bmod p$$
> de **grado $t-1$**, con coeficientes al azar. Las sombras son $(i,\,P(i))$ para $i = 1,\dots,n$.
>
> **Reconstruir** con $t$ sombras $(x_a, y_a)$: Lagrange evaluado directamente en 0,
> $$s = P(0) = \sum_{a=1}^{t} y_a \prod_{b\neq a}\frac{-x_b}{x_a - x_b} \pmod p .$$
> Dividir es multiplicar por el inverso mod $p$.
>
> **Detectar al espía**: las sombras legítimas están todas sobre el mismo polinomio. Armar el polinomio con $t$ de ellas y ver cuál queda afuera.
>
> **Jerarquías**: darle **más sombras** a quien tiene más poder (pesos), o componer esquemas (un camino al secreto por cada grupo autorizado).

**Ejemplo de la slide**, $(3,5)$ con $P(x) = 5x^2 + 3x + 7 \bmod 11$, reconstruyendo con $(2,0)$, $(3,6)$, $(5,4)$:

$$L_2(0) = \frac{(-3)(-5)}{(2-3)(2-5)} = \frac{15}{3} = 5, \qquad L_3(0) = \frac{(-2)(-5)}{(3-2)(3-5)} = \frac{10}{-2} \equiv 6, \qquad L_5(0) = \frac{(-2)(-3)}{(5-3)(5-2)} = 1$$

$$s = 0\cdot 5 + 6\cdot 6 + 4\cdot 1 = 40 \equiv 7 \pmod{11} \;\checkmark$$

> [!bug] Dos errores de la slide (ya anotados en [[Protocolos]])
> - $P(4) = 80+12+7 = 99 \equiv \mathbf{0}$, no 2: toda terna con la sombra $(4,2)$ reconstruye mal.
> - "Un polinomio de grado $t$ se especifica con $t$ puntos": hacen falta $t+1$. Para umbral $t$ el polinomio es de grado $t-1$, como en el propio ejemplo.

**Guía 6, ej 14** (está en el PDF): pares mod 11 $A(1,4)$, $B(3,7)$, $C(5,1)$, $D(7,2)$, cualquier par recupera el secreto → recta. Con $A$ y $B$: $a = \frac{3}{2} \equiv 3\cdot 6 \equiv 7$, $s = 4 - 7 \equiv 8$ → $f(x) = 7x + 8$. $f(7) = 57 \equiv 2$ ✓ ($D$ está), $f(5) = 43 \equiv 10 \neq 1$ → **$C$ es el espía** y el **mensaje es 8**.

> [!bug] La resolución del PDF despeja mal la recta
> Llega a $b = 3$, $x = 1$ (y después suma 6): esa recta no pasa por $B$. La conclusión sobre $C$ sale igual, pero el mensaje es **8** y el PDF no lo da. Verificado probando las 6 rectas: solo $7x+8$ contiene tres de los puntos.

**Guía 6, ej 15** (general, 2 coroneles, 5 suboficiales): la composición del PDF está bien. La versión con pesos es más corta: un solo Shamir con **umbral 10** y 30 sombras — **10 al general, 5 a cada coronel, 2 a cada suboficial**. Autorizados: general 10, dos coroneles 10, cinco suboficiales 10, un coronel + tres suboficiales 11. No autorizados: un coronel + dos suboficiales 9, cuatro suboficiales 8. Verificado sobre los 255 subconjuntos.

---

## 10. Banco de V/F — todos los de los cuatro parciales

| Parcial | Afirmación | | Qué escribir |
|---|---|---|---|
| 2C-25 5a | MD5 es un criptosistema asimétrico que no hay que usar porque su clave es de 128 bits | **F** | Es una **función de hash** (no tiene clave); 128 bits es el largo de la **salida**. No se usa porque tiene **colisiones** prácticas |
| 2C-25 5b | Un protocolo de autenticación solo con MAC da confidencialidad, integridad y no repudio | **F** | Da **integridad y autenticación**. Ni confidencialidad ni no repudio (la clave es compartida) |
| 2C-25 5c | El padding aleatorio de RSA es para que sea seguro ante texto cifrado elegido (CCA) | **F** | Es para que sea **no determinístico → CPA**. PKCS#1 v1.5 cae ante CCA (Bleichenbacher); para CCA hace falta OAEP |
| 2C-25 5d | Un certificado emitido por una CA contiene siempre la clave pública de la CA | **F** | Contiene la clave pública **del titular**, firmada con la **privada de la CA** |
| 1C-25 5a | Toda función de cifrado simétrica tiene que ser inyectiva | **V** | Si dos mensajes dieran el mismo cifrado con la misma clave, no se podría descifrar |
| 1C-25 5b | En clave pública se usa una clave para cifrar/descifrar y la otra para firmar | **F** | Cifrado: **pública cifra, privada descifra**. Firma: **privada firma, pública verifica** |
| 1C-25 5c | Con un código binario de 3 bits hay más cifrados de transposición que de sustitución | **F** | Transposiciones $3! = 6$ < sustituciones $8! = 40320$ |
| 1C-25 5d | Un certificado público contiene la clave privada de la entidad certificante | **F** | Contiene la **pública del titular** y la **firma** hecha con la privada de la CA |
| 1C-23/18 5a | Para privacidad e integridad: cifrar $m$ con $k_1$ y al mismo tiempo MAC de $m$ con $k_2$; se mandan $m$ y $t$; $k_1$ y $k_2$ pueden ser iguales | **F** | **Encrypt-then-MAC**: $c = Enc_{k_1}(m)$, $t = Mac_{k_2}(c)$, se mandan $c$ y $t$, con claves **independientes** |
| 1C-23/18 5b | La seguridad de un hash es la resistencia a preimágenes | **F** | Incompleta: **preimagen, segunda preimagen y colisiones** (la más fuerte) |
| 1C-23/18 5c | Diffie-Hellman permite que Alice le **envíe** una clave de sesión a Bob | **F** | Permite que la **acuerden**: nadie la envía, los dos calculan $g^{xy}$ |
| 1C-23/18 5d | En cifrado en bloque, sea cual sea el modo, la primitiva (BCE o PRF) tiene que ser reversible | **F** | ECB y CBC necesitan $D_k$ (permutación); **CTR, OFB y CFB** solo usan $E_k$ → alcanza con una **PRF** |
| 1C-23 1.3 / 1C-18 2.3 (a) | SSL da integridad y autenticación mediante un KDC centralizado | **F** | Usa **PKI** (certificados), no KDC |
| 1C-23 1.3 / 1C-18 2.3 (b) | SSL da confidencialidad, integridad y no repudio mediante PKI | **F** | **No** da no repudio: los datos van con MAC/AEAD de clave compartida |
| 1C-23 1.3 / 1C-18 2.3 (c) | TLS da confidencialidad, integridad y autenticación bajo PKI | **V** | |
| 1C-18 2.1 | La validación de un certificado incluye… | **(c)** | Verificar la **firma de la CA** incluida en el certificado |
| 1C-18 2.2 | Cifrado homofónico del Duque de Mantua | **(c)** | El IC no sirve tanto porque el histograma se aplana (ver §4) |

---

## 11. Errores verificados en las resoluciones del PDF de parciales

1. **1C-2025, 1b**: $2^3 \bmod 5 = 3$, no 1; y $y = p-1$ hace que la clave sea 1 siempre (§2).
2. **1C-2025, 1d**: la exponenciación costosa no es un problema de seguridad; responder con parámetros sin autenticar o falta de identidades (§2).
3. **1C-2025, 2b**: en OFB un error de bit no pasa a otro bloque (§3).
4. **1C-2025, 3b**: sin $k_2$ el atacante no puede armar un $c'$ válido (§7).
5. **1C-2023, 1.1c**: $K_0$ no es la clave pública del servidor, es el pre-master (§2).
6. **1C-2023, 4a**: el esquema **es** determinístico, por eso no es CPA (§5).
7. **1C-2023, 3b-c**: dice "no lo vimos" → respuestas en §6.
8. **2C-2025, 2**: salteado → clave CLAVE, resuelto en §6.
9. **2C-2025, 4**: la condición es $l \ge n$ (con clave uniforme y de un solo uso), no solo $l = n$ (§4).
10. **Guía 6, ej 14**: el mensaje es **8** (§9).

Y de las slides, lo que puede aparecer: Shamir $P(4) = 0$ y grado $t-1$ (§9), ipad/opad intercambiados (§7), y el logaritmo discreto **no** es NP-hard (ver [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital#La seguridad Diffie-Hellman|Clase 4]]).

---

## 12. Frases para la noche anterior

- **Determinístico ⇒ no CPA-secure.** ECB, textbook RSA, cualquier esquema con IV fijo.
- **Secreto perfecto ⇒ $\lvert K\rvert \ge \lvert M\rvert$**, clave uniforme, usada una sola vez. Cualquier sistema con secreto perfecto se reduce al OTP.
- **CBC**: CPA con IV aleatorio e impredecible; un bit mal en $C_i$ → $M_i$ entero + el mismo bit de $M_{i+1}$; cifrado secuencial, descifrado paralelo.
- **CTR / OFB**: no necesitan $D_k$ (alcanza una PRF); un bit mal → solo ese bit; nunca repetir $(k, \text{nonce/IV})$.
- **MAC**: integridad + autenticación. Ni confidencialidad, ni no repudio, ni protección contra replay.
- **Firma**: integridad + autenticación + **no repudio**; la privada firma, la pública verifica.
- **Certificado** = clave pública del titular + identidad, firmado con la privada de la CA.
- **Encrypt-then-MAC**, con claves independientes y verificando antes de descifrar.
- **Hash**: colisiones ($2^{\ell/2}$) es más fuerte que segunda preimagen, que es más fuerte que preimagen ($2^{\ell}$).
- **CBC-MAC**: IV fijo y longitud fija (o la longitud como **prefijo**).
- **DH** acuerda la clave, no la envía; resiste atacantes pasivos (DDH); sin autenticación → MITM.
- **Padding de RSA (PKCS#1 v1.5)** → CPA, no CCA. OAEP → CCA.
- **TLS**: confidencialidad + integridad + autenticación con PKI; sin no repudio y sin KDC.
- **Needham-Schroeder**: $B$ no puede saber si el ticket es fresco → Denning-Sacco agrega timestamps.
- **Kerckhoffs**: lo único secreto es la clave. Base64 no es cifrado.

---

## 13. Catálogo para reconocer — protocolos, modos y fugas sin clave

> [!abstract] Para qué está esta sección
> Las recetas de §2, §3 y §6 dicen *qué hacer*. Esto es la parte de **reconocer**: ver un protocolo o un encadenamiento en el enunciado y saber de qué familia es, qué garantiza y dónde se rompe. Tres bloques: **protocolos** (13.1–13.5), **modos de bloque inventados** (13.6) y cómo **demostrar que un esquema filtra información sin conocer la clave** (13.7). Todo lo marcado como verificado lo simulé con Python.

### 13.1 Leer la notación en diez segundos

| Se ve | Quién lo puede generar | Quién lo puede leer | Qué le prueba al receptor |
|---|---|---|---|
| $\{M\}_K$, $E_K(M)$ (simétrico) | quien tenga $K$ | quien tenga $K$ | que lo armó alguien con $K$, **si** el cifrado es autenticado (uno maleable como CTR u OFB no prueba nada) |
| $h_K(M)$, $Mac_K(M)$ | quien tenga $K$ | **todos** ($M$ va en claro) | integridad + que viene de alguien con $K$ (no de cuál de los dos) |
| $E_B(M)$ (pública de $B$) | **cualquiera** | solo $B$ | nada sobre el emisor: solo confidencialidad hacia $B$ |
| $S_A(M)$ (firma de $A$) | solo $A$ | **todos** | que lo firmó $A$; no a quién iba dirigido ni cuándo |
| $E_B(S_A(M))$ | solo $A$ | solo $B$ | confidencialidad + origen, **si** $M$ nombra a $B$ (ver ⑥) |
| $cert_X = S_{CA}(X, pk_X, \dots)$ | la CA | todos | que $pk_X$ es de $X$ |
| $N$, $r$, `rand` | quien lo genera | todos (suele ir en claro) | frescura, **solo para quien lo generó** |
| $T$, `time` | — | — | frescura si va **protegido** (cifrado, firmado o con MAC) y los relojes están sincronizados |

> [!warning] Las tres confusiones que más puntos cuestan
> - **Firmar no es cifrar.** $S_A\{N_1, K_s\}$ lo abre cualquiera con $pk_A$ (Guía 4, ej 3).
> - **Un nonce solo le sirve a quien lo generó.** Si $B$ recibe un nonce que eligió $A$, a $B$ no le prueba frescura.
> - **Un MAC o una firma válidos no dicen que el mensaje sea nuevo.** Un replay pasa todas las verificaciones.

### 13.2 ¿Qué intenta construir? — las familias

| Familia | Objetivo | Con qué | Señal en el enunciado |
|---|---|---|---|
| **Autenticación** (unilateral o mutua) | probar que del otro lado está quien dice | secreto compartido o firma | nonces que vuelven procesados; no aparece ninguna clave nueva |
| **Transporte de clave** | uno **elige** $K_s$ y se la **manda** al otro | cifrado simétrico o con la pública del otro | $K_s$ adentro de un cifrado |
| **Acuerdo de clave** | la clave **sale de los dos**, nadie la manda | Diffie-Hellman | $g^x$, $g^y$; $K = g^{xy}$ |
| **Distribución con tercero** | un servidor de confianza genera la clave o certifica las públicas | KDC simétrico ($K_{AT}$, $K_{BT}$) o servidor de claves públicas / CA | aparece $T$, Trent, KDC, CA |
| **Canal seguro** | autenticación + clave + protección de los datos | todo lo anterior junto | handshake y después "los datos van con $K$" (TLS) |

La pregunta *"¿qué tipo de protocolo es?"* se contesta con tres cosas: **qué construye** (autenticación de quién, clave de sesión o canal), **con qué** (secreto precompartido, tercero o PKI) y **contra qué atacante** (pasivo o activo).

### 13.3 Fichas de los protocolos

Cada ficha: **mensajes → cómo reconocerlo → qué garantiza → dónde se rompe → arreglo.**

#### ① Challenge-response unilateral

$$1.\ B\to A: N_B \qquad 2.\ A\to B: \{N_B\}_K \ \text{ o } \ h_K(N_B) \ \text{ o } \ S_A(N_B)$$

- **Reconocerlo**: dos mensajes; un número que va y vuelve transformado con una clave.
- **Garantiza**: $B$ autentica a $A$, con frescura porque $N_B$ lo eligió $B$. $A$ no sabe nada de $B$.
- **Se rompe si** la respuesta no nombra a nadie (reflexión, si $A$ también verifica con la misma clave) o si $A$ firma cualquier cosa que le manden (se vuelve un **oráculo de firma**).
- **Arreglo**: identidades adentro ($h_K(N_B, A)$) y, con firma, un nonce propio ($S_A(N_A, N_B, B)$).

#### ② Autenticación mutua con nonces cruzados (Guía 4, ej 4)

$$1.\ A\to B: N_1 \qquad 2.\ B\to A: N_2 \qquad 3.\ A\to B: \{N_2\}_{K_s} \qquad 4.\ B\to A: \{N_1\}_{K_s}$$

- **Reconocerlo**: protocolo **simétrico**: los dos hacen la misma operación con la misma clave y nada dice quién responde.
- **Se rompe — reflexión**: Mallory hace de $B$ frente a $A$ y le devuelve **su propio nonce**, $N_2 := N_1$. $A$ contesta $\{N_1\}_{K_s}$ en el paso 3 y Mallory se lo devuelve como paso 4. $A$ "autentica a $B$" sin que $B$ haya participado. La variante con **sesiones paralelas** abre otra sesión con $A$ para que ella misma calcule la respuesta que hace falta en la primera.
- **Lo que contesta la solución de la guía**: *"$A$ no debe permitir que le devuelvan el mismo nonce"*.
- **Arreglo de fondo**: la identidad del que responde adentro, $\{N_2, A\}_{K_s}$ y $\{N_1, B\}_{K_s}$, o claves distintas por dirección.

> [!success] Verificado — con $N_2 = N_1$, $A$ acepta a un "$B$" que nunca participó.

#### ③ Autenticación mutua con MACs y clave derivada (2C-2025)

Es ② bien hecho: identidades adentro del MAC, cada uno responde al nonce del otro y $W = h'_{K'}(r_B)$ deriva la clave. Resuelto en §2. Punto débil: sin forward secrecy.

#### ④ Intercambio de claves públicas sin certificados (Guía 4, ej 2)

$$1.\ A\to B: pk_A \qquad 2.\ B\to A: pk_B \qquad 3.\ A\to B: E_B(M) \qquad 4.\ B\to A: E_A(M)$$

- **Reconocerlo**: las claves públicas viajan **sueltas**, sin firma de nadie.
- **Se rompe — MITM de manual**: Mallory cambia $pk_A$ y $pk_B$ por $pk_M$ en tránsito. Los dos cifran para Mallory, que descifra, lee, modifica y recifra con la clave legítima.
- **Arreglo**: certificados, que atan la clave a la identidad.

#### ⑤ Transporte de clave firmado (Guía 4, ej 3)

$$1.\ A\to B: S_A\{N_1, K_s\} \qquad 2.\ B\to A: \{N_1+1\}_{K_s}$$

- **Reconocerlo**: una clave de sesión adentro de algo **firmado** pero no cifrado.
- **Se rompe**: la firma es **pública**, así que cualquiera con $pk_A$ lee $K_s$ y toda la comunicación posterior.
- **Arreglo de la guía**: firmar y cifrar, $E_B(S_A\{N_1, K_s\})$.
- **Para sumar puntos**: aun así la firma no dice **para quién** es $K_s$: $B$ podría reenviar $E_C(S_A\{N_1, K_s\})$ y hacerse pasar por $A$ frente a Carol (el problema de ⑥). Lo completo es $E_B(S_A\{B, N_1, K_s\})$. Y $N_1$ lo eligió $A$: a $B$ no le da frescura, falta un timestamp o un nonce de $B$.

#### ⑥ Distribución con servidor de claves públicas (Guía 4, ej 9)

$$1.\ A\to T: A, B \qquad 2.\ T\to A: S_T(B, pk_B),\ S_T(A, pk_A) \qquad 3.\ A\to B: E_B(S_A(K_s, T_A)),\ S_T(B,pk_B),\ S_T(A,pk_A)$$

- **Reconocerlo**: un tercero que **firma** pares (identidad, clave pública), o sea certificados caseros. Es el protocolo de clave pública de Denning-Sacco.
- **Qué hace $B$ (a)**: descifra con su privada y obtiene $S_A(K_s, T_A)$; saca $pk_A$ del certificado, verificado con $pk_T$; verifica la firma de $A$; chequea que $T_A$ sea reciente; usa $K_s$.
- **Se rompe (b) — masquerading**: lo que firmó $A$ no nombra al destinatario. $B$ se queda con $S_A(K_s, T_A)$, lo recifra para Carol y le manda $E_C(S_A(K_s, T_A))$ con los certificados. Carol cree que habla con $A$ (dentro de la ventana de $T_A$).
- **Arreglo**: $S_A(B, K_s, T_A)$, con el **destinatario adentro de la firma**.

#### ⑦ Diffie-Hellman anónimo (1C-2025)

- **Reconocerlo**: $g^x$, $g^y$ y nada firmado.
- **Garantiza**: secreto frente a un atacante **pasivo** (DDH).
- **Se rompe**: MITM (Mallory hace un DH con cada uno), parámetros sin autenticar, ninguna identidad. Resuelto en §2.

#### ⑧ Diffie-Hellman autenticado con firmas (Guía 4, ej 7)

$$1.\ A\to B: g^x \qquad 2.\ B\to A: B,\ cert_B,\ S_B(g^x, g^y),\ g^y \qquad 3.\ A\to B: A,\ cert_A,\ S_A(g^x, g^y)$$

- **Reconocerlo**: DH + firmas sobre los valores públicos + certificados. Es la base de TLS con (EC)DHE y de SSH.
- **Para qué las firmas (a)**: atan $g^y$ a $B$ y $g^x$ a $A$, así que el MITM de ⑦ deja de funcionar (Mallory no puede firmar un $g^m$ como $B$). Firmar **los dos** valores ata la firma a **esta** sesión, y una firma vieja no sirve.
- **Se rompe (b) — misbinding de identidad**: las firmas no nombran al **otro**. Mallory deja pasar 1 y 2 sin tocar y en el 3 cambia el mensaje de $A$ por $M,\ cert_M,\ S_M(g^x, g^y)$, que puede firmar porque vio $g^x$ y $g^y$ en claro. Al final $A$ cree que habla con $B$ y $B$ cree que habla con **Mallory**. Mallory no conoce $g^{xy}$, pero $B$ le atribuye a Mallory todo lo que mande $A$.
- **Arreglo**: firmar también la identidad del otro, $S_B(g^x, g^y, A)$ y $S_A(g^x, g^y, B)$, o probar que se tiene la clave (un MAC con $K$ sobre la identidad propia, como hace TLS 1.3, o cifrar las firmas con $K$, como STS).

#### ⑨ Diffie-Hellman a tres (Guía 4, ej 6)

$g^x \to g^{xy} \to g^{xyz}$: cada uno eleva lo que recibe y lo pasa, dos rondas. Sin autenticación, el mismo MITM.

#### ⑩ Needham-Schroeder simétrico, Denning-Sacco y Kerberos

- **Reconocerlo**: un **KDC** (Trent) que comparte una clave larga con cada uno ($K_{AT}$, $K_{BT}$), genera $k_s$ y devuelve un **token** $\{A, k_s\}_{K_{BT}}$ que $A$ reenvía sin poder leer.
- **Qué hace cada pieza** (lo que siempre preguntan):

| Pieza | Para qué |
|---|---|
| $r_1$ en 1 y adentro de 2 | $A$ sabe que 2 responde a **su** pedido (sin replay ni key reuse) |
| $B$ adentro de 2, cifrado | $A$ detecta si cambiaron $B$ por $E$ en el mensaje 1, que va en claro |
| $A$ adentro del token | $B$ sabe con quién comparte $k_s$ |
| $\{r_2\}_{k_s}$ y $\{r_2-1\}_{k_s}$ | challenge-response: $B$ comprueba que el otro tiene $k_s$; el $-1$ evita que le reflejen su propio mensaje 4 |
| $T$ en el token (Denning-Sacco) | $B$ rechaza tokens viejos: $\lvert Clock - T\rvert < \Delta t_1 + \Delta t_2$ |
| nonce de Bob que viaja al KDC (Guía 4, ej 5) | lo mismo sin relojes: el token trae un nonce que $B$ generó **antes** |

- **Se rompe (el original)**: $B$ entra recién en el paso 3 y **no puede saber si el token es fresco**. Con una $k_s$ vieja comprometida, Mallory reinyecta el token, contesta el desafío y se hace pasar por $A$.
- Detalle y verificación en [[Protocolos#Intercambio Simetrico]].

#### ⑪ Needham-Schroeder de clave pública — *extra, no está en las slides*

Es el ejemplo clásico de "falta la identidad adentro":

$$1.\ A\to B: E_B(N_A, A) \qquad 2.\ B\to A: E_A(N_A, N_B) \qquad 3.\ A\to B: E_B(N_B)$$

- **Se rompe (Lowe, 1995)**: $A$ abre una sesión legítima con Mallory. Mallory reenvía $E_B(N_A, A)$ a $B$; la respuesta de $B$, $E_A(N_A, N_B)$, se la pasa a $A$ como si fuera suya; $A$ la descifra y le devuelve $E_M(N_B)$. Mallory aprende $N_B$ y completa con $B$, que cree que habla con $A$.
- **Arreglo**: $E_A(N_A, N_B, B)$. $A$ esperaba "$M$", ve "$B$" y aborta.

> [!success] Verificado — en la simulación $B$ termina convencido de hablar con $A$ y Mallory conoce $N_B$.

#### ⑫ Handshake TLS (1C-2023)

- **Reconocerlo**: nonces de los dos lados ($N_C$, $N_S$) + certificado del servidor + un secreto que el cliente manda cifrado con la pública del servidor ($E_{pk_S}(K_0)$, el *pre-master*) o un DH firmado + una clave derivada $K = H(K_0, N_C, N_S)$ + mensajes finales con MAC de todo lo anterior (*Finished*).
- **Qué hace cada pieza**: nonces → clave nueva aunque se repita $K_0$; certificado → autentica al servidor (frena el MITM); derivación → frescura y claves separadas por uso; Finished → confirmación de clave y detectar si alguien tocó el handshake (por ejemplo, bajar la cipher suite).
- **Forward secrecy**: con transporte RSA **no** (si se filtra $sk_S$, se descifran todos los $K_0$ grabados); con DH efímero sí.
- Resuelto en §2 y §8.

### 13.4 Del enunciado al protocolo: señales

| Si ves… | Probablemente es… | Primera sospecha |
|---|---|---|
| $g^x$, $g^y$ sin firmas | DH anónimo (⑦) | MITM |
| $g^x$, $g^y$ firmados | DH autenticado (⑧) | ¿las firmas nombran al otro? → misbinding |
| KDC o $T$ con $K_{AT}$, $K_{BT}$; un bloque que $A$ no puede abrir | Needham-Schroeder / Kerberos (⑩) | ¿el token le da frescura a $B$? |
| $S_T(X, pk_X)$ | certificados caseros (⑥) | ¿lo que firmó $A$ nombra al destinatario? |
| claves públicas que viajan sueltas | intercambio sin certificados (④) | MITM |
| $S_A(\dots K_s \dots)$ sin cifrar | transporte firmado (⑤) | la clave queda pública |
| solo nonces y $\{N\}_K$ o $h_K(N)$ entre dos | challenge-response (①②③) | ¿es simétrico? → reflexión; ¿hay identidades adentro? |
| $E_B(N_A, A)$ y $E_A(N_A, N_B)$ | NS de clave pública (⑪) | Lowe |
| nonces de los dos + certificado + $E_{pk_S}(K_0)$ + MAC final | handshake TLS (⑫) | para qué cada pieza; forward secrecy |
| $T$ o `time` adentro de lo cifrado o firmado | Denning-Sacco / Kerberos | relojes sincronizados; ventana de replay |

### 13.5 Ataques: la señal, el guion y el arreglo

**Cómo escribir un ataque**: numerar las sesiones y escribir $M(A)$ para "Mallory haciéndose pasar por $A$", por ejemplo `1.1 A → M(B): N1` y `2.1 M(B) → A: N1`. Cerrar con **qué cree cada uno** al final y **qué propiedad se violó**.

| Ataque | Señal en el protocolo | Guion típico | Arreglo |
|---|---|---|---|
| **Replay** | el receptor no ve nada generado por él ni un timestamp protegido | reenviar un mensaje válido grabado (el cheque firmado que se cobra cien veces) | nonce propio, timestamp, número de secuencia |
| **Reflexión / sesiones paralelas** | protocolo simétrico: los dos hacen la misma cuenta con la misma clave, sin identidades | devolverle a $A$ su propio desafío, o abrir otra sesión para que $A$ calcule la respuesta | identidades en la respuesta, claves por dirección, rechazar el nonce propio |
| **Key reuse** | la respuesta del servidor no está atada al pedido | reinyectar un mensaje 2 viejo para forzar una $k_s$ vieja | nonce en el pedido que vuelve adentro de la respuesta |
| **MITM** | claves públicas o $g^x$ sin autenticar | Mallory sustituye la clave de cada lado y queda de relevo | certificados, firmas sobre los valores públicos, secreto precompartido |
| **Masquerading / misbinding** | lo firmado o cifrado no nombra al **destinatario** o al **otro** | reenviar lo que firmó $A$ a un tercero (⑥), cambiar la identidad en el último mensaje (⑧) | la identidad del otro **adentro** de la firma, el MAC o el cifrado |
| **Oráculo** | una parte transforma con su clave cualquier cosa que le manden | usar a la víctima para que firme o cifre lo que el atacante necesita | un nonce propio en lo que se firma; separar roles |
| **Clave vieja comprometida** | tokens sin fecha o sin forward secrecy | con una $k_s$ o una $sk$ filtrada, reusar tokens o descifrar tráfico grabado | timestamps; DH efímero |

> [!tip] La pregunta que resuelve la mayoría
> Por cada mensaje: **¿está atado a (a) quién lo manda, (b) a quién va, (c) a qué sesión y (d) a cuándo?** Si falta (a) → masquerading. Si falta (b) → reenvío a un tercero, Lowe, misbinding. Si falta (c) → replay entre sesiones, reflexión. Si falta (d) → replay a largo plazo.

### 13.6 Modos de bloque inventados: reconocerlos por la forma

Tres preguntas, en este orden (es la receta de §3 con atajos):

1. **¿Qué entra a $E_k$?** Si en algún bloque la entrada depende **solo de $M$** (sin IV ni nada aleatorio), ese bloque es ECB → determinístico → **no CPA**. Si además dos bloques del **mismo** mensaje se pueden igualar, ni siquiera es **EAV** (alcanza un desafío, sin oráculo).
2. **¿Qué se XORea con $M$?** Si es un keystream que se repite entre bloques o entre mensajes → $C \oplus C' = M \oplus M'$, un OTP reusado.
3. **¿Se cancela el encadenamiento con valores públicos?** Combinar $C_i$, $C_{i-1}$ e $IV$ para aislar $E_k(\text{algo que depende solo de } M_i)$. Si se puede, el encadenamiento es cosmético.

Para **errores**: escribir el descifrado de $M_i$, $M_{i+1}$ y $M_{i+2}$ y seguir a $C_i$. Si pasa por $D_k$ → bloque entero. Si solo entra en un XOR → el mismo bit. Si el $M$ ya descifrado entra en el cálculo del siguiente → **se propaga para siempre**.

| Forma | Qué es en realidad | ¿Válido? | ¿CPA-secure? | Un bit mal en $C_i$ |
|---|---|---|---|---|
| $C_i = E_k(M_i\oplus C_{i-1})$ | **CBC** | sí: $M_i = D_k(C_i)\oplus C_{i-1}$ | sí, con IV aleatorio e impredecible | $M_i$ entero + mismo bit de $M_{i+1}$ |
| $C_i = E_k(M_i)\oplus C_{i-1}$ | **ECB disfrazado**: $C_i\oplus C_{i-1} = E_k(M_i)$ | sí | **no**: $X\|X$ vs $X\|Y$, mirar si $C_2\oplus C_1 = C_1\oplus C_0$ | $M_i$ y $M_{i+1}$ enteros |
| $C_i = M_i\oplus E_k(C_{i-1})$ | **CFB** | sí, sin $D_k$ | sí, con IV aleatorio | mismo bit de $M_i$ + $M_{i+1}$ entero |
| $S_i = E_k(S_{i-1})$, $C_i = M_i\oplus S_i$ | **OFB** | sí, sin $D_k$ | sí, IV nunca repetido | solo ese bit |
| $C_i = M_i\oplus E_k(\text{nonce}\,\|\,i)$ | **CTR** | sí, sin $D_k$ | sí, $(k,\text{nonce})$ nunca repetido | solo ese bit |
| $C_i = M_i\oplus E_k(IV)$, el mismo pad en todos los bloques | OTP reusado dentro del mensaje | sí | **no, ni EAV**: $C_1\oplus C_2 = M_1\oplus M_2$; con $X\|X$ vs $X\|Y$ se ve si $C_1 = C_2$ | solo ese bit |
| $C_i = E_k(M_i\oplus IV)$, el mismo IV en todos los bloques | ECB con una máscara fija | sí | **no, ni EAV**: bloques iguales → cifrados iguales ($X\|X$ vs $X\|Y$) | solo $M_i$, entero |
| $C_i = E_k(M_i\oplus i)$, sin IV | ECB con una máscara conocida | sí | **no**: determinístico | solo $M_i$, entero |
| $C_i = E_k(M_i\oplus M_{i-1})$, $M_0 = IV$ | "CBC con el plano" | sí, en orden: $M_i = D_k(C_i)\oplus M_{i-1}$ | **no**: desde el bloque 2 la entrada no depende del IV; pedir $Enc(X\|Y)$ y comparar $C_2$ | $M_i$ y **todos** los siguientes |
| $C_i = M_i\oplus E_k(M_{i-1})$, $M_0 = IV$ | keystream que depende del plano anterior | sí, en orden | **no**: $Enc(X\|X)$ entrega $E_k(X) = C_2\oplus X$; con $X\|Y$ vs $X\|Y'$ se descifra el bloque 2 | mismo bit de $M_i$ + **todos** los siguientes |
| $C_i = E_k(M_i\oplus M_{i-1}\oplus C_{i-1})$ | **PCBC** (existe: Kerberos v4) | sí | se argumenta como CBC: la entrada lleva $C_{i-1}$, impredecible | $M_i$ y **todos** los siguientes |
| CBC con IV = último $C$ del mensaje anterior | CBC con IV **predecible** | sí | **no** (BEAST): si el próximo IV es $IV'$, pedir $Enc(IV'\oplus IV_c\oplus m_0)$ y comparar con el $C_1$ del desafío | como CBC |

> [!success] Verificado
> Simulé las doce formas con una permutación aleatoria de 8 bits como $E_k$: todas descifran bien, los ataques de la columna CPA ganan los 2000 juegos (CBC con IV aleatorio, como control, queda en ~50 %) y la propagación de errores sale como dice la última columna.

> [!tip] Cómo escribir "no es CPA-secure"
> Nombrar $m_0$ y $m_1$ del mismo largo, decir **qué se le pide al oráculo** (si hace falta), **qué se compara** en el desafío y cerrar con $\Pr[\text{PrivK}^{cpa}_{A,\Pi}=1] = 1 > \tfrac12 + negl(n)$. Si el ataque no usa el oráculo, agregar que ni siquiera es EAV-secure para mensajes de más de un bloque.

### 13.7 Mostrar que se filtra información sin la clave

Es el argumento para los ejercicios donde no hay probabilidades que calcular: **encontrar algo que se calcula sobre el cifrado, donde la clave se cancela, y que depende del mensaje.**

> [!example] Receta del invariante
> 1. **Buscar** una función del cifrado $f(c)$ que sea igual a una función del mensaje $g(m)$ **para toda clave**. Dónde mirar: restas o XOR entre posiciones que usan la misma parte de la clave, igualdades entre bloques, histogramas, largos.
> 2. **Demostrarlo** con álgebra: escribir $c$ en función de $m$ y $k$ y mostrar que $k$ desaparece. Por ejemplo, $c_j - c_{j+l} = (m_j + k) - (m_{j+l} + k) = m_j - m_{j+l}$.
> 3. **Elegir** $m_0$ y $m_1$ del mismo largo con $g(m_0) \neq g(m_1)$.
> 4. **Adversario**: calcula $f(c)$ y responde 0 si coincide con $g(m_0)$. Gana con probabilidad 1.
> 5. **Concluir según el modelo**:
>    - si alcanza con **un** cifrado → no hay secreto perfecto (forma 4 de §4) y tampoco es EAV-secure;
>    - si hacen falta **dos cifrados con la misma clave** → no es seguro para múltiples cifrados;
>    - si hace falta el **oráculo** → no es CPA-secure.
> 6. **Decir en palabras qué se filtra**: "qué posiciones son iguales", "la diferencia entre letras", "el XOR de los dos planos".

| Esquema | Invariante (no depende de $k$) | Qué revela |
|---|---|---|
| Sustitución monoalfabética | $c_i = c_j \iff m_i = m_j$; las alturas del histograma | patrón de repeticiones, frecuencias |
| Transposición | el histograma de $c$ es **idéntico** al de $m$ | qué letras tiene el mensaje |
| César / rotación | $c_i - c_j \equiv m_i - m_j$ | el mensaje, salvo una rotación |
| Vigenère de período $l < n$ | $c_j - c_{j+l} \equiv m_j - m_{j+l}$ | igualdades y diferencias entre letras a distancia $l$ |
| Afín $c = am+b$ | $c_i = c_j \iff m_i = m_j$; $\frac{c_1-c_2}{c_3-c_4} \equiv \frac{m_1-m_2}{m_3-m_4}$ (si el denominador es invertible mod 26) | cocientes de diferencias |
| Hill o cualquier cifrado lineal $c = Km$ | $Enc(m_1+m_2) = Enc(m_1)+Enc(m_2)$; $m = 0 \Rightarrow c = 0$ | relaciones lineales; con pocos pares sale $K$ |
| XOR con la misma clave (OTP reusado, CTR u OFB con nonce repetido) | $c\oplus c' = m\oplus m'$ | el XOR de los planos; con uno conocido sale el otro |
| ECB o cualquier esquema determinístico | bloques iguales → cifrados iguales; mismo $m$ → mismo $c$ | repeticiones |
| Cualquier esquema | $\lvert c\rvert$ depende de $\lvert m\rvert$ | el largo (por eso EAV exige $\lvert m_0\rvert = \lvert m_1\rvert$) |
| Textbook RSA *(extra)* | determinístico; $(m_1m_2)^e = m_1^e\,m_2^e$; símbolo de Jacobi $\left(\frac{c}{N}\right) = \left(\frac{m}{N}\right)$ porque $e$ es impar | un bit de $m$ sin factorizar |
| DH en $\mathbb{Z}_p^*$ completo *(extra)* | $g^{xy}$ es residuo cuadrático $\iff$ $g^x$ o $g^y$ lo es | distingue $g^{xy}$ de un valor al azar → DDH falla ahí; por eso se usa un **subgrupo de orden primo** |

> [!success] Verificado con Python
> Los invariantes del afín, Vigenère, XOR reusado, Jacobi en RSA y residuo cuadrático en DH, sobre 2000 casos aleatorios cada uno con claves y primos al azar.

> [!example] Modelo de respuesta completa (Vigenère, 2C-2025 ej 4, con $l < n$)
> *"Si $l < n$, las letras $j$ y $j+l$ usan la misma letra de la clave, entonces $c_j - c_{j+l} \equiv (m_j + k) - (m_{j+l} + k) \equiv m_j - m_{j+l} \pmod{26}$, que no depende de la clave. Tomo $m_0 = \texttt{a}^{l+1}$ y $m_1 = \texttt{a}^{l}\texttt{b}$. El adversario recibe $c$ y responde 0 si $c_1 = c_{l+1}$ y 1 si no. Si $b = 0$ la diferencia es 0 y acierta; si $b = 1$ la diferencia es $-1 \neq 0$ y también acierta. $\Pr[\text{PrivK}^{eav}_{A,\Pi} = 1] = 1 \neq \tfrac12$, así que no hay secreto perfecto. Se filtra la diferencia entre letras a distancia $l$."*

> [!note] Cuándo el invariante **no** existe
> Si cada símbolo usa una parte de la clave distinta, uniforme y usada una sola vez (OTP, Vigenère con $l \ge n$, el esquema $(r, ar+b+m)$ con un solo cifrado), cualquier resta o XOR entre posiciones sigue teniendo clave adentro. Ahí no se busca un invariante: se prueba el secreto perfecto con la forma 3 de §4.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Criptografía y Seguridad)**

- [[Cripto - Resumen Teórico Primer Parcial]] — la contraparte teórica: definiciones, experimentos, teoremas y tablas transversales que justifican cada receta de esta nota
- [[Criptografia y seguridad intro]] — Clase 1: criptosistema, clásicos y Kasiski; la base del §6
- [[Guia 1 - criptografia y seguridad]] — ejercicios de clásicos resueltos a mano, práctica directa del §6
- [[Criptografia y seguridad Clase 2 - Cifrado]] — Clase 2: secreto perfecto, EAV/CPA y los modos de bloque; la teoría del §3, §4 y §5
- [[Clase 3 - Criptografia - MACs y modo autenticado]] — Clase 3: maleabilidad, CCA, CBC-MAC, hash y Encrypt-then-MAC; la teoría del §7
- [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital]] — Clase 4: DH, RSA, ElGamal, firmas y el mini-resumen de la Guía 4; la teoría del §2 y §8
- [[Protocolos]] — Clase 5: PKI, Needham-Schroeder, TLS y Shamir, con las preguntas de parciales de esa clase resueltas en detalle
- [[Materia - Criptografía y Seguridad]] — índice de la materia

**Otras materias**

- **EDA** — [[EDA - Hashing]] — el mismo nombre, otro requisito: a un hash de tabla le alcanza con repartir uniforme; a uno criptográfico se le exige que encontrar colisiones sea inviable (§7)
- **Protos** — [[2. Protos - HTTP]] — HTTPS es TLS: certificados, pre-master y Finished del §8 funcionando juntos
- **Protos** — [[9. Protos - SSH]] — SSH resuelve el MITM de Diffie-Hellman firmando el intercambio con la host key, el arreglo del §2

<!-- notas-relacionadas:fin -->
