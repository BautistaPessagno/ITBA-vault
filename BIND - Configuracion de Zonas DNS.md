---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - DNS
  - BIND
  - practica
  - guia
Created: 2026-05-27
Materia: "[[protos.base|protos]]"
temas:
  - DNS
  - BIND
  - Zona
  - Registros
---
# BIND - Configuración de Zonas DNS

Guía general para resolver ejercicios de configuración de un servidor de nombres BIND.

---

## 1. Flujo general

```
1. Obtener datos externos con dig (IPs, MX de otras zonas)
2. Instalar BIND
3. Declarar la zona en named.conf.local
4. Crear el archivo de zona con los registros
5. Verificar sintaxis y reiniciar
6. Demostrar con dig
```

---

## 2. Obtener datos previos

Antes de configurar la zona, resolvé los datos que necesitás del entorno:

```bash
# IP de otro host (para usar en un A record)
dig <hostname> A

# MX records de otra zona (para copiarlos)
dig <dominio> MX

# IP de mi propio servidor (para el glue record del NS)
ip a
# o
hostname -I
```

---

## 3. Instalar BIND

```bash
sudo apt install bind9
cd /etc/bind
```

---

## 4. Declarar la zona en `named.conf.local`

```bash
sudo nano /etc/bind/named.conf.local
```

```
zone "mi-zona.com.ar" {
    type master;
    file "/etc/bind/mi-zona.com.ar.zone";
};
```

> [!tip] Zona secundaria
> Si el servidor es **secundario** (slave), usar `type slave` y agregar `masters { <IP del primario>; };`

---

## 5. Crear el archivo de zona

```bash
sudo nano /etc/bind/mi-zona.com.ar.zone
```

### Estructura completa

```dns
$TTL <ttl-default>
$ORIGIN mi-zona.com.ar.

; ── SOA ──────────────────────────────────────────────────────────────────
@   IN  SOA  ns.mi-zona.com.ar.  contacto.dominio.com.ar. (
        2026052701  ; serial   → YYYYMMDDNN
        7d          ; refresh
        1d          ; retry
        10d         ; expire
        1h          ; negative TTL
)

; ── NS ───────────────────────────────────────────────────────────────────
@           IN  NS   ns.mi-zona.com.ar.
ns          IN  A    <IP del servidor BIND>    ; glue record

; ── A records ────────────────────────────────────────────────────────────
@       1h  IN  A    <IP>           ; raíz de la zona, TTL 1h
host1   2h  IN  A    <IP>

; ── CNAME ────────────────────────────────────────────────────────────────
www     2w  IN  CNAME  mi-zona.com.ar.         ; alias → raíz de zona

; ── MX records ───────────────────────────────────────────────────────────
@       1w  IN  MX   10  mail1.dominio.com.ar.
@       1w  IN  MX   20  mail2.dominio.com.ar.
```

---

## 6. Registros explicados

### NS — Name Server

```dns
@   IN  NS   ns.mi-zona.com.ar.
ns  IN  A    10.0.0.5
```

Declara qué servidor es **autoritativo** para la zona. El `@` representa la raíz (`mi-zona.com.ar.`). El registro A que acompaña al NS se llama **glue record** — sin él, nadie puede llegar al servidor porque no sabe su IP.

---

### A — Address

```dns
@   1h  IN  A   10.0.0.1
```

Mapea un **nombre → IPv4**. Es el registro más básico. El TTL indica cuánto tiempo los resolvers pueden cachear ese dato antes de volver a preguntar.

---

### CNAME — Canonical Name

```dns
www   2w  IN  CNAME  mi-zona.com.ar.
```

Define un **alias**: no apunta a una IP sino a otro nombre, que después se resuelve al A record. Dos reglas importantes:

> [!warning] Restricciones del CNAME
> - **Nunca en `@`** (apex de zona): el apex ya tiene SOA y NS, un CNAME no puede coexistir con otros registros.
> - **Nunca con otros registros**: un nombre que tiene CNAME no puede tener A, MX ni nada más.

---

### MX — Mail Exchanger

```dns
@   1w  IN  MX   10  mail1.dominio.ar.
@   1w  IN  MX   20  mail2.dominio.ar.
```

Define los servidores de correo de la zona. El número es la **prioridad** — menor número = mayor prioridad. Si el ejercicio pide "los mismos MX que otra zona manteniendo el orden", hay que copiar exactamente las prioridades con `dig <otra-zona> MX`.

> [!tip] ¿Necesito agregar A records para los mail servers?
> Depende de dónde viven los servidores de mail:
>
> **Caso 1 — Mail servers dentro de tu zona** (nombre sin punto final = relativo):
> ```dns
> @        1w  IN  MX  10  nsmail        ; relativo → dentro de mi-zona.com.ar
> nsmail       IN  A   2.2.2.2           ; necesitás el A record vos
> ns2mail      IN  A   2.2.2.3
> ```
>
> **Caso 2 — Mail servers en otra zona** (nombre con punto final = FQDN externo):
> ```dns
> @        1w  IN  MX  10  mail1.otro-dominio.ar.   ; FQDN externo → no es tu zona
>                                                    ; NO agregás A record, lo resuelve esa zona
> ```

---

## 7. Timers del SOA

```dns
@   IN  SOA  ns.mi-zona.com.ar.  contacto.dominio.ar. (
        2026052701  ; serial
        7d          ; refresh
        1d          ; retry
        10d         ; expire
        1h          ; negative TTL
)
```

| Timer | Qué controla |
|-------|-------------|
| **Serial** | Versión de la zona. El secundario compara con el primario — si el primario tiene mayor serial, hace zone transfer. Formato convencional: `YYYYMMDDNN` |
| **Refresh** | Cada cuánto el **secundario** le pregunta al primario si hubo cambios |
| **Retry** | Si el primario no responde, cada cuánto reintenta el secundario. Debe ser `< Refresh` |
| **Expire** | Si tras este tiempo el secundario nunca contactó al primario, **descarta la zona**. Debe ser `> Refresh + Retry` |
| **Negative TTL** | Cuánto tiempo se cachea una respuesta `NXDOMAIN` (nombre inexistente) |

> [!info] En laboratorio
> Si no hay servidor secundario los timers no tienen efecto práctico, pero la **sintaxis SOA los exige**. En un lab podés usar tiempos cortos (`1m`) para testear más rápido.

---

## 8. El email en la SOA

El segundo campo después del NS en la SOA es el email del responsable, pero con sintaxis especial:

```
admin@mi-dominio.com.ar  →  admin.mi-dominio.com.ar.
```

El `@` se reemplaza por `.` y se agrega `.` final (FQDN). Cualquier punto que ya hubiera en el usuario del email también se escapa con `\`.

---

## 9. `$TTL` y `$ORIGIN`

```dns
$TTL 1h         ; TTL por defecto para todos los registros que no especifiquen uno propio
$ORIGIN mi-zona.com.ar.   ; se agrega al final de cualquier nombre relativo
```

Si un registro tiene su propio TTL explícito (`www 2w IN CNAME ...`), ese sobreescribe el `$TTL`.

---

## 10. Verificar y reiniciar

```bash
# Verificar sintaxis del archivo de zona
named-checkzone mi-zona.com.ar /etc/bind/mi-zona.com.ar.zone

# Verificar configuración general de BIND
named-checkconf

# Reiniciar
sudo systemctl restart bind9
```

---

## 11. Demostrar con `dig`

```bash
# Registro A
dig @localhost mi-zona.com.ar A

# CNAME
dig @localhost www.mi-zona.com.ar CNAME

# MX
dig @localhost mi-zona.com.ar MX

# SOA (verifica flag "aa" = authoritative answer)
dig @localhost mi-zona.com.ar SOA
```

> [!success] Qué buscar en la respuesta
> - Flag **`aa`** (Authoritative Answer): confirma que tu servidor es autoritativo
> - Sección **ANSWER**: los registros devueltos
> - Columna TTL: verificar que coincide con lo configurado (en segundos: `1h=3600`, `1w=604800`, `2w=1209600`)

---

## 12. Conversión de TTLs

| Expresión | Segundos |
|-----------|----------|
| `1h` | 3600 |
| `1d` | 86400 |
| `1w` | 604800 |
| `2w` | 1209800 |

---

## Referencias

- [DNS practica](DNS%20practica.md)
- [3. Protos - DNS](3.%20Protos%20-%20DNS.md)
- [DNS](DNS.md)

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [3. Protos - DNS](3.%20Protos%20-%20DNS.md) — teoría de DNS
- [DNS practica](DNS%20practica.md) — práctica de resolución

**Otras materias**

- **SO**  [File System](File%20System.md) — los archivos de zona viven en el FS del servidor

<!-- notas-relacionadas:fin -->
