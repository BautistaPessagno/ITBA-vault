---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: 2026-05-2019:07
Materia: "[[protos.base|protos]]"
temas:
---

# 1. HTTP

---

- Crear un servidor nginx    
![](Attachments/Pasted%20image%2020260520184428.png)
Los archivos de configuracion de nginx estan en /etc/nginx. Vamos a esa carpeta:
```bash
cd /etc/nginx
```
Si dentro de esta carpeta hacemos un ls, vemos lo siguiente
![](Attachments/Pasted%20image%2020260520184528.png)
- En el directorio sites-available, vamos a agregar nuestros sitios
- En el directorio sites-enabelde, vamos a tener un link simbolico a los sitios que queremos mostrar
Para poder configurar correctamente los sitios vamos a tener que seguir los siguientes pasos:    
1. Crear los archivos index..html que queremos que muestren los sitios
2. Crear los archivos en sites-available
3. Hacer los links a sites-enabeled
4. Hacer que al acceder a los links que queremos, nuestra pc resuelva los nombres a localhost.    
### Crear los archivos index.html

Creamos estos archivos en /var/www. Para cada una de las paginas que queremos mostrar, creamos un nuevo directorio. En este caso, uno para foo y otro para var.    
```bash
cd /var/www        
mkdir foo
tee index.html
```

Y agregamos el contenido. Lo mismo para bar
### Crear los archivos en sites-available
```bash
cd /etc/nginx/sites-available/
```
![](Attachments/Pasted%20image%2020260520184710.png)

Si queremos crear un nuevo sitio, creamos una copia de un archivo preexistente para no arrancar de 0

```bash
cp default nuevoArchivo
```

Lo modificamos para que quede asi, idem con bar
    
```nginx
server {
	listen 80 ;
    listen [::]:80 ;
    
    root /var/www/bar;
    
    access_log /var/log/nginx/bar_access.log;            
    error_log  /var/log/nginx/bar_error.log;            
    
    index index.html index.htm index.nginx-debian.html;
    
    server_name bar;
      
    location / {
        try_files $uri $uri/ =404;
    }
}
```
    
### Crear los archivos en sites-enabled
vamos a /etc/nginx/sites-enabled/ y creamos los links simbolicos
    
```bash
ln -s ../sites-available/foo .
ln -s ../sites-available/bar .
```
### Acceder a local host al ir a foo y bar

Modificar el archivo /etc/hosts y agregar que foo y bar se resuelven a 127.0.0.1
![](Attachments/Pasted%20image%2020260520184847.png)

### Reseterar el servidor para que se vean los cambios

service nginx restart

### Chequear resultados

![](Attachments/Pasted%20image%2020260520184915.png)
  ![](Attachments/Pasted%20image%2020260520184948.png)
  

Con curl

![](Attachments/Pasted%20image%2020260520185002.png)

- Crear un proxy

![](Attachments/Pasted%20image%2020260520185056.png)

 rta
	[https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
    [https://www.youtube.com/watch?v=1fBNOXcYHGQ](https://www.youtube.com/watch?v=1fBNOXcYHGQ)

### Apache

En el archivo

```bash
/etc/apache2/sites-available/000-default.conf
```

Camibar a <VirtualHost *:8080>

y en /etc/apache2/ports.conf

![](Attachments/Pasted%20image%2020260520185407.png)

reiniciar apache

### nginx

En el archivo

/etc/nginx/sites-available/apache-proxy (le cambie el nombre por los problemas en /foo

```bash
server {
    listen 80 ;
    listen [::]:80 ;

     root /var/www/foo;
        
    access_log /var/log/nginx/apache-proxy_access.log;
    error_log  /var/log/nginx/apache-proxy_error.log;
        
    index index.html index.htm index.nginx-debian.html;
        
    server_name apache-proxy;
        
    location / {
        try_files $uri $uri/ =404;
        proxy_pass http://localhost:8080;
        }
}
```

![](Attachments/Pasted%20image%2020260520185502.png)

- rta
	No podemos directamente modificar el archivo /etc/hosts y hacer que foo.pdc.lab apunte a la ip de [foo.leak.com.ar](http://foo.leak.com.ar) porque eso no va a modificar el header host que envia el browser.
    
    Debemos utilizar un proxy reverso.
    
	1. Creamos una pagina en nginx con el nombre foo.pdc.lab
    
	Creamos la pag en sites-available y creamos el link simbolico en sites-enabled, configurando correctamente los logs y un proxy_pass a http:/foo.leak.com.ar/
      ![](Attachments/Pasted%20image%2020260520185558.png)
    2. En etc/hosts hacemos que foo.pdc.lab apuntea localhost
    3. Verificamos que anda
     ![](Attachments/Pasted%20image%2020260520185620.png)
- Hace un get con headers específicos
	- Conectarse por netcat
	```bash
    nc -C google.com 80
    ```
	Escribir el request
    
    ```bash
    GET / HTTP/1.1
    Host: localhost:9090
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:103.0) Gecko/20100101 Firefox/103.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate, br
    Connection: keep-alive
    Upgrade-Insecure-Requests: 1
    Sec-Fetch-Dest: document
    Sec-Fetch-Mode: navigate
    Sec-Fetch-Site: none
    Sec-Fetch-User: ?1
    ```


# 2. DNS

---

- Crear un servidor dns
    ![](Attachments/Pasted%20image%2020260520185817.png)
    
    - rta
        
        Los logs de bind estan en /var/log/syslog
        
        Las conf de bind esta en /etc/bind
        
        Voy a cambiarle el nombre a practica.dns.bind
        
        1. Agregar la zona en el archivo /etc/bind/named.conf
            ![](Attachments/Pasted%20image%2020260520185841.png)
            
        2. Creamos el archivo bind.local en /etc/bind
	        ![](Attachments/Pasted%20image%2020260520185853.png)
            
        3. Verificamos que este funcionando correctamente.
            ![](Attachments/Pasted%20image%2020260520185901.png)
            
            Haciendo @127.0.0.1 hacemos que consulte a nuestro servidor dns local.

# 3. Mail

---

- Decodificar algo en base64
    
    ```bash
    echo '<el string base64>' | base64 -d > img.jfif
    ```
    
- Encontrar direcciones ip de un servidor de mail
    
    Cuales son todas las direcciones IP de los servidores SMTP que reciben el correo entrante en el dominio [proto.leak.com.ar](http://proto.leak.com.ar/)?
    
    ```bash
    dig -t MX +short proto.leak.com.ar | sort -n # ordenamos por prioridad
    ```
    
- Direcciones IP permitidas para enviar mails
    
    ### Según la configuración SPF de [itba.edu.ar](http://itba.edu.ar/): ¿desde qué direcciones IP se pueden enviar correos a servidores que implementen SPF?
    
    SPF es usado por servidores que reciben mail para verificar si una IP está autorizada para mandar mails proveniendo de cierto dominio. Se implementa con un registro TXT DNS en el dominio.
    
    Este registro TXT indica que hosts pueden mandar mails provenientes del dominio.
    
    Averiguémoslo con un query DNS:
    
    ```bash
    dig -t TXT +short itba.edu.ar.
    "v=spf1 include:_spf.google.com ip4:190.104.250.100 ip4:190.104.250.101 ip4:52.67.50.43 ip4:54.94.246.207 ip4:52.67.178.216 ip4:52.67.194.156 ip4:200.32.57.124 include:spf.perfit-mail.net include:spf.mandrillapp.com include:_spf.embluemail.com include:mh.b" "lackboard.com ~all"
    "pardot984701=93243cb68555ff6e03ec884e22e626e012889126e159b187b670320ea9647af5"
    ```
    
    A nosotros nos importa la primera línea, que tiene información de SPF.
    
    ```
    v=spf1
    	include:_spf.google.com
    	ip4:190.104.250.100
    	ip4:190.104.250.101
    	ip4:52.67.50.43
    	ip4:54.94.246.207
    	ip4:52.67.178.216
    	ip4:52.67.194.156
    	ip4:200.32.57.124
    	include:spf.perfit-mail.net
    	include:spf.mandrillapp.com
    	include:_spf.embluemail.com
    	include:mh.b" "lackboard.com ~all"
    ```
    
    Vemos una lista de IPs que tienen permitido emitir emails. Los include indican que se debe buscar los registros TXT de tal host y tomar las reglas especificadas ahí. Por ejemplo, el primer include dice `_spf.google.com` entonces hacemos un query para los registros TXT:
    
    ```
    dig +short -t TXT _spf.google.com.
    "v=spf1 include:_netblocks.google.com include:_netblocks2.google.com include:_netblocks3.google.com ~all"
    ```
    
    Y haciendo más queries a estas opciones encontramos más IPs permitidas:
    
    ```
    dig +short -t TXT _netblocks.google.com.
    "v=spf1 ip4:35.190.247.0/24 ip4:64.233.160.0/19 ip4:66.102.0.0/20 ip4:66.249.80.0/20 ip4:72.14.192.0/18 ip4:74.125.0.0/16 ip4:108.177.8.0/21 ip4:173.194.0.0/16 ip4:209.85.128.0/17 ip4:216.58.192.0/19 ip4:216.239.32.0/19 ~all"
    ```
    
    ### Fuentes:
    
    - [https://docs.oracle.com/en-us/iaas/Content/Email/Tasks/configurespf.htm](https://docs.oracle.com/en-us/iaas/Content/Email/Tasks/configurespf.htm)
    - [http://www.open-spf.org/SPF_Record_Syntax/](http://www.open-spf.org/SPF_Record_Syntax/)
- Crear un servidor de mail
    
    [Creando el server SMTP con postfix y dovecot](https://www.notion.so/Creando-el-server-SMTP-con-postfix-y-dovecot-37183a12a08847c3b80680f9210e7c0a?pvs=21)
    
- Eniviar un correo a alguien del itba desde la terminal
    
    Primero debemos averiguar cual es el servidor de mails
    
    ```bash
    dig +short -t MX itba.edu.ar. | sort -n
    ```
    
    Nos conectamos con el servidor de mail
    
    ```bash
    nc -v -C aspmx.l.google.com. 25
    ```
    
    Luego, tenemos que identificarnos con el comando EHLO, en este caso el servidor no va a verificar quien se esta identificando, podemos poner cualqueir cosa
    
    ```bash
    EHLO hola.com.ar
    ```
    
    Indicamos el remitente, quien lo recibe y luego q comienzan los datos
    
    ```bash
    MAIL FROM: <lala@leak.com.ar>
    RCPT TO: <destinatario@itba.edu.ar>
    DATA
    ```
    
    En la parte de data hay algunos headers:
    
    ```bash
    From: "Remitente Ejemplo" (personal) <cami@leak.com.ar>
    To: "Destinatario Ejemplo" <destinatario@itba.edu.ar>
    Subject: prueba pdc 
    MIME-Version: 1.0
    Content-type: text/plain; charset=UTF-8
    Content-transfer-Encoding: quoted-printable
    ```
    
    Dejamos una linea en blanco para indicar que comienza el cuerpo


# 4. Transporte

---

- Escanear puertos buscando servicios activos
    
    Si queremos ver que puertos tiene activos pampero
    ```bash
    nmap pampero.itba.edu.ar
    
    Starting Nmap 7.80 ( <https://nmap.org> ) at 2022-09-18 11:33 -03
    Nmap scan report for pampero.itba.edu.ar (200.5.121.137)
    Host is up (0.0068s latency).
    rDNS record for 200.5.121.137: 137.advance.com.ar
    Not shown: 996 closed ports
    PORT     STATE    SERVICE
    22/tcp   open     ssh
    113/tcp  filtered ident
    2000/tcp open     cisco-sccp
    5060/tcp open     sip
    
    Nmap done: 1 IP address (1 host up) scanned in 1.72 seconds
    ```
    
    - opciones para scanning con namp
        
        - `sS` manda un TCP SYN
        
	    ![](Attachments/Pasted%20image%2020260520190048.png)
        
        - `sT` intenta un Connect() TCP# Untitled 4

## Resumen



## Notas



## Preguntas

-


        ![](Attachments/Pasted%20image%2020260520190059.png)
        
        - `sA` manda un ACK
        
        ![](Attachments/Pasted%20image%2020260520190107.png)
        
        - `sW` window (?)
        
        ![](Attachments/Pasted%20image%2020260520190123.png)
        
        - `sM` maimon scans (?)
        
        ![](Attachments/Pasted%20image%2020260520190130.png)
        
        - `sU` escanea con UDP (ufff ta tardando)
        
        ![](Attachments/Pasted%20image%2020260520190141.png)
        
        Voy a asumir que se quedó esperando indefinidamente, pero no hay nada abierto.
        
        - `sF` manda TCP con flag de FIN
        
        ![](Attachments/Pasted%20image%2020260520190149.png)
        
        Algunos datos interesantes:
        
        - UDP Puede saber si un puerto está cerrado si recibe una respuesta ICMP indicando dicho error.
        - `sF` puede ser conveniente versus `sS` porque muchas veces los firewalls bloquean SYNs pero no FINs.


# 5. Red

---

- Imprimir la tabla de ruteo
    
    ```bash
    route -n # No resuelve nombres
    ```
    
- Asignar una ip a una interfaz
    
    En este caso, se asigna la ip 192.168.56.2 con la mascara de red 255.255.255.0 en la interfaz enp0s3
    
    ```bash
    ip addr add 192.168.56.2/24 dev enp0s3
    ```
    
- Agregar una entrada en la tabla de ruteo
    
    Decimos que para llegar a la red 192.168.1.0 hay que pasar a traves de 192.168.56.1
    
    ```bash
    sudo route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.56.1
    ```
    
- Desactivar el firewall
    
    Si hay problemas, ver si tenemos el firewall activado
    
    ```bash
    sudo iptables -L
    
    ```
    
    Para desactivar el firewall
    
    ```bash
    sudo iptables -F
    sudo iptables -X
    
    ```
    
    Configurar a mano INPUT FORWARD y OUTPUT
    
    ```bash
    iptables -P FORWARD ACCEPT
    iptables -P INPUT ACCEPT
    iptables -P OUTPUT ACCEPT
    ```
    
- Conexion a internet: activar snat
    
    Configuramos nat con iptables, vemos las reglas de nat con:
    
    ```bash
    iptables -L -t nat
    
    ```
    
    Activamos el nat
    
    ```bash
    iptables -t nat -A POSTROUTING -o wlp2s0 -j SNAT --to-source <ipPublica>
    
    ```
    
    - o wlp2s0 nos indica que son los paquetes qeu van a salir por la interfaz de red
    
    O con masquerade
    
    ```bash
    iptables -t nat -A POSTROUTING -o wlp2s0 -j MASQUERADE
    
    ```
    
    Con configurar snat cambiamos la direccion destino. Cuando sale el paquete se almacena en una tabla de donde vino para luego redireccionarlo al host q corresponde cuando llegue la respuesta.
    
    Para mas detalles ver [Configurando nuestro propia red con NAT](https://www.notion.so/Configurando-nuestro-propia-red-con-NAT-c80efc35187f448594983adbef5ab399?pvs=21)
    
- Activar/desactivar forwarding
    
    Desactivar
    
    ```bash
    sudo su
    echo 0 > /proc/sys/net/ipv4/ip_forward
    ```
    
    Activar
    
    ```bash
    sudo su
    echo 1 > /proc/sys/net/ipv4/ip_forward
    ```
    
- Evitar la comunicacion con una red
    
    ### Suponga que existe una red 192.168.14.0/24 que está dividida en 8 subredes con máscara de 27 bits. Por un requerimiento especifico, se necesita que ningún host de su red pueda comunicarse con una de estas subredes (192.168.14.0/27, siguiendo con el ejemplo). Se pide realizar los cambios necesarios para cumplir con este requerimiento. Hacerlo de dos maneras distintas.
    
    La primera forma es usando la sugerencia. Si leemos el man atentamente, se da una breve descripcion de como funciona reject.
    
    ```
    reject: install a blocking route, which will force  a  route  lookup  to
            fail.   This is for example used to mask out networks before us‐
            ing the default route. This is NOT for firewalling.
    
    ```
    
    Con lo cual, la primer solucion es agregar a la tabla de ruteo:
    
    ```
    >> route add -net 192.168.14.0/27 reject
    
    ```
    
    Al parecer, segun el segundo link insertado en las fuentes, una segunda solucion para bloquear la comunicacion con estas subredes es poner un mock getway que pertenezca a la red local.
    
    ```
    >> route add -net 192.168.14.0/27 gw 192.168.100.99
    
    ```
    
    De esta forma, nunca habra comunicacion posible entre subredes.
    
- Asignar mas de una ip a una interfaz, usando un alias
    
    Usando el alias eth0:0 agregamos la ip 192.168.1.6 a la interfaz eth0. Todas las ips deben encontrarse en la misma subred.
    
    ```sql
    ifconfig eth0:0 192.168.1.6 up
    ```


### DHCP

Imposible que toman algo con esto sin labo de info. Pueden darte una captura de Wireshark para ver si entendiste que es un paquete DHCP.

# 6. Enlace

---

Imposible que toman algo con esto sin labo de info. Pueden darte una captura de Wireshark para ver si entendiste lo de ARP.


# 7. SSH

---

- Transferencia de archivos con SCP
    ```bash
    scp usuario@servidor.com:/archivo/a/enviar /archivo/donde/recibir
    ```

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Protos)**

- [Resumen Protos](Resumen%20Protos.md) — teoría de respaldo
- [Guias Practicas Protos](Guias%20Practicas%20Protos.md) — guías prácticas en PDF
- [Link Notion](Link%20Notion.md) — exámenes resueltos de años anteriores

<!-- notas-relacionadas:fin -->
