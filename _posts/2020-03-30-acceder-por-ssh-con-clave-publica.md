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
```bash
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

Breve explicación de cada pregunta:
