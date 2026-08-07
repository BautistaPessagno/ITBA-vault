---
temas:
  - Unidad 2
  - transmisión digital
  - codificación de línea
  - arquitectura Von Neumann y Harvard
  - sistema de entrada y salida
  - mapa de memoria
  - memorias ROM y RAM
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
> [!info]+ Nota fusionada
> Reúne los apuntes de esta clase de las dos cursadas (1ro-2025 y 2do-2025), que cubrían el mismo tema. Las capturas de una y otra aparecen intercaladas.

# Intro a Transmisión Digital

![[image 282.png]]

todos los periféricos (ram rom, placa de wifi, de video, teclado, mouse, etc) hablan con procesador con 1 y 0s

## Codificación de linea

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.20.45.png]]

## Codificación unipolar

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.21.02.png]]

## Transmicion en serie y paralela

![[image 283.png]]

**Transmisión serie:** un dato atras del otro

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.34.07.png]]

**Transmisión paralela:** llega todo junto

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.34.51.png]]

me permite acceder a datos mas rapido

hoy en día es el que mas se usa

# Tipo de arquitectura

## Von Newman

memoria de código y datos se usa el mismo bus

se arma. un cuello de botella, se satura facilmente

es que me se usa porque es mas barato y no se necita dividir en dos memorias

un solo tipo de memoria para todo (memoria ram) — es la que se usa hoy en dia

![[image 284.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.41.30.png]]

## Harvard

se usan dos buses de datos, menos saturación

es mas caro y requiere dos tipo de memoria

![[image 285.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.41.48.png]]

# Sistema de Entrada y Salida

![[image 286.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.49.13.png]]

Un unico Bus por que todos se cuelgan

## CPU

![[image 287.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.00.00.png]]

### Resumen

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.49.32.png]]

- **Unidad de Control:** Recupera Instrucciones de memoria, las decodifica, Escribe en memoria
- **Unidad de Ejecución:** Lleva a cabo la ejecución de la instrucción
- **Registros:** Memoria interna utilizada como variable
- **Flags:** Indican eventos luego de ejecutar instrucciones

### Simulador

[VonSim — A 8088-like Assembly Simulator](https://vonsim.github.io/)

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.03.13.png]]

![[image 288.png]]

![[image 289.png]]

el cuadrado de la izquierda es el procesador mientras que lo de la derecha seria la memoria

el IP toma el valor 0100, el cual se transforma en binario y se pasan en el bus, en este caso los 16 bits salen por 16 cables.

todos reciben el bus pero solo el que esta en la ubicacion 0100 responde

cada periferico tiene un rango (una RAM de 8GB tiene 8GB de direcciones)

para el disco se pasan un par de direcciones nada mas

> [!note]+ # Repaso cantidad de bytes
> | Unidad | Abreviatura | Equivalencia en Bytes | Potencia de 2 |
> | --- | --- | --- | --- |
> | Byte | B | 1 | 2⁰ |
> | Kilobyte | KB | 1,024 | 2¹⁰ |
> | Megabyte | MB | 1,048,576 | 2²⁰ |
> | Gigabyte | GB | 1,073,741,824 | 2³⁰ |
> | Terabyte | TB | 1,099,511,627,776 | 2⁴⁰ |
> | Petabyte | PB | 1,125,899,906,842,624 | 2⁵⁰ |
> | Exabyte | EB | 1,152,921,504,606,846,976 | 2⁶⁰ |
> | Zettabyte | ZB | 1,180,591,620,717,411,303,424 | 2⁷⁰ |
> | Yottabyte | YB | 1,208,925,819,614,629,174,706,176 | 2⁸⁰ |

# Mapa de memoria

todo lo que puede apuntar un procesador

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.49.09.png]]

- Supongamos un procesador que tiene 16 líneas de bus de direcciones y 8 líneas de bus de datos. ¿Que cantidad de información puede acceder?
- ¿Y un procesador que tiene 16 líneas de bus de direcciones y 16 líneas de bus de datos?
- ¿Y un procesador con 32 líneas de datos y 32 líneas de direcciones?

### Ejemplo 1

![[image 290.png]]

cantidad de bus de datos no tiene que ser la misma que la de direcciones

puede apuntar a mas de las que e puede traer

![[image 291.png]]

puedo apuntar a $2^{16}=64k$

8 bits (8 lineas de datos) ⇒ 1 byte

entonces se puede acceder a 64kb ($64K*1B$)

### Ejemplo 2

![[image 292.png]]

en el segundo caso

32 lineas de BA ⇒ $2^{32}= 2^2*2^{32}= 4G$

32 lineas de BD ⇒ $32/8= 4B$

informacion total = $4G*4B=16GB$

![[image 293.png]]

# Registros de Intel

![[image 294.png]]

el valor que tome el instruction pinter al encenderse va a ser la primera dirección a la que apunte

tiene que haber código en dicha dirección (la BIOS) en la ROM ya que la misma mantiene la información

No se apunta al disco rigido ya que es mucho mas lento

## IP (puntero a instruccion)

se guarda la dirección de memoria de donde se puede conseguir la siguiente instrucción

tiene una incrementacion automatica y no hace falta incrementarlo

# Memorias

## Clasificación

Se clasifican:

- Por el **modo** en que se accede a los datos
- Por las **operaciones** que aceptan
- Por la **duración** de los datos

### ROM

ROM (Read Only Memory)

- Mantienen su información sin energía (no volátil)
- La escritura es más lenta que la RAM

![[image 295.png]]

### RAM

RAM (Random Access Memory) — mas dinamica que la ROM

- Pierde su información sin energía (volátil)

![[image 296.png]]

![[image 297.png]]

la dinamic es mas lenta y necesita refresco mientras que la static es mas rapida y no necesita

en las PC tenemos la dinamica ya que es menos costosa

la SRAM se usa para la memoria **cache**

#### Tipos de RAM

- DRAM:
    - Necesita refresco de valores cada n milisegundos
    - Menos compleja. Más económica.
    - Más lenta
- SRAM:
    - No necesita refresco.
    - Más compleja, más costosa.
    - Más rápida
    - Se suele utilizar para memoria cache.

## Tiempo de Acceso

Es el tiempo que le toma a una memoria RAM para completar un acceso después de otro. Se compone de:

- **Latencia:** tiempo que tarda la memoria en devolverte el valor
- **Transferencia:** tiempo que tarda un bit en viajar del procesador a la memoria

Las <u>DRAM</u> suelen tener tiempos entre 50 y 150 ns.

Las <u>SRAM</u> menores a 10 ns.

![[image 298.png]]

### Operacion

Las memorias para operar utilizan:

- Acción a realizar (lectura o escritura)
- Dirección de la palabra a acceder.
- Dato (entrante o saliente según acción)

## Estructura

Si el procesador, como es el caso de Intel, quiere mantener compatibilidad hacia atrás, permite acceder a la memoria a nivel byte. Por lo tanto la decodificación cambia según el tipo de memoria.

![[image 299.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.39.18.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.39.57.png]]

# Memoria comercial

![[image 300.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.40.19.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.40.27.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Clase 3 ASM y C]] — clase anterior
- [[Integrados Compuertas y decodificadores]] — lógica digital
- [[Memoria Cache]] — la SRAM que acá se clasifica es la que implementa la caché
- [[Interrupciones]] — la otra forma de manejar la E/S de los periféricos
- [[Resumen Criollo (Memoria, Deco, Perifericos)]] — repaso de memoria y periféricos

**Otras materias**

- **Protos**  [[8. Protos - Enlace]] — la codificación unipolar de acá es la misma técnica que codifica el frame en el medio físico
- **Protos**  [[Hub]] — el hub opera sobre esta señal, sin interpretarla

<!-- notas-relacionadas:fin -->
