# Sistemas Operativos 2018
## Trabajo para entregar

### Grupo:
* Onofri, Camila Ayelén 13735/6
* Raimondi, Sebastián

### 1) Instalar Docker CE en su sistema operativo. (Hint: seguir las instrucciones de la página de Docker. La instalación más simple es usando los respositorios: https://docs.docker.com/install/linux/docker-ce/debian/#install-using-the-repository​. Seleccionar las opciones “Jessie or newer” y “x86_64 / amd64”)


### 2) Usando las herramientas (comandos) provistas por Docker realizar las siguientes tareas:
**a) Instalar la versión más reciente de la imagen de Ubuntu ¿Cuál es el tamaño de la imagen obtenida? ¿Ya puede ser considerado un container?**

Primero buscamos las imágenes que contengan la palabra ubuntu con el comando search: `$ docker search ubuntu`.
Después de ejecutar el comando, se ve un listado de imágenes docker que coinciden con la búsqueda. Elegimos una de ellas (preferentemente la que tenga más estrellas), y la descargamos. Nosotros hicimos `$ docker pull ubuntu:latest`. Corriendo `docker images` vemos que la imagen instalada pesa 79.6MB. No, es simplemete una imagen Docker en nuestro repositorio local de imágenes Docker. Si queremos tener un contenedor a partir de ésta, debemos crearlo.

**b) Generar un container a partir de la imagen obtenida en el punto anterior que simplemente ejecute el comando ​`ls -l`.**

Hacemos lo pedido con el comando: `docker run --name ubuntuPunto2 -i -t ubuntu ls -l`. El container muestra el listado largo de contenido dentro del directorio actual en el (/) y luego finaliza su ejecución.

**c) ¿Qué sucede si ejecuta el comando ​ `docker [container] run ubuntu /bin/bash​` (la opción container podría no indicarse)?**

Al especificarse el argumento `/bin/bash` al comando `docker run`, se procedera a sobreescribir el programa por defecto especificado en el DockerFile de la imagen (definido para ser ejecutado cuando se use el comando run), por el parametrizado. En este caso justamente `/bin/bash` coincide con el programa definido en el DockerFile de la imagen ubuntu.

Por otra parte, al no proveerse las opciones `-i -t` al ejecutar el comando planteado, el container termina su ejecución justo luego de haber empezado a ejecutarse porque corrió la bash en modo no-interactivo y una vez que ésta finaliza el container no tiene nada que hacer.

i) Ejecutar el comando anterior pero con una terminal interactiva.

`docker [container] run -it ubuntu /bin/bash​`

ii) Crear un archivo con nombre “so2018” en el directorio “/”. Salir del container.

Dentro del container anterior que está ejecutando una terminal interactiva (es decir, que mantiene la sesión abierta para recibir input de la terminal), escribimos el comando `touch so2018` para crear el archivo en "/", y luego `exit` para salir del contenedor. Cabe mencionar termina la ejecución del contenedor.

**d) Ejecutar nuevamente un container con un bash en forma interactiva. ¿Existe el archivo creado en el punto anterior? ¿Por qué?**

NO, no existe el archivo recientemente creado porque se trata de un nuevo container a partir de la imagen ubuntu original descargada de Docker Hub, la cual no posee dicho archivo.

**e) Iniciar con el comando ​ `docker start -ia id_container` el contenedor en el cual se creó el archivo.**

i) ¿Cómo se obtiene el valor ​ id_container​ para ese comando?

Con `docker container ls -a` puedo ver la lista completa de todos los containers junto con datos adicionales, entre ellos su id. Busco el container en el que se creo el archivo so2018 (se le asignó un nombre random porque no especificamos ninguno, y si no lo recordamos, sabemos que es el anteúltimo que creamos), y así puedo ver su id.

ii) ¿Qué sucede? ¿Puede ver el archivo creado?

Sí, se puede ver el archivo creado.

**f) ¿Cuántos containers se están ejecutando actualmente? ¿En qué estado se encuentran cada uno de los containers ejecutados hasta el momento?**

Corremos el comando `docker container ls` y notamos actualmente no se está ejecutando ningún container. El estado de todos es 'Excited', se puede ver con el comando `docker container ls -a`

**g) Eliminar todos los contenedor creados hasta el momento.**

Para eliminar todos los contenedores creados hasta el momento debemos correr el comando `docker container ls -a` para obtener sus respectivos ids, y luego correr `docker container rm ` enviandole como parametro los ids de los containers que queremos borrar. Ejemplo: `docker container rm id_1 id_2 id_3`. Como nota vale aclarar que si queremos borrar un container que se encuentra en ejecución, debemos forzar su eliminación agregando la opción `-f` al comando `docker container rm`.

### 3) En los siguientes puntos se generará una nueva imagen a partir de un container:
**a) Generar un container a partir de la imagen de Ubuntu en modo interactivo y que ejecute una consola.**

`docker container run --name ubuntuPunto3 -ti ubuntu`

**b) Instalar, usando los siguientes comandos, el servidor Apache y salir del container.**
```sh
apt update -qq
apt install apache2 -qqy
```
Ejecutamos adicionalmente el comando `exit` para salir del container.

**c) Generar una nueva imagen a partir del container anterior. ¿Con qué nombre se genera?**

Creamos la nueva imagen a partir de dicho container con el comando `docker commit 82fd97cd1644`, siendo ese el id del container. Como no indicamos el nombre, no tiene ninguno. 

**d) Cambiar el nombre de la imagen de manera que en la columna Repository aparezca apache2 y en el tag v1:**

Con el comando `docker tag f09 apache2:v1` logramos el resultado esperado, siendo f09 los primeros 3 caracteres del id de la imagen. Vemos se realizó el cambio corriendo `docker images`.

**e) Ahora ejecutar un container con un servidor HTTP. Para esto crear un directorio ​ /apachedata en el directorio raíz del Sistema Operativo base y dentro de él un archivo con el contenido HTML que se puede ver al final de
este enunciado de práctica.**

```
$ cd /
$ sudo bash
# mkdir apachedata
# cd apachedata
# echo "<html><head><meta charset=utf-8><title>httpd en un container</title></head><body><h1>Trabajo Práctico SO 2018</h1></body></html>" > index.html
# exit
```

**f) Crear un contenedor que ejecute el servidor Apache y que sea capaz de mostrar el contenido del archivo. Para esto tener en cuenta lo siguiente:**

i) Se debe montar el directorio ​ /apachedata en el directorio raíz (o Document Root ​ ) de Apache: ​ /var/www/html

ii) Se debe exponer el puerto ​ 80 del contenedor en el ​ 8080 del Sistema Operativo base.

iii) Para ejecutar el servidor Apache
usar el comando: /usr/sbin/apache2ctl -D FOREGROUND 

Hice todo lo pedido en este punto con el siguiente comando:
`docker container run --publish 8080:80 --name webhost -v /apachedata:/var/www/html apache2:v1 usr/sbin/apache2ctl -D FOREGROUND`

Comprobé se veia el archivo desde el navegador, accediendo a localhost:8080.

**g) Modificar el archivo ​ index.html agregándole una línea con el nombre y nro. de alumno de cada integrante del grupo. ¿Es necesario reiniciar el container para ver el nuevo archivo?**

Modificamos el archivo index.html y vimos los cambios estando parados en el directorio /apachedata en el SO base, y de la siguinte manera:
```
$ sudo bash
# echo "<html><head><meta charset=utf-8><title>httpd en un container</title></head><body><h1>Trabajo Práctico SO 2018</h1><p>Onofri Camila 13735/6 | Raimondi Sebastian</p></body></html>" > index.html
# exit
$ cat index.html
```
No, no fue necesario reiniciar el container para ver los cambios, simplemente recargamos el navegador. 

**h) ¿Por qué es necesario ejecutar el comando ​ apache2ctl con la opción ​ -D FOREGROUND​ ?**

La opción -D FOREGROUND define una directiva especial de apache que lo que va a causar es que el proceso padre corra en primer plano y no se 'despegue' de la shell. Probando con la opción `-D BACKGROUND`, el container finalizaba su ejecución apenas comenzaba a ejecutarse.

### 4) En el siguiente punto se hará uso de un archivo ​Dockerfile para crear una imagen con similares características a la creada en el punto anterior:

a) Crear un archivo ​ Dockerfile​ que realice lo siguiente:

* i) Bajar la última versión de la imagen de Ubuntu desde un repositorio

* ii) Instalar el servidor Apacheiii)

* iv) Copiar el archivo ​ index.html (ubica dentro del directorio /apachedata del Sistema Operativo base) al directorio /var/www/html

* Indicar al container el comando que se ejecutará cuando se inicie (​ apache2ctl -D FOREGROUND​ ). Utilice la forma ​ exec para definir el comando.

* Hint.: las instrucciones necesarias para el archivo Dockerfile son: FROM, EXPOSE, RUN, COPY y CMD

```dockerfile
FROM ubuntu:latest
EXPOSE 80
RUN apt update -qq && apt install apache2 -qqy
COPY index.html /var/www/html
CMD ["apache2ctl","-D","FOREGROUND"]
```

b) Construir una imagen desde el ​ Dockerfile creado, guardarla localmente
con el nombre ​ apache2:so2018

Posicionado en el directorio donde estan ubicados Dockerfile y index.html ejecutamos: `docker build -t apache2:so2018 .`

Si no estuvieramos parados en el directorio que contiene el Dockerfile, podemos indicarle el path deseado con la opcion -f

c) Ejecutar el container con las opciones adecuadas y ver si es posible acceder al contenido del archivo ​ index.html desde un navegador (u otra herramienta que permita ver el contenido del archivo servido por Apache .

Ejecutamos el comando: `docker run -p 8080:80 apache2:so2018`

### 5) ¿Qué es un union-filesystem? ¿Cuál/es y cómo los utiliza Docker?

  Union filesystem es un servicio para sistemas de archivos linux que permite montar un filesystem formado por la unión de otros filesystems. Permite que archivos y directorios de diferentes filesystems, conocidos como ramas, sean transparentemente superpuestos, formando un único filesystem. Los contenidos de los directorios que tienen el mismo path entre las ramas superpuestas serán vistos como si estuvieran en el mismo directorio en el nuevo filesystem virtual.

  Las diferentes ramas pueden ser filesystems read-only o read-write, de forma que las escrituras al filesystem virtual combinado pueden realizarse sobre un filesystem real. Gracias a esto es posible hacer que un filesystem parezca modificable, sin permitir que las escrituras modifiquen el sistema de archivos, también conocido como copy-on-write.

  Docker usa union filesystems para crear las capas de una imagen Docker. A medida que se realizan acciones sobre la imagen base se crean y documentan nuevas capas de modo que cada capa describe cómo recrear una acción en su totalidad. Esta estrategia permite la existencia de las imágenes livianas en Docker, ya que solo las modificaciones de las capas deben ser propagadas. Esto le permite a docker no duplicar los contenidos de una imagen cada vez que se inicia un nuevo contenedor. La capa que genera un container durante su ejecución es eliminada cuando el container se elimina, pero se pueden guardar en una nueva imagen usando el comando `docker commit <container> <name:tag>`.

### 6) ¿Qué dirección IP tienen los containers? ¿De dónde la obtienen?

Por defecto, al contenedor se le asigna una dirección IP para cada red docker a la que se conecta. Cada red tiene asignada un pool de ips, del cual se toma una y se le asigna al contenedor. Cada red tiene también una máscara de subred y gateway default.

Se le puede asignar las direcciones ip a un container usando los flags `--ip` o `--ip6` al momento de asignarle una red con el flag `--network` durante su inicio o cuando se conecta el container a una nueva red con el comando `docker network connect`.

Para obtener la ip de un container se puede ejecutar el comando `docker inspect <contianer>` y la encontraremos dentro de `NetworkSettings` junto con mas informacion de la red.
En el caso del punto 4, la dirección que fue asignada por defecto al container creado fue `172.17.0.2`

### 7) Describa brevemente los tipos de redes que se pueden utilizar en Docker. ¿Cuál piensa que Docker utiliza por defecto? Contenido del archivo ​ index.html​:


Los tipos de red instalados por defecto son:

- `bridge`:
    Es el tipo de red por defecto. Una red bridge le permite a distintos containers conectados a comunicarse entre si, mientras están aisladas de los containers que no están conectados al `bridge`. Los bridge solo aplican a containers corriendo en el mismo host. Los contenedores no tendrán acceso a ninguna ruta externa.

- `host`: 
    No aíslan al container del host, por lo tanto si un container que implementa una red `host` y esta bindeado al puerto 80 podrá ser accedido en el puerto 80 de la ip del host. Es más eficiente que bridge ya que usa el stack de red nativo del host, mientras que usando bridge se debe atravesar una capa de virtualización.

- `overlay`: 
    Conectan múltiples docker daemons, haciendo posibles que los servicios swarm se comuniquen entre sí. También pueden ser usados para comunicar un servicio swarm con un container independiente o entre dos containers independientes en diferentes docker daemons, eliminando la necesidad de ruteo a nivel del sistema operativo entre estos containers.

- `macvlan`: 
    permiten asignar una dirección MAC a un container, permitiéndole aparecer como un dispositivo físico en una red. El Docker daemon rutea tráfico a los containers por su dirección MAC. Muchas veces es usado para trabajar con aplicaciones legacy.

- `none`: 
    Deshabilita toda la funcionalidad de red para un container. Generalmente usado en conjunto con un driver de red modificado. `none` no está disponible para servicios swarm.

Adicionalmente un usuario puede crear su propia red usando un driver de red de Docker o un plugin externo.

```html
<html>
    <head>
        <meta charset=utf-8>
        <title>httpd en un container!</title>
    </head>
    <body>
        <h1>Trabajo Práctico SO 2018</h1>
    </body>
</html>
```


