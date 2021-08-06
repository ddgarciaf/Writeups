![Vulnversity](https://imgur.com/VWmhR9j.png)

Hola!!

En este artículo vamos a enfrentarnos a una máquina de la plataforma Try Hack Me. Esta disponible de manera gratuita.

Descripción: "Easy Linux machine to practice your skills."

Dificultad: 2/10

--------------------------------------------------------

Lo primero que vamos a hacer, como de costumbre es conectarnos a la maquina y a nuestra VPN personal de THM.

Una vez establecida la conexión empezaremos con un escaneo de los puertos abiertos en la IP de la víctima bajo el protocolo TCP.

![1 port scan](https://imgur.com/nWj8NKl.png)

Vemos que hay 2 puertos abiertos y vemos que el puerto 80 esta abierto por lo que ya nos podemos esperar un servicio http. Vamos a hacer un escaneo de los servicios bajo esos puertos para asegurarnos:

![2 service scan](https://imgur.com/rHy7aAS.png)

Efectivamente, vemos que bajo el puerto 80 corre una página web. Vamos a sacar información básica como el servidor, versión, título, etc..

 ![3 web info](https://imgur.com/S4FHe00.png)

Vemos que la página bajo este puerto es la página predefinida por el servidor Apache la cual no suele traer información interesante por lo que vamos a intentar hacer fuzzing a ver si encontramos algún subdominio.

![4 fuzzing main page](https://imgur.com/pqoWRxM.png)

Solo hemos obtenido una web y parece estar en construcción. En otros casos habría dejado de investigar por este vector pero al saber que solo estaba el puerto 80 y el 22 abiertos vamos a asegurarnos de que no haya ningún subdominio bajo esta página web. Vamos a usar la herramienta wfuzz:

![5 fuzzing subdomain](https://imgur.com/IF06atv.png)

Estamos de enhorabuena, nos ha sacado 5 subdirectorios vamos a inspeccionar uno a uno a ver que podemos sacar de ellos:

![6 searching directories](https://imgur.com/Mypjnm1.png)

De los 5 subdirectorios destaca un panel de identificación y otro subdirectorio con capacidad de listar el contenido de la página. Vamos a ver si hay algún archivo de interés para sacar información de algún usuario.

![7 downloading db](https://imgur.com/frZ7s0h.png)

Bueno pues vemos que hay una copia de seguridad de la base de datos por lo que lo mas seguro es que podamos encontrar alguna base de datos con los usuarios y sus contraseñas encriptadas:

![8 searching credentials](https://imgur.com/ioqtERD.png)

¡Et voila! No solo hemos encontrado credenciales sino que hemos encontrado las credenciales del administrador. Vamos a identificar el método de encriptación con hash-identifier:

![9 hash identifier (command)](https://imgur.com/zXGOpS4.png)

Posiblemente sea MD5, así que vamos a comprobarlo intentándolo desencriptar con hashcat.

![code](https://imgur.com/JIOE5Nr)

![10 hash crack (command)](https://imgur.com/QaVG9ro.png)

¡Listo! Tenemos el usuario y la contraseña del admin, vamos a intentar identificarnos desde el panel de la web:

![11 login panel](https://imgur.com/TCfHDUp.png)

¡Estamos dentro! Si recordamos teniamos capacidad de directory listing y vimos archivos .php por lo que vamos a entablar una reverse shell por php subiéndola a un archivo ya existente de la página o creando uno.

Nos ponemos a la escucha con netcat y ...

![13 reverse TCP connection](https://imgur.com/jDTqaRj.png)

Efectivamente, estamos en la máquina (sin privilegios de administrador) por lo que primeramente vamos a buscar la flag del user:

![14 user flag](https://imgur.com/98kGPnz.png)

¡Perfecto! Ahora nos queda la fase de escalada de privilegios, vamos a hacer un listado de aplicaciones o servicios que podamos ejecutar como sudo con "sudo -l":

![15 searching files-programs](https://imgur.com/1kHwfB3.png)

Tenemos capacidad de ejecutar Perl y un archivo "".pl", así que en el caso de que podamos editar ese archivo ya tendríamos todo listo veamos si podemos.

![16 searching file content-data](https://imgur.com/ei6mIzV.png)

No ha habido suerte. Pero viendo mas detenidamente el archivo lo único que hace es ejecutar un script en bash ubicado en el directorio /etc que por lo general tenemos capacidad de edición.

Vamos a intentar cambiar el contenido de ese archivo de modo que nos ejecute una reverse shell a nuestro ordenador.

![17 changing file content](https://imgur.com/JHdrd3D.png)

Y ahora solo nos queda ponernos en escucha y ejecutar el script con Perl desde el archivo "backup.pl"

![18 privilege scalation](https://imgur.com/9Kbj9J7.png)

¡Listo! Ya estaríamos dentro con privilegios de administrador ahora solo nos quedaría hacer un tratamiento de la TTY (opcional) y buscar la flag del usuario root.

![19 root flag](https://imgur.com/bjkkGLc.png)

