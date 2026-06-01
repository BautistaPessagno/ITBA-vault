---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
### Objetivos

- Proveer recursos a las apps
- Administrar recursos

### Problemas

- Memoria insuficiente
- Fragmentación de memoria
- Seguridad

### ¿Cómo?

Abstracción (mostrar un ambiente no físico)

Interposición (interceptar acciones)

# Memoria Virtual

![[Captura_de_pantalla_2025-05-20_a_la(s)_10.45.04.png]]

muchas direcciones virtuales y pocas fisica (mucho para dar y poco para recibir)

se le agrego la [MMU](/1f2f5e1c86fe80598e5ac5e320b0c1ab#1f2f5e1c86fe805a8e12e64950c51fc2),  en el medio hay un mapeo

## Direccionamiento Fisico

![[Captura_de_pantalla_2025-05-20_a_la(s)_10.48.21.png]]

![[Captura_de_pantalla_2025-05-20_a_la(s)_11.41.46.png]]

## MMU - Unidad de Maneja de memoria

Permite:

- Dividir en páginas ó segmentos la memoria
- Chequear permisos
- Alterar (o no) la dirección “lógica” antes que sea “física”

## Direccionamiento virtual

![[Captura_de_pantalla_2025-05-20_a_la(s)_11.42.31.png]]

![[Captura_de_pantalla_2025-05-20_a_la(s)_11.42.41.png]]

proceso de mapeo para guardar todo en paginas de tamaño predefinido

# Paginación

- Divide al mapa de memoria y a la memoria física en “páginas”
- Cada página tiene un tamaño fijo.
- Genera mapeo de Pag-Virtuales (páginas) a Pag-Fisica (marcos)
- T iene esquema de permisos

podes tenes bastantes mapas que no se van a guardar una a continuacion de la otra

![[Captura_de_pantalla_2025-05-20_a_la(s)_11.44.28.png]]

## Ejercicio ejemplo 

![[Captura_de_pantalla_2025-05-20_a_la(s)_11.51.19.png]]

1. Se puede pensar un promedio del largo de los procesos
o se puede crear la pagina pensando en el proceso mas chico y luego los procesos mas grandes usan mas paginas
se busca un intermedio entre tamaño pagina y cantidad paginas
2. si uso un tamaño de pagina de 4k entonces la cantidad de paginas es 4GB/4k
3. se reparten las paginas que necesitan los procesos y en el momento del proceso se fija si el espacio es suficiente 
4. Ejemplo Valles
![[Captura_de_pantalla_2025-06-05_a_la(s)_11.00.27.png]]

## Elección de Intel para 32 bits

![[Captura_de_pantalla_2025-05-20_a_la(s)_12.02.05.png]]

el directorio hace de indice agrupando paginas por los n indices significativos

se tienen dos indices 

![[Captura_de_pantalla_2025-05-20_a_la(s)_12.04.39.png]]

![[Captura_de_pantalla_2025-05-20_a_la(s)_12.05.22.png]]

![[Captura_de_pantalla_2025-05-20_a_la(s)_12.16.59.png]]

## Ejemplo Valles

![[Captura_de_pantalla_2025-06-05_a_la(s)_11.11.47.png]]

# OBS FINAL 2025 VALLES

al hacer tener un mapa de memoria virtual, al acceder a elemento de memoria en codigo assembler, se accede desde la memoria virtual <u>**NO desde la fisica**</u>

El tamaño del offset se puede calcular con el tamaño de las paginas, ya que el tamaño 

de estas es el salto entre pagina y pagina y entre medio esta el offset

ej1: paginacion de $4\text{kb}\Rightarrow 2^2*2^{10}=2^{12} => \text{offset de 12 bits}$

ej final 2025: paginas de $1kb \Rightarrow 2^{10}\Rightarrow \text{offset de 10 bits}$

los bits para los directorios se calcula con el restante

ej1: en este caso se supone arquitectura intel de 32 bits, entones quedan 20 bits los cuales se reparten 10 y 10

ej2025: al tener **un solo nivel de indexación**, solo tiene un directorio(sin tabla de paginación),y el procesador es de 24 bits, entonces quedan 14 bits los cuales son para el directorio
