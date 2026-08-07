---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Árboles
  - BST
  - AVL
  - Árbol B
  - Red-Black Tree
---
# EDA — Árboles

## Resumen

### Árbol Binario
Estructura de datos formada por nodos. Cada nodo tiene: **datos**, **subárbol izquierdo** y **subárbol derecho**. Existe un nodo distinguido: **raíz**.

### BST (Binary Search Tree)
Árbol binario donde para cada nodo: todos los elementos del subárbol izquierdo son menores, y todos los del derecho son mayores.  
**Peor caso:** si se insertan elementos ordenados → el árbol degenera en una lista → O(n) para búsqueda.

### Árbol AVL (Balanceado)
BST donde la **diferencia de alturas** entre los subárboles de cada nodo es **a lo sumo 1**.  
→ Garantiza búsqueda, inserción y borrado en **O(log n)** siempre.

Para mantener el balance se realizan **rotaciones** al insertar/borrar.

**Peor caso del AVL (árbol de Fibonacci):**
- AVL de altura h tiene: 1 nodo + AVL(h-1) nodos + AVL(h-2) nodos.
- La cantidad mínima de nodos es Fibonacci(h+3) - 1.

### Árbol M-ario
Cada nodo almacena hasta **M-1 claves** y tiene hasta **M hijos**.  
La clave Cᵢ es mayor que todas las claves de su subárbol izquierdo y menor que todas las de su subárbol derecho.

### Árbol B de Orden N
Árbol M-ario que cumple:
1. Cada nodo tiene a lo sumo **2·N claves**.
2. Cada nodo (excepto la raíz) tiene al menos **N claves**.
3. Cada nodo no-hoja tiene M+1 descendientes (M = número de claves del nodo).
4. **Todas las hojas están al mismo nivel**.

**Búsqueda en Árbol B:** recorrer secuencialmente las claves del nodo y bajar al subárbol correspondiente.

**Inserción:** siempre se inserta en una hoja; si el nodo supera 2·N claves → se divide, la clave del medio sube al padre (proceso recursivo hasta la raíz).

### Red-Black Tree
Variante de BST balanceado con reglas de coloración (rojo/negro). Java `TreeMap`/`TreeSet` usan Red-Black Trees internamente.

## Notas
- AVL es más estrictamente balanceado que Red-Black Tree → mejor para búsquedas; Red-Black es mejor para inserciones frecuentes.
- Los Árboles B son la estructura estándar en bases de datos (índices de disco).

## Preguntas
- ¿Por qué el BST puede degenerar en una lista y qué soluciona AVL?
- ¿Cuándo conviene un Árbol B sobre un AVL?
- ¿Cuál es la complejidad de búsqueda en un Árbol B?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [[EDA - Grafos]] — un árbol es un grafo acíclico conexo

**Otras materias**

- **BD**  [[BD clase 16 programacion embebida]] — índices B-tree
- **Discrete Math**  [[Discrete Math - Árboles y Recorridos]] — la teoría de árboles
- **Protos**  [[3. Protos - DNS]] — el espacio de nombres es jerárquico
- **SO**  [[File System]] — el FS como árbol de directorios
- **TLA**  [[TLA -Análisis Sintáctico]] — árbol de derivación

<!-- notas-relacionadas:fin -->
