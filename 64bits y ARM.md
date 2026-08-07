---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
al principio no existia el de 64 bits sino que eran alrgues del de 32

![[image 184.png]]

![[image 185.png]]

una arquitectura es la misma cuando compartes las instrucciones

## MicroArquitectura

micro arquitectura= implementación de la arquitectura

define donde va a terminar el procesador (mobil, computadora, heladera, etc)

![[image 186.png]]

![[image 187.png]]

## Pentium

![[image 188.png]]

al pentium le metieron paginación

![[image 189.png]]

![[image 190.png]]

![[image 191.png]]

![[image 192.png]]

# ARM

![[image 193.png]]

![[image 194.png]]

arm no crea procesadores, los diseña

poco consumo y muchas instrucciones por segundo

![[image 195.png]]

![[image 196.png]]

## Crecimiento

![[image 197.png]]

SOC (sistem on a chip

## SOC (Sistem on a Chip)

![[image 198.png]]

esta todo integrado y es directamente una computadora en un chip, pero no se puede hacer un upgrade del sistema (ya esta todo integrado)

![[image 199.png]]

![[image 200.png]]

## Modos del procesador

![[image 201.png]]

esta el user space y el kernel space, pero hay mas detalles en cuanto al modo en el que trabaja el procesador

## Registros 

![[image 202.png]]

arm tiene 27 registros

![[image 203.png]]

## Flags

![[image 204.png]]

## Caracteristicas generales

RISC

![[image 205.png]]

### RISC y CISC

RISC= Reduced Instruction Set Computer

CISC = Complex Instruction Set Computer

los procesadores intel estaban basados en CISC

los procesadores son de tipo RISC donde casi todas las instrucciones tardan lo mismo en ejecutarse

ayuda en la sincronización y en que no se armen cuellos de botella (una instruccion ocupa demasiado)

tamaño fijo de instrucciones

## Mapa de memoria ARM

![[image 206.png]]

las primneras estan para la ROM luego la RAM y luego los perifericos ya estan mapeados arriba, no hay mapa de ES

# Pipeline en ARM7

no es un concepto solo de ARM, esta en todos los procesadores (Intel, amd, TODOS)

![[image 207.png]]

tres etapas:

- Buscar las instrucciones en memoria
- decodificar la instrucción
- ejecutar la instrucción

el fetch es el que mas tarda, el execute depende de si tiene que ir a buscar a memoria o no

lo que hizo

intel creo una predicción de salto (branch predictor)

si hay jump condicional pasa la instrucción a una sandbox para correr el código y ver lo que hace. de esa forma sabe si tiene que hacer el fetch y el decode de las instrucciones que siguen

## Instrucciones ARM

ARM creo instrucciones condicionales

![[image 208.png]]

![[image 209.png]]

### Condiciones

![[image 210.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Procesadores de 64 bits]] — arquitecturas de 64 bits
- [[ARM y GPU]] — ARM y cómputo paralelo
- [[Assembler de Intel]] — comparación con x86

<!-- notas-relacionadas:fin -->
