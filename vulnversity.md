![Vulnversity](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\Vulnversity.png)

Hola!!

En este artículo vamos a enfrentarnos a una máquina de la plataforma Try Hack Me. Esta disponible de manera gratuita.

Descripción: "Learn about active recon, web app attacks and privilege escalation."

Dificultad: 2/10

--------------------------------------------------------

Lo primero que vamos a hacer, como de costumbre es conectarnos a la maquina y a nuestra VPN personal de THM.

Una vez establecida la conexión empezaremos con un escaneo de los puertos abiertos en la IP de la víctima.

![1 port scan](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\1 port scan.png)

Vemos que hay 6 puertos abiertos y entre ellos tenemos el 3333, el cual destaca al ser un puerto no muy común en los escaneos que solemos realizar, vamos a ver un poco mas haciendo un escaneo de los servicios bajo estos puertos y sus respectivas versiones: 

![2 services version scan](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\2 services version scan.png) 

encontramos que bajo el puerto 3333 hay una pagina web con el título "Vuln University" vamos a obtener un poco de información de ella con la herramienta whatweb.

![3 web info](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\3 web info.png)

Vemos que la página esta corriendo mediante el servidor Apache en su versión 2.4.18.

Vamos a hacer fuzzing a la web para ver posibles subdominios con al herramienta wfuzz, a ver si encontramos algo de interés:

![4 fuzzing](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\4 fuzzing.png)

Nos ha sacado 5 directorios vamos a ver que podemos sacar de ellos inspeccionandolos 1 a 1:

![5 directories](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\5 directories.png)

Revisando todos los subdominios uno a uno, destaca el /internal por la capacidad de subir archivos. Por lo que si conseguimos una extensión válida podríamos subir una reverse shell a la página y poder penetrar la máquina de la víctima.

![6 extensions](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\6 extensions.png)

Probando una a una las extensiones más comunes vemos que nos deja subir archivos ".phtml" por lo que buscaremos en internet una reverse shell en php y nos la guardaremos en nuestro archivo ".phtml".

![7 reverse shell search](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\7 reverse shell search.png)

Modificaremos la IP de escucha por nuestra IP y lo subiremos al subdominio /internal:

![8 uploading shell](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\8 uploading shell.png)

Ahora lo que haremos sera irnos al directorio /internal/uploads que nos lo facilita THM, pero en el caso de que no nos lo facilitará tendríamos que realizar otro fuzzing, en este caso al subdominio /internal.

Una vez estemos en el directorio de los ficheros subidos nos abriremos a la esucha con NetCat el puerto que hayamos especificado en la reverse shell y ejecutaremos la shell desde la web:

![9 running reverse shell](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\9 running reverse shell.png)

¡Estamos dentro! Vemos que no tenemos privilegios asi que vamos a buscar la flag del user:

![10 user flag](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\10 user flag.png)

¡Et voila! Ya tendríamos toda la intrusión y la flag del user ahora solo nos quedaría la escalada de privilegios. Por lo que vamos a buscar archivos con privilegios SUID para poder aprovecharnos de ellos:

![11 searching SUID files](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\11 searching SUID files.png)

Vemos que hay una gran cantidad de archivos con privilegios SUID por lo que la escalada de privilegios se podria hacer de más de una forma nosotros lo haremos mediante la creación de un servicio aprovechando que systemctl tiene SUID.

Por lo tanto vamos a crearnos un servicio que ejecute una shell como usuario root y nos entable una reverse shell:

![12 creating root service](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\12 creating root service.png)

Quedaría tal que así, pues una vez creado el servicio vamos a copiarnoslo en la carpeta /tmp de la víctima que por lo normal un usuario tiene privilegios de escritura en esa carpeta.

Para hacerlo usaremos SimpleHTTPserver o su correspondiente http.server para Python3:

![13 downloading root service on victims pc](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\13 downloading root service on victims pc.png)

¡Ya tenemos todo lo necesario! Vamos a ver si todo ha salido bien.

Nos ponemos a la escucha, ejecutamos el servicio y....

![14 starting service](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\14 starting service.png)

¡Estamos dentro! Ahora solo nos queda ir en busca de la flag del usuario root y ya habríamos terminado la room.

![15 root flag](C:\Users\ddgar\Escritorio\Github\Writeups\Vulnversity\15 root flag.png)
