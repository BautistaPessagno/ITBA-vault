---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[Data Structures and Algorithms.base|Data Structures and Algorithms]]"
temas:
  - Hashing
  - Hash Table
  - Colisiones
  - Open Addressing
  - Chaining
---
# EDA — Hashing

## Resumen

### Hash Table
Estructura que usa un arreglo (*lookup table*) para almacenar pares **key/value**. Prioriza la búsqueda → acercarse a **O(1)**.

No mantiene contigüidad ni orden de los elementos.

### Función de Hash
```
hash(key) = prehash(key) % |LookUp|
```
El usuario provee `prehash`, sin necesidad de conocer el tamaño del LookUp.  
La función siempre devuelve una ranura válida del arreglo.

**Función perfecta:** inyectiva → key₁ ≠ key₂ ⟹ hash(key₁) ≠ hash(key₂). Sin colisiones.

### Factor de Carga
```
factor_carga = |Keys_usadas| / |LookUp|
```
Cuando supera el **umbral predefinido** (Load Factor Threshold) → duplicar espacio y **rehashear** todo.

### Colisiones
Ocurren cuando `hash(key₁) == hash(key₂)` con `key₁ ≠ key₂`.

**Dos estrategias de resolución:**

#### 1. Open Addressing (Closed Hashing)
Los elementos que colisionan se guardan **dentro** de la misma tabla. Cada ranura tiene 3 estados: elemento presente, baja lógica o baja física.

| Técnica | Descripción |
|---|---|
| **Linear Probing** | Si la ranura i está ocupada → intentar i+1, i+2… (circular) |
| **Quadratic Probing** | Intervalos cuadráticos: i+1², i+2², i+3²… Problema: puede no encontrar espacio aunque lo haya |
| Combinación determinística | La última técnica debe ser linear probing |

#### 2. Chaining (Open Hashing)
Los elementos que colisionan se almacenan **fuera** de la tabla (ej: lista enlazada en cada ranura).  
→ Estrategia de Java `HashSet`/`HashMap`: cada ranura tiene una lista de colisiones.

### Hash en Java
`HashSet` → implementado internamente con `HashMap`.  
`contains()` usa primero `hashCode()` y luego `equals()`.  
**Regla fundamental:** si dos objetos son `equals` → deben tener el mismo `hashCode`.

```java
@Override
public int hashCode() {
    return Objects.hash(campo1, campo2);
}
```

## Notas
- **No mutar un objeto** que está en un Set/Map → puede perder su ranura.
- En Ruby, los equivalentes son `hash` y `eql?`.

## Preguntas
- ¿Por qué el quadratic probing puede no encontrar espacio libre?
- ¿Qué pasa si no se implementa `hashCode` al sobreescribir `equals`?
- ¿Cuándo se rehashea y qué costo tiene?

[[Data Structures and Algorithms.base|Data Structures and Algorithms]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (EDA)**

- [[EDA - Listas Lineales]] — encadenamiento para colisiones

**Otras materias**

- **BD**  [[BD clase 16 programacion embebida]] — índices hash
- **POO**  [[POO - Colecciones Java]] — HashMap y HashSet
- **POO**  [[POO - Colecciones Ruby]] — Hash en Ruby

<!-- notas-relacionadas:fin -->
