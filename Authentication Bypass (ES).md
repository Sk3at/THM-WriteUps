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

Una vez chequeadas nuestras credenciales podemos responder la pregunta de la tarea.

![image](https://user-images.githubusercontent.com/92405961/218029624-4c339c75-2d5f-4d1e-9804-a3ef86b6c960.png)


### Task 4 'Logic Flaw'

*Un Logic Flaw, tambien conocido como "fallo logico" se presenta cuando se pasa por alto o manipula la ruta logica de la aplicación.*

En esta tarea examinaremos la funcion Reset Password de la web de Acme IT Support. Con fines demostrativos, utilizamos el mail de robert provisto en esta tarea.

![image](https://user-images.githubusercontent.com/92405961/218020447-1f7aaef4-7af1-4505-ae91-4d2c07e00f26.png)

Lo acabamos de realizar desde el navegador web, tambien es posible realizarlo con el comando **curl**

> **curl 'http://10.10.16.24/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'**

*En la logica de la aplicacion de esta web, en la funcion password reset, el mail es enviado usando los datos encontrados en la variable $_REQUEST de PHP.

La mejor forma de explicar la funcionalidad de esto es la siguiente, cuando ingresamos la funcion reset password se nos pide que ingresemos como primer paso un email valido, luego de eso se nos pide un nombre de usuario valido, cuando ingresamos el nombre de usuario y le damos a enviar, la logica de la aplicacion no compara todo el bloque enviado, ya que da por sentado que el bloque anterior (el email) fue exitoso, por lo tanto solo compara el nombre de usuario, lo que nos permite ingresar 'nuestro' mail en su lugar y de esa forma obtener un ticket de reinicio de contraseña para la cuenta del objetivo en nuestra direccion de correo.

**Veamos esto en accion:**

*Primero crearemos una cuenta en la web.
Luego utilizaremos la herramienta **curl** para 'reforjar' la peticion de password reset y enviar ese ticket a nuestra cuenta recien creada.*

![image](https://user-images.githubusercontent.com/92405961/218025526-15599695-61db-4b77-9019-de1733d090e0.png)

*Dentro de la respuesta a nuestro comando podemos observar lo siguiente:*

![image](https://user-images.githubusercontent.com/92405961/218025717-bf07da43-3d09-4b53-a4ff-16f20ac2cda4.png)


Lo que quiere decir que el comando se proceso exitosamente y dentro de nuestra cuenta recien creada deberiamos tener un ticket de soporte para realizar un reseteo de contraseña de la cuenta de robert. *Vamos a chequearlo*

![image](https://user-images.githubusercontent.com/92405961/218025972-b448b847-61a0-4abb-942a-7592d3399256.png)

Efectivamente vemos que se creo un nuevo ticket en nuestra cuenta, si ingresamos en el y seguimos su instruccion (entrar en un link detallado en el ticket), se nos redirigira a la cuenta perteneciente a robert, dentro de su cuenta podemos observar el ticket que creamos y dentro del mismo estara la *flag* de esta tarea.

### Task 5 'Cookie Tampering'

Las cookies muchas veces nos pueden otorgar mejores accesos que los usuarios normales, para ello muchas veces es necesario realizarles un decoding, modificar sus valores y volverlas a ensamblar.

Siguiendo las indicaciones de la tarea enviamos el siguiente comando para obtener la flag de la tarea.

![image](https://user-images.githubusercontent.com/92405961/218028009-cbf96def-4a71-47a9-aaff-44c8b137b702.png)

![image](https://user-images.githubusercontent.com/92405961/218028103-0708a0f9-6837-4a9e-a4b8-07271171a407.png)

Para la segunda pregunta recurrimos a crackstation, lo que nos da la segunda respuesta.

![image](https://user-images.githubusercontent.com/92405961/218028365-649af6a1-5502-422c-bd0a-68e0d11b36f7.png)

![image](https://user-images.githubusercontent.com/92405961/218028402-ccd85bb9-cf5c-45c6-95ca-7d3c954ce8bd.png)

Para la tercera pregunta recurriremos a base64 decode, y asi obtendremos la tercera respuesta.

![image](https://user-images.githubusercontent.com/92405961/218028674-fc664f8c-32b1-477c-a38d-8ecd2a477656.png)


Para la cuarta y ultima pregunta de esta sala utilizaremos base64 encode y obtendremos la respuesta.

![image](https://user-images.githubusercontent.com/92405961/218029027-fe13e611-1fae-4f95-8a5f-723f27a8fc52.png)


### The End
