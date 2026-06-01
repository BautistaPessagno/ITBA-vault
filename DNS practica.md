---
categories:
  - "[[ITBA.base|ITBA]]"
Tags:
  - DNS
  - practica
Created: 2026-03-3116:29
Materia: "[[protos.base|protos]]"
temas:
  - DNS
  - Practica
---
# DNS practica

## DIG

>[!description]
>**dig** is a flexible tool for interrogating DNS name servers. It performs DNS lookups and displays the answers that are returned from the name server(s) that were queried. Most DNS administrators use dig to troubleshoot DNS problems because of its flexibility, ease of use and clarity of output. Other lookup tools tend to have less functionality than dig.

```
dig google.com
```
![[Pasted image 20260331163024.png]]
esta la question section y el answer. no siempre tiene que haber un answer

![[Pasted image 20260331164256.png]]
el numero de tres digitos es el Time To Live


![[Pasted image 20260331165313.png]]
el numero antes del dominio es la prioridad

![[TLDS.excalidraw]]

el autoritativo nunca habla con el usuario. habla con cache



```
sudo apt install bind

cd /etc/bind
```

```
/etc/bind/named.conf.local


```

``` bash
/etc/bind/foo.pdc.lab.local

ARCHIVO: /etc/bind/foo.pdc.lab.local

$TTL 1m #si no especifico el TTL va a ser de un minuto
$ORIGIN FOO.PDC.LAB. #lo agrega siempre al final
foo.pdc.lab.   IN SOA ns.foo.pdc.lab.  timizrahi.itba.edu.ar(
          1  ; serial
          7d  ; Refresh
          1d  ; Retry
          10d  ; Expire
          1m  ; Negative TTL
)

#foo.pdc.lab.       1m   IN NS ns1.foo.pdc.lab.
#foo.pdc.lab.       2m   IN NS ns2.foo.pdc.lab.

#ns.foo.pdc.lab.    10   IN A 1.2.3.4
#ns1.foo.pdc.lab.   20   IN A 1.2.3.5
#ns2.foo.pdc.lab.   30   IN A 1.2.3.6

@       1m   IN NS ns1
@       2m   IN NS ns2

ns    30   IN A 1.2.3.4
ns1      IN A 1.2.3.5
ns2      IN A 1.2.3.6


@         2h IN MX 1 nsmail
@         2h IN MX 3 ns2mail
@         2h IN MX 3 ns3mail



nsmail        IN A 2.2.2.2
ns2mail       IN A 2.2.2.3
ns3mail       IN A 2.2.2.4


www          IN A 6.7.8.9
@            IN A 6.7.8.10
w3           CNAME www

```
el Negative TTL cachea un resultado negativo

- Serial: numero de version para saber si necesitas actualizarte o ya estas actualizado
- Refresh. cuanto tiempo te podes quedar con el dato
- Retry: cada cuanto tiempo puedo volver a intentar
- Expire: despues de X dias tenes que borrar los datos ( $Expire > Refresh + Retry$)
- Negative TTL: si te dio como que no existe, te cacheas que no existe 




# Guia Practica
## E42 
![[Pasted image 20260406105906.png]]

![[Pasted image 20260406105938.png|360]]![[Pasted image 20260406110010.png|299]]

![[Pasted image 20260406110129.png|351]] ![[Screenshot 2026-04-06 at 11.01.39.png|332]]

![[Pasted image 20260406110238.png]] ![[Pasted image 20260406110248.png]]

## E43
![[Pasted image 20260406110332.png]]
idem al E42 pero tengo que correr ```
```bash
dig <domain> mx
```
y con eso consigo el dominio
