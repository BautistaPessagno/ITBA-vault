---
Created: 2026-05-1419:54
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---

# Threads

![](Attachments/image%2084.png)

puede pasar de que se tienen muchos códigos con el mismo código, mismas direcciones, pero cada hilo tiene su propio stack

## Justificacion

![](Attachments/image%2085.png)

los threads son baratos, si ya tengo un proceso creado, es mas barato crear mas threads antes que crear mas procesos, y se puede reutilizar

puedo tener a multiples threads leyendo multiples a la vez

## Modelo de threads

![](Attachments/image%2086.png)

los threads son como procesos mas livianos(mas faciles de crear eliminar)

![](Attachments/image%2087.png)

todos los procesos tiene un thread

en la imagen b es un proceso con 3 threads

![](Attachments/image%2088.png)

todo lo que esta en pre-thread es la informacion del instante actual para saber que hacer

![](Attachments/image%2089.png)

los threads son como la memoria, nadie lo detiene al intentar modificarla lo que tiene disponible

![](Attachments/image%2090.png)

## POSIX:API

![](Attachments/image%2091.png)

### Ejemplo

![](Attachments/image%2092.png)

# Implementación en espacio de usado

![](Attachments/image%2093.png)

el kernel tiene solo una tabla de procesos
luego tiene una tabla de threads manejada por el proceso

todas las funciones para manejar el thread se manejan desde el proceso

![](Attachments/image%2094.png)

cuando se hace syscall bloqueante con threads table en el usuario, se bloquean todos los threads de ese proceso

cuando se hace con el threads table en el kernel, el resto de threads del proceso siguen

### Desventajas en usuario

![](Attachments/image%2095.png)

# **Implementación en espacio de Kernel**

![](Attachments/image%2096.png)

cuesta mas crear destruir threads, por lo que se suelen reutilizar

### ¿Qué pasa cuando un proceso con threads ejecuta fork?

### ¿Qué pasa con las señales? Por defecto son por proceso. 

# Implementación hibrida

![](Attachments/image%2097.png)

# Scheduler activations

![](Attachments/image%2098.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Procesos](Procesos.md) — hilo vs proceso
- [Scheduling](Scheduling.md) — planificación de hilos
- [IPC](IPC.md) — sincronización y comunicación

**Otras materias**

- **BD**  [BD clase 16 programacion embebida](BD%20clase%2016%20programacion%20embebida.md) — concurrencia y aislamiento transaccional
- **Protos**  [10. Protos - Sockets](10.%20Protos%20-%20Sockets.md) — servidores concurrentes

<!-- notas-relacionadas:fin -->
