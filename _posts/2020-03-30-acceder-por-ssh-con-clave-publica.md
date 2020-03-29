---
title: "Acceder por ssh con clave pública"
categories:
  - Informática
tags: 
  - ssh
  - sysadmin
---

Este tipo de artículos sobre tareas de un "sysadmin", los publicaré de vez en cuando, explicando de manera sencilla, para que tanto los que se dedican a ello, como los que no lo hacen y quieren saber más, aprendan, porque aun a día de hoy hay gente que me pregunta como "hacerlo" cuando debería ser una práctica casi obligatoria para acceder a nuestros sistemas por ssh, ya no hay que usar password hay que utilizar clave privada/clave pública... ¡Vamos a ello!
{: .text-justify}

Hoy trataremos como dice el artículo, como acceder a nuestro servidor vía ssh sin contraseña pero sí con nuestra propia clave pública/privada.
{: .text-justify}
## Creando nuestra clave

La clave se generará en nuestro ordenador de sobremesa, portátil... en definitiva desde el cual accederemos al servidor X.

1. Generamos nuestra clave
```bash
$ ssh-keygen -b 4096
```

Con el parámetro **-b** indicamos que queremos que sea una clave robusta de 4096 bits. Con 2048 bits sería suficiente, pero me gusta ser un tipo duro :D
{: .text-justify}
Durante la generación nos hará una serie de preguntas que tendremos que responder si queremos cambiar algo o bien dejar por defecto:
<pre>
jmdlr@casa:~$ ssh-keygen -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jmdlr/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/jmdlr/.ssh/id_rsa.
Your public key has been saved in /home/jmdlr/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:e1S4R43AcFqR3NADcOtS4wDth2RYAojBQp3E28WUGIQ jmdlr@casa
The key's randomart image is:
+---[RSA 4096]----+
|+++o=+B===OB     |
|o..E ..*+=+++o   |
|.   o .+o.= +..  |
|   . .  o=.=     |
|        S.= .    |
|         + .     |
|        . .      |
|         .       |
|                 |
+----[SHA256]-----+
</pre>

Breve explicación de cada pregunta:

* **Enter file in which to save the key (/home/jmdlr/.ssh/id_rsa):** ubicación donde se almacenará nuestra clave tanto privada como pública. Se puede modificar pero no es necesario.
* **Enter passphrase (empty for no passphrase):** se puede añadir una frase por si quieres fortalecer la seguridad de tu clave. Por experiencia dejadla en blanco, terminaréis por olvidarla si no la apuntáis y tendréis que generar una nueva.
{: .text-justify}
El resto de líneas solo confirman que se han creado las claves con tu figerprint asociado.
Ya estaría lista.

## Consultando nuestra clave pública

Ahora ya podremos ver nuestras claves generadas, si no hemos cambiado la ruta por defecto estará en una carpeta oculta dentro de la "home" de nuestro usuario llamada **.ssh/**
{: .text-justify}
Ahora tendríamos que ir a nuestra "home" en mi caso es **/home/jmdlr** y luego acceder a la carpeta oculta .ssh:
```bash
$ cd /home/jmdlr/
$ cd .ssh/
$ ls -lrth
total 8.0K
-rw------- 1 jmdlr jmdlr 3.2K Mar 30 00:42 id_rsa
-rw-r--r-- 1 jmdlr jmdlr  736 Mar 30 00:42 id_rsa.pub
```
Veremos que tenemos dos ficheros que tienen por nombre **id_rsa**. Una de ellas es la clave privada (id_rsa) la cual **NO** tendremos que compartir y luego la clave pública (id_rsa.pub) que es la que subiremos a nuestro servidor o al servidor que deseemos acceder sin que nos solicite password.
{: .text-justify}
Echamos un vistazo a la clave pública:
```bash
$ cat id_rsa.pub
```
<pre>
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDDGnQn+2Jld5Y/822sLmZc/1PC0+gm3ORtbRPEUvzuqMIMOVg2ImZPApj/d7+K57MPy7pQDn/iwi3QkLzpf/tL9LXF2+FouzVsSeZgR1dd7+3QGTMPch+5hXZxuims+zZOLFuXqXzhb00TSMbH5yVBH1gv+qCZj7N/DvRgok+2Fae1I+Fr0/7ALIaLfFEo5CYfrUNyfuD0ZpJ9GIrv6+sBWbCOomzuw5SMIccbX4VQ+DLmEoMwl3A3GQf3yNrAtjjolPm1zJ0YAd+Kr5UWREugxTgLslnMHFVozPn5byuj2fT4dQPiB0Zj/mzQf+BWGaT+lI3P/P08mG0gdpD39MUiKGjh0XYbQQh731cnMJK1gTzphPyinROv4S/Pj1HFhSEAbsZJP9l/ZyVi1qupTOFocWEQf0o4yNsVpogDpUQJwEyG5ftQDsmjbHD5EQRMFXktnItdSqhbTVL+qSiB96Jd66CV9+bklUQbp9w3pT0beZDrKQdPKmrlRYS2MeZB1z5kMnDTm/C1fSDHAuqYAxCyZTBc3ncBpzW7AnWczUdk0zIiToGVs8wE16suGWqlZA2gGLIkY3LdvAkzgm9HBIosgZVEPABN4nbMAqv/NqXJd2/e6cFICllX8YhyBcYL9OfwFPK6OUxQg0AKk/rJwSO6aGxDOIHVY16+DvwuQRx1SQ== jmdlr@casa
</pre>
Ese texto es lo que tenemos que copiar en todos los servidores remotos a los que queramos conectar vía ssh, pero para hacerlo más fácil, existe un comando que nos ayudará a copiarlo.
{: .text-justify}
## Copiando la clave pública al servidor

Simplemente usaremos el siguiente comando desde la ruta donde tenemos nuestra claves.

```bash
$ cd /home/jmdlr/.ssh
$ ssh-copy-id -i id_rsa.pub usuario@IP_servidor_remoto
```
Donde aparece **usuario** tendremos que poner nuestro nombre de usuario que utilizamos para acceder al servidor y donde aparece **IP_servidor_remoto** la dirección IP o el registro DNS que nos de acceso al servidor donde queremos añadir nuestra clave pública.
{: .text-justify}
En mi ejemplo, accediendo con mi usuario "jmdlr" sería así:
```bash
$ cd /home/jmdlr/.ssh
$ ssh-copy-id -i id_rsa.pub jmdlr@10.5.1.5
```
Y si todo va bien me diría lo siguiente hasta preguntarme cual es mi **password** del usuario para acceder por primera vez:
<pre>
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys

jmdlr@10.5.1.5's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'jmdlrk@10.5.1.5'"
and check to make sure that only the key(s) you wanted were added.
</pre>

En el paso que solicita el "password" tendremos que añadirlo y para la próxima vez que queramos acceder tendremos que hacer lo que indica más abajo, simplemente teclearemos **ssh jmdlr@10.5.1.5** y estaremos dentro sin solicitarnos contraseña alguna.
{: .text-justify}
Espero que os haya sido fácil de seguir, cualquier cosilla preguntadme en mi cuenta de Twitter: [@jmdelosreyes](https://twitter.com/jmdelosreyes)

>logout<
