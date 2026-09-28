---
Created: 2026-05-1419:50
Tags:
  - Teorica
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Introducción

# ¿Que es un sistema Operativo?

nos resuelve al nivel de llamar a funciones para cumplir tareas y permite abstraernos

### Modo Kernel vs Modo Usuario

modo de funcionamiento del procesado

hay un/dos bits que marca el modo en el cual opera

en modo kernel puede hacer cualquier funcion (como las funciones privilejiadas)

- El SO provee un conjunto abstracto y limpio de los recursos (Hardware)
- Administra los recursos

La shell es un proceso como cualquier otro, no es un proceso especial del kernel ni nada mas

## Maquina Extendida

![](Attachments/image%20169.png)

Oculta el hardware y ofrecer una interfaz limpia, elegante y consistente al programador

Las abstracciones son una de las claves para comprender los SOs

## Administrador de recursos

si hay dos o mas procesos que intentan una misma tarea. el SO los encola para que no se pisen

administrar un recurso incluye **multiplexar** estos recursos:

- tiempo
- espacio

**multiplexar en espacio:** partir la memoria en espacios y cada proceso usa un espacio

**multiplexar en tiempo:** se le da un espacio/intervalo de tiempo a cada espacio

# Revision Hardware

![](Attachments/Captura_de_pantalla_2025-08-07_a_la%28s%29_14.48.26.png)

## Procesador

![](Attachments/image%20170.png)

![](Attachments/image%20171.png)

![](Attachments/image%20172.png)

- Multithreding: Mantener el estado de 2 threads e intercambia entre ellos rapidamente cuando sea necesario
- Multicore: replicar los nucleos independientes. pueden soportar multiples threads

![](Attachments/image%20173.png)

## Memoria

jerarquia de memorias

![](Attachments/image%20174.png)

## Dispositivos I/O

3 formas de IO:

- **Busy interrupting**: esperar hasta tener el dato disponible, el cpu se encarga de ver si hay algo disponible
- **Interruption:** Una vez que tiene los datos interrumpe
- **DMA:** El una vez que tiene el dato lo guarda en un espacio en memoria

![](Attachments/image%20175.png)

## Booteo

![](Attachments/image%20176.png)

## Sistemas operativos

![](Attachments/Captura_de_pantalla_2025-08-07_a_la%28s%29_15.24.02.png)

![](Attachments/image%20177.png)

# System Calls

![](Attachments/Captura_de_pantalla_2025-08-07_a_la%28s%29_15.33.02.png)

es una llamada a funcion. en punto de vista del programador es lo mismo llamar a la funcion y al syscall

se usa memoria dinamica solo cuando no se sabe el tamaño de la memoria que se va a necesitar

![](Attachments/image%20178.png)

### Posix

![](Attachments/image%20179.png)

![](Attachments/image%20180.png)

### Usos desde la shell - pseudocódigo

![](Attachments/image%20181.png)

continuación en [SysCall](SysCall.md)…

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Estructura de un Sistema Operativo](Estructura%20de%20un%20Sistema%20Operativo.md) — tema siguiente
- [Procesos](Procesos.md) — la abstracción central del SO
- [Resumen SO](Resumen%20SO.md) — resumen integrador

<!-- notas-relacionadas:fin -->
