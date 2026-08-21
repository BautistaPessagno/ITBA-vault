---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - Servlet
  - web.xml
  - JAR
  - WAR
  - Reflection
  - Spring
  - Dependency Injection
Created: 2026-08-0319:02
Materia: "[[PAW.base|PAW]]"
temas:
  - Application Container
  - Servlet
  - web.xml
  - JAR
  - WAR
  - Reflection
  - Concurrencia
  - Spring Framework
  - Dependency Injection
  - Inversion of Control
  - Factory Pattern
  - Unit Testing
  - Programar contra Interfaces
---
# Intro - PAW

## Cliente - Servidor HTTP

![[Pasted image 20260803194318.png]]

El browser (cliente) y el HTTP server hablan HTTP; el body viaja en HTML. Puerto **80** por default.

## El Application Container usa Reflection

![[Pasted image 20260803204815.png]]

El contenedor no conoce mi servlet en compile-time: lo instancia por **reflection**, a partir del nombre de clase que yo declaro como texto en `web.xml`. Es el mismo mecanismo que cargar un driver JDBC:

```java
Class<?> c = Class.forName("org.postgresql.SQLDriver");
c.newInstance();
```

Todo servlet implementa (indirectamente) la interfaz de `javax.servlet`:

```java
interface HttpServlet {
    void doGet(HttpServletRequest, HttpServletResponse);
    void doPut(HttpServletRequest, HttpServletResponse);
    void doPost(HttpServletRequest, HttpServletResponse);
    void doDelete(HttpServletRequest, HttpServletResponse);
    ...
}
```

`web.xml` — declaración completa del servlet + su mapping (la foto corta en `<url-patt`):

```xml
<servlet>
    <servlet-name>indexServlet</servlet-name>
    <servlet-class>ar.edu.itba.paw.IndexServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>indexServlet</servlet-name>
    <url-pattern>/index</url-pattern>
</servlet-mapping>
```

### Notas
- En el pizarrón las etiquetas aparecen abreviadas como `<name>`/`<class>`; las etiquetas reales que exige el schema de Servlets son **`<servlet-name>`** y **`<servlet-class>`**.
- `<servlet>` y `<servlet-mapping>` son dos bloques separados a propósito: `<servlet>` dice qué clase es, `<servlet-mapping>` dice qué URL la dispara. Separarlos permite mapear la misma clase a varias URLs, o cambiar la URL sin tocar el código.
- El paralelismo con `Class.forName` no es casualidad: en ambos casos el framework recibe un **string** con el nombre de una clase y la instancia dinámicamente, sin `import` ni dependencia de compilación contra ella. Por eso el contenedor puede quedar totalmente desacoplado de mi lógica de negocio.

## El contenedor no necesita saber qué hace mi aplicación

Reflection + el contrato de `web.xml` permiten que la aplicación sea **autocontenida**: todo lo necesario para correrla (clases, dependencias, mapeos) viaja empaquetado en el `.war`, y el contenedor solo sabe leer ese contrato.

## JAR (Java ARchive)

![[Pasted image 20260803205932.png]]

Empaqueta clases compiladas respetando la estructura de paquetes:

```
ar/edu/itba/paw/IndexServlet.class   # paquete ar.edu.itba.paw
META-INF/MANIFEST.MF
```

```bash
java -jar myprogram.jar
```

### Notas
- Se puede declarar `Main-Class` dentro de `MANIFEST.MF` para que `java -jar` sepa qué clase correr sin pasarla por parámetro. La forma larga sin ese default sería indicar la clase explícitamente en el comando (lo que se ve en la diapo como recordatorio).

## WAR (Web ARchive)

![[Pasted image 20260803210301.png]]

```
miapp.war
├── META-INF/
├── WEB-INF/
│   ├── classes/
│   │   └── ar/edu/...
│   ├── lib/
│   │   └── log4j.jar
│   └── web.xml
├── index.html
├── styles.css
└── js/
    └── script.js
```

### Notas
- La diapo escribe `libs/`; la carpeta estándar de la spec de Servlets es `WEB-INF/lib/` (singular). Para un WAR real que levante Tomcat, es `lib/`.
- **Todo lo que no está en `WEB-INF/` es público**: se sirve directo por URL (`index.html`, `styles.css`, `js/`). Todo lo que está dentro de `WEB-INF/` (clases, libs, `web.xml`) el contenedor no lo expone directo al cliente — solo lo lee él para resolver los requests contra los servlets mapeados.

## Servlets: sin estado compartido

>[!warning]
>No tener estados compartidos para evitar concurrencias

### Notas
- El contenedor crea **una sola instancia** de cada servlet y la reutiliza para atender requests concurrentes, cada uno en su propio thread. Si guardo estado mutable en un atributo de instancia del servlet, dos requests simultáneos lo pisan entre sí → race condition.
- Corolario práctico: los datos que dependen de un request puntual (parámetros, resultados intermedios) van en variables **locales** del método (`doGet`/`doPost`), nunca en campos de la clase del servlet.

## Frameworks

![[Pasted image 20260803212557.png]]

Ediciones de Java: SE (Standard), ME (Mobile), EE (Enterprise) — EE es la que trae las specs de servlets/web. Frameworks web más viejos como **Struts** o **Tapestry** pedían que tus clases heredaran de clases del framework o implementaran sus interfaces. La idea de **Convention over Configuration** (menos XML, más defaults razonables) es lo que después empuja a Spring y a Spring Boot.

Hoy el estándar de facto es **Spring Framework**. Es modular: yo elijo qué partes uso (Spring MVC, Spring Data, Spring Security, etc.), no es todo o nada.

![[Pasted image 20260803213246.png]]

Spring es, en el fondo, un motor de **Dependency Injection (DI)** que implementa **Inversion of Control (IoC)**.

### DI manual con Factories (antes de un contenedor de IoC)

![[Pasted image 20260803214135.png]]

```java
class Car {
    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public Engine getEngine() {
        return engine;
    }
}

class EngineFactory {
    public static Engine newEngine() {
        return new ICEngine();
    }
}

class CarFactory {
    public static Car newCar() {
        return new Car(EngineFactory.newEngine());
    }
}

class Bike {
    private Engine engine;

    public Bike(Engine engine) {
        this.engine = engine;
    }
}
```

`Car` y `Bike` son **POJOs**: no crean su propio `Engine`, lo reciben ya armado por constructor. Quien sabe cómo construir cada pieza es la factory (`EngineFactory`, `CarFactory`) — `Car` no sabe ni le importa si el motor es un `ICEngine` u otro.

### Notas
- En la diapo, `CarFactory` llama a `EngineFactory.newFactory()`, pero `EngineFactory` solo declara `newEngine()`. Lo dejé corregido arriba como `newEngine()` asumiendo errata de tipeo del pizarrón; vale la pena confirmarlo contra el material de la cátedra.
- Esto **ya es DI**, solo que manual: `Car`/`Bike` no gestionan su propia dependencia (`Engine`), la reciben inyectada por constructor. Lo que automatiza Spring es exactamente este patrón: en vez de escribir un `XxxFactory` a mano por cada clase, declarás la clase como bean (`@Component`, `@Service`, etc.) y el contenedor arma el grafo de dependencias por **reflection** — el mismo mecanismo que el application container usa para instanciar servlets a partir de `web.xml`.
- `Car` y `Bike` dependen de `Engine` de la misma forma; si mañana sumo `Truck`, `Moto`, etc., escribir una factory a mano por cada una no escala. Ese dolor concreto es lo que motiva pasar de "factories a mano" a un contenedor de IoC.

### Spring: nosotros no instanciamos ni invocamos, eso lo hace el contenedor

Con Spring, mi código de aplicación **no** hace `new Car(...)` ni llama a mano a `CarFactory.newCar()`. Yo declaro las clases como beans (`@Component`, `@Service`, `@Bean`, etc.) y es el contenedor (`ApplicationContext`), al arrancar, quien:

1. instancia cada bean (por reflection, igual que venimos viendo con los servlets),
2. resuelve el grafo de dependencias entre beans e inyecta lo que corresponda,
3. y cuando otra clase necesita ese bean, se lo entrega ya construido — nunca soy yo quien invoca el constructor o el método factory.

Esa es la inversión completa: no solo se invierte *quién crea* el objeto, se invierte *quién tiene la iniciativa* de invocar el código de construcción. Yo declaro qué necesito; el contenedor decide cuándo y cómo lo arma.

### Cómo ayuda esto al Unit Testing

Como `Car` recibe su `Engine` por constructor —no lo crea con `new ICEngine()` adentro, ni lo pide con una llamada estática tipo `EngineFactory.newEngine()`— en un test unitario le puedo pasar cualquier implementación de `Engine`, incluido un mock, sin levantar el contenedor de Spring para nada:

```java
@Test
void testCarUsesEngine() {
    Engine mockEngine = mock(Engine.class);
    Car car = new Car(mockEngine);   // no hace falta Spring para este test

    assertEquals(mockEngine, car.getEngine());
}
```

Esto es justo lo que el approach viejo no permitía: si `Car` llamara `new ICEngine()` (o `EngineFactory.newEngine()`) dentro de su propio constructor, el test siempre correría contra la implementación real, con sus efectos colaterales, latencia o dependencias externas — no habría forma de sustituirla. Al desacoplar "quién construye la dependencia" de "quién la usa", DI permite testear cada clase **aislada** (unit test real) en vez de necesitar todo el entorno levantado (integration test) para probar una sola pieza.

### Notas
- Esto es lo que habilita usar frameworks de mocking (Mockito, etc.) sin arrancar la `ApplicationContext`: los tests unitarios quedan rápidos porque no dependen del contenedor, solo de la clase bajo test y sus mocks.
- Mismo principio que la nota de [[#Servlets: sin estado compartido]]: en ambos casos, que el framework sea quien controla la construcción/ciclo de vida del objeto obliga a diseñar clases desacopladas de sus dependencias concretas — eso es lo que después se aprovecha tanto para inyectar mocks en tests como para cambiar implementaciones en producción sin tocar código.

## Programar contra Interfaces

![[Pasted image 20260803215330.png]]

```java
public int sum(ArrayList<Integer> l) {
    int ret = 0;
    for (Integer i : l) {
        ret += (i == null ? 0 : i);
    }
    return ret;
}

sum(new ArrayList<>(linkedList));
```

Este ejemplo está armado a propósito como el caso **malo**: `sum` solo itera sobre `l` (for-each), no usa nada específico de `ArrayList` (como acceso por índice). Aun así, la firma pide el tipo concreto `ArrayList<Integer>`. Entonces si quien llama tiene un `LinkedList<Integer>`, no puede pasarlo directo — tiene que copiar todos sus elementos a un `ArrayList` nuevo solo para poder llamar al método: `sum(new ArrayList<>(linkedList))`. Esa copia es pura pérdida (tiempo + memoria) motivada únicamente por una firma demasiado específica.

La firma correcta, "programando contra la interfaz", pediría solo lo que el método realmente necesita:

```java
public int sum(Iterable<Integer> l) { ... }   // alcanza con poder iterar

sum(linkedList);   // ahora anda directo, sin copiar nada
sum(arrayList);    // sigue andando: ArrayList también es Iterable
```

Regla general: el tipo del parámetro (o campo, o valor de retorno) tiene que ser el más **general** de la jerarquía (`Iterable` ⊂ `Collection` ⊂ `List` ⊂ `ArrayList`) que todavía provea las operaciones que uso. El tipo concreto (`new ArrayList<>()`, `new LinkedList<>()`) queda relegado al lugar donde el objeto se construye, no se propaga por todo el código que solo lo consume.

### Relación con DI y Unit Testing

Es el mismo principio que venimos viendo con Spring, aplicado a un caso más chico: si el método/clase depende de una **interfaz** en vez de una implementación concreta, quien la llama —incluido un test— puede pasar cualquier cosa que cumpla ese contrato, sin tocar el método. `programar contra interfaces` es justamente el principio de diseño que hace *posible* la Dependency Injection: Spring puede inyectar cualquier implementación de `Engine` porque `Car` depende del tipo `Engine` (interfaz), no de `ICEngine` (clase concreta) — es exactamente el mismo error que pedir `ArrayList` en vez de `Iterable`. Y es lo que permite, en un test, pasar un mock que implementa la interfaz en lugar de la implementación real.

### Notas
- El chequeo `i == null ? 0 : i` hace falta porque `List<Integer>` puede contener `null` (autoboxing de `Integer`, no de `int` primitivo); con un `int[]` no existiría ese problema, pero tampoco se podría usar for-each sobre una interfaz genérica.
- Ojo con la trampa real de este ejemplo: `sum(new ArrayList<>(linkedList))` **funciona** — compila y da el resultado correcto — pero esconde una copia innecesaria de toda la lista en cada llamada. El bug no es funcional, es de performance/diseño, y por eso es fácil no notarlo si solo se corren tests de que el resultado es correcto.

## Preguntas

- ¿Ya en esta cátedra se ve la alternativa a `web.xml` con anotaciones (`@WebServlet`), o el curso se queda con el deployment descriptor XML?
- Más allá de tirar los `.jar` en `WEB-INF/lib`, ¿hay alguna forma de gestionar esas dependencias (Maven/Gradle) que se vea en la materia?
- Si el servlet no puede guardar estado de instancia, ¿dónde se guarda el estado de sesión de un usuario entre requests (`HttpSession`)?
- ¿`EngineFactory.newFactory()` en la diapo es errata por `newEngine()`, o hay un método adicional que no llegué a anotar?
- ¿En los TPs de la materia se usa Mockito (u otro framework de mocking) para testear beans en aislamiento, o los tests son de integración contra el `ApplicationContext` real?

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PAW)**

[[Clase 2 - PAW]] — Siguiente clase

**Otras materias**

- **Protos**  [[2. Protos - HTTP]] — protocolo HTTP que corre entre el browser y el application container
- **POO**  [[POO - Introduccion a Java]] — compilación a `.class` (`javac`/`java`), base de lo que empaqueta un JAR/WAR

<!-- notas-relacionadas:fin -->
