---
title: "Acceder por ssh con clave pública"
categories:
  - Informática
tags: 
  - ssh
  - sysadmin
---

Este tipo de artículos sobre tareas de un "sysadmin", los publicaré de vez en cuando, explicando de manera sencilla, para que tanto los que se dedican a ello, como los que no lo hacen y quieren saber más, aprendan, porque aun a día de hoy hay gente que me pregunta como "hacerlo" cuando debería ser una práctica casi obligatoria para acceder a nuestros sistemas por ssh, ya no hay que usar password hay que utilizar clave privada/clave pública.

Vamos a ello, hoy trataremos como dice el artículo, como acceder a nuestro servidor vía ssh sin contraseña pero sí con nuestra propia clave pública/privada.

<!--more-->


## Creando nuestra clave

La clave se generará en nuestro ordenador de sobremesa, portátil... en definitiva desde el cual accederemos al servidor X.

1. Generamos nuestra clave

```bash
$ ssh-keygen -t rsa -b 4096
```

2. Now do this:
   
   ```ruby
   def print_hi(name)
     puts "Hi, #{name}"
   end
   print_hi('Tom')
   #=> prints 'Hi, Tom' to STDOUT.
   ```
        
3. Now you can do this.
