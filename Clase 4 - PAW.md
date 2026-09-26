---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-08-3123:27"
Materia:
temas:
---
# Clase 4 - PAW

Clase práctica sobre unit testing en Java: uso de Mockito para testear la capa de servicios contra el contrato (no la implementación) e introducción a HSQLDB para testear la capa de persistencia.

  

### Mockito y testeo de servicios

- API fluida de Mockito: when/thenReturn, excepciones, retornos sucesivos; se lee casi como inglés natural.
- Mocks son 'nice' por defecto: devuelven valores falsy (null, 0, false) sin lanzar excepción si el método no está mapeado.
- Simplificación con anotaciones @Mock, @InjectMocks y MockitoJUnitRunner en vez de inicialización manual.

### Testear contrato, no implementación

- Validar solo lo observable desde afuera; usar verify/verifyNoMoreInteractions ata el test a la implementación.
- Si se necesita verify por métodos void, cambiar el diseño para que devuelvan info útil.
### Excepciones vs Optional

- Un usuario inexistente no es excepcional: el service devuelve Optional, el controller decide (404 vía exception handler).
- Violaciones de FK al crear publicación con userId inválido: dejar fallar la BD; es bug, no error de usuario.
### Testeo de persistencia con HSQLDB

- Moquear DataSource/Connection/Statement/ResultSet es inviable: setup más largo que el test.
- Se usará HSQLDB (base embebida en memoria) con modos de compatibilidad de sintaxis (ej. serial de Postgres).
### Próximos pasos

- (Speaker 1) Retomar tras el break incorporando la dependencia HSQLDB y mostrando su uso para testear la capa de persistencia.
- 
### Decisiones

- Usar HSQLDB embebida en memoria para testear la capa de persistencia en vez de moquear DataSource.
- La capa de servicio devuelve Optional; las excepciones para casos no excepcionales quedan descartadas.


Clase técnica sobre testing de persistencia con Spring usando HSQLDB en memoria, gestión de transacciones y rollback en tests, más introducción a la configuración de Spring Security para autenticación.  

### Tests de persistencia con HSQLDB

- Data source en memoria con HSQLDB en test config; usar sintaxis Postgres vía flag en el connection string
- Runner Spring JUnit + testconfig con component scan de persistencia y bins compartidos
- Esquema e initial data en archivos SQL cargados vía DataSourceInitializer, no en cada DAO
- - Base en memoria garantiza estado inicial conocido en cada corrida
- Validar create con JdbcTestUtils.countRowsInTableWhere, nunca llamando a otro método del DAO under test
### Transacciones y migraciones

- @Transactional + @Rollback por test para aislar; requiere enableTransactionManagement y DataSourceTransactionManager
- Mover schema.sql a main/resources para inicializar la base también en producción
- Esquema append-only con ALTER TABLE para migraciones; alternativa recomendada: FlywayDB
- @Transactional(readOnly=true) sirve como hint para usar réplicas de lectura

### Spring Security

- Módulo aparte (groupId propio, versiones desacopladas); implementa RBAC + ACL
- WebAuthConfig extiende WebSecurityConfigurerAdapter; reglas antMatchers evaluadas en orden, anyRequest al final
- UserDetailsService custom (PawUserDetailsService) mapea User de la base a UserDetails con roles
- Requiere filter springSecurityFilterChain en web.xml y dependencia javax.servlet-api en scope provided  

### Próximos pasos

- (Speaker 1) Próxima clase: mostrar funcionamiento de Spring Security y hashing de contraseñas (no texto plano)
- (Speaker 2) Consultar con los docentes si el primer sprint requiere Spring Security o MVP sin autenticación

### Decisiones

- Protección CSRF deshabilitada por ahora
- UserDetailsService propio en vez del DAO default de Spring Security


---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia 


**Otras materias**

<!-- notas-relacionadas:fin -->