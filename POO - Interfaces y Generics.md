---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Interfaces
  - Generics
  - Comparable
  - Comparator
  - Wildcard
  - Funciones Lambda
---
# POO — Interfaces y Generics en Java

## Resumen

### Interfaces
Tipo de referencia en Java: colección de métodos abstractos y constantes.

**Características:**
- No se pueden instanciar; no tienen constructor ni variables de instancia.
- Una clase puede **implementar** múltiples interfaces (`implements A, B`).
- Una interfaz puede **extender** múltiples interfaces.
- Las constantes son implícitamente `public static final`.
- Pueden tener métodos `default` (con implementación) y `static`.

```java
public interface Printable {
    void print(); // abstracto
    default String format() { return "default"; } // default
}
```

**Conflicto de default methods:**
1. Clase vs interfaz → gana la clase.
2. Subinterfaz vs superinterfaz → gana la subinterfaz.
3. Dos interfaces independientes con el mismo default → la clase **debe** sobreescribir.

### Generics
Permiten parametrizar tipos para evitar casteos y errores en tiempo de ejecución.  
Implementados con **Erasure**: el tipo parámetro se reemplaza por su bound (`Object` si no hay).

```java
public class Caja<E extends Comparable<E>> { ... }
```

**Wildcards (`?`):** cuando no importa el tipo exacto:
```java
void imprimir(List<?> lista) { ... }         // cualquier tipo
void sumar(List<? extends Number> lista) { } // subtipos de Number
```

**Métodos parametrizados** (cuando el tipo de retorno es genérico):
```java
<T extends Comparable<? super T>> T max(T a, T b) { ... }
```

### Comparable\<T\>
La clase indica que sus instancias tienen un **orden natural**. Requerida para `Arrays.sort`, colecciones ordenadas, etc.

```java
@Override
public int compareTo(MiClase o) {
    int r = Integer.compare(campo1, o.campo1);
    if (r == 0) r = Integer.compare(campo2, o.campo2);
    return r;
}
```
**Propiedades:** antisimetría, transitividad, consistencia con `equals`.  
Patrón: `Pair<T extends Comparable<? super T>>` para permitir comparación con ancestros.

### Comparator\<T\>
Criterio de ordenación externo (no modifica la clase):
```java
Arrays.sort(arr, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return Integer.compare(a.length(), b.length());
    }
});

// Equivalente con lambda:
Arrays.sort(arr, (a, b) -> Integer.compare(a.length(), b.length()));
```

### Interfaces Funcionales y Lambdas
Interfaz con un único método abstracto → puede usarse como **lambda**:
```java
// Interfaz funcional
Function<Double, Double> cuadrado = x -> x * x;

// Clase anónima equivalente
Function<Double, Double> cuadrado = new Function<Double, Double>() {
    public Double apply(Double x) { return x * x; }
};
```

## Notas
- No usar interfaces solo para almacenar constantes (anti-patrón).
- En Ruby el equivalente de `Comparable` es el operador `<=>` (spaceship).

## Preguntas
- ¿Por qué no se puede instanciar una interfaz?
- ¿Cuándo usar `Comparator` vs `Comparable`?
- ¿Qué es la técnica de Erasure y qué limitación impone con arrays?

[POO](Categories/POO.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [POO - Introducción a POO](POO%20-%20Introducción%20a%20POO.md) — polimorfismo
- [POO - Colecciones Java](POO%20-%20Colecciones%20Java.md) — genéricos en el JCF
- [POO - Clases Anidadas e Iterable](POO%20-%20Clases%20Anidadas%20e%20Iterable.md) — Iterable como interfaz

**Otras materias**

- **EDA**  [EDA - Estructuras Lineales y Ordenación](EDA%20-%20Estructuras%20Lineales%20y%20Ordenación.md) — Comparator para ordenar
- **PI**  [PI - TAD en C](PI%20-%20TAD%20en%20C.md) — separar interfaz de implementación

<!-- notas-relacionadas:fin -->
