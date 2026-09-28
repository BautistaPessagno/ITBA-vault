---
Created: 2026-05-1419:49
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# File System

## Motivaciones

![](Attachments/image.png)

32 bits y queremos almacenar mas de 4G, no podemos no en EDV

## Abstracciones

un proceso es una abstracción del CPU. guardamos el snapshot

lo mismo pasa con la memoria virtual. 

![](Attachments/image%201.png)

# Implementación

![](Attachments/image%202.png)

# File System

## Asignación Continua

![](Attachments/image%203.png)

es una asignación vieja pero la mas simple. se usa en discos de pasta, bluerays, etc

la fragmentación externa puede pasar al borrar un archivo. en la imagen nos quedamos sin espacio para almacenar un archivo de 11 bloques aunque los tengamos (5 y 6).

## Asignación con listas enlazadas (en disco)

![](Attachments/image%204.png)

se resuelve la fragmentacion externa ya que se pueden enlazar los bloques 

no hay acceso random

## Asignación con listas enlazadas (en memoria)

![](Attachments/image%205.png)

## I-nodes (index-node)

![](Attachments/image%206.png)

![](Attachments/image%207.png)

# Casos de usos

## Basic Disc Layout

![](Attachments/image%208.png)

## SuperBlock

![](Attachments/image%209.png)

toda la metadata del file system esta en el superblock, si se corrompe se pierde el superblock

### Contenidos SuperBlock

![](Attachments/image%2010.png)

el numero magico es el numero con el cual se identifica

gracias a esto se puede reconstruir luego de una particion mal hecha

## Inodos

![](Attachments/image%2011.png)

mode, user id, groupd id

link count = referencias al nodo. el mismo archivo puede tener muchos nombres

access time → se suele de deshabilitar

creation time

modification time

### Data Blocks

![](Attachments/image%2012.png)

## Inodo (3)

![](Attachments/image%2013.png)

![](Attachments/image%2014.png)

![](Attachments/image%2015.png)

# Manejo de consistencia

![](Attachments/image%2016.png)

![](Attachments/image%2017.png)

# Problemas 

![](Attachments/image%2018.png)

## primera mejora: BSD FFS

![](Attachments/image%2019.png)

## ext4

![](Attachments/image%2020.png)

los extents son grupos logicos de bloques continuos( mas grandes y mas faciles de manejar )

# Problemas de FS tradicionales

![](Attachments/image%2021.png)

# ZFS

![](Attachments/image%2022.png)

![](Attachments/image%2023.png)

![](Attachments/image%2024.png)

![](Attachments/image%2025.png)

se puede curar a si mismo

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [PIPELINES](PIPELINES.md) — todo es un archivo
- [SysCall](SysCall.md) — open, read, write

**Otras materias**

- **Arqui**  [Clase 4 Intro transmisión Digital](Clase%204%20Intro%20transmisión%20Digital.md) — el sistema de entrada y salida y el bus por donde viaja lo que el FS lee del disco
- **EDA**  [EDA - Árboles](EDA%20-%20Árboles.md) — el FS es un árbol de directorios
- **Protos**  [sendfile()](sendfile%28%29.md) — transferencia entre descriptores

<!-- notas-relacionadas:fin -->
