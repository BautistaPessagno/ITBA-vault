---
temas:
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-05-0516:25
Materia: "[[protos.base|protos]]"
---
# Routing Practica

## [ARP](https://datatracker.ietf.org/doc/html/rfc826)

``` bash
# hago 
route -n
# voy a tenr algo como
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.68.1    0.0.0.0         UG    100    0        0 enp0s8
192.168.0.0    0.0.0.0         255.255.252.0   U     100    0        0 enp0s8



```

>[!distincion wireshark]
la diferencia entre abrir `any` y una interface especifica es que en `any` no muestra los paquetes

hago
```bash
ping 192.168.0.101 #ping a una persona X
```

como mi computadora no sabe el mac address hace un ARP
![[Pasted image 20260506201340.png]]
hay un broadcast ARP porque como no sabe la direccion de la MAC se lo manda a todos
si ya lo tiene el MAC address entonces ya no usa ARP

> [!important] como funciona el lenguage
> el -> son comentarios sobre el RFC

```
?Do I have the hardware type in ar$hrd? -> se fija qu sea ethernet
Yes: (almost definitely)
  [optionally check the hardware length ar$hln]
  ?Do I speak the protocol in ar$pro? -> hablo el lenguage? (es decir ARP)
  Yes:
    [optionally check the protocol length ar$pln]
    Merge_flag := false -> se fija la tabla ARP
    If the pair <protocol type, sender protocol address> is
        already in my translation table, update the sender
        hardware address field of the entry with the new
        information in the packet and set Merge_flag to true.
        -> si ya esta la tabla actualizar la MAC
    ?Am I the target protocol address?
    Yes:
      If Merge_flag is false, add the triplet <protocol type,
          sender protocol address, sender hardware address> to
          the translation table.
      ?Is the opcode ares_op$REQUEST?  (NOW look at the opcode!!)
      Yes:
        Swap hardware and protocol fields, putting the local
            hardware and protocol addresses in the sender fields.
        Set the ar$op field to ares_op$REPLY
        Send the packet to the (new) target hardware address on
            the same hardware on which the request was received.
    -> primero actualizo mi tabla y despues me fijo si es un request o no
```

con `arp -n` puedo ver mi tabla de arp
```bash
arp -n
#obtrengo algo como lo siguiente:
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.68.1             ether   7c:f1:7e:9e:a9:0c   C                     enp0s8
192.168.68.65            ether   14:cb:19:45:03:ad   C                     enp0s8

# si alguien me tira un ping a mi address va a aparecer en mi tabla

arp -n

Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.68.65            ether   14:cb:19:45:03:ad   C                     enp0s8
192.168.68.1             ether   7c:f1:7e:9e:a9:0c   C                     enp0s8
192.168.68.64            ether   f4:5c:89:c8:5b:63   C                     enp0s8

```

puedo alterar/mainpular la tabla

```bash
#Le doy a una ip otra mac
arp -s <ip> <mac> #tabla estatica 
#la ip de pirulo la mac de peipto
```

ahora cuando una computadora haga una trama le va a llega a pepito. porque es su MAC address
pepito no lo va a aceptar porque no es para el

pepito tiene que correr
```bash
sudo -i
sysctl net.ipv4.ip_forward = 1
```

>[!tip] OBS
>ultimamente se estan agregando medidas para evitar estos tipos de problemas


borrar ips de la tabla
```bash
arp -d <ip>
```

luego puedo hacer lo mismo pero editando la broadcast
```bash
arp -s <ip> ff:ff:ff:ff:ff:ff
```
ahora al querer ir a la ip de pirulo va a salir para todos

>[!warning]
>el protocolo ARP asume que todos se van a portar bien asumiendo que tenes control de la red

- se arreglo hace poco
- las computadoras empezaron a filtrar
- los routers empezaron a agregar medidas. bloquean los ARPs o pasan ARPs falsos para desconfigurar
## Arping
el arping es un ping pero de capa 2
en lugar de un ICMP hace ARP
```bash
arping <ip>
```

![[Pasted image 20260506205900.png]]
se puede ver todos los llaados `arp` al IP
arping no afecta a mi tabla arp

se le puede pasar 4 argumentos:
-  `-s <mac>` para indicar la MAC origen
- `-S <IP>` indicar la IP origen
- `-t <mac>` para indicar la MAC destino
- `-T <IP>` indicar la IP destino

en arping puedo mentir con estos comandos permitiendo sobreescribir

>[!important]
>IPV6 no necesita arp porque incluye el MAC address

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[7. Protos - Routing]] — teoría de esta práctica
- [[Red Practica]] — tablas de ruteo

**Otras materias**

- **EDA**  [[EDA - Grafos]] — los algoritmos de camino mínimo que implementa el ruteo

<!-- notas-relacionadas:fin -->
