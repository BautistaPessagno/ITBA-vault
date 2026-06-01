---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
![[image 337.png]]

## Rutina de atención de interrupción

![[image 338.png]]

IRET = interrupt return. funciona igual que el ret, vuelve a la siguiente instrucción

# Interrupciones

estas las de hardware y las de software

![[image 339.png]]

la INTR se puede ignorar mientras que la NMI no (ej: bateria)

## Interrupciones de hardware

se habilitan y deshabilitan con sti y cli

![[image 340.png]]

### PIC (controlador programable de interrupciones)

hay ocho patitas donde se permite poner 8 perifericos, el pic recibe la interrupción y se la manda al procesador (el procesador solo tiene un periferico)

IRQ= interrupt request

IDT = interrupt descriptor table. tabla con todas las interrupciones 

## Interrupciones de software

![[image 341.png]]

el numero es la posicion en la tabla de interrupciones (en el syscall dispatcher)

### servicio de las Bios

![[image 342.png]]

![[image 343.png]]

# Excepciones

![[image 344.png]]

igual que las interrupciones pero generadas por el procesador. el procesador se interrumpe a si mismo

![[image 345.png]]

las detecta el procesador porque cuando se esta corriendo una instruccion el sistema operativo no lo puede ver eso