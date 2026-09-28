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

![](Attachments/image%2075.png)

## Memoria

![](Attachments/image%2076.png)

# Memory Manager

![](Attachments/image%2077.png)

## Memory management layers

![](Attachments/image%2078.png)

un ejemplo del user-space allocator es el malloc

malloc no es una syscall, puede caer en una como no

las responsabilidades del memory manager son:

- asignacion exclusiva de memoria libre
- liberacion de memoria previamente asignada

![](Attachments/image%2079.png)

# Consideraciones TP

![](Attachments/image%2080.png)

mapeo uno a uno entre virtual y fisica → no hay virtual (memoria real)

[x64BareBones/Bootloader/Pure64/Pure64 Manual.md at master · alejoaquili/x64BareBones](https://github.com/alejoaquili/x64BareBones/blob/master/Bootloader/Pure64/Pure64%20Manual.md)

[ITBA-72.11-SO/kernel-development/building.md at main · alejoaquili/ITBA-72.11-SO](https://github.com/alejoaquili/ITBA-72.11-SO/blob/main/kernel-development/building.md)

# c-unit-testing-example

# Implementaciones de Physical
Memory Allocators

![](Attachments/image%2081.png)

[Expanded Main Page - OSDev Wiki](https://wiki.osdev.org/Expanded_Main_Page)

## Ejemplos

![](Attachments/image%2082.png)

 https://github.com/jubalh/awesome-os 

# Ejemplo catedra

![](Attachments/Captura_de_pantalla_2025-09-30_a_la%28s%29_11.08.39.png)

![](Attachments/image%2083.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Memoria](Memoria.md) — tema principal
- [Procesos](Procesos.md) — memoria por proceso

**Otras materias**

- **Arqui**  [Intro Sistemas Operativos(Paginación)](Intro%20Sistemas%20Operativos%28Paginación%29.md) — paginación
- **PI**  [PI - Punteros en C](PI%20-%20Punteros%20en%20C.md) — malloc/free desde el lado del programa

<!-- notas-relacionadas:fin -->
