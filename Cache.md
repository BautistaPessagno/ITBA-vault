---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
## Ejemplo comercial

![[image 211.png]]

dentro del procesador se encuentran 15mb de memoria

no se puede programar la memoria cache, para el programador es transparente

![[image 212.png]]

es muy rapida

![[image 213.png]]

## Memoria Cache- Causa

![[image 214.png]]

de esta forma no tiene que ir a buscar cada instruccion a la memoria

## Funcionamiento

![[image 215.png]]

el procesador le pido primero al cache, si esta se lo da, si no esta va al bus a buscarlo a la memoria

## Historia

![[image 216.png]]

# Memoria Cache

![[image 217.png]]

se divide en bloques

### Ejemplo

![[image 218.png]]

![[image 219.png]]

etiquetas = punteros a cada bloque existente de la ram

![[image 220.png]]

el controlador se fija si tiene el bloque a puntero (la etiquieta), devuelve por los datos en y avisa que hubo un acierto sino un desacierto, entonces se va a buscarlo y lo trae por medio del bus

## Tipos de mapeo

![[image 221.png]]

hoy en dia se usa asociativo

## Politicas de restitución

[[NOTION_PAGE:28df5e1c-86fe-801c-b4d3-d715f6d2352c]] 

![[image 222.png]]

## Actualización de RAM

![[image 223.png]]

hoy en día se hace escritura obligada, primero se edita la cache y después la misma se sincroniza con la ram