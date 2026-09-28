---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
![](Attachments/image%20337.png)

## Rutina de atención de interrupción

![](Attachments/image%20338.png)

IRET = interrupt return. funciona igual que el ret, vuelve a la siguiente instrucción

# Interrupciones

estas las de hardware y las de software

![](Attachments/image%20339.png)

la INTR se puede ignorar mientras que la NMI no (ej: bateria)

## Interrupciones de hardware

se habilitan y deshabilitan con sti y cli

![](Attachments/image%20340.png)

### PIC (controlador programable de interrupciones)

hay ocho patitas donde se permite poner 8 perifericos, el pic recibe la interrupción y se la manda al procesador (el procesador solo tiene un periferico)

IRQ= interrupt request

IDT = interrupt descriptor table. tabla con todas las interrupciones 

## Interrupciones de software

![](Attachments/image%20341.png)

el numero es la posicion en la tabla de interrupciones (en el syscall dispatcher)

### servicio de las Bios

![](Attachments/image%20342.png)

![](Attachments/image%20343.png)

# Excepciones

![](Attachments/image%20344.png)

igual que las interrupciones pero generadas por el procesador. el procesador se interrumpe a si mismo

![](Attachments/image%20345.png)

las detecta el procesador porque cuando se esta corriendo una instruccion el sistema operativo no lo puede ver eso

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Interrupciones](Interrupciones.md) — tema principal
- [Clase 4 Intro transmisión Digital](Clase%204%20Intro%20transmisión%20Digital.md) — dispositivos que interrumpen
- [Resumen Arqui](Resumen%20Arqui.md) — resumen integrador

<!-- notas-relacionadas:fin -->
