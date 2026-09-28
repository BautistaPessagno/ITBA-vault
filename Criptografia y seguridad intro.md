---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-0616:03
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
temas:
  - Criptosistema
  - Principio de Kerckhoffs
  - Criptoanálisis
  - Cifrado por rotación (ROT-X)
  - Cifrado de sustitución
  - Análisis de frecuencias
  - Cifrado de Vigenère
  - Test de Kasiski
  - Sustitución polialfabética
  - Enigma
  - Secreto perfecto
---
**# Criptografia y seguridad intro
Usos:
- Comunicaciones seguras
- trafico web
- protección de archivos en disco
- autenticación de usuarios
- proteccion de contenido

# Criptosistema
![](Attachments/Pasted%20image%2020260806162752.png)
conjunto de dos funciones de las cuales
es necesario una clave
### Clave
![](Attachments/Pasted%20image%2020260806163155.png)

## Funcion de cifrado
![](Attachments/Pasted%20image%2020260806163401.png)
en algun lugar se tiene que correr el codigo de la funcion

de esta forma no se puede acceder al mensaje sin la clave

## Seguridad (informal)
![imagen|641](Attachments/Pasted%20image%2020260806163954.png)

# Historia
![](Attachments/Pasted%20image%2020260806164710.png)


# Cifrado por rotacion
![](Attachments/Pasted%20image%2020260806165127.png)
![imagen|651](Attachments/Pasted%20image%2020260806165901.png)

entonces ROT-X es inseguro
![](Attachments/Pasted%20image%2020260806170953.png)

# Cifrado de sustitucion
por cada simbolo del lenguaje se define por un simbolo por el cual se remplaza

![](Attachments/Pasted%20image%2020260806171040.png)

la combinatoria es 26!
el problema que tiene es la repeticion (vease el cifrado el pasaje de e a c)

![](Attachments/Pasted%20image%2020260806171847.png)

![](Attachments/Pasted%20image%2020260806172116.png)

# Cifrado vigente

![](Attachments/Pasted%20image%2020260806181558.png)

el polialfabetico se basa en el de rotacion pero en lugar de totar todo el mensaje se divide en grupos (ej de 4) y a cada uno se le da uno una variable de rotacion distinta
en la imnagen a la primera letra se la rota 4 a la siguiente 2, 5, 3 y para las proximas 4 letras se repite

## Criptoanalisis
hay secuencias de letras que aparecen multiples veces. las secuencias de letras iguales, se corresponden. puedo conseguir el tamaño de bloque de esa manera

![](Attachments/Pasted%20image%2020260806182904.png)

cuanto mas largo el mensaje mayor facilidad para encontrar el mensaje

# Sustitucion polialfabetica
![](Attachments/Pasted%20image%2020260806183655.png)


# Criptosistema (Definicion)

se lo define como una terna de algoritmos

generador, cifrado, descifrado

![](Attachments/Pasted%20image%2020260806184308.png)

## seguridad: secreto perfecto

![](Attachments/Pasted%20image%2020260806184551.png)

# Resumen

Resumen completo de la presentación **Clase 01 — Criptografía: Introducción** (35 diapositivas).

## 1. Qué es la criptografía

Del griego: **escritura secreta**. Tres maneras complementarias de definirla, según la clase:

- Un **conjunto de funciones matemáticas y técnicas**.
- Las **herramientas básicas** desde donde se construye seguridad (no *es* la seguridad: es el ladrillo).

> [!note] Criptografía ≠ seguridad
> La criptografía provee primitivas. Un sistema seguro se construye *con* ellas, pero puede ser inseguro aunque use criptografía correcta (mal manejo de claves, mala implementación, etc.).

### Usos

| Uso | Ejemplo de la clase |
|---|---|
| Comunicaciones seguras | Tráfico web, tráfico inalámbrico, enlaces satelitales |
| Protección de archivos en disco | Cifrado de disco / archivos |
| Autenticación de usuarios | Password + token OTP (CRYPTOCard) |
| Protección de contenido | DRM en un e-reader (Kindle) |
| Firmas digitales | Firma electrónica de documentos |
| Voto electrónico | Urna electrónica con comprobante |
| Dinero electrónico | Bitcoin |

## 2. Criptosistema (visión informal)

```
mensaje plano ──► [ CIFRAR ] ──► mensaje cifrado ──► [ DESCIFRAR ] ──► mensaje plano
                      ▲                                     ▲
                    clave ─────────────────────────────────┘
```

- **Cifrar / encriptar**: pasar de texto plano a texto cifrado.
- **Descifrar / desencriptar**: la operación inversa.
- Ejemplo de la clase: `"Nos vemos a las 18"` → `"skjejcaeidl dsfjkfj ieu olkjds kjkli"` → `"Nos vemos a las 18"`.

### Función de cifrado

Función de **2 parámetros**: `mensaje plano × clave → mensaje cifrado`

$$e_k(p) = c$$

> [!info] Distintas notaciones, mismo significado
> $e_k(p) = enc_k(p) = e(k, p) = \{\,p\,\}_k$

### Función de descifrado

También de **2 parámetros**: `mensaje cifrado × clave → mensaje plano`

$$d(c, k) = p$$

> [!info] Distintas notaciones, mismo significado
> $d_k(c) = dec_k(c) = d(k, c) = \{\,c\,\}^{-1}_k$

### La clave

Es un **bloque arbitrario de información**. Al analizar la seguridad de un sistema **se presupone que la clave está protegida** (si el atacante tiene la clave, no hay nada que discutir).

> [!important] Principio de Kerckhoffs
> Un criptosistema debe ser seguro **incluso si todo sobre el sistema, excepto la clave, es de público conocimiento**.

Consecuencia práctica: la seguridad **nunca** puede descansar en mantener secreto el algoritmo (*security through obscurity*). Solo la clave es secreta.

### Uso básico

El texto cifrado **no tiene información útil**, por lo que puede almacenarse o transmitirse libremente:

- **Sin la clave** → se obtiene basura (`hsdfudsahj sjhguy ydsiud gsjw`).
- **Con la clave** → se recupera el mensaje (`Nos vemos a las 18`).

Es decir: un criptosistema permite **controlar *quién* puede recuperar un mensaje**.

## 3. Seguridad (definición informal)

> [!important] Definición informal
> Un criptosistema es **seguro** si ningún adversario puede computar **cualquier función** del texto plano a partir del mensaje cifrado que posee.

Significa que a partir del cifrado no se puede:

- recuperar el mensaje,
- recuperar **parte** del mensaje,
- recuperar el **sentido** del mensaje.

> [!note] Criptoanálisis
> Las técnicas para probar o romper la seguridad de un criptosistema se denominan **criptoanálisis**.

La exigencia de "cualquier función" es fuerte a propósito: filtrar la longitud, el idioma o si el mensaje es "sí" o "no" ya rompe la definición.

## 4. Historia de la criptografía

| Fecha | Hito | Qué aporta |
|---|---|---|
| ~2500 AC | **Egipto** | Criptosistemas de sustitución: jeroglíficos no estándares |
| ~700–300 AC | **Scítala** | Criptosistema de **transposición** usando báculos |
| 50 AC | **César** | Criptosistema de **rotación** (subtipo de sustitución) |
| 800 DC | **Primeros documentos de criptoanálisis** | **Análisis de frecuencias** → empiezan a "romperse" los criptosistemas conocidos |
| 1500 | **Criptosistemas polialfabéticos** | Un símbolo cifrado representa distintos símbolos del mensaje original |
| 1939 | **Ataque de fuerza bruta: Bombe** | Enigma — exploración sistemática por fuerza bruta con protocomputadoras |
| 1949 | **Information Theory & Cryptography (Shannon)** | Publicaciones seminales que inician la **criptografía moderna** |

> [!warning] Precisión histórica
> La diapositiva rotula 1939 como "Bombe – Shannon". En rigor son dos cosas distintas: la **Bombe** es de Turing (1939-40), apoyada en la *bomba kryptologiczna* de Rejewski (1938); **Shannon** publica *Communication Theory of Secrecy Systems* recién en **1949** (la fila siguiente de la línea de tiempo).

Patrón que se repite en toda la historia: **cada criptosistema se considera seguro hasta que alguien publica cómo romperlo.**

## 5. Cifrado por rotación (ROT-X / César)

Cada letra se trata como un **número** = su posición en el alfabeto, **empezando en 0**.

- La clave `k` es un número entre **1 y 26** (cantidad de letras − 1 → la clase asume el alfabeto castellano de 27 símbolos, con `ñ`).
- **Cifrado**: reemplazar cada letra por la que está `k` posiciones más adelante, volviendo a la `a` después de la `z`.

$$e_k(p_i) = (p_i + k) \bmod n \qquad d_k(c_i) = (c_i - k) \bmod n$$

**Ejemplo**: `e(prueba, 4) = tvyife`

| p | r | u | e | b | a |
|---|---|---|---|---|---|
| t | v | y | i | f | e |

### Criptoanálisis: fuerza bruta

> [!fail] Debilidad
> **Hay muy pocas claves** (a lo sumo 26). El espacio de claves se recorre entero a mano.

Prueba y error sobre `c = tvyife`:

| k | `d(k, tvyife)` | ¿Tiene sentido? |
|---|---|---|
| 1 | suxhed | ✗ |
| 2 | rtwgdc | ✗ |
| 3 | qsvfcb | ✗ |
| 4 | **prueba** | ✓ |

→ `k = 4`, `m = prueba`.

### Límites de la fuerza bruta

¿ROT-X es inseguro? **Informalmente, sí** — pero conviene ver por qué la fuerza bruta no siempre alcanza.

> [!question] Contraejemplo de la clase
> Con ROT-X, ¿cuál es el mensaje original `p` si `e(p) = a`?
> Cualquier letra del alfabeto es un descifrado "válido": todas las candidatas son mensajes legítimos y no hay forma de elegir. Con mensajes de un símbolo, ROT-X **no filtra información**.

> [!important] Hipótesis de fuerza bruta
> El ataque por fuerza bruta funciona **solo si se puede discriminar un descifrado válido de uno inválido**. Si todos los candidatos son plausibles, enumerar claves no sirve de nada.

Esta observación es la semilla del **secreto perfecto** (sección 9).

## 6. Cifrado de sustitución (monoalfabético)

Reemplaza **un símbolo por otro**: la clave es una **permutación completa del alfabeto**.

```
k =  d u b l c m f t h i j n z p x q e a o s v k r w g y
     ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕
     a b c d e f g h i j k l m n o p q r s t u v w x y z
```

| | |
|---|---|
| Mensaje | `esto es una prueba` |
| Cifrado | `cosx co vpd qavcud` |

- **Espacio de claves**: $26! \approx 4\times10^{26}$ → la fuerza bruta es inviable.
- Los **símbolos pueden ser cualquier cosa**, no solo letras. Ejemplo de la clase: el criptograma hallado en una tumba del cementerio de **Trinity (NY, 1794)**, escrito con símbolos tipo *pigpen* y descifrado recién en **1986** con la tabla de traducción correspondiente.

### Criptoanálisis: análisis de frecuencias

> [!fail] Debilidad
> **Las propiedades estadísticas del lenguaje no se ven alteradas.** Un espacio de claves enorme no sirve de nada si el cifrado preserva la estructura del idioma.

Procedimiento:

1. Obtener la **frecuencia estimada** de cada símbolo en el **lenguaje** del mensaje.
2. Calcular la **frecuencia de cada símbolo en el texto cifrado**.
3. Asumir que los símbolos de mayor probabilidad **se corresponden**.
4. Formar **grupos de dos y tres letras comunes** (`el`, `la`, `de`, `las`, `los`, etc.) para confirmar o corregir hipótesis.

#### Tabla de frecuencias del español (%)

| Frec. Alta | | Frec. Media | | Frec. Baja | |
|---|---|---|---|---|---|
| E | 13,11 | C | 4,85 | Y | 0,79 |
| A | 10,60 | L | 4,42 | Q | 0,74 |
| S | 8,47 | U | 4,34 | H | 0,60 |
| O | 8,23 | M | 3,11 | Z | 0,26 |
| I | 7,16 | P | 2,71 | J | 0,25 |
| N | 7,14 | G | 1,40 | X | 0,15 |
| R | 6,95 | B | 1,16 | W | 0,12 |
| D | 5,87 | F | 1,13 | K | 0,11 |
| T | 5,40 | V | 0,82 | Ñ | 0,10 |

> [!tip] Regla práctica
> `E` y `A` juntas son ~24 % del texto; las 9 letras de frecuencia alta suman ~73 %. Por eso alcanza con un texto relativamente corto para arrancar el ataque.

> [!todo] Ejercicio de la clase (pendiente)
> Descifrar el criptograma de símbolos (♦ ♥ ♣ ♠ ☺ y dígitos), agrupado de a 5.
> - El mensaje original está en **castellano**.
> - La separación en grupos de 5 símbolos **no es parte del problema**, solo ayuda a contar.
> - **Gancho**: `LACABEZA` aparece en el mensaje plano → buscar el patrón `_A_A___A` (mismo símbolo en las posiciones 2, 4 y 8 de una ventana de 8).

## 7. Cifrado de Vigenère

Es una **sustitución polialfabética**: un mismo símbolo puede transformarse en distintos símbolos según su posición.

- La clave está compuesta por **n números**.
- Se aplica **ROT-X según la clave**, rotando cíclicamente.

**Ejemplo**: `K = ECFD = 4253` (longitud 4)

```
esto es una prueba
425342534253425342
iuyrdgxcyofcttzhfc
```

> [!note] Detalle del ejemplo
> El ejemplo desplaza también los espacios: usa un alfabeto de **27 símbolos** con el espacio en la posición 0 y `a` en la 1. Verificado símbolo a símbolo: `e(5)+4=9=i`, `s(19)+2=21=u`, `t(20)+5=25=y`, `o(15)+3=18=r`, `espacio(0)+4=4=d`, … `a(1)+2=3=c`.

### Historia

| Año | Hecho |
|---|---|
| 1553 | Se crea el cifrado |
| 1553–1863 | Durante casi **300 años** se lo consideró seguro (*le chiffre indéchiffrable*) |
| 1863 | **Friedrich Kasiski** publica un método para resolverlo |

> [!warning] Precisión histórica
> Lo publicado en **1553** es el cifrado de **Giovan Battista Bellaso**; el nombre "Vigenère" es una atribución errónea posterior (Blaise de Vigenère describe su cifrado *autokey* en 1586). Además, **Charles Babbage** lo había roto hacia 1854, pero no lo publicó — de ahí que el crédito quede en Kasiski (1863).

### Criptoanálisis de Vigenère

**Estrategia general** (divide y vencerás):

1. **Determinar la longitud del bloque** (largo de la clave).
2. **Analizar la clave de cada bloque por separado** → cada posición es un ROT-X independiente, y a cada uno se le aplica análisis de frecuencias.

#### Paso 1 — Test de Kasiski

Aparecen **n-gramas repetidos** cuando los mismos símbolos caen en la misma posición relativa de la clave.

1. Buscar **secuencias repetidas** en el texto cifrado.
2. Calcular la **distancia** entre cada par de repeticiones.
3. La **longitud de la clave es divisor del MCD** de las distancias halladas.

**Ejemplo de la clase**:

| n-grama | Repeticiones | Distancias |
|---|---|---|
| GGMP | 3 veces | 256 y 104 |
| YEDS | 2 veces | 72 |
| HASE | 2 veces | 156 |
| VSUE | 2 veces | 32 |

$$\text{MCD}(32,\,72,\,104,\,156,\,256) = 4$$

→ longitud de clave = **4**. *(Verificado: mcd(32,72)=8, mcd(8,104)=8, mcd(8,156)=4, mcd(4,256)=4.)*

> [!tip] Cuanto más largo el mensaje, más fácil el ataque
> Textos largos producen más repeticiones (y más muestras por cada grupo), lo que vuelve confiables tanto Kasiski como el análisis estadístico posterior.

#### Paso 2 — Método de coincidencia mutua

1. Determinar **estadísticamente** las letras más probables en cada grupo (posición módulo n).
2. **Estimar las rotaciones** que llevan a cada grupo a equipararse con las estadísticas del lenguaje.
3. **Probar las rotaciones**, eliminando caminos al encontrar combinaciones sin sentido.

## 8. Sustitución polialfabética: Enigma

La **máquina Enigma** lleva la idea polialfabética al extremo mecánico:

- Tres (o más) **rotores** de 26 posiciones → tres permutaciones en una sola pasada.
- Los rotores **avanzan con cada tecla**, así que el alfabeto de sustitución cambia en cada símbolo (período enorme, no 4 como en el ejemplo de Vigenère).
- Un **plugboard** agrega una permutación fija adicional antes y después de los rotores.
- La salida se muestra en un tablero de lámparas.

Fue el motor del hito de 1939 en la línea de tiempo: romperla requirió **ataques de exploración sistemática por fuerza bruta con protocomputadoras** (la Bombe).

## 9. ¿Hay criptosistemas seguros?

Primero hay que **definir qué consideramos un criptosistema seguro**.

Por mucho tiempo *"algo era seguro si no lo lograban analizar"* — una definición negativa y frágil (depende de quién lo intentó y con cuántos recursos).

Los avances de la **2ª Guerra Mundial** llevan a la **criptografía moderna**, que aporta:

- **Representaciones formales** de criptosistemas.
- **Definiciones del modelo de amenaza** (qué puede hacer el adversario).
- **Pruebas formales de seguridad**.

> [!warning] No hay una única definición de "es seguro"
> La seguridad se enuncia **siempre relativa a un modelo de amenaza**. "Seguro" sin decir contra qué adversario no significa nada.

### Criptosistema (definición formal)

Es una **terna de algoritmos**:

| Algoritmo | Firma | Rol |
|---|---|---|
| `Gen` | $() \rightarrow K$ | Generador de clave |
| `Enc` | $K \times P \rightarrow C$ | Cifrado |
| `Dec` | $K \times C \rightarrow P$ | Descifrado |

**Propiedad básica (corrección)**: para todo `m` y `k` válidos

$$d_k(e_k(m)) = m$$

> [!info] Conjuntos involucrados
> - **K**, *espacio de claves*: el conjunto de todas las **claves** posibles.
> - **P**, *espacio plano*: el conjunto de todos los **mensajes** posibles.
> - **C**, *espacio cifrado*: el conjunto de todos los **mensajes cifrados** posibles.

Nótese que `Gen` es parte del criptosistema: **cómo se generan las claves es tan parte del diseño como cifrar y descifrar**.

### Seguridad: secreto perfecto

Dado un criptosistema `Gen`, `e` y `d`:

> [!important] Secreto perfecto
> El criptosistema posee **secreto perfecto** si para toda distribución de probabilidades en `M`, cada mensaje `m` y cada mensaje cifrado `c` tal que $\Pr[C=c] > 0$:
> $$\Pr[M = m \mid C = c] = \Pr[M = m]$$

Interpretación: **ver el texto cifrado no cambia en nada lo que uno cree sobre el mensaje**. El cifrado no aporta *ninguna* información sobre el plano — es la formalización de la definición informal de la sección 3.

> [!question] La paradoja del secreto perfecto
> Notar que $c = e_k(m)$, así que las variables aleatorias discretas `C` y `M` son **dependientes** (una se calcula a partir de la otra). Sin embargo, la propiedad de secreto perfecto, interpretada probabilísticamente, dice que son **V.A.D. independientes**.
>
> *Resolución*: la dependencia está mediada por la clave `k`, que es aleatoria y desconocida. Promediando sobre todas las claves posibles, la distribución condicional se vuelve idéntica a la marginal.

Conecta directo con los "límites de la fuerza bruta" (sección 5): si todo descifrado es igual de plausible, no hay nada que discriminar.

## 10. Ideas clave para llevarse

1. La criptografía provee **herramientas**; la seguridad se **construye** con ellas.
2. **Kerckhoffs**: lo único secreto es la clave. Nunca el algoritmo.
3. Un criptosistema es **cifrar + descifrar + generar clave**, con $d_k(e_k(m)) = m$.
4. **Espacio de claves grande ≠ seguro**: la sustitución tiene 26! claves y cae con análisis de frecuencias.
5. La debilidad de ROT-X es el **espacio de claves chico**; la de la sustitución es que **preserva la estadística del lenguaje**; la de Vigenère es que **repite la clave** (Kasiski).
6. La fuerza bruta requiere poder **distinguir un descifrado válido de uno inválido**. Donde eso falla, aparece el **secreto perfecto**.
7. "Seguro" solo tiene sentido **relativo a un modelo de amenaza**, con una **definición formal** y una **prueba**.

## Preguntas

- ¿Por qué un espacio de claves de 26! no alcanza para hacer seguro al cifrado de sustitución?
- ¿En qué situación falla la hipótesis de fuerza bruta? ¿Cómo se relaciona con el secreto perfecto?
- ¿Por qué el test de Kasiski da un **múltiplo/divisor** del largo de clave y no el largo exacto?
- ¿Cómo se resuelve la paradoja de que `C` y `M` sean dependientes pero probabilísticamente independientes?
- ¿Por qué el principio de Kerckhoffs implica que *security through obscurity* no es una estrategia válida?

## Lectura recomendada

**Introduction to Modern Cryptography** — Katz & Lindell, **Capítulo 1**.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Criptografía y Seguridad)**

- [Materia - Criptografía y Seguridad](Materia%20-%20Criptografía%20y%20Seguridad.md) — índice de la materia
- [Guia 1 - criptografia y seguridad](Guia%201%20-%20criptografia%20y%20seguridad.md) — guía de ejercicios sobre estos cifrados clásicos
- [Practica 1 - criptografia y seguridad](Practica%201%20-%20criptografia%20y%20seguridad.md) — práctica asociada a esta clase

<!-- notas-relacionadas:fin -->