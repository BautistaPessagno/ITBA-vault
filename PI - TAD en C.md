---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - TAD
  - Tipos Abstractos de Datos
  - Encapsulamiento
  - Modularización
---
# PI — TAD (Tipos Abstractos de Datos) en C

## Resumen

### ¿Qué es un TAD?
Técnica de diseño que enfatiza módulos con:
- **Semántica clara** (el módulo hace una sola cosa bien).
- **Alta cohesión** (sus elementos están relacionados entre sí).
- **Bajo acoplamiento** (depende lo mínimo posible de otros módulos).

Es el paso previo a la POO. En C no hay construcciones sintácticas propias para TAD, pero se puede implementar con el convencio de archivos `.h` y `.c`.

### Ocultamiento de Información (Hiding)
- Se ocultan los detalles de implementación.
- Se define un **contrato** (interfaz pública) con las únicas operaciones disponibles.
- Funciona como una **caja negra** para el usuario.

### Estructura de un TAD en C

**Archivo de encabezado (`.h`) → interfaz pública:**
```c
/* stack.h - contrato del TAD Stack */
typedef struct TStackCDT * TStackADT;

/* Operaciones disponibles */
TStackADT  createStack(void);
void       push(TStackADT stack, int value);
int        pop(TStackADT stack);
int        peek(TStackADT stack);
int        isEmpty(TStackADT stack);
void       destroyStack(TStackADT stack);
```

**Archivo de implementación (`.c`) → oculto al usuario:**
```c
/* stack.c - implementación privada */
#include "stack.h"

struct TStackCDT {
    /* detalles de implementación ocultos */
    int data[MAX_SIZE];
    int top;
};

TStackADT createStack(void) { ... }
void push(TStackADT stack, int value) { ... }
```

### Opaque Pointer (CDT/ADT)
Técnica para ocultar completamente la estructura interna:
- En el `.h` se declara el struct como tipo incompleto: `struct TStackCDT`.
- El usuario solo conoce el puntero (`TStackADT`) pero no la estructura interna.
- La implementación está completamente en el `.c`.

### Ventajas del TAD
- **Portabilidad:** se puede cambiar la implementación sin afectar al usuario.
- **Mantenibilidad:** el usuario no depende de detalles internos.
- **Reusabilidad:** el TAD puede usarse en distintos programas.

## Notas
- En POO el TAD se convierte en una **clase** con atributos privados y métodos públicos.
- El nombre CDT = Concrete Data Type (la implementación); ADT = Abstract Data Type (la interfaz).
- Siempre hacer `destroy`/`free` del TAD al terminar de usarlo.

## Preguntas
- ¿Qué diferencia hay entre CDT y ADT?
- ¿Por qué el usuario no debería acceder directamente a los campos del struct?
- ¿Cómo se relaciona el TAD con las clases en POO?

[[PI.base|PI]]
