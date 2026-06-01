---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-03-1110:31
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - Guia 2
  - Modelo Relacional
---
# Mapeo del diagrama MER al modelo relacional

## Ej 1
Realizar el mapeo del diagrama MER del ejercicio 2.B de la práctica anterior al modelo relacional. Indicar cuáles son las restricciones que se pudieron representar y cuáles no. Explicar.

![[Pasted image 20260311104254.png]]
### Autor
**DNI**  | nombre

### Publicación
**ISBN** | Titulo 

### Correcto
**DNI** | nombre 

### Escrita por
**ISBN** | **DNI**

### Revisa
**DNI** | **ISBN**



## Ej 2
Realizar el mapeo del diagrama MER del ejercicio 4 de la práctica anterior al modelo relacional. Indicar cuáles son las restricciones que se pudieron representar y cuáles no se pudieron. Explicar.
![[Pasted image 20260311104919.png]]

### Departamento
**Nombre** 
### Articulo
**Código** | Stock | descripción | **Departamento.Nombre**

>[!Explicación]-
>El articulo al tener una relacion 1:N solo va a tener un departamento, por lo tanto lo incluimos en la tabla
### Proveedores
**Nombre** | Dirección
### Provee
**Articulo.código** | **Proveedores.Nombre** | Precio

>[!Explicaión]-
>Como la relacion Proveedores y Articulos es N:M lo que se hace es hacer una tabla separada para la relacion Provee, incluyendo el Precio
### Empleados
**Nombre** | Salario | **Departamento.Nombre**
### Jefe
**Nombre** | **Departamento.Nombre**


## Ej 3
Realizar el mapeo del diagrama MER del ejercicio 6 de la práctica anterior al modelo relacional. Indicar cuáles son las restricciones que se pudieron representar y cuáles no se pudieron. Explicar.
![[Pasted image 20260311111024.png]]

### Clientes
**Código** | nombre | dirección | DNI | Telefono

### Préstamo
**Cliente.Código** | **Código** | importe

>[!Expliacion]
>como la relacion es M:1 se incluye el codigo del cliente en el prestamo

### Pactado en
**Préstamo.Código** | fecha | importe | **num**

>[!explicacion]-
>Como la relacion es N:1 y a su vez es debil, lo que se hace es incluir la FK la cual va a ser el codigo del prestamo


## Ej 4
Realizar el mapeo del diagrama MER del ejercicio 8 de la práctica anterior al modelo relacional. Indicar cuáles son las restricciones que se pudieron representar y cuáles no se pudieron. Explicar.
![[Pasted image 20260311111324.png]]

### Alumnos 
**Legajo** | nombre | sexo | carrera

### Cursos
**Codigo** | nombre
### Inscrito
**Legajo** | **Codigo** | **año**

## Ej 5
Realizar el mapeo del diagrama MER del ejercicio 10 de la práctica anterior al modelo relacional. Indicar cuáles son las restricciones que se pudieron representar y cuáles no se pudieron. Explicar.

![[Pasted image 20260311113946.png]]

![[Pasted image 20260311114056.png]]![[Pasted image 20260311114059.png]]![[Pasted image 20260311114101.png]]![[Pasted image 20260311114103.png]]![[Pasted image 20260311114106.png]]![[Pasted image 20260311114109.png]]
