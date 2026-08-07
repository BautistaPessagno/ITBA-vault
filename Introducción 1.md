---
temas: []
Cuatri: 1ro-2025
Date: 2025-03-11
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
![[Captura_de_pantalla_2025-03-11_a_la(s)_10.15.04.png]]

# Simulador x86 intel

[VonSim — A 8088-like Assembly Simulator](https://vonsim.github.io/)

> [!note]+ # Bibliografia
> - Los microprocesadores Intel. Barry B. Brey. Prentice Hall.
> - Organización de Computadoras. Andrew S. Tanenbaum. Prentice
> Hall
> - Organización y Arquitectura de Computadoras. William Stallings.
> Megabyte.
> - Apuntes de la materia.
> 

> [!note]+ # Presentación de la materia
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_10.36.19.png]]

# Arquitecturas

![[Captura_de_pantalla_2025-03-11_a_la(s)_10.53.09.png]]

> [!note]+ ## PC vs MAC
> - las pc son de hardware libre
> - hardware cerrado(unico fabricante, apple)
> - android es un software pero con multiples fabricantes de software

# Programas y Binarios

> [!note]+ ## Compilación y linkedición en C
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_11.08.39.png]]

> [!note]+ ## Ejecución de un programa
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_11.14.49.png]]

> [!note]+ ## Programa en memoria
> <!-- Column 1 -->
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_11.22.43.png]]
> 
> <!-- Column 2 -->
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_11.22.51.png]]

> [!note]+ ## Programa en disco
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_11.56.27.png]]
> 

> [!note]+ ## Analisis de binario
> ```bash
> file <myFile> #ver info del file
> ```
> 
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_12.04.23.png]]
> 
> ```bash
> ./ #correr
> ```
> 
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_12.04.33.png]]
> 
> ```bash
> strings #ver todo el texto
> ```
> 
> ![[Captura_de_pantalla_2025-03-11_a_la(s)_12.11.10.png]]
> 
> - Con el **editor hexadecimal bless** podemos cambiar el texto y alterar el ejecutable
> - al querer quitar/agregar cosas  va a fallar porque hay que espacio que se debería quitar/agregar
> 
> ```bash
> objdump <archivo> #muestra informacion del archivo
> #depende de que le pidas
> ```
> 

# Introducción SO

![[Captura_de_pantalla_2025-03-11_a_la(s)_12.19.57.png]]

## System Calls

permite a los programas en el espacio de usuario interactuar con el kernel del sistema

![[Captura_de_pantalla_2025-03-11_a_la(s)_12.21.21.png]]

### Formas de ejecutar system call

- INT 80h
    - Interrupción nro 80. Consume más tiempo
- Instrucciones **SYSCALL**/SYSRET (Intel) y **SYSENTER**/SYSEXIT (AMD)
    - Disponibles desde Pentium II
    - La librería de C depende de la arquitectura
- vsyscall y VDSO
    - Syscall virtuales y Virtual Dynamic Shared Object
    - Linux crea páginas de memoria en user space para acelerar tiempos

### Ejemplo de syscall

![[Captura_de_pantalla_2025-03-11_a_la(s)_12.25.08.png]]

![[Captura_de_pantalla_2025-03-11_a_la(s)_12.25.24.png]]

# Procesadores y Lenguaje ASM
(en Intel)

## Registros de Intel para programas

![[Captura_de_pantalla_2025-03-11_a_la(s)_12.39.47.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Clase 1 Intro]] — clase introductoria
- [[Clase 2 ASM intel]] — siguiente tema
- [[Resumen Arqui]] — resumen integrador

<!-- notas-relacionadas:fin -->
