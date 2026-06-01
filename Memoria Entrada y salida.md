---
temas:
  - Unidad 2
Cuatri: 1ro-2025
Date: 2025-04-08
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
# Intro a Transmisión Digital

## Codificación de linea

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.20.45.png]]

## Codificación unipolar

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.21.02.png]]

## Transmisión serie

un dato atras del otro

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.34.07.png]]

## Transmisión Paralela

llega todo junto

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.34.51.png]]

me permite acceder a datos mas rapido

hoy en día es el que mas se usa

> [!note]+ # Tipos de Arquitectura
> ### Von Neuman
> 
> un solo tipo de memoria para todo(memoria ram)
> 
> memoria que se usa hoy en dia
> 
> ![[Captura_de_pantalla_2025-04-08_a_la(s)_10.41.30.png]]
> 
> ### Harvard
> 
> ![[Captura_de_pantalla_2025-04-08_a_la(s)_10.41.48.png]]
> 

# Sistema de Entrada y Salida

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.49.13.png]]

## CPU

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.00.00.png]]

### Resumen

![[Captura_de_pantalla_2025-04-08_a_la(s)_10.49.32.png]]

- **Unidad de Control:** Recupera Instrucciones de memoria, las decodifica, Escribe en memoria
- **Unidad de Ejecución:** Lleva a cabo la ejecución de la instrucción
- **Registros: **Memoria interna utilizada como variable
- **Flags:** Indican eventos luego de ejecutar instrucciones

### Veamos el simulador

[VonSim — A 8088-like Assembly Simulator](https://vonsim.github.io/)

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.03.13.png]]

# Mapa de Memoria

![[Captura_de_pantalla_2025-04-08_a_la(s)_11.49.09.png]]

- Supongamos un procesador que tiene 16 líneas de bus de
direcciones y 8 líneas de bus de datos.
- ¿Que cantidad de información puede acceder ?
- ¿Y un procesador que tiene 16 líneas de bus de direcciones y
16 líneas de bus de datos ?
- ¿Y un procesador con 32 líneas de datos y 32 líneas de
Direcciones ?

## IP (puntero a instruccion)

se guarda la dirección de memoria de donde se puede conseguir la siguiente instrucción

tiene una incrementacion automatica y no hace falta incrementarlo

# Memorias

## Clasificacion

- Por el **modo** en que se accede a los datos
- Por las **operaciones** que aceptan
- Por la **duración** de los datos

# Tipo ROM

ROM (Read only memory)

- Mantienen su información sin energía (no volátil)
- La escritura es más lenta que la RAM.

## Tipo RAM

RAM (Random Acces Memory)

se usan las dos, una para el ram y la otra para el Cache

- Pierde su información sin energía. (volátil)

### Tipos de Ram

- DRAM:
    - Necesita refresco de valores cada n milisegundos
    - Menos compleja. Más económica.
    - Más lenta
- SRAM:
    - No necesita refresco.
    - Más compleja, más costosa.
    - Más rápida
    - Se suele utilizar para memoria cache.

### Tiempos de Acceso

Es el tiempo que le toma a una memoria RAM para completar un acceso después de otro.
Se compone de:

- **Latencia** ( tiempo que tarda en devolver el valor la memoria )
- Transferencia 

Las <u>DRAM</u> suelen tener tiempos entre 50 y 150 ns.

Las <u>SRAM</u> menores a 10 ns.

### Operacion

Las memorias para operar utilizan:

- Acción a realizar (lectura o escritura)
- Dirección de la palabra a acceder.
- Dato (entrante o saliente según acción)

## Estructura

Si el procesador, como es el caso de Intel, quiere mantener compatibilidad hacia atrás, permite acceder a la memoria a nivel byte.

Por lo tanto la decodificación cambia según el tipo de memoria

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.39.18.png]]

![[Captura_de_pantalla_2025-04-08_a_la(s)_12.39.57.png]]

## Memoria Comercial

<!-- Column 1 -->
![[Captura_de_pantalla_2025-04-08_a_la(s)_12.40.19.png]]

<!-- Column 2 -->
![[Captura_de_pantalla_2025-04-08_a_la(s)_12.40.27.png]]