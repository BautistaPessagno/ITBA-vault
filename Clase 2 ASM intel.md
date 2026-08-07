---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
# Assembler 

## sintaxis

por cada código maquina de intel existe una instrucción en assembler

![[image 246.png]]

![[image 247.png]]

## Instrucciones

![[image 248.png]]

## Registros

![[image 249.png]]

el primero mueve el valor decimal 23 al ah

el segudo mueve el valor hexadecimal 99h a bl

### Lectura de memoria

![[image 250.png]]

al poner [100h] va al contenido en la posición de memoria 100h (en el ejemplo D0 12, ya que pide 2 bytes)

### escritura de memoria

![[image 251.png]]

escribe en en la ubicación de memoria 102h el valor de bl

## Modos de direccionamiento

![[image 252.png]]

![[image 253.png]]

en el direccionamiento indirecto se va a la ubicación del valor en bp, como el 4 esta en decimal se maneja en decimal

## Recordatorio (IMPORTANTE)

![[image 254.png]]

## Registros de Flags

![[image 255.png]]

hay instrucciones que cambian los flags y hay unas que no

se pueden comparar usar, ignorar, etc

### Ejemplo ASM

![[image 256.png]]

## Compilacion

```bash
#compilar
nasm -f elf64 <ej.asm> -o <ej.o>
#linkeditar
ld <ej.o> -o <ej>

```

![[image 257.png]]

![[image 258.png]]

la parte de la derecha es el codigo en assembler, lo del medio es el codigo maquina, mientras que el de la izquierda es el code segment (CS) y el IP

el stack lo reserva el sistema operativo

![[image 259.png]]

la sección bss se usa para guardar datos como el heap

### INT 80h

llama a la interrupción, es un vector con todas las llamadas a interrupciones

el que lo define es el pasaje al argumento en eax

![[image 260.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Clase 1 Intro]] — clase anterior
- [[Clase 3 ASM y C]] — clase siguiente
- [[Assembler de Intel]] — referencia completa de instrucciones
- [[Codigos Assembler]] — ejemplos de código

<!-- notas-relacionadas:fin -->
