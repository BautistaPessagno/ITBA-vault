---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-2419:07
Materia: "[[PAW.base|PAW]]"
temas:
---
# Clase 3 - Spring JDBC y Unit Testing

semana pasada se armo la capa de servicios con los controllers

la idea es convertir un error logico en un error de compilacion

maven es altamente util por las herramientas que nos da

maven define scopes 
el scope default se llama compile -> disponible para todo el mundo
runtime el que esta disponible en tiempo de ejecución pero no en tiempo de compilación
provided es lo contrario a provided

las interfaces las queremos en compile, 




el unico que da feedback al usuario es el modulo web

las validaciones que hay en services son en cuando a programación defensiva (validar datos correctos) pero despues el service nunca sabe si lo que lo llama es un usuario, API, etc
las validaciones al usuario van en el webapp

>[!warning]
>El controller no se encarga de las validaciones, el controller recibe los resultados directamente


# Resumen Wispr
## Maven Scopes and Spring MVC

## Summary
### Flow Summary
Clase sobre uso de scopes de Maven para forzar programación contra interfaces, estructuración en módulos (web, services, persistence, models con sus contratos), Spring JDBC y validaciones declarativas con JSR-303 en formularios.

### Scopes de Maven e interfaces

- Scopes: compile (default), test, runtime (solo ejecución), provided (solo compilación, lo aporta el container)
- Separar interfaces (compile) e implementaciones (runtime) obliga a programar contra interfaces: falla en compilación si no
- Dependencias de compile son transitivas, las de runtime no: hay que declararlas explícitamente en web

### Estructura en módulos y capas

- Nuevos módulos: services-contracts, persistence, persistence-contracts, models; paquetes iguales para no cambiar imports
- Cadena: web → services (runtime) + services-contracts (compile); services → persistence (runtime) + persistence-contracts (compile)
- DAO devuelve Optional<User>; controller lanza UserNotFoundException (RuntimeException) si no está

### Spring JDBC y persistencia

- JdbcTemplate + SimpleJdbcInsert reducen boilerplate de JDBC y evitan SQLException checked
- RowMapper como lambda estática final reutilizable entre queries
- DataSource configurado en web-config con SimpleDriverDataSource apuntando a Postgres local

### Spring MVC: parámetros y formularios

- @RequestParam (con defaultValue) y @PathVariable con placeholder {name} y regex opcional
- Form-backing object (POJO con constructor default, getters/setters) + @ModelAttribute + BindingResult
- Validaciones declarativas JSR-303/380 (@Size, @Email, @Pattern, @NotBlank) vía Hibernate Validator
- Spring form taglib mapea inputs al objeto (path=...) y <form:errors> muestra mensajes; recursos estáticos vía addResourceHandlers

### Responsabilidades por capa

- Validaciones de UX en la capa web (cerca del usuario); en servicios solo programación defensiva
- Validación siempre server-side; no confiar en JavaScript del cliente
- @ModelAttribute sobre método expone objeto (ej. currentUser) a todas las vistas del controller; usar binding=false por seguridad

### Próximos pasos

- (Speaker 1) Próxima clase: testing (especialmente capa de persistencia) y, si alcanza, autenticación
- (Speaker 1) Customizar mensajes de error de validación (ej. regla de username) la semana próxima

### Decisiones

- Interfaces en módulos \*-contracts con scope compile; implementaciones con scope runtime
- DAOs devuelven Optional<User>; ausencia se traduce a UserNotFoundException en el controller
- Validación de formularios vía JSR-303 declarativa, no lógica de validación en el controller ni en servicios

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia 


**Otras materias**

<!-- notas-relacionadas:fin -->