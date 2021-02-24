![portada](Imagenes/portada.png)

# Enunciado #

## Requerimiento 1 ##

![enunciado1](Imagenes/Enunciado1.png)

**Primero:** Tenemos que tener instalado Apache Tomcat. Para ello, abrimos nuestra terminal de Ubuntu. Tenemos que comprobar si tenemos instalado java, ya que sin ello no podremos acceder al servidor. Podemos comprobarlo con el siguiente comando:

	- java -version

![comando1](Imagenes/comando1.png)

Como vemos, no tenemos instalado java. Para instalarlo, utilizamos el comando que nos sugiere Ubuntu:

	- sudo apt install default-jre

![comando2](Imagenes/comando2.png)

Una vez lo hayamos instalado, podemos volver a ejecutar el primer comando *java -version* para comprobar que se ha instalado correctamente y además, ver qué versión de java tenemos. 

![comando3](Imagenes/comando3.png)

**Segundo:** Ahora que ya tenemos java instalado, vamos a proceder a instalar nuestro servidor Tomcat. Para ello, ejecutamos el siguiente comando:

	- sudo apt install tomcat9

![comando4](Imagenes/comando4.png)

**Nota:** Accedemos en nuestro navegador web y escribimos en la barra de dirección de búsqueda "localhost:8080". Nos aparecerá la página de tomcat que nos indicará que hemos instalado correctamente el servidor. La misma página nos indica también que al instalar tomcat9, necesitaremos tener instalada su documentación correspondiente tales como "docs", "examples" y "admin". Para poder instalar la documentación simplemente ejecutamos en nuestro terminal los siguientes comandos:

	- sudo apt install tomcat9-docs
	- sudo apt install tomcat9-examples
	- sudo apt install tomcat9-admin

Una vez hayamos descargado la documentación, comprobamos el estado del tomcat para saber que está ejecutándose y lo hacemos mediante el comando 

	- systemctl status tomcat9
	- sudo ufw allow 8080/tcp (para que escuche por el puerto 80)

![comando5](Imagenes/comando5.png)


**Tercero:** Ahora que ya tenemos tomcat instalado, hemos descargado su documentación y hemos comprobado que se está ejecutando, vamos a acceder a dichos documentos para modificarlos y poder acceder al servidor desde nuestro localhost.  

El primer documento al que vamos a acceder, es el documento de los usuarios. Ejecutamos el comando 

	- sudo nano /etc/tomcat9/tomcat-users.xml

Nos aparecerá el siguiente documento:
![comando6](Imagenes/comando6.png)

Nos fijamos que en el documento aparece comentado los usuarios de tomcat. Esta parte del documento habrá que modificarla.

Lo que hacemos es descomentar los usuarios que vienen por defecto y en ellos, modificamos los roles y los usuarios según nuestra necesidad. Para nosotros, nos interesa que haya 2 roles por ahora. 

El primer rol, será el de “manager-gui” y el segundo rol, el de “admin-gui” y como usuarios, vamos a dejar uno que será “admin” con password “admin” y role “manager-gui,admin-gui”. Insistiendo en que este usuario será el primero e iremos modificando más adelante si es preciso. 

![comando7](Imagenes/comando7.png)

Ahora que ya tenemos nuestros usuarios modificados, vamos a parar y arrancar de nuevo el servidor tomcat. Para ello, ejecutamos los siguientes comandos:

	- service tomcat9 stop (nos pedirá contraseña de nuestra máquina)
	- service tomcat9 start (nos volverá a solicitar la contraseña)

Una vez reiniciado el servidor, vamo a acceder a los siguientes documentos. Estos documentos serán por un lado, el *manager* y por otro, el *host-manager* con los siguientes comandos:

	- sudo nano /etc/share/tomcat9-admin/manager/META-INF/context.xml
	- sudo nano /etc/share/tomcat9-admin/host-manager/META-INF/context.xml

Ambos documentos deberán aparecer así:

![comando8](Imagenes/comando8.png)

Cuando hayamos comprobado que ambos documentos aparecen iguales, salimos del *context.xml* y reiniciamos el tomcat con el comando

	- sudo systemctl restart tomcat9

Y una vez hayamos reiniciado el servidor, nos dirigimos nuevamente a nuestro navegador y escribimos en la dirección de búsqueda “localhost:8080/manager/html” y nos deberá aparecer la página principal del manager de tomcat.

![comando9](Imagenes/comando9.png)

**Cuarto:** Ya tenemos todo a punto para manejar tomcat. Ahora, lo que vamos a hacer es desplegar el archivo war que facilita el profesor en su proyecto git. Lo descargamos en nuestra máquina virtual y en el apartado de tomcat donde dice "desplegar archivo war" lo subimos. 

![descargaWar](Imagenes/descargaWar.png)

Una vez hemos cargado el archivo, en el apartado "Aplicaciones" de nuestro tomcat, aparecerá. Pulsamos en el y nos lleva a la página de bienvenida. Si intentamos acceder a la Base de Datos, nos aparecerá en blanco porque aún no la hemos cargado. 

![cargaWar](Imagenes/cargaWar.png)



**Quinto:** Llegados a éste punto, el siguiente paso será instalar nuestra base de datos. Para ello, abrimos nuestra terminal de Ubuntu. Antes de nada, **se recomienda** actualizar nuestra terminal por si hubiese algún fichero o algo sin actualizar, pero esto simplemente como consejo. Si se quiere actualizar nuestra terminal, se usará el comando *"sudo apt-get update"*  y si aparecen ficheros listos para actualizar, ejecutaremos el comando *"sudo apt-get upgrade"*. En nuestro caso, decidimos que sí lo hacemos y la actualizamos. 

Dicho esto, ahora sí, vamos a instalar nuestra BBDD. El equipo hemos deliberado qué opción puede ser mejor para trabajar. Hemos decidido escoger la BBDD de MySQL ya que nos permite trabajar desde nuestra terminal, directamente por comandos y ahorrarnos el descargar por interfaz de usuario otras, como puedan ser WorkBrench, Oracle, etc.

Para la instalación de MySQL, ejecutamos el siguiente comando:

	- sudo apt install mysql-server

![comando10](Imagenes/comando10.png)

Una vez hemos instalado MySQL, ahora vamos a dar una contraseña. Ejecutamos el comando:

	- sudo mysql _secure_installation

Con este último comando, podemos acceder con nuestra contraseña del S.O. o cambiarla por otra que consideres oportuna. 

El comando anterior nos irá preguntando modalidades como ¿desea utilizar su contraseña habitual?¿quiere eliminar el usuario anónimo? etc.

![comando11](Imagenes/comando11.png)

Una vez hemos configurado las opciones que se muestran en la imagen anterior, ahora iniciamos MySQL con el comando

	- sudo mysql -u root -p

![comando12](Imagenes/comando12.png)

Cuando estamos dentro de mysql, lo siguiente que vamos a hacer es crear la tabla con los datos que se adjuntan en el enunciado. 

Para crear la tabla usamos el comando 

	- CREATE DATABAS nombreBaseDatos;
	- show databases; (Para comprobar que la hemos creado correctamente)

![comando13](Imagenes/comando13.png)

Cuando ya hemos creado la BBDD, el siguiente paso es crear la tabla. Vamos a seguir el patrón del enunciado y creamos la tabla personas con los datos del DNI como primaryKey, el nombre y apellidos. **Se ejecuta todo el comando en la misma línea** el patrón que tenemos que seguir es **CREATE TABLE nombreTabla (campo tipoDato(caracteres) primaryKey)** y todos los datos que queramos añadir separados entre comas.

![comando14](Imagenes/comando14.png) 


**Sexto:** Ahora tenemos que cambiar el recurso que se indica en el ejercicio para ello tenemos que acceder al **context** de tomcat. Utilizamos el comando:

	- sudo nano /var/lib/tomcat9/conf/contex.xml

Una vez hemos accedido, procedemos a modificar el contenido como se indica en los apuntes

![comando15](Imagenes/comando15.png)

**Séptimo:** Cuando ya tengamos modificado el archivo context.xml, lo que hacemos a continuación, es descargar el driver del conector. Para ello, nos dirigimos a la página facilitada por el profesor "https://mvnrepository.com/artifact/mysql/mysql-connector-java" y lo descargamos  