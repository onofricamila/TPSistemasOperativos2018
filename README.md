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
# echo "<html><p>Onofri Camila 13735/6 | Raimondi Sebastian</p></html>" >> index.html
# exit
$ cat index.html
```
No, no fue necesario reiniciar el container para ver los cambios, simplemente recargamos el navegador. 

**h) ¿Por qué es necesario ejecutar el comando ​ apache2ctl con la opción ​ -D FOREGROUND​ ?**

La opción -D FOREGROUND define una directiva especial de apache que lo que va a causar es que el proceso padre corra en primer plano y no se 'despegue' de la shell. Probando con la opción `-D BACKGROUND`, el container finalizaba su ejecución apenas comenzaba a ejecutarse.

### 4) En el siguiente punto se hará uso de un archivo ​Dockerfile para crear una imagen con similares características a la creada en el punto anterior:
a) Crear un archivo ​ Dockerfile​ que realice lo siguiente:
i)
Bajar la última versión de la imagen de Ubuntu desde un repositorio
ii)
Instalar el servidor Apacheiii)
iv)
Copiar el archivo ​ index.html (ubica dentro del directorio
/apachedata del Sistema Operativo base) al directorio
/var/www/html
Indicar al container el comando que se ejecutará cuando se inicie
(​ apache2ctl -D FOREGROUND​
). Utilice la forma ​ exec para definir el
comando.
Hint.: las instrucciones necesarias para el archivo Dockerfile son:
FROM, EXPOSE, RUN, COPY y CMD
b) Construir una imagen desde el ​ Dockerfile creado, guardarla localmente
con el nombre ​ apache2:so2018
c) Ejecutar el container con las opciones adecuadas y ver si es posible acceder
al contenido del archivo ​ index.html desde un navegador (u otra
herramienta que permita ver el contenido del archivo servido por Apache).

### 5) ¿Qué es un union-filesystem? ¿Cuál/es y cómo los utiliza Docker?

### 6) ¿Qué dirección IP tienen los containers? ¿De dónde la obtienen?

### 7) Describa brevemente los tipos de redes que se pueden utilizar en Docker. ¿Cuál piensa que Docker utiliza por defecto? Contenido del archivo ​ index.html​:
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


