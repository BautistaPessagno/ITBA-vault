---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Streams
  - Lambdas
  - Programación Funcional
  - Interfaces Funcionales
  - Optional
---
# POO — Streams y Lambdas en Java

## Resumen

### Imperativo vs Funcional
| Paradigma | Característica |
|---|---|
| Imperativo | Especifica los pasos; usa variables mutables; iteración externa (`for`) |
| Funcional | Especifica qué se quiere; inmutabilidad; iteración interna |

### Expresiones Lambda
Bloque de código que se pasa a un método para ejecutarse posteriormente.

```java
// Antes (clase anónima)
Arrays.sort(arr, new Comparator<String>() {
    public int compare(String a, String b) { return a.length() - b.length(); }
});

// Con lambda
Arrays.sort(arr, (a, b) -> a.length() - b.length());
```

**Alcance de variables:** una variable usada en un lambda debe ser `final` o *effectively final* (no se puede modificar después de asignada).

### Method Reference
Cuando el cuerpo del lambda es exactamente un método existente:
```java
lista.forEach(System.out::println);  // en vez de e -> System.out.println(e)
```

### Interfaces Funcionales (java.util.Function)
| Interfaz | Firma |
|---|---|
| `Supplier<T>` | `() → T` |
| `Consumer<T>` | `T → void` |
| `Predicate<T>` | `T → boolean` |
| `Function<T, R>` | `T → R` |
| `UnaryOperator<T>` | `T → T` |
| `BinaryOperator<T>` | `(T, T) → T` |
| `BiFunction<T, U, R>` | `(T, U) → R` |

### Optional\<T\>
Contenedor que puede tener un valor o ser nulo. Evita `NullPointerException`:
```java
Optional<String> opt = Optional.of("hola");
opt.isPresent();         // true
opt.get();               // "hola"
opt.orElse("default");   // "hola"
opt.orElseThrow();       // lanza NoSuchElementException si vacío
```
No usar como tipo de variable de instancia; reservar para valores de retorno opcionales.

### Streams API
Objetos que implementan `Stream<T>`. Permiten programación funcional sobre colecciones.

**Características:**
- No almacenan datos (opera sobre la colección original).
- No reutilizables.
- Las **operaciones intermedias** son *lazy* → no se ejecutan hasta una operación terminal.

```java
lista.stream()
     .filter(e -> e.length() > 3)    // intermedia
     .map(String::toUpperCase)        // intermedia
     .collect(Collectors.toList());   // terminal
```

**Tipos especializados:** `IntStream`, `DoubleStream`, `LongStream`.

## Notas
- La programación funcional con Streams **no es obligatoria** en parciales de POO/EDA en el ITBA.
- `Comparator.comparing(Person::getName).thenComparing(Person::getAge)` — encadenamiento de comparadores.

## Preguntas
- ¿Por qué las operaciones intermedias de un Stream son lazy?
- ¿Cuándo usar `Optional` vs simplemente retornar `null`?
- ¿Qué significa que una variable sea "effectively final"?

[[POO.base|POO]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [[POO - Colecciones Java]] — streams sobre colecciones
- [[POO - Clases Anidadas e Iterable]] — iteración externa vs interna
- [[POO - Interfaces y Generics]] — interfaces funcionales

**Otras materias**

- **BD**  [[BD clase 8 SQL consultas]] — estilo declarativo: filter/map vs WHERE/SELECT

<!-- notas-relacionadas:fin -->
