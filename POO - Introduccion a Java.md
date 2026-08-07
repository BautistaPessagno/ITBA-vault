---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Java
  - Tipos
  - Strings
  - Excepciones
  - Arrays
  - Autoboxing
---
# POO — Introducción a Java

## Resumen

### Java — Características
- Tipado estático; compilado a bytecode (.class) ejecutado en la JVM.
- No es OO puro: tiene tipos primitivos built-in (`int`, `long`, `double`, `boolean`, `char`…).
- Sintaxis similar a C. Sin herencia múltiple de clases.
- Manejo de errores mediante **excepciones**.

**Compilar y ejecutar:**
```bash
javac MiClase.java   # genera MiClase.class
java MiClase         # ejecuta con la JVM
```

### Tipos Primitivos vs Wrapper
| Primitivo | Wrapper |
|---|---|
| `int` | `Integer` |
| `long` | `Long` |
| `double` | `Double` |
| `boolean` | `Boolean` |
| `char` | `Character` |

**Autoboxing**: conversión automática primitivo → wrapper.  
**Unboxing**: conversión wrapper → primitivo.  
Preferir los tipos primitivos por eficiencia.

### Arreglos
```java
int[] v1 = new int[10];        // inicializa en 0
Integer[] v2 = new Integer[10]; // inicializa en null
System.out.println(v1.length);  // 10
System.out.println(Arrays.toString(v1)); // [0, 0, 0, ...]
```
Acceder fuera de rango → `ArrayIndexOutOfBoundsException`.

### Strings
Secuencia **inmutable** de caracteres. Cada concatenación con `+` crea un nuevo objeto.

```java
String s = s1 + ", " + s2;  // ineficiente en loops
```

Para construcción eficiente usar **StringBuilder**:
```java
StringBuilder sb = new StringBuilder();
sb.append(ch);  // no crea copias
String result = sb.toString();
```

**`==` vs `equals`:**
```java
Date d1 = new Date(2020, 11, 11);
Date d2 = new Date(2020, 11, 11);
d1 == d2;       // false (compara referencias)
d1.equals(d2);  // true (compara contenido si se sobreescribe)
```

**`instanceof` con pattern matching (Java 16+):**
```java
// Solo usar en el método equals
return o instanceof Date date && year == date.year;
```

### Excepciones
- **Checked Exception**: el compilador obliga a capturar o propagar (`catch or specify`).
- **Unchecked Exception** (`RuntimeException`): no es obligatorio capturar.

```java
try { ... }
catch (IOException e) { ... }
finally { ... }  // siempre se ejecuta

Assertions.assertThrows(RuntimeException.class, () -> metodo());
```

### hashCode
Requerido cuando se sobreescribe `equals`. Usado en colecciones basadas en hash.
```java
@Override
public int hashCode() {
    return Objects.hash(campo1, campo2, campo3);
}
```

### Modificadores de métodos
| Modificador | Significado |
|---|---|
| `static` | Método de clase (no requiere instancia) |
| `final` | No puede sobreescribirse |
| `abstract` | Sin implementación (solo en clase abstracta) |
| `synchronized` | Thread-safe |

## Notas
- `toString()` y `equals()` se definen en `Object`; siempre sobreescribir ambos juntos.
- Usar `String.formatted(...)` en lugar de concatenación para strings formateados.
- `System` es una clase con constructor privado → no se puede instanciar.
- Shortcut IntelliJ: `psvm` → `public static void main`, `sout` → `System.out.println`.

## Preguntas
- ¿Por qué `d1 == d2` es false aunque representen la misma fecha?
- ¿Cuándo debo implementar `hashCode`?
- ¿Cuál es la diferencia entre una Checked y una Unchecked exception?

[[POO.base|POO]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [[POO - Introducción a POO]] — tema anterior
- [[POO - Interfaces y Generics]] — tema siguiente
- [[POO - Colecciones Java]] — tipos y colecciones

**Otras materias**

- **BD**  [[BD clase 16 programacion embebida]] — JDBC
- **PI**  [[PI - Arreglos en C]] — arrays: C vs Java
- **SO**  [[Test Unitario-GitHub Workflow]] — JUnit

<!-- notas-relacionadas:fin -->
