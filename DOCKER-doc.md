# Guía Completa de Docker en Terminal

## Tabla de Contenidos

- [Guía Completa de Docker en Terminal](#guía-completa-de-docker-en-terminal)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [Introducción a Docker](#introducción-a-docker)
    - [¿Qué es Docker y para qué sirve?](#qué-es-docker-y-para-qué-sirve)
    - [Principales características de Docker:](#principales-características-de-docker)
    - [¿Por qué usar Docker?](#por-qué-usar-docker)
    - [Resumen:](#resumen)
  - [Instalación y Configuración Inicial](#instalación-y-configuración-inicial)
    - [Instalación de Docker en Linux](#instalación-de-docker-en-linux)
      - [Pasos para instalar Docker:](#pasos-para-instalar-docker)
      - [Opcional: Ejecutar Docker sin usar `sudo`](#opcional-ejecutar-docker-sin-usar-sudo)
    - [Verificación de la instalación](#verificación-de-la-instalación)
      - [Verificación adicional con contenedores:](#verificación-adicional-con-contenedores)
    - [Resumen:](#resumen-1)
  - [Comandos Básicos de Docker](#comandos-básicos-de-docker)
    - [Sintaxis básica de los comandos de Docker](#sintaxis-básica-de-los-comandos-de-docker)
    - [Comandos esenciales de Docker](#comandos-esenciales-de-docker)
      - [1. **`docker run`**: Crear y ejecutar un contenedor](#1-docker-run-crear-y-ejecutar-un-contenedor)
      - [2. **`docker ps`**: Ver contenedores en ejecución](#2-docker-ps-ver-contenedores-en-ejecución)
      - [3. **`docker ps -a`**: Ver todos los contenedores (activos y detenidos)](#3-docker-ps--a-ver-todos-los-contenedores-activos-y-detenidos)
      - [4. **`docker stop`**: Detener un contenedor en ejecución](#4-docker-stop-detener-un-contenedor-en-ejecución)
      - [5. **`docker start`**: Iniciar un contenedor detenido](#5-docker-start-iniciar-un-contenedor-detenido)
      - [6. **`docker restart`**: Reiniciar un contenedor](#6-docker-restart-reiniciar-un-contenedor)
      - [7. **`docker rm`**: Eliminar un contenedor](#7-docker-rm-eliminar-un-contenedor)
      - [8. **`docker images`**: Ver las imágenes disponibles](#8-docker-images-ver-las-imágenes-disponibles)
      - [9. **`docker pull`**: Descargar una imagen](#9-docker-pull-descargar-una-imagen)
      - [10. **`docker build`**: Crear una imagen personalizada](#10-docker-build-crear-una-imagen-personalizada)
      - [11. **`docker logs`**: Ver los logs de un contenedor](#11-docker-logs-ver-los-logs-de-un-contenedor)
    - [Resumen de los Comandos Básicos](#resumen-de-los-comandos-básicos)
  - [Gestión de Contenedores](#gestión-de-contenedores)
    - [Activar y desactivar contenedores](#activar-y-desactivar-contenedores)
      - [Activar un contenedor](#activar-un-contenedor)
      - [Detener un contenedor](#detener-un-contenedor)
      - [Reiniciar un contenedor](#reiniciar-un-contenedor)
    - [Listar contenedores activos y detenidos](#listar-contenedores-activos-y-detenidos)
      - [Listar contenedores activos](#listar-contenedores-activos)
      - [Listar todos los contenedores (activos y detenidos)](#listar-todos-los-contenedores-activos-y-detenidos)
    - [Acceso a contenedores](#acceso-a-contenedores)
      - [`docker exec`: Ejecutar comandos dentro de un contenedor](#docker-exec-ejecutar-comandos-dentro-de-un-contenedor)
      - [`docker attach`: Conectarse a la salida de un contenedor](#docker-attach-conectarse-a-la-salida-de-un-contenedor)
    - [Resumen de los Comandos de Gestión de Contenedores](#resumen-de-los-comandos-de-gestión-de-contenedores)
  - [Gestión de Imágenes](#gestión-de-imágenes)
    - [Descarga e instalación de imágenes desde Docker Hub](#descarga-e-instalación-de-imágenes-desde-docker-hub)
      - [**Descargar una imagen desde Docker Hub**](#descargar-una-imagen-desde-docker-hub)
      - [**Listar las imágenes descargadas**](#listar-las-imágenes-descargadas)
    - [Creación de imágenes personalizadas con `Dockerfile`](#creación-de-imágenes-personalizadas-con-dockerfile)
      - [**Sintaxis básica de un `Dockerfile`**](#sintaxis-básica-de-un-dockerfile)
      - [**Pasos para crear una imagen personalizada con `Dockerfile`**](#pasos-para-crear-una-imagen-personalizada-con-dockerfile)
    - [Eliminación de imágenes innecesarias](#eliminación-de-imágenes-innecesarias)
      - [**Eliminar una imagen específica**](#eliminar-una-imagen-específica)
      - [**Eliminar todas las imágenes no utilizadas**](#eliminar-todas-las-imágenes-no-utilizadas)
      - [**Eliminar todas las imágenes, contenedores, redes y volúmenes no utilizados**](#eliminar-todas-las-imágenes-contenedores-redes-y-volúmenes-no-utilizados)
    - [Resumen de Comandos de Gestión de Imágenes](#resumen-de-comandos-de-gestión-de-imágenes)
  - [Bases de Datos en Docker](#bases-de-datos-en-docker)
    - [Cómo configurar contenedores para bases de datos como **PostgreSQL** y **MongoDB**](#cómo-configurar-contenedores-para-bases-de-datos-como-postgresql-y-mongodb)
      - [**Configurar PostgreSQL en Docker**](#configurar-postgresql-en-docker)
      - [**Configurar MongoDB en Docker**](#configurar-mongodb-en-docker)
    - [Cómo exponer puertos y acceder a bases de datos desde la red local](#cómo-exponer-puertos-y-acceder-a-bases-de-datos-desde-la-red-local)
      - [**Exponer puertos al crear un contenedor**](#exponer-puertos-al-crear-un-contenedor)
      - [**Acceder a la base de datos desde la red local**](#acceder-a-la-base-de-datos-desde-la-red-local)
    - [Volúmenes de Docker para persistencia de datos](#volúmenes-de-docker-para-persistencia-de-datos)
      - [**Qué son los volúmenes en Docker**](#qué-son-los-volúmenes-en-docker)
      - [**Crear un volumen de Docker**](#crear-un-volumen-de-docker)
      - [**Montar un volumen en un contenedor**](#montar-un-volumen-en-un-contenedor)
      - [**Ver los volúmenes existentes**](#ver-los-volúmenes-existentes)
      - [**Eliminar un volumen**](#eliminar-un-volumen)
    - [Resumen de Comandos para Bases de Datos en Docker](#resumen-de-comandos-para-bases-de-datos-en-docker)
  - [Ejecutar Otros Programas en Docker](#ejecutar-otros-programas-en-docker)
    - [Ejecutar aplicaciones web como **Django** y **Flask** dentro de contenedores](#ejecutar-aplicaciones-web-como-django-y-flask-dentro-de-contenedores)
      - [**Ejecutar una aplicación Django en Docker**](#ejecutar-una-aplicación-django-en-docker)
      - [**Ejecutar una aplicación Flask en Docker**](#ejecutar-una-aplicación-flask-en-docker)
    - [Cómo gestionar variables de entorno y configuraciones](#cómo-gestionar-variables-de-entorno-y-configuraciones)
      - [**Definir variables de entorno en el `Dockerfile`**](#definir-variables-de-entorno-en-el-dockerfile)
      - [**Pasar variables de entorno al ejecutar el contenedor**](#pasar-variables-de-entorno-al-ejecutar-el-contenedor)
      - [**Usar archivos `.env` para gestionar configuraciones**](#usar-archivos-env-para-gestionar-configuraciones)
    - [Resumen de Comandos para Ejecutar Aplicaciones Web en Docker](#resumen-de-comandos-para-ejecutar-aplicaciones-web-en-docker)
  - [Gestión Avanzada de Contenedores](#gestión-avanzada-de-contenedores)
    - [Uso de múltiples contenedores simultáneamente (docker-compose)](#uso-de-múltiples-contenedores-simultáneamente-docker-compose)
      - [**¿Qué es Docker Compose?**](#qué-es-docker-compose)
      - [**Ejemplo básico de `docker-compose.yml`**](#ejemplo-básico-de-docker-composeyml)
      - [**Comandos básicos de Docker Compose**](#comandos-básicos-de-docker-compose)
    - [Creación y gestión de redes de contenedores](#creación-y-gestión-de-redes-de-contenedores)
      - [**Redes predeterminadas de Docker**](#redes-predeterminadas-de-docker)
      - [**Crear una red personalizada**](#crear-una-red-personalizada)
      - [**Conectar contenedores a redes**](#conectar-contenedores-a-redes)
      - [**Ver las redes disponibles**](#ver-las-redes-disponibles)
      - [**Conectar un contenedor existente a una red**](#conectar-un-contenedor-existente-a-una-red)
      - [**Eliminar una red**](#eliminar-una-red)
    - [Volúmenes y persistencia de datos](#volúmenes-y-persistencia-de-datos)
      - [**Crear un volumen de Docker**](#crear-un-volumen-de-docker-1)
      - [**Montar un volumen en un contenedor**](#montar-un-volumen-en-un-contenedor-1)
      - [**Ver los volúmenes disponibles**](#ver-los-volúmenes-disponibles)
      - [**Eliminar un volumen**](#eliminar-un-volumen-1)
      - [**Usar volúmenes con `docker-compose`**](#usar-volúmenes-con-docker-compose)
    - [Resumen de Comandos para Gestión Avanzada de Contenedores](#resumen-de-comandos-para-gestión-avanzada-de-contenedores)
  - [Buenas Prácticas y Seguridad](#buenas-prácticas-y-seguridad)
    - [Principios de seguridad en Docker](#principios-de-seguridad-en-docker)
      - [**1. Uso de imágenes oficiales y verificadas**](#1-uso-de-imágenes-oficiales-y-verificadas)
      - [**2. Mantén las imágenes y contenedores actualizados**](#2-mantén-las-imágenes-y-contenedores-actualizados)
      - [**3. Evitar ejecutar contenedores como root**](#3-evitar-ejecutar-contenedores-como-root)
      - [**4. Limitar los permisos del contenedor**](#4-limitar-los-permisos-del-contenedor)
      - [**5. Usar redes privadas para comunicación entre contenedores**](#5-usar-redes-privadas-para-comunicación-entre-contenedores)
      - [**6. Auditar contenedores con herramientas de seguridad**](#6-auditar-contenedores-con-herramientas-de-seguridad)
    - [Cómo optimizar contenedores para mayor eficiencia](#cómo-optimizar-contenedores-para-mayor-eficiencia)
      - [**1. Optimizar el tamaño de las imágenes**](#1-optimizar-el-tamaño-de-las-imágenes)
      - [**2. Usar capas de caché efectivas**](#2-usar-capas-de-caché-efectivas)
      - [**3. Limitar los recursos de los contenedores**](#3-limitar-los-recursos-de-los-contenedores)
    - [Estrategias de backup y restauración de datos en contenedores](#estrategias-de-backup-y-restauración-de-datos-en-contenedores)
      - [**1. Usar volúmenes de Docker para persistencia de datos**](#1-usar-volúmenes-de-docker-para-persistencia-de-datos)
      - [**2. Crear copias de seguridad de volúmenes**](#2-crear-copias-de-seguridad-de-volúmenes)
      - [**3. Restaurar datos desde un respaldo**](#3-restaurar-datos-desde-un-respaldo)
      - [**4. Respaldar contenedores completos**](#4-respaldar-contenedores-completos)
    - [Resumen de Comandos para Buenas Prácticas y Seguridad](#resumen-de-comandos-para-buenas-prácticas-y-seguridad)
  - [Recursos Adicionales](#recursos-adicionales)
    - [Documentación oficial de Docker](#documentación-oficial-de-docker)
      - [**Secciones importantes de la documentación**:](#secciones-importantes-de-la-documentación)
    - [Enlaces útiles y tutoriales recomendados](#enlaces-útiles-y-tutoriales-recomendados)
      - [**Tutoriales y cursos recomendados**:](#tutoriales-y-cursos-recomendados)
      - [**Herramientas adicionales**:](#herramientas-adicionales)
    - [Comunidades y Foros](#comunidades-y-foros)
    - [Libros recomendados sobre Docker](#libros-recomendados-sobre-docker)
    - [Resumen](#resumen-2)

---


## Introducción a Docker

### ¿Qué es Docker y para qué sirve?

**Docker** es una plataforma abierta que permite automatizar el despliegue, escalado y gestión de aplicaciones dentro de contenedores. Un **contenedor** es un entorno aislado en el que se puede ejecutar una aplicación junto con sus dependencias, sin interferencias del sistema operativo subyacente.

Docker tiene como objetivo principal hacer que las aplicaciones sean fácilmente portables, escalables y reproducibles, sin preocuparse por las diferencias entre los entornos de desarrollo, pruebas y producción. Esto se logra gracias a la virtualización de contenedores, que proporciona un entorno consistente para ejecutar aplicaciones en cualquier máquina.

### Principales características de Docker:
1. **Portabilidad**: Docker asegura que una aplicación funcione de la misma manera, sin importar el sistema operativo en el que se ejecute. Esto lo logra empaquetando la aplicación junto con todas sus dependencias.
   
2. **Aislamiento**: Los contenedores de Docker son entornos aislados que no afectan a otras aplicaciones o servicios en la misma máquina. Esto permite ejecutar múltiples aplicaciones en el mismo sistema sin conflictos.

3. **Escalabilidad**: Docker facilita la gestión de aplicaciones que requieren escalar en función de la demanda. Puedes desplegar contenedores adicionales rápidamente y manejar múltiples instancias de una misma aplicación.

4. **Eficiencia**: A diferencia de las máquinas virtuales, que requieren un sistema operativo completo por contenedor, los contenedores Docker comparten el mismo sistema operativo, lo que los hace mucho más ligeros y rápidos.

### ¿Por qué usar Docker?

- **Desarrollo y pruebas más rápidas**: Los desarrolladores pueden trabajar en sus aplicaciones sin preocuparse por las configuraciones del sistema operativo o las dependencias del proyecto. Docker permite a los desarrolladores ejecutar aplicaciones dentro de contenedores preconfigurados con todas las herramientas necesarias.
  
- **Despliegue y escalabilidad**: Docker facilita el despliegue de aplicaciones en cualquier entorno, lo que permite a las empresas escalar sus aplicaciones con facilidad. Además, Docker se integra bien con sistemas de orquestación como Kubernetes, lo que facilita el despliegue en la nube.

- **Consistencia**: Con Docker, puedes estar seguro de que la aplicación se ejecutará de la misma manera en todos los entornos, ya que el contenedor garantiza que todas las dependencias estén incluidas y configuradas correctamente.

- **Ecosistema amplio**: Docker cuenta con un ecosistema robusto de herramientas y servicios, como Docker Hub, que proporciona acceso a miles de imágenes de aplicaciones que se pueden usar de inmediato.

### Resumen:

Docker es una solución potente para empaquetar, distribuir y ejecutar aplicaciones de forma eficiente, lo que facilita el trabajo de los desarrolladores y administradores de sistemas. Con Docker, puedes asegurarte de que las aplicaciones sean portátiles, escalables y fáciles de gestionar en cualquier entorno.

---

## Instalación y Configuración Inicial

### Instalación de Docker en Linux

Docker se puede instalar fácilmente en Linux usando los repositorios oficiales de Docker. Los pasos pueden variar dependiendo de la distribución que estés usando, pero a continuación se muestra el proceso para instalar Docker en una distribución basada en Debian, como **Raspberry Pi OS** o **Ubuntu**.

#### Pasos para instalar Docker:

1. **Actualizar los repositorios y el sistema:**

   Primero, actualiza la lista de paquetes y asegúrate de que el sistema esté completamente actualizado.

   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Instalar dependencias necesarias:**

   Asegúrate de que tu sistema tenga las dependencias necesarias para instalar Docker. Usa el siguiente comando para instalar los paquetes esenciales.

   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
   ```

3. **Agregar la clave GPG oficial de Docker:**

   Docker requiere una clave GPG para asegurarse de que las descargas sean seguras. Añádela a tu sistema con el siguiente comando:

   ```bash
   curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
   ```

4. **Agregar el repositorio de Docker:**

   Ahora, añade el repositorio oficial de Docker a tu lista de fuentes de APT.

   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
   ```

5. **Instalar Docker:**

   Actualiza nuevamente la lista de paquetes y luego instala Docker.

   ```bash
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io -y
   ```

6. **Verificación de la instalación de Docker:**

   Al finalizar la instalación, asegúrate de que Docker esté funcionando correctamente.

   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

   Esto asegura que Docker se ejecute automáticamente cada vez que reinicies tu máquina.

#### Opcional: Ejecutar Docker sin usar `sudo`

Si prefieres ejecutar los comandos de Docker sin usar `sudo` cada vez, puedes agregar tu usuario al grupo `docker`:

```bash
sudo usermod -aG docker $USER
```

Luego, reinicia tu sesión o la máquina para que los cambios tengan efecto.

---

### Verificación de la instalación

Después de instalar Docker, es importante verificar que la instalación se haya realizado correctamente. Para hacer esto, ejecuta el siguiente comando para verificar la versión de Docker:

```bash
docker --version
```

Este comando debería devolver la versión de Docker instalada, algo como:

```
Docker version 20.10.7, build f0df350
```

#### Verificación adicional con contenedores:

Para asegurarte de que Docker está funcionando correctamente, puedes ejecutar el siguiente comando para probar un contenedor simple que imprima un mensaje en tu terminal:

```bash
docker run hello-world
```

Este comando descarga una imagen pequeña de Docker y crea un contenedor que imprime un mensaje de confirmación, indicando que Docker está instalado y funcionando correctamente.

### Resumen:

- Docker se instala fácilmente en sistemas basados en Debian usando los repositorios oficiales.
- Después de la instalación, puedes verificar que Docker está funcionando correctamente con el comando `docker --version` y ejecutando el contenedor `hello-world`.
- Puedes ejecutar Docker sin usar `sudo` agregando tu usuario al grupo `docker`.

---

## Comandos Básicos de Docker

### Sintaxis básica de los comandos de Docker

Docker utiliza comandos simples y consistentes para interactuar con contenedores, imágenes y redes. La sintaxis básica de un comando de Docker generalmente sigue este formato:

```bash
docker [opción] [comando] [argumentos]
```

- **`docker`**: Llama al ejecutable principal de Docker.
- **`[opción]`**: Son opciones opcionales que modifican el comportamiento del comando.
- **`[comando]`**: El comando específico que le dice a Docker qué hacer.
- **`[argumentos]`**: Son los parámetros necesarios para ejecutar el comando (como el nombre del contenedor o la imagen).

A continuación, verás una lista de los comandos más comunes que se usan en Docker, junto con ejemplos de su sintaxis.

---

### Comandos esenciales de Docker

#### 1. **`docker run`**: Crear y ejecutar un contenedor

Este comando crea un contenedor a partir de una imagen y lo ejecuta.

```bash
docker run [opciones] <imagen> [comando]
```

**Ejemplo**:

```bash
docker run hello-world
```

Este comando ejecuta el contenedor de la imagen `hello-world` que imprime un mensaje de confirmación.

**Opciones comunes**:
- `-d` o `--detach`: Ejecuta el contenedor en segundo plano (modo desanexado).
- `-p <puerto_local>:<puerto_contenedor>`: Mapea un puerto local a un puerto del contenedor.
  
**Ejemplo con opción**:

```bash
docker run -d -p 8080:80 nginx
```

Este comando ejecuta un contenedor en segundo plano usando la imagen `nginx` y mapea el puerto 8080 de tu máquina al puerto 80 del contenedor.

#### 2. **`docker ps`**: Ver contenedores en ejecución

Este comando muestra los contenedores activos (en ejecución).

```bash
docker ps
```

**Ejemplo**:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS        PORTS                  NAMES
a3c497ddc314   nginx     "nginx -g 'daemon of…"   2 minutes ago  Up 2 minutes  0.0.0.0:8080->80/tcp   cool_mayer
```

#### 3. **`docker ps -a`**: Ver todos los contenedores (activos y detenidos)

Si necesitas ver todos los contenedores, incluidos los detenidos, utiliza la opción `-a`.

```bash
docker ps -a
```

#### 4. **`docker stop`**: Detener un contenedor en ejecución

Este comando detiene un contenedor en ejecución de forma ordenada.

```bash
docker stop <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker stop cool_mayer
```

Este comando detiene el contenedor con el nombre `cool_mayer`.

#### 5. **`docker start`**: Iniciar un contenedor detenido

Este comando inicia un contenedor previamente detenido.

```bash
docker start <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker start cool_mayer
```

#### 6. **`docker restart`**: Reiniciar un contenedor

Reinicia un contenedor sin detenerlo y volverlo a iniciar manualmente.

```bash
docker restart <nombre_contenedor_o_ID>
```

#### 7. **`docker rm`**: Eliminar un contenedor

Este comando elimina un contenedor detenido.

```bash
docker rm <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker rm cool_mayer
```

Este comando elimina el contenedor llamado `cool_mayer` (debe estar detenido antes de poder eliminarlo).

#### 8. **`docker images`**: Ver las imágenes disponibles

Muestra una lista de todas las imágenes descargadas en tu sistema.

```bash
docker images
```

**Ejemplo de salida**:

```bash
REPOSITORY          TAG       IMAGE ID       CREATED          SIZE
nginx               latest    e6d7d86e9f6f   2 days ago       132MB
hello-world         latest    fce289e99eb9   5 months ago     1.84kB
```

#### 9. **`docker pull`**: Descargar una imagen

Este comando descarga una imagen desde Docker Hub o un repositorio especificado.

```bash
docker pull <imagen>
```

**Ejemplo**:

```bash
docker pull nginx
```

Este comando descarga la imagen `nginx` desde Docker Hub.

#### 10. **`docker build`**: Crear una imagen personalizada

Este comando crea una nueva imagen a partir de un `Dockerfile` en el directorio actual.

```bash
docker build -t <nombre_imagen> <directorio>
```

**Ejemplo**:

```bash
docker build -t my-image .
```

Este comando construye una imagen llamada `my-image` usando el `Dockerfile` del directorio actual.

#### 11. **`docker logs`**: Ver los logs de un contenedor

Este comando te permite ver los logs de un contenedor en ejecución.

```bash
docker logs <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker logs cool_mayer
```

---

### Resumen de los Comandos Básicos

- `docker run`: Ejecutar un contenedor.
- `docker ps`: Ver los contenedores en ejecución.
- `docker ps -a`: Ver todos los contenedores.
- `docker stop`: Detener un contenedor.
- `docker start`: Iniciar un contenedor detenido.
- `docker rm`: Eliminar un contenedor detenido.
- `docker images`: Ver las imágenes locales.
- `docker pull`: Descargar una imagen.
- `docker build`: Crear una imagen personalizada.
- `docker logs`: Ver los logs de un contenedor.

---

## Gestión de Contenedores

### Activar y desactivar contenedores

Docker permite iniciar, detener y reiniciar contenedores fácilmente. Aquí te mostramos cómo hacerlo.

#### Activar un contenedor

Para iniciar un contenedor que se ha detenido previamente, usa el comando `docker start`. Si el contenedor está en ejecución, no es necesario iniciar de nuevo.

**Sintaxis**:

```bash
docker start <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker start cool_mayer
```

Este comando iniciará el contenedor llamado `cool_mayer`.

#### Detener un contenedor

Para detener un contenedor en ejecución, utiliza el comando `docker stop`.

**Sintaxis**:

```bash
docker stop <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker stop cool_mayer
```

Este comando detendrá el contenedor llamado `cool_mayer`.

#### Reiniciar un contenedor

Si deseas reiniciar un contenedor, puedes hacerlo con el comando `docker restart`. Este comando detiene e inicia el contenedor nuevamente.

**Sintaxis**:

```bash
docker restart <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker restart cool_mayer
```

---

### Listar contenedores activos y detenidos

Docker proporciona comandos para listar contenedores tanto activos como detenidos.

#### Listar contenedores activos

El comando `docker ps` muestra los contenedores que están actualmente en ejecución.

**Sintaxis**:

```bash
docker ps
```

**Ejemplo de salida**:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                   NAMES
a3c497ddc314   nginx     "nginx -g 'daemon of…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp    cool_mayer
```

#### Listar todos los contenedores (activos y detenidos)

Si quieres ver todos los contenedores, incluidos los que están detenidos, utiliza el comando `docker ps -a`.

**Sintaxis**:

```bash
docker ps -a
```

**Ejemplo de salida**:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                   PORTS                   NAMES
a3c497ddc314   nginx     "nginx -g 'daemon of…"   2 minutes ago   Up 2 minutes             0.0.0.0:8080->80/tcp    cool_mayer
d2f784f91c94   postgres  "docker-entrypoint.s…"   1 day ago       Exited (0) 24 hours ago                           pensive_rutherford
```

---

### Acceso a contenedores

Docker ofrece dos formas principales de interactuar con un contenedor en ejecución: `docker exec` y `docker attach`.

#### `docker exec`: Ejecutar comandos dentro de un contenedor

El comando `docker exec` te permite ejecutar comandos dentro de un contenedor en ejecución. Esto es útil para ejecutar tareas específicas dentro de un contenedor, como ejecutar una shell o un script.

**Sintaxis**:

```bash
docker exec -it <nombre_contenedor_o_ID> <comando>
```

- **`-it`**: Las opciones `-i` (interactivo) y `-t` (pseudo-terminal) permiten interactuar con el contenedor.
  
**Ejemplo**: Acceder a un contenedor con una shell bash

```bash
docker exec -it cool_mayer bash
```

Este comando te dará acceso al contenedor `cool_mayer` y abrirá una shell interactiva de Bash. Desde allí, puedes ejecutar cualquier comando como si estuvieras en una terminal normal.

#### `docker attach`: Conectarse a la salida de un contenedor

El comando `docker attach` te permite conectarte al proceso principal de un contenedor y ver su salida en tiempo real. A diferencia de `docker exec`, no se ejecuta un nuevo comando, sino que se conecta directamente a lo que el contenedor está ejecutando.

**Sintaxis**:

```bash
docker attach <nombre_contenedor_o_ID>
```

**Ejemplo**:

```bash
docker attach cool_mayer
```

Este comando te conectará al proceso principal del contenedor `cool_mayer`. Ten en cuenta que si el contenedor está ejecutando un servicio o aplicación, verás su salida estándar (stdout) en tu terminal.

> **Nota**: Si un contenedor se ejecuta en segundo plano (con la opción `-d`), `docker attach` no abrirá una terminal interactiva. Si deseas interactuar con el contenedor, es mejor usar `docker exec`.

---

### Resumen de los Comandos de Gestión de Contenedores

- **`docker start <contenedor>`**: Inicia un contenedor detenido.
- **`docker stop <contenedor>`**: Detiene un contenedor en ejecución.
- **`docker restart <contenedor>`**: Reinicia un contenedor.
- **`docker ps`**: Lista los contenedores activos en ejecución.
- **`docker ps -a`**: Lista todos los contenedores, incluidos los detenidos.
- **`docker exec -it <contenedor> <comando>`**: Ejecuta un comando dentro de un contenedor.
- **`docker attach <contenedor>`**: Se conecta al proceso principal de un contenedor en ejecución.

---

## Gestión de Imágenes

### Descarga e instalación de imágenes desde Docker Hub

Docker Hub es un registro público donde puedes encontrar y descargar imágenes oficiales de muchas aplicaciones y servicios. Es muy fácil descargar una imagen utilizando el comando `docker pull`.

#### **Descargar una imagen desde Docker Hub**

Para descargar una imagen, usa el siguiente comando:

```bash
docker pull <nombre_imagen>
```

**Ejemplo**: Para descargar la imagen oficial de `nginx`, simplemente ejecuta:

```bash
docker pull nginx
```

Este comando descargará la última versión disponible de la imagen `nginx` desde Docker Hub.

#### **Listar las imágenes descargadas**

Después de descargar una o varias imágenes, puedes ver todas las imágenes locales en tu sistema utilizando el comando:

```bash
docker images
```

**Ejemplo de salida**:

```bash
REPOSITORY          TAG       IMAGE ID       CREATED        SIZE
nginx               latest    e6d7d86e9f6f   2 days ago     132MB
hello-world         latest    fce289e99eb9   5 months ago   1.84kB
```

Este comando muestra todas las imágenes descargadas en tu sistema, incluyendo su `ID`, el `tag`, la fecha de creación y el tamaño de la imagen.

---

### Creación de imágenes personalizadas con `Dockerfile`

A veces, querrás crear una imagen personalizada para tu aplicación. Esto se puede hacer mediante la creación de un archivo llamado `Dockerfile`, que es un archivo de texto que contiene una lista de instrucciones para construir una imagen.

#### **Sintaxis básica de un `Dockerfile`**

Un `Dockerfile` define cómo debe configurarse la imagen, incluyendo las dependencias y comandos necesarios para construirla. A continuación te mostramos un ejemplo simple de un `Dockerfile`:

```dockerfile
# Usar una imagen base de Ubuntu
FROM ubuntu:20.04

# Establecer un directorio de trabajo
WORKDIR /app

# Copiar archivos locales a la imagen
COPY . .

# Instalar dependencias
RUN apt-get update && apt-get install -y python3 python3-pip

# Establecer el comando por defecto cuando se ejecute el contenedor
CMD ["python3", "app.py"]
```

#### **Pasos para crear una imagen personalizada con `Dockerfile`**

1. **Crear el archivo `Dockerfile`**:
   Crea un archivo llamado `Dockerfile` (sin extensión) en el directorio donde tienes los archivos de tu aplicación.

2. **Escribir las instrucciones**:
   Añade las instrucciones necesarias dentro del `Dockerfile` (como las que mostramos arriba).

3. **Construir la imagen**:
   Una vez que tengas el `Dockerfile`, usa el comando `docker build` para crear la imagen. La opción `-t` se usa para etiquetar la imagen con un nombre.

   ```bash
   docker build -t mi-imagen-personalizada .
   ```

   Esto construirá la imagen utilizando el `Dockerfile` en el directorio actual (`.`).

4. **Verificar que la imagen se creó correctamente**:
   Después de construir la imagen, puedes verificar que se haya creado correctamente con:

   ```bash
   docker images
   ```

---

### Eliminación de imágenes innecesarias

A lo largo del tiempo, es probable que acumules imágenes que ya no necesitas. Docker proporciona un comando para eliminar imágenes no utilizadas o innecesarias.

#### **Eliminar una imagen específica**

Si deseas eliminar una imagen específica, usa el siguiente comando:

```bash
docker rmi <nombre_imagen>
```

**Ejemplo**:

```bash
docker rmi nginx
```

Este comando eliminará la imagen `nginx` de tu sistema.

#### **Eliminar todas las imágenes no utilizadas**

Si tienes imágenes que ya no están siendo utilizadas por contenedores activos, puedes eliminar todas las imágenes que no estén en uso con:

```bash
docker image prune
```

Este comando elimina todas las imágenes no utilizadas, pero te pedirá confirmación antes de proceder.

#### **Eliminar todas las imágenes, contenedores, redes y volúmenes no utilizados**

Si deseas realizar una limpieza más profunda, eliminando no solo imágenes, sino también contenedores, redes y volúmenes no utilizados, puedes usar:

```bash
docker system prune -a
```

Este comando eliminará todo lo que no esté en uso, incluyendo imágenes que no están siendo utilizadas, contenedores detenidos, redes no utilizadas y volúmenes huérfanos.

> **Nota**: El uso de `-a` eliminará todas las imágenes no utilizadas, lo que puede liberar mucho espacio, pero también eliminará imágenes que podrías querer mantener para futuros proyectos.

---

### Resumen de Comandos de Gestión de Imágenes

- **`docker pull <imagen>`**: Descargar una imagen desde Docker Hub.
- **`docker images`**: Listar las imágenes locales.
- **`docker build -t <nombre_imagen> <directorio>`**: Crear una imagen personalizada a partir de un `Dockerfile`.
- **`docker rmi <imagen>`**: Eliminar una imagen específica.
- **`docker image prune`**: Eliminar imágenes no utilizadas.
- **`docker system prune -a`**: Eliminar imágenes, contenedores, redes y volúmenes no utilizados.

---

## Bases de Datos en Docker

### Cómo configurar contenedores para bases de datos como **PostgreSQL** y **MongoDB**

Docker facilita la creación de contenedores que ejecutan bases de datos populares, como **PostgreSQL** y **MongoDB**. A continuación, te mostramos cómo configurar contenedores para cada una de estas bases de datos.

#### **Configurar PostgreSQL en Docker**

Para configurar un contenedor con PostgreSQL, puedes usar la imagen oficial de Docker para PostgreSQL. A continuación se muestran los pasos para crear y ejecutar un contenedor de PostgreSQL.

**1. Descargar la imagen de PostgreSQL**

```bash
docker pull postgres
```

**2. Crear y ejecutar un contenedor de PostgreSQL**

Usa el siguiente comando para crear un contenedor de PostgreSQL, asignando contraseñas y configuraciones iniciales.

```bash
docker run --name postgres-db -e POSTGRES_PASSWORD=mi_contraseña -d -p 5432:5432 postgres
```

- `--name postgres-db`: Asigna un nombre al contenedor.
- `-e POSTGRES_PASSWORD=mi_contraseña`: Define la contraseña para el usuario `postgres`.
- `-d`: Ejecuta el contenedor en segundo plano.
- `-p 5432:5432`: Mapea el puerto 5432 del contenedor al puerto 5432 de la máquina local.

**3. Acceder a la base de datos PostgreSQL**

Para acceder al contenedor de PostgreSQL y realizar consultas dentro de la base de datos, usa:

```bash
docker exec -it postgres-db psql -U postgres
```

Este comando te dará acceso a la terminal de PostgreSQL dentro del contenedor.

#### **Configurar MongoDB en Docker**

De manera similar, puedes configurar un contenedor para MongoDB utilizando la imagen oficial de Docker.

**1. Descargar la imagen de MongoDB**

```bash
docker pull mongo
```

**2. Crear y ejecutar un contenedor de MongoDB**

Este comando crea y ejecuta un contenedor para MongoDB.

```bash
docker run --name mongodb -d -p 27017:27017 mongo
```

- `--name mongodb`: Asigna un nombre al contenedor.
- `-d`: Ejecuta el contenedor en segundo plano.
- `-p 27017:27017`: Mapea el puerto 27017 del contenedor al puerto 27017 de la máquina local.

**3. Acceder a MongoDB desde la shell**

Puedes acceder a la shell interactiva de MongoDB utilizando el siguiente comando:

```bash
docker exec -it mongodb mongo
```

Este comando te conecta al contenedor `mongodb` y te da acceso a la shell de MongoDB.

---

### Cómo exponer puertos y acceder a bases de datos desde la red local

Cuando creas un contenedor, Docker mapea puertos entre el contenedor y tu máquina local. Esto es útil para poder acceder a los servicios de base de datos desde fuera del contenedor, como tu red local o incluso internet (si se configura adecuadamente).

#### **Exponer puertos al crear un contenedor**

En los ejemplos anteriores de PostgreSQL y MongoDB, hemos usado la opción `-p` para exponer puertos específicos:

```bash
-p <puerto_local>:<puerto_contenedor>
```

- `5432:5432` para PostgreSQL
- `27017:27017` para MongoDB

Esto permite que, al acceder a los puertos mapeados de la máquina local (`localhost:5432` o `localhost:27017`), puedas comunicarte con los contenedores y sus bases de datos.

#### **Acceder a la base de datos desde la red local**

Una vez que el contenedor esté en ejecución y los puertos estén mapeados, puedes acceder a la base de datos desde cualquier otra máquina en tu red local utilizando el **IP de tu Raspberry Pi** y el puerto correspondiente.

**Ejemplo para PostgreSQL**:

Accede a PostgreSQL en tu red local usando un cliente PostgreSQL y apuntando a la IP de la Raspberry Pi, por ejemplo, `192.168.2.120`:

```bash
psql -h 192.168.2.120 -p 5432 -U postgres
```

**Ejemplo para MongoDB**:

Accede a MongoDB desde otra máquina con:

```bash
mongo 192.168.2.120:27017
```

Estos comandos permiten la conexión desde fuera del contenedor, usando la IP de la máquina anfitriona y los puertos mapeados.

---

### Volúmenes de Docker para persistencia de datos

Cuando se usan contenedores con bases de datos, es crucial que los datos se persistan incluso después de detener o eliminar un contenedor. Docker proporciona **volúmenes** para gestionar esta persistencia.

#### **Qué son los volúmenes en Docker**

Los volúmenes son una forma de persistir datos fuera del contenedor. Un volumen es un área de almacenamiento en tu máquina que puede ser compartida entre contenedores, y cuyos datos persisten más allá de la vida del contenedor.

#### **Crear un volumen de Docker**

Puedes crear un volumen de Docker usando el siguiente comando:

```bash
docker volume create nombre_volumen
```

**Ejemplo**:

```bash
docker volume create pgdata
```

Este comando crea un volumen llamado `pgdata` que luego puede ser montado en un contenedor.

#### **Montar un volumen en un contenedor**

Cuando ejecutas un contenedor que usa una base de datos, puedes montar un volumen para que los datos de la base de datos se almacenen de forma persistente en el volumen. Para montar un volumen en el contenedor, usa la opción `-v` al ejecutar el contenedor:

```bash
docker run -d -v pgdata:/var/lib/postgresql/data -p 5432:5432 --name postgres-db postgres
```

- `-v pgdata:/var/lib/postgresql/data`: Esto monta el volumen `pgdata` en el directorio `/var/lib/postgresql/data` dentro del contenedor, donde PostgreSQL guarda los datos.
- `-p 5432:5432`: Mapea el puerto 5432 del contenedor al puerto 5432 de la máquina local.

#### **Ver los volúmenes existentes**

Puedes listar los volúmenes de Docker utilizando el siguiente comando:

```bash
docker volume ls
```

#### **Eliminar un volumen**

Si ya no necesitas un volumen, puedes eliminarlo con:

```bash
docker volume rm <nombre_volumen>
```

**Ejemplo**:

```bash
docker volume rm pgdata
```

> **Nota**: Si un volumen está siendo utilizado por un contenedor, primero debes detener o eliminar ese contenedor antes de poder eliminar el volumen.

---

### Resumen de Comandos para Bases de Datos en Docker

- **`docker pull postgres`**: Descargar la imagen oficial de PostgreSQL.
- **`docker run --name postgres-db -e POSTGRES_PASSWORD=mi_contraseña -d -p 5432:5432 postgres`**: Crear un contenedor de PostgreSQL.
- **`docker exec -it postgres-db psql -U postgres`**: Acceder a la base de datos de PostgreSQL desde la terminal del contenedor.
- **`docker pull mongo`**: Descargar la imagen oficial de MongoDB.
- **`docker run --name mongodb -d -p 27017:27017 mongo`**: Crear un contenedor de MongoDB.
- **`docker exec -it mongodb mongo`**: Acceder a la shell de MongoDB.
- **`docker volume create <nombre_volumen>`**: Crear un volumen para persistir datos.
- **`docker run -d -v <volumen>:/ruta/en/contenedor -p <puerto_local>:<puerto_contenedor> <imagen>`**: Montar un volumen en un contenedor.
- **`docker volume ls`**: Listar los volúmenes existentes.
- **`docker volume rm <nombre_volumen>`**: Eliminar un volumen.

---

## Ejecutar Otros Programas en Docker

### Ejecutar aplicaciones web como **Django** y **Flask** dentro de contenedores

Docker es una excelente herramienta para ejecutar aplicaciones web de forma aislada. A continuación, veremos cómo crear contenedores para ejecutar aplicaciones **Django** y **Flask**.

#### **Ejecutar una aplicación Django en Docker**

Para ejecutar una aplicación Django dentro de un contenedor, primero necesitas crear un `Dockerfile` que defina cómo se debe construir el contenedor. Aquí está un ejemplo básico para una aplicación Django:

**1. Crear un archivo `Dockerfile` para Django**

Dentro del directorio de tu proyecto Django, crea un archivo `Dockerfile`:

```dockerfile
# Usa una imagen oficial de Python
FROM python:3.9

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el archivo requirements.txt
COPY requirements.txt .

# Instalar dependencias
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código del proyecto al contenedor
COPY . .

# Exponer el puerto que la aplicación usará
EXPOSE 8000

# Ejecutar el servidor Django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

Este `Dockerfile` hace lo siguiente:
- Usa una imagen base de Python.
- Instala las dependencias de tu proyecto Django.
- Copia el código de tu aplicación al contenedor.
- Expone el puerto `8000` (el puerto predeterminado para Django).
- Inicia el servidor de desarrollo de Django.

**2. Crear un archivo `requirements.txt`**

El archivo `requirements.txt` debe contener todas las dependencias de Python necesarias para tu aplicación Django, por ejemplo:

```
Django>=3.2,<4.0
psycopg2>=2.9,<3.0
```

**3. Construir y ejecutar el contenedor**

Ahora que tienes el `Dockerfile` y el archivo `requirements.txt`, construye la imagen Docker con:

```bash
docker build -t django-app .
```

Una vez que la imagen esté construida, ejecuta el contenedor con:

```bash
docker run -d -p 8000:8000 --name django-container django-app
```

Esto ejecutará tu aplicación Django dentro de un contenedor y mapeará el puerto `8000` del contenedor al puerto `8000` de tu máquina local.

#### **Ejecutar una aplicación Flask en Docker**

Al igual que con Django, para ejecutar una aplicación Flask en Docker, necesitas crear un `Dockerfile`. Aquí tienes un ejemplo básico:

**1. Crear un archivo `Dockerfile` para Flask**

```dockerfile
# Usa una imagen oficial de Python
FROM python:3.9

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el archivo requirements.txt
COPY requirements.txt .

# Instalar dependencias
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código de la aplicación al contenedor
COPY . .

# Exponer el puerto que la aplicación usará
EXPOSE 5000

# Ejecutar la aplicación Flask
CMD ["python", "app.py"]
```

Este `Dockerfile` es similar al de Django, pero para una aplicación Flask. Asegúrate de tener un archivo `requirements.txt` con las dependencias de Flask, por ejemplo:

```
Flask==2.0.1
```

**2. Construir y ejecutar el contenedor**

Construye la imagen Flask:

```bash
docker build -t flask-app .
```

Y luego ejecuta el contenedor:

```bash
docker run -d -p 5000:5000 --name flask-container flask-app
```

Esto ejecutará tu aplicación Flask dentro del contenedor y mapeará el puerto `5000` del contenedor al puerto `5000` de tu máquina local.

---

### Cómo gestionar variables de entorno y configuraciones

Las aplicaciones web, como Django y Flask, a menudo requieren configuraciones específicas que varían entre entornos de desarrollo, prueba y producción. Docker te permite gestionar estas configuraciones y variables de entorno de manera sencilla.

#### **Definir variables de entorno en el `Dockerfile`**

Puedes definir variables de entorno dentro del `Dockerfile` utilizando la instrucción `ENV`. Por ejemplo, en una aplicación Django, puedes definir la variable `DEBUG` de la siguiente manera:

```dockerfile
ENV DEBUG=False
```

Esto establecerá la variable de entorno `DEBUG` con el valor `False` dentro del contenedor.

#### **Pasar variables de entorno al ejecutar el contenedor**

Si prefieres no definir las variables de entorno directamente en el `Dockerfile`, puedes pasarlas al momento de ejecutar el contenedor utilizando la opción `-e`:

```bash
docker run -d -p 8000:8000 -e DEBUG=True --name django-container django-app
```

Este comando pasará la variable de entorno `DEBUG=True` al contenedor cuando se ejecute. Luego, tu aplicación Django o Flask puede leer estas variables de entorno en su código.

**En Django**, por ejemplo, puedes acceder a la variable `DEBUG` en tu archivo `settings.py`:

```python
import os
DEBUG = os.environ.get('DEBUG', 'False') == 'True'
```

**En Flask**, puedes hacerlo en tu archivo `app.py` o donde configures la aplicación:

```python
import os
app.config['DEBUG'] = os.environ.get('DEBUG', 'False') == 'True'
```

#### **Usar archivos `.env` para gestionar configuraciones**

Si prefieres gestionar configuraciones más complejas o un conjunto de variables de entorno, puedes usar un archivo `.env`. Este archivo contiene pares clave-valor que se cargan al contenedor. Aquí tienes un ejemplo:

**Archivo `.env`**:

```
DEBUG=True
SECRET_KEY=mysecretkey
DATABASE_URL=postgres://user:password@localhost/dbname
```

Luego, puedes cargar este archivo en tu contenedor utilizando la opción `--env-file`:

```bash
docker run --env-file .env -d -p 8000:8000 --name django-container django-app
```

Esto cargará todas las variables definidas en el archivo `.env` en el contenedor.

---

### Resumen de Comandos para Ejecutar Aplicaciones Web en Docker

- **`docker build -t <nombre_imagen> .`**: Construir una imagen Docker a partir del `Dockerfile`.
- **`docker run -d -p <puerto_local>:<puerto_contenedor> --name <nombre_contenedor> <nombre_imagen>`**: Ejecutar un contenedor en segundo plano.
- **`docker exec -it <nombre_contenedor> bash`**: Acceder a un contenedor en ejecución para interactuar con él.
- **`docker run -e <variable>=<valor> ...`**: Pasar variables de entorno al contenedor al ejecutarlo.
- **`docker run --env-file .env ...`**: Cargar variables de entorno desde un archivo `.env`.

---

## Gestión Avanzada de Contenedores

### Uso de múltiples contenedores simultáneamente (docker-compose)

Cuando se gestionan aplicaciones que requieren varios servicios (como una aplicación web que necesita una base de datos y un servidor de caché), Docker **Compose** es una herramienta muy útil. **Docker Compose** te permite definir y ejecutar aplicaciones multicontenedor en Docker, facilitando la gestión de servicios relacionados.

#### **¿Qué es Docker Compose?**

**Docker Compose** es una herramienta para definir y ejecutar aplicaciones Docker de múltiples contenedores. Utilizando un archivo YAML (generalmente `docker-compose.yml`), puedes configurar todos los servicios que necesita tu aplicación, como bases de datos, servidores web, colas de mensajes, entre otros.

#### **Ejemplo básico de `docker-compose.yml`**

Imagina que tienes una aplicación web Django que usa PostgreSQL como base de datos. El archivo `docker-compose.yml` podría ser algo como esto:

```yaml
version: '3'
services:
  web:
    image: django-app
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    depends_on:
      - db
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Este archivo define dos servicios:
1. **web**: La aplicación Django, que se construye desde el `Dockerfile` local y se expone en el puerto 8000.
2. **db**: Un contenedor con PostgreSQL, que usa un volumen llamado `postgres_data` para persistir los datos.

#### **Comandos básicos de Docker Compose**

Una vez que hayas definido tu archivo `docker-compose.yml`, puedes usar los siguientes comandos para gestionar tus contenedores:

- **Construir y ejecutar contenedores**:

```bash
docker-compose up --build
```

Esto construye las imágenes (si es necesario) y arranca los contenedores definidos en el archivo `docker-compose.yml`.

- **Detener los contenedores**:

```bash
docker-compose down
```

Este comando detiene y elimina todos los contenedores y redes definidos en el archivo `docker-compose.yml`.

- **Ver los contenedores activos**:

```bash
docker-compose ps
```

Este comando muestra todos los contenedores activos que Docker Compose ha gestionado.

- **Escalar servicios**:

```bash
docker-compose up --scale web=3
```

Este comando escala el servicio `web` a 3 contenedores. Esto es útil si necesitas distribuir la carga de trabajo entre múltiples instancias de un servicio.

---

### Creación y gestión de redes de contenedores

Docker permite que los contenedores se comuniquen entre sí a través de redes. Puedes crear y gestionar redes para facilitar la comunicación entre contenedores.

#### **Redes predeterminadas de Docker**

Docker crea tres redes predeterminadas:
- **bridge**: Una red predeterminada para contenedores que no especifican una red.
- **host**: Usa la red de la máquina anfitriona.
- **none**: No se conecta a ninguna red.

#### **Crear una red personalizada**

Para crear una red personalizada, puedes usar el siguiente comando:

```bash
docker network create --driver bridge mi_red
```

Esto crea una red personalizada llamada `mi_red` usando el controlador `bridge`, que es el controlador predeterminado.

#### **Conectar contenedores a redes**

Puedes conectar un contenedor a una red personalizada usando la opción `--network` al ejecutar el contenedor:

```bash
docker run -d --name contenedor1 --network mi_red imagen
```

Esto conecta el contenedor `contenedor1` a la red `mi_red`. Los contenedores conectados a la misma red podrán comunicarse entre sí.

#### **Ver las redes disponibles**

Para listar todas las redes disponibles, usa:

```bash
docker network ls
```

#### **Conectar un contenedor existente a una red**

Si un contenedor ya está en ejecución y quieres conectarlo a una red adicional, usa el siguiente comando:

```bash
docker network connect mi_red contenedor1
```

Esto conecta el contenedor `contenedor1` a la red `mi_red`.

#### **Eliminar una red**

Si ya no necesitas una red, puedes eliminarla con:

```bash
docker network rm mi_red
```

---

### Volúmenes y persistencia de datos

Los volúmenes son cruciales cuando se trata de persistir datos en Docker. Los contenedores son efímeros por naturaleza, lo que significa que los datos dentro de un contenedor se pierden cuando el contenedor se detiene o elimina. Los volúmenes proporcionan una solución para mantener los datos a salvo de la destrucción de los contenedores.

#### **Crear un volumen de Docker**

Puedes crear un volumen de Docker utilizando el siguiente comando:

```bash
docker volume create mi_volumen
```

Este comando crea un volumen llamado `mi_volumen` que se puede usar para almacenar datos persistentes.

#### **Montar un volumen en un contenedor**

Al ejecutar un contenedor, puedes montar un volumen utilizando la opción `-v`:

```bash
docker run -d -v mi_volumen:/data --name contenedor1 imagen
```

Esto monta el volumen `mi_volumen` en el directorio `/data` del contenedor, asegurando que los datos dentro de ese directorio se persistan más allá de la vida del contenedor.

#### **Ver los volúmenes disponibles**

Para ver todos los volúmenes disponibles en tu sistema Docker, usa:

```bash
docker volume ls
```

#### **Eliminar un volumen**

Si ya no necesitas un volumen, puedes eliminarlo con:

```bash
docker volume rm mi_volumen
```

> **Nota**: Asegúrate de que el volumen no esté siendo utilizado por ningún contenedor antes de eliminarlo.

#### **Usar volúmenes con `docker-compose`**

Si estás utilizando **Docker Compose**, puedes definir volúmenes directamente en el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  web:
    image: django-app
    volumes:
      - pgdata:/app/data
  db:
    image: postgres
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

Esto define un volumen llamado `pgdata` y lo monta en el contenedor de la base de datos `db` para persistir los datos, así como en el contenedor de la aplicación `web`.

---

### Resumen de Comandos para Gestión Avanzada de Contenedores

- **`docker-compose up --build`**: Construir y ejecutar los contenedores definidos en `docker-compose.yml`.
- **`docker-compose down`**: Detener y eliminar todos los contenedores gestionados por Docker Compose.
- **`docker-compose ps`**: Ver los contenedores activos gestionados por Docker Compose.
- **`docker network create --driver bridge mi_red`**: Crear una red personalizada.
- **`docker run --network mi_red ...`**: Conectar un contenedor a una red personalizada al ejecutarlo.
- **`docker volume create mi_volumen`**: Crear un volumen para persistencia de datos.
- **`docker run -v mi_volumen:/data ...`**: Montar un volumen en un contenedor al ejecutarlo.
- **`docker network ls`**: Listar todas las redes disponibles en Docker.
- **`docker volume ls`**: Listar todos los volúmenes disponibles en Docker.
- **`docker volume rm mi_volumen`**: Eliminar un volumen.

---

## Buenas Prácticas y Seguridad

### Principios de seguridad en Docker

La seguridad en Docker es fundamental, ya que los contenedores pueden ser vulnerables si no se gestionan adecuadamente. A continuación, se mencionan algunas buenas prácticas para asegurar que los contenedores sean lo más seguros posible.

#### **1. Uso de imágenes oficiales y verificadas**

Siempre que sea posible, utiliza imágenes oficiales y verificadas de Docker Hub. Estas imágenes suelen estar más actualizadas y han sido revisadas por la comunidad y los mantenedores del proyecto. Si creas tus propias imágenes, asegúrate de seguir prácticas seguras en la creación de Dockerfiles.

**Ejemplo**:
```bash
docker pull postgres
```

#### **2. Mantén las imágenes y contenedores actualizados**

Es importante mantener tus imágenes Docker actualizadas, ya que las nuevas versiones incluyen correcciones de seguridad importantes. Utiliza los siguientes comandos para actualizar tus imágenes:

```bash
docker pull <nombre_imagen>
```

Para actualizar los contenedores en ejecución, elimina el contenedor antiguo y ejecuta uno nuevo basado en la versión más reciente de la imagen.

#### **3. Evitar ejecutar contenedores como root**

Evita ejecutar contenedores con privilegios de root, ya que esto podría dar acceso completo a tu sistema en caso de vulnerabilidades en el contenedor. Utiliza la opción `USER` en el `Dockerfile` para especificar un usuario no privilegiado.

**Ejemplo en el Dockerfile**:

```dockerfile
FROM python:3.9
# Crear un usuario no root
RUN useradd -m appuser
USER appuser
```

#### **4. Limitar los permisos del contenedor**

Puedes limitar los permisos de los contenedores utilizando el parámetro `--cap-drop` para reducir la cantidad de capacidades del contenedor.

**Ejemplo**:

```bash
docker run --cap-drop=ALL my-container
```

#### **5. Usar redes privadas para comunicación entre contenedores**

Utiliza redes privadas de Docker para limitar la visibilidad de los contenedores y restringir el acceso solo a los servicios necesarios. Puedes crear redes personalizadas con el siguiente comando:

```bash
docker network create --driver bridge mi_red
```

#### **6. Auditar contenedores con herramientas de seguridad**

Existen herramientas como **Clair**, **Trivy** y **Docker Bench** para auditar imágenes de Docker en busca de vulnerabilidades de seguridad. Estas herramientas te ayudarán a identificar posibles problemas en tus imágenes y contenedores.

---

### Cómo optimizar contenedores para mayor eficiencia

La eficiencia de los contenedores puede ser crucial cuando se ejecutan en producción o en entornos con recursos limitados. Aquí te dejamos algunas buenas prácticas para optimizar el rendimiento de los contenedores.

#### **1. Optimizar el tamaño de las imágenes**

Las imágenes más pequeñas tienen menos dependencias y son más rápidas de transferir, lo que mejora el tiempo de despliegue y reduce la superficie de ataque. Algunas recomendaciones para reducir el tamaño de las imágenes son:

- Utilizar imágenes base ligeras, como **alpine** en lugar de **debian** o **ubuntu**.
- Utilizar la instrucción `COPY` en lugar de `ADD` en el `Dockerfile`, ya que es más eficiente.
- Eliminar archivos y dependencias no necesarias durante el proceso de construcción con instrucciones como `RUN apt-get clean`.

**Ejemplo de Dockerfile optimizado**:

```dockerfile
FROM python:3.9-alpine
WORKDIR /app
COPY . .
RUN pip install --no-cache-dir -r requirements.txt
```

#### **2. Usar capas de caché efectivas**

Docker utiliza una caché para las capas de las imágenes, lo que puede hacer que las construcciones sean más rápidas. Asegúrate de estructurar tu `Dockerfile` de tal manera que las capas que cambian con menos frecuencia (como las dependencias) estén al principio, y las capas que cambian más a menudo (como el código fuente) estén al final.

**Ejemplo**:

```dockerfile
# Primero instala las dependencias
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Luego copia el código fuente
COPY . .
```

#### **3. Limitar los recursos de los contenedores**

Puedes limitar los recursos que un contenedor puede usar, como la CPU y la memoria, para evitar que consuma demasiados recursos. Utiliza las opciones `--memory` y `--cpus` al ejecutar el contenedor.

**Ejemplo**:

```bash
docker run --memory="500m" --cpus="1.0" my-container
```

Esto limita el contenedor a 500 MB de memoria y 1 CPU.

---

### Estrategias de backup y restauración de datos en contenedores

Los contenedores son efímeros, lo que significa que los datos dentro de un contenedor se perderán cuando el contenedor se detenga o se elimine. Es crucial usar volúmenes para almacenar datos persistentes y tener una estrategia de respaldo.

#### **1. Usar volúmenes de Docker para persistencia de datos**

Los volúmenes son la mejor manera de garantizar la persistencia de los datos en Docker. Los volúmenes existen independientemente de la vida útil de los contenedores y se pueden compartir entre varios contenedores.

**Ejemplo de creación y uso de volúmenes**:

```bash
docker volume create my_volume
docker run -v my_volume:/data my-container
```

Esto monta el volumen `my_volume` en el directorio `/data` dentro del contenedor.

#### **2. Crear copias de seguridad de volúmenes**

Para crear un respaldo de un volumen, puedes crear un contenedor temporal que copie los datos del volumen a un archivo comprimido.

**Ejemplo de comando para hacer una copia de seguridad de un volumen**:

```bash
docker run --rm -v my_volume:/volume -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /volume .
```

Este comando crea una copia de seguridad del volumen `my_volume` en un archivo `backup.tar.gz`.

#### **3. Restaurar datos desde un respaldo**

Para restaurar los datos desde un respaldo, puedes usar el siguiente comando:

```bash
docker run --rm -v my_volume:/volume -v $(pwd):/backup alpine tar xzf /backup/backup.tar.gz -C /volume
```

Este comando restaura los datos del archivo `backup.tar.gz` en el volumen `my_volume`.

#### **4. Respaldar contenedores completos**

Si necesitas hacer un respaldo de un contenedor completo, puedes crear una imagen del contenedor en ejecución:

```bash
docker commit my-container my-backup-image
```

Luego, puedes guardar esta imagen como un archivo tar con:

```bash
docker save -o my-backup-image.tar my-backup-image
```

Para restaurar, puedes cargar la imagen con:

```bash
docker load -i my-backup-image.tar
```

---

### Resumen de Comandos para Buenas Prácticas y Seguridad

- **`docker pull <imagen>`**: Descargar una imagen oficial y verificada desde Docker Hub.
- **`docker run --user <usuario> ...`**: Ejecutar un contenedor con un usuario no privilegiado.
- **`docker run --cap-drop=ALL ...`**: Limitar las capacidades de un contenedor.
- **`docker build -t <nombre_imagen> .`**: Construir una imagen optimizada.
- **`docker run --memory=<cantidad> --cpus=<cantidad> ...`**: Limitar los recursos de un contenedor.
- **`docker volume create <nombre_volumen>`**: Crear un volumen para persistencia de datos.
- **`docker run -v <volumen>:<ruta> ...`**: Montar un volumen en un contenedor.
- **`docker run --rm -v <volumen>:<ruta> -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz ...`**: Crear un respaldo de un volumen.
- **`docker commit <contenedor> <imagen>`**: Crear una imagen de un contenedor.
- **`docker save -o <archivo.tar> <imagen>`**: Guardar una imagen como un archivo tar.
- **`docker load -i <archivo.tar>`**: Restaurar una imagen desde un archivo tar.

---

## Recursos Adicionales

### Documentación oficial de Docker

La **documentación oficial de Docker** es el mejor lugar para profundizar en todos los aspectos de Docker, desde la instalación hasta los temas más avanzados como redes y orquestación de contenedores.

- **Documentación oficial de Docker**:  
  [https://docs.docker.com/](https://docs.docker.com/)

La documentación está organizada en secciones fáciles de seguir, y cubre tanto Docker Engine como Docker Compose, además de tutoriales, guías de referencia y más.

#### **Secciones importantes de la documentación**:

- **Guía de inicio rápido**:  
  [Guía de inicio rápido](https://docs.docker.com/get-started/) para comenzar con Docker de manera rápida y sencilla.
  
- **Referencias de comandos**:  
  [Docker CLI Reference](https://docs.docker.com/engine/reference/commandline/docker/) para explorar todos los comandos disponibles.

- **Docker Compose**:  
  [Docker Compose Documentation](https://docs.docker.com/compose/) para gestionar aplicaciones multicontenedor.

- **Seguridad en Docker**:  
  [Docker Security](https://docs.docker.com/engine/security/) para aprender cómo asegurar tus contenedores y su infraestructura.

### Enlaces útiles y tutoriales recomendados

Aquí tienes algunos enlaces y recursos que te ayudarán a profundizar más en Docker, además de tutoriales y ejemplos prácticos.

#### **Tutoriales y cursos recomendados**:

- **Docker Official Tutorials**:  
  [Tutoriales oficiales de Docker](https://docs.docker.com/get-started/) para aprender paso a paso sobre Docker.

- **Docker para principiantes en YouTube**:  
  [Docker for Beginners - YouTube](https://www.youtube.com/playlist?list=PLV5gE1f5rTe26fPU0B6mrO8X8_3FwHDGJ) - Canal oficial de Docker para principiantes.

- **FreeCodeCamp: Aprende Docker**:  
  [Aprende Docker de manera gratuita](https://www.freecodecamp.org/news/learn-docker-for-beginners/) - Un excelente recurso para aprender Docker desde cero.

- **Docker para desarrolladores de Python**:  
  [Docker para Python Developers](https://realpython.com/docker-python/) - Aprende cómo usar Docker con tus proyectos Python.

#### **Herramientas adicionales**:

- **Portainer**:  
  [Portainer.io](https://www.portainer.io/) - Una interfaz gráfica para gestionar tus contenedores Docker de manera más sencilla.

- **Watchtower**:  
  [Watchtower](https://github.com/containrrr/watchtower) - Herramienta para actualizar automáticamente tus contenedores.

- **Docker Desktop**:  
  [Docker Desktop](https://www.docker.com/products/docker-desktop) - Herramienta para gestionar Docker en entornos de desarrollo local, tanto en Mac como Windows.

- **Docker Hub**:  
  [Docker Hub](https://hub.docker.com/) - El lugar donde puedes encontrar millones de imágenes de contenedores listas para usar.

---

### Comunidades y Foros

Si necesitas ayuda o quieres aprender de otros usuarios, estas comunidades y foros pueden ser muy útiles:

- **Docker Community Forums**:  
  [Foros de la comunidad de Docker](https://forums.docker.com/) - Para preguntas y respuestas, y soporte técnico.
  
- **Stack Overflow - Docker**:  
  [Stack Overflow Docker](https://stackoverflow.com/questions/tagged/docker) - Una gran comunidad de desarrolladores con respuestas rápidas a preguntas frecuentes.

- **Reddit Docker**:  
  [Subreddit de Docker](https://www.reddit.com/r/docker/) - Un lugar activo para compartir experiencias y aprender de otros.

- **Docker Slack**:  
  [Docker Community Slack](https://www.docker.com/slack) - Únete a la comunidad de Docker en Slack para estar al tanto de las novedades y obtener ayuda en tiempo real.

---

### Libros recomendados sobre Docker

Si prefieres aprender con libros, aquí tienes algunas recomendaciones:

- **"Docker Deep Dive" de Nigel Poulton**:  
  Un libro completo sobre Docker que cubre desde lo básico hasta lo avanzado.

- **"The Docker Book" de James Turnbull**:  
  Un libro fácil de seguir para aprender a usar Docker en proyectos reales.

- **"Docker in Action" de Jeffrey Nickoloff**:  
  Este libro cubre Docker desde una perspectiva práctica con ejemplos de uso.

---

### Resumen

Aquí tienes una lista rápida de recursos clave para seguir aprendiendo sobre Docker:

- [Documentación oficial de Docker](https://docs.docker.com/)
- [Tutoriales de Docker](https://www.docker.com/resources/what-container)
- [Docker for Beginners - YouTube](https://www.youtube.com/playlist?list=PLV5gE1f5rTe26fPU0B6mrO8X8_3FwHDGJ)
- [Portainer.io](https://www.portainer.io/)
- [Docker Hub](https://hub.docker.com/)


