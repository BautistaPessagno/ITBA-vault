---
temas:
  - nc 
  - HTTP
  - trace
  - curl
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - p
Created: 2026-04-0317:02
Materia: "[[protos.base|protos]]"
---
# Direccionamiento y HTTP

## [HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Reference/Resources_and_specifications)

## E12
>En la siguiente URI https://tools.ietf.org/html/rfc2616#section-14.19 ¿Cómo se llama la parte “section-14.19”? ¿Se envía por la red al hacer el request? ¿Por qué? Piense usos.

se llama fragmento y se identifica por ir despues del # . no se envia en el request y es procesado por el agent. sirve mas que nada para la navegacion dentro de una pagina

## E13 
>¿Cuál es la diferencia entre una URI y una URI reference?

la diferencia es que el URI es una direccion absoluta mientras que el URI reference puede ser relativa

## E15
E15. Equivalencia de URLs según HTTP
Las tres URLs son:

a. http://abc.com:80/~smith/home.html
b. http://ABC.com/%7Esmith/home.htm
c. http://ABC.com:/%7esmith/home.htm

Reglas de comparación HTTP (RFC 7230 / RFC 3986 §6.2.2 y §6.2.3):
Scheme: es case-insensitive → http = http en todas. 
Host: es case-insensitive → abc.com = ABC.com. 
Puerto: para HTTP, el puerto por defecto es 80. Si se omite, se asume 80. Entonces :80 es equivalente a omitirlo. En (a) se indica :80 explícitamente, en (b) se omite (= 80 implícito), en (c) se pone : sin número, lo cual es un puerto vacío que también equivale al default.
Path y percent-encoding: %7E y %7e son equivalentes (los dígitos hex son case-insensitive) y ambos decodifican al carácter ~. Como ~ es un carácter unreserved según RFC 3986, %7E es equivalente a ~. Entonces ~smith = %7Esmith = %7esmith. 

# Netcat


## E17 
>Comunique dos terminales utilizando **netcat**.
```bash
#en la primera terminal escribimos 
nc -l 9090 # para iniciar a escuchar, el -l es por listen

#luego en la segunda escribimos
# nc [hostname] [port]
nc localhost 9090
```


## E18
>{W} HTTP/1.1 y otros protocolos utilizan las secuencias CR LF como marcas de fin de línea.
¿Como usted puede lograr enviar esta secuencia con netcat desde una terminal donde se
consumen caracteres desde la entrada estándar?

me contecto a wireshark usando `tcp.port == 9090` y contecto nc al puero 9090
al mandar un mensaje hola voy a encontrar el siguiente log
```
91	2.690025	127.0.0.1	127.0.0.1	TCP	61	65360 → 9090 \[PSH, ACK] Seq=1 Ack=1 Win=6380 Len=5 TSval=3034802700 TSecr=3220219600
```

![](Attachments/Pasted%20image%2020260403174339.png)
el 0a es el LF que pasa cuando hago enter. como hice el LF no se ve el CR od

pero si en lugar de pasarlo yo con enter hago con printf
```
printf "hola\r\n" | nc localhost 9090
```
recibo lo siguiente
![](Attachments/Pasted%20image%2020260403174726.png)
ahora si tengo el 0d 0a

# Respuestas E24 a E41 — HTTP

## E24 — Transfer-Encoding

**1. ¿Qué significa `Transfer-Encoding: chunked`?**

Significa que el body de la respuesta se envía fragmentado en "chunks" (trozos). Cada chunk va precedido por su tamaño en hexadecimal, seguido de CRLF, luego los datos, y otro CRLF. El final se marca con un chunk de tamaño 0. Ejemplo:

```
HTTP/1.1 200 OK
Transfer-Encoding: chunked

4\r\n
Wiki\r\n
5\r\n
pedia\r\n
0\r\n
\r\n
```

**2. ¿Qué otras codificaciones existen?**

- `gzip`: compresión con el algoritmo gzip (LZ77 + Huffman)
- `compress`: compresión LZW (obsoleto)
- `deflate`: compresión con zlib/deflate
- `identity`: sin transformación (valor por defecto)

**3. ¿Para qué sirven?**

Permiten transformar el cuerpo del mensaje durante la transferencia sin modificar el recurso original. `chunked` permite enviar respuestas cuyo tamaño total no se conoce de antemano (contenido generado dinámicamente). Las codificaciones de compresión reducen el tamaño de los datos transferidos.

**4. ¿En qué ocasiones utilizaría cada uno?**

- **chunked**: cuando el servidor genera contenido dinámicamente (streaming, respuestas grandes) y no puede calcular el `Content-Length` antes de empezar a enviar.
- **gzip/deflate**: cuando se quiere reducir el ancho de banda. Ideal para texto (HTML, CSS, JS, JSON). No tiene mucho sentido para contenido ya comprimido (imágenes JPEG, videos).

---

## E25 — Media Types

**a. ¿Qué es un media type?**

Es un identificador estandarizado que indica el formato de los datos. También conocido como MIME type. Está definido en los RFC 2045/2046 y registrado en IANA.

**b. ¿Para qué se usa?**

Para que el receptor (UA o servidor) sepa cómo interpretar y procesar el contenido recibido. En HTTP aparece en los headers `Content-Type` (qué tipo de dato se envía) y `Accept` (qué tipos acepta el cliente).

**c. ¿Cómo está estructurado?**

`tipo/subtipo ;parámetro=valor`

Donde _tipo_ es la categoría general (text, image, application, etc.) y _subtipo_ el formato específico.

**d. Media types asociados:**

|Contenido|Media Type|
|---|---|
|i. Páginas web (HTML)|`text/html`|
|ii. Texto plano|`text/plain`|
|iii. CSS|`text/css`|
|iv. JavaScript (.js)|`application/javascript`|
|v. Imágenes JPEG|`image/jpeg`|
|v. Imágenes GIF|`image/gif`|
|v. Imágenes PNG|`image/png`|
|vi. PDF|`application/pdf`|
|vii. .exe / formato desconocido|`application/octet-stream`|

---

## E26 — Conexiones persistentes

**a. Requisitos para usar conexiones persistentes en HTTP/1.1:**

En HTTP/1.1, las conexiones son persistentes **por defecto**. El cliente y el servidor deben soportar HTTP/1.1. Si alguno quiere cerrar la conexión, envía el header `Connection: close`. El servidor debe incluir `Content-Length` o usar `Transfer-Encoding: chunked` para que el cliente sepa cuándo termina cada respuesta.

**b. Ventajas:**

- Se evita el overhead del three-way handshake de TCP para cada request.
- Se evita el slow-start de TCP repetido.
- Se reduce la latencia total al descargar múltiples recursos (imágenes, CSS, JS de una página).
- Menor uso de recursos (menos sockets abiertos).

**c. ¿Qué es el pipelining?**

Es la capacidad de enviar múltiples requests HTTP **sin esperar la respuesta** del anterior, todo sobre la misma conexión TCP. Las respuestas deben llegar en el mismo orden que los requests. En la práctica se usó poco por problemas de implementación (head-of-line blocking). HTTP/2 resolvió esto con multiplexación verdadera.

---

## E27 — Métodos HTTP

**a. Métodos que provee HTTP:**

GET, HEAD, POST, PUT, DELETE, PATCH, OPTIONS, TRACE, CONNECT.

**b. ¿Qué significa que un método es idempotente?**

Un método es idempotente si hacer la misma solicitud una o varias veces produce el mismo resultado en el servidor. GET, HEAD, PUT, DELETE y OPTIONS son idempotentes. POST y PATCH no lo son (cada POST puede crear un recurso nuevo).

**c. Caso del "acelerador de internet":**

- **¿Qué sucede?** El plugin recolecta todos los links (incluyendo los de "eliminar" tipo `/items/123/delete`) y hace GET a cada uno. Al hacer GET a esos links, se eliminan todos los ítems del listado.
- **¿Quién tiene la culpa?** El desarrollador. Usó GET para una operación que modifica estado (eliminar). GET debe ser un método **safe** (sin efectos secundarios). Las eliminaciones deberían usar DELETE o POST.
- **¿Cómo solucionarlo?** Cambiar la eliminación para que use DELETE (o POST) en vez de GET. Así el plugin solo hará GET a los links, que no tendrá efecto destructivo.

**d. ¿Por qué DELETE es idempotente?**

Porque si hacés DELETE a `/items/123` una vez, el recurso se borra (200 OK). Si lo hacés de nuevo, el recurso ya no existe y recibís 404. Pero el estado del servidor es el mismo: el recurso no existe. Idempotencia no significa que la respuesta sea igual, sino que el efecto sobre el recurso es el mismo.

**e. ¿Cuándo usar POST?**

Cuando la operación no es idempotente, cuando se envían datos al servidor para ser procesados (formularios, creación de recursos), cuando el resultado depende del estado actual del servidor, o cuando los datos son demasiado grandes o sensibles para ir en la URL.

---

## E28 — TRACE

Al hacer TRACE contra `www.google.com.ar`, probablemente recibas un **405 Method Not Allowed**. Google (y la mayoría de servidores modernos) tiene TRACE deshabilitado por seguridad, ya que puede ser explotado mediante ataques Cross-Site Tracing (XST) para robar cookies y credenciales.

Si TRACE estuviera habilitado y hubiera un proxy, el body de la respuesta contendría los headers tal como llegaron al servidor (incluyendo los headers agregados por el proxy como `Via`, `X-Forwarded-For`, etc.), lo que permitiría detectar su presencia.

---

## E29 — Códigos de retorno

|Situación|Código|Nombre|
|---|---|---|
|a. Recurso se mudó permanentemente, se conoce nueva dirección|**301**|Moved Permanently|
|b. Cliente sin permiso (autenticado pero sin autorización)|**403**|Forbidden|
|c. Cliente no autenticado|**401**|Unauthorized|
|d. Recurso editado por otro desde que lo comenzamos a editar (conflicto)|**409**|Conflict|
|e. Recurso eliminado permanentemente (vs 404)|**410**|Gone|
|f. Falta un parámetro en la URL del GET|**400**|Bad Request|
|g. Formato de parámetro incorrecto|**400**|Bad Request|
|h. Media type no soportado en upload|**415**|Unsupported Media Type|
|i. Sistema externo requerido no disponible|**502**|Bad Gateway (o **503** Service Unavailable)|
|j. DELETE no soportado (solo GET)|**405**|Method Not Allowed|

**Diferencia entre 410 y 404:** El 404 significa que el recurso no se encontró (puede que exista en el futuro). El 410 indica que el recurso **existió pero fue eliminado intencionalmente** y no volverá. Los buscadores usan 410 para desindexar más rápido.

---

## E30 — Authorization Basic

**a.** El header `Authorization: Basic ...` implementa el esquema de autenticación HTTP Basic (RFC 7617). El cliente envía las credenciales codificadas en Base64 con cada request.

**b.** Sí, se puede recuperar fácilmente. Base64 **no es cifrado**, es solo codificación. Decodificando:

```bash
echo "YWxndW51c3VhcmlvOmFsZ3VuYXBhc3N3b3Jk" | base64 -d
```

Resultado: `algunusuario:algunapassword`

El formato es `usuario:contraseña`. Por esto, Basic Auth solo debe usarse sobre HTTPS.

---

## E31 — Estrategias para minimizar tráfico

HTTP provee varias estrategias:

1. **Caching** (Cache-Control, Expires): el UA o proxies almacenan respuestas y las reusan sin consultar al servidor.
2. **GET condicional** (If-Modified-Since, If-None-Match / ETags): el servidor responde 304 Not Modified si el recurso no cambió, sin enviar el body.
3. **Compresión** (Content-Encoding: gzip): reduce el tamaño del body.
4. **Conexiones persistentes**: evitan el overhead de establecer nuevas conexiones TCP.
5. **Pipelining / Multiplexación (HTTP/2)**: múltiples requests simultáneos en una conexión.
6. **Range requests** (header Range): permite descargar solo una porción del recurso.
7. **HEAD**: obtiene solo los headers sin el body.

---

## E32 — GET condicional con CURL

**a. GET condicional con fecha de última modificación:**

```bash
# Primero obtenemos la fecha de Last-Modified
curl -i http://protos.foo/

# Luego hacemos el GET condicional
curl -i -H "If-Modified-Since: Thu, 01 Jan 2025 00:00:00 GMT" http://protos.foo/
```

Si el recurso no cambió desde esa fecha, el servidor responde **304 Not Modified** sin body.

**b. GET condicional con Entity Tags:**

```bash
# Primero obtenemos el ETag
curl -i http://protos.foo/
# Supongamos que devuelve ETag: "abc123"

# Luego hacemos el GET condicional
curl -i -H 'If-None-Match: "abc123"' http://protos.foo/
```

Si el ETag no cambió, el servidor responde **304 Not Modified**.

---

## E33 — Cache-Control: max-age=3600, must-revalidate

El UA interpreta: puede usar la copia cacheada durante **3600 segundos** (1 hora) sin consultar al servidor. Pero una vez que ese tiempo expire (**must-revalidate**), el UA **debe** revalidar con el servidor antes de usar la copia cacheada. No puede servir contenido stale bajo ninguna circunstancia (ni siquiera si el servidor no responde).

---

## E34 — Shallow Etag

Un **Shallow ETag** (o "weak ETag") se genera a partir de la respuesta ya renderizada (por ejemplo, un hash MD5 del body). El servidor genera la respuesta completa, calcula su hash, y lo compara con el ETag del cliente. Si coincide, responde 304.

La ventaja: es simple de implementar. La desventaja: el servidor sigue haciendo todo el trabajo de generar la respuesta (consultar base de datos, renderizar template, etc.) — solo se ahorra el ancho de banda de enviar el body.

Un **Deep ETag**, en cambio, verifica la versión del recurso antes de generar la respuesta (por ejemplo, consultando un campo de versión en la base de datos), ahorrando también el cómputo del servidor.

---

## E35 — campus.itba.edu.ar

**a.** Los datos del formulario de login se envían mediante un **POST** con el body codificado en `application/x-www-form-urlencoded`. Los datos se ven como: `username=xxx&password=yyy`.

**b.** El servidor mantiene la sesión usando **cookies**. Tras el login exitoso, envía un `Set-Cookie` con un session ID. El browser reenvía esa cookie en cada request subsiguiente con el header `Cookie`, y así el servidor identifica al usuario.

**c.** Usando curl sin cookies, el servidor no sabe quién sos, así que probablemente te redirija al login o muestre contenido diferente. No verás lo mismo que en el browser.

**d.** Para obtener el mismo HTML, hay que enviar la cookie de sesión:

```bash
# Primero hacer login y guardar cookies
curl -c cookies.txt -d "username=TU_USER&password=TU_PASS" https://campus.itba.edu.ar/login

# Luego acceder con las cookies guardadas
curl -b cookies.txt https://campus.itba.edu.ar/tu-materia
```

---

## E36 — Virtual Hosts con nginx

Se crean dos archivos de configuración en `/etc/nginx/sites-available/`:

**Archivo `foo`:**

```nginx
server {
    listen 80;
    server_name foo;
    location / {
        return 200 "Bienvenido a Foo";
        add_header Content-Type text/plain;
    }
}
```

**Archivo `bar`:**

```nginx
server {
    listen 80;
    server_name bar;
    location / {
        return 200 "Bienvenido a Bar";
        add_header Content-Type text/plain;
    }
}
```

**Archivo default (para cualquier otro nombre):**

```nginx
server {
    listen 80 default_server;
    server_name _;
    location / {
        return 200 "What?";
        add_header Content-Type text/plain;
    }
}
```

Se habilitan con symlinks en `sites-enabled/` y se reinicia nginx.

El problema al acceder a `http://foo/` desde Chrome/Firefox es que los browsers modernos interpretan palabras sueltas como búsquedas en Google en vez de hostnames. Se soluciona escribiendo `http://foo/` explícitamente con el protocolo.

---

## E37 — Probar sin DNS

Se puede probar con **curl usando el header Host** manualmente:

```bash
curl -H "Host: foo" http://localhost/
curl -H "Host: bar" http://localhost/
curl http://localhost/   # debería mostrar "What?"
```

Esto funciona porque nginx decide qué virtual host servir en base al header `Host`, no en base al DNS.

Para acceder desde el navegador, agregar en `/etc/hosts`:

```
127.0.0.1   foo
127.0.0.1   bar
```

---

## E38 — Diferencias entre acceder, F5 y Ctrl+F5

|Acción|Comportamiento|Headers relevantes|
|---|---|---|
|**Acceder** (click en link / barra)|Usa la caché local si es válida. Si el recurso está cacheado y no expiró, no hace request al servidor.|Envía `If-Modified-Since` y/o `If-None-Match` si tiene versión cacheada expirada|
|**F5** (refresh)|Revalida con el servidor. Envía GET condicional.|Envía `If-Modified-Since` / `If-None-Match`. Puede agregar `Cache-Control: max-age=0`|
|**Ctrl+F5** (hard refresh)|Ignora completamente la caché. Descarga todo de cero.|Envía `Cache-Control: no-cache` y `Pragma: no-cache`. NO envía `If-Modified-Since` ni `If-None-Match`|

En Wireshark/DevTools se puede ver que con F5 las respuestas pueden ser 304 Not Modified, mientras que con Ctrl+F5 siempre son 200 OK con el body completo.

---

## E39 — Proxy reverso con nginx

**a/b.** Se levanta un server Python simple:

```bash
mkdir -p /tmp/protos && cd /tmp/protos
echo "Hola mundo" > index.html
python3 -m http.server 8080
```

Verificar en `http://localhost:8080/`.

**c.** Configurar nginx como proxy reverso editando el virtual host `foo`:

```nginx
server {
    listen 80;
    server_name foo;
    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Ahora al acceder a `http://foo/`, nginx recibe el request y lo reenvía internamente a Python en el puerto 8080.

**d. Ventajas del esquema:**

- **Seguridad**: el servidor de aplicación (Python) no está expuesto directamente a internet. Solo nginx recibe conexiones externas.
- **TLS/SSL**: nginx maneja HTTPS y el backend puede ser HTTP plano (terminación SSL).
- **Balanceo de carga**: nginx puede distribuir requests entre múltiples servidores backend.
- **Caché**: nginx puede cachear respuestas del backend.
- **Servir estáticos**: nginx sirve archivos estáticos eficientemente y solo pasa al backend los requests dinámicos.
- **Virtual hosting**: una IP pública, múltiples aplicaciones en distintos puertos internos.

---

## Guía 02 — Preguntas adicionales

### 1. ¿Cómo sabe un proxy que la copia en caché es válida?

Dos formas (y una tercera combinando ambas): el recurso puede tener un header indicando cuánto tiempo más es válido (`Cache-Control: max-age=...`, `Expires`), o incluir un tag de versión (`ETag`). En el segundo caso, el proxy al re-solicitar el recurso indica la versión que tiene almacenada (`If-None-Match`) y el servidor responde 304 si no cambió.

### 2. ¿Cómo mantiene sesión una aplicación web?

Principalmente mediante **cookies**. El servidor envía un `Set-Cookie` con un session ID, y el browser lo reenvía en cada request. Así, aunque HTTP sea stateless, la aplicación puede asociar requests al mismo usuario.

### 3. ¿Es correcto que la única forma de obtener un recurso sin ir al servidor es que el proxy lo tenga?

**No, es incorrecto.** El recurso también podría estar cacheado en el **propio browser** (caché local del UA), sin necesidad de que haya un proxy.

### 4. ¿Habilitaría TRACE en el servidor con el header secreto del proxy?

**No.** TRACE hace que el servidor devuelva en el body los headers tal como los recibió. Como el servidor recibe el header secreto `X-PHRASE: I AM THE PROXY` (agregado por el proxy), un cliente que haga TRACE vería esa frase secreta, comprometiendo la seguridad.

### 5. Afirmaciones:

**a. "HTTP es un protocolo de texto"** → **Depende de la versión.** HTTP/1.0 y 1.1 son protocolos de texto. HTTP/2 es un protocolo binario.

**b. "MIME es un protocolo de la capa de presentación del modelo OSI"** → **Correcto.** MIME define cómo se codifican y representan los datos, lo cual es función de la capa de presentación.

**c. "Third party cookies son cookies que un servidor deja para ser enviadas a otro servidor"** → **No es válida.** Las cookies siempre están asociadas al dominio del servidor que las creó. Una third-party cookie es una cookie seteada por un dominio **distinto** al que el usuario está visitando (típicamente por contenido embebido como publicidad o trackers), pero igualmente se envía solo al dominio que la creó, no a "otro servidor" arbitrario.

### 6. ¿Por qué algunos recursos se cachean y otros no con el mismo Cache-Control?

Una razón es que algunos recursos se obtuvieron con **GET** mientras que otros fueron solicitados con **POST**. Las respuestas a POST normalmente no se cachean por defecto. También puede influir la presencia de headers como `Vary`, `Set-Cookie`, o si la respuesta fue servida sobre HTTPS sin indicaciones explícitas de caching.

### 7. Si un recurso se cachea en mi browser, ¿también se cachea en mi proxy?

**No necesariamente.** La directiva `Cache-Control` puede incluir `private` o `public`. Si es `private`, indica que el recurso solo tiene sentido para el cliente que lo solicitó (por ejemplo, contiene datos personalizados), y los proxies intermedios no deberían cachearlo. Si es `public` (valor por defecto), los proxies sí pueden cachearlo.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [2. Protos - HTTP](2.%20Protos%20-%20HTTP.md) — teoría de HTTP
- [HTTP Practica](HTTP%20Practica.md) — laboratorio base
- [6. Protos - Red](6.%20Protos%20-%20Red.md) — direccionamiento IP
- [Cheatsheet](Cheatsheet.md) — comandos y RFCs de referencia

<!-- notas-relacionadas:fin -->
