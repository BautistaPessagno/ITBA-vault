---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Struct
  - Union
  - typedef
  - Registros
---
# PI — Struct y Union en C

## Resumen

### Struct (Registro)
**Arreglo:** datos homogéneos, acceso por índice.  
**Struct:** datos **heterogéneos** sin orden, acceso por nombre de campo.

```c
struct nombreRegistro {
    tipo1 campo1;
    tipo2 campo2;
    /* ... */
    tipoN campoN;
};
```

> La declaración de `struct` define un **nuevo tipo**, NO reserva memoria.

**Inicialización y acceso:**
```c
struct Fecha {
    int dia, mes, anio;
};

struct Fecha hoy = {1, 6, 2026};  // inicialización en declaración
hoy.dia = 15;                      // acceso y asignación por campo
```

**typedef** (abreviación del tipo):
```c
typedef struct {
    int dia, mes, anio;
} Fecha;

Fecha hoy = {1, 6, 2026};  // sin "struct" delante
```

### Reglas de Jerarquía
- Al anidar structs se crean jerarquías.
- Dos campos en la **misma jerarquía** → nombres distintos.
- Dos campos en **jerarquías distintas** → pueden coincidir en nombre.

### Struct como Parámetro
Se envía una **copia** al stack → modificaciones no afectan al original.  
Para modificar el original → pasar un **puntero al struct**.

```c
// Acceso con puntero a struct:
struct Fecha * p = &hoy;
p->dia = 20;  // equivale a (*p).dia = 20
```

### Bit Fields
Para campos enteros que necesitan pocos bits:
```c
typedef struct {
    unsigned int size:       6;  // 6 bits
    unsigned int bold:       1;  // 1 bit
    unsigned int italic:     1;
} Format;
```

### Almacenamiento en Memoria
Los campos se almacenan en forma **contigua** según el orden de declaración, pero puede haber **bytes de alineamiento** entre campos de distintos tipos.  
→ Ordenar campos del más grande al más pequeño para optimizar memoria.

### Union
Sintaxis idéntica a struct, pero sus elementos **NO pueden coexistir**: todos comparten la misma área de memoria. Solo un campo tiene valor válido a la vez.

```c
union tipoNro {
    int entero;
    float realSimple;
    double realDoble;
};
```

Uso: representar un valor que puede ser de distintos tipos según el contexto.

## Notas
- Las estructuras como unidad **no se pueden comparar** directamente (`==` no funciona). Comparar campo por campo.
- Para usar puntero a struct: `struct Fecha * p` → acceder con `p->campo`.

## Preguntas
- ¿Por qué `struct` no reserva memoria al declararse?
- ¿Cuándo conviene usar `union` sobre `struct`?
- ¿Por qué ordenar los campos del más grande al más pequeño ahorra memoria?

[PI](Categories/PI.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PI)**

- [PI - TAD en C](PI%20-%20TAD%20en%20C.md) — structs como base de un TAD
- [PI - Punteros en C](PI%20-%20Punteros%20en%20C.md) — punteros a struct

**Otras materias**

- **BD**  [BD clase 16 programacion embebida](BD%20clase%2016%20programacion%20embebida.md) — variables host en SQL embebido
- **POO**  [POO - Enums](POO%20-%20Enums.md) — enums y tipos con valores fijos
- **Protos**  [10. Protos - Sockets](10.%20Protos%20-%20Sockets.md) — sockaddr y structs de la API

<!-- notas-relacionadas:fin -->
