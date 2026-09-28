---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
## Ejemplo comercial

![](Attachments/image%20211.png)

dentro del procesador se encuentran 15mb de memoria

no se puede programar la memoria cache, para el programador es transparente

![](Attachments/image%20212.png)

es muy rapida

![](Attachments/image%20213.png)

## Memoria Cache- Causa

![](Attachments/image%20214.png)

de esta forma no tiene que ir a buscar cada instruccion a la memoria

## Funcionamiento

![](Attachments/image%20215.png)

el procesador le pido primero al cache, si esta se lo da, si no esta va al bus a buscarlo a la memoria

## Historia

![](Attachments/image%20216.png)

# Memoria Cache

![](Attachments/image%20217.png)

se divide en bloques

### Ejemplo

![](Attachments/image%20218.png)

![](Attachments/image%20219.png)

etiquetas = punteros a cada bloque existente de la ram

![](Attachments/image%20220.png)

el controlador se fija si tiene el bloque a puntero (la etiquieta), devuelve por los datos en y avisa que hubo un acierto sino un desacierto, entonces se va a buscarlo y lo trae por medio del bus

## Tipos de mapeo

![](Attachments/image%20221.png)

hoy en dia se usa asociativo

## Politicas de restitución 

![](Attachments/image%20222.png)

## Actualización de RAM

![](Attachments/image%20223.png)

hoy en día se hace escritura obligada, primero se edita la cache y después la misma se sincroniza con la ram

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Memoria Cache](Memoria%20Cache.md) — desarrollo completo del tema
- [Clase 4 Intro transmisión Digital](Clase%204%20Intro%20transmisión%20Digital.md) — DRAM vs SRAM y tiempos de acceso

<!-- notas-relacionadas:fin -->
