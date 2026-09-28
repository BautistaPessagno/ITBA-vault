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

![](Attachments/image%2026.png)

# Memoria

![](Attachments/image%2027.png)

Interposición: se pone entre los procesos y los recursos, se interpone entre ambos

cada proceso accede directo a memoria fisica 

## Problemas

### donde cada proceso accede directo a memoria

![](Attachments/image%2028.png)

uno no debería poder meterse en la clase del rector

![](Attachments/image%2029.png)

No se puede dar mas memoria que lo que tiene el sistema

El programador se encarga de la participación de información y memoria

![](Attachments/image%2030.png)

![](Attachments/image%2031.png)

se hacen duplicados y ocupan memoria

![](Attachments/image%2032.png)

el dato tiene que estar cargado necesariamente en 0x402000 porque sino apunta a cualquier lado

![](Attachments/image%2033.png)

el stack crece para abajo mientras que el heap crece para arriba, haciendo que pueda haber problemas

![](Attachments/image%2034.png)

si nadie se interpone entonces todos tienen los mismos permisos

## Solucion

![](Attachments/image%2035.png)

### Espacio de direcciones

![](Attachments/image%2036.png)

numero marcado vs ubicacion/telefono

un mismo numero esta asociado a distintos servicios

![](Attachments/image%2037.png)

cada instancia de bash tiene su propio espacio de direcciones, y después SO se encarga de evitar direcciones. Cada rayita es un mapeo realizado por el SO

![](Attachments/image%2038.png)

![](Attachments/e985bd0c-b41b-4975-ab5d-1ece7b09f864.png)

![](Attachments/07b99bc8-0eb5-4849-a114-85ffdb0b0892.png)

# Falta una parte de paginación

# Analisis de Costos

la sola ejecución de una instrucción requiere acceso a memoria, podemos tener multiples accesos a memoria por cada instrucción

un dato puede no estar alineado (estar en dos paginas diferentes)

![](Attachments/image%2039.png)

![](Attachments/image%2040.png)

![](Attachments/image%2041.png)

## Translation Lookaside buffer

se hacen muchas referencias a pocas paginas ya que la informacion esta junta

![](Attachments/image%2042.png)

la tabla muestra frecuencia absoluta y el numero de pagina

![](Attachments/image%2043.png)

porque cambiarias el page frame?

![](Attachments/image%2044.png)

## Tabla de Pagina Multinivel

![](Attachments/image%2045.png)

![](Attachments/image%2046.png)

## Tablas de Paginas Invertidas

una entrada por cada PAGINA FISICA

asi se ahorra espacio de EDV

hace falta hacer una tabla hasheada de la virtual page, esto hace de indice de la PF correspondiente

![](Attachments/image%2047.png)

![](Attachments/image%2048.png)

# Algoritmo de remplazo de paginas

![](Attachments/image%2049.png)

## Not recently used (NRU)

![](Attachments/image%2050.png)

## First-in-First Out

![](Attachments/image%2051.png)

## Second-Chance(SC)

![](Attachments/image%2052.png)

si es la ultima y no fue referenciada entonces la borro, sino la paso al principio

## Clock(C)

![](Attachments/image%2053.png)

## Least Recently Used(LRU)

![](Attachments/image%2054.png)

la realidad es que esto no es mas que una estimacion

## Not Frecuently Used (NFU)

![](Attachments/image%2055.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Memory Management](Memory%20Management.md) — algoritmos de gestión
- [Procesos](Procesos.md) — espacio de direcciones del proceso

**Otras materias**

- **Arqui**  [Intro Sistemas Operativos(Paginación)](Intro%20Sistemas%20Operativos%28Paginación%29.md) — paginación y MMU en el hardware
- **Arqui**  [Memoria Cache](Memoria%20Cache.md) — jerarquía de memoria
- **PI**  [PI - Punteros en C](PI%20-%20Punteros%20en%20C.md) — qué es realmente una dirección

<!-- notas-relacionadas:fin -->
