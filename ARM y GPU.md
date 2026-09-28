---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
## Historia de arm

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.18.26.png)

en el 2004 se convierte en el diseño de los telefonos celulares

y en el 2018 se convirtio en el procesador mas implementado a nivel mundial

se crea entre las empresas Acorn Apple y VLSI

<u>**NO**</u>** **crean procesadores, los diseñan y hacen simulaciones

# SoC (system on a chip)

esta integrado por:

- Procesador
- Memorias (ROM, RAM, Flash)
- Osciladores
- Conversores A/D y D/A
- interfaces(USB, Ethernet, USART)

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.36.17.png)

## Modos de procesador

se pueden separar en con y sin privilegios

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.47.58.png)

es mas detallista en el modo que trabajamos, se separa en muchos modos, en Intel el modo protegido cubre todas

los registros son 1, 2, 3…

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.49.20.png)

el r15 es instruccion pointer 

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.49.48.png)

## Registros Flags

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_13.50.14.png)

## Caracteristicas Generales

- casi todas las instrucciones se ejecutan en un ciclo clock y tienen tamaño fijo
    - es decir casi todas las instrucciones tardan lo mismo en ejecutarse
- todas las familias de procesadores comparten el mismo conjunto de procesadores
- tipos de datos 8/16/32 bits
- pocos modos de direccionamiento
- No se crea fragmentación de memoria
- usa RISC

## Mapa de memoria de una ARM

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.08.30.png)

0 esta abajo y arriba hay 4G

las primeras direcciones estan reservadas para la ROM (Non-volatile)

despues viene la ram

despues espacio reservado para uso futuro

el mapa de memoria tiene un lugar reservado para los perifericos

<u>**NO**</u> tiene IO/M no tienen (no tienen mapa de entrada y salida), estan en la zona alta del mapa de memoria

## Pipeline en ARM7

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.12.41.png)

El concepto de pipeline existe en todos los procesadores del mundo(intel, ARM, TODOS)

Para ejecutar una instruccion se hacen tres pasos

Fetch, Decode, Execute

1. Fetch, busca la instruccion en memoria
2. Decode, la decodifica
3. Execute, la ejectura

Se creo Pipelines para poder hacer distintas etapas al mismo tiempo

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.22.32.png)

los jumps son problematicos porque pueden hacer que se haga fetchs y decode al pedo

Intel creo un jump prediction para poder administrar el jump

cuando detecta un jump condicional, pasa el codigo a una sandbox donde simula el codigo para saber si hace el jump

## Instrucciones en ARM

instrucciones condicionales

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.29.14.png)

Vemos que con las instrucciones condicionales se evitan los saltos en el código que demoran la ejecución del programa

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.29.41.png)

Antes de la ejecución de cada instrucción se chequea los bits de condición (
31:28, los 4 bits significativos  ) para determinar si se debe ejecutar o no.
Por ejemplo:
La condición ‘0000’ significa EQUAL. Por lo tanto la instrucción sólo podrá ser
ejecutada si el flag Z ( zero ) está activo.

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_14.31.46.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [64bits y ARM](64bits%20y%20ARM.md) — ARM
- [GPU](GPU.md) — detalle de GPU

<!-- notas-relacionadas:fin -->
