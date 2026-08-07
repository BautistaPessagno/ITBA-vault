---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - C
  - Tipos de datos
  - Include
  - Compilación
  - gcc
---
# PI — Introducción al Lenguaje C

## Resumen

### Características de C
- Pocas palabras reservadas; se complementa con **bibliotecas estándar**.
- Compilado: el código fuente (`.c`) se compila a código objeto (`.o`) y se enlaza.
- Tipado estático fuerte.

**Compilación con gcc:**
```bash
gcc -c archivo.c        # solo compilar → genera .o
gcc archivo.c -o salida # compilar y enlazar
gcc -E archivo.c        # solo preprocesamiento
```
Si gcc reporta error con `ld` → falló la **linkedición** (el código fuente compiló bien).

### Tipos de Datos
**Enteros:**
```c
signed char     // [-128, 127]       1 byte
unsigned char   // [0, 255]          1 byte
short           // 16 bits
int             // signed por defecto
unsigned int
long int        // 32 bits
long long int   // 64 bits
```
`limits.h` → define el máximo de cada tipo entero.

**Reales:**
```c
float   // IEEE 754 simple precisión
double  // IEEE 754 doble precisión
```
`float.h` → máximos de tipos reales.

**Valores de verdad:**
```c
0  // falso
1  // verdadero (cualquier valor != 0)
```

### Include
```c
#include <stdio.h>      // biblioteca estándar (busca en rutas del sistema)
#include "miarchivo.h"  // archivo propio (busca en directorio local)
```

**Convenciones de archivos:**
- `.c` → código fuente
- `.h` → archivo de encabezado (declaraciones, prototipos)
- `README` → descripción del contenido

### Retorno de main
```c
int main(void) {
    // ...
    return 0;   // 0 = éxito
                // != 0 = error
}
```

### Comentarios
```c
/* comentario de bloque */
// comentario de línea (C99+)
```

Comentarios deben describir **qué** hace el código, no **cómo**.

### Tips de compilación
- `'u'` en C → valor numérico ASCII de `u`.
- Usar `-c` para separar errores de compilación de los de linkedición.

## Notas
- El tipo `char` es un entero de 1 byte; siempre especificar `signed` o `unsigned` para claridad.
- `limits.h` es indispensable para código portable.

## Preguntas
- ¿Cuál es la diferencia entre `#include <...>` y `#include "..."`?
- ¿Qué significa el código de retorno de `main`?
- ¿Por qué el flag `-c` es útil para diagnosticar errores?

[[PI.base|PI]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PI)**

- [[PI - Funciones en C]] — tema siguiente
- [[PI - Arreglos en C]] — tipos y arreglos

**Otras materias**

- **Arqui**  [[Clase 3 ASM y C]] — a qué compila el C
- **TLA**  [[frontend]] — un proyecto real en C

<!-- notas-relacionadas:fin -->
