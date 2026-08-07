---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Enums
  - Java
  - Constantes
  - Polimorfismo
---
# POO — Enums en Java

## Resumen

### ¿Qué es un Enum?
Un Enum es un tipo especial de clase cuyos objetos son un conjunto **fijo y predefinido de instancias**. En su forma más simple, actúa como contenedor de constantes (similar a `enum` en C).

```java
public enum Rating {
    SUCCESS, GOOD, REGULAR, BAD
}
// Uso:
Rating r = Rating.GOOD;
```

### Enums con Variables de Instancia
Cada constante puede tener sus propios datos y comportamiento:

```java
public enum Rating {
    SUCCESS("Éxito", 10),
    GOOD("Bien", 7),
    REGULAR("Regular", 5),
    BAD("Mal", 2);

    private final String description;
    private final int value;

    Rating(String description, int value) {
        this.description = description;
        this.value = value;
    }

    @Override
    public String toString() { return description; }

    public int intValue() { return value; }
}
```

### Enums con Métodos Abstractos
Cada constante puede sobreescribir el comportamiento:

```java
public enum Operation {
    ADD("+") {
        @Override
        public double apply(double op1, double op2) { return op1 + op2; }
    },
    SUBTRACT("-") { @Override public double apply(...) { return op1 - op2; } };

    private final String symbol;
    Operation(String symbol) { this.symbol = symbol; }

    public abstract double apply(double op1, double op2);
}
```
→ Reemplaza el `switch` imperativo por polimorfismo (más elegante y mantenible).

### Métodos Comunes
| Método | Descripción |
|---|---|
| `values()` | Array con todas las constantes del enum |
| `ordinal()` | Posición de la constante (0-based) en el orden de declaración |
| `name()` | Nombre de la constante como String |
| `valueOf(String)` | Retorna la constante con ese nombre |

### EnumSet / EnumMap
Colecciones optimizadas para Enums — más eficientes que `HashSet`/`HashMap` para este caso.

## Notas
- Los Enums en Java son **clases** con instancias fijas; pueden implementar interfaces.
- Preferir polimorfismo con métodos abstractos sobre `switch` con Enums.
- Las constantes de Enum son `public static final` implícitamente.

## Preguntas
- ¿Por qué es mejor usar un método abstracto en un Enum que un `switch`?
- ¿Qué retorna `ordinal()` y cuándo es útil?
- ¿Puede un Enum extender una clase? ¿Puede implementar interfaces?

[[POO.base|POO]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [[POO - Introduccion a Java]] — tipos en Java
- [[POO - Introducción a POO]] — clases y constantes

**Otras materias**

- **PI**  [[PI - Struct y Union en C]] — enum en C

<!-- notas-relacionadas:fin -->
