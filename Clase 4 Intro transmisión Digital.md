---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
![[image 282.png]]

todos los periféricos (ram rom, placa de wifi, de video, teclado, mouse, etc) hablan con procesador con 1 y 0s

## Transmicion en serie y paralela

![[image 283.png]]

# Tipo de arquitectura

## Von Newman

memoria de código y datos se usa el mismo bus

se arma. un cuello de botella, se satura facilmente

es que me se usa porque es mas barato y no se necita dividir en dos memorias

![[image 284.png]]

## Harvard

se usan dos buses de datos, menos saturación

es mas caro y requiere dos tipo de memoria

![[image 285.png]]

# Sistema de Entrada y Salida

![[image 286.png]]

Un unico Bus por que todos se cuelgan

## CPU

![[image 287.png]]

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

# Memorias

## Clasificación

### ROM

![[image 295.png]]

### RAM

mas dinamica que la ROM

![[image 296.png]]

![[image 297.png]]

la dinamic es mas lenta y necesita refresco mientras que la static es mas rapida y no necesita

en las PC tenemos la dinamica ya que es menos costosa

la SRAM se usa para la memoria **cache**

## Tiempo de Acceso

Transferencia: tiempo que tarda un bit en viajar del procesador a la memora

Latencia: tiempo que tarda la memoria en devolverte el valor

![[image 298.png]]

## Estructura

![[image 299.png]]

# Memoria comercial

![[image 300.png]]