---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
No puede ir C y ASM en un mismo archivo, se tiuenen que compilar por separado y luego linkeditarlos juntos

![[image 261.png]]

las cosas se retornan por la pila y los registros

## Repaso de la pila

![[image 262.png]]

la pila crece hacia abajo, las direcciones mas bajas arriba

![[image 263.png]]

cuando se hace push se decrementa el valor del ESP (stack pointer) y termina apuntando a valor agregado

![[image 264.png]]

el pop es al reves y guardo el valor en el registrto asignado

## Instrucción RET

![[image 265.png]]

salta a la posición salta a la posición del stack pinter.

## Instrucción CALL

el call pushea la dirección de la proxima instrucción después del call

![[image 266.png]]

# Analisis de C

![[image 267.png]]

## Pasaje de Argumentos en C

![[image 268.png]]

![[image 269.png]]

![[image 270.png]]

### Pasaje de argumentos por la pila

![[image 271.png]]

![[image 272.png]]

# Armado/desarmado stack

```assembly
;armado stack
	push ebp
	mov ebp, esp
	
	;desarmado stack
	mov esp, ebp
	pop ebp
```

# Convenciones en C

![[image 273.png]]

se mandan en pila y el valor de retorno se devuelve en eax si es mayor a eax se retorna usando edx:eax

## Llamadas de ASM a C

hay que declararla como externa `extern` donde el linkeditador resuelve

![[image 274.png]]

## Llamadas de C a ASM

![[image 275.png]]

## Inline Assembler (No lo usamos)

![[image 276.png]]

## Salidas en ASM

![[image 277.png]]

el -S lo compila y lo deja en ASM

### Ejemplo ASM 32 bits

![[image 278.png]]

![[image 279.png]]

el `mov [ebp-4], 10`  es la asignación de la variable numero, **NO** la declaración, la declaración se hizo cuando se guardo el lugar en el stack (`sub esp, 8`)

![[image 280.png]]

### Ejemplo ASM 64 bits

![[image 281.png]]

se cambio el ESP y EBP por RSP y RBP

y se cambio el push por guardados en los registros (esi, edi)

## Canary

el canary va entre la dirección de retorno y el EBP y antes de retornar llama a una función para chequear que el canary no se haya modificado. si cambio detiene la ejecución del programax