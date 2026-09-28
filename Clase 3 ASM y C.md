---
temas: []
Cuatri: 2do-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
No puede ir C y ASM en un mismo archivo, se tiuenen que compilar por separado y luego linkeditarlos juntos

![](Attachments/image%20261.png)

las cosas se retornan por la pila y los registros

## Repaso de la pila

![](Attachments/image%20262.png)

la pila crece hacia abajo, las direcciones mas bajas arriba

![](Attachments/image%20263.png)

cuando se hace push se decrementa el valor del ESP (stack pointer) y termina apuntando a valor agregado

![](Attachments/image%20264.png)

el pop es al reves y guardo el valor en el registrto asignado

## Instrucción RET

![](Attachments/image%20265.png)

salta a la posición salta a la posición del stack pinter.

## Instrucción CALL

el call pushea la dirección de la proxima instrucción después del call

![](Attachments/image%20266.png)

# Analisis de C

![](Attachments/image%20267.png)

## Pasaje de Argumentos en C

![](Attachments/image%20268.png)

![](Attachments/image%20269.png)

![](Attachments/image%20270.png)

### Pasaje de argumentos por la pila

![](Attachments/image%20271.png)

![](Attachments/image%20272.png)

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

![](Attachments/image%20273.png)

se mandan en pila y el valor de retorno se devuelve en eax si es mayor a eax se retorna usando edx:eax

## Llamadas de ASM a C

hay que declararla como externa `extern` donde el linkeditador resuelve

![](Attachments/image%20274.png)

## Llamadas de C a ASM

![](Attachments/image%20275.png)

## Inline Assembler (No lo usamos)

![](Attachments/image%20276.png)

## Salidas en ASM

![](Attachments/image%20277.png)

el -S lo compila y lo deja en ASM

### Ejemplo ASM 32 bits

![](Attachments/image%20278.png)

![](Attachments/image%20279.png)

el `mov [ebp-4], 10`  es la asignación de la variable numero, **NO** la declaración, la declaración se hizo cuando se guardo el lugar en el stack (`sub esp, 8`)

![](Attachments/image%20280.png)

### Ejemplo ASM 64 bits

![](Attachments/image%20281.png)

se cambio el ESP y EBP por RSP y RBP

y se cambio el push por guardados en los registros (esi, edi)

## Canary

el canary va entre la dirección de retorno y el EBP y antes de retornar llama a una función para chequear que el canary no se haya modificado. si cambio detiene la ejecución del programax

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Clase 2 ASM intel](Clase%202%20ASM%20intel.md) — clase anterior
- [Clase 4 Intro transmisión Digital](Clase%204%20Intro%20transmisión%20Digital.md) — clase siguiente
- [ASM y C](ASM%20y%20C.md) — misma temática
- [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — cómo se ve la pila desde ASM

**Otras materias**

- **PI**  [PI - Funciones en C](PI%20-%20Funciones%20en%20C.md) — convención de llamada de funciones
- **PI**  [PI - Intro C](PI%20-%20Intro%20C.md) — el C que se compila a ASM

<!-- notas-relacionadas:fin -->
