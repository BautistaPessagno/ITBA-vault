---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-08-1019:13
Materia: "[[PAW.base|PAW]]"
temas:
---
ir a mapear las URLs contra su servet era muy molesto

spring es un motor de injeccion de dependencias

MVC = model view controller 

el request llega al Controller → el Controller le pide datos o una operación al Model → el Model devuelve el resultado → el Controller se lo pasa a la View → la View genera la respuesta (HTML) que vuelve al cliente.

Aplicaciones interactivas

la aplicación queda a la espera de un nuevo input de usuario

tres grandes piezas:
1. Controller: recibe el input del usuario, validarlo, interpretarlo. le pide al modelo que actualice el estado. 
2. Modelo (Dominio): reglas de negocio, casos de uso, etc. se actualiza el estado
3. Vistas: El controller elige una vista, acceso de lectura sobre el modelo. y con eso el usuario recibe una vista actualizada
![[Pasted image 20260810192408.png|589]]

busca que los errores no se propaguen. muchos cambios se pueden resolver dentro de una y el resto no se tiene que enterar (Ej: cambiar el diseño de la pagina solo afecta vistas). la vista y el controller estan atados a donde esta armado (Mobile, desktop, web) mientras que el modelo que son las reglas del negocio es agnostico
![[Pasted image 20260810192901.png|489]]

Servlet = Controller
Vistas = JSP (Java Server Page)

Front Controller (Servlet)

se para enfrente de los otros controllers

![[Pasted image 20260810193353.png]]

/* -> frontcontroller. sin importar la URL se lo manda al frontcontroller se lo manda al controller correcto 
ya no hay que mapear a mano los controllers

## Maven
herramienta que gestiona el ciclo de vida de un proyecto de software

el proyecto va a estar subdividido

![[Pasted image 20260810194759.png]]

cualquier version que sea de desarrollo tiene que tener el -SNAPSHOT al final

![[Pasted image 20260810195356.png]]


```bash
mvn archetype:generate

# pregunta del catalogo cual quiero usar
# genero un root POM
# me pide las coordenadas
# las properties debrian estar en todo el proyecto asi que se mueven al pom padre ya que asi heredan del POM padre
# Configuracion decentralizada
```

>[!warning]
>escapar las cosas con scape con
>```
>c:out
># y
>escapeXml()
>```

**`c:out`** — tag de la librería core de JSTL (`http://java.sun.com/jsp/jstl/core`). Sirve para imprimir un valor en el JSP escapando HTML por default:
```html
<h2><c:out value="${usuario.nombre}" escapeXml="true" /></h2>
```


@Bean se agrega en el webConfig.java en el view resolver que es para definir un objeto que hay que traquear


## Cosas a resolver
implementar:
- entidades
- persistencia de datos
- reglas de negocio / casos de uso

![[Pasted image 20260810212816.png]]

en los niveles de abstraccion el use case esta mucho mas arriba
hay un nivel mas alto que es el frontend (web app)


## service
use cases 