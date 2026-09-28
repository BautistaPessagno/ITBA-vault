---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Arreglos
  - Matrices
  - Vectores
  - sizeof
---
# PI — Arreglos en C

## Resumen

### Arreglos (Vectores)
Colección de datos **homogénea y ordenada**. Almacenamiento contiguo en memoria.

```c
tipo nombre[cantidad];

int vec[5] = {5, 7, 9, 10, 2};  // inicialización completa
int vec[5] = {5, 7};             // resto se inicializa en 0
```

**`vec` es un rótulo/constante** que apunta al inicio del arreglo en memoria. No es una variable; no se puede reasignar.

**Acceso:** `arreglo[i]` (índice 0 a cantidad-1).  
C **no verifica** límites → acceder fuera del rango es comportamiento indefinido.

**Cantidad de elementos:**
```c
int n = sizeof(vec) / sizeof(vec[0]);
// sizeof(vec)    = bytes totales del arreglo
// sizeof(vec[0]) = bytes de un elemento
```

### Matrices (Arreglos Bidimensionales)
```c
int mat[3][4];           // 3 filas, 4 columnas

int mat[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};

mat[1][2] = 6;   // fila 1, columna 2
```

**Como parámetro de función:** siempre se debe especificar la cantidad de columnas (puede ser variable si está declarada antes):
```c
void identidad(int filas, int cols, int mx[][cols]) { ... }
```

### Arreglos como Parámetros
Al pasar un arreglo a una función, se pasa el **puntero al primer elemento** (no se copia el arreglo). Las modificaciones dentro de la función afectan al original.

```c
void inicializar(int v[], int n) {
    for (int i = 0; i < n; i++)
        v[i] = 0;  // modifica el arreglo original
}
```

## Notas
- `vec[i]` es equivalente a `*(vec + i)` (aritmética de punteros).
- Los arreglos en C se almacenan en el stack (si son locales) o en heap (si son dinámicos con `malloc`).

## Preguntas
- ¿Por qué C no detecta acceso fuera de los límites del arreglo?
- ¿Cómo obtener la cantidad de elementos de un arreglo con `sizeof`?
- ¿Qué ocurre cuando se pasa un arreglo a una función sin especificar tamaño?

[PI](Categories/PI.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PI)**

- [PI - Punteros en C](PI%20-%20Punteros%20en%20C.md) — arreglos y punteros
- [PI - Intro C](PI%20-%20Intro%20C.md) — tipos de datos

**Otras materias**

- **EDA**  [EDA - Estructuras Lineales y Ordenación](EDA%20-%20Estructuras%20Lineales%20y%20Ordenación.md) — arreglos como estructura lineal
- **POO**  [POO - Introduccion a Java](POO%20-%20Introduccion%20a%20Java.md) — arrays en Java

<!-- notas-relacionadas:fin -->
