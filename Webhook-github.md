# Guía Completa para Implementar Webhooks en AlmaLinux para Descargar Repositorios Automáticamente desde GitHub tras un Push

## Tabla de Contenidos

1. [¿Qué es un Webhook?](#que-es-un-webhook)
2. [Requisitos Previos](#requisitos-previos)
3. [Configuración del Webhook en GitHub](#configuracion-del-webhook-en-github)
4. [Automatización del Proceso de Descarga en una Carpeta Específica](#automatizacion-del-proceso-de-descarga)
5. [Configuración del Servidor con AlmaLinux para Recibir el Webhook](#configuracion-del-servidor-con-almalinux-para-recibir-el-webhook)
6. [Seguridad del Webhook](#seguridad-del-webhook)
7. [Solución de Problemas](#solucion-de-problemas)
8. [Conclusión](#conclusion)

---

## 1. ¿Qué es un Webhook?

Un **Webhook** es un mecanismo que permite a las aplicaciones enviar notificaciones a otras aplicaciones cuando ocurre un evento específico, como un **push** en GitHub. En lugar de que tu servidor revise constantemente si hay cambios, GitHub envía una solicitud automáticamente a tu servidor cada vez que alguien realiza un **push**. Esto permite automatizar tareas como la actualización de código en un servidor en cuanto se hace un push al repositorio.

---

## 2. Requisitos Previos

### Servidor con AlmaLinux
Necesitarás un servidor con **AlmaLinux** instalado y acceso SSH con permisos para instalar y configurar software.

### Instalación de Git
Git debe estar instalado para poder clonar y actualizar los repositorios. Si no tienes Git instalado, puedes hacerlo con:

```bash
sudo dnf install git
```

### Repositorio en GitHub
Debes tener un repositorio activo en GitHub donde configurarás el webhook para enviar notificaciones a tu servidor.

---

## 3. Configuración del Webhook en GitHub

### Pasos para Configurar el Webhook

1. **Accede a tu repositorio en GitHub**.
2. Ve a la pestaña **Settings**.
3. En el menú lateral izquierdo, selecciona **Webhooks**.
4. Haz clic en **Add Webhook**.
5. Completa los campos:
   - **Payload URL**: Esta es la URL de tu servidor que recibirá las notificaciones de GitHub. Ejemplo: `http://tu-servidor.com/webhook`.
   - **Content Type**: Selecciona `application/json` para que el payload se envíe en formato JSON.
   - **Secret**: (Opcional) Puedes agregar un secret para validar la autenticidad del webhook.
   - **Which events would you like to trigger this webhook?**: Selecciona **Push events**.

6. Guarda los cambios. Ahora, cada vez que hagas un **push** a tu repositorio, GitHub enviará una solicitud POST a la URL que has configurado.

---

## 4. Automatización del Proceso de Descarga en una Carpeta Específica 

### Creación del Script en Bash

Vamos a crear un script en **Bash** que descargue o actualice automáticamente el repositorio en una carpeta específica en tu servidor cada vez que se reciba un webhook.

#### Script `git_pull.sh`:

```bash
#!/bin/bash

# Ruta donde se descargará el repositorio
REPO_DIR="/ruta/a/tu/carpeta/especifica"
BRANCH="main"

# Verifica si la carpeta del repositorio existe, si no, clona el repositorio
if [ ! -d "$REPO_DIR" ]; then
    git clone https://github.com/usuario/repositorio.git $REPO_DIR
fi

# Cambia al directorio del repositorio
cd $REPO_DIR || exit

# Actualiza el repositorio con los últimos cambios
git fetch origin $BRANCH
git reset --hard origin/$BRANCH

# Opcional: Reiniciar servicios si es necesario (Ej. Nginx o Gunicorn)
# sudo systemctl restart nginx
# sudo systemctl restart gunicorn
```

### Hacer ejecutable el script

Guarda este script en el servidor en el directorio `/usr/local/bin/git_pull.sh` y asegúrate de que sea ejecutable con:

```bash
chmod +x /usr/local/bin/git_pull.sh
```

Este script hará un **pull** automático del código cuando se reciba el webhook. Si el repositorio no está clonado, lo hará automáticamente, y si ya está clonado, actualizará la carpeta a la última versión del código.

---

## 5. Configuración del Servidor con AlmaLinux para Recibir el Webhook 

Para recibir el webhook en el servidor, configuraremos un servidor web ligero usando **Flask**, que escuchará en un puerto específico (por ejemplo, el puerto 5000) para las solicitudes POST de GitHub.

### Instalación de Flask

Primero, instala **Flask** en tu servidor:

```bash
pip install Flask
```

### Crear el servidor Flask

A continuación, crea un archivo llamado `webhook.py` que contendrá el código de tu servidor Flask:

```python
from flask import Flask, request, abort
import subprocess

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def webhook():
    if request.method == 'POST':
        # Ejecuta el script para hacer el pull del repositorio
        subprocess.call(['/usr/local/bin/git_pull.sh'])
        return 'Webhook received and executed!', 200
    else:
        abort(400)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Ejecutar Flask como un Servicio

Para que Flask esté corriendo continuamente en segundo plano, lo configuraremos como un servicio de **systemd**.

1. Crea un archivo de servicio en `/etc/systemd/system/flask-webhook.service`:

```ini
[Unit]
Description=Webhook Flask App
After=network.target

[Service]
User=tu-usuario
WorkingDirectory=/ruta/a/tu/proyecto
ExecStart=/usr/bin/python3 /ruta/a/tu/proyecto/webhook.py
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Habilita y ejecuta el servicio con los siguientes comandos:

```bash
sudo systemctl daemon-reload
sudo systemctl start flask-webhook
sudo systemctl enable flask-webhook
```

Este servicio ejecutará Flask automáticamente cada vez que se inicie el servidor.

### Abrir el Puerto 5000 en el Firewall

Asegúrate de que el puerto 5000 esté abierto en el firewall de tu servidor:

```bash
sudo firewall-cmd --zone=public --add-port=5000/tcp --permanent
sudo firewall-cmd --reload
```

---

## 6. Seguridad del Webhook 

### Uso de un Secret

Para asegurar que solo GitHub pueda activar tu webhook, puedes configurar un **secret** tanto en GitHub como en tu servidor. Esto permite verificar que las solicitudes POST provienen de una fuente confiable.

En el archivo `webhook.py`, puedes agregar la validación de la firma del webhook utilizando HMAC:

```python
import hashlib
import hmac
import os

@app.route('/webhook', methods=['POST'])
def webhook():
    secret = os.getenv('WEBHOOK_SECRET').encode('utf-8')
    signature = 'sha1=' + hmac.new(secret, request.data, hashlib.sha1).hexdigest()

    if hmac.compare_digest(signature, request.headers.get('X-Hub-Signature')):
        subprocess.call(['/usr/local/bin/git_pull.sh'])
        return 'Webhook received and verified!', 200
    else:
        abort(403)
```

No olvides configurar la variable de entorno `WEBHOOK_SECRET` en tu servidor con el mismo secret que configuraste en GitHub.

---

## 7. Solución de Problemas 

### 1. El Webhook no se activa

- **Causa:** El puerto 5000 está bloqueado.
- **Solución:** Revisa la configuración del firewall para asegurarte de que el puerto esté abierto.

### 2. Error de permisos en el script

- **Causa:** El usuario que ejecuta Flask no tiene los permisos adecuados para acceder a la carpeta del repositorio.
- **Solución:** Asegúrate de que el usuario tenga permisos de lectura/escritura en la carpeta del repositorio.

### 3. El servicio Flask no se inicia

- **Causa:** Errores en la configuración del servicio systemd.
- **Solución:** Revisa el archivo del servicio para asegurarte de que las rutas y permisos sean correctos.

---

## 8. Conclusión 

Has configurado con éxito un sistema automatizado en tu servidor AlmaLinux que descarga y actualiza el repositorio automáticamente cada vez que haces un push a GitHub, simulando el comportamiento de Heroku. Este sistema es ideal para despliegues rápidos y automáticos sin intervención manual.


