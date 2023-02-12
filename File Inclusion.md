## [TryHackme](https://tryhackme.com)

## [File Inclusion](https://tryhackme.com/room/fileinc)

## Task 1 'Introduction'

Esta sala nos equipara con el conocimiento esencial para explotar la vulnerabilidad de File Inclusion.
Incluyendo Local File Inclusion (LFI), Remote File Inclusion (RFI) y Directory Traversal.
Tambien discutiremos los riesgos de este tipo de vulnerabilidades y sus remediaciones.

En algunos escenarios, las aplicaciones web estan escritas para solicitar acceso a archivos en el sistema,
incluyendo imagenes, texto estatico, etc, via parametros. Estos parametros son parametros de consulta
incluidos en la URL. La siguiente imagen nos dara un aproximamiento.

![image](https://user-images.githubusercontent.com/92405961/218331239-cd5c5e3c-edb0-426b-8c49-9a771224c3c7.png)

Pongamos de ejemplo un escenario donde un usuario solicita acceso a archivos en un servidor web.
Primero, el usuario envia una solicitud HTTP al servidor web que invluye el archivo a mostrar.
Si el usuario quisiera acceder a su CV dentro de la aplicacion web, la solicitud se veria de la siguiente manera:
**http://webapp.thm/get.php?file=userCV.pdf** donde **file** es el parametro y **userCV.pdf** es el archivo.

![image](https://user-images.githubusercontent.com/92405961/218331400-5f741818-97bd-40a4-abbc-180a50a2bddf.png)

¿Por que ocurren las vulnerabilidades de File Inclusion?
El problema principal es que no se realiza la input validation, donde la entrada no esta correctamente
sanitizada, por ende el usuario la puede controlar pasando cualquier input a la funcion generando la vulnerabilidad.

¿Cual es el riesgo de File Inclusion?
Depende del atacante, ya que podria utilizar File Inclusion para leer data sensible.
Incluso si el atacante pudiese escribir en el directorio **/tmp** seria posible ganar un RCE.
(Remote Command Execution).

![image](https://user-images.githubusercontent.com/92405961/218331620-b63e896e-d283-4fe5-82c9-4205d92fe4e1.png)

### Task 2 'Deploy the VM'

Nos conectamos a nuestra VPN y realizamos el deploy de la maquina.

![image](https://user-images.githubusercontent.com/92405961/218331707-68f31c33-92f8-49e2-8a23-94a6554ec3bd.png)


### Task 3 'Path Traversal'

Tambien conocido como Directory Traversal, esta vulnerabilidad web permite al atacante
leer los recuros del sistema operativo, como archivos locales corriendo en el servidor que contiene la aplicacion.
El atacante explota esta vulnerabilidad manipulando y abusando la URL de la aplicacion para localizar
y acceder a archivos o directorios que se encuentran fuera del directorio de la aplicacion.

Este tipo de vulnerabilidades ocurren cuando un usuario pasa un input a una funcion
(como **[file_get_contents](https://www.php.net/manual/en/function.file-get-contents.php)** en **PHP**). Es importante notar que la funcion no es el contribuidor 
principal de la vulnerabilidad, sino la poca sanitizacion del input.

Esta vulnerabilidad tambien tiene el nombre de **dot-dot-slash** ya que se aprovecha del movimiento
entre directorios utilizando **../**. Si el atacante encuentra el punto de entrada (en caso de php seria algo como
**get.php?file=**) entonces podria enviar algo asi a travez de la URL:
> http://webapp.thm/get.php?file=../../../../etc/passwd

A continuacion una pequeña lista de ubicaciones utiles:

![image](https://user-images.githubusercontent.com/92405961/218332486-664f7eb0-9169-4c5b-acdc-a6a722aa0e11.png)

![image](https://user-images.githubusercontent.com/92405961/218332542-bd2cd669-c792-4b5d-b766-e30e301ac34f.png)


### Task 4 'Local File Inclusion - LFI'

Esto trabaja como veniamos discutiendo en la tarea anterior asi vamos a ponerlo en practoca.

Lab #1
Primero nos dirigimos a la welcome.php
Luego realizamos LFI modificando la URL para que nos devuelva el /etc/passwd
![image](https://user-images.githubusercontent.com/92405961/218333438-632927ef-f30d-4866-be5f-8660de70eafb.png)

Lab #2
Colocamos un input erroneo para verificar a que directorio apunta la function.include
![image](https://user-images.githubusercontent.com/92405961/218333589-778ee975-a877-4b51-a0c8-80b8bf9122be.png)

![image](https://user-images.githubusercontent.com/92405961/218333607-55078a2a-451b-46fc-b8d9-c2cb13fcd135.png)






