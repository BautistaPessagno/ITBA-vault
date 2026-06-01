---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
[grok.com](https://grok.com/share/bGVnYWN5_aeb1da6a-afec-400f-9d8a-0cc51916512c)

el modo protegido evita que entras a lugares que no deberias

# Modo protegido-Conmutación de tareas

el sistema operativo(SO) es el primer programa que se ejecuta (no contamos el BIOS)

vas moviendo los datos de disco a memoria

de ahi nace el concepto de memoria virtual

![[Captura_de_pantalla_2025-05-13_a_la(s)_10.53.43.png]]

![[Captura_de_pantalla_2025-05-13_a_la(s)_11.00.52.png]]

La protección de tareas previene interferencias, usando aislamiento de memoria y niveles de privilegio. El PDF en la página 4 pregunta "Como puede evitar el SO este acceso?", indicando que el sistema operativo usa segmentación y paginación para restringir accesos. Hay cuatro anillos (0 3), con 0 para el kernel y 3 para aplicaciones, detallado en la página 13 con "DPL (Nivel de privilegio)". Esto evita que un programa dañe memoria de otra tarea o del kernel, como se menciona en Modo protegido.

# MMU

![[Captura_de_pantalla_2025-05-13_a_la(s)_11.56.37.png]]

- **Unidad de segmentacion:** todods los segmentos son de tamaño variable y se le asigna lo que requiera a cada proceso. No se puede deshabilitar. vendira a ser la GDT
- **Unidad de paginación: si se puede deshabilitar **entonces la dirección lineal == dirección fisica. la unidad de paginacion vuelve a mapear la direccion
- **Memoria fisica**

# Direccion Logica

![[Captura_de_pantalla_2025-05-13_a_la(s)_12.06.03.png]]

## Dirección Logica a lineal

![[Captura_de_pantalla_2025-05-13_a_la(s)_12.06.20.png]]

## GDT y LDT

![[Captura_de_pantalla_2025-05-13_a_la(s)_12.12.51.png]]

usa solo el GDT ya que se paso a usar paginacion

todo lo que esta en memoria tiene que estar descrito por la GDT

## Descriptores

describen un segmento en memoria, donde inicia y donde termina

![[Captura_de_pantalla_2025-05-13_a_la(s)_12.13.15.png]]

### Descriptores del segmento

![[Captura_de_pantalla_2025-05-13_a_la(s)_12.16.42.png]]

si es ejecutable pongo un 1 en la e si no pongo un 0

### Atributos

- **P** (Bit de presencia)
    - Si P=1 el segmento esta cargado en la memoria física.
    - Si P=0 es segmento esta ausente.
- **DPL** (Nivel de privilegio)
    - Indica el nivel de privilegio del segmento
    - 0 = máximo privilegio, 3=minimo privilegio
    - en 00 estaria el kernel, luego en 11 metieron mas user
    - hoy en dia se usan 2
- **S** (tipo de segmento)
    - Si S=1 se trata de un segmento normal (código datos o pila)
    - Si S=0 se trata de un segmento de sistema (puerta de llamada, TSS, etc).
- **G** es el bit de granuladidad
    - si es 1 el limite se multiplica x4
    - si es 0 no
- en el **TYPE** se define se el segmento es ejecutable o no (E → 1 exe y 0 no)
    - si es W se puede escribir

## Dirección lineal

![[Captura_de_pantalla_2025-06-05_a_la(s)_10.15.53.png]]

# Carga de IDT

## Interrupciones en PRE_TPE

El código necesario para cargar su propia interrupción sólo debería involucrar:

1. Definir la rutina de atención de interrupción
2. Cargar la interrupción dentro de load_idt invocando a setup_IDT_entry
pasandole el número de la interrupción y el puntero a la rutina de atención
de interrupción
3. Si la interrupción es de hardware, dicha rutina debe enviar el EOI y deben
habilitar el IRQ con la máscara correspondiente
