# [Tryhackme](https://tryhackme.com/)

## [IDOR](https://tryhackme.com/room/idor)

### Task 1 'What is an IDOR?'

Esta sala esta enfocada en aprender que es una vulverabilidad IDOR.
**IDOR significa Insecure Direct Object Reference** y es una vulnerabilidad de control de acceso.

Este tipo de vulnerabilidad ocurre cuando un servidor web recibe un pedido del usuario para devolver objetos (archivos, datos, documentos).
Se ha puesto mucha confianza en el input del usuario y el server no confirma que la informacion requerida sea del usuario correspondiente.

![image](https://user-images.githubusercontent.com/92405961/218192986-9177f6e8-3c61-45a0-b29e-c4c2e6c5b7ee.png)


### Task 2 'An IDOR Example'
 
Lanzamos el sitio dentro de la tarea:
 
![image](https://user-images.githubusercontent.com/92405961/218193270-558b387a-0bc3-4452-8e9b-c2dac08e5b4b.png)

Buscamos entre las distintas opciones hasta encontrar la web que podria ser vulnerable a IDOR.

![image](https://user-images.githubusercontent.com/92405961/218193492-50adaf99-0f71-43b7-bedb-335d9f59d37d.png)

Esa direccion probablemente sea manipulable a travez de la URL para que nos traiga otro tipo de datos, revisemos.

![image](https://user-images.githubusercontent.com/92405961/218193637-fff219be-8a57-4bd8-afa5-fbc2c3958a2f.png)

Efectivamente esa pagina es vulnerable a IDOR, procedemos con lo especificado en la tarea y cambiamos la informacion de la URL para obtener la flag.

![image](https://user-images.githubusercontent.com/92405961/218193938-d0b22411-bfb1-4a19-9444-596cfc5940f1.png)

![image](https://user-images.githubusercontent.com/92405961/218194004-343bbfae-ba3e-479d-aff0-b62fb106d98f.png)


### Task 3 'Finding IDORs in Encoded IDs'

Cuando los datos pasan de pagina en pagina (sea por un metodo post, consulta, o cookies) los desarrolladores web generalmente tomaran los datos en 'crudo' (raw data)
y le realizaran un encode (codificacion). La codificacion le permite al servidor web entender el contenido.
La codificacion cambia los datos de binario a ASCII.
La tecnica mas utilizada en la web es base64 encoding y puede ser muy facil de descubrir.
Para decodificar o codificar en base64 se puede utilizar la siguiente herramienta online:
**[Base64](https://www.base64encode.org/)**

![image](https://user-images.githubusercontent.com/92405961/218274930-a199bce8-bf30-4a36-a841-0b18f8b91148.png)


### Task 4 'Finding IDORs in Hashed IDs'

Los Hashed IDs son un poco mas complicados de lidear que la codificacion. Lo bueno es que siguen un patron predecible.
Por ejemplo, el **ID N°: 123** se transformaria en **202cb962ac59075b964b07152d234b70** si se utilizara el hash **MD5**.
Una herramienta online de Hashing es:
**[Crackstation](https://crackstation.net/)**

![image](https://user-images.githubusercontent.com/92405961/218275045-fe0d5360-3c64-4cc9-bce4-d9f34aeb5416.png)

### Task 5 'Finding IDORs in Unpredictable IDs'

Si un numero de ID no puede ser detectado utilizando los metodos anteriormente encontrados, un excelente metodo para la deteccion es crear dos cuentas e intercambiar
el numero de ID entre ellas. Si se puede visualizar el contenido del otro usuario utilizando su numero de ID estando logueado desde una cuenta diferente, podemos
decir que nos encontramos ante una vulnerabilidad valida de IDOR.

![image](https://user-images.githubusercontent.com/92405961/218275139-1fdceaa2-c508-4061-ae8d-6962d011d7b9.png)

### Task 6 'Where are IDORs located'

Un endpoint vulnerable al que estamos apuntando no siempre estara visible en la URL. Puede ser contenido que nuestro navegador carga a travez de una peticion
AJAX o algo que se encuentra referenciado dentro del archivo Javascript.

Algunas veces los endpoints tienen parametros sin referencia que fueron utilizados durante la fase de desarrollo y fueron llevados a produccion. 
Por ejemplo podriamos notar una llamada a **/users/details** mostrando nuestra informacion (siendo autenticados por nuestra sesion). Pero durante un minado de parametros
descubrimos que hay un parametro llamado **user_id** que podemos utilizar para mostrar la informacion de otros usuarios, por ejemplo: **/user/detals?user_id=123**.

![image](https://user-images.githubusercontent.com/92405961/218275675-c07731a1-18ae-4e49-a2cb-35dc793bc6b9.png)

### Task 7 'A Practical IDOR Example'

Esta tarea sera un ejemplo practico de una vulnerabilidad IDOR.
Pasos a seguir:

- Nos conectamos a nuestra VPN y realizamos un deploy de la maquina objetivo.
- Nos dirigimos a la pagina del laboratorio https://10-10-231-172.p.thmlabs.com/customers/login (Esta es la direccion a la que me corresponde conectarme).
- Creamos una nueva cuenta.
- Abrimos las herramientas de desarrolador y dentro de nuestra cuenta nos dirigimos al apartado 'Your Account'.

Podemos visualizar a traves de las herramientas de desarrollador que se realiza una peticion JSON al backend con el customer?id=15

![image](https://user-images.githubusercontent.com/92405961/218276684-6e0ab9c2-43c5-4c49-8225-93f2bbd629eb.png)

Podemos reforjar la peticion para que nos devuelva los resultadas para el userid1 y el userid3 y asi responder las preguntas de la tarea.
Para ello realizamos un resend de la peticion original con el valor ID del usuario en 1 y revisamos la respuesta dentro de la consola.

![image](https://user-images.githubusercontent.com/92405961/218276905-fdd4f08e-bdcb-4ca0-a297-2b85f7e57a03.png)

Realizamos lo mismo pero para el user ID 3.

![image](https://user-images.githubusercontent.com/92405961/218276975-928d124d-5053-43ed-ad06-d764e65b26f9.png)

![image](https://user-images.githubusercontent.com/92405961/218277016-25e81f43-fe95-47bc-a5d9-ae0c90b9713e.png)


### The End






