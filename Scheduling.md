---
Created: 2026-05-1419:54
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Scheduling


el scheduler es el que decide cual es el proximo prcesos( o thread ) a correr, debe elegir uno (no lo elige de maera random)

![[image 56.png]]

no es un proceso, es un componente del kernel

![[image 57.png]]

multiprogramar: preparar a uno mientras corre otro

la mayor parte del tiempo nos sobra cpu

## Comportamiento de un proceso

tienen un “brust” hasta que se bloquean

I/O se refiere a bloquearse esprando algo

![[image 58.png]]

hay procesos que rara vez se bloquean y otros que se bloquean con mucha frecuencia

![[image 59.png]]

a es cpu bound mientras que b es I/O bound. hay que fijarse en la rafaga de cpu porque es donde esta haciendo algo, ya que la rafaga I/O no depende del procesos

ej: el proceso shell es I/O bound (siempre esperando teclas)

## Cuando

![[image 60.png]]

el timer puede ser mas alto/bajo

## Clasificacion

preempt = proactivo

scheduler preemptive: te quita el control del procesador. corre aunque el proceso este en ready

![[image 61.png]]

parapoder escalar un no preemptive los procesos tienen que ser colaborativos

# Categorias

![[image 62.png]]

### interactivo

es preemptive porque se necesita que se le de

### Batch

se necesita un quantum mas largo ya que el content switch es costoso

## Objetivos

![[image 63.png]]

- no se priorizan procesos sobre otros
- lo que se asigna se respeta
- tiempo de respuesta: responde rapido
- alcanza las expectativas del usuario

# Batch

## first-come first-served

![[image 64.png]]

al ser non-preemptive puede perder un buen balance

## Shortest Job First

![[image 65.png]]

El turnaround es el tiempo que el proceso pasa en el sistema desde que entro hasta que se fue (no solo el que corrio)

## Shortest Remaining Time Next

atiende al proceso que menos le falta

![[image 66.png]]

solo compara el que llega y el que esta, poque supuestamente el que esta es el mas corto

beneficia la metrica de turnaround

# Interactivo

## Round-Robin

version preemptive del FIFO

![[image 67.png]]

![[image 68.png]]

## Priority scheduling

![[image 69.png]]

### Multiple queues

![[image 70.png]]

### Shortest Process Next

![[image 71.png]]

envejecimiento: busca darle mas peso a las ejecuciones mas antiguas/recientes

### Guaranteed scheduling

cada vez que pusiste a correr un dato se guarda la ejecución y 

![[image 72.png]]

## Lottery scheduling

![[image 73.png]]

convierte el siguiente evento a correr en una loteria

## Fair-Share Scheduling

![[image 74.png]]
