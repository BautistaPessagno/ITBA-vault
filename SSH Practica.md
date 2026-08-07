---
temas:
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-05-1216:14
Materia: "[[protos.base|protos]]"
---
# SSH Practica


Al conectarse por SSH al querer conectarme la pido la clave publica
al principio tenes que confiar que la clave que recibis es la del deseado

>[!important] importante
>Siempre se pide y se comparte la publica


```bash
ssh
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address]
           [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
           [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file]
           [-J destination] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-P tag] [-p port] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           destination [command [argument ...]]
       ssh [-Q query_option]
```

`ssh-keygen` es para la generacion de claves ssh

```bash
ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/bauti/.ssh/id_ed25519): 
Enter passphrase for "/home/bauti/.ssh/id_ed25519" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/bauti/.ssh/id_ed25519
Your public key has been saved in /home/bauti/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:EqHsuK5ZWR4dz6aBVOQak3vuvRYZlm+hqwjuDIIir/8 bauti@bauti-vm
The key's randomart image is:
+--[ED25519 256]--+
|     .+          |
|   . = .         |
|    B +  .       |
|   + B =+ .      |
|  . B =.S= .     |
|.  = + =+ o      |
|* = . o  +       |
|+O . o .o        |
|+=B.E ooo.       |
+----[SHA256]-----+

```
la imagen del final permite visualizar la entriopia

```bash
bauti@bauti-vm:~$ cd .ssh
bauti@bauti-vm:~/.ssh$ ls
authorized_keys  id_ed25519  id_ed25519.pub
```

```bash
cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
***REDACTED***
-----END OPENSSH PRIVATE KEY-----
```

```bash
sudo apt install openssh-server

cd /etc/ssh/
ls
moduli      ssh_config.d        ssh_host_ecdsa_key.pub  ssh_host_ed25519_key.pub  ssh_host_rsa_key.pub  sshd_config
ssh_config  ssh_host_ecdsa_key  ssh_host_ed25519_key    ssh_host_rsa_key          ssh_import_id         sshd_config.d

```

```bash
ls -l
total 628
-rw-r--r-- 1 root root 592383 Apr 27 21:24 moduli
-rw-r--r-- 1 root root   1668 Sep 29  2025 ssh_config
drwxr-xr-x 2 root root   4096 Apr 16 10:53 ssh_config.d
-rw------- 1 root root    505 Mar 10 18:52 ssh_host_ecdsa_key
-rw-r--r-- 1 root root    175 Mar 10 18:52 ssh_host_ecdsa_key.pub
-rw------- 1 root root    399 Mar 10 18:52 ssh_host_ed25519_key
-rw-r--r-- 1 root root     95 Mar 10 18:52 ssh_host_ed25519_key.pub
-rw------- 1 root root   2602 Mar 10 18:52 ssh_host_rsa_key
-rw-r--r-- 1 root root    567 Mar 10 18:52 ssh_host_rsa_key.pub
-rw-r--r-- 1 root root    342 Dec  7  2020 ssh_import_id
-rw-r--r-- 1 root root   4307 Apr 27 21:24 sshd_config
drwxr-xr-x 2 root root   4096 Apr 27 21:24 sshd_config.d

```
se puede ver en los permisos que solo los admin puede ver todos los archivos, el resto no puede ver las key privadas

```bash
systemctl start ssh
systemctl status ssh
```

me conecto a ssh
```bash
ssh bauti@localhost
ssh bauti@localhost -v #para ver el debug
```

```bash
cd .ssh
cat known_hosts
```


le damos el ssh_key al servidor para authenticarse con la pub key

```bash
ssh-copy-id localhost #copiamos el ssh key en el servidor en localhost
```

al hacer `ssh -v localhost` nuevamente

esta vez optenemos:
`Authenticated to localhost ([127.0.0.1]:22) using "publickey".`

## SCP
vamos a copiar desde la VM a la macbook
``
en mi mac
```bash
ssh bauti@192.168.0.147

#para poder conectarme con ssh_key
ssh-copy-id bauti@192.168.0.147
ssh bauti@192.168.0.147
```


en un servidor intermedio deberia poder comunicar con el cliente y el servidor ssh para el manejo de keys

>[!note] ssh-agent
>ssh-agent es un proceso en segundo plano que gestiona tus claves privadas de SSH. Su función es mantener las claves desencriptadas en la memoria, permitiéndote autenticarte en servidores sin tener que introducir tu contraseña en cada conexión.


>[!tip] ssh-agent con Balanceador de Carga 
>1.  Origen (Tu PC): Ejecutas ssh usuario@balanceador. El ssh-agent entrega automáticamente la llave privada al cliente SSH para que la conexión comience.
>2.  Tránsito (El Balanceador): El tráfico llega al Balanceador de Carga. El balanceador (en Capa 4) ve una petición en el puerto 22 y dice: "Elegiré al Servidor B para esta conexión". Redirige el tráfico hacia allí.
>3.  Destino (Servidor B): El Servidor B recibe la conexión. Recibe la llave que el ssh-agent envió originalmente desde tu PC. El servidor valida la llave y te da acceso.
En resumen: El ssh-agent se encarga de "quién eres" (identidad) en el origen, y el balanceador se encarga de "a dónde vas" (destino) en la red.

```bash

ssh-agent
#SSH_AUTH_SOCK=/tmp/ssh-4POYFXj0Mc0k/agent.7551; export SSH_AUTH_SOCK;
#SSH_AGENT_PID=7552; export SSH_AGENT_PID;
#echo Agent pid 7552;

eval $(ssh-agent)
#Agent pid 7555

ssh-add
#Identity added: /home/bauti/.ssh/id_ed25519 (bauti@bauti-vm)

ssh-add -l
#256 SHA256:EqHsuK5ZWR4dz6aBVOQak3vuvRYZlm+hqwjuDIIir/8 bauti@bauti-vm (ED25519)

```

# Tunel
cliente-servidor

podemos tener el puerto 5432
el tunel es un encapsulamiento del contenido
redirige a los puertos del cliente

ida y vuelta de los caminos


```bash
# en terminal 1 (computadora)
nc -l 45101

# en terminal 2 
ssh -R 9999:localhost:45101 bpessagno@pampero.itba.edu.ar #conecto a pampero

nc localhost 9999 # conecta el localhost 45101 de mi computadora con el 9999 de pampero
# puertoServidor:destino:puertoDestino



```


es un duplex. hace que funcione para los dos lados. lo que pongas en el 45101 va a l 9999 y vice versa

```bash
#terminal 1 (pampero)
nc -kl 1414

#terminal 2
 ssh -L 1234:localhost:1414  bpessagno@pampero.itba.edu.ar
#se puede pner cualquier numero al ser local
#puertoLocal:origen:puertoServidor
 
 
 #terminal 3
 ssh bpessagno@pampero.itba.edu.ar
 nc localhost 1234
```



```bash
#terminal 1
ssh -L 8080:google.com:80 bpessagno@pampero.itba.edu.ar
#puertoLocal:destino:puerto
#al hacer esto al acceder a pampero se conecta a google en el puerto 80 (HTTP)

#terminal 2
curl localhost:8080
<!DOCTYPE html>
<html lang=en>
  <meta charset=utf-8>
  <meta name=viewport content="initial-scale=1, minimum-scale=1, width=device-width">
  <title>Error 404 (Not Found)!!1</title>
  <style>
    *{margin:0;padding:0}html,code{font:15px/22px arial,sans-serif}html{background:#fff;color:#222;padding:15px}body{margin:7% auto 0;max-width:390px;min-height:180px;padding:30px 0 15px}* > body{background:url(//www.google.com/images/errors/robot.png) 100% 5px no-repeat;padding-right:205px}p{margin:11px 0 22px;overflow:hidden}ins{color:#777;text-decoration:none}a img{border:0}@media screen and (max-width:772px){body{background:none;margin-top:0;max-width:none;padding-right:0}}#logo{background:url(//www.google.com/images/branding/googlelogo/1x/googlelogo_color_150x54dp.png) no-repeat;margin-left:-5px}@media only screen and (min-resolution:192dpi){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat 0% 0%/100% 100%;-moz-border-image:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) 0}}@media only screen and (-webkit-min-device-pixel-ratio:2){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat;-webkit-background-size:100% 100%}}#logo{display:inline-block;height:54px;width:150px}
  </style>
  <a href=//www.google.com/><span id=logo aria-label=Google></span></a>
  <p><b>404.</b> <ins>That’s an error.</ins>
  <p>The requested URL <code>/</code> was not found on this server.  <ins>That’s all we know.</ins>
```

```bash
ssh -D 8080 bpessagno@pampero.itba.edu.ar  #Dinamyc Port Forwarding

#terminal 2
netstat -tlnp
#la ultima entrada dice 8080 ssh

curl -x socks://localhost:8080  google.com -v

curl -x socks://localhost:8080  ifconfig.me
```
la resolucion de DNS se hace de manera local (en mi computadora) 
si queremos que la resolucion pase del otro (en pampero/ servidor ssh) lado se tiene que hacer
```bash
curl -x socks5h://localhost:8080 ifconfig.me
```

>[!importante] sobre la resolucion
>La diferencia radica en quién resuelve el DNS (traduce el nombre de dominio a una dirección IP):
>*   socks://: Tu máquina local resuelve la IP de ifconfig.me y luego le envía al proxy la instrucción de conectarse a esa IP.
>*   socks5h://: El cliente le envía el nombre ifconfig.me al proxy, y es el servidor proxy quien se encarga de resolver el DNS.
>La versión con h es más privada (tu ISP no ve las consultas DNS) y te permite acceder a dominios que solo son visibles desde la red del proxy.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [[9. Protos - SSH]] — teoría de esta práctica

**Otras materias**

- **SO**  [[Entorno de desarrollo]] — trabajo en consola remota

<!-- notas-relacionadas:fin -->
