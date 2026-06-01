---
SO: "[[Clases.base]]"
Created: 2026-05-1419:53
Tags:
  - Practica
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# PIPELINES

todo se maneja como un archivo, sin importar que sea

read, write y close

en linux cada comando hace una cosa y se van juntando con pipelines

# System Calls

## Posix

![[image 166.png]]

en read devuelva la cantidad de bytes que leyo mientras que en write la cantidad de writes que escribió

![[image 167.png]]

son locales al proceso

## Pipe

mecanismo de comunicacion unidireccional. el mas simple

![[image 168.png]]

![[Captura_de_pantalla_2025-08-12_a_la(s)_14.19.28.png]]