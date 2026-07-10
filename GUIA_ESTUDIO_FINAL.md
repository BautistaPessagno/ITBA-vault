---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-06-2314:35
Materia: "[[MNA.base|MNA]]"
temas:
---
# Guía de estudio — Final MNA (ITBA)

> Objetivo: con poco tiempo, saber **exactamente qué practicar** y **qué script de calculadora** resuelve cada parte. Los finales son **muy repetitivos**: dominá 4 ejercicios y viste todo el examen.

---

## 1. Cómo es el final (el esqueleto se repite en TODOS)

| Slot | Qué pide | Script Casio |
|------|----------|--------------|
| **Ej 1 — Transformación lineal** | núcleo/imagen **o** matriz `M_BB` en una base dada, **y** ¿diagonalizable? / clasificar / hallar `k` (paramétrico) | `TL.py` + `EIG.py` ✓ |
| **Ej 2 — Serie de Fourier** | hallar la serie de `f` en `[0,1]` periódica, **y** "¿a qué converge en `x=0` y `x=1/2`?" | `FOURIER.py` ✓ |
| **Ej 3 — Esquema implícito (dif. finitas)** | calor `∂u/∂t=∂²u/∂x²`, **4 nodos interiores**, BC Dirichlet/Neumann/mixtas | `HEAT.py` ✓ |
| **Ej 4 — Transf. de Fourier / Descomposición** | TF continua de un `f(t)` chico, **o** SVD/QR/PLU de una matriz dada | `FT.py` ✓ + `SVD/QR/PLU.py` ✓ |

**Notas al revisar los PDFs:**
- `MNA_Final_Tema_IV.pdf` tiene encabezado interno **"Tema VII"** (es un 2º Tema VII distinto).
- Hay **dos "Tema XI"** (`MNA_Final_Tema_XI.pdf` y `MNA_Final_Tema_XI(1).pdf`).
- En la práctica son ~13 finales distintos, muy solapados entre sí.

---

## 2. Plan priorizado de ejercicios (hacer ANTES de practicar finales)

Las prácticas mapean 1-a-1 a los slots del final. Hacé en este orden y frená cuando puedas resolver finales sin ayuda.

### 🔴 P1 — Ej 1 (está en TODOS los finales). Empezá por acá.
- **TP IV (Transf. lineales):** Ej **3** (matriz del operador), **6** (cambio de base), **8 y 10** (núcleo/imagen vía matriz en base). → `TL.py`
- **TP V (Autovalores):** Ej **1, 2** (diagonalizar, multiplicidades), **6** (autoespacios de una T). → `EIG.py`

### 🟠 P2 — Ej 2 y Ej 4 (Fourier, en casi todos)
- **TP VIII (Serie de Fourier):** Ej **4** (series de ondas cuadrada/triangular), **12, 13, 14** (serie + convergencia en un punto). → `FOURIER.py`
- **TP IX (Transformada continua):** Ej **1** (transformada por integración). → `FT.py`
- *Prerrequisitos si flojeás:* **TP I** (complejos: De Moivre, `eʷ=z`) y **TP III Ej B5/B6** (ortogonalidad de senos/cosenos/exponenciales — **es el motivo por el que Fourier funciona**).

### 🟡 P3 — el ejercicio de descomposición
- **TP VI:** Ej **3** (QR), **4** (LU/PLU), **8** (resolver sistema vía QR y LU). → `QR.py`, `PLU.py`
- **TP VII:** Ej **2** (SVD + pseudoinversa), **3** (rango/inversa/normas vía SVD). → `SVD.py`

### ⚪ P4 — solo si sobra tiempo
- **TP II** (sistemas paramétricos, determinantes) → soporta los Ej 1 paramétricos ("hallar `k`").
- **TP III** (espacios, bases, normas) → base de todo.

### ⚠️ Hueco importante
El **esquema implícito de la ecuación del calor (Ej 3)** **no tiene práctica** en `Practicas/` (solo van TP I–IX). Estudialo de apuntes. La receta es siempre la misma:

```
sistema implícito (Euler atrás):  -r·u_{i-1} + (1+2r)·u_i - r·u_{i+1} = u_i^n
                                   r = Δt/h² ,  h = 1/5  (4 nodos interiores)
A·u^{n+1} = b   con A tridiagonal: diag (1+2r), fuera de diag (-r)
- Dirichlet (u fijo): el borde es dato → pasa al lado derecho b.
- Neumann (u_x=0): el borde es incógnita → nodo fantasma reflejado (u_{-1}=u_1) → la fila de borde queda diag (1+2r), vecino (-2r).
```

`HEAT.py` arma esa matriz solo: le decís el tipo de borde y la condición inicial.

---

## 3. Cheat-sheet de scripts (`casio/`) — qué editar antes de correr

| Script | Resuelve | Editar antes de correr |
|--------|----------|------------------------|
| `TL.py` | núcleo, imagen, `T(v)=b`, matriz `M_B1B2` | nada (todo por `input`) |
| `EIG.py` | autovalores + diagonalización 3×3 (P, D) | nada |
| `SVD.py` | SVD + pseudoinversa `A⁺` | nada |
| `QR.py` | QR + mínimos cuadrados | nada |
| `PLU.py` | `PA=LU` + resolver `Ax=b` | nada |
| `FOURIER.py` | serie de Fourier `a₀,aₙ,bₙ,cₙ` + valor de convergencia | **`def f(t)`**, `t0`, `T` |
| `FT.py` | transformada continua `X(w)=∫f(t)e^{-iwt}dt` (para verificar) | **`def f(t)`**, `t0`, `t1` |
| `HEAT.py` | matriz del esquema implícito del calor + un paso | **`def u0(x)`** (condición inicial) |

**Tip de examen:** `FOURIER.py` responde directo *"¿a qué converge en x?"* mediante `(f(x⁻)+f(x⁺))/2`. `FT.py` te da `X(w)` numérico para **chequear** la cuenta simbólica hecha a mano.
