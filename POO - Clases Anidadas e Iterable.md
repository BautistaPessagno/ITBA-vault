---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Clases Anidadas
  - Inner Class
  - Static Nested Class
  - Iterable
  - Iterator
  - For-each
---
# POO — Clases Anidadas e Iterable en Java

## Resumen

### Clases Anidadas
Una clase definida **dentro de otra clase**. Uso: cuando la clase solo tiene sentido en el contexto de la clase contenedora.

**Ventajas:**
- Agrupa lógicamente clases usadas en un solo lugar.
- Mayor encapsulamiento (puede ser `private`).
- Código más legible (la clase queda cerca de su uso).

**Regla:** un archivo `.java` debe tener solo una clase pública con el mismo nombre que el archivo.

#### Inner Class (No estática)
- Puede ser `private` o pública.
- Para instanciarla desde afuera → se necesita una instancia de la outer class.
- Tiene acceso a los campos y métodos de instancia de la outer class.

```java
public class OuterClass {
    private int data;

    public class InnerClass {
        public void doSomething() {
            System.out.println(data); // accede a la outer class
        }
    }
}
// Instanciar desde fuera:
OuterClass outer = new OuterClass();
OuterClass.InnerClass inner = outer.new InnerClass();
```

#### Static Nested Class
- No necesita instancia de la outer class.
- **No** puede acceder a variables/métodos de instancia de la outer class.
- Se instancia directamente: `new OuterClass.StaticNested()`.

```java
public class Lista<E> {
    // Nodo es static nested porque no necesita acceder al estado de Lista
    private static class Nodo<E> {
        E dato;
        Nodo<E> siguiente;
    }
}
```

#### Con Generics
- La **inner class** hereda el tipo genérico de la outer class.
- La **static nested class** debe declarar su propio tipo genérico.

---

### Iterator e Iterable
Permiten recorrer colecciones sin exponer la implementación interna.

#### Iterator\<E\>
```java
interface Iterator<E> {
    boolean hasNext();  // ¿hay más elementos?
    E next();           // retorna el siguiente y avanza
}
```

#### Iterable\<E\>
Una clase que implementa `Iterable<E>` puede usarse en el **for-each**:

```java
interface Iterable<E> {
    Iterator<E> iterator();  // retorna un Iterator
}

// Habilita el for-each:
for (String s : miColeccion) { ... }
// El compilador lo traduce a:
Iterator<String> it = miColeccion.iterator();
while (it.hasNext()) { String s = it.next(); ... }
```

**Patrón común:** la inner class del iterador accede al estado privado de la colección (arreglo, cabeza de lista, etc.).

## Notas
- Las **lambda** son la evolución de las clases anónimas: misma funcionalidad, menos código.
- La inner class del Iterator usa `static nested class` si no necesita acceder al estado mutable de la colección; usa inner class si sí lo necesita.
- `remove()` en `Iterator` es opcional; lanzar `UnsupportedOperationException` si no implementado.

## Preguntas
- ¿Cuándo usar inner class vs static nested class?
- ¿Por qué la clase Iterator suele ser inner?
- ¿Qué debe implementar una clase para poder usarse en el for-each?

[[POO.base|POO]]
