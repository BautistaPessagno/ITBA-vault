# ITBA Vault

Apuntes de Ingeniería Informática en el [ITBA](https://www.itba.edu.ar/): notas de clase,
prácticas, resúmenes para parciales y finales, y guías de ejercicios resueltas.

Es un vault de [Obsidian](https://obsidian.md/), pero las notas son Markdown común: se
pueden leer directo en GitHub o en cualquier editor.

> Son apuntes personales, escritos mientras cursaba. Pueden tener errores o estar
> incompletos, y reflejan cómo dio cada tema *esa* cátedra en *ese* cuatrimestre. Usalos
> como complemento, no como reemplazo de la bibliografía oficial.

## Materias

Cada materia tiene una nota índice. Arrancá por ahí.

| Materia | Cursada | Temas |
|---|---|---|
| [Matemática Discreta](Materia%20-%20Discrete%20Math.md) | 1C 2024 | grafos, caminos, árboles, planaridad, coloreo |
| [Programación Orientada a Objetos](Materia%20-%20POO.md) | 2C 2024 | Java, herencia, generics, colecciones, streams, Ruby |
| [Química](Materia%20-%20Quimica.md) | 2C 2024 | uniones, cinética, equilibrio, buffers, titulación, electroquímica |
| [Arquitectura de Computadoras](Materia%20-%20Arqui.md) | 1C 2025 | assembler x86, ASM y C, interrupciones, caché, paginación, ARM |
| [Estructuras de Datos y Algoritmos](Materia%20-%20EDA.md) | 1C 2025 | complejidad, árboles, grafos, hashing, técnicas algorítmicas |
| [Bases de Datos](Materia%20-%20BD.md) | 2C 2025 | E/R, álgebra relacional, SQL, normalización, triggers |
| [Interacción Humano-Computadora](Materia%20-%20HCI.md) | 2C 2025 | diseño centrado en el usuario, usabilidad, heurísticas |
| [Sistemas Operativos](Materia%20-%20SO.md) | 2C 2025 | procesos, scheduling, IPC, memoria, file system |
| [Métodos Numéricos](Materia%20-%20MNA.md) | 1C 2026 | álgebra lineal, diagonalización, LU/QR/SVD, cuadrados mínimos |
| [Programación Imperativa](Materia%20-%20PI.md) | 1C 2026 | C, punteros, listas, recursividad, TADs |
| [Protocolos de Comunicación](Materia%20-%20Protos.md) | 1C 2026 | HTTP, DNS, mail, TCP/UDP, IP, routing, SSH, sockets |
| [Teoría de Lenguajes y Autómatas](Materia%20-%20TLA.md) | 1C 2026 | autómatas, gramáticas, parsing LL/LR, Turing |
| [Criptografía y Seguridad](Materia%20-%20Criptografía%20y%20Seguridad.md) | 2C 2026 | cifrado simétrico y asimétrico, MACs, firma digital |
| [Derecho](Materia%20-%20Derecho.md) | 2C 2026 | fuentes, persona, obligaciones, responsabilidad civil, derecho comercial, marcas y patentes |
| [Economía](Materia%20-%20Economia.md) | 2C 2026 | microeconomía, oferta y demanda, elasticidad, mercados |
| [Machine Learning](Materia%20-%20Machine%20Learning.md) | 2C 2026 | supervisado, KNN/SVM/árboles, bias-variance, no supervisado |
| [Proyecto de Aplicaciones Web](Materia%20-%20PAW.md) | 2C 2026 | Spring, Maven, arquitectura web |

Las materias de 2C 2026 están en curso, así que todavía se van completando.

## Cómo navegarlo

**En Obsidian (recomendado).** Cloná el repo y abrí la carpeta como vault
(*Open folder as vault*). Así tenés:

- las vistas de `Categories/*.base`, que listan las notas de cada materia (hace falta
  Obsidian 1.9 o posterior),
- el grafo y los backlinks,
- los dibujos de Excalidraw y el canvas renderizados.

**En GitHub.** Todo se lee bien salvo los `.base`, los dibujos de Excalidraw y el
canvas, que son formatos propios de Obsidian.

### Estructura

La organización es plana: todas las notas viven en la raíz y se agrupan por las
propiedades del frontmatter y por links, no por carpetas.

| Carpeta | Contenido |
|---|---|
| raíz | todas las notas de contenido |
| `Categories/` | una vista `.base` por materia, más `ITBA.base` (todo) y `Materias.base` (índice) |
| `Attachments/` | imágenes y capturas que usan las notas |
| `Excalidraw/` | dibujos (plugin Excalidraw) |
| `Templates/` | template para notas nuevas |

Cada nota declara su materia en el frontmatter y termina con un bloque de **notas
relacionadas**: links a otras notas de la misma materia y, sobre todo, a conceptos que
reaparecen en otra materia (por ejemplo, el grafo completo de Discreta en la distribución
de claves de Cripto). Son buenos puntos de entrada para repasar un tema desde otro ángulo.

## Plugins

El vault funciona sin plugins. El repo guarda su configuración pero no su código, así
que la primera vez Obsidian los va a mostrar como faltantes: instalalos desde
*Settings → Community plugins* si los querés. Todos son opcionales:

- [Excalidraw](https://github.com/zsviska/obsidian-excalidraw-plugin), para ver y editar los dibujos.
- Calendar, File Color e Iconize, solo estéticos.

## Contribuir

Si encontrás un error, abrí un issue o un PR. Las convenciones del vault (frontmatter,
links, bloque de notas relacionadas) están en [CLAUDE.md](CLAUDE.md).
