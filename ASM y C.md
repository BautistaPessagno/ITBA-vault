---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
Terminar de entender el Stack

[wtf is “the stack” ?](https://www.youtube.com/watch?v=CRTR5ljBjPM&t=78s)

# Mezcla de lenguajes en un ejecutable

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.43.07.png)

## Pila

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.43.20.png)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.45.25.png)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.45.35.png)

# Instruccion RET

Cuando se ejecuta una instrucción RET, el procesador toma el contenido
de los apuntado por ESP y salta a esa posición de memoria

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.46.22.png)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.46.44.png)

# Pasaje de argumentos en C

- Según la arquitectura el compilador pasa de manera diferentes los argumentos de las funciones
- Arquitectura de 32 bits
    - Se pasan por la pila
- Arquitectura de 64 bits
    - Se pasan primero por registros y luego por la pila

## Pasaje de argumentos por registros

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_11.48.24.png)

## Convenciones en C

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_12.03.58.png)

## Armado Stack 

```assembly
	;armado stack
	push ebp
	mov ebp, esp
	
	;desarmado stack
	mov esp, ebp
	pop ebp
```

## Resguardo y actualización EBP y Valores a Retornar(IMPORTANTE)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_12.04.04.png)

## Llamadas

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_12.11.42.png)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_12.11.48.png)

![](Attachments/Captura_de_pantalla_2025-03-31_a_la%28s%29_11.17.51.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Clase 3 ASM y C](Clase%203%20ASM%20y%20C.md) — clase de esta temática
- [Assembler de Intel](Assembler%20de%20Intel.md) — referencia de instrucciones
- [Codigos Assembler](Codigos%20Assembler.md) — ejemplos

**Otras materias**

- **PI**  [PI - Funciones en C](PI%20-%20Funciones%20en%20C.md) — pasaje de parámetros
- **PI**  [PI - Intro C](PI%20-%20Intro%20C.md) — el lado C de la interoperación

<!-- notas-relacionadas:fin -->
