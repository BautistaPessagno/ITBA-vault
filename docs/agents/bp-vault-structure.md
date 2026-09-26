<!-- bp-vault-skills:start -->

# Contrato de estructura de BP Vault

## Límite del vault

La raíz del vault es `/Users/bautistapessagno/Documents/ITBA`.

Toda búsqueda, lectura, edición, creación y resolución de links debe permanecer dentro de esta raíz. No seguir links ni usar materiales de otros vaults salvo que el usuario los proporcione expresamente como fuentes externas.

Las guías para agentes son:

- `AGENTS.md`
- `CLAUDE.md`

Ambas apuntan a estos contratos:

- `docs/agents/bp-vault-structure.md`
- `docs/agents/bp-vault-method.md`

## Idioma

Responder y crear contenido en español.

Si una nota existente está escrita en inglés, conservar su idioma al editarla. Los nombres técnicos y la notación de la cátedra se mantienen aunque estén en otro idioma.

## Ubicación y reconocimiento de objetos

Las notas de contenido viven en la raíz del vault. La organización es plana y depende del frontmatter, los links y los archivos `.base`.

No crear notas de contenido dentro de estas carpetas administrativas:

- `_Claude/`
- `Attachments/`
- `Excalidraw/`
- `Templates/`
- `Categories/`

`ML TP1/` contiene artefactos de un trabajo, como notebooks y datos. No establece una ubicación general para notas.

Los tipos de nota se reconocen por nombre, contenido y links. No existe una propiedad de tipo obligatoria.

- `Materia - <Nombre>.md` representa una materia.
- Las notas de clase suelen incluir `Clase` o una numeración en el nombre.
- Las prácticas, guías, resúmenes, TPs, defensas y repasos suelen identificarse en el nombre o el título.
- Una nota pertenece a una materia según la propiedad `Materia`, no según el alias ni el nombre visible.
- La intención de una nota no se deduce únicamente del prefijo del archivo.

No crear una taxonomía, carpeta o propiedad para estos tipos sin una solicitud separada.

## Template

Las notas nuevas parten de `Templates/Nota de Clase.md`.

Al crear una nota:

1. Mostrar el borrador y obtener aprobación.
2. Crear el archivo en la raíz.
3. Completar el frontmatter mínimo.
4. Mantener el título y el nombre de archivo claros.
5. Enlazar la `.base` de la materia en el cuerpo.
6. Agregar el bloque de notas relacionadas.

El template existente tiene incompleto el encabezado `**Misma materia`. Este setup no lo corrige. Al usarlo, producir el bloque válido definido en `AGENTS.md` y `CLAUDE.md`.

## Propiedades

El frontmatter mínimo de una nota de contenido contiene:

```yaml
categories:
  - "[[ITBA.base|ITBA]]"
Materia: "[[<materia>.base|<alias>]]"
temas:
  - <tema>
```

Reglas:

- `categories` debe incluir `[[ITBA.base|ITBA]]`.
- `Materia` apunta al archivo `.base` correspondiente.
- El nombre del `.base` determina la materia. El alias no es estable.
- `temas` es una lista libre y suficientemente descriptiva.
- No existe una propiedad obligatoria de estado, intención, desarrollo o aprendizaje.

Propiedades adicionales observadas:

- `Tags`: lista opcional, normalmente vacía y sin vocabulario controlado.
- `Created`: fecha o fecha-hora opcional con formatos históricos distintos.
- `Cuatri`: propiedad heredada con valores no uniformes.
- `Cuatrimestre`: `"1C"` o `"2C"` en notas de materia.
- `Año`: año numérico en notas de materia.
- `aliases`: lista opcional.
- `Date`: propiedad heredada y poco usada.

No normalizar propiedades heredadas ni inventar valores durante una edición no relacionada.

Los conceptos `developed` y `learned` no se mapean a propiedades. Sus criterios están en el contrato de método.

## Índices y navegación

`Categories/ITBA.base` es la vista global. Incluye archivos que enlazan `ITBA.base`.

`Categories/Materias.base` indexa las notas `Materia - <Nombre>.md` mediante el link a `Materias.base`. Agrupa por `Año + " - " + Cuatrimestre`, con el año primero.

Los demás archivos `Categories/*.base` suelen representar materias. Las excepciones se determinan por su contenido y función, no por una lista fija.

Para descubrir el estado actual:

1. Enumerar `Categories/*.base`.
2. Excluir las bases globales, temáticas o de índice.
3. Leer `Materia` para clasificar una nota.
4. Consultar `temas` y headings para conocer su alcance.
5. Consultar `Materias.base` y las notas de materia para cuatrimestre y resumen.

Una nota nueva de materia requiere:

- su `.base` en `Categories/`;
- una nota `Materia - <Nombre>.md` en la raíz;
- `Cuatrimestre` y `Año`;
- link a `Materias.base`.

Esta operación siempre necesita borrador y aprobación.

## Convenciones de Markdown y links

Usar wikilinks de Obsidian:

- `[[Nota]]`
- `[[Nota|alias]]`
- `[[Nota#Heading|alias]]`
- `![[Adjunto]]`
- `![[Adjunto|ancho]]`

El enlace a una materia en frontmatter sigue la forma `[[<materia>.base|<alias>]]`.

Cada nota de contenido termina con un único bloque delimitado por:

```markdown
<!-- notas-relacionadas:inicio -->
...
<!-- notas-relacionadas:fin -->
```

Todo link dentro de ese bloque lleva una razón.

Los links cruzados indican la materia antes del link. No se exige reciprocidad y no se fuerzan relaciones entre materias.

El título principal usa `#`. Los apartados internos siguen una jerarquía de headings coherente. No normalizar nombres, guiones, acentos o capitalización de notas antiguas sin aprobación.

## Cambios mecánicos permitidos

Un agente puede aplicar directamente solo mantenimiento literal:

- corregir indentación, espacios o formato sin cambiar significado;
- corregir marcadores duplicados o dañados cuando el contenido efectivo no cambia;
- actualizar referencias exactas después de un movimiento o renombre ya aprobado;
- reemplazar el interior de un bloque administrado con contenido previamente aprobado;
- comprobar rutas, links y archivos `.base` después de una operación aprobada.

## Cambios que requieren borrador y aprobación

Mostrar el contenido y las acciones estructurales antes de:

- crear o borrar una nota;
- reescribir contenido;
- cambiar frontmatter o `temas`;
- agregar o modificar evidencia de aprendizaje;
- elegir links o razones para notas relacionadas;
- dividir o fusionar notas;
- mover o renombrar archivos;
- crear o cambiar una materia, template o archivo `.base`.

Después de crear, mover, renombrar o borrar una nota, revisar las bases afectadas y los links relacionados.

## Convenciones incompletas o en conflicto

- Algunas notas heredadas no cumplen el frontmatter mínimo.
- Algunas no tienen bloque de notas relacionadas.
- `Created` y `Cuatri` tienen formatos históricos distintos.
- Los filtros `.base` alternan links con y sin el prefijo `Categories/`.
- El template tiene incompleto el encabezado de misma materia.
- No existe una propiedad local para intención, desarrollo o aprendizaje.
- No existe un objeto local dedicado a capturas.
- El skill `obsidian-markdown`, previsto por los BP Vault Skills, no está instalado. Hasta instalarlo, deben seguirse las convenciones explícitas de estas guías y contratos.

Estas inconsistencias se documentan. No autorizan una migración ni una limpieza masiva.

<!-- bp-vault-skills:end -->
