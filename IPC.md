---
Created: 2026-05-1419:50
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# IPC

![[image 99.png]]

# Memoria compartida y pasaje de mensajes

![[image 100.png]]

## Memoria Compartida

![[image 101.png]]

para crearla hay que armar un syscall

tenes un puntero y para el sistema es indistinguible si es un malloc o memoria compartida

no se garantiza sincronizacion

## Pasaje de Mensajes

![[image 102.png]]

me puedo “olvidar” de la información que paso

### Buffering

![[image 103.png]]

# Pipes

![[image 104.png]]

![[image 105.png]]

simplifica y util para los esquemas productor consumidor

## Anonimo / ordinarios

![[image 106.png]]

no tiene un nombre asociado

![[image 107.png]]

# Files vs Pipes

# Shared Memory

![[image 108.png]]

una vez que creo un array retornan un puntero. una vez que tengo el puntero puedo escribir en memoria sin necesidad del kernel

# Race Condition

un dato puede ser accedido por multiples personas

ejemplo: homebanking, puede ser accedido desde multiples instancias

![[image 109.png]]

![[image 110.png]]

![[image 111.png]]

## exclusion mutua

![[image 112.png]]

## Region Critica

![[image 113.png]]

![[image 114.png]]

# Mecanismo de Sincronización

## Semaforo

![[image 115.png]]

up = post

down = wait

![[image 116.png]]

![[image 117.png]]

![[image 118.png]]

no olvidarse de inicializar los semaforos

mutex se inicializa en 1 para que se pueda entrar

empty y full se encargan de asegurarse que existan espacios libres

# DeadLock

los dos estan esperando a otro

![[Captura_de_pantalla_2025-09-03_a_la(s)_10.12.20.png]]

en la imagen todas las filas estan esperando a la otra fila

![[image 119.png]]

foo se esta esperando asi mismo

![[image 120.png]]

<u>OBS:</u> siempre que hay casos donde se hace up y down de un mismo semaforo al mismo tiempo en distintos lugares es una invitación al deadlock 

![[image 121.png]]

en este caso siguiendo el orden, se hace un wait en men, luego se hace el wait en women. luego ambos codigos van a entrar al iff y se van a quedar eternamente en el if

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [[Threads]] — sincronización entre hilos
- [[PIPELINES]] — pipes como IPC
- [[Procesos]] — comunicación entre procesos

**Otras materias**

- **Protos**  [[10. Protos - Sockets]] — sockets como IPC en red

<!-- notas-relacionadas:fin -->
