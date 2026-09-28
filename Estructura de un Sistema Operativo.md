---
Created: 2026-05-1419:49
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Estructura de un Sistema Operativo

Existen distintas estructuras

- Monolithic system
- Microkernels
- Virtual Machines

## Monolithic system

![](Attachments/image%20155.png)

un ejemplo es el tp de arqui. todo compila en un unico binario

## Micro Kernels

![](Attachments/image%20156.png)

dejamos lo esencial en el kernel. el resto lo dejamos por fuera como clientes (ej: drivers)

el resto corre en user-mode

### MINIX

![](Attachments/image%20157.png)

## Monolithic vs Microkernels

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_20.24.47.png)

### Mecanismo vs Politica

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_20.25.50.png)

## Virtual Machines

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_20.44.25.png)

![](Attachments/image%20158.png)

En el hipervisor 1 tengo simultaneamente corriendo los dos SO simultaneamente

el hipervisor 2 es como en el caso de Quemu

el 1 es mas eficiente porque se salta una capa

# Practica

![](Attachments/image%20159.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Introducción](Introducción.md) — clase anterior
- [SysCall](SysCall.md) — la interfaz del kernel
- [Procesos](Procesos.md) — tema siguiente

**Otras materias**

- **Arqui**  [Modo protegido](Modo%20protegido.md) — modo usuario vs kernel en el hardware

<!-- notas-relacionadas:fin -->
