---
title: "Acceder por ssh con clave pública"
categories:
  - Informática
tags: 
  - ssh
  - sysadmin
---

Este tipo de artículos sobre tareas de un "sysadmin", los publicaré de vez en cuando, explicando de manera sencilla, para que tanto los que se dedican a ello, como los que no lo hacen y quieren saber más, aprendan, porque aun a día de hoy hay gente que me pregunta como "hacerlo" cuando debería ser una práctica casi obligatoria para acceder a nuestros sistemas por ssh, ya no hay que usar password hay que utilizar clave privada/clave pública... ¡Vamos a ello!

Hoy trataremos como dice el artículo, como acceder a nuestro servidor vía ssh sin contraseña pero sí con nuestra propia clave pública/privada.

## Creando nuestra clave

La clave se generará en nuestro ordenador de sobremesa, portátil... en definitiva desde el cual accederemos al servidor X.

1. Generamos nuestra clave
```bash
$ ssh-keygen -b 4096
```

Con el parámetro **-b** indicamos que queremos que sea una clave robusta de 4096 bits. Con 2048 bits sería suficiente, pero me gusta ser un tipo duro :D

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

**Enter file in which to save the key (/home/jmdlr/.ssh/id_rsa):** ubicación donde se almacenará nuestra clave tanto privada como pública. Se puede modificar pero no es necesario.
**Enter passphrase (empty for no passphrase):** se puede añadir una frase por si quieres fortalecer la seguridad de tu clave. Por experiencia dejadla en blanco, terminaréis por olvidarla si no la apuntáis y tendréis que generar una nueva.

El resto de líneas solo confirman que se han creado las claves con tu figerprint asociado.
Ya estaría lista.

## Consultar nuestras claves

Ahora ya podremos ver nuestras claves generadas, si no hemos cambiado la ruta por defecto estará en una carpeta oculta dentro de la "home" de nuestro usuario llamada **.ssh/**

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

