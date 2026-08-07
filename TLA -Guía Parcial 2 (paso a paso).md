---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-06-15"
Materia: "[[TLA.base|TLA]]"
temas:
  - Guía de resolución Parcial 2
  - Forma Normal de Greibach
  - Lema de Bombeo CFL
  - Autómatas de Pila
  - Gramáticas Libres de Contexto
  - Máquina de Turing
  - Análisis Sintáctico Descendente LL(1)
  - Análisis Sintáctico Ascendente
---
# TLA - Guía de Resolución del Parcial 2 (paso a paso)

Guía práctica armada a partir de los parciales 2 de **Autómatas, Teoría de Lenguajes y Compiladores (72.39)** entre 2016 y 2023. La idea no es repetir la teoría, sino darte un **algoritmo simple a seguir** para cada tipo de ejercicio, el **paso a paso** de cómo armarlo, y las **variaciones** que aparecen.

Materia: [[TLA.base|TLA]] · Pertenece a [[ITBA.base|ITBA]]

> [!tip] Regla de oro del parcial
> Todos los enunciados repiten la misma advertencia: *"todos los ejercicios deben resolverse utilizando los algoritmos vistos en clase"* y *"se evalúa por lo que está escrito, no por lo que se quiso poner"*. Por eso esta guía sigue los algoritmos de la cátedra y siempre te pide **escribir la definición formal completa** y **justificar cada paso**. La condición mínima de aprobación es **acumular 6 puntos**.

---

## Mapa: qué ejercicio es cada uno

Casi siempre el parcial es la misma combinación de 5 (o 4) ejercicios. Si reconocés el "tipo", ya sabés el algoritmo:

| Parcial | Ej. 1 | Ej. 2 | Ej. 3 | Ej. 4 | Ej. 5 |
|---|---|---|---|---|---|
| 2016 2Q | FNG | AP | Bombeo | MT reconocedora | MT calculadora |
| 2017 1Q | FNG | AP | Bombeo | MT reconocedora | MT calculadora |
| 2017 2Q | FNG | AP | Bombeo | LL(1) descendente | — |
| 2019 1Q | FNG | Bombeo | AP | Escribir gramática | MT serie |
| 2019 2Q | FNG | Bombeo | AP | Escribir gramática | MT serie |
| 2022 2Q | Bombeo | Gramática + FNG | AP | Ascendente (LR) | MT calculadora |
| 2023 1Q | Bombeo | Gramática + FNG | AP | Ascendente (LR) | — |

> [!info] Las 8 familias de ejercicio
> 1. [[#1. Pasar una gramática a Forma Normal de Greibach (FNG)|Forma Normal de Greibach]] (transformar una gramática)
> 2. [[#2. Demostrar que un lenguaje NO es libre de contexto (Lema de Bombeo)|Demostrar que un lenguaje NO es libre de contexto]] (Lema de Bombeo)
> 3. [[#3. Autómata de Pila que acepta por pila vacía|Autómata de Pila por pila vacía]]
> 4. [[#4. Escribir una gramática que genere un lenguaje|Escribir una gramática (GLC)]]
> 5. [[#5. Máquina de Turing reconocedora|Máquina de Turing reconocedora]]
> 6. [[#6. Máquina de Turing calculadora (transductora)|Máquina de Turing calculadora]]
> 7. [[#7. Análisis sintáctico descendente LL(1)|Análisis sintáctico descendente LL(1)]]
> 8. [[#8. Análisis sintáctico ascendente (LR / SLR)|Análisis sintáctico ascendente (LR/SLR)]]

---

## 1. Pasar una gramática a Forma Normal de Greibach (FNG)

> [!abstract] Cómo lo reconocés
> Te dan una gramática `G` y te piden una **equivalente** donde todas las producciones tengan la forma `A → mβ` (empiezan con un terminal `m`, seguido de cero o más símbolos `β`), o bien `S → λ`. Eso es la **Forma Normal de Greibach**.

Apoyo teórico: [[TLA -Formas Normales y Lema de Bombeo CFL]].

### Algoritmo (los 5 pasos, siempre en este orden)

1. **Eliminar símbolos inútiles** (primero los no útiles, después los no alcanzables).
2. **Eliminar producciones nulas** (anulables), salvo que se conserve `S → λ` si `λ ∈ L`.
3. **Eliminar producciones unitarias** (`A → B`).
4. **Eliminar recursividad a izquierda** (`A → Aα`).
5. **Llevar a la forma pedida** (que cada producción arranque con un terminal).

> [!tip] Truco mnemotécnico
> **I-N-U-R-F**: **I**núntiles → a**N**ulables → **U**nitarias → **R**ecursividad izq. → **F**orma final. Si cambiás el orden, te queda mal.

### Paso a paso

**Paso 1 — Símbolos inútiles.**
- *Útiles*: arrancá de los terminales y marcá qué variables pueden derivar finalmente en una cadena de solo terminales. La que no llega, se elimina (con todas sus producciones).
- *Alcanzables*: arrancá de `S` y marcá qué variables se pueden alcanzar. La que no, se elimina.
- Escribí el nuevo conjunto `P1` y aclará explícitamente *qué símbolo sacaste y por qué* (ej.: "`T` es inútil porque solo deriva en `T b | T c`, nunca termina").

**Paso 2 — Anulables.** Buscá toda variable `A` tal que `A →* λ`. Por cada producción, generá todas las combinaciones quitando los anulables (pero **no** agregues la producción vacía resultante). Conservá `S → λ` solo si la palabra vacía estaba en el lenguaje. Resultado: `P2`.

**Paso 3 — Unitarias.** Por cada `A → B` (un solo no terminal), reemplazala por las producciones de `B` que no sean unitarias. Resultado: `P3`.

**Paso 4 — Recursividad a izquierda.** Para `A → Aα₁ | … | Aαₙ | β₁ | … | βₘ` (las β no empiezan con `A`), introducí una variable nueva `A'`:
$$A \to \beta_1 \mid \dots \mid \beta_m \mid \beta_1 A' \mid \dots \mid \beta_m A'$$
$$A' \to \alpha_1 \mid \dots \mid \alpha_n \mid \alpha_1 A' \mid \dots \mid \alpha_n A'$$
Resultado: `P4`.

**Paso 5 — Forma final.** Si una producción empieza con una variable (`A → Bγ`), reemplazá esa `B` del comienzo por cada una de sus producciones, hasta que **toda** producción arranque con un terminal. Resultado: `P5`.

**Cierre.** Escribí la **definición completa** de la gramática final: `G = ⟨ V, T, S, P5 ⟩`.

### Variaciones

- **Solo transformar** (2016, 2017, 2019): te dan `G` y la convertís. *Ojo*: a veces un paso "no hace falta" (ej.: no hay anulables) → **aclaralo explícitamente**, lo piden.
- **Generar + normalizar** (2022 2Q ej.2, 2023 1Q ej.2): primero [[#4. Escribir una gramática que genere un lenguaje|escribís una gramática]] que genere el lenguaje y *recién después* la pasás a la forma pedida.

> [!warning] Errores comunes
> - Cambiar el orden de los 5 pasos.
> - Olvidar reintroducir `S → λ` cuando la palabra vacía pertenece al lenguaje.
> - Terminar con producciones que empiezan con variable (Paso 5 incompleto).
> - No escribir la cuádrupla `G = ⟨V, T, S, P⟩` al final.

---

## 2. Demostrar que un lenguaje NO es libre de contexto (Lema de Bombeo)

> [!abstract] Cómo lo reconocés
> "Demostrar que `L = {…}` **no es libre de contexto**". El lenguaje suele tener un crecimiento "raro": potencias (`2ⁿ`), primos, cuadrados/cubos, factoriales, o estructuras de espejo con tamaños atados (`ωγωʳ`).

Apoyo teórico: [[TLA -Formas Normales y Lema de Bombeo CFL]] (sección Lema de Bombeo para LLC).

### Algoritmo (esquema fijo de demostración por el absurdo)

1. **Suponé** que `L` *es* libre de contexto → entonces cumple el Lema de Bombeo, con una constante `N`.
2. **Elegí una palabra** `ω ∈ L` con `|ω| ≥ N`, conveniente (la que rompa fácil).
3. Por el lema, `ω = r x y z s` con `|xyz| ≤ N` y `|xz| ≥ 1`.
4. **Bombeá**: para algún `i` (típicamente `i = 2`, o un `i` grande), la palabra `r xⁱ y zⁱ s` *debería* seguir en `L`.
5. **Mostrá que se va de `L`**: acotá la nueva longitud y demostrá que cae *entre* dos valores válidos consecutivos (o que no cumple la propiedad). → **Absurdo**.
6. **Concluí**: `L` no es libre de contexto.

> [!tip] La clave está en el paso 5
> Casi siempre el argumento es: "la longitud nueva es mayor que el valor permitido `f(N)` pero menor que el siguiente permitido `f(N+1)`, así que no hay ningún valor que la justifique". Para eso te conviene tener a mano el "salto" entre dos valores consecutivos de la propiedad.

### Paso a paso (con la palabra correcta para cada caso)

La parte difícil es **elegir `ω`**. Según el patrón del lenguaje:

| Patrón de `L` | Palabra `ω` que conviene | Por qué se rompe al bombear |
|---|---|---|
| `aⁿ`, `n = 2ᵏ` (potencias de 2) | `ω = a^(2^N)` | `2^N + |xz|` queda entre `2^N` y `2^(N+1)` |
| `aⁿ`, `n` primo (2017 1Q) | `ω = aᵖ`, `p > N` primo | con `i = p+1`, la longitud `p(1+|xz|)` es compuesta |
| `aⁿ`, `n` cubo perfecto (2017 2Q) | `ω = a^(N³)` | `N³ < N³+|xz| < (N+1)³` |
| `aⁿ`, `n = x!` (2019 1Q) | `ω = a^(N!)` | `N!+|xz|` no llega al siguiente factorial |
| espejo con tamaño atado `ωγωʳ`, `|γ|=|ω|²` (2022 2Q) | palabra que fuerce las 3 zonas | al bombear se descompensan los bloques |
| `|x| = n ∧ |y| = 2ⁿ` (2016 2Q) | `ω = 0^(N+2^N)` con `x=0^N`, `y=0^(2^N)` | `|xz|` empuja fuera del intervalo válido |

**Receta concreta (caso "cubo perfecto", como referencia):**
1. Supongo `L` libre de contexto, constante `N`.
2. Tomo `ω = a^(N³)` (está en `L` y `|ω| ≥ N`).
3. `ω = rxyzs`, `|xyz| ≤ N`, solo `a`, con `|xz| ≥ 1`.
4. Bombeo con `i = 2`: `rx²yz²s = a^(N³+|xz|)`.
5. Como `|xz| ≥ 1`: `N³ < N³+|xz|`. Como `|xz| ≤ N ≤ 3N²+3N < 3N²+3N+1`: `N³+|xz| < (N+1)³`. Entonces la longitud cae *entre* dos cubos consecutivos → **no es cubo perfecto** → `rx²yz²s ∉ L`.
6. Absurdo → `L` no es libre de contexto.

### Variaciones

- **Bombear "para arriba"** (`i = 2`) sirve para potencias/cubos/factoriales.
- **Bombear "mucho"** (`i = p+1`) sirve para primos (forzás que la longitud sea un producto, o sea compuesto).
- En lenguajes de **espejo** (`ωγωʳ`, `ααα ʳ`) la idea es que al bombear se rompe la simetría o la relación de tamaños entre bloques.

> [!warning] Errores comunes
> - Elegir `ω` "fácil" pero que **no** se rompe al bombear (perdés el ejercicio).
> - Olvidar las condiciones `|xyz| ≤ N` y `|xz| ≥ 1`.
> - No cerrar con el "entre dos valores consecutivos": la cota es lo que demuestra el absurdo.

---

## 3. Autómata de Pila que acepta por pila vacía

> [!abstract] Cómo lo reconocés
> "Escribir un **autómata de pila** que acepte **por pila vacía** el lenguaje `L = {…}`". Casi siempre te piden además: (a) **explicar el funcionamiento** y (b) **mostrar formalmente** que reconoce una palabra concreta (la traza).

Apoyo teórico: [[TLA -Autómatas de Pila]].

### Algoritmo

1. **Entendé qué hay que contar/aparear**: el AP usa la pila como memoria para *contar* o *recordar* una parte y *compensarla* con otra (ej.: tantas `a` como `b`, subcadenas `ab`, etc.).
2. **Diseñá la estrategia de pila**: en la primera parte de la palabra **apilás** marcas; en la segunda **desapilás** apareando.
3. **Definí estados** para cada "fase" de la palabra (antes/después de un separador, etc.).
4. **Vaciá la pila al final** (incluido `z₀`) con transiciones-`λ` para aceptar.
5. **Escribí la definición formal completa** y el **diagrama**.
6. **Hacé la traza** de la palabra pedida.

### Paso a paso

**Definición formal** (siempre escribila):
$$A = \langle Q,\ \Sigma,\ \Gamma,\ \delta,\ q_0,\ z_0,\ F \rangle$$
- `Q`: estados · `Σ`: alfabeto de entrada · `Γ`: símbolos de pila (incluí `z₀` y tus marcas, ej. `A`, `N`).
- Aceptación por pila vacía ⇒ `F` no se usa para aceptar (lo que importa es vaciar la pila).

**Diseño de transiciones** `δ(estado, símbolo_entrada, tope_pila) = (estado, qué_apilo)`:
- Para **contar `a`**: en `a` apilo una marca `A`; en `b` que deba aparear, desapilo.
- Si hay un **separador `c`** (lenguajes `αcβ`): cambiás de fase/estado al leerlo.
- Para relaciones tipo `|α|ₐ = 2|β|_b`: apilás 2 marcas por cada símbolo de un lado y desapilás 1 por el otro (o viceversa), según la relación.
- **Cierre**: transiciones `λ, z₀ / λ` (y similares) para vaciar y aceptar `λ` cuando corresponde.

**La traza** se escribe como secuencia de **configuraciones** `[estado, pila, entrada_restante]`:
$$[q_0, z_0, \omega] \vdash [q_1, Az_0, \dots] \vdash \dots \vdash [q_f, \lambda, \lambda]$$
La palabra se acepta si llegás a **pila vacía** habiendo consumido toda la entrada. Usá `⊢` entre pasos y `⊢*` para varios pasos iguales.

> [!example] Idea típica (2016 2Q, `#ab(α) = #ab(β)`)
> Apilo una marca cada vez que detecto una subcadena `ab` en `α`; al pasar el separador `c`, por cada `ab` en `β` desapilo una marca. Si terminé con la pila vacía, los conteos coincidían.

### Variaciones

- **Conteos con factor** (`|α|ₐ = 2|β|_b`, 2017): apilás/desapilás en proporción 2:1.
- **Contar subcadenas** (`#ab`, 2016): apilás al cerrar el patrón `ab`.
- **Relaciones de orden** (`n > 2m > 0`, 2019 2Q; `aⁿ⁺ᵐbᵐ⁺ᵗaᵗbⁿ`, 2022): combinás marcas distintas para varias variables ligadas.
- **Mezcla con `1ⁿ` u otra cola** (2019 1Q): una fase cuenta y otra verifica la longitud final.

> [!warning] Errores comunes
> - No vaciar `z₀` al final (no aceptás por pila vacía).
> - Olvidar el caso `λ` o las palabras borde.
> - Traza incompleta: tenés que llegar a `[q, λ, λ]`.

---

## 4. Escribir una gramática que genere un lenguaje

> [!abstract] Cómo lo reconocés
> "Escribir una **gramática** (libre de contexto) que genere `L = {…}`. Escribir la definición completa." A veces sigue con "…en esta forma normal" (entonces encadenás con [[#1. Pasar una gramática a Forma Normal de Greibach (FNG)|FNG]]).

Apoyo teórico: [[TLA -Autómatas de Pila]] (equivalencia GLC ↔ AP) y [[TLA -Formas Normales y Lema de Bombeo CFL]].

### Algoritmo

1. **Descomponé el lenguaje** en sus "piezas" (prefijo, núcleo, sufijo, relaciones de igualdad).
2. **Una variable por cada patrón** (ej.: una para `aⁿ…bⁿ`, otra para la parte libre).
3. **Atá las cantidades** generando los símbolos relacionados en la **misma producción** (así quedan iguales/proporcionales).
4. **Concatená** las piezas desde `S`.
5. **Verificá** generando 2–3 palabras del lenguaje y revisando que no genere de más.
6. **Escribí la definición completa** `G = ⟨V, T, S, P⟩`.

### Paso a paso (patrones útiles)

- **Igual cantidad apareada** `{aⁿbⁿ}`: `S → aSb | λ`.
- **Espejo / palíndromo** `{ωωʳ}`: `S → aSa | bSb | λ`.
- **Parte central libre con conteos** (`#a = #b`): `M → aMb | bMa | MM | λ` (genera cualquier cadena con igual número de `a` y `b`).
- **Relación con desigualdad** (`m ≥ n`): generás el bloque igualado y agregás "extra" de un lado: `S → aSb | T`, `T → Tb | λ` (más `b` que `a`).
- **Separadores fijos** (`#c(α)=2`, posiciones de `c`): poné los `c` explícitos en `S` y dejá las variables generando lo demás entre medio.

> [!example] Ejemplo (2019 1Q ej.4): `L = {0ⁿ ω 1ᵐ / ω ∈ {2,3}*, #2(ω)=#3(ω), m ≥ n ≥ 0}`
> - `0ⁿ … 1ᵐ` con `m ≥ n`: `S → 0S1 | A1 | A` (apareás `0` con `1` y dejás `1` de más).
> - `ω` con igual `2` y `3`: `A → 2A3 | 3A2 | AA | λ`.
> Después escribís `G = ⟨{S, A}, {0,1,2,3}, S, P⟩`.

### Variaciones

- Si piden **además FNG**, primero generás la gramática "natural" y luego aplicás los 5 pasos.
- Cuidado con los **bordes** (`n = 0`, `m = 0`): asegurate de que `λ` o las palabras mínimas se generen.

---

## 5. Máquina de Turing reconocedora

> [!abstract] Cómo lo reconocés
> "Diseñar una **máquina de Turing** que **reconozca** las palabras de `L = {…}`." Pide explicar el funcionamiento y, a veces, mostrar formalmente que reconoce una palabra.

Apoyo teórico: [[TLA -Máquina de Turing]] y [[TLA -Máquina de Turing (Parte 2)]].

### Algoritmo

1. **Pensá la estrategia en una frase** (es lo que pide "explicar brevemente"). Casi siempre es **marcar y aparear**: "por cada `b` busco y marco dos `a`", etc.
2. **Elegí símbolos de marca** (`X`, `Y`, …) para tachar lo ya contado.
3. **Diseñá el ciclo**: ir a la derecha buscando un símbolo, marcarlo, volver a la izquierda buscar su pareja, marcarla; repetir.
4. **Condición de aceptación**: cuando todo quedó marcado en la proporción correcta, vas a `qf`. Si falta o sobra, te "colgás" (rechazás).
5. **Dibujá la tabla/diagrama** con transiciones `símbolo_leído / símbolo_escrito, dirección`.

### Paso a paso

- Notación de transición: `a/X, R` = "si leo `a`, escribo `X`, muevo a la **derecha**". `L` = izquierda.
- **Técnica de marcado**: reemplazás los símbolos ya contados por marcas (`X`, `Y`) para no recontarlos; al final podés (o no) restaurarlos.
- Para relaciones tipo `|ω|ₐ = 2|ω|_b` (2016 2Q ej.4): por cada `b`, marcás 2 `a`. Al terminar, si no quedan `a` ni `b` sin marcar, aceptás.
- Para lenguajes con **separador** `αcβ` (2017 1Q ej.4): la MT compara las dos mitades alrededor de `c`.
- **Traza formal** (si la piden): secuencia de configuraciones instantáneas mostrando cinta + estado + cabezal, hasta `qf`.

### Variaciones

- **Conteo proporcional** (`2·|ω|_b`): marcás de a dos.
- **Comparar dos mitades** (`αcβ`): apareás símbolo a símbolo cruzando el separador.
- **Igualdad de subcadenas** (`#ab` en `α` y `β`): contás patrones en cada lado.

> [!warning] Errores comunes
> - No definir qué pasa con la marca al volver (te quedás en un loop).
> - No manejar el `B` (blanco) de los extremos.
> - Olvidar el caso de la palabra vacía / mínima.

---

## 6. Máquina de Turing calculadora (transductora)

> [!abstract] Cómo lo reconocés
> "Diseñar una MT que **resuelva** `f(x,y) = …`" o que "**obtenga** la serie/sucesión `{…}`", con los operandos en **unario** (cadenas de `1`), separados por `*`, y el resultado en la cinta al final. Te dan ejemplos de "Situación Inicial → Situación Final".

Apoyo teórico: [[TLA -Máquina de Turing]] y [[TLA -Máquina de Turing (Parte 2)]].

### Algoritmo

1. **Leé bien los ejemplos**: te dicen exactamente el formato de entrada y de salida (dónde queda el `*`, el `#`, etc.).
2. **Pensá la cuenta como copiar/mover/borrar unos**: en unario, sumar = concatenar, multiplicar por 2 = copiar dos veces, restar = aparear y cancelar, dividir entre 2 = aparear de a dos.
3. **Usá una marca de borde** (`#` o `*`) para delimitar dónde escribís el resultado.
4. **Diseñá el ciclo de copia**: marcás un `1` del origen (`1→X`), vas al área de resultado, escribís un `1`, volvés; repetís hasta que no queden `1` sin marcar.
5. **Limpiá** las marcas al final para dejar la salida pedida.

### Paso a paso (operaciones unarias típicas)

- **Sumar** `x + y`: borrás el separador o juntás los bloques de `1`.
- **Multiplicar por 2** (`2·x`): por cada `1` del origen, escribís **dos** `1` en el resultado.
- **Restar / valor absoluto** (`|x − y|`): apareás un `1` de `x` con uno de `y` y los cancelás; lo que sobra es la diferencia (2016 2Q ej.5: `2·|x−y|`, primero diferencia y después duplicar).
- **Dividir entre 2** (`/2`, división entera, 2017 1Q ej.5): apareás de a dos `1` y escribís uno en el resultado.
- **Cuadrado** (`n²`, 2022 2Q ej.5): por cada `1` de `n`, copiás `n` unos en el resultado (suma repetida `n` veces).
- **Series** `{2i+1}` o `{3i}` (2019): generás cada término en unario y los separás con `.` (punto); el `n` inicial te dice cuántos términos.

> [!example] Ejemplo de lectura de los ejemplos (2017 1Q, `(2x+y)/2`)
> `Bq0 11*11B → qf *111`: hay `x=2`, `y=2` → `(2·2+2)/2 = 3` → resultado `111`. Eso te confirma el formato: resultado precedido por `*`.

### Variaciones

- **Una función aritmética** (resta, división, cuadrado): patrón copiar/cancelar.
- **Generar una serie**: ciclo que produce términos crecientes separados por `.`, usando `n` (en unario) como contador.

> [!tip] Cómo "explicar brevemente el algoritmo"
> Alcanza con 3–5 renglones describiendo el ciclo: qué marcás, a dónde copiás, cuándo parás. La cátedra valora la explicación clara tanto como el diagrama.

---

## 7. Análisis sintáctico descendente LL(1)

> [!abstract] Cómo lo reconocés
> Te dan una gramática y piden: (a) **transformarla a LL(1)**, (b) calcular **PRIMEROS y SIGUIENTES**, (c) armar la **tabla de análisis descendente**, (d) escribir el **pseudocódigo** de los procedimientos, (e) **reconocer** una cadena.

Apoyo teórico: [[TLA -Análisis Sintáctico]].

### Algoritmo

1. **Hacer LL(1)**: eliminar recursividad a izquierda y **factorizar a izquierda** (sacar prefijos comunes).
2. **PRIMEROS** de cada no terminal.
3. **SIGUIENTES** de cada no terminal.
4. **Tabla** `M[No terminal, terminal]`.
5. **Pseudocódigo** (un procedimiento por no terminal) o **seguimiento** con pila.

### Paso a paso

**Paso 1 — LL(1).** Quitá recursividad por izquierda y factorizá prefijos comunes introduciendo variables nuevas (`L'`, `M`, …). El resultado debe poder decidir la producción mirando **un solo** símbolo de adelante.

**Paso 2 — PRIMEROS(X).** Símbolos terminales con los que puede *empezar* lo que deriva `X`. Si `X →* λ`, agregá `λ`.

**Paso 3 — SIGUIENTES(A).** Terminales que pueden aparecer *inmediatamente después* de `A`. Reglas: `$` está en SIGUIENTES(símbolo inicial); para `B → αAβ`, agregá PRIMEROS(β)∖{λ}; si `β →* λ`, agregá SIGUIENTES(B).

**Paso 4 — Tabla.** Para cada `A → α`: poné `A → α` en `M[A, t]` para cada `t ∈ PRIMEROS(α)`; si `λ ∈ PRIMEROS(α)`, ponela también en `M[A, t]` para cada `t ∈ SIGUIENTES(A)`. **Si una celda queda con dos producciones, la gramática NO es LL(1).**

**Paso 5 — Reconocimiento.** Con pila (`$` al fondo, `S` arriba): si el tope es terminal, debe matchear la entrada; si es no terminal, lo reemplazás por la producción que indica la tabla. Aceptás cuando pila y entrada quedan vacías. Mostralo como tabla **Entrada | Pila**.

> [!example] Ejemplo (2017 2Q ej.4)
> `E → (L) | id`, `L → L;E | intE`. Transformada: `E → (L) | id`, `L → intEM`, `L' → ;EM`, `M → λ | L'`. Después PRIMEROS/SIGUIENTES, tabla y seguimiento de `(int id;id;(int id))`.

> [!warning] Errores comunes
> - Calcular SIGUIENTES sin haber sacado recursividad/factorizado.
> - Olvidar el `$` en SIGUIENTES del inicial.
> - No usar SIGUIENTES para las producciones-`λ` en la tabla.

---

## 8. Análisis sintáctico ascendente (LR / SLR)

> [!abstract] Cómo lo reconocés
> "Efectuar el **grafo de análisis ascendente**", "indicar en qué **estados hay conflictos**", "explicar cómo se resuelven (tabla sin conflictos)" y "hacer el **seguimiento**" de cadenas. Es análisis **bottom-up** (desplazamiento-reducción).

Apoyo teórico: [[TLA -Análisis Ascendente]] y [[TLA -Análisis Sintáctico]].

### Algoritmo

1. **Aumentá la gramática** con `S' → S`.
2. **Construí los ítems LR(0)** y el **autómata** (estados = conjuntos de ítems, usando CLAUSURA e IR-A).
3. **Detectá conflictos**: shift-reduce o reduce-reduce (estados con un ítem completo `A → α·` junto a otro que desplaza o reduce).
4. **Resolvé con SLR(1)**: una reducción `A → α·` solo se aplica si el símbolo de entrada está en **SIGUIENTES(A)**. Mostrá la tabla ya **sin conflictos**.
5. **Seguimiento**: con pila de estados/símbolos, hacé `shift`/`reduce` según la tabla hasta `aceptar`.

### Paso a paso

- **Ítems**: una producción con un punto `·` marcando lo ya leído (ej.: `A → a·Bc`).
- **CLAUSURA(I)**: si hay `A → α·Bβ`, agregá los ítems `B → ·γ` (y repetís).
- **IR-A(I, X)**: mové el punto sobre `X` en todos los ítems de `I` y cerrá.
- **Tabla**: `ACCION` (shift `sN` / reduce `rk` / `aceptar`) y `IR-A` (goto). En conflictos, usá SIGUIENTES para decidir las reducciones (eso es lo SLR).
- **Seguimiento**: columnas típicas **Pila | Entrada | Acción**, hasta reducir a `S'`.

> [!example] Ejemplo (2022 2Q ej.4)
> `S → Ma | bMc | db | bdb`, `M → d`. Construís el grafo LR(0), marcás dónde se cruzan shift y reduce, resolvés con SIGUIENTES, y seguís el reconocimiento de `bdb` y `bdc` (esta última debe **rechazar**).

> [!warning] Errores comunes
> - No aumentar la gramática (`S' → S`).
> - Confundir shift-reduce con reduce-reduce al describir el conflicto.
> - Resolver "a ojo" en vez de justificar con SIGUIENTES (es lo que hace válida la tabla SLR).

---

## Checklist final antes de entregar

> [!check] Revisá esto en cada ejercicio
> - ¿Usé **el algoritmo de la cátedra** (no un atajo propio)? Si no, vale 0.
> - ¿Escribí la **definición formal completa** del autómata / gramática / MT?
> - ¿**Justifiqué** cada paso y, donde un paso "no hacía falta", lo aclaré?
> - En AP/MT: ¿hice la **traza** completa hasta el estado/condición de aceptación?
> - En bombeo: ¿la palabra elegida **realmente se rompe** y cerré con la cota?
> - ¿Está todo **en orden y claro**? Se corrige lo escrito, no la intención.

---

## Enlaces a los resúmenes de teoría

- [[TLA -Formas Normales y Lema de Bombeo CFL]] — FNG, FNC, Lema de Bombeo
- [[TLA -Autómatas de Pila]] — AP, aceptación por pila vacía, equivalencia con GLC
- [[TLA -Máquina de Turing]] · [[TLA -Máquina de Turing (Parte 2)]] — MT reconocedoras y calculadoras
- [[TLA -Análisis Sintáctico]] — descendente LL(1), PRIMEROS/SIGUIENTES
- [[TLA -Análisis Ascendente]] — LR(0), SLR, conflictos
- [[TLA -Lenguajes Regulares]] · [[TLA -Expresiones Regulares]] — base de la jerarquía

Vista del curso: [[TLA.base]]

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (TLA)**

- [[TLA -Formas Normales y Lema de Bombeo CFL]] — FNG y lema de bombeo
- [[TLA -Autómatas de Pila]] — autómatas de pila
- [[TLA -Análisis Sintáctico]] — LL(1)
- [[ejercicios-parcial-ii]] — checklist de ejercicios

<!-- notas-relacionadas:fin -->
