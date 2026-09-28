---
temas:
  - Laboratorio
  - HTTP
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - HTTP
Created: 2026-03-1716:19
Materia: "[[protos.base|protos]]"
---
# Laboratorio

## Resumen
``` bash
sudo -i #entra en modo admin 

nginx -t #sirve para saber si todo funciono y nos da informacion, verifica la sintaxis

systemctl reload daemon-reload
systemctl reload nginx

```

### [Proxy reverso](2.%20Protos%20-%20HTTP.md#Proxy%20reverso)
![](Attachments/Pasted%20image%2020260317184603.png)


## Notas
key dirs
``` bash

/etc/nginx/sites-available
/etc/nginx/sites-enable
/var/www/
/tmp/protos

```
## Preguntas

-

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [2. Protos - HTTP](2.%20Protos%20-%20HTTP.md) — teoría de esta práctica
- [Direccionamiento y HTTP - Practica](Direccionamiento%20y%20HTTP%20-%20Practica.md) — práctica extendida de HTTP

<!-- notas-relacionadas:fin -->
