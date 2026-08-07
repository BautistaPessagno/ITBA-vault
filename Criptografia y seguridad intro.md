---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-0616:03
Materia: "[[Criptografía y Seguridad.base|Criptografia y seguridad]]"
temas:
---
# Criptografia y seguridad intro
Usos:
- Comunicaciones seguras
- trafico web
- protección de archivos en disco
- autenticación de usuarios
- proteccion de contenido

# Criptosistema
![[Pasted image 20260806162752.png]]
conjunto de dos funciones de las cuales
es necesario una clave
### Clave
![[Pasted image 20260806163155.png]]

## Funcion de cifrado
![[Pasted image 20260806163401.png]]
en algun lugar se tiene que correr el codigo de la funcion

de esta forma no se puede acceder al mensaje sin la clave

## Seguridad (informal)
![[Pasted image 20260806163954.png|641]]

# Historia
![[Pasted image 20260806164710.png]]


# Cifrado por rotacion
![[Pasted image 20260806165127.png]]
![[Pasted image 20260806165901.png|651]]

entonces ROT-X es inseguro
![[Pasted image 20260806170953.png]]

# Cifrado de sustitucion
por cada simbolo del lenguaje se define por un simbolo por el cual se remplaza

![[Pasted image 20260806171040.png]]

la combinatoria es 26!
el problema que tiene es la repeticion (vease el cifrado el pasaje de e a c)

![[Pasted image 20260806171847.png]]

![[Pasted image 20260806172116.png]]

# Cifrado vigente

![[Pasted image 20260806181558.png]]

el polialfabetico se basa en el de rotacion pero en lugar de totar todo el mensaje se divide en grupos (ej de 4) y a cada uno se le da uno una variable de rotacion distinta
en la imnagen a la primera letra se la rota 4 a la siguiente 2, 5, 3 y para las proximas 4 letras se repite

## Criptoanalisis
hay secuencias de letras que aparecen multiples veces. las secuencias de letras iguales, se corresponden. puedo conseguir el tamaño de bloque de esa manera

![[Pasted image 20260806182904.png]]

cuanto mas largo el mensaje mayor facilidad para encontrar el mensaje

# Sustitucion polialfabetica
![[Pasted image 20260806183655.png]]


# Criptosistema (Definicion)

se lo define como una terna de algoritmos

generador, cifrado, descifrado

![[Pasted image 20260806184308.png]]

## seguridad: secreto perfecto

![[Pasted image 20260806184551.png]]