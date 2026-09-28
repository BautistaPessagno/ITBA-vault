---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[POO.base|POO]]"
temas:
  - Ruby
  - Colecciones
  - Arrays
  - Hash
  - Set
  - Enumerable
  - Map
  - Reduce
---
# POO — Colecciones en Ruby

## Resumen

### Arrays
Colecciones ordenadas; acceso por posición (base 0). Índice negativo = relativo al final.

```ruby
a = [1, 2, 3, 4]
a[0]   # => 1
a[-1]  # => 4
a.push(5)     # agrega al final
a.pop         # saca del final
```

**Operaciones de conjuntos:**
```ruby
a | b   # unión
a & b   # intersección
a - b   # resta
a.uniq  # elimina duplicados (no modifica original)
a.uniq! # modifica original (bang method)
```

**Funciones de orden superior:**
```ruby
a.map  { |e| e * 2 }    # nueva colección con la transformación aplicada
a.select { |e| e > 2 }  # filtrar elementos
a.count { |e| e.odd? }  # contar elementos que cumplen condición

# reduce: combina elementos con una función binaria
a.reduce { |acc, e| acc.length > e.length ? acc : e }
```

### Set
Colección **sin orden y sin repetidos**. Implementado con tabla hash (similar a `HashSet` en Java).

```ruby
require 'set'
s = Set.new([1, 2, 3])
```

Para funcionar en un Set, la clase debe definir:
- `hash`: para indexar en la tabla hash.
- `eql?(other)`: para comparar equivalencia.

**SortedSet:** requiere `<=>` implementado; usa RBTree internamente (similar a `TreeSet` en Java).

### Hash (Mapa clave-valor)
Colección **no ordenada** de pares clave→valor. Similar a `HashMap` en Java.

```ruby
h = { nombre: "Juan", edad: 25 }
h[:nombre]     # => "Juan"
h[:inexistente] # => nil

# Valor por defecto con llaves (evalúa el bloque cada vez):
h = Hash.new { |hash, key| hash[key] = [] }
```

**Diferencia importante:** valor por defecto con `()` → **misma instancia**; con `{}` → **nueva instancia en cada acceso**.

### Módulo Enumerable
Las colecciones de Ruby incluyen `Enumerable` e implementan `each` para iterar.

```ruby
# each
[1, 2, 3].each { |e| puts e }

# reverse_each
[1, 2, 3].reverse_each { |e| puts e }

# Enumerator externo
enum = [1, 2, 3].each
enum.next  # => 1
enum.next  # => 2
```

### Operador Spaceship `<=>`
Retorna -1, 0 o 1 (o nil si son de distinto tipo). Base para el módulo `Comparable`.

```ruby
class Temperatura
  include Comparable
  def <=>(other)
    @valor <=> other.valor
  end
end
# Habilita: <, >, <=, >=, between?, clamp, sort
```

## Notas
- `bang methods` (terminan en `!`) modifican el objeto original.
- `clamp(min, max)` → limita un valor dentro del rango dado.
- `SortedSet` requiere instalar la gema: `gem install sorted_set`.

## Preguntas
- ¿Por qué la diferencia entre `Hash.new(0)` y `Hash.new { 0 }` importa?
- ¿Qué métodos habilita incluir el módulo `Comparable`?
- ¿Cuál es la diferencia entre `map` y `select`?

[POO](Categories/POO.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (POO)**

- [POO - Intro Ruby](POO%20-%20Intro%20Ruby.md) — tema anterior
- [POO - Colecciones Java](POO%20-%20Colecciones%20Java.md) — el equivalente en Java
- [POO - Streams y Lambdas](POO%20-%20Streams%20y%20Lambdas.md) — Enumerable y bloques vs streams

**Otras materias**

- **EDA**  [EDA - Hashing](EDA%20-%20Hashing.md) — Hash en Ruby

<!-- notas-relacionadas:fin -->
