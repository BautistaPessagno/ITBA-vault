---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-01
Materia: "[[PI.base|PI]]"
temas:
  - Funciones
  - Pasaje por valor
  - Modularización
  - Top-Down
  - Bottom-Up
---
# PI — Funciones en C

## Resumen

### Definición de Funciones
```c
tipoRetorno nombreFuncion(tipo param1, tipo param2, ...) {
    // cuerpo
    return valor;
}
```

**Prototipo** (declaración previa, necesaria si la función se usa antes de su definición):
```c
double calcular(int a, double b);  // en el .h o al inicio del .c
```

### Pasaje de Parámetros — Siempre por VALOR
En C, los parámetros se pasan **siempre por valor**: se crea una copia local.  
Los cambios en la copia **no afectan** a la variable original.

```c
void incrementar(int n) {
    n++;       // modifica la copia, no el original
}

int main(void) {
    int x = 5;
    incrementar(x);
    // x sigue siendo 5
}
```

Para modificar el original → usar **punteros** (`&` y `*`).

### Reglas de Diseño de Funciones
1. **Una función = una tarea**. Si es difícil elegir un nombre claro → la función hace demasiado o muy poco.
2. Cada función debe tener un **prólogo** (comentario) que describa qué hace y cómo usarla.
3. **Programación defensiva**: no presuponer que algo "nunca va a ocurrir".
4. Someter cada función a pruebas (caja blanca y caja negra).

### Modularización
Estrategia de descomposición del problema en módulos (funciones/archivos).

**Top-Down:** empezar con el programa principal y luego crear las funciones.  
**Bottom-Up:** empezar con las funciones auxiliares y luego el programa principal.

Cada módulo debe tener:
- **Alta cohesión**: los elementos del módulo están relacionados entre sí.
- **Bajo acoplamiento**: el módulo depende poco de otros módulos.

### Compilación Multi-archivo
```bash
gcc -c modulo1.c    # genera modulo1.o
gcc -c modulo2.c    # genera modulo2.o
gcc modulo1.o modulo2.o -o programa   # linkedición
```

## Notas
- Las funciones en C tienen visibilidad global por defecto. Usar `static` para limitar la visibilidad al archivo.
- El prototipo en el `.h` permite que varios `.c` usen la misma función sin redefinirla.

## Preguntas
- ¿Por qué en C el pasaje es siempre por valor?
- ¿Cómo se logra que una función modifique una variable del llamador?
- ¿Cuándo conviene usar Top-Down vs Bottom-Up?

[PI](Categories/PI.base)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (PI)**

- [PI - Intro C](PI%20-%20Intro%20C.md) — tema anterior
- [PI - Recursividad en C](PI%20-%20Recursividad%20en%20C.md) — tema siguiente

**Otras materias**

- **Arqui**  [ASM y C](ASM%20y%20C.md) — convención de llamada
- **Arqui**  [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — cómo se ve una llamada en la pila
- **BD**  [BD clase 13 SQL PSM](BD%20clase%2013%20SQL%20PSM.md) — funciones y procedimientos en SQL

<!-- notas-relacionadas:fin -->
