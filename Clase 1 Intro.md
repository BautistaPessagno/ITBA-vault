---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
# Introducción a la materia

![[image 224.png]]

El procesador es el que se encarga de proteger una aplicación A de una B, la primera barrera de defensa

> [!note]+ ### Bibliografia
> ![[image 225.png]]

# Actualidad

![[image 226.png]]

> [!note]+ ## Consolas
> ![[image 227.png]]
> 
> casi el mismo procesador que una pc con menos memoria
> 
> lo que mas cambia es el sistema operativo y que esta pensado para monotareas

# Programas y Binarios

![[image 228.png]]

antes de convertirse en codigo binario se convierte en codigo assembler

## Ejecución de un programa

![[image 229.png]]

El call o jump es un salto a donde tiene que ir. Analogia: es como una “Búsqueda del tesoro (papelito que dice a donde ir a buscar la siguiente pista)”

Se lo lleva del disco a la memoria porque la memoria es mucho mas rapida (la proxima vez lee mas rapido). En cuanto a programas sirve para loops, llamados, etc

## Programa en Memoria

![[image 230.png]]

El lenguaje C crea la pila y la maneja el lenguaje
El Codigo son las instrucciones, los datos es lo que definimos

Heap: ejemplo: malloc.

![[image 231.png]]

los punteros son registros

## Programa en Disco

![[image 232.png]]

Encabezdo y datos del archivo
el encabezado (Header) es la informacion del archivo que se crea en la linkeditacion

# Análisis de binario (en linux)

![[image 233.png]]

linux no tiene extension (.txt, .pdf, etc), la información del tipo de archivo se consigue usando **file** el cual consigue la información del header

Con el comando **strings** se puede conseguir todas las cadenas de caracteres sobre un archivo que tengan representación ASCII que se puedan imprimir

Para analizar y editar el binario se necesita un editor hexadecial como **bless**

![[image 234.png]]

# Llamadas a funciones call

programas en memoria

![[image 235.png]]

el Ret hace pop del stack para conseguir el siguiente valor a correr del call y salta a esa dirección

# Intro a SO

![[image 236.png]]

un sistema operativo es un programa el cual es el primero que corre al iniciar una PC. (imposible que corra otra cosa). antes que todos corre la bios, la cual viene en la memoria ROM

el sistema operativo gestiona el hardware de la pc con system calls

![[image 237.png]]

ejemplos de syscalls

![[image 238.png]]

![[image 239.png]]

# Procesadores y Lenguaje ASM (en intel)

## Registros de intel

se encuentran en el procesador

Son un acceso directo, son mas rápidos

![[image 240.png]]

![[image 241.png]]

trabajan de a pares. el CS apunta al inicio del segmento mientras que el EIP es la posición relativa con respecto al inicio del segmento

![[image 242.png]]

Procesador 32 bits

![[image 243.png]]

![[image 244.png]]

cuando estas haciendo muchas cosas en realidad en un instante de tiempo solo estas haciendo una cosa (multitarea). de correr todos los procesos al mismo tiempo se debería tener 5 instruction pointers.

## Registros

![[image 245.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Introducción 1]] — introducción a la materia
- [[Clase 2 ASM intel]] — clase siguiente
- [[Resumen Arqui]] — resumen integrador

<!-- notas-relacionadas:fin -->
