---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Stack
  - Pila
  - LIFO
  - Infija-Postfija
---
# EDA — Stack (Pila)

## Resumen

### Stack / Pila / LIFO
Colección de datos ordenada por **orden de llegada**. Solo se accede por el **TOPE** (último elemento en llegar).

**Operaciones:**
| Operación | Descripción |
| --- | --- |
| `push(e)` | Agrega elemento → pasa a ser el tope |
| `pop()` | Quita el tope (destructiva; error si vacía) |
| `peek()` | Lee el tope sin removerlo (error si vacía) |
| `isEmpty()` | Retorna true si no hay elementos |

**Casos de uso:**
- **Undo/Redo** en editores de texto.
- **Runtime call stack**: cada llamada a un método genera un stack frame (parámetros, locales, dirección de retorno).
- Simular recursión con una pila explícita cuando el lenguaje no soporta recursión.

### Implementaciones
**Con arreglo:** contigüidad garantizada, operaciones solo en el tope → no hay huecos internos. Si se agota el espacio, crecer/decrecer de a *chunks*.

**Con lista lineal simplemente encadenada:** preferida. El tope apunta al primer nodo; push/pop nunca requieren recorrer la lista.

Java tiene `java.util.Stack` en la biblioteca estándar.

### Algoritmo Infija → Postfija
Convierte una expresión infija (a+b) a postfija (ab+) usando un Stack para manejar precedencia de operadores.

**Reglas:**
1. Cada **operando** se copia directo a la salida.
2. Cada **operador** se compara con el tope de la pila:
   - Si la pila está vacía → `push` del operador.
   - Si el tope tiene **mayor precedencia** → `pop` hasta encontrar uno de menor precedencia o pila vacía, luego `push` del operador actual.
   - Si el tope tiene **menor precedencia** → `push` del operador actual.
3. Al terminar la expresión: `pop` todos los operadores restantes a la salida.

## Notas
- La lista simplemente encadenada es más eficiente que el arreglo para Stack porque evita realocaciones.
- El runtime stack del procesador funciona igual que este TAD: `call` ≡ `push PC`, `ret` ≡ `pop PC`.

## Preguntas
- ¿Por qué `peek()` y `pop()` solo pueden usarse si la pila no está vacía?
- ¿Cuál es la complejidad de push y pop en ambas implementaciones?
- ¿Cómo se evalúa una expresión postfija con una pila?

[Data Structures and Algorithms](Categories/Data%20Structures%20and%20Algorithms.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [EDA - Queue (Cola)](EDA%20-%20Queue%20%28Cola%29.md) — la otra estructura LIFO/FIFO
- [EDA - Listas Lineales](EDA%20-%20Listas%20Lineales.md) — implementación subyacente

**Otras materias**

- **Arqui**  [Assembler de Intel](Assembler%20de%20Intel.md) — push y pop a nivel máquina
- **Arqui**  [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — el stack frame real en memoria
- **PI**  [PI - Recursividad en C](PI%20-%20Recursividad%20en%20C.md) — la pila de llamadas
- **TLA**  [TLA -Autómatas de Pila](TLA%20-Autómatas%20de%20Pila.md) — la pila como modelo de cómputo

<!-- notas-relacionadas:fin -->
