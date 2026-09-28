---
Created: 2026-05-1419:53
Tags:
  - Practica
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# PIPELINES

todo se maneja como un archivo, sin importar que sea

read, write y close

en linux cada comando hace una cosa y se van juntando con pipelines

# System Calls

## Posix

![](Attachments/image%20166.png)

en read devuelva la cantidad de bytes que leyo mientras que en write la cantidad de writes que escribió

![](Attachments/image%20167.png)

son locales al proceso

## Pipe

mecanismo de comunicacion unidireccional. el mas simple

![](Attachments/image%20168.png)

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_14.19.28.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [SysCall](SysCall.md) — read, write, close
- [IPC](IPC.md) — pipes como mecanismo de comunicación
- [File System](File%20System.md) — todo es un archivo
- [Entorno de desarrollo](Entorno%20de%20desarrollo.md) — encadenar comandos en bash

<!-- notas-relacionadas:fin -->
