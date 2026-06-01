---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - intro
Created: 2026-03-0309:01
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - DBMS
  - Modelo de Datos
  - Arquitectura tres niveles
---
# BD clase 1 — Características de un DBMS

## Resumen

Una **Base de Datos** es una colección de datos relacionados que: representa algún aspecto del mundo real, tiene coherencia lógica y fue diseñada para un propósito específico. Un **DBMS** (Database Management System) es el software que permite crear y mantener esa base de datos.

### Diferencias DBMS vs. Sistema de Archivos Tradicional

| Característica | Sistema de Archivos | DBMS |
|---|---|---|
| Autodescripción | Definición en el código | Catálogo (meta-data) |
| Independencia | Estructura acoplada al programa | Estructura en el catálogo |
| Vistas | Difícil de gestionar | Views por grupo de usuarios |
| Integridad | Validaciones en cada programa | Restricciones en el esquema |
| Concurrencia | Compleja de coordinar | Manejada automáticamente |
| Atomicidad | Muy difícil de garantizar | Procesamiento transaccional |

> [!important] Las más importantes
> Las dos características que más distinguen a un DBMS de un sistema de archivos son el **manejo de la concurrencia** y el **procesamiento transaccional (atomicidad)**.

### Modelo de Datos

Conjunto de conceptos para describir la estructura de la BD (tipos de datos, relaciones, restricciones). Tres niveles de abstracción:

- **Conceptual / alto nivel**: cómo perciben los datos los usuarios.
  - *Basado en entidades*: usa entidad, atributo y relación → **Modelo Entidad-Relación** (el que vemos en la materia).
- **Representación / lógico**: usa estructuras de registro.
  - Red (CODASYL, años '60–'70)
  - Jerárquico (IMS de IBM, '60s)
  - **Relacional** (Codd/IBM, 1970) — el más común hoy: Oracle, PostgreSQL, MySQL, etc.
- **Físico / bajo nivel**: detalles de almacenamiento (formato, ordenamiento, estructuras de acceso).

### Meta-Data vs. Datos

| Concepto | También llamado | Descripción |
|---|---|---|
| Esquema | Meta-data / Catálogo / Intensión | Descripción de la BD, cambia poco |
| Datos | Extensión | Estado actual, cambia frecuentemente |

El DBMS garantiza que todos los estados sean válidos respecto al esquema.

### Arquitectura de Tres Niveles (ANSI/SPARC)

```
┌─────────────────────────────────────────┐
│  Nivel Externo  (vistas por grupo)      │
├─────────────────────────────────────────┤
│  Nivel Conceptual  (estructura global)  │
├─────────────────────────────────────────┤
│  Nivel Interno  (almacenamiento físico) │
└─────────────────────────────────────────┘
```

- **Independencia física**: cambiar el esquema interno no afecta el conceptual ni el externo.
- **Independencia lógica**: cambiar el esquema conceptual no afecta las vistas externas.
- Los datos *solo existen realmente* en el nivel físico; los otros son descripciones.

### Perfiles de usuarios

**Proveedores de DBMS:**
- *Implementador*: desarrolla el DBMS.
- *Desarrollador de herramientas*: crea herramientas que trabajan sobre DBMS existentes.

**Dentro de una organización:**
- *DBA (Administrador)*: accesos, monitoreo, backups.
- *Diseñador*: define estructura y restricciones según requerimientos.
- *Usuarios*: comunes (usan apps), programadores (hacen las apps), ocasionales (consultas únicas).

### Lenguajes

| Sigla | Nombre | Uso |
|---|---|---|
| DDL | Data Definition Language | Define esquema conceptual |
| SDL | Storage Definition Language | Define esquema interno |
| VDL | View Definition Language | Define vistas externas |
| DML | Data Manipulation Language | Insertar, modificar, eliminar datos |
| **SQL** | Structured Query Language | **Unifica todos los anteriores** |

- **Query language**: DML usado interactivamente desde terminal → procesado por el *query compiler*.
- **DML embebido**: sentencias DML dentro de código host (C, Java) → procesadas por un *precompiler*.
- **ODBC**: API genérico que permite conectarse a múltiples DBMS sin recompilar (a costa de performance).

### Arquitectura Cliente/Servidor

- **Centralizada (2-tier)**: DBMS y BD en un servidor; clientes en otras máquinas.
- **Distribuida homogénea**: mismo software DBMS en varios servidores (escalado).
- **Distribuida heterogénea**: distintos software DBMS interconectados formando un gran sistema.

---

## Notas

- Excel **no es** una base de datos: no permite perfiles de usuario ni restricciones de acceso, y falla en eficiencia con grandes volúmenes.
- Un DBMS es **software de base**, al nivel de un Sistema Operativo o Compilador: no tiene sentido que cada programador construya el suyo.
- El catálogo es consultable también por usuarios que quieran conocer la estructura de la BD.

## Preguntas

- ¿Cuál es la diferencia entre independencia física e independencia lógica?
- ¿Por qué ODBC sacrifica performance a cambio de portabilidad?
- ¿En qué situación conviene una arquitectura distribuida heterogénea vs. homogénea?


