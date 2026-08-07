---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[arqui.base|Arquitectura]]"
temas:
  - Assembler
  - Intel
  - Interrupciones
  - Cache
  - Modo Protegido
  - MMU
  - Pipelines
  - ARM
---
# Resumen — Arquitectura de Computadoras

## Assembler Intel (x86)

### Sintaxis Intel vs AT&T
```assembly
mov EAX, 1        ; Intel: destino primero
movl $1, %eax     ; AT&T: fuente primero, sufijo de tamaño, % en registros
```
GCC genera AT&T por defecto.

### Modos de Direccionamiento
| Modo | Ejemplo | Descripción |
|---|---|---|
| Inmediato | `mov ax, 0ffffh` | El operando es el valor literal |
| Registro | `mov edx, eax` | Operando en registro |
| Directo | `mov ax, [57D1h]` | Operando en esa dirección de memoria |
| Indirecto | `mov ax, [bx]` | Operando en la dirección que contiene el registro |
| Indexado | `mov ax, [bx+si]` | Base + índice |

### Registros x86
- **EAX, EBX, ECX, EDX** → propósito general (32 bits).
- **ESP** → stack pointer; **EBP** → base pointer.
- **EIP** → instruction pointer.
- **FLAGS** → estado: CF (carry), ZF (zero), SF (sign), OF (overflow).

### Instrucciones Clave
```assembly
mov dest, src     ; mover/cargar
add/sub/mul/div   ; aritméticas
push/pop          ; stack
call/ret          ; subrutinas (call: push EIP + jump; ret: pop EIP)
jmp / jz / jnz    ; saltos
cmp               ; comparación (solo afecta flags)
int n             ; interrupción software
```

---

## Interrupciones

**Interrupción:** señal que interrumpe al CPU para atender un evento.

### Tipos
| Tipo | Descripción |
|---|---|
| **Hardware (INTR)** | Dispositivos externos; puede enmascararse con `cli`/`sti` (flag IF) |
| **Hardware (NMI)** | No enmascarable; usada para errores críticos (batería, temperatura) |
| **Software (INT n)** | Instrucción explícita; ej `INT 21h` |
| **Excepción (Fault/Trap/Abort)** | Generada por el propio CPU ante condición anómala |

### Flujo de Atención
1. Ocurre la interrupción.
2. CPU guarda en el stack la dirección de la siguiente instrucción.
3. CPU busca en la **IDT** (Interrupt Descriptor Table) la rutina de atención.
4. Ejecuta la rutina.
5. Retorna al programa original.

### PIC (Programmable Interrupt Controller)
Gestiona múltiples líneas de interrupción hardware (IRQs). En cascada: un PIC master + uno slave para ampliar la cantidad de IRQs.  
EOI (End Of Interrupt) debe enviarse al PIC al finalizar la rutina.

### BIOS
Al iniciar la PC, guarda en memoria rutinas básicas accesibles por interrupciones software (INT 10h, INT 13h, etc.).

---

## Memoria Cache

**Objetivo:** reducir la latencia de acceso a RAM aprovechando la **localidad** (espacial y temporal).

### Estructura
- **Memoria de datos:** bloques copiados de la RAM.
- **Memoria de etiquetas:** identifica qué bloque de RAM está en cada ranura.
- **Controlador:** maneja el mapeo y las políticas.

### Cálculo de Etiquetas
```
etiquetas = tamaño_cache / tamaño_bloque
bits_etiquetas = log₂(memoria_física / tamaño_bloque)
```

### Tipos de Mapeo
| Tipo | Descripción |
|---|---|
| **Directo** | Cada bloque de RAM → única ranura posible en cache |
| **Asociativo** | Cualquier bloque → cualquier ranura (el más usado hoy) |

### Políticas de Sustitución (al haber fallo de cache)
- **FIFO:** sale el más antiguo.
- **LRU** (Least Recently Used): sale el menos usado recientemente. Requiere flags.
- **Random:** sale uno al azar.

### Escritura
- **Inmediata (write-through):** escribe en cache Y en RAM simultáneamente.
- **Obligada (write-back):** escribe solo en cache; se actualiza la RAM al desalojar el bloque.  
  → Hoy se usa principalmente write-back.

---

## Modo Protegido

**Objetivo:** evitar que programas accedan a zonas de memoria que no les pertenecen.

### Niveles de Privilegio (Rings)
- **Ring 0:** kernel (máximo privilegio).
- **Ring 3:** aplicaciones de usuario (mínimo privilegio).
- DPL (Descriptor Privilege Level): 0=máximo, 3=mínimo.

### MMU (Memory Management Unit)
Compuesta de dos unidades:
1. **Unidad de Segmentación** → convierte dirección lógica → lineal. **No se puede deshabilitar**.
2. **Unidad de Paginación** → convierte dirección lineal → física. **Se puede deshabilitar**.

Si paginación está deshabilitada → dirección lineal == dirección física.

### GDT (Global Descriptor Table)
Tabla con descriptores de todos los segmentos de memoria activos.  
Cada descriptor incluye: base, límite, tipo (S, E, W), bit de presencia (P), nivel de privilegio (DPL), granularidad (G).

### Conmutación de Tareas
**Timer Tick:** interrupción periódica del hardware que permite al SO recuperar el control y realizar el context switch.  
El SO es una "tarea más" que tiene privilegio máximo.

---

## Pipelines y Arquitecturas Modernas

**Pipeline:** permite superponer fases de distintas instrucciones (fetch → decode → execute → writeback).  
Aumenta el throughput; puede haber *hazards* (de datos, de control).

**64 bits / x86-64:** extiende los registros a 64 bits (RAX, RBX…); amplia el espacio de direccionamiento.

**ARM:** arquitectura RISC. Instrucciones de tamaño fijo, muchos registros, menor consumo. Usada en dispositivos móviles y servidores modernos.

**GPU:** procesador masivamente paralelo; miles de cores pequeños. Ideal para cómputo vectorial/matricial.

---

## Notas
- `call` = `push IP` + `jmp`; `ret` = `pop IP`.
- La NMI nunca se puede enmascarar; siempre ejecuta la rutina en INT 2h.
- Siempre enviar EOI al PIC al finalizar una rutina de interrupción hardware.
- La unidad de segmentación no se puede deshabilitar en x86 protegido.

[[arqui.base|Arquitectura]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Clase 1 Intro]] — introducción
- [[Assembler de Intel]] — assembler
- [[Interrupciones]] — interrupciones
- [[Memoria Cache]] — caché
- [[Modo protegido]] — modo protegido
- [[Intro Sistemas Operativos(Paginación)]] — MMU y paginación
- [[Resumen Criollo (Memoria, Deco, Perifericos)]] — repaso de memoria y periféricos

<!-- notas-relacionadas:fin -->
