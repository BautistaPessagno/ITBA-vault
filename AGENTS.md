# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with this vault.

## Agent Instructions

When answering questions of a theoretical, academic, or technical nature (protocols, computer science, databases, compilers, OS, etc.), always search the vault first for relevant notes or summaries before answering from training data.

## What This Is

An Obsidian vault dedicated to **ITBA university course notes**. Courses covered include:

- **Protos** — Protocolos de Comunicación (HTTP, DNS, TCP/IP, routing, SSH, sockets)
- **BD** — Bases de Datos (entity-relation model, SQL, normalization)
- **TLA** — Teoría de Lenguajes y Autómatas (regular languages, automata, grammars, Turing machines)
- **Arqui** — Arquitectura de Computadoras (assembler, x86, cache, memory, I/O)
- **SO** — Sistemas Operativos (processes, scheduling, memory management, file systems)
- **Química** — Química General
- **MNA** — Métodos Numéricos y Análisis

## Vault Organization

**Flat, bottom-up structure** (Steph Ango style) — notes live at vault root, never in content subfolders.

- Organization is via the `categories` frontmatter property (multitext), not folders
- `.base` files at vault root provide per-course browsable views
- `Categories/ITBA.base` shows all ITBA notes

### Base Files

| File | Course |
|------|--------|
| `protos.base` | Protocolos de Comunicación |
| `BD.base` | Bases de Datos |
| `TLA.base` | Teoría de Lenguajes y Autómatas |
| `arqui.base` | Arquitectura |
| `SO.base` | Sistemas Operativos |
| `Quimica.base` | Química |
| `MNA.base` | Métodos Numéricos |
| `OSI.base` | OSI model (cross-subject) |
| `Polaridad.base` | Polaridad (Química) |
| `Unión Covalente.base` | Unión Covalente (Química) |
| `Categories/ITBA.base` | All ITBA notes |

### Admin Folders (not for content notes)

`_Claude/`, `Attachments/`, `Excalidraw/`, `Templates/`

## Creating Notes

1. **Always place at vault root** — never in subfolders
2. **Always include `categories:` in frontmatter** referencing `ITBA.base` plus the subject base:

```yaml
---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: Protos
temas:
  - HTTP
---
```

3. **Use the `Nota de Clase` template** from `Templates/` (has Materia, temas fields)
4. **Write content in Spanish** unless the existing note is already in English
5. After creating a note, link to the relevant course `.base` in the body so it appears in that course view

## Frontmatter Format

```yaml
---
categories:
  - "[[ITBA.base|ITBA]]"
Materia: <course name>
temas:
  - <topic>
---
```

## .base Files

Base files use Obsidian Bases format. `Categories/ITBA.base` filters with `file.hasLink("ITBA.base")`. Course bases (e.g. `protos.base`) filter with `file.hasLink("protos.base")`.

## Auto-update

After creating, modifying, or deleting notes, update any affected `.base` files or category references if the change impacts vault organization.
