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


Continuación de la clase [[Introducción]] 

![[Captura_de_pantalla_2025-08-12_a_la(s)_18.16.59.png]]

UNA SYSCALL ES UNA FUNCION

![[Captura_de_pantalla_2025-08-12_a_la(s)_18.17.23.png]]

![[image 160.png]]

cuando necesitamos acceder al kernel usamos las syscalls

en este caso el syscall read lo que hace es leer un archivo, que retorna la cantidad de caracteres leidos

para todo call deberia haber un ret. el call pushea todo mietras que el ret popea todo

# POSIX

![[image 161.png]]

### File Descriptors

![[image 162.png]]

fork: permite crear nuevos procesos

waitpid(): esperar a que un proceso hijo termine, indica cuando el proceso hijo termina

![[image 163.png]]

## Usos desde la shell

### Pseudocodigo

![[image 164.png]]

esto es una shell muy simplificada

### Caso Real

![[image 165.png]]

si quiero que el comando para cambiar el directorio del proceso del. padre se cambie desde el chill, lo que deberia hacer es pasarle el PID del padre al chdir

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [[Estructura de un Sistema Operativo]] — la frontera usuario/kernel
- [[PIPELINES]] — read, write, close y pipe

**Otras materias**

- **Arqui**  [[Interrupciones]] — la interrupción de software que entra al kernel
- **Arqui**  [[Modo protegido]] — cambio de nivel de privilegio
- **Protos**  [[10. Protos - Sockets]] — la API de sockets son syscalls
- **Protos**  [[sendfile()]] — ejemplo concreto de syscall

<!-- notas-relacionadas:fin -->
