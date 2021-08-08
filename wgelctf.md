![Wgel_CTF](C:\Users\ddgar\Escritorio\Wgel_CTF.png)

Hola!

En este artículo vamos a enfrentarnos a una máquina de la plataforma Try Hack Me. Esta disponible de manera gratuita.

Descripción: "Can you exfiltrate the root flag?".

Dificultad: 1/10

--------------------------------------------------------

Lo primero que vamos a hacer, como de costumbre es conectarnos a la maquina y a nuestra VPN personal de THM.

Una vez establecida la conexión vamos a hacer un escaneo de los puertos abiertos en la IP de la víctima.

![1 port scan](C:\Users\ddgar\Escritorio\1 port scan.png)

Nos ha sacado dos puertos, así que vamos a hacer un escaneo de los servicio s que corren bajo esos puertos:

![2 service scan](C:\Users\ddgar\Escritorio\2 service scan.png)

Vemos que bajo el puerto 80 hay una página web. Vamos a ver más información sobre esta con la herramienta whatweb.

![3 web info](C:\Users\ddgar\Escritorio\3 web info.png)

Vemos que es una página default de el servidor Apache, pero inspeccionando su código fuente nos damos cuenta de que hay comentada una parte del código para que la revise una tal Jessie, por lo que ya tenemos un posible usuario.

Vamos a hacer un fuzzing de la web con la herramienta wfuzz, a ver que subdominios nos saca.

![4 fuzzing](C:\Users\ddgar\Escritorio\4 fuzzing.png)

Vemos que nos saca un subdominio .ssh por lo que vamos a ver si contiene algo interesante:

![5 ssh](C:\Users\ddgar\Escritorio\5 ssh.png)

Nos encontramos con una clave privada RSA, por lo que vamos a intentar establecer una conexión con la clave y el usuario Jessie mediante SSH:

![6 connecting via ssh](C:\Users\ddgar\Escritorio\6 connecting via ssh.png)

¡Estamos dentro! Vamos a buscar el user flag como de costumbre:

![7 searhing user flag](C:\Users\ddgar\Escritorio\7 searhing user flag.png)

Toca buscar vulnerabilidades en el equipo. Vamos a buscar comandos que podamos ejecutar como sudo siendo usuarios sin privilegios. Lo haremos con "sudo -l".

![8 searching sudo commands](C:\Users\ddgar\Escritorio\8 searching sudo commands.png)

Vemos que tenemos la posibilidad de ejecutar wget como sudo, por lo que intuyendo que la flag del usuario root tenga el mismo patrón que la del user vamos a intentar descargarnos en nuestra máquina la flag.

![9 root flag](C:\Users\ddgar\Escritorio\9 root flag.png)

¡Et voila! Ya tenemos la flag del root y como habéis visto no ha hecho falta una escalada de privilegios por lo que ha sido una tarea fácil.
