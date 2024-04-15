# Manual de usuario

## Sobre Smartgrow

SmartGrow DataControl es un software avanzado diseñado específicamente para la captura y análisis de variables fisiológicas ambientales en cultivos interiores de Cannabis sativa. Este sistema facilita una integración eficiente entre dispositivos inalámbricos y bases de datos de código abierto, priorizando la eficacia energética mediante el empleo de protocolos de comunicación estandarizados como TCP/IP, MQTT, HTTP. La principal finalidad de SmartGrow DataControl es optimizar la recolección de datos esenciales para el estudio detallado de la fisiología del Cannabis sativa, incorporando el uso innovador de ́óptica y tecnologías del Internet de las Cosas (IoT). Este documento detalla principalmente, como el usuario final puede hacer uso del aplicativo, profundiza en aspectos tales como la instalación, los requerimientos de hardware y software y algunos casos de uso.  

## Repositorio de GitHub
[Repositorio de github](https://github.com/santiagoSuarez219/smartgrow)

## Instalacion 
Smartgrow fue pensado con un aplicativo web, por lo tanto, hay dos maneras en las que se puede instalar y utilizar la aplicación. La primera es de forma local, es decir, la aplicación corre en una red local y solo es asequible para los dispositivos conectados a esta red, la segunda es realizar un despliegue en un servidor web, para esto es necesario realizar el proceso de despliegue, el cual se explicará posteriormente. De esta manera, si el usuario final cuenta con la infraestructura de red necesaria podrá acceder a la aplicación mediante internet desde cualquier lugar. 

### Requerimientos

#### Sistema Operativo
Smartgrow es compatible con cualquier sistema operativo que soporte un navegador web. sin embargo, se recomienda utilizar un sistema operativo Ubuntu puesto que el proceso de instalación es más sencillo.

#### Requerimientos de Software
Para poder instalar y utilizar Smartgrow, el usuario debe contar con los siguientes requerimientos de software:

1. Instalar y configurar Docker Compose en su computadora local o en su servidor web. Para esto, lo más recomendable es buscar y seguir la guía oficial de instalación y configuración Docker Compose según el sistema operativo que el usuario tenga, ya que para la fecha en que el usuario este leyendo este documento, puede que la forma de instalación haya cambiado por lo tanto siempre será mejor seguir la documentación más actualizada. Aun así, en el Apéndice A de este documento encontrara una guía acerca de cómo instalar Docker Compose en su versión v2.24.3, (esta versión fue utilizada para las pruebas de funcionamiento de este proyecto) en un sistema operativo Ubuntu 22.04 LTS y en el Apéndice B encontrara como instalar la misma versión de Docker Compose en un sistema operativo Windows 11.
2. Instalar y configurar git (Ver apendice C).
3. Si el sistema operativo del usuario es Windows 11, adicionalmente, el usuario debe instalar y configurar WSL 2. En el Apéndice D de este documento encontrara una guía acerca de cómo instalar WSL 2 en un sistema operativo Windows 11.


#### Pasos para la intalacion en Windows 11
1. Abrir Docker Desktop y asegurarse de que el motor de Docker este corriendo, para esto el usuario puede ver en la parte inferior izquierda de la ventana de Docker Desktop un indicador verde con el texto Engine running.
2. Abrir WSL y clonar el repositorio de github en la carpeta de su preferencia. Para esto, el usuario puede utilizar el siguiente comando:

```bash
git clone git@github.com:santiagoSuarez219/smartgrow.git
```

Cabe aclarar que para ejecutar el comando anterior, el usuario debe tener instalado y configurado git en su computadora local o en su servidor web, si el usuario no tiene git instalado, puede seguir la guia de instalacion y configuracion de git en el apendice C de este documento.

3. Acceder al directorio donde se encuentra el proyecto y posteriormente acceder al directorio backend utilizando el siguiente comando: 

```bash
cd smartgrow/backend
```

4. Crear un archivo llamado .env y abrirlo con nano. Para esto, el usuario puede utilizar el siguiente comando:

```bash
touch .env
nano .env
```

5. Copiar y pegar el siguiente contenido en el archivo .env y luego guardar los cambios:

```
# Mongo
MONGO_DB = 'smartgrow'
MONGO_HOST = my-database
MONGO_PORT = 27017
MONGO_INITDB_ROOT_USERNAME = 'smartgrow'
MONGO_INITDB_ROOT_PASSWORD = 'smartgrow1234'
MONGO_CONNECTION = 'mongodb'

# MQTT
MQTT_HOST = emqx
MQTT_PORT = 1883
```

Para guardar los cambios y salir del editor de texto, el usuario debe presionar la tecla Ctrl + X, luego confirmar los cambios con Y y luego presionar Enter.

6. Volver al directorio raíz del proyecto utilizando el comando 

```bash
cd ..
```

7. Ejecutar el siguiente comando para desplegar la base de datos 

```bash
sudo docker compose up -d my-database
```

8. Ejecutar el siguiente comando para desplegar el servidor MQTT (Ver figura 3)

```bash
sudo docker compose up -d emqx
```

9. Ejecutar el siguiente comando para desplegar el backend de la aplicación

```bash
sudo docker compose up -d backend
```

El usuario podra verificar que todo ha salido correctamente abriendo el navegador y colocando la direccion URL: localhost:3000. Debe aparecer el texto *Hello World!*

10.  Acceder al directorio frontend utilizando el siguiente comando: 

```bash
cd frontend
```

11. Crear un archivo llamado .env y abrirlo con nano. Para esto, el usuario puede utilizar el siguiente comando:

```bash
touch .env
nano .env
```

12. Copiar y pegar el siguiente contenido en el archivo .env y luego guardar los cambios:

```
VITE_BROKER_MQTT = ws://emqx:8083/mqtt
VITE_BACKED_URL = http://backend:3000
```

Para guardar los cambios y salir del editor de texto, el usuario debe presionar la tecla Ctrl + X, luego confirmar los cambios con Y y luego presionar Enter.

13. Volver al directorio raíz del proyecto utilizando el comando 

```bash
cd ..
```

14. Ejecutar el siguiente comando para desplegar el frontend de la aplicación

```bash
sudo docker compose up -d frontend
```

El usuario podra verificar que todo ha salido correctamente abriendo el navegador y colocando la direccion URL: localhost:8080. Debe aparecer la pagina de inicio de la aplicación.

Luego, cuando el usuario quiera correr la aplicacion, debe abrir docker desktop, en el menu de la izquierda seleccionar la opcion de contenedores, luego, ubicar el contenedor *appsmartgrow* y en el item de *acciones* seleccionar la opcion de *start* o boton de *play* y de esta manera la aplicacion estara corriendo.

Tambien puede correr la aplicacion utilizando el siguiente comando en la terminal, ubicado en la carpeta raiz del proyecto:

```bash
sudo docker compose up -d
```

## Funcionalidades















## Stack Tecnologico



## Funcionamiento


## Apendice A: Instalacion de Docker Compose en Ubuntu 22.04 LTS
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04


## Apendice B: Instalacion de Docker Compose en Windows 11
https://docs.docker.com/desktop/install/windows-install/

## Apendice C: Instalacion y configuracion de git



## Apendice D: Instalacion de WSL


## Autores



