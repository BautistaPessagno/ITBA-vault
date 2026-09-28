---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
al principio no existia el de 64 bits sino que eran alrgues del de 32

![](Attachments/image%20184.png)

![](Attachments/image%20185.png)

una arquitectura es la misma cuando compartes las instrucciones

## MicroArquitectura

micro arquitectura= implementación de la arquitectura

define donde va a terminar el procesador (mobil, computadora, heladera, etc)

![](Attachments/image%20186.png)

![](Attachments/image%20187.png)

## Pentium

![](Attachments/image%20188.png)

al pentium le metieron paginación

![](Attachments/image%20189.png)

![](Attachments/image%20190.png)

![](Attachments/image%20191.png)

![](Attachments/image%20192.png)

# ARM

![](Attachments/image%20193.png)

![](Attachments/image%20194.png)

arm no crea procesadores, los diseña

poco consumo y muchas instrucciones por segundo

![](Attachments/image%20195.png)

![](Attachments/image%20196.png)

## Crecimiento

![](Attachments/image%20197.png)

SOC (sistem on a chip

## SOC (Sistem on a Chip)

![](Attachments/image%20198.png)

esta todo integrado y es directamente una computadora en un chip, pero no se puede hacer un upgrade del sistema (ya esta todo integrado)

![](Attachments/image%20199.png)

![](Attachments/image%20200.png)

## Modos del procesador

![](Attachments/image%20201.png)

esta el user space y el kernel space, pero hay mas detalles en cuanto al modo en el que trabaja el procesador

## Registros 

![](Attachments/image%20202.png)

arm tiene 27 registros

![](Attachments/image%20203.png)

## Flags

![](Attachments/image%20204.png)

## Caracteristicas generales

RISC

![](Attachments/image%20205.png)

### RISC y CISC

RISC= Reduced Instruction Set Computer

CISC = Complex Instruction Set Computer

los procesadores intel estaban basados en CISC

los procesadores son de tipo RISC donde casi todas las instrucciones tardan lo mismo en ejecutarse

ayuda en la sincronización y en que no se armen cuellos de botella (una instruccion ocupa demasiado)

tamaño fijo de instrucciones

## Mapa de memoria ARM

![](Attachments/image%20206.png)

las primneras estan para la ROM luego la RAM y luego los perifericos ya estan mapeados arriba, no hay mapa de ES

# Pipeline en ARM7

no es un concepto solo de ARM, esta en todos los procesadores (Intel, amd, TODOS)

![](Attachments/image%20207.png)

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

![](Attachments/image%20208.png)

![](Attachments/image%20209.png)

### Condiciones

![](Attachments/image%20210.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Procesadores de 64 bits](Procesadores%20de%2064%20bits.md) — arquitecturas de 64 bits
- [ARM y GPU](ARM%20y%20GPU.md) — ARM y cómputo paralelo
- [Assembler de Intel](Assembler%20de%20Intel.md) — comparación con x86

<!-- notas-relacionadas:fin -->
