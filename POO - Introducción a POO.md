---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - ADT
  - Clases
  - Herencia
  - Polimorfismo
  - Composición
  - Java
---
# POO — Introducción a Programación Orientada a Objetos

## Resumen

### ADT (Abstract Data Type)
- **Hiding** (ocultamiento de información): alta cohesión + bajo acoplamiento.
- **Encapsulamiento**: el estado solo se modifica mediante los métodos de la clase.
- **Contrato**: interfaz pública bien definida.

### POO — Conceptos Clave
- **Clase**: molde/modelo para crear objetos. Define propiedades y comportamiento.
- **Objeto/Instancia**: ocurrencia de una clase. Tiene su propio estado.
- **Mensaje**: invocación de un método de instancia.
- **Método de clase** (`static`): se puede invocar sin instancia.
- **Variable de clase** (`static`): compartida por todos los objetos de la clase.
- `this.` → referencia a la propia instancia.
- `super` → referencia a la clase padre.

**Modificadores de acceso:**
| Modificador | Acceso |
|---|---|
| `private` | Solo la propia clase |
| `protected` | La clase y sus subclases |
| `public` | Todos |

### Herencia
Crear una nueva clase en base a una existente. La subclase **hereda** atributos y métodos del padre.

```java
class DateTime extends Date {
    private int hour, minute, second;
    public DateTime(int y, int m, int d, int h, int min, int s) {
        super(y, m, d); // llama al constructor del padre
        this.hour = h; ...
    }
}
```

`@Override` → indica que se sobreescribe un método del padre (el compilador verifica que exista).

**Reglas de diseño de herencia:**
1. Agrupar propiedades comunes en superclases.
2. No usar campos `protected` — ofrecer setters protegidos en su lugar.
3. Usar herencia para modelar la relación **"es-un"** (Cuadrado es un Rectángulo).
4. Usar herencia solo si **todos** los métodos heredados tienen sentido.
5. Preferir polimorfismo sobre `instanceof`.

**Clase abstracta:** no se puede instanciar. Sus métodos abstractos deben ser implementados por las subclases.
```java
public abstract class File { public abstract void delete(); }
```

### Polimorfismo
- **Sobrecarga**: métodos con mismo nombre en distintas clases.
- **Paramétrico**: mismo nombre, distintos parámetros (en la misma clase).
- **Redefinición**: la clase hija sobreescribe un método del padre (`@Override`).

### Composición vs Herencia
- **Herencia** → relación "es-un".
- **Composición** → relación "tiene-un" (una clase contiene instancias de otra).
- **Asociación** → dos objetos trabajando juntos.

Preferir composición cuando la relación no es "es-un" o cuando el cambio de rol en tiempo de ejecución es necesario.

## Notas
- Java no tiene herencia múltiple (para clases), pero sí con interfaces.
- La resolución de mensajes siempre parte del **constructor utilizado** (tipo real del objeto).
- No confundir Clase con Instancia: si solo habrá pocas instancias, considerar Enum o constantes.

## Preguntas
- ¿Cuál es la diferencia entre polimorfismo de sobrecarga y de redefinición?
- ¿Por qué es mejor evitar `instanceof` fuera del método `equals`?
- ¿En qué caso conviene composición sobre herencia?

[POO](Categories/POO.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [POO - Introduccion a Java](POO%20-%20Introduccion%20a%20Java.md) — tema siguiente
- [POO - Interfaces y Generics](POO%20-%20Interfaces%20y%20Generics.md) — polimorfismo e interfaces

**Otras materias**

- **PI**  [PI - TAD en C](PI%20-%20TAD%20en%20C.md) — el TAD como antecedente de la clase

<!-- notas-relacionadas:fin -->
