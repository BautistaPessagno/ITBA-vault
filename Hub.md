---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[protos.base|protos]]"
temas:
  - Hub
  - Capa 1
  - Dominio de colisión
---
Un **hub** es un dispositivo de red **simple/tonto** que opera en la **capa 1 (Física)** del modelo OSI.

**¿Qué hace?**

- **No distingue** direcciones MAC ni destinos
- Cuando recibe una señal, la **replica a TODOS los puertos** sin excepción
- Todos los dispositivos comparten el mismo **dominio de colisión**
- Si dos dispositivos transmiten a la vez, ocurre una **colisión**

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[8. Protos - Enlace]] — dominio de colisión en capa de enlace
- [[Swicth]] — la versión inteligente de capa 2
- [[Router]] — el de capa 3

**Otras materias**

- **Arqui**  [[Clase 4 Intro transmisión Digital]] — el hub repite esta señal eléctrica sin interpretarla

<!-- notas-relacionadas:fin -->
