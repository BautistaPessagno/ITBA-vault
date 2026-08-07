---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
## Compilar sin Proteccion del Stack en C

```bash
gcc detalle3.c –o detalle3 -fno-stack-protector
```

# SSP(stack smashing protector)

- Lo implementa gcc
- Utiliza la función __stack_chk_fail
- Ubica un valor entre EBP y las variables locales que se lo
denomina CANARY
- Antes de retornar verifica que el CANARY no haya sido
modificado
- Si lo fue termina la ejecución
![[Captura_de_pantalla_2025-04-01_a_la(s)_12.31.59.png]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [[Assembler de Intel]] — push/pop y registros EBP/ESP
- [[Clase 3 ASM y C]] — clase de ASM y C

**Otras materias**

- **EDA**  [[EDA - Stack]] — la pila como estructura de datos
- **PI**  [[PI - Funciones en C]] — convención de llamada
- **PI**  [[PI - Punteros en C]] — punteros a variables locales
- **PI**  [[PI - Recursividad en C]] — cada llamada recursiva es un stack frame
- **SO**  [[Procesos]] — el stack dentro del espacio de direcciones del proceso

<!-- notas-relacionadas:fin -->
