---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
trae bloques de la memoria principal para tenerlos a mano

Los programas se ejecutan en pasos secuenciales
Las variables se alojan en zonas adyacentes

![](Attachments/Captura_de_pantalla_2025-05-27_a_la%28s%29_11.03.20.png)

![](Attachments/Captura_de_pantalla_2025-05-27_a_la%28s%29_11.20.21.png)

[https://docs.google.com/file/d/1OuDzy262jvp52JOsGNaZYoMpdaIMtfSW/preview](https://docs.google.com/file/d/1OuDzy262jvp52JOsGNaZYoMpdaIMtfSW/preview)

## Estructura

Se compone de:

- Memoria de datos
- Memoria de etiquetas
- Controlador
    - Selecciona cuantos y cuales bytes se copian a la memoria de
datos. Utiliza diferentes algoritmos.
- El controlador ve a la RAM en bloques de tamaño fijo
- Por ejemplo de 32 bytes
- Entonces RAM de 1MB son 32768 bloques
![](Attachments/Captura_de_pantalla_2025-05-27_a_la%28s%29_11.22.17.png)

## Memoria Cache ejemplos

- Suponemos RAM de 1 MB (1M x 8).
- Bloques de 32 bytes.
- Caché de 4 K para datos (sin etiquetas)
![](Attachments/Captura_de_pantalla_2025-05-27_a_la%28s%29_11.23.05.png)
por lo tanto:
$$
\frac{cache}{bloque}= etiquetas
$$
![](Attachments/Captura_de_pantalla_2025-05-27_a_la%28s%29_11.23.11.png)
$$
\frac{mem\_fisica}{tamaño\_bloques}=2^{bits\_etiquetas}
$$

## Tipos de Mapeo

### Directo

un bloque de memoria solo se puede mapear a un unica ranura de cache

### Asociativo

se puede mapear a cualquier ranura

es la que se usa actualmente

## Políticas de sustitución

Se actualiza la cache al haber fallo o ausencia de
palabra buscada.

- First In First Out (FIFO)
- Least Recently Used (LRU) (necesita flag !)
- Random

## Escritura inmediata vs obligada

![](Attachments/Captura_de_pantalla_2025-06-05_a_la%28s%29_12.58.29.png)

hoy en dia se usa mas que nada la obligada

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Cache](Cache.md) — nota corta de caché
- [Clase 4 Intro transmisión Digital](Clase%204%20Intro%20transmisión%20Digital.md) — clasificación de memorias: la SRAM es la que implementa la caché
- [Intro Sistemas Operativos(Paginación)](Intro%20Sistemas%20Operativos%28Paginación%29.md) — TLB: caché de traducciones

**Otras materias**

- **SO**  [Memoria](Memoria.md) — gestión de memoria desde el SO

<!-- notas-relacionadas:fin -->
