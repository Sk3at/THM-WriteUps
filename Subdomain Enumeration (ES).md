![image](https://tryhackme-badges.s3.amazonaws.com/Skr34t.png)


# [TryHackme Room](https://tryhackme.com/) 

## Subdomain Enumetarion


*Hola, bienvenid@ a este, mi primer writeup de una sala de Tryhackme. En este caso vamos a completar la sala [Subdomain Enumeration](https://tryhackme.com/room/subdomainenumeration).*

### Task 1 'Brief'
*La enumeración de subdominios nos permite obtener mas vectores de ataque, o en todo caso, mas informacion de nuestro objetivo.*

**En esta sala solo vamos a recorrer tres técnicas de enumeración de subdominios:**
  - Brute Force
  - OSINT (Open-Source Intelligence)
  - Virtual Host


![image](https://user-images.githubusercontent.com/92405961/217439623-7f85ce38-1706-4b69-af08-789b7ede31b7.png)


### Task 2 'SSL/TLS Certificates'

*Cuando un certificado **SSL/TSL** es creado para un dominio por una **CA** se crea un log. Esto quiere decir que cada vez que se crea un certificado para un
dominio, se crea un log publico para ese dominio. Lo que nos permite realizar un revision de estos certificados para ver los subdominios creados para un dominio en particular.*

**CA (Certificate Authority)**

**SSL/TLS (Security Socket Layer / Transport Security Layer)**

*Las siguientes páginas nos ofrecen una base de datos en la cual buscar estos logs historicos y actuales:*
  - https://crt.sh
  - https://ui.ctsearch.entrust.com/ui/ctsearchui 

**Para responder la pregunta de esta tarea debemos ingresar a https://crt.sh y buscar tryhackme.com , una vez hecho esto podemos utilizar las teclas 'CTRL + F'
y buscar la fecha correspondiente (en mi caso: 2020-12-26)**
**Con esta busqueda solo dos resultados coinciden**
![image](https://user-images.githubusercontent.com/92405961/217442140-541baeaf-2120-4759-949d-76408da6fd10.png)

![image](https://user-images.githubusercontent.com/92405961/217442282-604f6d59-a4d4-4858-84c9-3aa832090e75.png)


### Task 3 'OSINT - Search Engines'

*Podemos utilizar los motores de busqueda para que nos brinden la informacion que estamos buscando si somos precisos a la hora de tipear nuestra busqueda.
Es por ello que para obtener mas presición utilizaremos los **Google Search Operators***

https://ahrefs.com/blog/google-advanced-search-operators/

Para responder la pregunta a continuacion simplemente utilizamos la siguiente busqueda:
- -site:www.tryhackme.com  site:*.tryhackme.com

La cual nos devuelve lo siguiente:

![image](https://user-images.githubusercontent.com/92405961/217443671-5cdb3aa3-5442-4115-a2ec-07bbc6aa47b9.png)

![image](https://user-images.githubusercontent.com/92405961/217443751-4af64608-4530-4fbc-a289-f8a2244226bc.png)

### Task 4 'DNS Bruteforce'

*Utilizando el comando **dnsrecon** podemos obtener listas de posibles subdominios. Cabe destacar que es una herramienta con multiples opciones para la enumeración.
En caso de ser necesario se puede recurrir al manual de la misma **man dnsrecon**.*
Desplegando el sitio para la tarea:
![image](https://user-images.githubusercontent.com/92405961/217450689-83147867-7b82-481c-a18c-91d39757d05f.png)

![image](https://user-images.githubusercontent.com/92405961/217450770-a0536eba-2453-49fb-9038-32d17bd5ee7d.png)


### Task 5 'OSINT - Sublist3r'

*Todas estas tareas las podemos realizar de forma automatica utilizando herramientas disponibles diseñadas por la comunidad.
Una de estas herramientas es [Sublist3r](https://github.com/aboul3la/Sublist3r) la cual realiza busquedas automatizadas en varios motores de busqueda.*

![image](https://user-images.githubusercontent.com/92405961/217454821-5ae62010-6de9-49bd-a86f-3cc20de692d4.png)

![image](https://user-images.githubusercontent.com/92405961/217454930-239e5307-b809-43a0-a9f0-4b7c6a00f904.png)

Con esta información podemos responder la pregunta:

![image](https://user-images.githubusercontent.com/92405961/217455077-2066b78b-b518-43fb-8d6c-8d968e3af66f.png)


### Task 6 'Virtual Hosts'

*En esta ultima tarea utilizaremos otra herramienta, llamada **ffuf**, está nos permite realizar una enumeracion de subdominios "localizada" y su sintaxis es la siguiente:
**ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP***

![image](https://user-images.githubusercontent.com/92405961/217464178-aae3e91c-706a-468e-8867-1a5a3b7c86b3.png)

![image](https://user-images.githubusercontent.com/92405961/217464284-1646949d-df12-494a-a93e-9ab4b4669b39.png)



### The End
