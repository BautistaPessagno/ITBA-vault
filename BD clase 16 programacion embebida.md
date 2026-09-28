---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - SQL
  - programacion-embebida
  - transacciones
Created: 2026-03-0314:00
Materia: "[[BD.base|Bases de Datos]]"
temas:
  - SQL embebido en C
  - Manejo de errores (SQLCA, SQLCODE, SQLSTATE, WHENEVER, Indicator)
  - Cursores explicitos e implicitos
  - Propiedades ACID
  - Niveles de aislamiento transaccional
  - Anomalias transaccionales
  - Diseno fisico y claves ficticias
  - Secuencias y atributos autonumericos
  - JDBC arquitectura y tipos de drivers
  - PreparedStatement y SQL Injection
---

# BD clase 16 — Programacion Embebida y Transacciones

## Resumen

### SQL Embebido: Concepto y Flujo de Compilacion

**SQL embebido** es la tecnica de incrustar sentencias SQL dentro de un lenguaje de proposito general (el **host language**, como C, COBOL o ADA). Esto permite construir aplicaciones cliente que combinan logica procedural con acceso a base de datos en una arquitectura **cliente/servidor 2-Tier**.

El compilador del host language no entiende SQL, por lo que el DBMS provee un **precompilador** que transforma cada bloque `EXEC SQL` en invocaciones a funciones de su biblioteca nativa. El flujo completo es:

| Paso | Entrada | Herramienta | Salida |
|------|---------|-------------|--------|
| 1. Precompilacion | `.pgc` (PostgreSQL) / `.sqc` (DB2) / `.pc` (Oracle) | `ecpg` / precompilador del DBMS | `.c` con llamadas a biblioteca |
| 2. Compilacion | `.c` generado | `cc` / `gcc` | `.o` |
| 3. Linkeo | `.o` + biblioteca DBMS | `cc -lecpg` | ejecutable |

Existen dos modalidades de ejecucion:

- **SQL Estatico**: la sentencia se conoce en tiempo de programacion.
- **SQL Dinamico**: la sentencia se construye como string en tiempo de ejecucion (ej. `PREPARE`, `EXECUTE`, `EXECUTE IMMEDIATE`).

> [!important]
> Los precompiladores de los DBMS corren **antes** que el preprocesador de C. Por lo tanto, no se puede usar `#IFDEF` / `#ELSE` para manejar diferencias de conexion entre motores.

### Patron Completo de Programa con SQL Embebido

Una aplicacion cliente con SQL embebido sigue siempre la misma estructura: conectar, operar, manejar errores, desconectar.

```c
/* PostgreSQL: plantilla completa */
#include <stdio.h>
#include <stdlib.h>

/* Prototipos de manejo de errores */
void ErrorConexion(void);
void Error(void);

int main(void)
{
    EXEC SQL BEGIN DECLARE SECTION;
        /* host variables: tipos permitidos:
           char, char[n], int, short, long, float, double, varchar[n] */
        long   miValor;
        char   nombre[25 + 1];  /* +1 para NULL terminator de C */
        short  indicador;       /* variable indicator (tipo short) */
    EXEC SQL END DECLARE SECTION;

    /* --- Conexion --- */
    EXEC SQL WHENEVER SQLERROR DO ErrorConexion();
    EXEC SQL CONNECT TO PROOF@localhost:5432
        as myConn USER :user IDENTIFIED BY :passwd;

    /* --- Operaciones --- */
    EXEC SQL WHENEVER SQLERROR DO Error();
    EXEC SQL WHENEVER NOT FOUND DO BREAK;

    EXEC SQL DECLARE myCursor CURSOR FOR
        SELECT COUNT(*), nombre FROM anotado
        GROUP BY nombre ORDER BY nombre;

    EXEC SQL OPEN myCursor;
    for ( ; ; )
    {
        EXEC SQL FETCH myCursor INTO :miValor, :nombre;
        printf("En %s hay %ld anotados\n", nombre, miValor);
    }
    EXEC SQL CLOSE myCursor;

    /* --- Desconexion --- */
    EXEC SQL DISCONNECT myConn;
    return 0;
}

void ErrorConexion(void) {
    printf("%s\t%s\n", SQLSTATE, sqlca.sqlerrm.sqlerrmc);
    exit(EXIT_FAILURE);
}
void Error(void) {
    printf("%s\t%s\n", SQLSTATE, sqlca.sqlerrm.sqlerrmc);
    EXEC SQL DISCONNECT myConn;
    exit(EXIT_FAILURE);
}
```

> [!tip]
> Las **host variables** llevan el prefijo `:` solo dentro de `EXEC SQL`. En el resto del codigo C se usan sin prefijo. Siempre reservar +1 byte para `char[]` por el NULL terminator.

### Manejo de Errores: SQLCA, SQLCODE, SQLSTATE, WHENEVER e Indicator

El DBMS reporta el resultado de cada sentencia SQL. Existen cuatro mecanismos para detectar errores, del mas antiguo al mas expresivo:

| Mecanismo | Descripcion | Portabilidad |
|-----------|-------------|-------------|
| **SQLCA** (forma implicita antigua) | Estructura `sqlca` con campo `sqlcode` (0=OK, <0=error, >0=warning/not found) y `sqlerrm.sqlerrmc` para mensaje | Campos varian entre DBMS |
| **SQLCODE / SQLSTATE** (forma moderna) | `SQLCODE` (long) y `SQLSTATE` (char[6]). `"00000"` = exito, `"02000"` = NOT FOUND, `"22012"` = division por cero | Nombres estandar SQL2, valores pueden diferir |
| **WHENEVER** | Seteo declarativo que aplica desde donde se declara hasta el final del fuente o hasta redefinicion | Estandar, limpia el codigo |
| **Variable Indicator** | Variable `short` asociada a cada host variable para detectar NULLs (-1 = NULL recuperado) | ANSI, implementado en PostgreSQL |

**Condiciones WHENEVER** y sus acciones:

```
EXEC SQL WHENEVER condicion accion;
```

| Condicion | Significado |
|-----------|------------|
| `SQLWARNING` | `SQLCODE > 0 && SQLCODE != 100` |
| `SQLERROR` | `SQLCODE < 0` |
| `NOT FOUND` | `SQLCODE == 100` (equivale a `SQLSTATE "02000"`) |

| Accion | Efecto |
|--------|--------|
| `CONTINUE` | Sigue a la proxima sentencia (default) |
| `DO fn(args)` | Transfiere control a funcion de manejo de errores |
| `GOTO label` | Salta a un rotulo |
| `STOP` | Termina con `exit(1)` y rollback automatico |
| `DO BREAK` | Break dentro de loops |

**Sintaxis de variable indicator** para detectar NULLs:

```c
EXEC SQL SELECT MAX(capacidad) INTO :aValor:aValorInd FROM aula;
/* alternativa: INTO :aValor INDICATOR :aValorInd */
if (aValorInd == -1)
    printf("El resultado fue NULL\n");
```

> [!important]
> Si el DBMS recupera un NULL para un atributo y **no** hay variable indicator asociada, se produce un `SQLERROR`. En C no existe el concepto de NULL para tipos primitivos (a diferencia de PSM), por lo que las variables indicator son la unica forma de detectar NULLs en SQL embebido.

### Cursores: Implicitos y Explicitos

Cuando un `SELECT` devuelve **una unica tupla** (ej. con funciones de agregacion como `MAX`), se usa un **cursor implicito** con `SELECT ... INTO`:

```c
EXEC SQL SELECT MAX(capacidad) INTO :aValor FROM aula;
```

Cuando el resultado es un **conjunto de tuplas**, se necesita un **cursor explicito** con el ciclo declarar-abrir-fetch-cerrar:

```sql
-- Declaracion (admite SCROLL para navegacion bidireccional)
EXEC SQL DECLARE nombre_cursor [SCROLL] CURSOR FOR clausula_select;

-- Apertura
EXEC SQL OPEN nombre_cursor;

-- Navegacion (sin SCROLL solo NEXT es valido)
EXEC SQL FETCH {NEXT | PRIOR | FIRST | LAST | RELATIVE n | ABSOLUTE n}
    nombre_cursor INTO :var1, :var2, ...;

-- Cierre
EXEC SQL CLOSE nombre_cursor;
```

> [!tip]
> Si se necesita informacion **ordenada**, pedirlo en el `ORDER BY` del cursor. Nunca ordenar del lado del cliente: el servidor tiene indices y mayor potencia de procesamiento.

### Propiedades ACID de las Transacciones

Una **transaccion** es una secuencia de sentencias SQL tratadas como una unidad atomica. Se confirma con `COMMIT` y se deshace con `ROLLBACK`. En SQL92, el fin de una transaccion marca implicitamente el inicio de la siguiente.

| Propiedad | Significado | Ejemplo |
|-----------|------------|---------|
| **Atomicidad** (Atomicity) | Se ejecutan todas las operaciones o ninguna | Una transferencia bancaria: debito + credito deben ocurrir juntos o no ocurrir |
| **Consistencia** (Consistency) | La transaccion no viola ninguna restriccion de la BD. Chequeo puede ser `IMMEDIATE` o `DEFERRED` | Un FK con `INITIALLY DEFERRED` se valida recien en el `COMMIT` |
| **Aislamiento** (Isolation) | La ejecucion paralela produce el mismo resultado que la secuencial | Dos ventas simultaneas de asientos no deben vender el mismo asiento |
| **Durabilidad** (Durability) | Los efectos de un `COMMIT` son permanentes | Si el servidor se reinicia, los datos committed persisten |

```c
/* Transaccion embebida */
EXEC SQL INSERT INTO profesor (A1, A2, C)
    VALUES ('Analia', 'Pelikan', 'Objetos')
    RETURNING ID INTO :aValor;

EXEC SQL INSERT INTO tesista VALUES ('Jorge', 'OLAP', :aValor);

EXEC SQL COMMIT;   /* confirma ambas inserciones como unidad */
/* EXEC SQL ROLLBACK;  desharia ambas */
```

> [!important]
> Una vez ejecutado `COMMIT` o `ROLLBACK`, no hay vuelta atras. Se inicia automaticamente una nueva transaccion. **No abrir transacciones** cuando se muestra un dialogo al usuario (puede irse a tomar cafe y dejar otras terminales lockeadas). Hacer transacciones **pequenas**, recien al presionar OK.

### ACID vs. Consistencia Eventual

Los **RDBMS** garantizan **consistencia estricta** (strict consistency): sin importar el nodo consultado, el dato es siempre el mismo. Las bases de datos **NoSQL distribuidas** suelen relajar consistencia a favor de performance, adoptando **consistencia eventual** (eventual consistency): los datos seran consistentes en el futuro, no necesariamente al momento de consulta.

| Modelo | Garantia | Trade-off | Uso tipico |
|--------|----------|-----------|------------|
| Consistencia estricta | Dato identico en todo nodo, en todo momento | Mayor overhead, menor throughput | Banca, reservas |
| Consistencia eventual | Dato convergera en el futuro | Mejor performance y disponibilidad | Twitter, Facebook |

### Niveles de Aislamiento y Anomalias Transaccionales

SQL2/SQL3 definen cuatro niveles de aislamiento. Solo **SERIALIZABLE** (el default en el estandar) evita todas las anomalias.

| Nivel de Aislamiento | Dirty Read | Non-Repeatable Read | Phantom |
|---------------------|------------|-------------------|---------|
| **READ UNCOMMITTED** | Puede darse | Puede darse | Puede darse |
| **READ COMMITTED** | Imposible | Puede darse | Puede darse |
| **REPEATABLE READ** | Imposible | Imposible | Puede darse |
| **SERIALIZABLE** | Imposible | Imposible | Imposible |

**Definicion de cada anomalia:**

- **Dirty Read**: una transaccion lee un dato escrito por otra que aun no hizo `COMMIT`. Si esa otra hace `ROLLBACK`, se leyo un valor "sucio" que nunca existio realmente.
- **Non-Repeatable Read**: una transaccion lee un valor, otra transaccion lo modifica (`UPDATE`/`DELETE`) y hace `COMMIT`, y al releer se obtiene un valor distinto.
- **Phantom**: una transaccion ejecuta una consulta y obtiene un conjunto de tuplas, otra transaccion inserta nuevas tuplas que cumplen la condicion, y al re-ejecutar la consulta aparecen tuplas "fantasma".

```sql
-- Seteo de nivel de aislamiento
SET TRANSACTION [READ WRITE | READ ONLY]
    ISOLATION LEVEL [READ UNCOMMITTED | READ COMMITTED |
                     REPEATABLE READ | SERIALIZABLE];

-- Embebido:
EXEC SQL SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

> [!important]
> PostgreSQL no implementa `READ UNCOMMITTED`; si se declara, lo trata como `READ COMMITTED`. El DBMS resuelve concurrencia mediante **locks**: `SHARED LOCK` para lecturas (pueden coexistir multiples) y `EXCLUSIVE LOCK` para escrituras (bloquea a todos). Esto puede derivar en **deadlocks** que el DBMS resuelve matando alguna transaccion.

### Mecanismo de Locks

| Situacion del objeto | Solicitud SHARED | Solicitud EXCLUSIVE |
|---------------------|-----------------|-------------------|
| Sin lock previo | Concedido | Concedido |
| Con SHARED LOCK | Concedido | **Denegado** (espera) |
| Con EXCLUSIVE LOCK | **Denegado** (espera) | **Denegado** (espera) |

### Diseno Fisico: Claves Ficticias y Secuencias

Una **clave ficticia** es un atributo numerico autogenerado que reemplaza a la PK compuesta o de gran tamano para optimizar el espacio en las FK. **No es una clave del modelo relacional** y no reemplaza la restriccion `UNIQUE` sobre los atributos originales.

```sql
-- Crear secuencia
CREATE SEQUENCE myID START WITH 300 INCREMENT BY 1;

-- Tabla con clave ficticia
CREATE TABLE profesor (
    ID   INT NOT NULL DEFAULT nextVal('myID'),
    A1   CHAR(100) NOT NULL,
    A2   CHAR(50)  NOT NULL,
    C    CHAR(50),
    PRIMARY KEY(ID),
    UNIQUE(A1, A2)   -- la unicidad real se sigue garantizando
);

-- Alternativa con SERIAL (crea secuencia automaticamente)
CREATE TABLE profesor (
    ID   SERIAL,
    nombre CHAR(100) NOT NULL,
    PRIMARY KEY(ID),
    UNIQUE(nombre)
);
```

> [!tip]
> Nunca generar la clave ficticia con `SELECT MAX(ID) + 1`: dos usuarios concurrentes obtendrian el mismo valor. Usar `SEQUENCE` o `SERIAL`, que incrementan atomicamente. La secuencia no se decrementa aunque la transaccion haga rollback, por lo que pueden existir huecos (holes), lo cual no es problema.

**Referencia a claves ficticias con `RETURNING`:**

```c
/* INSERT que devuelve el ID autogenerado sin SELECT intermedio */
EXEC SQL INSERT INTO profesor (A1, A2, C)
    VALUES ('Analia', 'Pelikan', 'Objetos')
    RETURNING ID INTO :aValor;

/* Usar aValor directamente en la insercion del hijo */
EXEC SQL INSERT INTO tesista VALUES ('Jorge', 'OLAP', :aValor);
EXEC SQL COMMIT;
```

### JDBC: Arquitectura y Tipos de Drivers

**JDBC** (Java DataBase Connectivity) provee una API estandar para que aplicaciones Java se conecten a cualquier DBMS. La clase `DriverManager` gestiona los drivers registrados.

| Tipo | Nombre | Requiere en cliente | Caracteristica |
|------|--------|-------------------|---------------|
| 1 | JDBC-ODBC Bridge | ODBC + biblioteca nativa | Lento (indirecciones). Eliminado en Java 8 |
| 2 | Native-API | Driver JDBC + biblioteca nativa | Mejor que tipo 1, requiere licencia cliente |
| 3 | Net-Protocol (3-tier) | Solo driver JDBC | Middle-tier con biblioteca nativa. Ideal para applets |
| 4 | Native-protocol | Solo driver JDBC | Conexion directa al DBMS. Minima configuracion |

PostgreSQL ofrece drivers tipo 3 y 4. DB2 y Oracle ofrecen los cuatro tipos.

**Flujo basico en JDBC:**

```java
// 1. Cargar driver (cuatro formas posibles)
Class.forName("org.postgresql.Driver");

// 2. Conectar
Connection conn = DriverManager.getConnection(
    "jdbc:postgresql://localhost:5432/PROOF", "user", "pass");

// 3. Ejecutar sentencias
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM alumno WHERE legajo > 30");
while (rs.next()) {
    System.out.println(rs.getInt("legajo") + " - " +
                       rs.getString("nombre"));
}
stmt.close();

// 4. Cerrar conexion
conn.close();
```

**Metodos de ejecucion:**

| Metodo | Uso para | Devuelve |
|--------|---------|---------|
| `executeQuery()` | `SELECT` | `ResultSet` (cursor) |
| `executeUpdate()` | `INSERT`, `UPDATE`, `DELETE`, DDL | `int` (filas afectadas) |
| `execute()` | Cualquier sentencia | `boolean` (true si hay ResultSet) |
| `executeBatch()` | Multiples sentencias agrupadas con `addBatch()` | `int[]` |

### PreparedStatement vs. createStatement y SQL Injection

`createStatement` no permite parametrizacion: los valores se concatenan al string, lo que abre la puerta a **SQL Injection**.

```java
// VULNERABLE a SQL Injection:
stmt.executeQuery("SELECT * FROM alumno WHERE legajo > " + input);
// Si input = "30 or true" -> dump completo de la tabla

// SEGURO con PreparedStatement:
PreparedStatement pstmt = conn.prepareStatement(
    "UPDATE alumno SET promedio = promedio - ? WHERE legajo > ?");
pstmt.setDouble(1, 0.5);
pstmt.setInt(2, 40);
pstmt.executeUpdate();
// Reusar con nuevos parametros (sentencia precompilada en el DBMS):
pstmt.setDouble(1, 1.0);
pstmt.setInt(2, 60);
pstmt.executeUpdate();
pstmt.close();
```

> [!important]
> `PreparedStatement` ofrece dos ventajas: **seguridad** (evita SQL injection al separar datos de codigo) y **performance** (la sentencia se compila y optimiza una sola vez en el DBMS, luego solo viajan los parametros).

**NULLs en JDBC** se detectan con `wasNull()` despues de cada `getXxx()`:

```java
double sueldo = cursor.getDouble("sueldo");
if (cursor.wasNull())
    // el valor recuperado era NULL, descartar el 0
```

### JDBC avanzado: Claves autogeneradas y CallableStatement

```java
// Obtener clave autogenerada tras INSERT
PreparedStatement pstmt = conn.prepareStatement(
    "INSERT INTO profesor (nombre) VALUES (?)",
    Statement.RETURN_GENERATED_KEYS);
pstmt.setString(1, "Salerno");
pstmt.execute();
ResultSet rs = pstmt.getGeneratedKeys();
if (rs.next())
    System.out.println("ID generado: " + rs.getString(1));

// Invocar PSM desde JDBC
CallableStatement cstmt = conn.prepareCall("{call ratio (?, ?, ?)}");
cstmt.setString(1, "suerte en BD");          // IN
cstmt.setInt(2, 4);                          // INOUT
cstmt.registerOutParameter(2, Types.INTEGER); // INOUT (salida)
cstmt.registerOutParameter(3, Types.DOUBLE);  // OUT
cstmt.execute();
System.out.println(cstmt.getDouble(3));
```

### Comparacion: SQL Embebido vs. ODBC vs. JDBC

| Aspecto | SQL Embebido (C) | ODBC | JDBC |
|---------|-----------------|------|------|
| Lenguaje host | C, COBOL, ADA, Fortran | C/C++ | Java |
| Compilacion | Requiere precompilador del DBMS | Compilador estandar + bibliotecas ODBC | `javac` + driver JAR en classpath |
| Portabilidad DBMS | Baja (sintaxis de conexion varia) | Media (API comun, dialectos SQL difieren) | Media-Alta (API comun, dialectos difieren) |
| Tipo de SQL | Estatico (tipicamente) | Dinamico | Dinamico |
| Variables host | `:variable` en `EXEC SQL` | Parametros por posicion en funciones C | `?` en `PreparedStatement` / `setXxx()` |
| Deteccion NULL | Variable indicator (`short`) | Variable indicator / `SQLGetData` | `wasNull()` despues de `getXxx()` |
| Manejo de errores | `SQLCA` / `SQLCODE` / `SQLSTATE` / `WHENEVER` | `SQLError()` / return codes | `SQLException` (excepciones Java) |
| Pool de conexiones | No nativo | Depende del driver manager | `DataSource` / `ConnectionPoolDataSource` |

> [!tip]
> Para la catedra de BD, la estrategia preferida con PostgreSQL es **SQL embebido en C** (archivo `.pgc`, precompilador `ecpg`) y **JDBC tipo 4** (driver `org.postgresql.Driver`, conexion directa sin bibliotecas nativas). ODBC no se cubre en detalle pero su arquitectura conceptual es analoga a JDBC.

## Notas

- En PostgreSQL, `SERIAL` internamente crea una `SEQUENCE` independiente de la tabla. Esto significa que la secuencia podria compartirse entre tablas (a diferencia de `IDENTITY` en DB2).
- Para tablas que se **autoreferencian** (FK circulares), usar `INITIALLY DEFERRED` en las restricciones para que se validen recien en el `COMMIT`.
- El manejo de transacciones con `setAutoCommit(false)` en JDBC es equivalente a agrupar sentencias en una transaccion explicita. Sin esto, cada sentencia es su propia transaccion.
- La creacion de **indices no-unique** por eficiencia es decision del DBA, nunca del programador. Mejoran consultas pero degradan inserciones/updates/deletes.
- En JDBC, `DataSource` (JDBC 2.0+) reemplaza a `DriverManager` para conexiones mas limpias y con soporte de connection pooling.

## Preguntas

1. Si en un programa con SQL embebido se hace `EXEC SQL SELECT` sobre una columna que contiene NULL pero **no** se declaro variable indicator, que ocurre? (Se genera un `SQLERROR`).
2. Por que no se puede usar `#IFDEF` para manejar diferencias de conexion entre DBMS en SQL embebido? (Porque el precompilador del DBMS corre antes que el preprocesador de C).
3. Cual es la diferencia entre Non-Repeatable Read y Phantom? (NRR involucra `UPDATE`/`DELETE` de tuplas existentes; Phantom involucra `INSERT` de nuevas tuplas que cumplen la condicion).
4. Por que `SELECT MAX(ID) + 1` no es seguro para generar claves ficticias? (Dos transacciones concurrentes pueden leer el mismo MAX y producir el mismo valor).
5. Que ventaja tiene `PreparedStatement` sobre `createStatement` mas alla de la seguridad? (La sentencia se compila y optimiza una sola vez en el DBMS; en invocaciones sucesivas solo viajan los parametros).
6. En un nivel `REPEATABLE READ`, puede una transaccion ver tuplas nuevas insertadas por otra transaccion? (Si, esa es exactamente la anomalia Phantom que este nivel permite).

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (BD)**

- [BD clase 14 triggers](BD%20clase%2014%20triggers.md) — clase anterior
- [BD clase 8 SQL consultas](BD%20clase%208%20SQL%20consultas.md) — las consultas que se embeben

**Otras materias**

- **EDA**  [EDA - Hashing](EDA%20-%20Hashing.md) — índices hash
- **EDA**  [EDA - Árboles](EDA%20-%20Árboles.md) — índices B-tree en el diseño físico
- **PI**  [PI - Intro C](PI%20-%20Intro%20C.md) — SQL embebido en C
- **PI**  [PI - Struct y Union en C](PI%20-%20Struct%20y%20Union%20en%20C.md) — structs y variables host en C
- **POO**  [POO - Introduccion a Java](POO%20-%20Introduccion%20a%20Java.md) — JDBC desde Java
- **SO**  [Threads](Threads.md) — concurrencia y niveles de aislamiento

<!-- notas-relacionadas:fin -->
