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

Completo en [[Red Practica]]
## DORA

DISCOVER, OFFER, RESPONSE, ACK

![[Pasted image 20260421082115.png]]

## Armando servidor DHCP

en el R (router)
primero necesito apagar el NetworkManager.service
```shell
systemctl stop NetworkManager.service

sudo apt install isc-dhcp-server

```

