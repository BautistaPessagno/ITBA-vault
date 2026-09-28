---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Colecciones
  - Java Collections Framework
  - List
  - Set
  - Map
  - HashMap
  - TreeMap
---
# POO — Colecciones en Java

## Resumen

### Java Collections Framework
Conjunto de interfaces, clases y métodos para manipular colecciones abstrayendo la implementación.

### Guía rápida — ¿Qué interfaz elegir?

| Orden | Repetidos | Interfaz | Implementación | Sobreescribir |
|---|---|---|---|---|
| Inserción | Sí | `Collection` / `SequencedCollection` | `ArrayList` | `equals` (si usa `contains`) |
| No importa | NO | `Set` | `HashSet` | `equals` + `hashCode` |
| Sí (natural) | NO | `SortedSet` | `TreeSet` | `Comparable` o `Comparator` |
| No (en claves) | NO (en claves) | `Map` | `HashMap` | `equals` + `hashCode` |
| Sí (en claves) | NO (en claves) | `SortedMap` | `TreeMap` | `Comparable` o `Comparator` |

### Interfaz Collection
Extiende `Iterable<>`. Métodos: `add`, `remove`, `contains`, `size`, `isEmpty`, `iterator`.

### List
Colección ordenada por inserción, con repetidos.
- `ArrayList` → arreglo redimensionable. Mejor en casi todas las operaciones.
- `LinkedList` → lista doblemente encadenada.

```java
List<String> lista = new ArrayList<>();
lista.add("hola");
lista.get(0);  // "hola"
```

### Set
Colección **sin repetidos**.
- `HashSet` → tabla hash; rápido `contains` — requiere `equals` + `hashCode`.
- `TreeSet` → árbol Red-Black ordenado — requiere `Comparable` o `Comparator`.
- `EnumSet` → optimizado para elementos Enum.

### Map
Pares clave-valor. **No extiende** `Iterable`.
- `HashMap` → tabla hash en claves; `HashSet` internamente usa `HashMap`.
- `TreeMap` → árbol ordenado por clave.

```java
Map<Integer, String> m = new HashMap<>();
m.put(1, "uno");
String v = m.get(1);         // "uno"
m.getOrDefault(99, "N/A");   // "N/A"
m.putIfAbsent(1, "dos");     // no cambia porque ya existe la clave 1
m.merge(1, "extra", String::concat);
```

### Deque
Extiende Queue agregando `push`/`pop` (se comporta como Stack en un extremo). Implementación: `LinkedList`.

### Factory Methods (inmutables)
```java
List.of(1, 2, 3)
Set.of("a", "b")
Map.of("key", "value")
Map.ofEntries(Map.entry("k1", "v1"), Map.entry("k2", "v2"))
```

### Herencia de Colecciones
En POO/aprendizaje se extiende la clase concreta cuando la composición repetiría código. En producción → preferir **composición**.

## Notas
- `contains()` en `HashSet`: usa `hashCode()` primero → muy rápido; luego `equals()`.
- **No mutar** un objeto mientras está en un `Set`/`Map` → puede corromper la estructura hash.
- `ArrayList` supera a `LinkedList` en casi todas las operaciones clásicas.

## Preguntas
- ¿Por qué `HashSet` es internamente un `HashMap`?
- ¿Cuándo usar `TreeSet` vs `HashSet`?
- ¿Por qué `Map` no extiende `Iterable`?

[POO](Categories/POO.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [POO - Interfaces y Generics](POO%20-%20Interfaces%20y%20Generics.md) — genéricos
- [POO - Clases Anidadas e Iterable](POO%20-%20Clases%20Anidadas%20e%20Iterable.md) — recorrer colecciones
- [POO - Colecciones Ruby](POO%20-%20Colecciones%20Ruby.md) — el equivalente en Ruby

**Otras materias**

- **EDA**  [EDA - Hashing](EDA%20-%20Hashing.md) — HashMap y HashSet
- **EDA**  [EDA - Listas Lineales](EDA%20-%20Listas%20Lineales.md) — List
- **EDA**  [EDA - Árboles](EDA%20-%20Árboles.md) — TreeMap y TreeSet

<!-- notas-relacionadas:fin -->
