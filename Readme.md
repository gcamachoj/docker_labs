## Alcance del proyecto
Este proyecto tiene como alcance el realizar tareas básicas con contenedores empleando comandos de docker.
No se incluye comandos de docker compose ya que para esto se realizará otro laboratorio.

Cada uno de los directorios (Lab-1, Lab-2, etc.) Contiene una práctica o experimento:

### Laboratorio 1: Crear y ejecutar un contenedo nginx en un solo comando. Validar su funcionamiento.

### Laboratorio 2:  Crear una imagen personalizada con un Dockerfile con comandos y ejecutar validacion del contenedor desde cmd. El contenedor se debe crear y eliminar después de la prueba automáticamente.

### Laboratorio 3: 

### Laboratorio 4:

### Estructura del proyecto:
docker-laboratory/ 

│ 

├── Dockerfile 

├── README.md 

├── lab-1/ 

│   └── run_nginx/ 

├── lab-2/ 

│   └── create_custom_image/ 

├── lab-3/ 

│   └── mount_volume/ 

├── lab-4/ 

│   └── manage_networks/ 

└── scripts/ 

    └── docker_commands.sh 

# Introducción

## Conceptos previos:
### Qué es docker:
Docker es un sistema operativo para contenedores. De manera similar a cómo una máquina virtual virtualiza (elimina la necesidad de administrar directamente) el hardware del servidor, los contenedores virtualizan el sistema operativo de un servidor. Docker se instala en cada servidor y proporciona comandos simples que puede usar para crear, iniciar o detener contenedores.

### Que es un contenedor de docker?
Un contenedor de Docker es un entorno de ejecución que permite ejecutar una aplicación sin utilizar las dependencias de la máquina host. Los contenedores de Docker son independientes, ligeros y ejecutables, e incluyen todo lo necesario para ejecutar una aplicación, como: Bibliotecas, Herramientas del sistema, Código, Tiempo de ejecución. 

## Que es una Imagen de docker?
Una imagen de Docker es una plantilla de solo lectura que contiene las instrucciones para crear un contenedor. Es una instantánea de las bibliotecas, dependencias y archivos que necesita un contenedor para ejecutarse. 


## Entorno:
Este laboratorio fue creado en un entorno local en una máquina con windows, sin embargo se puede ejecutar desarrollar desde ambientes linux con los comandos acordes para la inicialización de docker.

## Prerequisitos del laboratorio
1. Instalar docker
2. Crear la variable de entorno de docker para inicializar docker desde cmd:
    * Ir a la Configuracion avanzada del sistema, sección variables de entorno.
    * En variables del sistema, seleccionar la variable "Path", y adicionar una linea con la ruta de la carpeta donde está instalado docker, para este caso era: 
    C:\Program Files\Docker\Docker\
    ej: 
3. inicializar docker.
    * Se puede inicializar docker desde el acceso directo en el menú inicio, o también desde la línea de comandos CMD, ejecutando el comando  start "" "Docker Desktop.exe"


## Instrucciones

### Laboratorio 1: Crear el contenedor de NGINX:
Ejecutar el siguiente comando bash:
    
    __docker run -d --name my_nginx -p 8080:80 nginx__

Este comando levantará un contenedor con el servidor de NGINX expuesto en el puerto 8080
Verificar que esté funcionando: Acceder a http://localhost:8080 en su navegador.
Este se ejecuta el contenedor, el cual si no existe y no existe la imagen de NGINX en el equipo de forma local, el sistema genera la descarga de dicha imagen y la almacena en la biblioteca local de imágenes creando a partir de esta a su vez un contenedor y ejecutándolo bajo el nombre my_nginx en el puerto 8080.

* Para validar los contenedores activos tambien puede pulsar el en bash:
    **docker ps -a**
Donde:
* -d: Ejecuta el contenedor en segundo plano liberando la terminal. De lo contrario, la terminal sería tomada por la ejecución del contenedor.
* 

### LABORATORIO 2: Crear una imagen personalizada:
En este laboratorio, se crea una imagen personalizada de Docker usando Dockerfile.
* Objetivo: Crear una imagen que instale curl en una imagen base de ubuntu.

#### Pasos:

1. Crear un archivo denominado DOckerfile en el directorio lab-2/create_custom_image/ con el siguiente contenido:


FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl
CMD ["curl", "--version"]

2. Ejecutar comando para construir la imagen:
docker build -t my-ubuntu-curl .


3. Verificar la imagen: Ejecutar el contenedor para verificar que curl estpé instalado:
docker run --rm my-ubuntu-cyul

Se puede validar desde "docker desktop" que el contenedor se crea y se ejecutan los comandos internos del dockerfile, posteriormente se elimina automáticamente.

### LABORATORIO 3: Implementar un volúmen en Docker:

* Crear un contenedor:
**<docker run -d -v my_data:/data --name my_volume_container alpine sleep infinity>**

* -d: ejecuta el contenedor en segundo plano, el contenedo rse ejecuta de forma independiente, permitiendo seguir usando la terminal activa para otros comandos. Sin "-d", el contenedor permanecería vinculado a la terminal, mostrando su salida estándar en tiempo real. 
* Alpine_ es la imagen que se va usar.
* -v my_data:/data: Crea o utiliza un volumen llamado my_data montado en la ruta /data dentro del contenedor
* sleep infinity: Mantiene el contenedor en ejecución indefinidamente

* Valide con el comando <docker ps -a> la activación del contenedor.
* Liste los vlúmenes de almacenamiento de docker en docker desktop en la sección Volumenes. Confirme la existencia del volumen my_data

* Acceda al contenedor y cree un archivo dentro del volumen nuevo:
**docker exec -it my_volume_container sh**

Esto abrirá una shell interacrtiva dentro del contenedor.

* Crea un archivo dentro del volumen:  
**echo "Este es mi archivo de prueba" > /data/archivo_prueba.txt**

* Verifique el contenido del archivo (también lo puede validar desde el volumen en docker desktop):
**cat /data/archivo_prueba.txt** 

* Detener el contenedor:
**<docker stop my_volume_container>**

* Confirmar que el archivo persiste aún después de detener o eliminar el contenedor. Incluso se puede utilizar en otros contenedores.


## LABORATORIO 4: ADMINISTRACIÓN DE REDES EN DOCKER
En este laboratorio, aprenderás a crear y conectar contenedores a redes personalizadas.
Ejecutar:
**<docker network create my_network>**
Este comando crea una red denominada my_network.

* Crear dos contenedores y conectarlos a la red:

**<docker run -d --name container1 --network my_network ubuntu sleep 1000>**
**<docker run -d --name container2 --network my_network ubuntu sleep 1000>**

* Verificar la conectividad: Ejecuta docker exec para entrar a container1 y probar el ping a container2:

* Ejecutar:
**<docker exec -it container1 bash>** 
Este comando ingresa al bash del contenedor1

* Luego instala ping dentro del contenedor. Si es una imagen basada en Ubuntu/Debian, usa:
**<apt update && apt install -y iputils-ping>**
* Para imágenes basadas en alpine Linux usar:
**<apk add iputils>**
* probar ping con el comando:
**<ping google.com>** 
Si funciona pulsar Control+C para detener el proceso (ping).


* Ejecutar:
**<ping container2>**

Esto permitirá validar la conexión entre los contenedores.

* Para salir del modo bash, ejecute el comando <exit>.