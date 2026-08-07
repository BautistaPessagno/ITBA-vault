---
Created: 2026-05-1419:53
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Memory Management

# Abstracción de recursos

![[image 75.png]]

## Memoria

![[image 76.png]]

# Memory Manager

![[image 77.png]]

## Memory management layers

![[image 78.png]]

un ejemplo del user-space allocator es el malloc

malloc no es una syscall, puede caer en una como no

las responsabilidades del memory manager son:

- asignacion exclusiva de memoria libre
- liberacion de memoria previamente asignada

![[image 79.png]]

# Consideraciones TP

![[image 80.png]]

mapeo uno a uno entre virtual y fisica → no hay virtual (memoria real)

[x64BareBones/Bootloader/Pure64/Pure64 Manual.md at master · alejoaquili/x64BareBones](https://github.com/alejoaquili/x64BareBones/blob/master/Bootloader/Pure64/Pure64%20Manual.md)

[ITBA-72.11-SO/kernel-development/building.md at main · alejoaquili/ITBA-72.11-SO](https://github.com/alejoaquili/ITBA-72.11-SO/blob/main/kernel-development/building.md)

# c-unit-testing-example

# Implementaciones de Physical
Memory Allocators

![[image 81.png]]

[Expanded Main Page - OSDev Wiki](https://wiki.osdev.org/Expanded_Main_Page)

## Ejemplos

![[image 82.png]]

 https://github.com/jubalh/awesome-os 

# Ejemplo catedra

![[Captura_de_pantalla_2025-09-30_a_la(s)_11.08.39.png]]

![[image 83.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [[Memoria]] — tema principal
- [[Procesos]] — memoria por proceso

**Otras materias**

- **Arqui**  [[Intro Sistemas Operativos(Paginación)]] — paginación
- **PI**  [[PI - Punteros en C]] — malloc/free desde el lado del programa

<!-- notas-relacionadas:fin -->
