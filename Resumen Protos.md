---
temas:
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - cheatsheet
  - protos
Created: 2026-05-14
Materia: "[[protos.base|protos]]"
---
# Resumen Protos
---

## Conceptos

### HTTP [[2. Protos HTTP -resumen claude]]

**Versiones:**

| Versión | Características |
|---|---|
| 1.0 | Una conexión TCP por recurso |
| 1.1 | Conexiones persistentes, pipelining, cache, negociación de contenido |
| 2 | Binario, compresión de headers, multiplexación, server push |

**Métodos + idempotencia:**

| Método | Idempotente | Safe | Cacheable | Uso |
|---|---|---|---|---|
| GET | Sí | Sí | Sí | Obtener recurso |
| HEAD | Sí | Sí | Sí | Solo headers |
| PUT | Sí | No | No | Crear/reemplazar recurso |
| DELETE | Sí | No | No | Borrar recurso |
| POST | No | No | No | Enviar datos al servidor |
| OPTIONS | Sí | Sí | No | Consultar métodos disponibles |
| TRACE | Sí | Sí | No | Diagnóstico (detectar proxy) |

> [!warning] Trampa clásica
> `GET /items/123/delete` viola safe + idempotencia. Los **pre-fetchers** (extensiones, bots) ejecutan GETs arbitrarios y pueden borrar datos. Solución: usar `DELETE /items/123`. Si DELETE falla, `rm` fue, igual que si se llama dos veces — sigue siendo idempotente porque el estado final es "recurso borrado".

> [!warning] POST no es cacheable
> Ni en browser ni en proxy. Preguntas de parcial afirman que si la respuesta llegó OK se cachea → **Falso** si fue POST.

**Status codes importantes:**

| Código | Significado |
|---|---|
| 200 | OK |
| 301 | Moved Permanently |
| 304 | Not Modified (cache válido) |
| 400 | Bad Request |
| 401 | Unauthorized (no autenticado) |
| 403 | Forbidden (sin permiso) |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 409 | Conflict |
| 410 | Gone (borrado permanentemente; a diferencia de 404 se sabe que existió) |
| 415 | Unsupported Media Type |
| 422 | Unprocessable Entity |
| 503 | Service Unavailable |

**Cache HTTP:**

| Directiva | Significado |
|---|---|
| `Cache-Control: max-age=3600` | Cachear 1h; luego revalidar |
| `Cache-Control: must-revalidate` | No servir stale: revalidar obligatoriamente al expirar |
| `Cache-Control: no-store` | No cachear nunca |
| `Last-Modified` + `If-Modified-Since` | Revalidación por fecha |
| `ETag` + `If-None-Match` | Revalidación por tag (más precisa) |

Flujo: cliente pide → si fresh en cache → responde sin ir al servidor. Si stale → revalida → servidor responde 304 (sin body) o 200 (cuerpo nuevo).

> [!tip] Cache en browser y proxy
> La "única forma de no acceder al servidor" NO es solo el proxy — el **browser** también puede tener la copia cacheada.

**Headers clave:**

| Header | Dirección | Uso |
|---|---|---|
| `Host` | request | Obligatorio en HTTP/1.1. Al usar IP directa en URL, se envía la IP como Host → puede romper multihoming/HTTPS |
| `Set-Cookie` | response | Servidor envía cookie al cliente |
| `Cookie` | request | Cliente envía cookie al servidor |
| `X-Forwarded-For` | request | IP real del cliente detrás de reverse proxy |
| `Authorization: Basic <b64>` | request | Base64 reversible → NO seguro sin TLS |
| `Transfer-Encoding: chunked` | response | Cuerpo en trozos; útil cuando no se conoce el tamaño antes de enviar |

**Cookies:** mecanismo para simular **sesión** sobre HTTP stateless. El servidor las emite con `Set-Cookie`; el cliente las adjunta en cada request con `Cookie`.

**Proxy reverso (nginx):** el cliente habla con nginx; el backend ve como origen a nginx (127.0.0.1). Solución: `proxy_set_header X-Forwarded-For $remote_addr;`

**TLS vs SSL:**
- **TLS**: mismo puerto estándar; negociación TLS sobre la misma conexión → **STARTTLS**.
- **SSL (legacy)**: puerto especial dedicado a TLS desde el inicio.

> [!important]
> "TLS se diferencia de SSL en que la conexión se inicia sin seguridad por el puerto estándar y luego se negocia TLS sobre la misma conexión (vs SSL que requería puerto especial)" → **Verdadero**.

---

### DNS [[3. Protos DNS -resumen claude]]

**Jerarquía:** árbol invertido. Raíz (`.`) → TLDs (`.com`, `.ar`) → SLDs → subdomains.

**Tipos de servidores:**

| Tipo | Función |
|---|---|
| Raíz | 13 root servers; redirigen a TLDs |
| TLD | Responsables de `.com`, `.ar`, etc. |
| Autoritativo | Fuente definitiva para una zona |
| Recursivo/Caché | Realiza la consulta completa; cachea TTL |
| Local | Cada ISP; proxy de consultas |

**Registros DNS (RR):**

| Tipo | Uso |
|---|---|
| A | nombre → IPv4 |
| AAAA | nombre → IPv6 |
| NS | zona → nameserver |
| CNAME | alias → canónico |
| MX | zona → mail server (con prioridad; menor número = mayor prioridad) |
| PTR | IP inversa → nombre |
| SOA | Start of Authority (serial, refresh, retry, expire, min-TTL) |
| TXT | texto libre (SPF, DKIM, verificaciones) |

**Cómo sabe el cliente SMTP a qué servidor conectarse:** lee el dominio luego del `@`, hace consulta **MX** al DNS, obtiene el servidor de correo del dominio destino.

**Proceso de resolución de `pampero.it.itba.edu.ar`:**
1. DNS local → root: ¿NS de `.ar`?
2. → NS de `.ar`: ¿NS de `edu.ar`?
3. → NS de `edu.ar`: ¿NS de `itba.edu.ar`?
4. → NS de `itba.edu.ar`: ¿NS de `it.itba.edu.ar`?
5. → NS de `it.itba.edu.ar`: IP de `pampero`

> [!warning] Errores típicos de BIND9 en exámenes
> 1. **RNAME en SOA usa `.` no `@`**: `admin@foo.com` se escribe `admin.foo.com.`
> 2. **FQDN sin punto final**: `ns1.foo.com` (sin `.`) se concatena con `$ORIGIN` → incorrecto. Siempre terminar FQDNs con `.`.
> 3. **`www IN A pampero` inválido**: tipo A requiere IP. Para alias: `www IN CNAME pampero`.
> 4. **Serial no incrementado** → secundarios no propagan el cambio.
> 5. **Refresh alto** → propagación lenta; reducirlo para propagación rápida.

---

### SMTP / POP3 / IMAP [[4. Protos Mail -resumen claude]]

**Puertos:**

| Protocolo | Puerto | Notas |
|---|---|---|
| SMTP | 25 | Servidor a servidor |
| SMTP AUTH / Submission | 587 | Cliente a servidor (STARTTLS) |
| POP3 | 110 | |
| POP3S | 995 | |
| IMAP | 143 | |
| IMAPS | 993 | |

**Transacción SMTP manual:**
```
HELO cliente.foo.com
MAIL FROM: <remitente@foo.com>
RCPT TO: <destinatario@bar.com>
DATA
Subject: Asunto
From: Nombre <remitente@foo.com>
To: Destinatario <destinatario@bar.com>
MIME-Version: 1.0
Content-Type: text/plain; charset="UTF-8"

Cuerpo del mensaje
.
QUIT
```
Fin del body: línea con solo `.` → `\r\n.\r\n`.

> [!warning] Trampa clásica
> "SMTP usa TCP → garantiza que el mail será recibido por el destinatario final" → **Falso**. Solo garantiza entrega al **servidor SMTP saliente** (MTA propio). El servidor del destinatario puede rechazarlo o estar caído.

**POP3 vs IMAP:**

| | POP3 | IMAP |
|---|---|---|
| Almacenamiento | Local (descarga y puede borrar del servidor) | Servidor (centralizado) |
| Múltiples dispositivos | No | Sí |
| Puerto | 110 | 143 |

**MIME:** extensión de mail para contenido no-ASCII.
- `MIME-Version: 1.0`
- `Content-Type: multipart/alternative; boundary="---B"`
- `Content-Transfer-Encoding: base64` | `quoted-printable` | `7bit`

**Tipos multipart:** `mixed` (adjuntos), `alternative` (mismo contenido en distintos formatos), `related` (HTML + imágenes inline), `digest`, `report`.

**Base64:** 3 bytes → 4 chars (+33%). Para evitar quoted-printable (legible por todos los MUAs), usar base64.

**SPF:** registro TXT/SPF en DNS que lista IPs autorizadas a enviar mail del dominio. El receptor consulta DNS del remitente para verificar.

---

### DHCP [[6. Protos Red -resumen claude]]

**DORA (4 mensajes):**
```
Cliente                             Servidor DHCP
  |-- DISCOVER (broadcast) -------->|  src: 0.0.0.0:68, dst: 255.255.255.255:67
  |<-- OFFER (broadcast/unicast) ---|  yiaddr = IP ofrecida
  |-- REQUEST (broadcast) -------->|  "acepto esa IP"
  |<-- ACK (broadcast/unicast) ----|  lease_time = X
```

> [!tip] Detalles para preguntas Wireshark
> - Los 4 mensajes tienen el **mismo XID** (Transaction ID).
> - El cliente usa broadcast porque aún no tiene IP (RFC 2131 §2).

**Reserva por MAC:** `host { hardware ethernet 08:00:27:AE:C8:6A; fixed-address 192.168.100.8; }`

---

### TCP [[5. Protos Transporte -resumen claude]]

**3-way handshake:**
```
Cliente                      Servidor
  |-- SYN seq=x ----------->|  LISTEN → SYN_RCVD
  |<-- SYN-ACK seq=y,ack=x+1|
  |-- ACK ack=y+1 ---------->|  → ESTABLISHED
```

**Máquina de estados (estados clave):**
```
CLOSED → LISTEN → SYN_RCVD → ESTABLISHED (passive open)
CLOSED → SYN_SENT → ESTABLISHED (active open)
ESTABLISHED → FIN_WAIT_1 → FIN_WAIT_2 → TIMED_WAIT → CLOSED (quien cierra primero)
ESTABLISHED → CLOSE_WAIT → LAST_ACK → CLOSED (quien recibe FIN)
```

**Control de flujo vs congestión:**

| | Control de flujo | Control de congestión |
|---|---|---|
| Propósito | Receptor no saturado | Red no saturada |
| Mecanismo | Campo `Window` en ACK | AIMD, slow start, cwnd |

> [!warning]
> "El control de congestión TCP evita enviar info que el otro extremo no puede procesar" → eso es **control de flujo**. **Falso** para congestión.

**Window scaling:** campo Window de TCP = 16 bits (máx 64 KB). Para redes modernas, se usa la **opción TCP** `WSopt` (RFC 7323) que agrega factor de escala → ventanas de hasta ~1 GB. No se agregó campo nuevo al header; se usó el mecanismo de opciones.

> [!warning] send() bloqueante
> `send()` SÍ es bloqueante, pero NO espera ACKs. Bloquea cuando el **buffer de salida del kernel** no tiene espacio libre.

**Sockets:**
- **Pasivo (listen):** acepta conexiones. Puerto fijo. Solo acepta — **no recibe datos**.
- **Activo:** uno por conexión. Identificado por 4-tupla (IP src, port src, IP dst, port dst).

---

### UDP

- Sin conexión, sin confiabilidad, sin orden, header 8 bytes.
- Si la respuesta no entra en un datagrama → **la aplicación** debe gestionarlo.
- Analogía: "carta recibida sin validación" → **UDP**.

---

### IPv4 [[6. Protos Red -resumen claude]]

**Header (20 bytes mínimo):** Vers, HLen, ToS, Total Length, Identification, **Flags (DF/MF)**, **Fragment Offset**, **TTL**, Protocol, Checksum, Src IP, Dst IP.

- **MF=1** → hay más fragmentos; **MF=0** → último fragmento.
- **Fragment Offset** en unidades de 8 bytes.
- **TTL** decrementado en cada router. Al llegar a 0 → descarte + ICMP Type 11.
- IP es **no confiable**, **sin ACKs**.

**Fragmentación (paquete 1700B, MTU 1500):**
- Frag 1: 1500B (1480 payload), MF=1, Offset=0
- Frag 2: 220B (200 payload), MF=0, Offset=185 (1480÷8)

---

### IPv6

**3 ventajas sobre IPv4 (para el parcial):**
1. **Mayor cantidad de direcciones** (128 bits).
2. **Header de tamaño fijo** (40 bytes).
3. **MAC embebida en la dirección** (EUI-64) → no necesita ARP.

**Tunneling:** IPv6 puede encapsularse en IPv4 → no requiere infraestructura 100% IPv6.

> [!warning]
> "IPv6 requiere routers intermedios IPv6" → **Falso**.

**Notación:** `::` solo una vez (reemplaza la secuencia más larga de grupos de ceros).

---

### ICMP

| Type | Mensaje |
|---|---|
| 0 | Echo Reply |
| 3 | Destination Unreachable |
| 5 | Redirect (ignorado por defecto — spoofing) |
| 8 | Echo Request |
| 11 | Time Exceeded (traceroute) |

**ICMP Redirect ignorado:** atacante podría redirigir tráfico por gateway malicioso → MITM.

**`***` en traceroute:** el router no notificó TTL exceeded. No significa que no funcione.

> [!warning]
> "`***` aparece porque el router no responde al ICMP Echo Request" → **Falso**. Los routers no responden Echo Request. El `***` es por TTL exceeded no notificado.

---

### NAT / SNAT / DNAT

- **SNAT:** modifica IP origen + puerto origen (+ checksum).
- **MASQUERADE:** SNAT con IP pública dinámica.
- **DNAT:** modifica IP/puerto destino → port forwarding hacia servidor interno.
- **Tabla NAT:** almacena **conexiones activas**, no hosts. 4 hosts ≠ "a lo sumo 4 entradas" → **Falso**.

---

### Subnetting / VLSM

**Validez de IP en tabla de ruteo:** `IP AND Máscara == IP` (bits de host = 0).
- `10.0.0.128/24` → `10.0.0.128 AND 255.255.255.0 = 10.0.0.0 ≠ 10.0.0.128` → **inválida**.
- `192.168.0.1/24` → **inválida**.

**Dirección de red** (host ID todo 0) y **broadcast** (host ID todo 1) → no asignables.

**Proceso VLSM:** ordenar mayor→menor hosts; `2^n ≥ hosts+2`; máscara = `/32-n`; asignar rangos contiguos.

**Tabla CIDR rápida:**

| CIDR | Hosts útiles |
|---|---|
| /30 | 2 |
| /29 | 6 |
| /28 | 14 |
| /27 | 30 |
| /26 | 62 |
| /25 | 126 |
| /24 | 254 |

> [!warning] Gotcha /28
> Bloques de 16: 0-15, 16-31, 32-47… `192.168.0.13` (bloque 0-15) y `192.168.0.18` (bloque 16-31) → **no comparten /28**.

---

### Tabla de Ruteo

**Entradas mínimas** (host 192.168.3.2, GW 192.168.3.1):

| Red | Máscara | Interface | Gateway |
|---|---|---|---|
| 127.0.0.0 | 255.0.0.0 | lo | — |
| 0.0.0.0 | 0.0.0.0 | eth0 | 192.168.3.1 |
| 192.168.3.2 | 255.255.255.255 | lo | — |
| 192.168.3.0 | 255.255.255.0 | eth0 | — |

Con VPN (red 10.3.0.0/24 por vpn0): agregar `10.3.0.0 / 255.255.255.0 / vpn0 / —`.

> [!tip] VPN con túnel dinámico
> No se agregan modificaciones en la tabla de ruteo.

**Longest prefix match:** si múltiples entradas coinciden, usar la de máscara más larga.

---

### ARP

**Cuándo se hace ARP request:**
1. Destino en **mismo segmento** → ARP al host destino (sin su MAC).
2. Destino en **otro segmento** → ARP al **gateway** (sin su MAC).

> [!warning]
> Para otro segmento se necesita la MAC del **gateway**, no la del host destino.

**Entradas dinámicas:** también se puede aprender la MAC del solicitante al recibir un ARP request (no solo al recibir respuesta propia).

**ARP Spoofing:** ARP replies no solicitados con IPs legítimas → MACs maliciosas → MITM.

**Modo promiscuo:** captura todo solo con **HUB**. Con switch: solo ves tu propio tráfico.

---

### Capa de Enlace

**Función principal:** transportar paquetes entre **hosts adyacentes**.

**Control de flujo:** regular velocidad entre nodos adyacentes (no end-to-end).

> [!warning]
> "Cadena de enlaces confiables → red confiable" → **Falso**. Enlace confiable ≠ confiabilidad end-to-end.
> "TTL existe por switches en ciclo" → **Falso**. Switches no procesan IP. TTL evita loops entre **routers**.

---

### Túneles SSH [[9. Protos SSH -resumen claude]] [[SSH Practica]]

| Tipo | Flag | Socket pasivo en | Redirige a | Uso típico |
|---|---|---|---|---|
| Local | `-L localport:host:remoteport` | Cliente | Host remoto vía SSH server | Acceder servicio remoto/interno |
| Remoto | `-R remoteport:host:localport` | Servidor SSH | Host local vía cliente | Exponer servicio local; sortea NAT |
| Dinámico | `-D localport` | Cliente (SOCKS5) | Cualquier destino vía SSH | Proxy general, bypass firewall |

```bash
# Local: acceder a web server en red privada del server
ssh -L 8080:10.1.0.10:80 user@sshserver
curl localhost:8080

# Remoto: exponer BD local en pampero
ssh -R 9999:localhost:5432 bpessagno@pampero.itba.edu.ar
# Desde pampero: psql -h localhost -p 9999

# Dinámico: proxy SOCKS5
ssh -D 1080 bpessagno@pampero.itba.edu.ar
curl -x socks5h://localhost:1080 ifconfig.me
```

> [!warning] Local vs remoto NO son simétricos
> "Si puedo con túnel remoto, también con local" → **Falso**.
> Remoto: cliente inicia conexión SSH (sortea NAT). Local: requiere alcanzar el servicio directamente (puede estar bloqueado).

> [!tip] Sortear proxy corporativo
> Crear **túnel dinámico** SSH (`-D`) hacia servidor externo. El tráfico sale del servidor externo, invisible para el proxy corporativo.

**Fingerprint:** `ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key.pub`. Primera conexión: verificar manualmente y guardar en `~/.ssh/known_hosts`.

**socks5 vs socks5h:** `socks5://` → DNS resuelto localmente. `socks5h://` → DNS resuelto en el servidor proxy (más privado).

---

### Modelo de capas

> [!warning]
> "Si capa OSI es con conexión, la superior también" → **Falso**. Capas independientes. HTTP (stateless) sobre TCP (orientado a conexión).

### RIP

**RIPv2:** protocolo de ruteo, vector de distancia, **UDP puerto 520**.

> [!warning]
> "Red de un solo segmento + RIPv2 está bien" → **Falso**. Sin múltiples redes/subredes no tiene sentido usar protocolo de ruteo.

---

## Práctico

### VirtualBox — Tipos de red

| Tipo | Descripción | Uso |
|---|---|---|
| NAT | VM sola + SNAT automático | Internet sin config |
| NAT Network | Varias VMs en LAN virtual + SNAT | VMs entre sí + Internet |
| Host-Only | LAN VMs ↔ máquina real; sin NAT | SSH desde host a VM |
| Internal Network | Solo VMs; máquina real excluida | Topología aislada |
| Bridge | VM en red física real | Acceso a red real |

> [!warning] WiFi ITBA + Bridge Adapter no funciona. Usar Host-Only o Internal.

**Clonar VM:** "Generate new MAC addresses for all network adapters".

**Modo promiscuo:** adaptador → Advanced → Promiscuous Mode: Allow All.

---

### Wireshark — Filtros

[[Analisis Wireshark | Analizar Wireshark]]

```
# Protocolos
ip  icmp  tcp  udp  dns  http  dhcp  smtp  arp  ssh

# Por IP
ip.src == 1.2.3.4
ip.dst == 1.2.3.4
ip.addr == 1.2.3.4

# Por puerto
tcp.port == 80
udp.port == 53
tcp.srcport == 443

# Combinaciones
dns && ip.addr == 8.8.8.8
http && ip.src == 192.168.1.10
tcp.port == 25 || tcp.port == 587
```

---

### Configuración de red Linux

```bash
# Ver interfaces
ifconfig          # activas
ifconfig -a       # incluye apagadas
ip addr           # moderno

# Asignar IP
ifconfig enp0s8 192.168.1.1/24
ip addr add 192.168.1.1/24 dev enp0s8

# IP forwarding
sysctl net.ipv4.ip_forward=1
echo 1 > /proc/sys/net/ipv4/ip_forward

# Tabla de ruteo
route -n
ip route

# Agregar rutas
ip route add 10.0.0.0/24 via 192.168.1.1       # via gateway
ip route add 10.0.0.0/24 dev enp0s8             # enlace directo
ip route add default via 192.168.1.1             # default
ip route replace 10.0.0.0/24 via 192.168.1.2    # reemplazar

# Borrar rutas
ip route delete 10.0.0.0/24

# Qué ruta usaría
ip route get 8.8.8.8
```

---

### NAT con iptables

```bash
# Ver reglas
iptables -L -t nat
iptables -L -t nat --line-numbers

# SNAT con IP fija
iptables -t nat -A POSTROUTING -o enp0s3 -j SNAT --to-source 200.1.2.3

# MASQUERADE (IP dinámica)
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# DNAT / Port forwarding
iptables -t nat -A PREROUTING -d 200.1.2.3 -p tcp --dport 80 -j DNAT --to-destination 192.168.1.10:8080

# Redirigir a proxy transparente
iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-ports 8080

# Borrar regla por número
iptables -t nat -D POSTROUTING 1

# Habilitar forwarding (necesario para router)
sysctl net.ipv4.ip_forward=1
iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
```

---

### ARP

```bash
# Ver tabla ARP
arp -n
ip neigh

# Entrada estática
arp -s 192.168.1.5 AA:BB:CC:DD:EE:FF
ip neigh add 192.168.1.5 lladdr AA:BB:CC:DD:EE:FF dev enp0s3

# Borrar
arp -d 192.168.1.5
ip neigh flush all

# arping (detectar IP duplicada: dos replies)
arping 192.168.1.5 -I enp0s3

# ARP Spoofing manual
arping <ip_victima> -I enp0s3 -S <ip_gateway>
arping <ip_gateway> -I enp0s3 -S <ip_victima>

# ARP Spoofing con arpspoof (dsniff)
arpspoof -i enp0s3 -c both -t 192.168.1.10 -r 192.168.1.1

# MITM transparente
mitmproxy --mode transparent -w capture.http
iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-ports 8080
```

---

### HTTP — curl, netcat y wget

```bash
# curl
curl -i http://ejemplo.com                          # con headers de respuesta
curl -H "Accept: text/plain" -H "Accept-Language: es" URL
curl -d "param=valor" URL                           # POST
curl -v URL                                         # verbose
curl --insecure URL                                 # ignorar cert TLS
curl -x socks5://localhost:1080 URL                 # proxy SOCKS5 (DNS local)
curl -x socks5h://localhost:1080 URL                # proxy SOCKS5 (DNS en proxy)
curl --http1.1 URL
curl --http2 URL
curl --compressed URL

# wget (descragar)
wget http://example.com/archivo.zip
wget -O archivo.zip http://example.com/file # con nombre
wget -r http://example.com # recursivo


# GET condicional (cache)
curl -H 'If-Modified-Since: Wed, 01 Jan 2025 00:00:00 GMT' URL
curl -H 'If-None-Match: "etag"' URL

# HTTP manual con netcat (CRLF obligatorio)
nc -C www.google.com 80
GET / HTTP/1.1
Host: www.google.com
                    # <Enter dos veces>
```

---

### nginx — Configuración

```bash
# Directorios clave
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/
/var/www/
/tmp/protos/

# Comandos
nginx -t                   # verificar sintaxis
systemctl reload nginx
ln -s origen destino #symlink de sites-available a enable
```

**Virtual host:**
ver [[Direccionamiento y HTTP - Practica#E36 — Virtual Hosts con nginx]]

en `/etc/ngix/sites-available`:
```nginx
server {
    listen 80;
    server_name foo;
    root /var/www/foo;
    index index.html;
}
```

segun practica:
```Nginx
server {
        listen 80 ; #se le puede agregar default_server
        listen [::]:80 ;

        root /var/www/bar;

        access_log /var/log/nginx/bar_access.log;            
        error_log  /var/log/nginx/bar_error.log;            

        index index.html index.htm index.nginx-debian.html;

        server_name bar;
        # En caso de ser default cambiar el nombre por __
        
        location / {
                try_files $uri $uri/ =404;
        }
}
```

**Reverse proxy:**
en `/etc/ngix/sites-available`:
```nginx
server {
    listen 80;
    server_name foo.example.org;
    location / {
        proxy_pass http://backend:8080/;
        proxy_set_header Host backend; #puede ser $host
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Parcial-Protos $http_x_parcial_protos;
    }
}
```

>[!important]
>el `proxy_set_header` se pone en base a lo que pidan. si te piden que mantengan un header simplemente poner el header en *snake_case* con `http` adelante
>ejemplo:
>para mantener el header X-Mi-Header necesito poner:
> `proxy_set_header X-Mi-Header $http_x_mi-h:weader;`

**HTTPS con mkcert:**
```bash
mkcert -install
mkcert foo             # genera foo.pem y foo-key.pem
```
```nginx
server {
    listen 443 ssl;
    server_name foo;
    ssl_certificate /tmp/foo.pem;
    ssl_certificate_key /tmp/foo-key.pem;
    location / {
        proxy_pass http://localhost:8080/;
    }
}
# Redirect HTTP → HTTPS
server {
    listen 80;
    server_name foo;
    return 301 https://$host$request_uri;
}
```

**Compresión:**
```nginx
gzip on;
gzip_types text/html text/plain application/json text/css application/javascript;
```

---

### DNS / BIND9
[[BIND - Configuracion de Zonas DNS]]

```bash
# Herramientas
dig @8.8.8.8 www.itba.edu.ar A         # consultar al server 8.8.8.8
dig +short www.itba.edu.ar              # solo respuesta
dig MX itba.edu.ar                      # registros MX
dig NS itba.edu.ar                      # registros NS
dig -x 157.92.27.21                     # reverse DNS
dig +trace www.itba.edu.ar              # trazado completo desde raíz
dig AXFR itba.edu.ar @ns1               # transferencia de zona
host www.itba.edu.ar
nslookup www.itba.edu.ar
```

**Archivo de zona (`/etc/bind/db.foo.com`):**
```dns
$TTL 86400
$ORIGIN foo.com.

@   IN SOA  ns1.foo.com.  admin.foo.com. (
                2026051401  ; Serial — incrementar en cada cambio
                3600        ; Refresh
                600         ; Retry
                1209600     ; Expire
                300 )       ; Negative TTL

@       IN  NS   ns1.foo.com.
ns1     IN  A    192.168.1.1
@       IN  A    192.168.1.2
www     IN  CNAME foo.com.
@   1w  IN  MX 10  mail.foo.com.
mail    IN  A    192.168.1.3
@       IN  TXT  "v=spf1 mx ~all"
```

**`/etc/bind/named.conf.local`:**
```
zone "foo.com" {
    type master;
    file "/etc/bind/db.foo.com";
};
```

> [!warning] Errores típicos BIND9 — Parcial 2Q2025
> 1. `admin@it.itba.edu.ar.` → `admin.it.itba.edu.ar.`
> 2. `ns1.it.itba.edu.ar` sin `.` final → agregar `.`
> 3. `www IN A pampero` → `www IN CNAME pampero`
> 4. Incrementar serial
> 5. Reducir Refresh para propagación rápida

**Ejemplo Joaco:**
![[Pasted image 20260527155221.png]]
```bind9
$TTL 1h
$ORIGIN demiloo.com.ar.

@   IN SOA ns.demiloo.com.ar. demiloo.leak.com.ar. (
        2026033101 ; Serial
        7d         ; Refresh
        1d         ; Retry
        10d        ; Expire
        1m         ; Negative TTL
)

; Servidor autoritativo de la zona
@       IN NS      ns.demiloo.com.ar.
ns      IN A       <IP_DE_TU_SERVIDOR_DNS>

; La raíz de la zona demiloo.com.ar apunta a la IP de foo.leak.com.ar
@       1h IN A    <IP_DE_FOO_LEAK_COM_AR>

; www.demiloo.com.ar es alias de la raíz de la zona
www     2w IN CNAME demiloo.com.ar.

; MX iguales a los de proto.leak.com.ar, manteniendo el orden
@       1w IN MX   1 nsmail.demiloo.com.ar.
@       1w IN MX   3 ns2mail.demiloo.com.ar.
@       1w IN MX   3 n23mail.demiloo.com.ar.

; Hosts de mail
nsmail  IN A       2.2.2.2
ns2mail IN A       2.2.2.3
n23mail IN A       2.2.2.4
```

---

### SMTP / POP3 manual

donde se recibe se define en `/etc/postfix/main.cf`
default se guarda en `/var/mail/`
pero con la linea `home_mailbox= Maildir/` se cambia a `~/Maildir/new`
`systemctl restart postfix`

```bash
# SMTP: usar -C para CRLF obligatorio
nc -C mail.foo.com 25
EHLO mihost
MAIL FROM: <remitente@foo.com>
RCPT TO: <40123@mail.foo.com>
DATA
MIME-Version: 1.0
Subject: =?UTF-8?B?wqFCdWVub3MgZMOtYXMh?=
From: Nombre Apellido <remitente@itba.edu.ar>
To: Destinatario <40123@mail.foo.com>
X-Parcial: 2024/2
Content-Type: text/plain; charset="UTF-8"
Content-Transfer-Encoding: base64

wqFIb2xhIG11bmRvIQ==

.
QUIT:

# POP3
nc mail.foo.com 110
USER 40123@mail.foo.com
PASS password
STAT
LIST
RETR 1
QUIT

# base64
echo -n "¡Hola mundo!" | base64          # codificar
echo "wqFIb2xhIG11bmRvIQ==" | base64 -d  # decodificar

# Subject con caracteres especiales
Subject: =?UTF-8?B?<base64 del subject>?=
```

>[!important]
>es importante que el itba valida la direccion del mail
#### Vaciar el queue
```
# Ver qué hay en la cola
mailq

# Eliminar TODOS los mails en cola
sudo postsuper -d ALL

# Si solo querés eliminar los diferidos (deferred)
sudo postsuper -d ALL deferred
```

---

### DHCP — dhcpd.conf
[[Red Practica]]

```
ddns-update-style none;

subnet 192.168.100.0 netmask 255.255.255.0 {
    range 192.168.100.10 192.168.100.200;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option routers 192.168.100.1;
    default-lease-time 3600;
    max-lease-time 86400;
}

host impresora-critica {
    hardware ethernet 08:00:27:AE:C8:6A;
    fixed-address 192.168.100.8;
}
```

**Verificar con nmap:**
```bash
sudo nmap --script broadcast-dhcp-discover \
  --script-args "broadcast-dhcp-discover.mac=AA:BB:CC:DD:EE:FF"
```

Limitar interfaz: editar `/etc/default/isc-dhcp-server` con `INTERFACES="enp0s8"`.

---

### SSH — Túneles y autenticación

```bash
# Generar claves
ssh-keygen                                         # ~/.ssh/id_ed25519
ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key.pub    # fingerprint del servidor
ssh-copy-id user@servidor                          # copiar clave pública

# Transferencia de archivos
scp user@servidor:/etc/passwd .
sftp user@servidor

# conectarse a un puerto especifico
ssh -p 2222 bpessagno@pampero.itba.edu.ar 
# pasar una clave privada
ssh -i miclave bpessagno@pampero.itba.edu.ar 

# Túnel local (pampero accede a smtp.interno:25 vía pampero)
ssh -L 9999:smtp.interno:25 user@pampero.itba.edu.ar
# App config: localhost:9999

# Túnel remoto (exponer BD local en pampero)
nc -l 45101                                        # terminal 1: servicio local
ssh -R 9999:localhost:45101 user@pampero.itba.edu.ar  # terminal 2
nc localhost 9999

# Túnel dinámico SOCKS5
ssh -D 1080 user@pampero.itba.edu.ar
curl -x socks5h://localhost:1080 ifconfig.me

# Solo tunneling sin shell
ssh -N -L 8080:google.com:80 user@pampero.itba.edu.ar

# tail -f remoto (2022-2C)
# En pampero:   
tail -f /tmp/pdc | nc -l localhost 12345
# Local túnel:  
ssh -L 12345:localhost:12345 user@pampero
# Local leer:   
nc localhost 12345 > archivo.log && tail -f archivo.log
```

---

### nmap

```bash
nmap -sS -Pn 10.0.0.1          # SYN scan (stealth, requiere root)
nmap -sT 10.0.0.1               # TCP connect scan (visible en logs)
nmap -sU 10.0.0.1               # UDP scan
nmap -sV 10.0.0.1               # detectar versión de servicios
nmap -O 10.0.0.1                # OS fingerprinting

# Diferencia -sS vs -sT:
# -sS: SYN → SYN-ACK → RST (no completa handshake, no aparece en logs app, requiere root)
# -sT: completa 3-way handshake → aparece en logs de la app

# Con ruta previa a red inaccesible
sudo ip route add 10.0.0.0/24 via 192.168.50.250
nmap -sS -Pn 10.0.0.0/24
```

---

### traceroute

```bash
traceroute www.google.com           # default: UDP a puertos altos
traceroute -M icmp www.google.com   # con ICMP Echo Request
traceroute -M tcp www.google.com    # con TCP SYN
traceroute www.google.com 4000      # paquetes de 4000B (fuerza fragmentación)
```

`***`: router no envió ICMP Time Exceeded. No significa que el router no funcione.

---

### Sockets con netcat

```bash
nc -l 9090                           # escuchar TCP
nc servidor 9090                     # conectar TCP
nc -u servidor 9090                  # UDP
nc -C servidor 25                    # forzar CRLF (SMTP)
nc -v -X 5 -x localhost:1080 host 80 # cliente SOCKS5

# Transferir archivo
nc -l 9090 > output.bin
nc servidor 9090 < input.bin

# Medir throughput
nc -l 9090 | pv > /dev/null
nc servidor 9090 < /dev/zero

sha1sum archivo                      # verificar integridad
```

---

## Banco de preguntas

> [!tip] Cómo usar
> Leer la pregunta, intentar responder, expandir el bloque para ver la respuesta.

---

### Parcial Teórico 1Q2025 (35 preguntas)

**1. V/F — "SMTP usa TCP → garantiza que el mail será recibido por el destinatario final."**

> [!success]- Respuesta
> **Falso.** TCP solo garantiza entrega al **MTA saliente** (servidor SMTP propio). No garantiza que el servidor del destinatario lo acepte ni que el destinatario exista.

**2. V/F — "Si todos los enlaces A→M, M→N, N→Z son confiables, entonces la red A-Z también lo será."**

> [!success]- Respuesta
> **Falso.** Enlace confiable = sin errores en ese tramo. La confiabilidad end-to-end la provee la **capa de transporte** (TCP).

**3. MC — ¿Qué protocolo garantiza mínimo delay? (UDP / TCP / Ninguno / SCTP / IP)**

> [!success]- Respuesta
> **Ninguno.** Ningún protocolo de la pila TCP/IP garantiza latencia mínima.

**4. V/F — "TLS se diferencia de SSL en que la conexión se inicia por el puerto estándar sin seguridad y luego se negocia TLS sobre la misma conexión."**

> [!success]- Respuesta
> **Verdadero.** TLS usa STARTTLS. SSL legacy usaba puerto dedicado desde el inicio.

**5. MC — ¿A qué se parece recibir una carta sin validar si llegó bien? (UDP / TCP / Ninguno)**

> [!success]- Respuesta
> **UDP.** Sin ACK, sin garantías.

**6. MC — Manipulación ARP para asociar IPs legítimas con MACs maliciosas:**

> [!success]- Respuesta
> **Ataque de suplantación de identidad (ARP Spoofing)**. Permite MITM en la red local.

**7. V/F — "IPv6 extremo-a-extremo requiere que todos los routers intermedios soporten IPv6."**

> [!success]- Respuesta
> **Falso.** Se puede usar **tunneling 6in4**: IPv6 encapsulado en IPv4 en los tramos sin soporte.

**8. MC — ¿Para qué sirven las MACs?**

> [!success]- Respuesta
> **Identificar dispositivos en una red local.** Operan en capa de enlace, solo dentro del mismo segmento.

**9. MC — Casos en que un host hace petición ARP (marcar correctos):**
- b) Envía paquete a host en su segmento, no conoce su MAC
- c) Envía paquete a otro segmento, no conoce MAC del host destino
- e) Envía paquete a otro segmento, no conoce MAC del gateway

> [!success]- Respuesta
> **Correctas: b y e.**
> - b: destino en mismo segmento → ARP al host.
> - e: destino en otro segmento → ARP al **gateway** (no al host destino).
> - c es trampa: para otro segmento se necesita la MAC del gateway, no la del destino.

**10. MC — Propósito del control de flujo en capa de enlace:**

> [!success]- Respuesta
> **Regular la velocidad de transmisión entre nodos adyacentes** para que no se pierdan tramas.

**11. MC — Si TCP no recibe ACK en tiempo determinado:**

> [!success]- Respuesta
> **Retransmite el paquete.**

**12. V/F — "En TCP el socket pasivo escucha en un puerto, pero luego cada socket activo escuchará en un puerto distinto."**

> [!success]- Respuesta
> **Verdadero.** Socket pasivo = puerto fijo. Socket activo = identificado por 4-tupla (incluye puerto efímero del cliente).

**13. MC — Función principal de la capa de enlace:**

> [!success]- Respuesta
> **Transportar paquetes entre hosts adyacentes.** (No "hosts finales", no "routers adyacentes".)

**14. V/F — "Si una capa OSI es orientada a conexión, la superior también lo será."**

> [!success]- Respuesta
> **Falso.** Capas independientes. Ejemplo: HTTP (stateless) sobre TCP (orientado a conexión).

**15. V/F — "Si el cable Ethernet supera la longitud máxima, ese host no podrá comunicarse."**

> [!success]- Respuesta
> **Falso.** Superar el máximo no garantiza funcionamiento correcto pero tampoco garantiza fallo total.

**16. V/F — "Red privada con 4 hosts y router NAT → a lo sumo 4 entradas en tabla NAT."**

> [!success]- Respuesta
> **Falso.** La tabla NAT almacena **conexiones activas**, no hosts. Un solo host puede tener muchas conexiones simultáneas.

**17. MC — ¿Cuáles son subredes válidas de 10.15.0.0/16?**

> [!success]- Respuesta
> **Válidas: 10.15.255.0/24, 10.15.0.0/24, 10.15.15.0/24.**
> Inválidas: `10.17.1.0/24` (no pertenece a 10.15.x.x), `10.15.1.0/16` (máscara = madre, no es subred).

**18. Ensayo — ¿Por qué los hosts ignoran ICMP Redirect por defecto?**

> [!success]- Respuesta
> **Por seguridad, para evitar spoofing.** Un atacante podría enviar ICMP Redirects falsos para redirigir tráfico por un gateway malicioso (MITM). También puede indicar un gateway inexistente.

**19. Ensayo — El cliente de mail solo pide cuenta y contraseña. ¿Cómo sabe el servidor SMTP?**

> [!success]- Respuesta
> Por el dominio luego del **`@`** en la dirección de correo. El MUA hace consulta DNS de tipo **MX** al dominio del destinatario → obtiene el hostname del servidor de correo.

**20. Ensayo — Ventana TCP de 16 bits (64 KB). ¿Aceptable hoy? ¿Qué se hizo?**

> [!success]- Respuesta
> **No es aceptable** para redes modernas. Se agregó la **opción TCP** `Window Scale` (RFC 7323) en el campo OPTIONS del header: valor real = Window × 2^scale. Permite hasta ~1 GB. No se agregó campo nuevo al header; se usó el mecanismo de opciones existente.

**21. Ensayo — V/F: "Empresa con única red de 50 hosts + RIPv2 está bien."**

> [!success]- Respuesta
> **Falso.** RIPv2 es para intercambiar información de ruteo entre múltiples redes/subredes. Con una sola red no hay nada que rutear entre redes.

**22. MC — (variante de la pregunta 9)** → Ver pregunta 9.

**23. MC — Características verdaderas de IPv4:**

> [!success]- Respuesta
> **No confiable** y **Sin reconocimiento (sin ACKs)**.
> IPv4 NO elige el mejor camino (eso es el protocolo de ruteo). Sus datos NO corresponden a la capa de transporte.

**24. MC — Objetivo de un protocolo de routing:**

> [!success]- Respuesta
> **Juntar información para que los routers puedan construir sus tablas de forwarding.** (No es forwardear paquetes — eso hace el router con la tabla ya construida.)

**25. Ensayo — V/F: "La única forma dinámica de actualizar la tabla ARP es recibir respuesta a un ARP request propio."**

> [!success]- Respuesta
> **Falso.** Los hosts también pueden aprender la MAC del **solicitante** cuando reciben un ARP request. Es una optimización configurable.

**26. MC — Entradas incorrectas en tabla de ruteo:**

> [!success]- Respuesta
> **10.0.0.128/24** (`10.0.0.128 AND 255.255.255.0 = 10.0.0.0 ≠ 10.0.0.128`) y **192.168.0.1/24** (`... = 192.168.0.0 ≠ 192.168.0.1`).

**27. MC — SNAT puede cambiar (en paquete originado en mi host):**

> [!success]- Respuesta
> **El puerto de origen y la IP de origen.** SNAT = Source NAT.

**28. Ensayo — Ejemplo de uso de túnel SSH remoto:**

> [!success]- Respuesta
> BD PostgreSQL en mi PC (detrás de NAT). Un colega en pampero necesita acceder:
> ```bash
> ssh -R 1234:localhost:5432 usuario@pampero.itba.edu.ar
> ```
> Crea socket pasivo en pampero:1234. El colega se conecta a `localhost:1234` en pampero y llega a mi BD. Mi PC inicia la conexión SSH (sortea NAT, no necesita IP pública).

**29. Ensayo — UDP: si la respuesta no entra en un datagrama, ¿qué hace UDP?**

> [!success]- Respuesta
> **UDP no ofrece ningún servicio.** Es responsabilidad de la **aplicación** dividir y gestionar múltiples datagramas.

**30. Ensayo — Alumno con proxy HTTP corporativo que filtra sitios. ¿Puede saltarlo?**

> [!success]- Respuesta
> **Sí**, con **túnel dinámico SSH** (`ssh -D`). El tráfico se encapsula en el túnel hacia un servidor externo. El proxy ve solo una conexión SSH cifrada y no puede inspeccionar ni filtrar el contenido.

**31. Ensayo — Traceroute ICMP, *** en algunas líneas. ¿El router no responde ICMP Echo Request?**

> [!success]- Respuesta
> **Falso.** Los routers no responden ICMP Echo Request (eso solo lo hace el host destino). Traceroute envía paquetes con TTL incremental; cuando TTL=0, el router **puede** enviar ICMP Time Exceeded. El `***` indica que ese router **no envió** la notificación de TTL exceeded.

**32. Ensayo — Tabla de ruteo completa del host D:**

> [!success]- Respuesta
> (IP 192.168.3.2, GW 192.168.3.1, VPN 10.3.0.0/24 por vpn0)
>
> | Red | Máscara | Interface | Gateway |
> |---|---|---|---|
> | 127.0.0.0 | 255.0.0.0 | lo | — |
> | 0.0.0.0 | 0.0.0.0 | eth0 | 192.168.3.1 |
> | 192.168.3.2 | 255.255.255.255 | lo | — |
> | 192.168.3.0 | 255.255.255.0 | eth0 | — |
> | 10.3.0.0 | 255.255.255.0 | vpn0 | — |

**33. Ensayo — Tres ventajas de IPv6 sobre IPv4:**

> [!success]- Respuesta
> 1. **Mayor cantidad de direcciones** (128 bits → 3.4×10³⁸).
> 2. **Header de tamaño fijo** (40 bytes) → routers más rápidos.
> 3. **MAC embebida en la dirección** (EUI-64) → autoconfiguración, sin necesidad de ARP.

**34. Ensayo (crédito adicional) — V/F: "send() en socket TCP bloquea porque espera los ACKs."**

> [!success]- Respuesta
> **Falso.** `send()` SÍ puede bloquear, pero NO por los ACKs. Bloquea cuando el **buffer de salida del kernel** no tiene espacio libre. Los ACKs se procesan de forma asíncrona.

**35. Ensayo — Tomcat en Docker dejó de funcionar el túnel SSH para enviar mails. ¿Por qué? ¿Cómo solucionarlo?**

> [!success]- Respuesta
> **Por qué:** cada contenedor Docker tiene su propia red. El socket del túnel SSH quedó en `localhost` del **host**, no accesible desde dentro del contenedor.
>
> **Solución:** hacer que el túnel escuche en `0.0.0.0` (no en `127.0.0.1`):
> ```bash
> ssh -L 0.0.0.0:9999:smtp.server.com:25 user@pampero
> ```
> Luego en la config de la app apuntar a la IP del host en la red Docker (ej: `172.17.0.1:9999`).

---

### Preguntas Teóricas del Curso (47 ejercicios)

#### Direccionamiento / Subredes / Tabla de Ruteo

**Ej. 4 — IPs no asignables en 10.15.0.0/16:**

> [!success]- Respuesta
> **No asignables: 10.17.5.5** (no pertenece), **10.15.0.0** (dirección de red), **10.15.255.255** (broadcast). Las demás (10.15.0.255 y 10.15.255.0) sí son asignables porque la red es /16 y sus bits de host no son todos 0 ni todos 1.

**Ej. 11 — Entradas incorrectas en tabla de ruteo:**

> [!success]- Respuesta
> **Incorrectas: 10.0.0.128/24 y 192.168.0.1/24** (bits de host ≠ 0 aplicando la máscara). Las demás son válidas.

**Ej. 32 — Subredes válidas de 10.15.0.0/16:**

> [!success]- Respuesta
> **Válidas: 10.15.255.0/24, 10.15.0.0/24, 10.15.15.0/24.** Inválidas: 10.17.1.0/24 (no pertenece), 10.15.1.0/16 (máscara igual a la red madre).

**Ej. 33 — ISP con 256 IPs y 500 casas:**

> [!success]- Respuesta
> **Sí puede dar servicio usando NAT.** Múltiples casas comparten IPs públicas.

**Ej. 45 — Subnetting /24 para 3 empresas de 80 hosts:**

> [!success]- Respuesta
> **No es posible.** Para 80 hosts se necesitan 2^7=128 → /25. Pero /25 solo da 2 subredes en un /24. Con /26 (62 útiles) tampoco alcanza.

#### VPN / Túneles SSH

**Ej. 1 — VPN con túnel dinámico: ¿cambia tabla de ruteo?**

> [!success]- Respuesta
> **No.** Túnel dinámico no modifica la tabla de ruteo.

**Ej. 10 — Saltear proxy corporativo:**

> [!success]- Respuesta
> **Sí, con túnel dinámico SSH** (`ssh -D`).

**Ej. 29 — V/F: "Con acceso SSH a pampero, ¿puedo acceder al SMTP interno?"**

> [!success]- Respuesta
> **Verdadero.** Con `ssh -L localport:smtp.interno:25 user@pampero`.

**Ej. 31 — Ejemplo de túnel remoto:**

> [!success]- Respuesta
> Ver Parcial pregunta 28. `ssh -R 1234:localhost:5432 user@servidor`. Socket pasivo en servidor; mi BD local queda accesible desde el servidor.

**Ej. 39 — V/F: "Si puedo con túnel remoto, también con local."**

> [!success]- Respuesta
> **Falso.** Propósitos opuestos. Remoto sortea NAT. Local requiere acceso directo al servicio.

#### ARP / MAC / Capa de Enlace

**Ej. 6 — Casos de ARP request:** → Ver Parcial pregunta 9.

**Ej. 16 — Control de flujo enlace:** → Ver Parcial pregunta 10.

**Ej. 20 — Función capa de enlace:** → Ver Parcial pregunta 13.

**Ej. 21 — V/F: "Modo promiscuo captura TODO el tráfico en red cableada."**

> [!success]- Respuesta
> **Falso.** Solo con **hub**. Con switch, el unicast va solo al puerto destino.

**Ej. 23 — Confiabilidad end-to-end:** → Ver Parcial pregunta 2.

**Ej. 24 — MACs:** → Ver Parcial pregunta 8.

**Ej. 34 — ARP dinámico:** → Ver Parcial pregunta 25.

**Ej. 35 — Cable largo Ethernet:** → Ver Parcial pregunta 15.

**Ej. 40 — V/F: "TTL existe por switches en ciclo."**

> [!success]- Respuesta
> **Falso.** Switches no procesan IP. TTL evita loops entre **routers**.

**Ej. 43 — V/F: "Switch es 100% seguro para unicast."**

> [!success]- Respuesta
> **Falso.** ARP Spoofing sigue siendo posible. Se requieren medidas adicionales.

#### IPv4 / IPv6

**Ej. 5 — Características verdaderas de IPv4:** → Ver Parcial pregunta 23.

**Ej. 13 — Ventajas IPv6:** → Ver Parcial pregunta 33.

**Ej. 27 — IPv6 y routers intermedios:** → Ver Parcial pregunta 7.

#### ICMP

**Ej. 3 — ICMP Redirect ignorado:** → Ver Parcial pregunta 18.

**Ej. 12 — traceroute y `***`:** → Ver Parcial pregunta 31.

**Ej. 30 — V/F: "Router puede mandar ICMP al droppear."**

> [!success]- Respuesta
> **Verdadero.** Puede enviar ICMP Destination Unreachable u otros mensajes al remitente.

#### TCP / UDP / Transporte

**Ej. 2 — UDP y respuesta grande:** → Ver Parcial pregunta 29.

**Ej. 9 — Window scaling:** → Ver Parcial pregunta 20.

**Ej. 17 — V/F: "Control de congestión evita que el otro extremo se sature."**

> [!success]- Respuesta
> **Falso.** Eso es **control de flujo**. Congestión evita que la **red** (routers) se sature.

**Ej. 18 — V/F: "Socket pasivo TCP solo acepta conexiones, no recibe datos."**

> [!success]- Respuesta
> **Verdadero.** Solo acepta (accept()). Los datos van al socket activo generado.

**Ej. 37 — Mínimo delay:** → Ver Parcial pregunta 3. **Ninguno.**

**Ej. 38 — Carta sin validar:** → Ver Parcial pregunta 5. **UDP.**

**Ej. 41 — TCP sin ACK:** → Ver Parcial pregunta 11. **Retransmite.**

#### SMTP / DNS

**Ej. 7 — Cliente de mail y servidor SMTP:** → Ver Parcial pregunta 19.

**Ej. 19 — SMTP garantiza entrega:** → Ver Parcial pregunta 1. **Falso.**

#### HTTP / Proxy / Cache

**Ej. 15 — nginx y X-Forwarded-For:**

> [!success]- Respuesta
> nginx agrega `proxy_set_header X-Forwarded-For $remote_addr;` para pasar la IP real del cliente al backend.

**Ej. 26 — IP directa en URL: ¿Siempre/A veces/Nunca funciona?**

> [!success]- Respuesta
> **A veces.** El header `Host` se envía con la IP → puede romper virtual hosting (el servidor no sabe qué sitio servir) y HTTPS (certificado es para el nombre, no la IP).

**Ej. 42 — ¿Cómo se mantiene sesión sin estado en HTTP?**

> [!success]- Respuesta
> Mediante **cookies**. El servidor emite cookie al autenticar; el cliente la adjunta en cada request.

**Ej. 44 — V/F: "200 OK → se cachea en cliente y proxy."**

> [!success]- Respuesta
> **Falso.** Si fue **POST**, no es cacheable. También depende de headers Cache-Control.

**Ej. 46 — V/F: "Única forma de no acceder al servidor es que el proxy tenga la copia."**

> [!success]- Respuesta
> **Falso.** El **browser** también puede tener el recurso cacheado.

**Ej. 47 — ¿Por qué usar HTTPS en lugar de DNS sobre UDP para resolver nombres?**

> [!success]- Respuesta
> Por **seguridad**. DNS sobre UDP es vulnerable a spoofing y MITM. HTTPS (DoH) cifra y autentica → protege confidencialidad e integridad.

#### NAT / Firewall

**Ej. 8 — SNAT puede cambiar:** → Ver Parcial pregunta 27. Puerto origen + IP origen.

**Ej. 28 — Tabla NAT con 4 hosts:** → Ver Parcial pregunta 16. **Falso.**

#### TLS/SSL

**Ej. 25 — TLS vs SSL:** → Ver Parcial pregunta 4. **Verdadero.**

#### Modelo de capas

**Ej. 36 — Capas y orientación a conexión:** → Ver Parcial pregunta 14. **Falso.**

---

### Parcial Práctico 2Q2025

**Ejercicio I — DHCP con reserva por MAC**

Configurar DHCP en `192.168.100.0/24`: impresora `08:00:27:AE:C8:6A` → siempre `192.168.100.8`; DNS Google; rango `.10`–`.200`.

> [!success]- Solución
> ```
> ddns-update-style none;
>
> subnet 192.168.100.0 netmask 255.255.255.0 {
>     range 192.168.100.10 192.168.100.200;
>     option domain-name-servers 8.8.8.8, 8.8.4.4;
>     option routers 192.168.100.1;
> }
>
> host impresora-critica {
>     hardware ethernet 08:00:27:AE:C8:6A;
>     fixed-address 192.168.100.8;
> }
> ```
> Verificar: `sudo nmap --script broadcast-dhcp-discover --script-args "broadcast-dhcp-discover.mac=AA:BB:CC:DD:EE:FF"`

---

**Ejercicio II — Descubrir host en red remota**

Gateway `192.168.50.250` para `10.0.0.0/24`. Descubrir servidor, ver servicios, acceder por HTTP, mostrar routers.

> [!success]- Solución
> ```bash
> sudo ip route add 10.0.0.0/24 via 192.168.50.250
> nmap -sS -Pn 10.0.0.0/24          # descubrir hosts/puertos
> nmap -sV -Pn 10.0.0.1              # ver servicios del host encontrado
> curl http://10.0.0.1/
> traceroute 10.0.0.1
> ```

---

**Ejercicio III — Reparar zona BIND9**

Zona `it.itba.edu.ar` con errores en `/etc/bind/db.it.itba.edu.ar`.

Errores en el archivo original del parcial:
```dns
@   IN SOA ns1.it.itba.edu.ar. admin@it.itba.edu.ar. (...)
ns1.it.itba.edu.ar      IN A 192.168.70.1    ← sin punto final
www     IN A pampero                           ← A no acepta nombre
```

> [!success]- Correcciones
> ```dns
> $TTL 604800
> @   IN SOA ns1.it.itba.edu.ar. admin.it.itba.edu.ar. (
>     2025060501  ; Serial INCREMENTADO
>     60          ; Refresh reducido para propagación rápida
>     30          ; Retry
>     2419200     ; Expire
>     604800 )
>
> @           IN NS  ns1.it.itba.edu.ar.
> @           IN MX 10 mail.it.itba.edu.ar.
>
> ns1.it.itba.edu.ar.  IN A 192.168.70.1   ; punto final agregado
> pampero              IN A 192.168.70.10
> mail                 IN A 192.168.70.10
>
> www    IN CNAME pampero                  ; era "A pampero" → CNAME
> correo IN CNAME pampero
> ```
> **Errores corregidos:** RNAME `.` en lugar de `@`; FQDN con punto final; `A pampero` → `CNAME pampero`; serial incrementado; Refresh reducido.

---

**Ejercicio IV — Reverse proxy HTTPS**

`parcial.protos.foo` corre HTTP. Exponer en `https://seguro.protos.foo` sin modificar la URL.

> [!success]- Solución
> ```bash
> mkcert -install
> mkcert seguro.protos.foo
> ```
> ```nginx
> server {
>     listen 443 ssl;
>     server_name seguro.protos.foo;
>     ssl_certificate /tmp/seguro.protos.foo.pem;
>     ssl_certificate_key /tmp/seguro.protos.foo-key.pem;
>     location / {
>         proxy_pass http://parcial.protos.foo/;
>         proxy_set_header Host parcial.protos.foo;
>     }
> }
> ```
> Verificar: `curl https://seguro.protos.foo`

---

### Ejercicios Prácticos de Exámenes Viejos

**2024-2C Ej. I — Reverse proxy con header preservation**

`http://foo.example.org/` → sirve contenido de `http://protos.sebikul.com/`, conservando `X-Parcial-Protos`.

> [!success]- Solución
> ```nginx
> server {
>     listen 80;
>     server_name foo.example.org;
>     location / {
>         proxy_pass http://protos.sebikul.com/;
>         proxy_set_header Host protos.sebikul.com;
>         proxy_set_header X-Parcial-Protos $http_x_parcial_protos;
>     }
> }
> ```
> Verificar: `curl -i -H "X-Parcial-Protos: test" http://foo.example.org`

---

**2024-2C Ej. II — curl vía SOCKS5**

Obtener `text/plain` en español de `http://ejercicio2.sebikul.com:8080/foo/` vía `socks5h://proxy.sebikul.com:1080`.

> [!success]- Solución
> ver si hay un Authorization. de ser asi es necesario hacer -u con la Password
> puede ser necesario descifrar con base64
> ```bash
> curl -x socks5h://proxy.sebikul.com:1080 \
>      -H "Accept: text/plain" \
>      -H "Accept-Language: es" \
>      http://ejercicio2.sebikul.com:8080/foo/
> ```

---

**2024-2C Ej. III — Descubrir servicio vía gateway**

Gateway `192.168.1.3` para `10.0.0.0/24`. Descubrir host y acceder por HTTP.

> [!success]- Solución
> ```bash
> sudo ip route add 10.0.0.0/24 via 192.168.1.3
> nmap -sS -Pn -sV 10.0.0.1
> curl http://10.0.0.1/
> traceroute 10.0.0.1
> ```

---

**2024-2C Ej. IV — SMTP/MIME con base64**

Enviar mail con subject `¡Buenos días!`, body `¡Hola mundo!`, header `X-Parcial: 2024/2`, legible por cualquier MUA.

> [!success]- Solución
> ```bash
> nc -C mail.sebikul.com 25
> HELO mihost
> MAIL FROM: <legajo@itba.edu.ar>
> RCPT TO: <40123@mail.sebikul.com>
> DATA
> MIME-Version: 1.0
> Subject: =?UTF-8?B?wqFCdWVub3MgZMOtYXMh?=
> From: Nombre Apellido <legajo@itba.edu.ar>
> To: 40123 <40123@mail.sebikul.com>
> X-Parcial: 2024/2
> Content-Type: text/plain; charset="UTF-8"
> Content-Transfer-Encoding: base64
>
> wqFIb2xhIG11bmRvIQ==
>
> .
> QUIT
> ```
> Codificar: `echo -n "¡Hola mundo!" | base64` → `wqFIb2xhIG11bmRvIQ==`

---

**2022-2C — tail -f remoto vía SSH**

Seguir `/tmp/pdc` de pampero con `tail -f archivo.log` local. Sin túneles vale la mitad de puntos.

> [!success]- Solución
> ```bash
> # En pampero (terminal 1):
> tail -f /tmp/pdc | nc -l localhost 12345
>
> # Local (terminal 2): crear túnel
> ssh -L 12345:localhost:12345 bpessagno@pampero.itba.edu.ar
>
> # Local (terminal 3): leer
> nc localhost 12345 > archivo.log &
> tail -f archivo.log
> ```

---

**2022-2C — BIND9 zona `demiloo.com.ar`**

A → IP de `foo.leak.com.ar` (TTL 1h); `www` CNAME raíz (TTL 2 semanas); mismos MX que `proto.leak.com.ar` (TTL 1 semana); responsable `demiloo@leak.com.ar`.

> [!success]- Solución
> ```dns
> $TTL 7d
> $ORIGIN demiloo.com.ar.
>
> @   IN SOA ns.demiloo.com.ar. demiloo.leak.com.ar. (
>     1   7d  1d  14d  1m )
>
> @       IN NS   ns.demiloo.com.ar.
> ns      IN A    1.2.3.4
>
> @   3600    IN A     <IP de foo.leak.com.ar>
> www 14d     IN CNAME demiloo.com.ar.
>
> @   7d      IN MX 20 smtp20.proto.leak.com.ar.
> @   7d      IN MX 30 smtp30.proto.leak.com.ar.
> ```

---

**SSH con llave privada protegida por passphrase (MAC de interfaz)**

Obtener shell del usuario `pdc` en `parcial.leak.com.ar:2222`. La llave privada está en `http://parcial.leak.com.ar/llave.pem` y su passphrase es la MAC de `en3` de `pampero.itba.edu.ar` (formato `XX:XX:XX:XX:XX:XX`, 17 caracteres).

> [!success]- Solución
> ```bash
> # Paso 1: obtener la passphrase (MAC de en3 de pampero)
> ssh tu_usuario@pampero.itba.edu.ar
> ifconfig
> # Buscar la línea: ether XX:XX:XX:XX:XX:XX
> # Esa MAC es la passphrase
>
> # Paso 2: descargar la llave privada
> wget http://parcial.leak.com.ar/llave.pem
> chmod 600 llave.pem    # obligatorio — SSH rechaza llaves con permisos abiertos
>
> # Paso 3: conectarse
> ssh -i llave.pem -p 2222 pdc@parcial.leak.com.ar
> # Enter passphrase for key 'llave.pem': XX:XX:XX:XX:XX:XX
>
> # Paso 4: ejecutar comando en el shell remoto
> date
> ```
> **Nota:** Si no tenés acceso SSH a pampero, podés obtener la MAC desde la misma red L2 con `ping pampero.itba.edu.ar && arp -n pampero.itba.edu.ar`. **No** hacer ARP al gateway — eso daría la MAC del router, no la de pampero.

---

**Dar acceso a internet a host con IP privada sin NAT (túnel dinámico remoto)**

`parcial.leak.com.ar` tiene IP privada y su gateway no hace NAT. Desde el shell del ejercicio anterior, hacer que `curl` pueda acceder a cualquier sitio de internet. Soluciones sin túneles valen la mitad.

> [!success]- Solución
> Usar **`-R <puerto>` sin destino** al conectarse: crea un SOCKS5 dinámico en el lado remoto que sale por nuestra máquina.
>
> ```bash
> # Desde nuestra máquina: conectarse con túnel dinámico remoto
> ssh -i llave.pem -p 2222 -R 1080 pdc@parcial.leak.com.ar
>
> # Dentro del shell de parcial.leak.com.ar:
> # Opción A: pasar el proxy en cada curl 
> curl -x socks5h://localhost:1080 http://api.ipify.org 
> curl -x socks5h://localhost:1080 https://campus.itba.edu.ar/ 
> # Opción B: exportar la variable y usar curl normalmente 
> export http_proxy=socks5h://localhost:1080 
> export https_proxy=socks5h://localhost:1080
>
> # 2. Ya se puede usar curl normalmente
> curl http://api.ipify.org
> curl https://campus.itba.edu.ar/
> ```
>
> | Flag | SOCKS5 escucha en | Útil para |
> |---|---|---|
> | `-D 1080` | nuestra máquina | nosotros navegamos por el remoto |
> | `-R 1080` (sin destino) | **servidor remoto** | el remoto sale a internet por nosotros ← este |
>
> **`socks5h://` y no `socks5://`** — la `h` hace que la resolución DNS ocurra en nuestra máquina. Sin ella, parcial intentaría resolver los nombres localmente y fallaría.

---

### Trampas y Errores Recurrentes

> [!danger] Lista de trampas que aparecen en todos los parciales

1. **Control de flujo ≠ congestión.** Flujo = receptor saturado (Window). Congestión = red saturada (cwnd/AIMD).
2. **SMTP/TCP solo garantiza entrega al MTA saliente**, no al destinatario final.
3. **`***` en traceroute** = router no notificó TTL exceeded (no que no funcione).
4. **Tabla NAT** = conexiones activas, no hosts. 4 hosts ≠ "a lo sumo 4 entradas".
5. **`send()` bloquea** por buffer de salida sin espacio, **no** por esperar ACKs.
6. **ICMP Redirect ignorado por defecto** → seguridad contra MITM.
7. **IPv6 puede tunelizarse sobre IPv4** → no requiere toda la infraestructura en IPv6.
8. **Túnel remoto sortea NAT; túnel local no.** No son simétricos ni intercambiables.
9. **Modo promiscuo útil solo con hub.** Con switch: solo ves tu tráfico.
10. **BIND9:** RNAME usa `.` no `@`; FQDN necesita punto final; `IN A nombre` inválido (usar CNAME); serial debe incrementarse.
11. **`GET /items/X/delete`** viola safe + idempotencia → prefetchers borran datos.
12. **POST no es cacheable** en ningún lado.
13. **Cache también en browser**, no solo en proxy.
14. **`Host` con IP directa** puede romper virtual hosting y TLS.
15. **WiFi ITBA bloquea Bridge Adapter** en VirtualBox.
16. **`/28`:** bloques de 16. `.13` (bloque 0-15) y `.18` (bloque 16-31) → no comparten /28.
17. **ARP request para otro segmento** → se necesita MAC del **gateway**, no del host destino.
18. **Socket pasivo** solo acepta conexiones (accept). Los datos van al socket activo.
19. **IPv4 no elige camino** (eso es el protocolo de ruteo). No envía ACKs.
20. **RIPv2 en red de un solo segmento** no tiene sentido.
