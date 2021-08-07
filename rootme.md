![Vulnversity](https://imgur.com/jtQbnLs.png)

Hola!!

En este artículo vamos a enfrentarnos a una máquina de la plataforma Try Hack Me. Esta disponible de manera gratuita.

Descripción: "A ctf for beginners, can you root me?"

Dificultad: 1/10

--------------------------------------------------------

Lo primero que vamos a hacer, como siempre, es conectarnos a la maquina y a nuestra VPN personal de THM.

Una vez establecida la conexión empezaremos con un escaneo de los puertos abiertos en la IP de la víctima bajo el protocolo TCP.

![1 port scan](https://imgur.com/opON3XM.png)

Vemos que hay 2 puertos abiertos y vemos que el puerto 80 esta abierto por lo que ya nos podemos esperar una página web. Por lo que vamos a hacer un escaneo más profundo de los servicios que corren bajo los puertos descubiertos.

![2 services under ports](https://imgur.com/EyH4eV9.png)

Como decíamos, hay una página web bajo el puerto 80. Así que vamos a sacar información de ella con la herramienta whatweb.

![3 web info](https://imgur.com/qZykRe4.png)

En este caso, podemos ver que la página no es la que trae por defecto Apache por lo que nos podemos esperar que haya contenido dentro de la web o en algún subdominio que contenga. Sabiendo esto, vamos a hacer fuzzing a la web con la herramienta wfuzz:

![4 fuzzing](https://imgur.com/KuTwBzH.png)

Vemos que nos ha sacado 3 directorios. Vamos a ver que contienen.

El directorio IP/panel vemos que nos da permisos para subir archivos a la web, por lo que vamos a descargarnos una reverse shell:

![5 downloading reverse shell](https://imgur.com/d2xWGuH.png)

La modificamos con nuestros datos y procedemos a subirla. Vamos a probar en diferentes formaos de php hasta que encontremos uno que nos deje.

![7 reverse shell uploaded via php3](https://imgur.com/pHGuXsj.png)

Al final nos ha dejado con el formato .php3 ya que .php estaba restringido. Una vez subido el archivo nos hemos de dirigir al directorio IP/uploads. Ponernos a la escucha con Netcat y abrir nuestra reverse shell que previamente hemos subido a la web:

![8 opening revshell](https://imgur.com/EggjRRR.png)

Y ya estaríamos dentro, ahora quedaría encontrar la flag del user y después proceder a escalar privilegios. Por lo que vamos a buscar la flag:

![9 user flag](https://imgur.com/toQ5BwK.png)

Esta vez me ha costado un poco mas encontrarla ya que se encontraba en un directorio poco común. Pero bueno lo importante es que la tenemos.

Para la escalada de privilegios vamos a buscar programas que tengan privilegios SUID:

![10 searching SUID vulns](https://imgur.com/5lhl6Uv.png)

Como vemos, Python esta entre estos programas por lo que tenemos todo listo para proceder a ejecutar una shell con privilegios de super usuario. Para ello buscaremos en Google como iniciar una shell en Python teniendo permisos SUID y procedemos a copiar el comando en la máquina de la víctima:

![11 executing shell with python as sudo](https://imgur.com/89jHWIf.png)

¡Et voila! ya estaríamos dentro de la maquina con permisos SU por lo que solo nos quedaría encontrar la flag del usuario root. 

![12 root flag](https://imgur.com/q3EgMoo.png)

Como veis, una máquina muy sencillita pero que engloba lo básico para iniciarse.
