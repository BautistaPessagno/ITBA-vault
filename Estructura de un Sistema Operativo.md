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

![[image 155.png]]

un ejemplo es el tp de arqui. todo compila en un unico binario

## Micro Kernels

![[image 156.png]]

dejamos lo esencial en el kernel. el resto lo dejamos por fuera como clientes (ej: drivers)

el resto corre en user-mode

### MINIX

![[image 157.png]]

## Monolithic vs Microkernels

![[Captura_de_pantalla_2025-08-12_a_la(s)_20.24.47.png]]

### Mecanismo vs Politica

![[Captura_de_pantalla_2025-08-12_a_la(s)_20.25.50.png]]

## Virtual Machines

![[Captura_de_pantalla_2025-08-12_a_la(s)_20.44.25.png]]

![[image 158.png]]

En el hipervisor 1 tengo simultaneamente corriendo los dos SO simultaneamente

el hipervisor 2 es como en el caso de Quemu

el 1 es mas eficiente porque se salta una capa

# Practica

![[image 159.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [[Introducción]] — clase anterior
- [[SysCall]] — la interfaz del kernel
- [[Procesos]] — tema siguiente

**Otras materias**

- **Arqui**  [[Modo protegido]] — modo usuario vs kernel en el hardware

<!-- notas-relacionadas:fin -->
