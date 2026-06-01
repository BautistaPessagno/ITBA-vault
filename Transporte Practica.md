---
temas:
  - Capa de Transporte
  - Servicios de transporte
  - nmap
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - transporte
Created: 2026-04-1416:18
Materia: "[[protos.base|protos]]"
---


# Modos de red de una VM para conectarse a Internet

El hipervisor (VirtualBox, VMware, QEMU/KVM, etc.) expone a la VM una NIC virtual que se puede conectar al mundo exterior de distintas maneras. Los tres modos más comunes son **NAT interna**, **Host-only** y **Puente (Bridge)**. Se diferencian en qué capa de red ocupa la VM y en quién la ve desde afuera.

Referencia: Kurose & Ross, *Computer Networking: A Top-Down Approach*, cap. 4 (NAT, §4.3.4) y cap. 6 (switching/bridging en la capa de enlace).

### NAT interna

El hipervisor implementa un router virtual con NAT/NAPT (igual al router hogareño). La VM recibe una IP **privada** por DHCP del hipervisor (ej: `10.0.2.0/24` en VirtualBox) y todo el tráfico saliente se traduce al IP del host.

- La VM **sí tiene Internet** (outbound).
- Desde afuera (LAN, host, otras VMs) **no se la ve** salvo que se configure *port forwarding* (DNAT).
- Cada VM queda aislada en su propia subred virtual.
- Análogo exacto al NAPT que ya vimos: reescribe `(IP_src, puerto_src)` y mantiene una tabla de traducción.

>[!tip]
> Es el modo por default. Sirve cuando la VM sólo necesita salir (updates, navegación, clonar repos) y no se quiere exponerla.

Ver [[5. Protos - Transporte#NAPT (aka NAT)|NAPT]] y [[5. Protos - Transporte#DNAT (port forwarding)|DNAT]].

### Host-only

El hipervisor crea una red virtual **aislada** entre el host y las VMs. No hay NAT ni salida a Internet: es un switch virtual privado sin uplink.

- La VM **no tiene Internet**.
- Host ↔ VM se ven entre sí por una NIC virtual dedicada del host (ej: `vboxnet0`).
- VMs en el mismo host-only también se ven entre sí.
- Desde la LAN física la VM no existe.

>[!important]
> Sirve para laboratorios aislados: probar servidores, firewalls o capturas sin riesgo de "escape" al exterior.

### Puente (Bridge)

La NIC virtual de la VM se **puentea** a la NIC física del host. El hipervisor funciona como un switch de capa 2: las tramas Ethernet de la VM salen directo al segmento físico con su propia MAC.

- La VM aparece como **una máquina más de la LAN**: pide IP al DHCP de la red real, tiene su propia MAC visible en los switches.
- Tiene Internet igual que cualquier host físico.
- Es **accesible desde toda la LAN** (y desde Internet si la red lo permite).
- No hay traducción de direcciones: la VM comparte el dominio de broadcast con el host.

>[!important]
> Es lo más cercano a "enchufar otra máquina al switch". Útil cuando la VM debe ofrecer servicios a otros equipos de la red.

### Comparación rápida

| Aspecto | NAT interna | Host-only | Puente |
|---|---|---|---|
| Internet desde la VM | Sí (vía NAPT del hipervisor) | No | Sí (directo) |
| VM accesible desde el host | Sólo con port forwarding | Sí | Sí |
| VM accesible desde la LAN | No | No | Sí |
| IP de la VM | Privada, subred virtual | Privada, subred host-only | La que da el DHCP de la LAN |
| MAC visible en la LAN física | No (la del host) | No | Sí (propia de la VM) |
| Capa donde actúa el hipervisor | L3 (router + NAT) | L2 (switch aislado) | L2 (bridge al físico) |

>[!note]
> Regla mental: **NAT** = router con NAT, **Host-only** = switch sin uplink, **Puente** = switch con uplink al cable físico.



# Transporte Practica
## xinetd
```bash
systemctl restart xinetd.service
systemctl status xinetd.service
netstat -nltp #me fijo que sea listening con el -l. netstat sirve para ver si el socket esta abierto o no
```

esta el servicio discard el cual sirve para debuggear

el echo repyite todo lo que recibe

corriendo:
```bash
cat /etc/services | grep <service>
```
podemos ver donde esta el servicio para correrlo con `ncat`

## telnetd
no se usa mas porque no esta encriptado



# tamaño de ventana
![[Pasted image 20260414182249.png]]
se tienen que pasar el tamaño de ventana para saber de cuanto va a ser el cuello de botella


## velocidad de descarga
la velocidad maxima posible es el menor valor de la cadena ya que es el cuello de botella donde se llena e, buffer t


en xinetd hacemos un servicio telnet
```
service {
        disable = 0
        id = telnet
        socket_type = stream
        protocol = tcp
        wait = no
        user = root
        server = /usr/sbin/telnetd
}

```

al mandar cosas por TCP tenes que mandar el tamaño de la ventana



## nmap
```shell
sudo apt install nmap
man nmap

nmap <ip>
#ejemplo
nmap 192.168.0.147  #a mi ip
nmap 192.168.0.0/24 #a la red


nmap -sS --top-ports=20 #escanea 20 puertos

sudo nmap -sS --top-ports=20 -O 192.168.0.0/24

sudo nmap -sU -A 192.168.0.0/24 #habilita deteccino de servicio y script scanning

```


