---
temas:
  - TP
Created: 2026-06-0419:09
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Materia: "[[protos.base|protos]]"
---
# Protos TP

esta el echo y el echo iterativo
el echo iterativo no tiene una cola sino un array de clientes y acepta muchos clientes a la vez

se puede migrar el repo a EPOLL:
select -> epoll

cerrr files descriptors tambien cierra buffers

## strace
imprime todas las llamadas del sistema

>[!tip]
>recomendado para el debuggear, ayuda mucho y se va a usar para corregir

implementar el codigo entre que se despierta un pselect y se vuelve a dormir

lo podemos cambiar a epoll

ver parches en el campus

cada parche es un commit

correr secuencialmente


```shell
git am ../parche00n.patch
```

el parche 2 lo vamos a tenes que aplicar nosotros

el resto se pueden apliclar sin problemas

## Auth
selection kit
una estructure en el `selector.h`
qie guarda un 
fd_selector
fd
data -> aca podemos guardar el estado del socket (autenticado, desconocido, etc)


# Recomendación sobre como hacer el TP

1.  aplicar parches
2. primera implementación nada sobre sock5
3. echo server usando el codigo dado nada mas con las abstracciones dadas
4. ***echo servidor funcionando bien 👍***
5. agregar en el handle de lectura el parceo de protocolo socks
6. aplicar auth
7. conectarse con el origen
8. protocolo/servidor de monitoreo (otro servidor sobre el mismo selector)

>[!tip]
>despues de implementar el echo bien ya luego se puede paralelizar las tareas

se puede implementar socks sin implementacion y despues si
resolucion de DNS bloqueante y despues bloqueante

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[Diseño de protocolos]] — requisitos de diseño del TP
- [[spec]] — especificación del protocolo
- [[10. Protos - Sockets]] — implementación con sockets
- [[Protos TP - Preguntas de defensa]] — preguntas de la defensa
- [[Protos TP - Defensa de commits]] — recorrido por los commits

**Otras materias**

- **PI**  [[PI - Punteros en C]] — manejo de buffers en C
- **SO**  [[SysCall]] — select/poll y llamadas al sistema del servidor
- **SO**  [[Threads]] — modelo de concurrencia del servidor

<!-- notas-relacionadas:fin -->
