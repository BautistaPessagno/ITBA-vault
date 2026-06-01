---
Created: 2026-05-1419:54
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---

# Threads

![[image 84.png]]

puede pasar de que se tienen muchos códigos con el mismo código, mismas direcciones, pero cada hilo tiene su propio stack

## Justificacion

![[image 85.png]]

los threads son baratos, si ya tengo un proceso creado, es mas barato crear mas threads antes que crear mas procesos, y se puede reutilizar

puedo tener a multiples threads leyendo multiples a la vez

## Modelo de threads

![[image 86.png]]

los threads son como procesos mas livianos(mas faciles de crear eliminar)

![[image 87.png]]

todos los procesos tiene un thread

en la imagen b es un proceso con 3 threads

![[image 88.png]]

todo lo que esta en pre-thread es la informacion del instante actual para saber que hacer

![[image 89.png]]

los threads son como la memoria, nadie lo detiene al intentar modificarla lo que tiene disponible

![[image 90.png]]

## POSIX:API

![[image 91.png]]

### Ejemplo

![[image 92.png]]

# Implementación en espacio de usado

![[image 93.png]]

el kernel tiene solo una tabla de procesos
luego tiene una tabla de threads manejada por el proceso

todas las funciones para manejar el thread se manejan desde el proceso

![[image 94.png]]

cuando se hace syscall bloqueante con threads table en el usuario, se bloquean todos los threads de ese proceso

cuando se hace con el threads table en el kernel, el resto de threads del proceso siguen

### Desventajas en usuario

![[image 95.png]]

# **Implementación en espacio de Kernel**

![[image 96.png]]

cuesta mas crear destruir threads, por lo que se suelen reutilizar

### ¿Qué pasa cuando un proceso con threads ejecuta fork?

### ¿Qué pasa con las señales? Por defecto son por proceso. 

# Implementación hibrida

![[image 97.png]]

# Scheduler activations

![[image 98.png]]