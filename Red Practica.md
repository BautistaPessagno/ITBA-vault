---
temas:
  - tablas ruteo
  - ping
  - ARP (request, reply, proxy, spoofing)
  - NAT / NAPT
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-04-2710:46
Materia: "[[protos.base|protos]]"
---
# Red Practica

hacer `ping` es un paquete ICMP

![[Pasted image 20260427105606.png]]

esta el TTL el cual es el time to live y despues se tira

poner un TTL absurdamente bajo va a hacer que siempre se dorpee el paquete

se puede correr `traceroute -n google.com --icmp` 

al conectar dos virtual machines. pasa que en el host no hay wifi y en el `r` si

Apago el NetworkManager.service

```shell
sudo -i
ifconfig #veo mis interfazes
```

les asignamos una IP
## Armar IP manual
ver cuales tienen libre con ifconfig

```shell
ifconfig 
sudo ifconfig enp0s9 192.168.101.1/24 up #en R

sudo ifconfig enp0s8 192.168.101.2/24 up #en H

```

luego en R:

en `/etc/dhcp/dhcpd.config`
```shell
#el IP y la mascara, en este caso 24(255.255.255.0 ya que son 24 1s, 2^8 = 256) 
subnet 192.168.101.0 netmask 255.255.255.0{ 
        range 192.168.101.100 192.168.101.200; #rango de direcciones
        option domain-name-servers 1.1.1.1; #servidor (ej google 8.8.8.8)
        option subnet-mask 255.255.255.0;
        option routers 192.168.101.1;
        option broadcast-address 192.168.101.255;
        default-lease-time 20;
        max-lease-time 600;
}

```

y en /etc/default/isc-dhcp-server
```shell
INTERFACESv4="enp0s9" #basado en la config de IP
```


despues corro
```shell
# lo instalo si no lo tengo
sudo apt install isc-dhcp-server

systemctl restart isc-dhcp-server
systemctl status isc-dhcp-server
```

## Conectarse a otros dispositivos a R (como impresoras)
configurar un host
se puede hacer a nivel innet o global

 `/etc/dhcp/dhcpd.conf`
```shell
subnet 192.168.101.0 netmask 255.255.255.0{
        range 192.168.101.100 192.168.101.200;
        option domain-name-servers 1.1.1.1;
        option subnet-mask 255.255.255.0;
        option routers 192.168.101.1;
        option broadcast-address 192.168.101.255;
        default-lease-time 20;
        max-lease-time 600;

	host xXx_Impresora69420_xXx {
		hardware ethernet aa:bb:cc:dd:ee:ff;
		fixed-address 192.168.101.40;
		option host-name "pablito";
	}
}

```

### dir MAC address
al correr `ifconfig`
`ether 08:00:27:27:b3:88`

**no va a haber wifi porque sigue faltando el ip forwarding**
lo prendemos

```shell
sudo sysctl net.ipv4.ip_forward=1
```

usamos [[6. Protos Red -resumen claude#10. NAT - SNAT (Source NAT) |NAT]] en R
```shell
sudo -i
iptables -t nat -L

# se puede usar
iptables -t nat -A POSTROUTING -o enp0S8 -j SNAT --to-source 192.168.0.155
# pero es mejor el siguiente porque el IP puede cambiar
sudo iptables -t nat -A POSTROUTING -o enp0s8 -j MASQUERADE # a donde hay internet

# borrar reglas
iptables -t nat -D POSTROUTING 1
```

# Problema de Internet en host H a través de R con NAT

## Contexto

- **R**: router (Ubuntu VM), dos interfaces: `enp0s8` (NAT/internet) y `enp0s9` (red interna 192.168.101.0/24)
- **H**: host (Ubuntu VM), una interfaz: `enp0s8` (red interna, IP asignada por DHCP desde R)
- Objetivo: que H tenga acceso a internet pasando por R

---

## Problema 1 — MASQUERADE en la interfaz equivocada

**Síntoma:** `ping 8.8.8.8` no funciona desde H.

**Causa:** La regla iptables usaba `-o enp0s9` (interfaz interna) en lugar de `-o enp0s8` (interfaz hacia internet). Además, la regla no estaba efectivamente aplicada (POSTROUTING vacía).

**Verificación:**
```bash
sudo iptables -t nat -L POSTROUTING -v --line-numbers
```

**Solución en R:**
```bash
sudo iptables -t nat -F POSTROUTING
sudo iptables -t nat -A POSTROUTING -o enp0s8 -j MASQUERADE
```

> La interfaz del MASQUERADE debe ser siempre la que da hacia internet (la "pública"), no la interna.

---

## Problema 2 — H sin default gateway

**Síntoma:** `ping 8.8.8.8` no funciona desde H.

**Causa:** H no tenía ruta `default`. `ip route show` mostraba solo la ruta de red local, sin línea `default via 192.168.101.1`.

**Verificación:**
```bash
ip route show
# Debe aparecer: default via 192.168.101.1 dev enp0s8
```

**Solución temporal en H:**
```bash
sudo ip route add default via 192.168.101.1
```

**Solución permanente en R (`dhcpd.conf`):**
```
option routers 192.168.101.1;
```
Luego renovar en H:
```bash
sudo dhclient -r && sudo dhclient enp0s8
```

---

## Problema 3 — H sin DNS

**Síntoma:** `ping 8.8.8.8` funcionaba pero `ping google.com` y `curl google.com` fallaban.

**Causa:** `systemd-resolved` no tenía DNS upstream configurado para la interfaz. `resolvectl status` mostraba `Current Scopes: none` y `Default Route: no`.

**Verificación:**
```bash
resolvectl status
# Buscar en la interfaz: DNS Servers, Default Route
```

**Solución temporal en H:**
```bash
sudo resolvectl dns enp0s8 8.8.8.8
sudo resolvectl default-route enp0s8 yes
```

**Solución permanente en R (`dhcpd.conf`):**
```
option domain-name-servers 8.8.8.8;
```
Luego renovar en H:
```bash
sudo dhclient -r && sudo dhclient enp0s8
```

---

## Cadena completa que debe funcionar

```
H ──(default via 192.168.101.1)──► R ──(MASQUERADE en enp0s8)──► Internet
         enp0s8: 192.168.101.2        enp0s8: 192.168.68.x
                                      enp0s9: 192.168.101.1
```

## Checklist final

- [ ] `ip_forward=1` en R: `sudo sysctl net.ipv4.ip_forward=1`
- [ ] MASQUERADE sobre interfaz correcta (la de internet): `iptables -t nat -A POSTROUTING -o enp0s8 -j MASQUERADE`
- [ ] `option routers` en `dhcpd.conf` → H recibe default gateway
- [ ] `option domain-name-servers` en `dhcpd.conf` → H recibe DNS

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[6. Protos - Red]] — teoría de esta práctica
- [[IP practica]] — direccionamiento IP
- [[DHCP Practica]] — DHCP
- [[Routing Practica]] — tablas de ruteo
- [[Analisis Wireshark]] — capturas de ARP e ICMP

<!-- notas-relacionadas:fin -->
