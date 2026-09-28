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

![](Attachments/image%20246.png)

![](Attachments/image%20247.png)

## Instrucciones

![](Attachments/image%20248.png)

## Registros

![](Attachments/image%20249.png)

el primero mueve el valor decimal 23 al ah

el segudo mueve el valor hexadecimal 99h a bl

### Lectura de memoria

![](Attachments/image%20250.png)

al poner [100h] va al contenido en la posición de memoria 100h (en el ejemplo D0 12, ya que pide 2 bytes)

### escritura de memoria

![](Attachments/image%20251.png)

escribe en en la ubicación de memoria 102h el valor de bl

## Modos de direccionamiento

![](Attachments/image%20252.png)

![](Attachments/image%20253.png)

en el direccionamiento indirecto se va a la ubicación del valor en bp, como el 4 esta en decimal se maneja en decimal

## Recordatorio (IMPORTANTE)

![](Attachments/image%20254.png)

## Registros de Flags

![](Attachments/image%20255.png)

hay instrucciones que cambian los flags y hay unas que no

se pueden comparar usar, ignorar, etc

### Ejemplo ASM

![](Attachments/image%20256.png)

## Compilacion

```bash
#compilar
nasm -f elf64 <ej.asm> -o <ej.o>
#linkeditar
ld <ej.o> -o <ej>

```

![](Attachments/image%20257.png)

![](Attachments/image%20258.png)

la parte de la derecha es el codigo en assembler, lo del medio es el codigo maquina, mientras que el de la izquierda es el code segment (CS) y el IP

el stack lo reserva el sistema operativo

![](Attachments/image%20259.png)

la sección bss se usa para guardar datos como el heap

### INT 80h

llama a la interrupción, es un vector con todas las llamadas a interrupciones

el que lo define es el pasaje al argumento en eax

![](Attachments/image%20260.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Clase 1 Intro](Clase%201%20Intro.md) — clase anterior
- [Clase 3 ASM y C](Clase%203%20ASM%20y%20C.md) — clase siguiente
- [Assembler de Intel](Assembler%20de%20Intel.md) — referencia completa de instrucciones
- [Codigos Assembler](Codigos%20Assembler.md) — ejemplos de código

<!-- notas-relacionadas:fin -->
