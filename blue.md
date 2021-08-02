![Blue](https://imgur.com/PqS5cl5.png)

Hola!

En este artículo vamos a enfrentarnos a una máquina de la plataforma Try Hack Me. Esta disponible de manera gratuita.

Descripción: "Deploy & hack into a Windows machine, leveraging common misconfigurations issues.".

Dificultad: 1/10

--------------------------------------------------------

Lo primero que vamos a hacer, como de costumbre es conectarnos a la maquina y a nuestra VPN personal de THM.

Una vez establecida la conexión vamos a hacer un escaneo de los puertos abiertos en la IP de la víctima.

![1 port scan](https://imgur.com/twFUIlh.png)

Vemos que hay 4 puertos abiertos y entre ellos tenemos el 445 por lo que posiblemente haya un protocolo SMB por detrás de ese puerto. Para obtener mas información de los puertos abiertos, vamos a hacer un reconocimiento de vulnerabilidades para los puertos que hemos encontrado:

<div style="text-align:center"><img src="https://imgur.com/ekuLvi9.png" /></div>

y encontramos que bajo el puerto 445 hay un script vulnerable, mas específicamente el ms17-010. Vamos a echar un vistazo en "exploit.db" para saber más sobre el script:

![2 checking vuln scripts for all services](https://imgur.com/aompKE6.png)

Nos encontramos que el script ms17-010 se llama EternalBlue, sabiendo esto y que es vulnerable en la máquina de nuestra víctima, vamos a iniciar metasploit y a buscar el exploit:

![3 searching eternal blue exploit](https://imgur.com/UbDCGTG.png)

Lo seleccionamos y nos disponemos a configurarlo.

![4 exploit config](https://imgur.com/UbDCGTG.png)

Una vez hemos configurado la IP de la víctima y la IP del atacante vamos a intentar ejecutar el exploit:

![5 exploited](https://imgur.com/Cg2MBer.png)

¡Ha habido suerte! (En el caso de que no os funcionara cancelar el proceso de explotación y volverlo a ejecutar).

Una vez dentro del sistema vamos a ejecutar el comando "hashdump" para conseguir las credenciales encriptadas de los usuarios:

![6 hashdump](https://imgur.com/IUtOqwH.png)

Vemos que tenemos un usuario "Jon" que no es un usuario predefinido, por lo que vamos a copiar su has y con la herramienta "hashcat" procedemos a hacer un ataque de fuerza bruta para desencriptarla.

Una vez hecho esto volvemos a conectarnos a la sesión de metasploit. Nos dirigiremos a la raíz y buscaremos archivos de texto que contengan la palabra "flag".

![7 searching the flags](https://imgur.com/TK6N9Rj.png)

Nos muestra 3 resultados, por lo que para guardar la información, nos descargamos los archivos en nuestro entorno de trabajo y procedemos a leerlas.

![8 flags](https://imgur.com/jkWJLpn.png)

¡Y listo! Hemos obtenidos todas las flags, como veis es una máquina muy sencillita pero muy recomendable para iniciarse en el uso de metasploit.
