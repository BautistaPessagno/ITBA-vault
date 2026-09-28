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
![](Attachments/Captura_de_pantalla_2025-04-01_a_la%28s%29_12.31.59.png)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Assembler de Intel](Assembler%20de%20Intel.md) — push/pop y registros EBP/ESP
- [Clase 3 ASM y C](Clase%203%20ASM%20y%20C.md) — clase de ASM y C

**Otras materias**

- **EDA**  [EDA - Stack](EDA%20-%20Stack.md) — la pila como estructura de datos
- **PI**  [PI - Funciones en C](PI%20-%20Funciones%20en%20C.md) — convención de llamada
- **PI**  [PI - Punteros en C](PI%20-%20Punteros%20en%20C.md) — punteros a variables locales
- **PI**  [PI - Recursividad en C](PI%20-%20Recursividad%20en%20C.md) — cada llamada recursiva es un stack frame
- **SO**  [Procesos](Procesos.md) — el stack dentro del espacio de direcciones del proceso

<!-- notas-relacionadas:fin -->
