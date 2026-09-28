---
Created: 2026-05-1419:50
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# IPC

![](Attachments/image%2099.png)

# Memoria compartida y pasaje de mensajes

![](Attachments/image%20100.png)

## Memoria Compartida

![](Attachments/image%20101.png)

para crearla hay que armar un syscall

tenes un puntero y para el sistema es indistinguible si es un malloc o memoria compartida

no se garantiza sincronizacion

## Pasaje de Mensajes

![](Attachments/image%20102.png)

me puedo “olvidar” de la información que paso

### Buffering

![](Attachments/image%20103.png)

# Pipes

![](Attachments/image%20104.png)

![](Attachments/image%20105.png)

simplifica y util para los esquemas productor consumidor

## Anonimo / ordinarios

![](Attachments/image%20106.png)

no tiene un nombre asociado

![](Attachments/image%20107.png)

# Files vs Pipes

# Shared Memory

![](Attachments/image%20108.png)

una vez que creo un array retornan un puntero. una vez que tengo el puntero puedo escribir en memoria sin necesidad del kernel

# Race Condition

un dato puede ser accedido por multiples personas

ejemplo: homebanking, puede ser accedido desde multiples instancias

![](Attachments/image%20109.png)

![](Attachments/image%20110.png)

![](Attachments/image%20111.png)

## exclusion mutua

![](Attachments/image%20112.png)

## Region Critica

![](Attachments/image%20113.png)

![](Attachments/image%20114.png)

# Mecanismo de Sincronización

## Semaforo

![](Attachments/image%20115.png)

up = post

down = wait

![](Attachments/image%20116.png)

![](Attachments/image%20117.png)

![](Attachments/image%20118.png)

no olvidarse de inicializar los semaforos

mutex se inicializa en 1 para que se pueda entrar

empty y full se encargan de asegurarse que existan espacios libres

# DeadLock

los dos estan esperando a otro

![](Attachments/Captura_de_pantalla_2025-09-03_a_la%28s%29_10.12.20.png)

en la imagen todas las filas estan esperando a la otra fila

![](Attachments/image%20119.png)

foo se esta esperando asi mismo

![](Attachments/image%20120.png)

<u>OBS:</u> siempre que hay casos donde se hace up y down de un mismo semaforo al mismo tiempo en distintos lugares es una invitación al deadlock 

![](Attachments/image%20121.png)

en este caso siguiendo el orden, se hace un wait en men, luego se hace el wait en women. luego ambos codigos van a entrar al iff y se van a quedar eternamente en el if

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Threads](Threads.md) — sincronización entre hilos
- [PIPELINES](PIPELINES.md) — pipes como IPC
- [Procesos](Procesos.md) — comunicación entre procesos

**Otras materias**

- **Protos**  [10. Protos - Sockets](10.%20Protos%20-%20Sockets.md) — sockets como IPC en red

<!-- notas-relacionadas:fin -->
