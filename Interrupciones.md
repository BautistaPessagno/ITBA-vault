---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
## Interrupción

Es una señal externa que interrumpe al micro para requerir un servicio de atención

## E/S aislada

es una señal especial del micro indica la ejecucion de una operacion de E/S

## Acceso directo a memoria(DMA)

la información se transfiere directamente a la memoria, no requiere de intervención del CPU 

## Mapeo de memoria 

se le otorga un sector de memoria al dispositivo

# Interrupciones

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.13.27.png]]

corre el programa principal → interrupción de teclado(deja de correr el programa principal)→ corre el programa principal → interrupción de mouse(deja de correr el programa principal) → corre el programa → interrupción X → corre el programa

esto sucede en monoprocesadores(los vistos en esta materia), los cuales tienen un solo nucleo, y solo pueden ejecuar un programa a la vez. 

los multi-nucleos igualmente solo tienen un solo busAdress

Programa de interrupcion de Mouse == DRIVER de mouse

habla con el mosue para interpretar las instrucciones que le da

## Rutina de atención de interrupción

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.21.01.png]]

despues de la instruccion n se llama a la inerrupcion y  la siguiente instruccion a ejecutar se guarda en la pila

## Tipos de interrupciones

### Interrupciones de Hardware

Se interrupe la ejecucion del programa activando alguna de la dos entradas que tiene el micro procesador (INTR y NMI)

El procesador tiene una patita para que lo interrumpan

Pero después te dan la opción de **enmascarar** las interrupciones (ignorar las interrupciones)

Esto es porque hay procesos importantes que no tienen que ser interrumpidos de ninguna manera

Entonces la INTR se puede enmascarar mientras que la NMI no se puede

cosas que si o si se tienen que interrupir (bateria,temperatura,)

### Interrupciones de software

se interrumpe la ejecucion del programa al ejecuar la instrucion INT. por ejemplo INT 44h (donde 44h es el numero de rutina de interrupcion a ejecutar) 

## [Interrupciones de Hardware](/1e4f5e1c86fe80a1b489d9fe1c4d2d59#1e4f5e1c86fe803d8654c7d28e7c213e)

el flag IF indica si se debe atender a las interrupciones externas. si IF = 1 ( habilitado ) si IF=0 ( deshabilitado )

### Interrupciones enmascarable

el flag IF se controla con las instrucciones *sti ( set interrups ) y cli ( clear interrups )*

### Interrupciones NO enmascarable

las interrupciones que ingresan por la patita NMI no se pueden enmascarar. Y siempre ejecutan la rutina que se encuantra en la posicion 2h del vector de interrupciones ( INT 2h )

# PIC ( controlador programable de interrupciones )

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.38.24.png]]

los fabricantes de PC agregaron el PIC el cual funiona como gestor de interrupciones

vos le mandas una señal al pic y el pic le manda la interrupcion al procesador

hace que se permitan mas patas

funciona de la siguiente manera:

1. se hace un IRQ(interrupt request)
2. de ahi se manda la señal al procesasdor y el procesador le contesta al pic con INTA(interrupt aknoledge)

de ahi va al IDT (interrupt descriptor table) la cual tiene todos los punteros a funcion con la rutina a interrupcion

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.49.53.png]]

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.51.15.png]]

el pic se cambian en el 20h y 21h 

## PIC en cascada

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.52.39.png]]

colocando 2 PICs en cascada se amplia la cantidad de interrupciones de hardware en la PC

En la PC, se utiliza el IRQ2 del master para conectar el Save

# [Interrupciones de Software](/1e4f5e1c86fe80a1b489d9fe1c4d2d59#1e4f5e1c86fe80bbb335cd927ca700ba)

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.54.03.png]]

# Servicio de BIOS

en el BIOS al iniciar la PC guarda en memoria de rutinas basicas para poder empezar a operar

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.54.53.png]]

# Interrupciones de Hardware por Default

![[Captura_de_pantalla_2025-04-29_a_la(s)_09.59.06.png]]

# Excepciones

Una excepción es un evento generado por el procesador cuando detecta uno o mas condiciones predefinidas al ejecutar instrucciones.

Es decir el procesador se interrumpe a si mismo.

Existen 3 tipos de excepciones:

- **Faults**: Excepción que pueden corregirse.El procesador guarda en la pila la dirección de la instrucción que produjo la falla
- **Trap:** Se utilizan para realizar accesos al sistema operativo.
- **Abort:** No siempre se pueden obtener la instrucción que causo la excepción. Reporta errores severos 

## Excepciones

![[Captura_de_pantalla_2025-04-29_a_la(s)_10.03.42.png]]

es el procesador el que encuentra los errores en la programación

# Modo protegido

al cerrar programas por mal funcionamiento lo que se hace es proteger al sistema

## Conmutación de tareas

![[Captura_de_pantalla_2025-04-29_a_la(s)_10.17.24.png]]

Todas las aplicaciones corren pero un ratito, pero va tan rapido que parece que va todo al tiempo

se usa el **Timer Tick** que es un integrado que esta en la pc que interrumpe al procesador cada cierta cantidad de tiempo

![[Captura_de_pantalla_2025-05-21_a_la(s)_20.49.44.png]]

el SO es un tarea mas que se lo llama en los errores/interrupciones/etc

no tienen las mismas reglas que las otras tareas sino que tiene mas control

## Modo Protegido- Proteccion de Tareas

![[Captura_de_pantalla_2025-04-29_a_la(s)_10.44.27.png]]

### Memory management Unit (MMU)

![[Captura_de_pantalla_2025-04-29_a_la(s)_10.44.16.png]]

- La unidad de segmentacion NO se puede deshabilitar
- La unidad de paginacion SI se puede deshabilitar
![[Captura_de_pantalla_2025-04-29_a_la(s)_10.59.18.png]]

### GDT y LDT

![[Captura_de_pantalla_2025-04-29_a_la(s)_11.01.05.png]]

tiene que haber un puntero por cada espacio de memoria

los descriptores describen cada cacho de memoria que asignaste

esta tabla es importante ya que antes de hacer un acceso a me memoria se fija en la tabla si hay un espacio correspondiente para el proceso

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Interrupciones 1]] — continuación del tema
- [[Clase 4 Intro transmisión Digital]] — E/S por interrupciones vs polling
- [[Modo protegido]] — cambio de contexto y privilegio

**Otras materias**

- **SO**  [[Scheduling]] — el timer que dispara el cambio de proceso
- **SO**  [[SysCall]] — la interrupción de software que entra al kernel

<!-- notas-relacionadas:fin -->
