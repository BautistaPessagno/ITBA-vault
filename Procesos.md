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

![[image 122.png]]

el software se organiza en procesos secuenciales o simplemente procesos

proceso: abstracción de programa en ejecución

programa: almacenado en disco, no hace nada. archivo con instrucciones

se le asigna un periodo de tiempo a cada procesos

el switch entre un proceso y otro tiene un costo

![[image 123.png]]

en el grafico . c se ve como cada instante corresponde a un proceso especifico

![[image 124.png]]

# Procesos

![[image 125.png]]

cada proceso tiene su propio binario

un proceso no puede ejecutar su propio binario

## Creación de Procesos

![[image 126.png]]

no se puede crear un proceso sin el sistema operativo → se usan syscalls

el primero proceso es creado por el sistema operativo ya que el sistema operativo no es un proceso por si mismo

### Unix

![[image 127.png]]

el execve cambia la imagen del proceso, en el fork el proceso hijo compartia heap, stack y código

### Win32

![[image 128.png]]

## Terminación de Procesos

![[image 129.png]]

al programa, el compilador le agrega cosas, por lo tanto al terminar el main hay mas cosas por correr. por lo tanto al final hay un exit

![[image 130.png]]

todo lo distinto de 0 se considera un error

![[image 131.png]]

caso &&: ejecutar elk comando 1, si retorna 0 ejecutar el segundo

caso ||: contrario al del &&

## Jerarquia de procesos

![[image 132.png]]

el proceso innit es e padre de todos los procesos en UNIX

![[image 133.png]]

de A a B se puede hacer un execve que no va a dejar rastro de relación entre padre e hijo

## Grupo de Procesos - UNIX

![[image 134.png]]

## Estados de procesos

![[image 135.png]]

![[image 136.png]]

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

![[image 137.png]]

es un struct el cual puede tener el tamaño que uno le otorgue

como el padre sabe del retorno del hijo?
accede al PCB a traves de una syscall(en el wait)

![[image 138.png]]

### estado zombie y huerfano

un proceso esta en estado zombie desde que hace exit hasta que alguien le hace waitpid

si desaparece un proceso padre, entonces los hijos quedan huérfanos, lo que se hace es que innit lo adopte

innit lo que hace es adoptarlos y hacer el wait para que no queden zombies

# Implementación de procesos

![[Captura_de_pantalla_2025-08-27_a_la(s)_10.52.33.png]]

es una tabla de structs

![[image 139.png]]

por ejemplo el estado del proceso le puede importar al scheduler

![[image 140.png]]

![[image 141.png]]

# Modelando multiprogramación

![[image 142.png]]

![[image 143.png]]

no, no es realista

![[image 144.png]]

![[image 145.png]]