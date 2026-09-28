---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - DHCP
  - practica
Created: 2026-04-2816:15
Materia: "[[protos.base|protos]]"
temas:
  - DHCP
  - Practica
---
# DHCP Practica 

Completo en [Red Practica](Red%20Practica.md)
## DORA

DISCOVER, OFFER, RESPONSE, ACK

![](Attachments/Pasted%20image%2020260421082115.png)

## Armando servidor DHCP

en el R (router)
primero necesito apagar el NetworkManager.service
```shell
systemctl stop NetworkManager.service

sudo apt install isc-dhcp-server

```

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [6. Protos - Red](6.%20Protos%20-%20Red.md) — teoría de DHCP
- [Analisis Wireshark](Analisis%20Wireshark.md) — captura de un intercambio DHCP
- [Red Practica](Red%20Practica.md) — práctica de red

<!-- notas-relacionadas:fin -->
