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

![](Attachments/image%2056.png)

no es un proceso, es un componente del kernel

![](Attachments/image%2057.png)

multiprogramar: preparar a uno mientras corre otro

la mayor parte del tiempo nos sobra cpu

## Comportamiento de un proceso

tienen un “brust” hasta que se bloquean

I/O se refiere a bloquearse esprando algo

![](Attachments/image%2058.png)

hay procesos que rara vez se bloquean y otros que se bloquean con mucha frecuencia

![](Attachments/image%2059.png)

a es cpu bound mientras que b es I/O bound. hay que fijarse en la rafaga de cpu porque es donde esta haciendo algo, ya que la rafaga I/O no depende del procesos

ej: el proceso shell es I/O bound (siempre esperando teclas)

## Cuando

![](Attachments/image%2060.png)

el timer puede ser mas alto/bajo

## Clasificacion

preempt = proactivo

scheduler preemptive: te quita el control del procesador. corre aunque el proceso este en ready

![](Attachments/image%2061.png)

parapoder escalar un no preemptive los procesos tienen que ser colaborativos

# Categorias

![](Attachments/image%2062.png)

### interactivo

es preemptive porque se necesita que se le de

### Batch

se necesita un quantum mas largo ya que el content switch es costoso

## Objetivos

![](Attachments/image%2063.png)

- no se priorizan procesos sobre otros
- lo que se asigna se respeta
- tiempo de respuesta: responde rapido
- alcanza las expectativas del usuario

# Batch

## first-come first-served

![](Attachments/image%2064.png)

al ser non-preemptive puede perder un buen balance

## Shortest Job First

![](Attachments/image%2065.png)

El turnaround es el tiempo que el proceso pasa en el sistema desde que entro hasta que se fue (no solo el que corrio)

## Shortest Remaining Time Next

atiende al proceso que menos le falta

![](Attachments/image%2066.png)

solo compara el que llega y el que esta, poque supuestamente el que esta es el mas corto

beneficia la metrica de turnaround

# Interactivo

## Round-Robin

version preemptive del FIFO

![](Attachments/image%2067.png)

![](Attachments/image%2068.png)

## Priority scheduling

![](Attachments/image%2069.png)

### Multiple queues

![](Attachments/image%2070.png)

### Shortest Process Next

![](Attachments/image%2071.png)

envejecimiento: busca darle mas peso a las ejecuciones mas antiguas/recientes

### Guaranteed scheduling

cada vez que pusiste a correr un dato se guarda la ejecución y 

![](Attachments/image%2072.png)

## Lottery scheduling

![](Attachments/image%2073.png)

convierte el siguiente evento a correr en una loteria

## Fair-Share Scheduling

![](Attachments/image%2074.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Procesos](Procesos.md) — qué se planifica
- [Threads](Threads.md) — planificación de hilos

**Otras materias**

- **Arqui**  [Interrupciones](Interrupciones.md) — el timer que dispara la replanificación
- **EDA**  [EDA - Queue (Cola)](EDA%20-%20Queue%20%28Cola%29.md) — las colas de listos son FIFO/prioridad

<!-- notas-relacionadas:fin -->
