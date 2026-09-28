---
Created: 2026-05-1419:53
Tags: []
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[SO.base | SO]]"
temas:
---
# Procesos

# Modelo de Procesos

![](Attachments/image%20122.png)

el software se organiza en procesos secuenciales o simplemente procesos

proceso: abstracción de programa en ejecución

programa: almacenado en disco, no hace nada. archivo con instrucciones

se le asigna un periodo de tiempo a cada procesos

el switch entre un proceso y otro tiene un costo

![](Attachments/image%20123.png)

en el grafico . c se ve como cada instante corresponde a un proceso especifico

![](Attachments/image%20124.png)

# Procesos

![](Attachments/image%20125.png)

cada proceso tiene su propio binario

un proceso no puede ejecutar su propio binario

## Creación de Procesos

![](Attachments/image%20126.png)

no se puede crear un proceso sin el sistema operativo → se usan syscalls

el primero proceso es creado por el sistema operativo ya que el sistema operativo no es un proceso por si mismo

### Unix

![](Attachments/image%20127.png)

el execve cambia la imagen del proceso, en el fork el proceso hijo compartia heap, stack y código

### Win32

![](Attachments/image%20128.png)

## Terminación de Procesos

![](Attachments/image%20129.png)

al programa, el compilador le agrega cosas, por lo tanto al terminar el main hay mas cosas por correr. por lo tanto al final hay un exit

![](Attachments/image%20130.png)

todo lo distinto de 0 se considera un error

![](Attachments/image%20131.png)

caso &&: ejecutar elk comando 1, si retorna 0 ejecutar el segundo

caso ||: contrario al del &&

## Jerarquia de procesos

![](Attachments/image%20132.png)

el proceso innit es e padre de todos los procesos en UNIX

![](Attachments/image%20133.png)

de A a B se puede hacer un execve que no va a dejar rastro de relación entre padre e hijo

## Grupo de Procesos - UNIX

![](Attachments/image%20134.png)

## Estados de procesos

![](Attachments/image%20135.png)

![](Attachments/image%20136.png)

ejecutando es que este corriendo en el cpu

en ready no esta ejecutando en ese momento exacto pero esta esperando a ser usado

> [!note]+ ### Preguntas estilo parcial
> > Se rompió el timer. No existe otra fuente de int ni excepciones. ¿puedo el SO volver a tomar el control?
> 
> kernel puede tomar control con syscalls
> 
> > porque no hay ready → blocked o blocked→ running
> 
> para bloquear el proceso tiene que hacer algo que lo bloquee, y para eso tiene que estar corriendo
> 
> (no tiene sentido que lo bloquee el so "desde afuera")
> 
> ocurre de manera sincronica, de manera directa por haber hecho algo. estando ready uno no ejecuta nada, por lo tanto no se puede bloquear
> 
> para desbloquear el proceso tiene que primero ser desbloqueado para ready por el so luego de algo externo, entonces no pasa nunca de blocked a running sin un intermedio en el que se pone en ready mientras corre algo más
> 

## Implementación de procesos

![](Attachments/image%20137.png)

es un struct el cual puede tener el tamaño que uno le otorgue

como el padre sabe del retorno del hijo?
accede al PCB a traves de una syscall(en el wait)

![](Attachments/image%20138.png)

### estado zombie y huerfano

un proceso esta en estado zombie desde que hace exit hasta que alguien le hace waitpid

si desaparece un proceso padre, entonces los hijos quedan huérfanos, lo que se hace es que innit lo adopte

innit lo que hace es adoptarlos y hacer el wait para que no queden zombies

# Implementación de procesos

![](Attachments/Captura_de_pantalla_2025-08-27_a_la%28s%29_10.52.33.png)

es una tabla de structs

![](Attachments/image%20139.png)

por ejemplo el estado del proceso le puede importar al scheduler

![](Attachments/image%20140.png)

![](Attachments/image%20141.png)

# Modelando multiprogramación

![](Attachments/image%20142.png)

![](Attachments/image%20143.png)

no, no es realista

![](Attachments/image%20144.png)

![](Attachments/image%20145.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (SO)**

- [Estructura de un Sistema Operativo](Estructura%20de%20un%20Sistema%20Operativo.md) — dónde vive el proceso
- [Scheduling](Scheduling.md) — cómo se eligen los procesos
- [Threads](Threads.md) — hilos dentro del proceso
- [Memoria](Memoria.md) — espacio de direcciones

**Otras materias**

- **Arqui**  [Intro Sistemas Operativos(Paginación)](Intro%20Sistemas%20Operativos%28Paginación%29.md) — paginación del espacio de direcciones
- **Arqui**  [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — el stack del proceso en detalle

<!-- notas-relacionadas:fin -->
