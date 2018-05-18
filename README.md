# Sistemas Operativos 2018
## Trabajo para entregar

### Grupo:
* Onofri, Camila Ayelén 13735/6
* Raimondi, Sebastián

### 1) Instalar Docker CE en su sistema operativo. (Hint: seguir las instrucciones de la página de Docker. La instalación más simple es usando los respositorios: https://docs.docker.com/install/linux/docker-ce/debian/#install-using-the-repository​. Seleccionar las opciones “Jessie or newer” y “x86_64 / amd64”)


### 2) Usando las herramientas (comandos) provistas por Docker realizar las siguientes tareas:
**a) Instalar la versión más reciente de la imagen de Ubuntu ¿Cuál es el tamaño de la imagen obtenida? ¿Ya puede ser considerado un container?**

b) Generar un container a partir de la imagen obtenida en el punto anterior que
simplemente ejecute el comando ​ ls -l.

c) ¿Qué sucede si ejecuta el comando ​ docker [container] run ubuntu /bin/bash​ ( ​ la opción container podría no indicarse)?
i) Ejecutar el comando anterior pero con una terminal interactiva.
ii) Crear un archivo con nombre “so2018” en el directorio “/”. Salir del container.

d) Ejecutar nuevamente un container con un bash en forma interactiva. ¿Existe el archivo creado en el punto anterior? ¿Por qué?e) Iniciar con el comando ​ docker start -ia id_container el contenedor en el cual se creó el archivo.
i) ¿Cómo se obtiene el valor ​ id_container​ para ese comando?
ii) ¿Qué sucede? ¿Puede ver el archivo creado?

f) ¿Cuántos containers se están ejecutando actualmente? ¿En qué estado se encuentran cada uno de los containers ejecutados hasta el momento?

g) Eliminar todos los contenedor creados hasta el momento.

### 3) En los siguientes puntos se generará una nueva imagen a partir de un container:
a) Generar un container a partir de la imagen de Ubuntu en modo interactivo y
que ejecute una consola.
b) Instalar, usando los siguientes comandos, el servidor Apache y salir del
container.
apt update -qq
apt install apache2 -qqy
c) Generar una nueva imagen a partir del container anterior. ¿Con qué nombre
se genera?
d) Cambiar el nombre de la imagen de manera que en la columna Repository
aparezca apache2 y en el tag v1:
e) Ahora ejecutar un container con un servidor HTTP. Para esto crear un
directorio ​ /apachedata en el directorio raíz del Sistema Operativo base y
dentro de él un archivo con el contenido HTML que se puede ver al final de
este enunciado de práctica.
f) Crear un contenedor que ejecute el servidor Apache y que sea capaz de
mostrar el contenido del archivo. Para esto tener en cuenta lo siguiente:
i)
Se debe montar el directorio ​ /apachedata en el directorio raíz (o
Document Root ​ ) de Apache: ​ /var/www/html
ii)
Se debe exponer el puerto ​ 80 del contenedor en el ​ 8080 del Sistema
Operativo base.
iii)
Para
ejecutar
el
servidor
Apache
usar
el comando:
/usr/sbin/apache2ctl -D FOREGROUND
g) Modificar el archivo ​ index.html agregándole una línea con el nombre y
nro. de alumno de cada integrante del grupo. ¿Es necesario reiniciar el
container para ver el nuevo archivo?
h) ¿Por qué es necesario ejecutar el comando ​ apache2ctl con la opción ​ -D
FOREGROUND​
?

### 4) En el siguiente punto se hará uso de un archivo ​ Dockerfile para crear una imagen con similares características a la creada en el punto anterior:
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


