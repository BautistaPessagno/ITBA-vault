---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-03-1016:19
Materia: "[[protos.base|protos]]"
temas:
  - Material Didactico
  - Snapshots
---
# Protos - Material Didactico


```c
#include <time.h>

typedef enum{
	TRACE,
	DEBUG
	WARN
	ERROR
}level;

struct logentry{
time_t when;
char *msg;
level l;
};
```

# VM
subir el uso del disco
se va a tocar la configuracion la red mas adelante


# Snapshots
Se puede sacar snapshots para saber cuando funciona
en caso de que se ropa o hagamos algo mal podemos volver al snapshot con un restore

# Direccionamiento y HTTP

en la siguiente pagina:
https://datatracker.ietf.org/doc/html/rfc2616#section-14.19
solo el /doc/html/rfc2616 es el path.
lo que esta luego del `#` no se pasa, es simplemente algo que se le pasa al navegador para saber a que fragmento quiere ir
al poner signo de pregunta `?` lo que se hace es pasar los parametros

```
https:// #https
datatracker.ietf.org #host
/doc/html/rfc2616 #path
#section-14.19  #
```




>[!question]- porque hay que esperar a la respuesta para hacer el siguiente pedido? 
>Si se le mandan muchos 


# Idempotencia
que no cambia la respuesta entre la primera y la n vez

- GET
- PUT
- DELETE

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [1. Protos - introducción](1.%20Protos%20-%20introducción.md) — teoría de esta práctica

**Otras materias**

- **SO**  [Entorno de desarrollo](Entorno%20de%20desarrollo.md) — setup de entorno y trucos de bash

<!-- notas-relacionadas:fin -->
