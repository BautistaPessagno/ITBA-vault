---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
# Mapear Ram

para mapear una ram, lo que hago es lo siguiente

## Hasta cuanto puedo usar?

si me dan un programa hago lo siguiente

lineas de bus de direcciones→ $2^\text{lineas bus de direcciones}$

lineas bus de datos→ $* \text{bus de datos}$

entonces → 

$$
2^{direcciones}*datos
$$

donde  los datos son el ancho y las direcciones son el alto

Entonces uno de 16k x 16 son igual a 2 de 16k x 8

**despues igualdades de lineas de datos:**

16 lineas de datos → $2^{4} = 2*2^{3}= 2\space Bytes$

# Diseño de Circuito

## Como hago para llamar a un lugar(ROM, RAM,etc)?

1. **De donde a donde ocupa espacio la RAM/ROM/etc??**
Eso lo consigo midiendo de donde arranca hasta donde va usando su espacio(ej 32k*16 == $2^5*2^{10}*16$)
    ### Cantidad de bytes

| Unidad | Abreviatura | Equivalencia en Bytes | Potencia de 2 |
| --- | --- | --- | --- |
| Byte | B | 1 | 2⁰ |
| Kilobyte | KB | 1,024 | 2¹⁰ |
| Megabyte | MB | 1,048,576 | 2²⁰ |
| Gigabyte | GB | 1,073,741,824 | 2³⁰ |
| Terabyte | TB | 1,099,511,627,776 | 2⁴⁰ |
| Petabyte | PB | 1,125,899,906,842,624 | 2⁵⁰ |
| Exabyte | EB | 1,152,921,504,606,846,976 | 2⁶⁰ |
| Zettabyte | ZB | 1,180,591,620,717,411,303,424 | 2⁷⁰ |
| Yottabyte | YB | 1,208,925,819,614,629,174,706,176 | 2⁸⁰ |

| Unidad | Abrev. | Equiv. en bytes | Potencia de 2 (bytes) |
| --- | --- | --- | --- |
| bit | b | 1 b = ⅛ B | 2⁻³ B |
| byte | B | 1 B = 1 B | 2⁰ B |
| kibibyte | KiB | 1 KiB = 1 024 B | 2¹⁰ B |
| mebibyte | MiB | 1 MiB = 1 048 576 B | 2²⁰ B |
| gibibyte | GiB | 1 GiB = 1 073 741 824 B | 2³⁰ B |
| tebibyte | TiB | 1 TiB = 1 099 511 627 776 B | 2⁴⁰ B |
| pebibyte | PiB | 1 PiB = 1 125 899 906 842 624 B | 2⁵⁰ B |
2. **Veo que bits comparten el inicio y el final**
por ejemplo va del 0000h al 7FFFh → comparten los primeros comparten el ultimo bit(000 0…0 y 0111 1…1 ) entonces comparten el $A_{15}$ mientras que son indiferentes el $A_{14}-A_{0}$
3. **junto las lineas**
en el ejemplo anterios de la linea 0 al 14 mando directo al rom/ram/etc y despues el 15 lo mando por una compuerta o un decodificador

## Decodificador

Entradas y Salidas

la cantidad de salidas es $2^{\text{cantidad de entradas}}$

y el output correspondiente es igual a la sumatoria de los inputs donde el valor de cada input es $2^{\text{numero del input}}$ (si estan prendidas)

## Periferico

Periferico usa su propio mapa de memoria y el como mandar las lineas de direcciones funciona igual que con el ROM/RAM

Usa el I/O donde 1 es si esta prendido y 0 no

## **Bus de control**

Además del Bus de Direcciones y el Bus de Datos, hay un tercer Bus, llamado Bus de Control. Este Bus lleva la información digital de lo que deben hacer los distintos periféricos. Por ejemplo:

- R/W: Esta línea del bus indica si se va a leer un dato o se va a escribir. Es decir, el dispositivo que esté activado (por ejemplo la Memoria) debe saber si debe tomar o escribir un dato en el bus de datos. Cuando esta línea está en 0, significa que el dispositivo activo debe escribir un dato en el bus de datos. Si está en 0, significa que debe guardar el dato que esté presente en el bus de datos.
- IO/MEM: Existe el mapa de memoria, que es al que accede el procesador cada vez que ejecuta una instrucción de acceso a memoria con la instrucción **mov**. Pero también está el mapa de E/S. A este mapa se accede con las instrucciones IN/OUT. Esta línea está en 0 si se quiere acceder al mapa de memoria y 1 si se accede al mapa de entrada y salida.

Ver Ejemplos en la Guia 5

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Clase 4 Intro transmisión Digital]] — memoria y periféricos
- [[Integrados Compuertas y decodificadores]] — decodificadores
- [[Resumen Arqui]] — resumen general de la materia

<!-- notas-relacionadas:fin -->
