## Uso de un disco externo como almacenamiento de archivos en Django sobre AlmaLinux

---

### Tabla de contenidos

- [Uso de un disco externo como almacenamiento de archivos en Django sobre AlmaLinux](#uso-de-un-disco-externo-como-almacenamiento-de-archivos-en-django-sobre-almalinux)
  - [Tabla de contenidos](#tabla-de-contenidos)
- [1. Objetivo y escenario](#1-objetivo-y-escenario)
- [2. Preparar el disco externo en AlmaLinux](#2-preparar-el-disco-externo-en-almalinux)
  - [2.1 Detectar el disco](#21-detectar-el-disco)
  - [2.2 Usar el script XEMI Disk Helper como asistente](#22-usar-el-script-xemi-disk-helper-como-asistente)
  - [2.3 Formatear el disco a ext4](#23-formatear-el-disco-a-ext4)
  - [2.4 Montar el disco en una ruta estable](#24-montar-el-disco-en-una-ruta-estable)
  - [2.5 Hacer el montaje persistente con fstab](#25-hacer-el-montaje-persistente-con-fstab)
- [3. Permisos y estructura de carpetas para proyectos Django](#3-permisos-y-estructura-de-carpetas-para-proyectos-django)
- [4. Integración con Django](#4-integración-con-django)
  - [4.1 Configurar MEDIA\_ROOT y MEDIA\_URL](#41-configurar-media_root-y-media_url)
  - [4.2 Mini aplicación de subida de archivos de prueba](#42-mini-aplicación-de-subida-de-archivos-de-prueba)
    - [Crear la aplicación](#crear-la-aplicación)
    - [Modelo de ejemplo](#modelo-de-ejemplo)
    - [Formulario](#formulario)
    - [Vista](#vista)
    - [Rutas de la aplicación](#rutas-de-la-aplicación)
    - [Rutas principales](#rutas-principales)
    - [Plantilla sencilla](#plantilla-sencilla)
- [5. Seguridad y control de acceso a los archivos](#5-seguridad-y-control-de-acceso-a-los-archivos)
  - [5.1 Qué ocurre por defecto con MEDIA\_URL](#51-qué-ocurre-por-defecto-con-media_url)
  - [5.2 Desactivar la exposición pública de media](#52-desactivar-la-exposición-pública-de-media)
  - [5.3 Ejemplo de vista protegida por usuario](#53-ejemplo-de-vista-protegida-por-usuario)
- [6. Checklist para repetir este montaje en otro servidor](#6-checklist-para-repetir-este-montaje-en-otro-servidor)

---

## 1. Objetivo y escenario

Escenario típico

Tienes un servidor con AlmaLinux y proyectos Django, y quieres que
los archivos subidos por los usuarios se guarden en un disco externo
conectado por USB en lugar de ocupar espacio en el disco principal.

Además, quieres dos cosas importantes

* Que el disco se monte siempre en el arranque
* Que los archivos se gestionen desde Django con control de acceso

Ejemplo que se toma como referencia

* Disco externo montado en
  `/srv/cloud_data`
* Proyecto Django que guarda sus archivos en
  `/srv/cloud_data/cloudweb_media`

Todo lo que se describe se puede replicar en otros proyectos y otros servidores.

---

## 2. Preparar el disco externo en AlmaLinux

### 2.1 Detectar el disco

Conecta el disco por USB al servidor y ejecuta

```bash
lsblk
```

El objetivo es localizar algo como

```text
sdb      232G
└─sdb1   232G
```

Notas importantes

* Nunca se trabaja sobre el disco completo si ya tiene partición
  sino sobre la partición, por ejemplo `sdb1`
* Hay que identificar también el disco del sistema
  para no tocarlo por error, por ejemplo `sda` con su `sda1` montada en `/`

Opcionalmente

```bash
sudo blkid
```

sirve para ver tipos de sistema de archivos y `UUID` de cada partición.

---

### 2.2 Usar el script XEMI Disk Helper como asistente

El script XEMI Disk Helper sirve como asistente interactivo para

* Listar discos
* Formatear particiones que no sean adecuadas
* Montar una partición en la ruta elegida
* Añadir una entrada a `/etc/fstab` con copia de seguridad previa

Flujo típico con el script

1. Ejecutar

   ```bash
   ./xemi_disk_helper
   ```

2. Opción para listar discos y verificar que la partición correcta es por ejemplo `sdb1`

3. Opción para formatear o para montar, según el estado actual del disco

Si el disco viene de Windows con formato `ntfs`, el script puede detectar ese formato, explicar que no es ideal para servidor Linux y ofrecer una opción de reformateo rápido a `ext4`.

---

### 2.3 Formatear el disco a ext4

Para uso en servidor con Django la opción habitual es `ext4`.

Puede hacerse a mano o con el script. Versión manual

```bash
# Peligro, esto borra todo el contenido de la partición
sudo mkfs.ext4 -L CLOUD_DATA /dev/sdb1
```

Después del formateo

* Se crea una carpeta `lost+found` en la raíz del sistema de archivos
  que es normal en ext4
* Esta carpeta se usa para que `fsck` pueda colocar ahí fragmentos
  de archivos recuperados en caso de errores en disco

No hay que borrar `lost+found`, se ignora y se deja donde está.

---

### 2.4 Montar el disco en una ruta estable

Para almacenamiento de proyectos web es habitual usar rutas como

* `/srv/cloud_data`
* o `/mnt/cloud_data`

Por ejemplo

```bash
sudo mkdir -p /srv/cloud_data
sudo mount /dev/sdb1 /srv/cloud_data
```

Comprobación

```bash
df -h | grep /srv/cloud_data
ls -l /srv/cloud_data
```

Si aparece el volumen y se ve la carpeta `lost+found`, el montaje es correcto.

---

### 2.5 Hacer el montaje persistente con fstab

Si no se hace nada más, al reiniciar el servidor el disco deja de estar montado.

La forma estándar de hacer persistente el montaje es usar `/etc/fstab` y el `UUID`.

1. Obtener el `UUID`

   ```bash
   sudo blkid /dev/sdb1
   ```

   Ejemplo

   ```text
   /dev/sdb1: UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" TYPE="ext4" PARTUUID="..."
   ```

2. Editar `/etc/fstab` con copia de seguridad

   ```bash
   sudo cp /etc/fstab /etc/fstab.backup
   sudo nano /etc/fstab
   ```

3. Añadir una línea similar a

   ```text
   UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  /srv/cloud_data  ext4  defaults  0  0
   ```

4. Probar la configuración sin reiniciar

   ```bash
   sudo umount /srv/cloud_data
   sudo mount -a
   ```

   Si no hay errores y el disco vuelve a estar montado, la entrada es válida.

El script XEMI Disk Helper puede automatizar esta parte, haciendo copia de seguridad del `fstab`, añadiendo la entrada y probando `mount -a`.

---

## 3. Permisos y estructura de carpetas para proyectos Django

Una vez montado el disco en `/srv/cloud_data`, se organiza por proyectos.

Ejemplo para un proyecto Django llamado “cloudweb”

```bash
sudo mkdir -p /srv/cloud_data/cloudweb_media
sudo chown -R baker:baker /srv/cloud_data/cloudweb_media
sudo chmod -R 775 /srv/cloud_data/cloudweb_media
```

Notas

* `baker` sería el usuario de sistema que ejecuta Django y gunicorn
* Con permisos `775` el propietario y el grupo pueden leer y escribir

En un servidor con varias aplicaciones se pueden crear varias rutas

```text
/srv/cloud_data/cloudweb_media
/srv/cloud_data/portfolio_media
/srv/cloud_data/otro_proyecto_media
```

Cada proyecto apunta su `MEDIA_ROOT` a la carpeta correspondiente.

---

## 4. Integración con Django

### 4.1 Configurar MEDIA_ROOT y MEDIA_URL

En el archivo de configuración del proyecto, por ejemplo `_core/settings.py`

```python
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

# Archivos estáticos de proyecto, CSS y JavaScript por ejemplo
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / "static",
]

# Archivos subidos por usuarios
MEDIA_URL = '/media/'
MEDIA_ROOT = '/srv/cloud_data/cloudweb_media'
```

Con esa configuración ocurre lo siguiente

* Cuando un modelo tiene un `FileField` o `ImageField`, Django guarda los archivos físicamente dentro de la carpeta `MEDIA_ROOT`
* La ruta que se guarda en la base de datos es relativa a `MEDIA_ROOT`, por ejemplo `uploads/2025/11/archivo.pdf`
* La URL pública que se construye es `MEDIA_URL + ruta_relativa`, por ejemplo `/media/uploads/2025/11/archivo.pdf`

Durante desarrollo se suele añadir en `urls.py` algo como

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # otras rutas
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

Esto hace que el servidor de desarrollo de Django sirva los archivos de media de forma pública durante el desarrollo.

Para seguridad en producción se tratará este punto más adelante.

---

### 4.2 Mini aplicación de subida de archivos de prueba

Objetivo, comprobar que los archivos se almacenan en el disco externo.

#### Crear la aplicación

```bash
python manage.py startapp uploader
```

Registrar en `INSTALLED_APPS`

```python
INSTALLED_APPS = [
    # otras apps
    'uploader',
]
```

#### Modelo de ejemplo

`uploader/models.py`

```python
from django.db import models

class UploadedFile(models.Model):
    file = models.FileField(upload_to='uploads/%Y/%m/%d/')
    uploaded_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"{self.file.name} ({self.uploaded_at:%Y-%m-%d %H:%M})"
```

Con este modelo, al guardar un archivo se creará algo como

```text
/srv/cloud_data/cloudweb_media/uploads/2025/11/nombre_del_archivo
```

#### Formulario

`uploader/forms.py`

```python
from django import forms
from .models import UploadedFile

class UploadForm(forms.ModelForm):
    class Meta:
        model = UploadedFile
        fields = ['file']
```

#### Vista

`uploader/views.py`

```python
from django.shortcuts import render, redirect
from .forms import UploadForm
from .models import UploadedFile

def upload_file(request):
    if request.method == 'POST':
        form = UploadForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('upload_file')
    else:
        form = UploadForm()

    files = UploadedFile.objects.all().order_by('-uploaded_at')
    context = {
        'form': form,
        'files': files,
    }
    return render(request, 'uploader/upload.html', context)
```

#### Rutas de la aplicación

`uploader/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.upload_file, name='upload_file'),
]
```

#### Rutas principales

En el `urls.py` principal del proyecto

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('uploader/', include('uploader.urls')),
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

#### Plantilla sencilla

`uploader/templates/uploader/upload.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Upload test</title>
    <style>
        body { font-family: sans-serif; margin: 30px; background: #fafafa; }
        form { margin-bottom: 20px; }
        .file-list { background: #fff; border: 1px solid #ccc; padding: 15px; border-radius: 8px; }
        .file-item { margin: 5px 0; }
    </style>
</head>
<body>
    <h1>Upload test</h1>

    <form method="POST" enctype="multipart/form-data">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Upload</button>
    </form>

    <div class="file-list">
        <h3>Uploaded files</h3>
        {% for f in files %}
            <div class="file-item">
                <a href="{{ f.file.url }}">{{ f.file.name }}</a> — {{ f.uploaded_at }}
            </div>
        {% empty %}
            <p>No files uploaded yet.</p>
        {% endfor %}
    </div>
</body>
</html>
```

Después de arrancar el servidor de desarrollo y subir un archivo en la ruta `/uploader`, se debería ver el archivo físicamente dentro de

```text
/srv/cloud_data/cloudweb_media/uploads/...
```

Esto confirma que Django está usando el disco externo.

---

## 5. Seguridad y control de acceso a los archivos

### 5.1 Qué ocurre por defecto con MEDIA_URL

En desarrollo, cuando se añade en `urls.py`

```python
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

el servidor de desarrollo de Django expone todo el contenido de `MEDIA_ROOT` de forma pública.

Esto significa que si alguien conoce la URL completa de un archivo de media, puede acceder directamente a él, sin autenticación y sin pasar por ninguna vista.

En producción, con un servidor como Nginx, se puede configurar algo parecido a

```nginx
location /media/ {
    alias /srv/cloud_data/cloudweb_media/;
}
```

y se obtiene exactamente el mismo comportamiento, acceso público a los archivos si se conoce la ruta.

Para un cloud privado o archivos que deben ser visibles solo para usuarios con sesión iniciada y permisos adecuados, no es lo que se quiere.

---

### 5.2 Desactivar la exposición pública de media

La idea general es

* Mantener `MEDIA_ROOT` apuntando al disco externo
* No exponer directamente `MEDIA_URL` desde Django ni desde Nginx
* Servir archivos a través de vistas que comprueban permisos

En desarrollo, para empezar a pensar en el modelo seguro se puede

1. Comentar la línea de `static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)`

   ```python
   # if settings.DEBUG:
   #     urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
   ```

2. Aceptar que solo las vistas que se creen de forma explícita serán capaces de devolver archivos al navegador

En producción, no se configura `/media` como alias directo en Nginx para el contenido privado.
En su lugar, se usan vistas que devuelven el archivo con control de permisos.

Se puede incluso separar

* Contenido público
  `PUBLIC_MEDIA_ROOT` servido por Nginx
* Contenido privado
  `PROTECTED_MEDIA_ROOT` servido por vistas protegidas en Django

---

### 5.3 Ejemplo de vista protegida por usuario

Modelo de archivo privado con propietario

`files/models.py`

```python
from django.conf import settings
from django.db import models

class PrivateFile(models.Model):
    owner = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        related_name='private_files'
    )
    file = models.FileField(upload_to='private/%Y/%m/%d/')
    uploaded_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.file.name
```

Vista que comprueba autenticación y propiedad

`files/views.py`

```python
from django.contrib.auth.decorators import login_required
from django.http import FileResponse, Http404
from django.shortcuts import get_object_or_404
from .models import PrivateFile

@login_required
def download_private_file(request, pk):
    obj = get_object_or_404(PrivateFile, pk=pk)

    # Comprobar que el usuario es el dueño
    if obj.owner != request.user:
        raise Http404("File not found")

    # Abrir el archivo desde el FileField
    file_handle = obj.file.open('rb')
    response = FileResponse(file_handle, as_attachment=True)
    return response
```

Rutas

`files/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('download/<int:pk>/', views.download_private_file, name='download_private_file'),
]
```

En este esquema

* El archivo se guarda igualmente en `MEDIA_ROOT`
* No se expone `MEDIA_URL` directamente en `urls.py` de desarrollo ni en Nginx
* El usuario solo puede descargar el archivo si está autenticado y es el propietario

Se puede ampliar este enfoque para compartir archivos mediante tokens y enlaces temporales, pero la base es siempre la misma idea, no exponer el directorio de media de forma pública y servir el archivo desde una vista con reglas de seguridad.

---

## 6. Checklist para repetir este montaje en otro servidor

Resumen de pasos prácticos para otro servidor AlmaLinux con un proyecto Django nuevo o distinto.

1. Conectar disco externo y detectar la partición

   ```bash
   lsblk
   sudo blkid
   ```

2. Formatear a ext4 si es necesario

   ```bash
   sudo mkfs.ext4 -L NOMBRE_LABEL /dev/sdX1
   ```

3. Crear punto de montaje estable

   ```bash
   sudo mkdir -p /srv/cloud_data
   sudo mount /dev/sdX1 /srv/cloud_data
   ```

4. Hacer persistente el montaje con `/etc/fstab` usando `UUID`

   ```bash
   sudo blkid /dev/sdX1
   sudo cp /etc/fstab /etc/fstab.backup
   sudo nano /etc/fstab
   # Añadir línea con UUID y /srv/cloud_data
   sudo umount /srv/cloud_data
   sudo mount -a
   ```

5. Crear carpeta de media para el proyecto Django concreto

   ```bash
   sudo mkdir -p /srv/cloud_data/<nombre_proyecto>_media
   sudo chown -R <usuario_django>:<grupo> /srv/cloud_data/<nombre_proyecto>_media
   sudo chmod -R 775 /srv/cloud_data/<nombre_proyecto>_media
   ```

6. Configurar `MEDIA_ROOT` y `MEDIA_URL` en `settings.py`

   ```python
   MEDIA_URL = '/media/'
   MEDIA_ROOT = '/srv/cloud_data/<nombre_proyecto>_media'
   ```

7. Crear una mini aplicación de subida de prueba o integrar un `FileField` en modelos existentes para comprobar que los archivos se guardan en el disco externo

8. Durante desarrollo, se puede usar `static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)` para probar rápidamente, siendo consciente de que la media es pública

9. Para un sistema de cloud privado y producción, no exponer directamente `MEDIA_URL` y servir los archivos con vistas protegidas que comprueban autenticación y permisos

Con esta guía se puede repetir la misma idea en cualquier proyecto, tanto para un cloud de archivos de usuarios como para una aplicación web que simplemente quiera mover su almacenamiento pesado a un disco externo montado en AlmaLinux.
