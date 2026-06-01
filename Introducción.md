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

![[image 169.png]]

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

![[Captura_de_pantalla_2025-08-07_a_la(s)_14.48.26.png]]

## Procesador

![[image 170.png]]

![[image 171.png]]

![[image 172.png]]

- Multithreding: Mantener el estado de 2 threads e intercambia entre ellos rapidamente cuando sea necesario
- Multicore: replicar los nucleos independientes. pueden soportar multiples threads

![[image 173.png]]

## Memoria

jerarquia de memorias

![[image 174.png]]

## Dispositivos I/O

3 formas de IO:

- **Busy interrupting**: esperar hasta tener el dato disponible, el cpu se encarga de ver si hay algo disponible
- **Interruption:** Una vez que tiene los datos interrumpe
- **DMA:** El una vez que tiene el dato lo guarda en un espacio en memoria

![[image 175.png]]

## Booteo

![[image 176.png]]

## Sistemas operativos

![[Captura_de_pantalla_2025-08-07_a_la(s)_15.24.02.png]]

![[image 177.png]]

# System Calls

![[Captura_de_pantalla_2025-08-07_a_la(s)_15.33.02.png]]

es una llamada a funcion. en punto de vista del programador es lo mismo llamar a la funcion y al syscall

se usa memoria dinamica solo cuando no se sabe el tamaño de la memoria que se va a necesitar

![[image 178.png]]

### Posix

![[image 179.png]]

![[image 180.png]]

### Usos desde la shell - pseudocódigo

![[image 181.png]]

continuación en [[SysCall]]…