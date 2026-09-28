---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Ruby
  - Clases
  - Herencia
  - Módulos
  - Bloques
  - Excepciones
---
# POO — Introducción a Ruby

## Resumen

### Ruby — Características
- Tipado **dinámico**: no se declara el tipo de las variables.
- Archivos `.rb`. Naming convention: **snake_case**.
- Sin herencia múltiple de clases; soportada mediante **módulos** (mixins).
- No existe polimorfismo paramétrico (solo un constructor: `initialize`).
- Framework web principal: **Ruby on Rails**.

### Clases y Objetos
```ruby
class Date
  def initialize(year, month, day)
    @year = year    # variable de instancia (comienza con @)
    @month = month
    @day = day
  end
  
  attr_accessor :day, :month, :year  # genera getter y setter automáticos
end

aux = Date.new(2024, 10, 9)
puts aux.day     # getter
aux.day = 30     # setter
```

**Variables de clase** comienzan con `@@`:
```ruby
class Date
  @@start_year = 1900
end
```

**Constantes:** `NOMBRE_CONSTANTE = valor`

### Herencia
```ruby
class DateTime < Date   # < indica herencia
  def initialize(day, month, year, hour, min, sec)
    super(day, month, year)  # llama al mismo método en la clase padre
    @hour = hour
  end
end
```

`super` en Ruby llama al **mismo método** en la clase padre (a diferencia de Java que permite llamar constructores específicos).

### Métodos
```ruby
# Método de instancia
def metodo(param1, param2 = "default")
  # ...
end

# Método de clase
def self.metodo_de_clase
  # ...
end

# Privado
private

def metodo_privado
end

# Protegido: visible desde cualquier instancia de la misma clase
protected

def metodo_protegido
end
```

### Bloques
Pedazos de código asociados a un nombre; se invocan con `yield`.
```ruby
def ejecutar
  yield             # invoca el bloque
end

ejecutar { puts "Hola" }

# Bloque con variable
def iterar
  yield(42)
end
iterar { |n| puts n }
```

### Condiciones y Loops
```ruby
puts "Hola" if condicion
puts "No" unless condicion

while condicion do ... end
until condicion do ... end  # hasta que sea verdadera
```

`case`/`when` usa `===` para comparar (mal estilo en Ruby si hay alternativa OO).

### Excepciones
```ruby
raise 'Error' unless lado > 0
raise MiError.new("mensaje")

begin
  # código
rescue MiError => e
  # manejo
end
```
- Solo capturar **errores** (subclases de `StandardError`), no excepciones de `Exception`.
- Las **clases abstractas** se simulan lanzando error en `initialize`.

### Módulos (Mixins)
```ruby
module Comparable
  def mayor?(otro)
    self > otro
  end
end

class Temperatura
  include Comparable   # métodos del módulo como instancia
  extend Comparable    # métodos del módulo como clase
end
```

### Comparación Java ↔ Ruby
| Java | Ruby |
|---|---|
| `instanceof` | `is_a?` |
| `getClass()` | `instance_of?` |
| `equals` | `==` (hay que sobreescribir) |
| `==` | `equal?` |
| `toString()` | `to_s` |

## Notas
- Solo `nil` y `false` son falsos en Ruby; cualquier otro valor es verdadero.
- `to_s` → representación string; `inspect` → para debugging.
- Usar `unless` en lugar de `if !condicion`.
- `::` → acceder a constantes de una clase (`Date::ENERO`).

## Preguntas
- ¿Por qué Ruby no tiene polimorfismo paramétrico?
- ¿Cuál es la diferencia entre `include`, `extend` y `prepend`?
- ¿Cómo se simula una clase abstracta en Ruby?

[POO](Categories/POO.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [POO - Colecciones Ruby](POO%20-%20Colecciones%20Ruby.md) — tema siguiente
- [POO - Introducción a POO](POO%20-%20Introducción%20a%20POO.md) — clases y herencia
- [POO - Introduccion a Java](POO%20-%20Introduccion%20a%20Java.md) — comparación entre lenguajes

<!-- notas-relacionadas:fin -->
