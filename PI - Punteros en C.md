---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Punteros
  - Memoria
  - Arreglos
  - swap
---
# PI — Punteros en C

## Resumen

### ¿Qué es un puntero?
Variable que almacena la **dirección de memoria** de otra variable.  
Alternativa a usar arreglos para devolver múltiples valores.

```c
tipoApuntado * nombrePuntero;

char * pChar;
int  * intPunt;
double * pDouble;
```

### Operadores
- **`&` (dirección):** aplicado a un l-value, retorna su dirección de memoria.
- **`*` (desreferencia):** aplicado a un puntero, retorna el valor apuntado.

```c
int a = 10;
int * p = &a;   // p apunta a 'a'
*p = 20;        // cambia el valor de 'a' a 20 mediante el puntero
```

**NULL:** constante simbólica (definida en `stdio.h`) que indica que el puntero no apunta a nada válido. Siempre inicializar los punteros a NULL.

### Punteros como Parámetros
Permiten que una función **modifique la variable original** del llamador (simular pasaje por referencia):

```c
void swap(int * p1, int * p2) {
    int aux = *p1;
    *p1 = *p2;
    *p2 = aux;
}

int main(void) {
    int a = 10, b = 3;
    swap(&a, &b);   // pasa direcciones
    // ahora a=3, b=10
}
```

### Arreglos y Punteros
En C, el nombre de un arreglo **es** un puntero al primer elemento:
```c
int v[5] = {1, 2, 3, 4, 5};
int * p = v;       // p apunta a v[0]
*(p + 1) == v[1];  // aritmética de punteros
```

**Aritmética de punteros:** `p + n` avanza n posiciones del tipo apuntado (no n bytes).

### Operaciones de Comparación entre Punteros
Se pueden comparar punteros del mismo tipo con `==`, `!=`, `<`, `>`.  
Útil para recorrer arreglos.

## Notas
- `*` en la declaración → indica que es puntero. `*` en expresión → desreferencia.
- Un puntero mal inicializado (puntero "salvaje") puede causar crashes o comportamiento indefinido.
- Para punteros a estructuras usar la flecha: `pStruct->campo`.

## Preguntas
- ¿Cuál es la diferencia entre `*p` y `&p`?
- ¿Por qué `swap(a, b)` no funciona sin punteros?
- ¿Qué significa `NULL` y por qué es importante inicializar punteros a NULL?

[[PI.base|PI]]
