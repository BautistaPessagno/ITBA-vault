---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-1016:09
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
temas:
---
# Practica 1 criptografia y seguridad

Esquemas clasicos de encripcion 

$$
\pi(Gen,Enc,Dec)_{priv} 
$$
k <- Gen()
c <- $Enc_k(m)$
m:=$Dec_k(c)$

la seguridad debe recaer en que la clave se mantenga en secreto

es mas facil cambiar la clave que el algoritmo
## Esquemas clasicos
### cifrado simetrico
mismo k para encriptar que desencriptar

### Sustitucion
intercambiar 
**monoalfabetico**: usa un unico alfabeto (ejs: rotacion, Cesar)
es bastante inseguro debido a la frecuencia de letras

se puede calcular las letras

**Polialfabetico:** Cada letra usa un alfabeto distinto

### Trasposicion
cambiar las cosas de lugar
"Este es un mensaje" con k=3
EST
EES
UNM
ENS
AJE

c = EEUEASENNJTSMSE

![Drawing 2026-08-10 16.17.09.excalidraw](Excalidraw/Drawing%202026-08-10%2016.17.09.excalidraw.md)

# Ataques
## Pasivo
### Ataque de texto cifrado solo
ataque de texto cifrado solo
- Dato: cifrado
- Obtiene todo el plano
### Ataque de texto plano conocido
- datos: pares (Cifrado,plano)
- Obtiene todo el plano

## Activo
### Ataque de texto plano elegido
- obtiene pares (cifrado, plano)
- obtiene todo el plano
### Ataque de texto cifrado elegido
obtiene pares(cifrado, plano)
obtiene todo el plano