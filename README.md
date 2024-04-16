# Manual de usuario

## Sobre Smartgrow

SmartGrow DataControl es un software avanzado diseñado específicamente para la captura y análisis de variables fisiológicas ambientales en cultivos interiores de Cannabis sativa. Este sistema facilita una integración eficiente entre dispositivos inalámbricos y bases de datos de código abierto, priorizando la eficacia energética mediante el empleo de protocolos de comunicación estandarizados como TCP/IP, MQTT, HTTP. La principal finalidad de SmartGrow DataControl es optimizar la recolección de datos esenciales para el estudio detallado de la fisiología del Cannabis sativa, incorporando el uso innovador de ́óptica y tecnologías del Internet de las Cosas (IoT). Este documento detalla principalmente, como el usuario final puede hacer uso del aplicativo, profundiza en aspectos tales como la instalación, los requerimientos de hardware y software y las funcionalidades del sistema.  

## Repositorio de GitHub
[Repositorio de github](https://github.com/santiagoSuarez219/smartgrow)

## Instalacion 
Smartgrow fue pensado con un aplicativo web, por lo tanto, hay dos maneras en las que se puede instalar y utilizar la aplicación. La primera es de forma local, es decir, la aplicación corre en una red local y solo es asequible para los dispositivos conectados a esta red, la segunda es realizar un despliegue en un servidor web, para esto es necesario realizar el proceso de despliegue, el cual se explicará posteriormente. De esta manera, si el usuario final cuenta con la infraestructura de red necesaria podrá acceder a la aplicación mediante internet desde cualquier lugar. 

### Requerimientos

#### Sistema Operativo
Smartgrow es compatible con cualquier sistema operativo que soporte un navegador web. sin embargo, se recomienda utilizar un sistema operativo Ubuntu puesto que el proceso de instalación es más sencillo.

#### Requerimientos de Software
Para poder instalar y utilizar Smartgrow, el usuario debe contar con los siguientes requerimientos de software:

1. Instalar y configurar Docker Compose en su computadora local o en su servidor web. Para esto, lo más recomendable es buscar y seguir la guía oficial de instalación y configuración Docker Compose según el sistema operativo que el usuario tenga, ya que para la fecha en que el usuario este leyendo este documento, puede que la forma de instalación haya cambiado por lo tanto siempre será mejor seguir la documentación más actualizada. Aun así, en el Apéndice A de este documento encontrara una guía acerca de cómo instalar Docker Compose en su versión v2.3.3, (esta versión fue utilizada para las pruebas de funcionamiento de este proyecto) en un sistema operativo Ubuntu 22.04 LTS y en el Apéndice B encontrara como instalar la misma versión de Docker Compose en un sistema operativo Windows 11.
2. Instalar y configurar git (Ver apendice C).
3. Si el sistema operativo del usuario es Windows 11, adicionalmente, el usuario debe instalar y configurar WSL 2. En el Apéndice D de este documento encontrara una guía acerca de cómo instalar WSL 2 en un sistema operativo Windows 11.

#### Pasos para la intalacion
1. **Solo aplica para windows 11** Abrir Docker Desktop y asegurarse de que el motor de Docker este corriendo, para esto el usuario puede ver en la parte inferior izquierda de la ventana de Docker Desktop un indicador verde con el texto Engine running.
2. Abrir WSL (Windows 11) o la terminal (Ubuntu 22.04 LTS) y clonar el repositorio de github en la carpeta de su preferencia. Para esto, el usuario puede utilizar el siguiente comando:

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
VITE_BROKER_MQTT = ws://localhost:8083/mqtt
VITE_BACKED_URL = http://localhost:3000
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
- Monitorear el estado de las variables ambientales en tiempo real.
- Monitorear el estado de los actuadores del sistema.
- Monitorear el estado de las conexiones del sistema.
- Interacturar con los actuadores del sistema, encendiendo y/o apagando actuadores
- Interacturar con el sistema de control de Ph y Electroconductividad permitiendo modificar los niveles deseados.

## Stack Tecnologico
- JavaScript
- Node.js
- Nest.js
- MongoDB
- MQTT
- Docker
- React.js
- Tailwind CSS

## Funcionamiento
1. Para monitorear el estado de las variables ambientales en tiempo real, el usuario debe acceder a la pagina de inicio de la aplicación. En el menu de navegacion ubicado en la parte superior  para pantallas grandes y en la parte inferior para pantallas pequeñas como celulares, el usuario podra navegar entre la seccion Hidroponico y Cultivo. En estas secciones, el usuario encontrara valores en tiempo real de las variables ambientales como la temperatura, la humedad, dioxido de carbono, PPF, VPD, PPFD, temperatura del agua, ph, conductividad electrica, nivel de agua. Adicionalmente, el usuario podra visualizar la fecha y hora en la que se tomaron las ultimas mediciones.
2. Para monitorear el estado de los actuadores del sistema, el usuario podrá visualizar algunos indicadores ubicados en la parte superior derecha para pantallas grandes y en la parte superior para pantallas pequeñas. El usuario podrá visualizar si los actuadores están encendidos o apagados. Así como también, podrá visualizar el estado de las conexiones del sistema.
3. Para interactuar con los actuadores del sistema, el usuario podra acceder a la pagina actuadores desde el menu de navegacion ubicado en la parte superior para pantallas grandes y en la parte inferior para pantallas pequeñas. En esta seccion, el usuario podra visualizar el estado de los actuadores del sistema representados por botones de encendido y apagado. El usuario podra cambiar el estado de los actuadores del sistema, encendiendo y/o apagando actuadores.
4. Para interactuar con el sistema de control de Ph y Electroconductividad, el usuario podra acceder a la pagina de control desde el menu de navegacion ubicado en la parte superior para pantallas grandes y en la parte inferior para pantallas pequeñas. En esta seccion, el usuario podra visualizar el valor actual deseado de Ph y Electroconductividad. Adicionalmente, el usuario los podra modificar a su gusto y enviarlos al sistema de control.


## Apendice A: Instalacion de Docker Compose en Ubuntu 22.04 LTS
Para crear esta guia de instalacion, se utilizo la documentacion encontrada en la pagina oficial de Digital Ocean. Para mas informacion, el usuario puede acceder al siguiente enlace:

1. [Como Instalar y usar Docker en Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)
2. [Como Instalar y usar Docker Compose en Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04)

Antes de instalar docker compose en Ubuntu 22.04 LTS, el usuario debe tener instalado y configurado Docker en su computadora local o en su servidor web. Para esto, el usuario puede seguir los siguientes pasos:

1. Actualizar el sistema operativo y los paquetes instalados en el sistema. Para esto, el usuario puede utilizar los siguientes comandos:

```bash
sudo apt update
sudo apt upgrade
```

2. Instalar los paquetes necesarios para permitir que apt utilice un repositorio sobre HTTPS. Para esto, el usuario puede utilizar el siguiente comando:

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

3. Agregar la clave GPG oficial de Docker al sistema. Para esto, el usuario puede utilizar el siguiente comando:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4. Configurar el repositorio estable de Docker. Para esto, el usuario puede utilizar el siguiente comando:

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. Actualizar el sistema operativo y los paquetes instalados en el sistema. Para esto, el usuario puede utilizar los siguientes comandos:

```bash
sudo apt update
sudo apt upgrade
```

6. Instalar Docker en el sistema. Para esto, el usuario puede utilizar el siguiente comando:

```bash
sudo apt install docker-ce
```

7. Verificar que Docker se ha instalado correctamente. Para esto, el usuario puede utilizar el siguiente comando:

```bash
sudo systemctl status docker
```

Despues de instalar Docker en Ubuntu 22.04 LTS, el usuario puede instalar Docker Compose en su computadora local o en su servidor web. Para esto, el usuario puede seguir los siguientes pasos:

1. Descargar la versión estable actual de Docker Compose. Para esto, el usuario puede utilizar el siguiente comando:

```bash
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
```

2. Dar permisos de ejecución al archivo de Docker Compose. Para esto, el usuario puede utilizar el siguiente comando:

```bash
chmod +x ~/.docker/cli-plugins/docker-compose
```

3. Verificar que Docker Compose se ha instalado correctamente. Para esto, el usuario puede utilizar el siguiente comando:

```bash
docker compose --version
```

## Apendice B: Instalacion de Docker Compose en Windows 11
Para instalar Docker Compose en Windows 11, el usuario puede instalar Docker Desktop. Para esto, el usuario puede seguir los siguientes pasos:

1. Descargar el instalador de Docker Desktop desde el sitio web oficial de Docker. Para esto, el usuario puede acceder al siguiente enlace:

[Docker Desktop](https://www.docker.com/products/docker-desktop)

2. Ejecutar el instalador de Docker Desktop y seguir las instrucciones del asistente de instalación.
3. Iniciar Docker Desktop y esperar a que el motor de Docker esté en funcionamiento.
4. Configurar Docker Desktop para que utilice WSL 2. Para esto, el usuario puede seguir los siguientes pasos:
5. Hacer clic en el icono de Docker Desktop ubicado en la bandeja del sistema.
6. Seleccionar la opción de Settings.
7. Hacer clic en la opción de Resources.
8. Hacer clic en la opción de WSL Integration.
9. Seleccionar la casilla de verificación de Enable integration with my default WSL distro.
10. Hacer click en la opcion de General y seleccionar la opcion de *Use the WSL 2 based engine*
11. Hacer clic en la opción de Apply & Restart.


## Apendice C: Instalacion y configuracion de git
Para instalar git en Ubuntu 22.04 LTS, el usuario puede utilizar el siguiente comando:

```bash
sudo apt install git
```

Para configurar git en Ubuntu 22.04 LTS, el usuario puede utilizar los siguientes comandos:

```bash
git config --global user.name "Your Name"
git config --global user.email "
```

Para crear una llave SSH en Ubuntu 22.04 LTS, el usuario puede utilizar el siguiente comando:

```bash
ssh-keygen -t rsa -b 4096 -C "tu@email.com"
```

Para agregar la llave SSH al agente SSH en Ubuntu 22.04 LTS, el usuario puede utilizar el siguiente

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Para ver la llave SSH en Ubuntu 22.04 LTS, el usuario puede utilizar el siguiente comando:

```bash
cat ~/.ssh/id_rsa.pub
```

Luego, el usuario debe copiar la llave SSH y agregarla a su cuenta de GitHub. Para esto, el usuario debe seguir los siguientes pasos:

1. Iniciar sesión en su cuenta de GitHub.
2. Hacer clic en su foto de perfil y seleccionar la opción de Settings.
3. Hacer clic en la opción de SSH and GPG keys.
4. Hacer clic en la opción de New SSH key.
5. Pegar la llave SSH en el campo de Key y darle un nombre a la llave.
6. Hacer clic en la opción de Add SSH key.
7. Confirmar la acción ingresando su contraseña de GitHub.

## Apendice D: Instalacion de WSL
Para instalar WSL 2 en Windows 11, el usuario puede seguir los siguientes pasos:

1. Ingresar a la Microsoft Store.
2. Buscar la aplicación de Windows Subsystem for Linux.
3. Hacer clic en la opción de Install.
4. Esperar a que se complete la instalación.
5. Buscar la aplicación de Ubuntu.
6. Hacer clic en la opción de Install.
7. Esperar a que se complete la instalación.

*Recomendado*
8. Buscar la aplicación de Windows Terminal.
9. Hacer clic en la opción de Install.
10. Esperar a que se complete la instalación.
11. Ingresar a la aplicación de Windows Terminal.

Despues de abrir la aplicacion de Windows Terminal, el usuario debe configurar Ubuntu. Para esto, el usuario puede seguir los siguientes pasos:

1. Ingresar a la aplicación de Windows Terminal.
2. Hacer clic en la opción de la flecha hacia abajo ubicada en la parte superior izquierda de la ventana.
3. Hacer clic en la opción de Settings.
4. Hacer clic en la opción de Default Profile.
5. Hacer clic en la opción de Ubuntu.
6. Hacer clic en el botón de Save.
7. Reiniciar la aplicación de Windows Terminal.



