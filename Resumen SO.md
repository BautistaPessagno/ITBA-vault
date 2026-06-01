---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[SO.base|Sistemas Operativos]]"
temas:
  - Procesos
  - Scheduling
  - Threads
  - IPC
  - Sincronización
  - Memory Management
  - File System
  - SysCalls
---
# Resumen — Sistemas Operativos

## Procesos

**Proceso:** abstracción de un programa en ejecución. Tiene su propio espacio de memoria (código, heap, stack).  
**Programa:** archivo en disco; no hace nada por sí solo.

### Creación (Unix)
- `fork()` → crea un proceso hijo (copia del padre).
- `execve()` → reemplaza la imagen del proceso hijo con un nuevo programa.
- El proceso **init** (PID 1) es padre de todos en Unix.

### Estados de un Proceso
```
Nuevo → Listo (Ready) → Ejecutando → Bloqueado → Terminado
```
- `Ready → Blocked` no existe directamente; un proceso debe estar corriendo para bloquearse.
- `Blocked → Running` no existe; primero pasa a Ready.

### PCB (Process Control Block)
Estructura (tabla de structs) con: estado, registros, PID, jerarquía, mapeo de memoria.

**Zombie:** proceso que terminó pero cuyo padre no hizo `waitpid`.  
**Huérfano:** proceso hijo cuyo padre murió → adoptado por init.

### Multiprogramación
Maximizar uso de CPU alternando procesos cuando uno se bloquea (I/O).

---

## Scheduling

**Scheduler:** componente del kernel que decide qué proceso ejecutar. No es un proceso.

### Tipos de Schedulers
| Tipo | Preemptive | Uso |
|---|---|---|
| **Batch** | No (generalmente) | Trabajos largos sin interacción |
| **Interactivo** | Sí | Tiempo de respuesta rápido |
| **Tiempo real** | Depende | Plazos estrictos |

### Algoritmos de Scheduling

**Batch:**
- **FCFS** (First-Come First-Served) → non-preemptive; puede perder balance.
- **SJF** (Shortest Job First) → minimiza turnaround; non-preemptive.
- **SRTN** (Shortest Remaining Time Next) → preemptive de SJF.

**Interactivo:**
- **Round-Robin** → versión preemptive de FIFO con quantum fijo.
- **Priority Scheduling** → prioridades estáticas o dinámicas; con envejecimiento.
- **Multiple Queues** → colas separadas por prioridad.
- **Lottery Scheduling** → siguiente proceso = lotería; favorece a quien tiene más tickets.
- **Fair-Share** → equidad por usuario, no por proceso.

**Turnaround:** tiempo total desde que el proceso entró hasta que salió del sistema.

---

## Threads

**Thread:** unidad de ejecución dentro de un proceso. Más liviano que un proceso.  
Comparte código, heap y datos del proceso; pero cada thread tiene su **propio stack**.

**Justificación:** crear un thread es más barato que crear un proceso; permite paralelismo dentro del mismo proceso.

### Implementaciones
| Tipo | Ventaja | Desventaja |
|---|---|---|
| **User-space** | Sin syscall para crear/cambiar | Syscall bloqueante bloquea todos los threads |
| **Kernel-space** | Syscall no bloquea los demás | Más costoso crear/destruir |
| **Híbrida** | Balance | Compleja |

**API POSIX:** `pthread_create`, `pthread_join`, `pthread_exit`.

---

## IPC (Inter-Process Communication)

### Mecanismos
| Mecanismo | Descripción |
|---|---|
| **Memoria compartida** | Un segmento de memoria accesible por múltiples procesos. Sin sincronización automática. |
| **Pasaje de mensajes** | Los procesos envían/reciben mensajes; el kernel intermedia. |
| **Pipes** | Canal de comunicación unidireccional; útil para productor-consumidor. |

### Race Condition y Sincronización

**Race Condition:** dos procesos acceden a datos compartidos sin sincronización → resultado indeterminado.

**Región Crítica:** fragmento de código que accede a recursos compartidos. Solo un proceso a la vez.

**Semáforo:**
- `down()/wait()` → decrementa; si 0 → bloquea.
- `up()/post()` → incrementa; desbloquea si hay esperando.
- `mutex`: inicializado en 1 → exclusión mutua.
- `empty`/`full`: coordinación productor-consumidor.

**Deadlock:** dos procesos se esperan mutuamente → ninguno avanza.  
Señal de alerta: `up` y `down` del mismo semáforo en lugares distintos simultáneamente.

---

## SysCalls

**SysCall:** función para pedir servicio al kernel desde espacio de usuario.  
Invocación: genera un trap → kernel toma control → ejecuta el servicio → retorna.

**POSIX comunes:**
- `fork()` → crear proceso hijo.
- `exec()` → reemplazar imagen del proceso.
- `waitpid()` → esperar fin de proceso hijo.
- `read()`, `write()` → E/S mediante file descriptors.
- `exit()` → terminar proceso.

**File Descriptors:** enteros que representan archivos/pipes/sockets abiertos (0=stdin, 1=stdout, 2=stderr).

---

## Memory Management

**Responsabilidades:** asignación exclusiva de memoria libre, liberación de memoria asignada.

**`malloc`:** no es una syscall en sí; usa syscalls como `brk`/`mmap` internamente.  
**Memoria virtual:** abstracción que mapea direcciones virtuales a físicas (MMU).

---

## File System

### Estrategias de Almacenamiento
| Estrategia | Ventaja | Desventaja |
|---|---|---|
| **Asignación continua** | Simple, acceso aleatorio | Fragmentación externa |
| **Lista enlazada (disco)** | Sin fragmentación | Sin acceso aleatorio |
| **I-nodes** | Flexible, acceso aleatorio | Límite de niveles de indirección |

### I-node (Index Node)
Metadatos del archivo: modo, UID, GID, timestamps, link count, punteros a bloques de datos.  
Un archivo puede tener múltiples nombres (hard links) → mismo i-node.

### Superblock
Estructura al inicio del sistema de archivos con toda la metadata del FS.  
Si se corrompe → se pierde el FS (pero puede reconstruirse).

### Sistemas Modernos
- **ext4** → usa *extents* (grupos lógicos de bloques continuos).
- **ZFS** → auto-reparable (puede curarse a sí mismo); integridad de datos.

---

## Notas
- El scheduler NO es un proceso; es parte del kernel.
- El proceso init (PID 1) adopta procesos huérfanos para evitar zombies perpetuos.
- Siempre inicializar semáforos antes de usarlos.

[[SO.base|Sistemas Operativos]]
