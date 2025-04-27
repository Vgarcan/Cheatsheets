# 📖 Guía Completa de Cron Jobs para Principiantes en Desarrollo Web y Software

---

## 📑 Tabla de Contenidos
- [📖 Guía Completa de Cron Jobs para Principiantes en Desarrollo Web y Software](#-guía-completa-de-cron-jobs-para-principiantes-en-desarrollo-web-y-software)
  - [📑 Tabla de Contenidos](#-tabla-de-contenidos)
- [📚 ¿Qué es un Cron Job?](#-qué-es-un-cron-job)
- [📚 ¿Qué es el Cron Daemon?](#-qué-es-el-cron-daemon)
- [📚 ¿Cómo funcionan los Cron Jobs?](#-cómo-funcionan-los-cron-jobs)
- [📚 Sintaxis de un Cron Job](#-sintaxis-de-un-cron-job)
  - [🛠️ Estructura detallada](#️-estructura-detallada)
    - [🧩 Opciones especiales:](#-opciones-especiales)
  - [🛠️ Ejemplos de fecha y hora listos para copiar](#️-ejemplos-de-fecha-y-hora-listos-para-copiar)
- [📚 Comandos útiles con Cron](#-comandos-útiles-con-cron)
- [📚 Crear, listar, editar y eliminar Cron Jobs](#-crear-listar-editar-y-eliminar-cron-jobs)
  - [✏️ Editar el `crontab`](#️-editar-el-crontab)
- [📚 Cómo crear Logs para tus Cron Jobs](#-cómo-crear-logs-para-tus-cron-jobs)
  - [🛠️ Redireccionando salidas](#️-redireccionando-salidas)
  - [🛠️ Formato de logs mejorado con timestamps](#️-formato-de-logs-mejorado-con-timestamps)
  - [🛠️ Enviar logs por correo electrónico](#️-enviar-logs-por-correo-electrónico)
- [📚 Variables de entorno en Cron Jobs](#-variables-de-entorno-en-cron-jobs)
- [📚 Errores comunes y cómo solucionarlos](#-errores-comunes-y-cómo-solucionarlos)
- [📚 Buenas prácticas para programar Cron Jobs](#-buenas-prácticas-para-programar-cron-jobs)
- [📚 Casos de uso reales de Cron Jobs](#-casos-de-uso-reales-de-cron-jobs)
- [Proyectos prácticos](#proyectos-prácticos)
  - [🛠 Proyecto 1: Backup automático de base de datos MySQL](#-proyecto-1-backup-automático-de-base-de-datos-mysql)
  - [🎯 Objetivo:](#-objetivo)
  - [📦 Pasos:](#-pasos)
    - [1. Crear el script de backup](#1-crear-el-script-de-backup)
    - [2. Programar el cron job](#2-programar-el-cron-job)
  - [🛠 Proyecto 2: Ejecución diaria de tareas Django con Cron](#-proyecto-2-ejecución-diaria-de-tareas-django-con-cron)
  - [🎯 Objetivo:](#-objetivo-1)
  - [📦 Pasos:](#-pasos-1)
    - [1. Crear un script para comandos Django](#1-crear-un-script-para-comandos-django)
    - [2. Programar el cron job](#2-programar-el-cron-job-1)
  - [🛠 Proyecto 3: Limpieza semanal de archivos antiguos](#-proyecto-3-limpieza-semanal-de-archivos-antiguos)
  - [🎯 Objetivo:](#-objetivo-2)
  - [📦 Pasos:](#-pasos-2)
    - [1. Crear el script de limpieza](#1-crear-el-script-de-limpieza)
    - [2. Programar el cron job](#2-programar-el-cron-job-2)
  - [🛠 Proyecto 4: Monitorización de servicios con Python y Cron](#-proyecto-4-monitorización-de-servicios-con-python-y-cron)
  - [🎯 Objetivo:](#-objetivo-3)
  - [📦 Pasos:](#-pasos-3)
    - [1. Crear el script en Python](#1-crear-el-script-en-python)
    - [2. Programar el cron job](#2-programar-el-cron-job-3)
  - [🛠 Proyecto 5: Automatizar un cronjob para WordPress](#-proyecto-5-automatizar-un-cronjob-para-wordpress)
  - [🎯 Objetivo:](#-objetivo-4)
  - [📦 Pasos:](#-pasos-4)
    - [1. Desactivar el `wp-cron` automático en `wp-config.php`](#1-desactivar-el-wp-cron-automático-en-wp-configphp)
    - [2. Crear un cron job para llamarlo manualmente](#2-crear-un-cron-job-para-llamarlo-manualmente)
- [📚 Plantillas de Cron Jobs Listas para Usar](#-plantillas-de-cron-jobs-listas-para-usar)
    - [Plantilla para backup de base de datos](#plantilla-para-backup-de-base-de-datos)
    - [Plantilla para limpiar sesiones de Django](#plantilla-para-limpiar-sesiones-de-django)
    - [Plantilla para limpieza de `/tmp`](#plantilla-para-limpieza-de-tmp)
- [📚 Monitoreo avanzado de Cron Jobs](#-monitoreo-avanzado-de-cron-jobs)
  - [📦 Supervisar que los Cron Jobs realmente se ejecuten](#-supervisar-que-los-cron-jobs-realmente-se-ejecuten)
    - [Ejemplo:](#ejemplo)
  - [📦 Enviar notificaciones si un Cron Job falla](#-enviar-notificaciones-si-un-cron-job-falla)
    - [Forma sencilla:](#forma-sencilla)
- [📚 Trabajar con Anacron: Cron Jobs para sistemas que no están siempre encendidos](#-trabajar-con-anacron-cron-jobs-para-sistemas-que-no-están-siempre-encendidos)
  - [📦 ¿Qué es Anacron?](#-qué-es-anacron)
  - [📦 Diferencias entre Cron y Anacron](#-diferencias-entre-cron-y-anacron)
  - [📦 Cómo usar Anacron](#-cómo-usar-anacron)
    - [1. Archivos principales](#1-archivos-principales)
    - [2. Formato de entrada en `anacrontab`:](#2-formato-de-entrada-en-anacrontab)
    - [3. Ejemplo de `anacrontab`:](#3-ejemplo-de-anacrontab)
- [📚 Errores comunes de principiantes con Cron](#-errores-comunes-de-principiantes-con-cron)
- [📚 Bonus: Tabla de trucos y atajos útiles](#-bonus-tabla-de-trucos-y-atajos-útiles)
---

# 📚 ¿Qué es un Cron Job?

Un **Cron Job** es una tarea programada que se ejecuta automáticamente en el sistema operativo Linux o Unix, sin necesidad de intervención humana.  

Es una de las **herramientas más potentes** para la **automatización** de tareas repetitivas.

💬 **Definición simple:**  
_"Es como poner una alarma a un comando para que se ejecute solo cuando tú quieras."_

---

# 📚 ¿Qué es el Cron Daemon?

El **Cron Daemon** (`cron`) es el servicio que:
- Corre **en segundo plano**.
- **Espera** a que llegue el momento programado.
- **Ejecuta** los comandos configurados.

🛠️ **Comandos para gestionarlo:**
```bash
sudo systemctl status cron    # Ver estado
sudo systemctl start cron     # Iniciar
sudo systemctl stop cron      # Detener
sudo systemctl restart cron   # Reiniciar
sudo systemctl enable cron    # Habilitar al arrancar
```

📌 **Nota:** Si `cron` no está corriendo, tus tareas programadas **nunca se ejecutarán**.

---

# 📚 ¿Cómo funcionan los Cron Jobs?

Los cron jobs se guardan en un **archivo llamado `crontab`**, que contiene las reglas de:
- **Qué ejecutar**.
- **Cuándo** ejecutarlo.

Cada usuario del sistema puede tener **su propio crontab**, independiente de otros usuarios.  
El sistema también tiene su propio crontab general (`/etc/crontab`).

Hay carpetas especiales:
- `/etc/cron.daily/`
- `/etc/cron.weekly/`
- `/etc/cron.monthly/`
- `/etc/cron.hourly/`

Donde puedes poner **scripts** directamente, y `cron` los ejecutará automáticamente en el intervalo correspondiente.

---

# 📚 Sintaxis de un Cron Job

## 🛠️ Estructura detallada

La sintaxis general es:

```bash
* * * * * comando
│ │ │ │ │
│ │ │ │ └── Día de la semana (0-7) (Domingo = 0 o 7)
│ │ │ └──── Mes (1-12)
│ │ └────── Día del mes (1-31)
│ └──────── Hora (0-23)
└────────── Minuto (0-59)
```

### 🧩 Opciones especiales:
| Símbolo o valor | Significado |
|:---|:---|
| `*` | Todos los valores posibles |
| `,` | Lista de valores (ej: `1,5,10`) |
| `-` | Rango de valores (ej: `1-5`) |
| `/` | Paso de valores (ej: `*/10` cada 10 unidades) |

---

## 🛠️ Ejemplos de fecha y hora listos para copiar

```bash
* * * * * /ruta/al/script.sh
# Ejecuta cada minuto

0 0 * * * /ruta/al/script.sh
# Ejecuta todos los días a las 00:00 (medianoche)

15 3 * * * /ruta/al/script.sh
# Ejecuta todos los días a las 3:15 AM

0 6 * * 1 /ruta/al/script.sh
# Ejecuta todos los lunes a las 6:00 AM

30 5 1 * * /ruta/al/script.sh
# Ejecuta el día 1 de cada mes a las 5:30 AM

*/15 * * * * /ruta/al/script.sh
# Ejecuta cada 15 minutos

0 2 * 12 * /ruta/al/script.sh
# Ejecuta todos los días de diciembre a las 2:00 AM

0 21 * * 6,0 /ruta/al/script.sh
# Ejecuta los sábados y domingos a las 9:00 PM
```

---

# 📚 Comandos útiles con Cron

| Acción | Comando |
|:---|:---|
| Listar tus cron jobs | `crontab -l` |
| Editar tu crontab | `crontab -e` |
| Borrar todos tus cron jobs | `crontab -r` |
| Ver cron jobs de otro usuario | `sudo crontab -u usuario -l` |
| Ver los logs de ejecución de cron | `cat /var/log/syslog | grep CRON` |

---

# 📚 Crear, listar, editar y eliminar Cron Jobs

## ✏️ Editar el `crontab`
```bash
crontab -e
```
Cuando lo ejecutes:
- Se abrirá el editor de texto configurado (generalmente `nano`).
- Puedes añadir o editar tus tareas programadas.

🔵 **Nota:** Siempre usa **rutas absolutas** en los comandos del cron job.

---

# 📚 Cómo crear Logs para tus Cron Jobs

## 🛠️ Redireccionando salidas

Para registrar lo que pasa en el cron:

```bash
* * * * * /ruta/al/script.sh >> /ruta/al/log/script.log 2>&1
```
- `>>` agrega al archivo.
- `2>&1` captura los errores también.

---

## 🛠️ Formato de logs mejorado con timestamps

Podemos usar `date` para marcar cada entrada en el log:

```bash
* * * * * echo "$(date '+\%Y-\%m-\%d \%H:\%M:\%S') - Iniciando tarea" >> /ruta/al/log/script.log 2>&1
```

---

## 🛠️ Enviar logs por correo electrónico

Puedes configurar el `MAILTO` al principio de tu `crontab`:

```bash
MAILTO="tuemail@ejemplo.com"
```

De este modo, cada vez que se ejecute un cron job, el resultado se enviará al correo configurado.

---

# 📚 Variables de entorno en Cron Jobs

El entorno de `cron` es **minimalista**.  
No tiene todas las variables de tu terminal.

💬 **Solución:** Declara las variables que necesites en tu crontab o en tu script.

Ejemplo:

```bash
PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin
```

---

# 📚 Errores comunes y cómo solucionarlos

| Problema | Causa | Solución |
|:---|:---|:---|
| Script no corre | Servicio `cron` detenido | `sudo systemctl start cron` |
| Rutas incorrectas | Cron no usa tu `PATH` habitual | Usa rutas absolutas |
| Permisos de ejecución | Script no ejecutable | `chmod +x script.sh` |
| Sin salida de errores | No capturas `stderr` | Agrega `2>&1` |

---

# 📚 Buenas prácticas para programar Cron Jobs

- **Usa scripts externos**: No pongas lógica compleja directamente en el crontab.
- **Siempre usa rutas absolutas**.
- **Documenta** tus cron jobs con comentarios `#`.
- **Agrega logs**.
- **Agrupa cron jobs similares** para mayor claridad.
- **Evita ejecutar tareas pesadas en horas punta**.

---

# 📚 Casos de uso reales de Cron Jobs

| Caso | Explicación |
|:---|:---|
| Respaldos automáticos | Copiar bases de datos, archivos importantes |
| Actualizaciones de caché | Regenerar cachés de una página web |
| Notificaciones por correo | Recordatorios o alertas |
| Limpieza de archivos | Borrar logs viejos, eliminar basura |
| Automatización de tareas devops | Deployments programados, reinicios |

---

# Proyectos prácticos

## 🛠 Proyecto 1: Backup automático de base de datos MySQL

## 🎯 Objetivo:
- Realizar una **copia de seguridad diaria** de una base de datos MySQL.
- Guardar los backups en un directorio específico.
- Mantener un **log de ejecución**.

---

## 📦 Pasos:

### 1. Crear el script de backup

**Archivo:** `/home/usuario/scripts/backup_mysql.sh`

```bash
#!/bin/bash
# Script para respaldar una base de datos MySQL

# Configuraciones
FECHA=$(date +"%Y-%m-%d")
DESTINO="/home/usuario/backups"
USUARIO="nombre_usuario"
PASSWORD="contraseña_segura"
DATABASE="nombre_base_datos"

# Crear el backup
mysqldump -u $USUARIO -p$PASSWORD $DATABASE > $DESTINO/backup_$FECHA.sql

# Registrar en el log
echo "$(date +"%Y-%m-%d %H:%M:%S") - Backup realizado: $DESTINO/backup_$FECHA.sql" >> /home/usuario/logs/backup.log
```

🔵 **Notas:**
- Asegúrate de que el script sea ejecutable:
  ```bash
  chmod +x /home/usuario/scripts/backup_mysql.sh
  ```

---

### 2. Programar el cron job

```bash
0 2 * * * /home/usuario/scripts/backup_mysql.sh >> /home/usuario/logs/backup_cron.log 2>&1
```

🕑 **Se ejecutará todos los días a las 2:00 AM.**

---

## 🛠 Proyecto 2: Ejecución diaria de tareas Django con Cron

## 🎯 Objetivo:
- Automatizar la ejecución de comandos de gestión (`manage.py`) en Django.

---

## 📦 Pasos:

### 1. Crear un script para comandos Django

**Archivo:** `/home/usuario/scripts/django_command.sh`

```bash
#!/bin/bash
# Ejecutar tareas programadas de Django

# Activar entorno virtual (si existe)
source /home/usuario/mi_proyecto/venv/bin/activate

# Ir al directorio del proyecto
cd /home/usuario/mi_proyecto

# Ejecutar comando Django
python manage.py clearsessions

# Log
echo "$(date +"%Y-%m-%d %H:%M:%S") - Comando Django ejecutado" >> /home/usuario/logs/django_cron.log
```

🔵 **Notas:**
- `clearsessions` borra sesiones expiradas de Django.

---

### 2. Programar el cron job

```bash
0 4 * * * /home/usuario/scripts/django_command.sh >> /home/usuario/logs/django_cron.log 2>&1
```

🕓 **Se ejecutará todos los días a las 4:00 AM.**

---

## 🛠 Proyecto 3: Limpieza semanal de archivos antiguos

## 🎯 Objetivo:
- Eliminar archivos de más de 30 días automáticamente.

---

## 📦 Pasos:

### 1. Crear el script de limpieza

**Archivo:** `/home/usuario/scripts/limpieza_tmp.sh`

```bash
#!/bin/bash
# Borrar archivos viejos de /tmp

find /tmp -type f -mtime +30 -exec rm {} \;

# Log
echo "$(date +"%Y-%m-%d %H:%M:%S") - Limpieza de /tmp completada" >> /home/usuario/logs/limpieza.log
```

---

### 2. Programar el cron job

```bash
0 3 * * 0 /home/usuario/scripts/limpieza_tmp.sh >> /home/usuario/logs/limpieza.log 2>&1
```

🕒 **Se ejecutará todos los domingos a las 3:00 AM.**

---

## 🛠 Proyecto 4: Monitorización de servicios con Python y Cron

## 🎯 Objetivo:
- Comprobar si un servicio web sigue activo.
- Notificar si el servicio cae.

---

## 📦 Pasos:

### 1. Crear el script en Python

**Archivo:** `/home/usuario/scripts/monitor_web.py`

```python
#!/usr/bin/env python3
import requests
import datetime

url = "https://midominio.com"

try:
    response = requests.get(url, timeout=10)
    if response.status_code == 200:
        with open("/home/usuario/logs/monitor_web.log", "a") as log:
            log.write(f"{datetime.datetime.now()} - OK\n")
    else:
        with open("/home/usuario/logs/monitor_web.log", "a") as log:
            log.write(f"{datetime.datetime.now()} - FALLA: {response.status_code}\n")
except Exception as e:
    with open("/home/usuario/logs/monitor_web.log", "a") as log:
        log.write(f"{datetime.datetime.now()} - ERROR: {str(e)}\n")
```

🔵 **Notas:**
- Necesitas instalar `requests` si no lo tienes:
  ```bash
  pip install requests
  ```

---

### 2. Programar el cron job

```bash
*/10 * * * * /usr/bin/python3 /home/usuario/scripts/monitor_web.py
```

🔵 **Se ejecutará cada 10 minutos.**

---

## 🛠 Proyecto 5: Automatizar un cronjob para WordPress

## 🎯 Objetivo:
- Ejecutar tareas de WordPress (wp-cron.php) de manera manual con cron para optimizar rendimiento.

---

## 📦 Pasos:

### 1. Desactivar el `wp-cron` automático en `wp-config.php`

Agrega:
```php
define('DISABLE_WP_CRON', true);
```

---

### 2. Crear un cron job para llamarlo manualmente

```bash
*/15 * * * * curl https://tuweb.com/wp-cron.php?doing_wp_cron > /dev/null 2>&1
```

🔵 **Notas:**
- Esto llama `wp-cron.php` cada 15 minutos.
- Ayuda a evitar sobrecarga en visitas reales.

---

# 📚 Plantillas de Cron Jobs Listas para Usar

### Plantilla para backup de base de datos
```bash
0 2 * * * /home/usuario/scripts/backup_mysql.sh >> /home/usuario/logs/backup.log 2>&1
```

### Plantilla para limpiar sesiones de Django
```bash
0 4 * * * /home/usuario/scripts/django_command.sh >> /home/usuario/logs/django_cron.log 2>&1
```

### Plantilla para limpieza de `/tmp`
```bash
0 3 * * 0 /home/usuario/scripts/limpieza_tmp.sh >> /home/usuario/logs/limpieza.log 2>&1
```
---

# 📚 Monitoreo avanzado de Cron Jobs

## 📦 Supervisar que los Cron Jobs realmente se ejecuten

Aunque cron **intenta** ejecutar todo, **no garantiza** que:
- El script funcione.
- El servidor no tenga un problema.

🔵 **Solución básica:**  
Programar un cron job que registre una marca de vida.

### Ejemplo:
```bash
* * * * * echo "Cron funcionando $(date)" >> /home/usuario/logs/cron_alive.log
```

**Después:** Puedes crear otro script que **verifique** si la marca de vida está actualizada.

---

## 📦 Enviar notificaciones si un Cron Job falla

🔔 **Enviarte un correo si un cron job falla es una buena práctica.**

### Forma sencilla:
1. Configura `MAILTO` en tu `crontab`.
   
```bash
MAILTO="tuemail@ejemplo.com"
```
2. No redirijas la salida estándar (`stdout`) ni de error (`stderr`) en ese cron particular.

Así, **si algo falla**, recibirás automáticamente un correo.

---

# 📚 Trabajar con Anacron: Cron Jobs para sistemas que no están siempre encendidos

---

## 📦 ¿Qué es Anacron?

- `anacron` es una herramienta para **programar tareas periódicas** como cron.
- Pero **no requiere que el sistema esté encendido** a una hora exacta.
- Ideal para **laptops, PC personales**, o **servidores que se apagan**.

---

## 📦 Diferencias entre Cron y Anacron

| Característica | Cron | Anacron |
|:--------------|:----|:-------|
| Precisión de tiempo | Sí | No estricta |
| Requiere estar encendido | Sí | No |
| Usado en | Servidores | Laptops, PCs |
| Ejecuta tareas perdidas | No | Sí |

---

## 📦 Cómo usar Anacron

### 1. Archivos principales
Anacron se configura en:
- `/etc/anacrontab`

### 2. Formato de entrada en `anacrontab`:
```bash
PERIOD  DELAY  IDENTIFICADOR  COMANDO
```

| Campo | Significado |
|:-----|:------------|
| PERIOD | Intervalo de ejecución en días |
| DELAY | Retraso aleatorio inicial en minutos |
| IDENTIFICADOR | Nombre único para registrar |
| COMANDO | Comando o script a ejecutar |

---

### 3. Ejemplo de `anacrontab`:

```bash
# /etc/anacrontab

1   5   backup_db   /home/usuario/scripts/backup_mysql.sh
7   10  limpiar_tmp /home/usuario/scripts/limpieza_tmp.sh
```

- `backup_db` corre **cada día** (1 día de periodo) con **5 minutos de retraso**.
- `limpiar_tmp` corre **una vez a la semana** (7 días).

---

# 📚 Errores comunes de principiantes con Cron

| Error | Cómo evitarlo |
|:---|:---|
| Olvidar usar rutas absolutas | Siempre usa `/usr/bin/python3` en vez de `python3` |
| Script no ejecutable | `chmod +x script.sh` para dar permisos |
| El cron job no hace nada | Agrega `2>&1` para ver errores |
| Pensar que cron usa tu PATH normal | Configura tu `PATH` en el script o crontab |
| No registrar logs | Siempre agrega `>> log 2>&1` |
| No probar manualmente antes de programar | Siempre ejecuta manualmente primero |

---

# 📚 Bonus: Tabla de trucos y atajos útiles

| Truco | Código |
|:---|:---|
| Ejecutar cada 10 minutos | `*/10 * * * * comando` |
| Ejecutar cada 2 horas | `0 */2 * * * comando` |
| Ejecutar el primer lunes del mes | `0 0 1-7 * 1 comando` |
| Ejecutar solo en días laborables | `0 9 * * 1-5 comando` |
| Ejecutar cuando el sistema arranca | Agrega en `/etc/rc.local` o usa `@reboot` en cron |
