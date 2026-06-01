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

![[image.png]]

32 bits y queremos almacenar mas de 4G, no podemos no en EDV

## Abstracciones

un proceso es una abstracción del CPU. guardamos el snapshot

lo mismo pasa con la memoria virtual. 

![[image 1.png]]

# Implementación

![[image 2.png]]

# File System

## Asignación Continua

![[image 3.png]]

es una asignación vieja pero la mas simple. se usa en discos de pasta, bluerays, etc

la fragmentación externa puede pasar al borrar un archivo. en la imagen nos quedamos sin espacio para almacenar un archivo de 11 bloques aunque los tengamos (5 y 6).

## Asignación con listas enlazadas (en disco)

![[image 4.png]]

se resuelve la fragmentacion externa ya que se pueden enlazar los bloques 

no hay acceso random

## Asignación con listas enlazadas (en memoria)

![[image 5.png]]

## I-nodes (index-node)

![[image 6.png]]

![[image 7.png]]

# Casos de usos

## Basic Disc Layout

![[image 8.png]]

## SuperBlock

![[image 9.png]]

toda la metadata del file system esta en el superblock, si se corrompe se pierde el superblock

### Contenidos SuperBlock

![[image 10.png]]

el numero magico es el numero con el cual se identifica

gracias a esto se puede reconstruir luego de una particion mal hecha

## Inodos

![[image 11.png]]

mode, user id, groupd id

link count = referencias al nodo. el mismo archivo puede tener muchos nombres

access time → se suele de deshabilitar

creation time

modification time

### Data Blocks

![[image 12.png]]

## Inodo (3)

![[image 13.png]]

![[image 14.png]]

![[image 15.png]]

# Manejo de consistencia

![[image 16.png]]

![[image 17.png]]

# Problemas 

![[image 18.png]]

## primera mejora: BSD FFS

![[image 19.png]]

## ext4

![[image 20.png]]

los extents son grupos logicos de bloques continuos( mas grandes y mas faciles de manejar )

# Problemas de FS tradicionales

![[image 21.png]]

# ZFS

![[image 22.png]]

![[image 23.png]]

![[image 24.png]]

![[image 25.png]]

se puede curar a si mismo