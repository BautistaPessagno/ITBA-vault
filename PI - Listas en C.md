---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Listas
  - Punteros
  - TAD
  - Estructuras Dinámicas
---
# PI — Listas en C

## Resumen

### Definición de Lista
Colección de datos **homogénea** y ordenada cuyos elementos se acceden iterando.  
Definición recursiva: una lista está **vacía** o tiene una **cabeza** seguida de una **cola** (que también es una lista).

```
Lista = ∅  |  [cabeza | cola]
```

### Características
1. Cada elemento (excepto el último) tiene un único sucesor.
2. No hay contigüidad física; cada nodo apunta al siguiente.
3. La definición recursiva facilita algoritmos recursivos.
4. Pueden ser **ordenadas** o **desordenadas** desde el punto de vista del usuario.

### Implementación en C (Lista Enlazada)
```c
typedef struct TNode {
    int elem;
    struct TNode * tail;
} TNode;

typedef TNode * TList;
```

**Operaciones:**
```c
TList createList(void);            // lista vacía = NULL
TList insertFirst(TList l, int e); // inserta al frente
int   head(TList l);               // retorna la cabeza
TList tail(TList l);               // retorna la cola
int   isEmpty(TList l);            // 1 si vacía
```

### Concatenación de Listas
Para concatenar dos listas correctamente se crea una **copia** de los nodos:
```c
TList concat(TList l1, TList l2) {
    if (isEmpty(l1)) return l2;  // caso base
    TNode * aux = malloc(sizeof(TNode));
    aux->elem = head(l1);
    aux->tail = concat(tail(l1), l2);
    return aux;
}
// INCORRECTO: hacer l1.ultimo->tail = l2 (modifica la estructura original)
```

### Cuándo Usar Lista vs Arreglo
| Operación | Lista | Arreglo |
|---|---|---|
| Insertar/borrar al frente | O(1) | O(n) |
| Acceso por índice | O(n) | O(1) |
| Búsqueda por clave | O(n) | O(log n) con binaria |
| Menor uso de memoria (sin tamaño fijo) | ✓ | ✗ |
| Eficiencia general | Buena si inserciones frecuentes | Mejor para acceso aleatorio |

**Regla:** si necesitas `get(i)` eficiente → arreglo; si inserción/borrado frecuente → lista.

## Notas
- La lista se implementa con `struct` y punteros.
- Siempre liberar memoria (`free`) al borrar nodos para evitar memory leaks.
- La lista es la base de Stack, Queue y otras estructuras dinámicas.

## Preguntas
- ¿Por qué no se puede concatenar haciendo `tail` de l1 apuntar a l2?
- ¿Cómo se accede al n-ésimo elemento de una lista?
- ¿Cuándo usar arreglo vs lista para implementar una cola?

[[PI.base|PI]]
