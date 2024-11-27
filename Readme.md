## Alcance del proyecto
Este proyecto tiene como alcance el realizar tareas básicas con contenedores empleando comandos de docker.
No se incluye comandos de docker compose ya que para esto se realizará otro laboratorio.

Cada uno de los directorios (Lab-1, Lab-2, etc.) Contiene una práctica o experimento:

## Laboratorio 1: 

## Laboratorio 2:  

## Laboratorio 3:

## Laboratorio 4:

## Estructura del proyecto:
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
    docker run -d --name my_nginx -p 8080:80 nginx

Este comando levantará un contenedor con el servidor de NGINX expuesto en el puerto 8080
Verificar que esté funcionando: Acceder a http://localhost:8080 en su navegador.
Este se ejecuta el contenedor, el cual si no existe y no existe la imagen de NGINX en el equipo de forma local, el sistema genera la descarga de dicha imagen y la almacena en la biblioteca local de imágenes creando a partir de esta a su vez un contenedor y ejecutándolo bajo el nombre my_nginx en el puerto 8080.


### LABORATORIO 2: Crear una imagen personalizada:
En este laboratorio, se crea una imagen personalizada de Docker usando Dockerfile.
* Objetivo: Crear una imagen que instale curl en una imagen base de ubuntu.

#### Pasos:

1. Crear un archivo denominado DOckerfile en el directorio lab-2/create_custom_image/ con el siguiente contenido:


FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl
CMD ["curl", "--version"]



