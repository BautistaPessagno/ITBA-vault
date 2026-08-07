# AGENTS.md

Guía para agentes que trabajan en este vault.

## Qué es esto

Un vault de Obsidian con **apuntes universitarios** (ITBA): notas de clase, prácticas,
resúmenes de parcial/final, TPs y guías de ejercicios.

El vault **crece cada cuatrimestre**: se agregan materias, se archivan otras, y los temas
cambian. Por eso este archivo describe **convenciones**, no un inventario. Nunca asumas
qué materias existen — averigualo (ver abajo).

## Antes de responder

Para cualquier pregunta teórica, académica o técnica, **buscá primero en el vault**
antes de contestar desde conocimiento general. Si existe una nota del tema, esa es la
fuente de verdad: refleja cómo lo dio *esta* cátedra, con su notación y su recorte.
Si el vault y tu conocimiento general difieren, decilo explícitamente en vez de elegir
en silencio.

## Cómo descubrir el estado actual del vault

No hay una lista fija de materias. Derivala en el momento:

1. **Materias existentes** → los archivos `Categories/*.base`. Casi todos son una materia,
   con dos excepciones: `ITBA.base` (vista global) y cualquier `.base` temático o de índice
   (p. ej. `OSI.base`, `Materias.base`) — estos no representan una materia en sí.
2. **Materia de una nota** → la propiedad `Materia` de su frontmatter, que apunta al
   `.base` correspondiente.
3. **Notas de una materia** → las que linkean a ese `.base`.
4. **Temas cubiertos** → la propiedad `temas` del frontmatter, más los headings.
5. **Resumen + cuatrimestre de cada materia** → `Categories/Materias.base` (ver abajo).

Un `grep` sobre `^Materia:` en la raíz da el censo completo en una pasada.

### `Categories/Materias.base`

Índice de materias con cuatrimestre y año de cursada. Cada materia tiene **una** nota
`Materia - <Nombre>.md` en la raíz con:

```yaml
---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[<materia>.base|<alias>]]"
Cuatrimestre: "1C"   # o "2C"
Año: 2026
temas:
  - <tema principal 1>
  - <tema principal 2>
---
# Materia - <Nombre>

Resumen MUY breve (2-3 líneas) de los temas de la materia.

[[Materias.base]]
```

El link a `[[Materias.base]]` en el cuerpo es lo que hace que la nota entre en ese índice
(mismo mecanismo que cualquier otro `.base`). `Materias.base` agrupa por una fórmula
`Año + " - " + Cuatrimestre` — **el año va primero** para que el orden alfabético
coincida con el orden cronológico (si se agrupara por `Cuatrimestre + Año`, "2C 2024"
ordenaría antes que "1C 2026", que es incorrecto).

Al agregar una materia nueva, creá también su nota `Materia - <Nombre>.md`.

## Organización

**Estructura plana, bottom-up** (estilo Steph Ango): las notas de contenido viven en la
**raíz del vault**, nunca en subcarpetas. La organización es por propiedades de
frontmatter y por links, no por carpetas.

Carpetas administrativas — **no poner notas de contenido acá**:
`_Claude/`, `Attachments/`, `Excalidraw/`, `Templates/`, `Categories/`

## Frontmatter

Contrato mínimo de toda nota de contenido:

```yaml
---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[<materia>.base|<alias>]]"
temas:
  - <tema>
---
```

- `categories` incluye siempre `ITBA.base` — es lo que la hace aparecer en la vista global.
- `Materia` apunta al `.base` de la materia. **El alias después del `|` no es estable**
  (hay variantes históricas y espacios de más); parseá el nombre del `.base`, no el alias.
- `temas` alimenta la búsqueda y el repaso. Vale la pena que sea generoso.
- Campos opcionales que aparecen en notas existentes: `Tags`, `Created`, `Cuatri`.

## Crear notas

1. Ubicarla en la **raíz**.
2. Partir del template `Templates/Nota de Clase.md`.
3. Completar `categories`, `Materia` y `temas`.
4. **Escribir en español**, salvo que estés editando una nota que ya está en inglés.
5. Linkear al `.base` de la materia en el cuerpo para que aparezca en esa vista.
6. Agregar el bloque de notas relacionadas (abajo).

## Notas relacionadas

Cada nota de contenido termina con un bloque delimitado por marcadores:

```markdown
---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (<Materia>)**

- [[Nota vecina]] — razón corta

**Otras materias**

- **<Materia>**  [[Nota de otra materia]] — qué concepto comparten

<!-- notas-relacionadas:fin -->
```

Reglas:

- **Todo link lleva una razón.** Un link sin justificación no aporta; el valor está en
  nombrar *qué* comparten las dos notas.
- Los links cruzados llevan la materia en negrita adelante, porque los nombres de nota
  no siempre dejan claro de qué materia son.
- Los marcadores HTML existen para que el bloque sea **regenerable**: reemplazá lo que
  está entre ellos, nunca lo apilas ni tocás el contenido de arriba.
- Obsidian ya muestra backlinks, así que no hace falta que cada link tenga su recíproco
  explícito. Priorizá el sentido en que uno realmente navegaría.

Qué linkear:

- **Dentro de una materia**: clase anterior/siguiente, teoría ↔ práctica de ese tema,
  resumen ↔ las clases que resume, TP ↔ los temas que aplica.
- **Entre materias**: el mismo concepto visto desde otro ángulo. Estos son los links que
  más rinden para estudiar, y son los que nadie hace solo. Buscá conceptos que reaparecen
  con otro nombre o en otra capa de abstracción.
- **No fuerces links cruzados.** Algunas materias son legítimamente disjuntas del resto.
  Un bloque con solo "Misma materia" es un resultado correcto.

## Archivos `.base`

Formato Obsidian Bases. `Categories/ITBA.base` filtra por `file.hasLink("ITBA.base")`;
cada base de materia filtra por `file.hasLink("<materia>.base")`. Si creás una materia
nueva, creá su `.base` en `Categories/` siguiendo uno existente como molde.

## Al terminar

Después de crear, mover o borrar notas, revisá si hay que actualizar:

- los `.base` afectados,
- los bloques de notas relacionadas que apuntaban a una nota renombrada o borrada
  (un link roto es peor que un link faltante).
