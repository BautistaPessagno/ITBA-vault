---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Recursividad
  - Stack
  - Caso Base
  - Wrapper
---
# PI — Recursividad en C

## Resumen

### Recursividad
Una función es recursiva cuando se llama a sí misma para resolver una versión más pequeña del mismo problema.

**Dos partes esenciales:**
1. **Caso base:** condición de parada. Sin él → recursión infinita → stack overflow.
2. **Caso recursivo:** llamada a la propia función con un subproblema más pequeño.

```c
int factorial(int n) {
    if (n == 0) return 1;      // caso base
    return n * factorial(n-1); // caso recursivo
}
```

### Cómo Funciona en Memoria
Cada llamada recursiva genera un nuevo **stack frame** en el call stack del runtime con:
- Parámetros formales (copia del valor).
- Variables locales.
- Dirección de retorno.

Las llamadas se apilan hasta llegar al caso base, luego se desapilan resolviendo.

### Wrapper de Validación
Si se necesita validar parámetros antes de la recursión, se usa una función *wrapper*:

```c
// Función wrapper (valida, luego llama a la recursiva)
int potencia(int base, int exp) {
    if (exp < 0) {
        fprintf(stderr, "Error: exponente negativo\n");
        return -1;
    }
    return potencia_rec(base, exp);
}

// Función recursiva pura
static int potencia_rec(int base, int exp) {
    if (exp == 0) return 1;
    return base * potencia_rec(base, exp - 1);
}
```

### Recursividad vs Iteración
| Aspecto | Recursivo | Iterativo |
|---|---|---|
| Legibilidad | Más claro para problemas recursivos por naturaleza | Más claro para loops simples |
| Rendimiento | Más stack frames → overhead | Sin overhead de llamada |
| Riesgo | Stack overflow para N grande | Sin riesgo de stack overflow |

### Cuándo usar recursión
- Estructuras naturalmente recursivas: árboles, grafos.
- Algoritmos como DFS, divide y conquista.
- Cuando la solución iterativa sería mucho más compleja.

## Notas
- Comparado con la cola (BFS iterativa), la recursión implícitamente usa una pila (DFS).
- `static` en la función recursiva interna la limita al archivo → buena práctica de encapsulamiento.

## Preguntas
- ¿Qué es un stack overflow y cómo lo provoca la recursión?
- ¿Para qué sirve un wrapper en una función recursiva?
- ¿En qué casos es mejor usar iteración sobre recursión?

[PI](Categories/PI.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PI)**

- [PI - Funciones en C](PI%20-%20Funciones%20en%20C.md) — llamadas a función

**Otras materias**

- **Arqui**  [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — stack frames de la recursión
- **EDA**  [EDA - Stack](EDA%20-%20Stack.md) — la pila de llamadas
- **EDA**  [EDA - Tipos de Algoritmos y Heurísticas](EDA%20-%20Tipos%20de%20Algoritmos%20y%20Heurísticas.md) — divide y conquista, backtracking
- **TLA**  [TLA -Análisis Sintáctico](TLA%20-Análisis%20Sintáctico.md) — descenso recursivo

<!-- notas-relacionadas:fin -->
