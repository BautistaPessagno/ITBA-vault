---
Created: 2026-05-1419:54
Tags:
  - Teorica
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# SysCall


Continuación de la clase [Introducción](Introducción.md) 

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_18.16.59.png)

UNA SYSCALL ES UNA FUNCION

![](Attachments/Captura_de_pantalla_2025-08-12_a_la%28s%29_18.17.23.png)

![](Attachments/image%20160.png)

cuando necesitamos acceder al kernel usamos las syscalls

en este caso el syscall read lo que hace es leer un archivo, que retorna la cantidad de caracteres leidos

para todo call deberia haber un ret. el call pushea todo mietras que el ret popea todo

# POSIX

![](Attachments/image%20161.png)

### File Descriptors

![](Attachments/image%20162.png)

fork: permite crear nuevos procesos

waitpid(): esperar a que un proceso hijo termine, indica cuando el proceso hijo termina

![](Attachments/image%20163.png)

## Usos desde la shell

### Pseudocodigo

![](Attachments/image%20164.png)

esto es una shell muy simplificada

### Caso Real

![](Attachments/image%20165.png)

si quiero que el comando para cambiar el directorio del proceso del. padre se cambie desde el chill, lo que deberia hacer es pasarle el PID del padre al chdir

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Estructura de un Sistema Operativo](Estructura%20de%20un%20Sistema%20Operativo.md) — la frontera usuario/kernel
- [PIPELINES](PIPELINES.md) — read, write, close y pipe

**Otras materias**

- **Arqui**  [Interrupciones](Interrupciones.md) — la interrupción de software que entra al kernel
- **Arqui**  [Modo protegido](Modo%20protegido.md) — cambio de nivel de privilegio
- **Protos**  [10. Protos - Sockets](10.%20Protos%20-%20Sockets.md) — la API de sockets son syscalls
- **Protos**  [sendfile()](sendfile%28%29.md) — ejemplo concreto de syscall

<!-- notas-relacionadas:fin -->
