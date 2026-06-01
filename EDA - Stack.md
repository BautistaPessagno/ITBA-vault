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

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]
