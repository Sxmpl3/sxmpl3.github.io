---
layout: single
title: Shoppy Writeup
excerpt: "Shoppy es una máquina easy de la plataforma HackTheBox"
date: 2023-01-12
classes: wide
header:
  teaser: /assets/images/shoppy/shoppyteaser.png
  teaser_home_page: true
  icon: #/assets/images/thm.png
categories:
  - Writeup
tags:
  - Shoppy
---

## Writeup Shoppy HackTheBox

Empezaremos realizando un escaneo de puertos abiertos a la dirección IP que nos ofrecen

En el escaneo obtendremos la siguiente información:

![](/assets/images/shoppy/nmap.png)

Si leemos la cabezera de la web obtendremos el dominio: http://shoppy.htb

Añadiremos el dominio al archivo /etc/hosts y haremos una visita a la página web

![](/assets/images/shoppy/shoppy.png)

Dado que no encontramos nada más que un simple contador, fuzzearemos directorios para ver si encontramos algo interesente

![](/assets/images/shoppy/wfuzzdirect.png)

Encontramos el directorio login y el directorio admin, vamos a probar con el directorio login

![](/assets/images/shoppy/bypasslogin.png)

Podremos bypassear el login con el payload que se muestra en la foto

![](/assets/images/shoppy/shoppylogin1.png)

Tras logearnos, podremos un buscador de usuarios 

![](/assets/images/shoppy/shoppylogin2.png)

Podremos obtener informacion acerca de los usuarios registrados utilizando el payload anterior

![](/assets/images/shoppy/shoppylogin3.png)

Al exportar la busqueda obtenemos 2 usuarios y 2 contraseñas

![](/assets/images/shoppy/hash.png)

Descifrar el hash de josh con [crackstation](https://crackstation.net/)

Fuzzeando subdominios encontramos el siguiente: mattermost.shoppy.htb, lo añadiremos al archivo /etc/hosts

![](assets/images/shoppy/mattermostlogin.png)

Al hacer una visita a la web, nos reenviara a un login

Podremos logearnos con las credenciales que hemos obtenido anteriormente

![](/assets/images/shoppy/mattermost2.png)

Encontramos un chat en el que salen las siguientes credenciales: jaeger:Sh0ppyBest@pp!

![](/assets/images/shoppy/shell1.png)

Podremos conectarnos por ssh con las credenciales obtenidas

![](/assets/images/shoppy/flag1.png)

Podremos leer la primera flag sin ningun tipo de problema


## Escada de privilegios

Realizaremos un "sudo -l" para ver privilegios de sudoers

![](/assets/images/shoppy/shell2.png)

Como podemos ver, podemos ejecutar el binario "password-manager" como el usuario "deploy"

![](/assets/images/shoppy/shell3.png)

Al ejecutar el archivo podemos observar que tenemos escribir una contraseña

Si le hacemos un cat al archivo "password-manager" obtendremos la contraseña "Sample"

![](/assets/images/shoppy/shell4.png)

Al escribir la contraseña "Sample", obtendremos las credenciales del usuario "deploy"

![](/assets/images/shoppy/shell5.png)

Nos conectaremos por ssh con las credenciales anteriores

![](/assets/images/shoppy/shell6.png)

Si reecordamos la información que hemos leido en el mattermost, sabremos que hay un docker corriendo en la máquina, en este caso, tiraremos de [gtfobins](https://gtfobins.github.io/gtfobins/docker/#shell) para escalar privilegios

![](assets/images/shoppy/flag2.png)

¡Listo!, hemos pwneado shoppy :)
