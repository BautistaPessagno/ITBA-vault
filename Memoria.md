---
Created: 2026-05-1419:50
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Memoria

# Reflexion

![[image 26.png]]

# Memoria

![[image 27.png]]

Interposición: se pone entre los procesos y los recursos, se interpone entre ambos

cada proceso accede directo a memoria fisica 

## Problemas

### donde cada proceso accede directo a memoria

![[image 28.png]]

uno no debería poder meterse en la clase del rector

![[image 29.png]]

No se puede dar mas memoria que lo que tiene el sistema

El programador se encarga de la participación de información y memoria

![[image 30.png]]

![[image 31.png]]

se hacen duplicados y ocupan memoria

![[image 32.png]]

el dato tiene que estar cargado necesariamente en 0x402000 porque sino apunta a cualquier lado

![[image 33.png]]

el stack crece para abajo mientras que el heap crece para arriba, haciendo que pueda haber problemas

![[image 34.png]]

si nadie se interpone entonces todos tienen los mismos permisos

## Solucion

![[image 35.png]]

### Espacio de direcciones

![[image 36.png]]

numero marcado vs ubicacion/telefono

un mismo numero esta asociado a distintos servicios

![[image 37.png]]

cada instancia de bash tiene su propio espacio de direcciones, y después SO se encarga de evitar direcciones. Cada rayita es un mapeo realizado por el SO

![[image 38.png]]

![[e985bd0c-b41b-4975-ab5d-1ece7b09f864.png]]

![[07b99bc8-0eb5-4849-a114-85ffdb0b0892.png]]

# Falta una parte de paginación

# Analisis de Costos

la sola ejecución de una instrucción requiere acceso a memoria, podemos tener multiples accesos a memoria por cada instrucción

un dato puede no estar alineado (estar en dos paginas diferentes)

![[image 39.png]]

![[image 40.png]]

![[image 41.png]]

## Translation Lookaside buffer

se hacen muchas referencias a pocas paginas ya que la informacion esta junta

![[image 42.png]]

la tabla muestra frecuencia absoluta y el numero de pagina

![[image 43.png]]

porque cambiarias el page frame?

![[image 44.png]]

## Tabla de Pagina Multinivel

![[image 45.png]]

![[image 46.png]]

## Tablas de Paginas Invertidas

una entrada por cada PAGINA FISICA

asi se ahorra espacio de EDV

hace falta hacer una tabla hasheada de la virtual page, esto hace de indice de la PF correspondiente

![[image 47.png]]

![[image 48.png]]

# Algoritmo de remplazo de paginas

![[image 49.png]]

## Not recently used (NRU)

![[image 50.png]]

## First-in-First Out

![[image 51.png]]

## Second-Chance(SC)

![[image 52.png]]

si es la ultima y no fue referenciada entonces la borro, sino la paso al principio

## Clock(C)

![[image 53.png]]

## Least Recently Used(LRU)

![[image 54.png]]

la realidad es que esto no es mas que una estimacion

## Not Frecuently Used (NFU)

![[image 55.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [[Memory Management]] — algoritmos de gestión
- [[Procesos]] — espacio de direcciones del proceso

**Otras materias**

- **Arqui**  [[Intro Sistemas Operativos(Paginación)]] — paginación y MMU en el hardware
- **Arqui**  [[Memoria Cache]] — jerarquía de memoria
- **PI**  [[PI - Punteros en C]] — qué es realmente una dirección

<!-- notas-relacionadas:fin -->
