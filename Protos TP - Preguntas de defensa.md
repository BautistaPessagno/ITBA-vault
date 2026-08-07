---
temas:
  - TP
  - defensa
  - preguntas
Created: 2026-07-1400:00
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - socks5
  - redes
  - git
  - defensa
Materia: "[[protos.base|protos]]"
aliases:
  - Preguntas defensa Protos
  - Banco de preguntas defensa
---
# Protos TP - Preguntas de defensa

> [!warning] Documento privado de estudio
> Banco de preguntas anticipadas para practicar la defensa oral del TPE SOCKS5.
> No es material de entrega. Complementa a [[Protos TP - Defensa de commits]]:
> acá están las preguntas organizadas por tema para autoevaluarse
> (leer la pregunta → responder de memoria → contrastar con la guía).

Relacionado: [[Protos TP - Defensa de commits]] · [[Protos TP]] · [[10. Protos - Sockets]] · [[10. Protos - Aplicaciones de Red]] · [[5. Protos - Transporte]] · [[3. Protos - DNS]] · [[Resumen Protos]] · [[Diseño de protocolos]]

> [!tip] Cómo usar el banco
> Cada pregunta trae una **guía breve** de 2–5 líneas, no un guion para recitar.
> Las líneas **⚠️ evitar** marcan el error o la afirmación que la corrección puede
> castigar. La regla de oro de toda la defensa: **primero ubicar el snapshot/hash
> y decir qué existía en ese momento; después explicar qué commit posterior lo
> corrigió**. No mezclar hecho histórico con justificación.

---

## Parte A — Teórico / general del proyecto

Basado en las teóricas: [[10. Protos - Sockets]], [[10. Protos - Aplicaciones de Red]],
[[5. Protos - Transporte]], [[3. Protos - DNS]], [[1. Protos - introducción]],
[[Diseño de protocolos]].

### A.1 Sockets y Socket API

**¿Qué diferencia hay entre un socket pasivo y uno activo?**
El pasivo es el del servidor tras `listen`: no transfiere datos, sólo espera
conexiones y por eso puede compartir puerto. `accept` devuelve un socket **activo**
nuevo por cada conexión establecida, que es el que hace `send`/`recv`.

**Repasá la secuencia de llamadas de un servidor TCP y qué hace cada una.**
`socket` (reservar recurso) → `bind` (publicar IP:puerto, "modo avión") →
`listen` (activar, definir backlog) → `accept` (aceptar, obtener el peer, tipo
caller ID) → `recv`/`send` → `close`. En el cliente: `socket` → `connect` → I/O.

**¿Qué es `INADDR_ANY` / `AF_INET` vs `AF_INET6`?**
`INADDR_ANY` (`0.0.0.0`) en un socket pasivo acepta conexiones por **cualquier
interfaz**. `AF_INET`/`AF_INET6` seleccionan la familia (IPv4/IPv6); `AF_UNSPEC`
deja que la resolución decida. Cuidado con confundir `type` (SOCK_STREAM/DGRAM)
con `protocol`.

**¿Por qué un socket es como un file descriptor?**
El SO guarda los fd como vector de punteros a estructuras; el socket agrega una
abstracción de red sobre esa misma tabla. Por eso `read`/`write`/`close` y el
agotamiento de fds (EMFILE) aplican igual a sockets.

### A.2 Multiplexación: select / poll / epoll

**¿Por qué un servidor concurrente sin fork? ¿Qué gana la multiplexación?**
`fork` por cliente es demasiado pesado. Un solo hilo con `select` multiplexa
listeners, clientes y orígenes, y evita que un peer lento hambree a los demás
(combinado con I/O no bloqueante).

**¿Qué diferencia hay entre select, poll y epoll?**
`select` usa `fd_set` acotado por **FD_SETSIZE** (típico 1024) y reescanea todo en
cada vuelta. `poll` usa un array de `pollfd` sin ese tope fijo. `epoll` (Linux)
escala mejor: registrás fds una vez y el kernel te devuelve sólo los listos;
`epoll_data` es una unión donde guardás el fd o un puntero a tu estructura.

**¿Qué significan POLLIN / POLLOUT / POLLHUP?**
`POLLIN`: hay datos para leer. `POLLOUT`: se puede escribir sin bloquear.
`POLLHUP`: el otro extremo colgó (fin de conexión). `POLLERR`/`POLLNVAL`: error o
fd inválido.

⚠️ evitar: decir que "el TP podría escalar infinito con select". El techo lo pone
FD_SETSIZE; por eso el proyecto **presupuesta** conexiones y contempla migrar a
epoll.

### A.3 I/O no bloqueante

**¿Qué hace `O_NONBLOCK` y por qué es imprescindible acá?**
Cambia el comportamiento de `read`/`write`/`accept`/`connect`: en vez de bloquear,
retornan de inmediato. Es lo que permite que el event loop nunca se detenga por un
solo peer. No avisa por sí mismo que un fd está listo: para eso está `select`.

**Si `recv` devuelve -1, ¿la conexión se cerró?**
No necesariamente. `EAGAIN`/`EWOULDBLOCK` = no había datos ahora (normal en no
bloqueante); `EINTR` = interrumpido por señal, se reintenta. Sólo `recv` == 0 es
EOF ordenado del peer.

**¿Qué es `SO_LINGER`?**
Si está activo, `close`/`shutdown` no retorna hasta enviar toda la data encolada o
agotar el timeout; si no, cierra en background. Relacionado con la decisión de
drenar buffers antes de cerrar.

### A.4 Transporte y TCP

**¿Qué garantiza "confiable" en TCP? ¿Significa que siempre llega?**
No. Confiable = **sabés el estado**: llegó / no llegó / falló, con ACKs y
retransmisión, e informa al nivel superior si no pudo. Orientado a conexión =
handshake antes de datos.

**¿Por qué el relay es full-duplex y qué es half-close?**
TCP permite cerrar una sola dirección: un cliente puede terminar su request
(`shutdown(SHUT_WR)`) y seguir esperando la respuesta. Por eso el proxy **no**
cierra todo al primer EOF; drena lo pendiente de esa dirección y hace shutdown del
peer. El túnel termina cuando **ambas** direcciones cerraron y los buffers están
vacíos.

**¿`shutdown` o `close`? ¿Cuál y por qué?**
`shutdown(SHUT_WR)` cierra sólo la escritura tras drenar, dejando leer todavía;
`close` liberaría ambas direcciones y truncaría datos válidos de request/response.

**¿Qué es Nagle y cuándo conviene TCP_NODELAY?**
Nagle agrupa escrituras chicas para reducir segmentos. `TCP_NODELAY` lo desactiva
para bajar latencia en mensajes pequeños (handshake/relay), a costa de más
paquetes. Es una **hipótesis según el patrón**, no una mejora universal, y hay que
medirla.

### A.5 DNS y resolución de nombres

**¿Cómo se resuelve un FQDN a IP y por qué no en el selector?**
`getaddrinfo` (más completo que gethostbyname) consulta `/etc/hosts` y/o el
servidor DNS y devuelve una **lista** `addrinfo`. Es **bloqueante**, así que corre
en un hilo aparte: el worker sólo resuelve y despierta al selector; todo el I/O de
sockets queda en el hilo principal. La consigna sólo permite threads para
`getaddrinfo`.

**¿Por qué se recorre toda la lista addrinfo?**
Un FQDN puede resolver a varios destinos (IPv4/IPv6). Se prueba `connect` con cada
candidato hasta conectar o agotar la lista (retry), como pide la consigna.

**¿Existe alternativa a un pool de threads para DNS?**
`getaddrinfo_a` es la versión asincrónica. Se optó por un pool acotado propio
porque da control explícito sobre concurrencia y memoria.

### A.6 SOCKS5 (RFC 1928 / RFC 1929)

**Describí el handshake SOCKS5 completo.**
1) Greeting: cliente manda versión + métodos de auth; servidor elige uno.
2) Auth user/pass (RFC 1929) si se negoció. 3) Request: `VER CMD RSV ATYP DST.ADDR
DST.PORT` (acá CMD = CONNECT). 4) Reply: `VER REP RSV ATYP BND.ADDR BND.PORT`.

**¿Qué es `ATYP` y qué valores maneja el proxy?**
Tipo de dirección del destino: IPv4 literal, IPv6 literal o FQDN. Literales evitan
`getaddrinfo`; FQDN usa el pool DNS.

**¿Qué representa `BND.ADDR`/`BND.PORT` en un CONNECT?**
La dirección **local** que el proxy usó hacia el destino (la que eligió el kernel),
**no** la pedida por el cliente. Se obtiene con `getsockname` sobre el socket
origen ya conectado.

**¿Cómo se traducen los errores a códigos REP?**
`ECONNREFUSED`→connection refused; `ENETUNREACH`→network unreachable;
`EHOSTUNREACH`→host unreachable; `ETIMEDOUT`/timeout de connect→TTL expired (según
la política implementada); no mapeables→general failure.

### A.7 Diseño de protocolos y servidor de administración

**¿Qué hay que definir a nivel de aplicación en un protocolo?**
Tipos de mensaje, sintaxis, semántica y **reacción frente a errores**. En un
protocolo de texto se suma el framing (cómo se delimita cada mensaje/respuesta).

**¿Por qué el protocolo de administración va en otro socket?**
La consigna pide un protocolo **separado**. El cliente de administración puede ser
bloqueante, pero el servidor sigue siendo multiplexado en el mismo selector (otro
listener sobre el mismo event loop).

---

## Parte B — Mis aportes específicos

Reorganizado por línea de trabajo (no por hash). Detalle por commit en
[[Protos TP - Defensa de commits]].

### B.1 Presentación de 30 segundos

**Contame en pocas palabras qué hiciste vos.**
Convertí el handler SOCKS5 en un proxy **CONNECT real** y robustecí su ciclo de
vida: DNS fuera del selector, connect no bloqueante con retry, relay bidireccional
y cierres parciales. Después trabajé timeouts, cancelación y ownership para evitar
fugas y use-after-free, y una **batería de estrés** que verifica 500 túneles y mide
degradación. Tengo claro qué fue prototipo no fusionado y qué código posterior es
de otros integrantes.

### B.2 Núcleo SOCKS5 / CONNECT

**¿Por qué el DNS no corre dentro del selector?**
Porque `getaddrinfo` es bloqueante. El thread sólo resuelve; el resultado vuelve al
hilo principal (vía `selector_notify_block`), que conserva todo el I/O de sockets.
Así una resolución lenta no frena a los demás clientes.

**¿Cómo manejás el connect al origen sin bloquear?**
`connect` no bloqueante: se parquea el fd, se espera writable en el selector y se
consulta el resultado; si falla, se prueba el siguiente `addrinfo` (retry) antes de
devolver error.

**¿Por qué guardás intereses de I/O por lado (cliente/origen)?**
El backpressure es **direccional**: sólo leo del cliente si hay espacio en el
buffer cliente→origen, y sólo escribo al cliente si hay bytes origen→cliente.
Discriminar el fd evita interpretar un write-ready del cliente como fin del connect
del origen.

**¿De dónde sale el `BND.ADDR` que respondés?**
De `getsockname` sobre el socket origen conectado: da familia, dirección y puerto
locales reales, correcto también para salida IPv6. Antes respondía direcciones
sintéticas.
⚠️ evitar: decir que basta responder `0.0.0.0:0`; muchos clientes lo toleran pero
pierde información y es incorrecto con IPv6.

**¿Por qué no cerrás el túnel al primer EOF?**
Porque TCP es full-duplex: drenar la dirección con EOF y hacer `shutdown(SHUT_WR)`
en el peer preserva request/response con half-close. (ficha ca0bd8b / 3d65c25)

### B.3 Lifecycle y robustez

**Explicá el reaper: ¿qué hace y por qué O(n)?**
Lista intrusiva doble de conexiones + `last_activity`; el loop principal barre una
vez por segundo (throttle) y cierra handshakes ociosos (timeout inicial 60 s). Para
cientos de conexiones, un barrido O(n) es más simple y predecible que un timer por
fd.

**En la primera versión no cosechabas una resolución DNS vencida. ¿Por qué?**
Todavía no había cancelación segura: el worker podía despertar usando una conexión
ya liberada → use-after-free. Preferí no introducir ese bug; después corregí el
modelo de ownership (b3f690d, 3db880b).

**¿Por qué refcount y ownership del resultado DNS?**
Cliente, origen y job DNS sobreviven tiempos distintos compartiendo el mismo
estado. Sólo se libera cuando **no quedan referencias**. Cancelar lógicamente **no**
autoriza a liberar memoria que el worker todavía usa.

**¿Qué pasa con el job DNS número 65? (pool 4/64)**
Se rechaza de forma controlada y el cliente recibe un fallo SOCKS5. La prioridad es
no permitir crecimiento ilimitado de threads/memoria bajo carga adversa.
⚠️ evitar: presentar 4 workers / 64 jobs / 4 KiB como óptimos. Son decisiones
explícitas **sin benchmark** en ese commit; con 500 clientes una ráfaga de FQDN
podría chocar contra la cota — se dimensionarían con mediciones.

**¿Por qué `CLOCK_MONOTONIC` y no `time(NULL)`?**
El timeout es una **duración**, no una fecha. `CLOCK_REALTIME` puede saltar por NTP
o ajuste manual: adelantar cosecharía conexiones sanas; atrasar prolongaría
vencidas. Monotónico no retrocede.

**¿Por qué validás dominios con NUL embebido?**
Un NUL haría que `getaddrinfo` resuelva un prefijo distinto del nombre validado. Se
chequea por longitud binaria antes de convertir a string C.

**¿Quitar el parámetro `force` no rompe el shutdown?**
No: `force` sugería liberar memoria aún referenciada por el worker. El shutdown deja
de aceptar, cancela lógicamente y **espera/reclama** los workers antes de destruir
el selector. Quitarlo elimina estados imposibles.

### B.4 Escala y disponibilidad

**¿Por qué drenás `accept` en loop hasta EAGAIN?**
`select` es level-triggered y una ráfaga puede traer más de una conexión. Aceptar
una por wakeup deja llenar el backlog. Drenar procesa toda la cola disponible por
evento.
⚠️ evitar: afirmar el "~40 ms" del comentario como resultado: no hay benchmark que
lo respalde en ese commit.

**¿Qué hacés ante EMFILE/ENFILE en `accept`?**
Desuscribo el listener (lo pauso) para no hacer busy-spin al 100 % de CPU, y lo
reactivo cuando el cierre de cualquier conexión libera un fd. Convierte una falla
transitoria en backpressure.

**¿Cuál es el peligro de un fd "stale" en el listener pausado?**
Los enteros de fd se reutilizan. Guardar `17` tras cerrar el listener no garantiza
que `17` siga siendo ese listener; por eso se limpia `accept_paused_fd` por
identidad antes de desregistrar (`socks5_forget_paused_listener`).

**Backlog / SOMAXCONN / RLIMIT_NOFILE / FD_SETSIZE: ¿son lo mismo?**
No. `backlog` (→SOMAXCONN, sugerencia acotada por el kernel) regula **conexiones
pendientes**, no activas. `RLIMIT_NOFILE` es el límite de fds del proceso (subible),
distinto de `FD_SETSIZE`, que acota `fd_set` en `select`.

**¿Por qué `max_connections` "bajó" de 1024 a 500?**
No bajó la capacidad: se **sinceró**. Cada túnel CONNECT usa **dos** fds (cliente +
origen), más listeners y reservas. Con `FD_SETSIZE` 1024: `(1024 − 24)/2 ≈ 500` es
el techo real. Antes se anunciaba el doble de lo alcanzable.
⚠️ evitar: la reserva de 24 fds también es una constante sin benchmark.

### B.5 Validación de escala / stress tests

**¿Qué mide tu batería de estrés y con qué escenarios?**
Tres: **gate** de 500 túneles 10 s con payload verificado; **búsqueda de máximo**
hasta 600; **throughput** de 64 MiB en concurrencias 1/50/100/250/500 (3 repes,
mediana); **soak** de 500 durante 60 s con CPU/RSS desde procfs. Backend echo,
proxy y carga en procesos separados con fork/exec+pipes; auxiliares con `poll`.

**¿Por qué procesos separados y `poll` en los auxiliares?**
Para que el event loop del generador/echo no se mezcle con el proxy, y para que el
instrumento no reproduzca el límite `FD_SETSIZE` del servidor y oculte el cuello
medido.

**¿El máximo observado es el máximo real del proxy?**
No necesariamente. Es el máximo **en ese entorno** y hasta el techo 600 del runner;
cuando llega a 600 sin fallar, el reporte lo marca como **cota inferior**.

**El reporte muestra "degradación" negativa (−196 %, −1247 %). ¿Está mal?**
La fórmula es `(baseline − valor)/baseline`; si el throughput agregado **sube** con
concurrencia, da negativo: matemáticamente consistente pero de nombre confuso.
Sería mejor reportar cambio relativo con signo o separar throughput agregado y por
conexión.
⚠️ evitar: presentar los MiB/s como resultado universal. Son **una corrida local
fechada** en loopback (no WAN), cubre IPv4+auth, no FQDN/IPv6.

### B.6 Administración / cliente SMCP

**Tu commit `admin` era completo, ¿por qué quedó afuera?**
Era un prototipo de rama (`b22abe4`, sólo en `issue-8`) con decisiones de protocolo
propias. El equipo integró otra línea. La historia muestra mi prototipo, pero **no
corresponde confundirlo** con la versión entregada.
⚠️ evitar: decir "b22abe4 es el protocolo administrativo actual". Además ese
prototipo no tenía TLS, access log ni timeout, reenviaba credenciales en claro y
heredaba el máximo de 10 usuarios.

**¿Qué es `smcp_result` y por qué un enum en vez de un bool?**
`OK / REJECTED / TRANSPORT_ERROR`. Un bool obligaba a cerrar la sesión ante un
`-ERR` legítimo (p. ej. usuario inexistente). Con el enum, un rechazo de aplicación
deja la sesión usable y sólo un error de **transporte** la corta.

**¿Por qué el greeting "+OK SMCP" se acepta sólo en AUTH?**
Es el primer intercambio de la sesión. Aceptarlo en cualquier comando permitiría que
una línea espontánea desalinee todas las respuestas siguientes; después de AUTH cada
respuesta debe corresponder a un comando (framing). Prefiero fallar explícito.

**¿Por qué procesás una respuesta a la vez y no parseás todos los comandos juntos?**
El buffer de salida es acotado. Si genero varias respuestas antes de drenarlo puedo
fragmentar una o perder backpressure. Si el socket bloquea, conservo el output y
pauso el parsing de nuevos comandos.

### B.7 Merges y autoría

**¿Qué hiciste en un merge además de "apretar merge"?**
Depende del merge. En los de diff combinado vacío sólo integré una línea (no soy
autor de esos commits). En **9d6eb78** el diff combinado tiene 7 rutas: reconcilié
la modularización de `refactor/structure` con stress/retry_accept/tests de main.
Eso prueba trabajo de **integración**, no autoría de todas las líneas.
⚠️ evitar: "escribí todo lo que entró en mis merges" y "un diff combinado vacío
prueba que no resolví conflictos".

**¿Cómo distinguís tu autoría real de lo que sólo integraste?**
Comparando los dos padres y los historiales exclusivos de cada lado
(`git log p1..p2`), y con `git show --cc`. Un merge firmado por mi correo prueba
participación en la integración, no autoría del código de la otra rama.

---

## Parte C — Cierre: invariantes para defender

La defensa fuerte no es "cada versión fue perfecta", sino explicar los
**invariantes** y su evolución:

- Ningún I/O de sockets debe bloquear el selector.
- Cada byte pendiente se conserva hasta enviarse o fallar (partial I/O).
- Ninguna conexión se libera mientras un fd o job DNS la referencia.
- Los timeouts miden duraciones **monotónicas**.
- La escala se demuestra con metodología reproducible **y sus límites**.
- Un merge integra historias, pero no transfiere autoría.

> [!note] Transparencia
> Si preguntan por asistencia de IA: el commit `c9275a8` lleva
> `Co-Authored-By: Claude Opus 4.8`. Debo poder explicar el cambio (buffer de
> `read_line` para no hacer un `recv` por byte), su revisión y las reglas
> académicas aplicables.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[Protos TP]] — el TP defendido
- [[Protos TP - Defensa de commits]] — el detalle commit a commit
- [[spec]] — la especificación del protocolo
- [[10. Protos - Sockets]] — la API que se defiende

<!-- notas-relacionadas:fin -->
