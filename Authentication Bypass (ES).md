<img src="https://tryhackme-badges.s3.amazonaws.com/Skr34t.png" alt="TryHackMe">

## [Tryhackme](https://tryhackme.com) 


## [Authentication Bypass](https://tryhackme.com/room/authenticationbypass)


### Task 1 'Brief'

*Para completar esta tarea lo unico que tenemos que hacer es conectarnos a la **VPN**.* 

### Task 2 'Username Enumeration'

La enumeracion de user sirve para obtener los nombres de usuario que se utilizan para loguear en algun tipo de sistema.
Existen diversas tecnicas, en esta ocacion nos centramos en la enumeracion manual y en la herramienta fuff.

*La enumeracion **manual** de nombres de usuario puede realizarce desde una página de logueo, en la cual insertaremos un nombre conocido esperando
que la pagina nos devuelva un **aviso** de que ese nombre de usuario se encuentra en uso. Conocer el nombre de usuario nos permite realizar un ataque de fuerza bruta en la página de logueo consiguiendo asi el acceso y las credenciales de sesion.

![image](https://user-images.githubusercontent.com/92405961/217763558-9a7cbc62-93b5-4b57-91eb-f444ee741b85.png)

**Utilizando fuff para enumerar usuarios:**
Esto nos permite realizar una suerte de enumeracion manual (ya que dependemos de listas con posibles nombres de usuarios) pero de forma automatizada y extremadamente veloz. 

![image](https://user-images.githubusercontent.com/92405961/217765551-5cb88aa8-c482-4f61-862f-ac1ea17f3b58.png)

*Comando utilizado*

> **ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.244.198/customers/signup -mr "username already exists" -o validusers.txt -t 200**


*Sintaxis del comando*

> - -w wordlist (lista de los posibles nombres de usuarios) Generalmente Kali tiene instalado /usr/share/wordlists/SecLists/Usernames/Names/names.txt
> - -X metodo HTTP con el cual enviamos los datos de logeo al servidor (GET, POST, ETC)
> - -d especificamos la data que vamos a enviar (donde se encuentra el FUZZ es donde se introduciran los valores de la lista)
> - -H agrega los headers con los que se comunica el host con el servidor/aplicación. Los mismos pueden ser visualizados con burpsuite.
> - -u especifica la url del objetivo.
> - -o para indicar un output al resultado del comando
> - -t para indicar cuantos threads poner en funcionamiento, a mayor cantidad, mas rapido se hara el trabajo.
> - **-mr es el texto que esperamos la pagina nos devuelta para reconocer la existencia de un usuario, en este caso es que ese nombre de usuario ya existe**


Una vez ejecutado el comando podemos completar las respuestas

![image](https://user-images.githubusercontent.com/92405961/217765821-4952a138-231d-41c8-8b3c-a1642d20092e.png)


### Task 3 'Brute Force'

Ahora que tenemos los nombres de usuarios podemos intentar un ataque de fuerza bruta con la herramienta fuff.
En esta oportunidad el comando se utilizara con dos listas una para los nombres de usuario anteriormente encontrados y la otra sera la lista
de contraseñas con las que la herramienta intentara ingresar.

*Comando utilizado*

> **ffuf -w validusers.txt:W1,/home/kali/Tryhackme/rockyou.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.244.198/customers/login -fc 200 -t 200**

*En este caso -w esta compuesto de dos listas, la primera aclarada con W1 y la segunda con W2
En -d especificamos que la W1 sera utilizada en el nombre de usuario y W2 en contraseña (recordar que poseemos los nombres de usuario de la tarea anterior y las contraseñas de alguna lista disponible)
Con el -fc logramos que nos devuelta un codigo en particular (en este caso, el 200 que es OK para HTTP/HTTPS) y en este caso indicaria un login exitoso.*

**Una vez ejecutado el comando la herramienta comenzara a "forzar las cerraduras" y en caso de econtrar una coincidencia nos lo informara.**

![image](https://user-images.githubusercontent.com/92405961/217768638-86bb6828-bf65-4e80-8fa1-fec17390b80e.png)

**Comprobamos las credenciales con la página de login:**

![image](https://user-images.githubusercontent.com/92405961/217768950-9e8e701b-59c3-4bf6-9230-9d0d3ce2b2d9.png)





