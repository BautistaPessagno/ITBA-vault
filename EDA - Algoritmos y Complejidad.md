---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Complejidad
  - Big-O
  - TDD
  - JUnit
  - Maven
---
# EDA — Algoritmos y Complejidad

## Resumen

### Complejidad Computacional
Métrica que caracteriza los recursos (tiempo/espacio) que consume un algoritmo, independiente del hardware.

**Análisis temporal — Big-O:**  
Sea T(N) el tiempo de ejecución. T(N) es O(g(N)) si ∃ c > 0 y n₀ > 0 tal que ∀ N ≥ n₀: T(N) ≤ c·g(N).

| Complejidad | Clase |
|---|---|
| O(1) | Constante |
| O(log n) | Logarítmica |
| O(√n) | Raíz cuadrada |
| O(n) | Lineal |
| O(n log n) | Lineal-logarítmica |
| O(n²) | Cuadrática |
| O(2ⁿ) | Exponencial |

Siempre analizar el **peor caso** salvo que se indique lo contrario.  
Para calcular O(N): sumar operaciones primitivas (comparaciones, aritmética, transferencia de control) y quedarse con el término dominante.

**Complejidad espacial:** cota O del espacio RAM extra (heap + stack frames). Cada invocación genera un *stack frame* (parámetros, locales, dirección de retorno).

### Maven
Herramienta de construcción y gestión de proyectos Java. Gestiona dependencias, compilación, tests y empaquetado.

**Fases del ciclo de vida:**
```
compile → test → package → install
```
Cada fase ejecuta todas las anteriores.

**pom.xml mínimo:**
```xml
<groupId>ar.edu.itba</groupId>
<artifactId>MiProyecto</artifactId>
<version>1.0</version>
<properties>
  <maven.compiler.source>21</maven.compiler.source>
  <maven.compiler.target>21</maven.compiler.target>
</properties>
```

### TDD — Test-Driven Development
Metodología: primero escribir los tests, luego implementar para que pasen.

**Características de un buen unit test:** automático, verifica un único caso, repetible, independiente, rápido.  
Cubrir: casos típicos, de borde, de error/excepción.

### JUnit 5
Framework de testing para Java.

```java
// Assertions básicas
Assertions.assertEquals(esperado, obtenido);
Assertions.assertTrue(valor);
Assertions.assertThrows(RuntimeException.class, () -> ...);

// Ciclo de vida
@BeforeEach  // se ejecuta antes de cada @Test
@AfterEach   // se ejecuta después de cada @Test
@BeforeAll / @AfterAll  // una sola vez por clase
```

**pom.xml (dependencias JUnit):**
```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-engine</artifactId>
  <version>5.12.0</version>
  <scope>test</scope>
</dependency>
```

## Notas
- Las asignaciones se ignoran en el conteo de operaciones primitivas.
- El `GC` libera el heap; cada `new` aloca en heap.
- Configurar JVM: `-Xms512m -Xmx4G` (heap), `-Xss2048k` (stack).
- Una clase es **testeable** si sus métodos no tienen efectos secundarios ni dependencias externas.

## Preguntas
- ¿Cuándo conviene usar complejidad espacial vs temporal como criterio de decisión?
- ¿Por qué `mvn package` siempre compila antes de empaquetar?
- ¿Cuál es la diferencia entre `@BeforeAll` y `@BeforeEach`?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]
