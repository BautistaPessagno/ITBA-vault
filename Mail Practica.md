---
temas:
  - SMTP
  - POP3 / IMAP
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-04-0716:11
Materia: "[[protos.base|protos]]"
---
# **Correo Electrónico**

no funciona con posts porque no podes enviar cosas si el otro tiene la computadora apagada. no tiene conectividad

hoy al final del dia lo que mas se usa el HTTP

> Cuando usás Gmail en el browser, en realidad estás usando HTTP para todo — el browser habla HTTP con los servidores de Google, que internamente gestionan los mails. IMAP/SMTP quedan "atrás" de la interfaz web.

## RFC

si piden un ejercicio de armau un mail hacer lo siguiente

- mandarse el mail a uno mismo
- mostrar original
- descargar original


## Tipos de mail

en el .eml

### multipart/alternative
da dos variantes de un mensaje equivalente

### multipart/mixed

da dos mensajes de cosas distintas (ej: imagen y texto)

### multipart/related
independiente entre si pero se relacionan

>[!example]
>El caso más típico es un **email HTML que tiene imágenes embebidas** (no adjuntas, sino incrustadas dentro del HTML).


# SMPT

el cliente no es el primero en hablar es el servidor

```bash
dig mx <dir>
# consigo el mail
ncat <ubi> 25 -v -C
```


## Configurando un servidor SMPT

```shell
sudo apt install postfix 
```

creo un postfix, en mi caso `bauti.protos`

si quiero hacer un camibo tengo que ir a `/etc/systemd`
con 
``` shell
systemctl status postfix #vemos si esta prendido

netstat -tulpn #al ser servidor SMPT vemos el puerto 25

nc -C localhost 25 #hace falta el -C
```

mandar mail al servidor:

![ejemplo clase](4.%20Protos%20-%20MAIL.md#Como%20enviar%20mail%20de%20un%20host%20a%20otro%3F#SMPT#Formato%20mail%20SMPT)

Error Tipico de parcial es ver el queued y pensar que se mando

hago el `QUIT`

y hago 
```shell
ls /var/spool/mail/
```

para ya tenerlo mas simple vamos a crear un `email.txt`

y hago 
``` shell
cat email.txt | nc -C localhost 25
cat email.txt | nc -C localhost smpt #si pongo el protocolo nc resuelve sol
```

si dos personas le escriben a la misma persona puede haber una corrupcion de datos

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [4. Protos - MAIL](4.%20Protos%20-%20MAIL.md) — teoría de esta práctica
- [Direccionamiento y HTTP - Practica](Direccionamiento%20y%20HTTP%20-%20Practica.md) — también se usa nc para hablar el protocolo a mano

<!-- notas-relacionadas:fin -->
