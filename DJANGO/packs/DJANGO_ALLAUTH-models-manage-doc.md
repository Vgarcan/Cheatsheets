### Guía Completa para Extender Formularios y Modelos de Django-allauth

En muchas aplicaciones web, los requisitos de autenticación y gestión de usuarios van más allá de lo que ofrecen las configuraciones predeterminadas. Con el paquete **Django-allauth**, puedes extender y personalizar tanto los **formularios** como los **modelos** para adaptarlos a las necesidades específicas de tu proyecto. Ya sea que estés creando un flujo de registro personalizado, agregando campos adicionales a los usuarios, o integrando OAuth con redes sociales, esta guía te llevará paso a paso a través del proceso.

---

## **Índice**

1. [Visión General de Django-allauth](#vision-general-de-django-allauth)
2. [Extender el Modelo de Usuario](#extender-el-modelo-de-usuario)
3. [Extender Formularios en Django-allauth](#extender-formularios-en-django-allauth)
    - [Formulario de Registro (SignupForm)](#formulario-de-registro-signupform)
    - [Formulario de Inicio de Sesión (LoginForm)](#formulario-de-inicio-de-sesion-loginform)
    - [Formulario de Restablecimiento de Contraseña (PasswordResetForm)](#formulario-de-restablecimiento-de-contraseña-passwordresetform)
4. [Modificar Otros Modelos en Django-allauth](#modificar-otros-modelos-en-django-allauth)
    - [EmailAddress](#emailaddress)
    - [SocialAccount](#socialaccount)
5. [Consideraciones al Modificar Modelos y Formularios](#consideraciones-al-modificar-modelos-y-formularios)
    - [Migraciones y Gestión de la Base de Datos](#migraciones-y-gestion-de-la-base-de-datos)
    - [Compatibilidad con OAuth](#compatibilidad-con-oauth)
6. [Solución de Problemas Comunes](#solucion-de-problemas-comunes)

---

### 1. Visión General de Django-allauth

`django-allauth` es una herramienta robusta que permite la **autenticación** y **gestión de cuentas** mediante múltiples proveedores de OAuth (Google, Facebook, Twitter, etc.) y también mediante credenciales locales. Está diseñado para ser fácil de extender, lo que lo convierte en una solución ideal si necesitas agregar campos personalizados, ajustar el flujo de registro, o manejar datos adicionales sobre los usuarios.

---

### 2. Extender el Modelo de Usuario

Cuando necesitas agregar nuevos campos al modelo de usuario predeterminado de Django, el primer paso es **extender el modelo de usuario**. Esto es fundamental si quieres almacenar datos adicionales como "teléfono", "empresa", o cualquier otro atributo personalizado.

#### Paso 1: Crear un Modelo de Usuario Personalizado

Puedes extender el modelo de usuario utilizando `AbstractUser`, lo que te permitirá agregar los campos que desees.

```python
# users/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    es_empresa = models.BooleanField(default=False)
    telefono = models.CharField(max_length=15, blank=True)
```

#### Paso 2: Actualizar el Modelo de Usuario en `settings.py`

Para que Django use tu modelo personalizado, asegúrate de configurar `AUTH_USER_MODEL` en tu archivo `settings.py`:

```python
# settings.py
AUTH_USER_MODEL = 'users.CustomUser'
```

---

### 3. Extender Formularios en Django-allauth

Una vez que has extendido el modelo de usuario, el siguiente paso es personalizar los formularios que `django-allauth` proporciona. Esto te permitirá adaptar el proceso de registro, inicio de sesión y recuperación de contraseñas.

#### a. Formulario de Registro (SignupForm)

El formulario de registro (`SignupForm`) es el más comúnmente extendido. Puedes agregar campos adicionales al formulario para capturar información extra durante el registro.

```python
# users/forms.py
from allauth.account.forms import SignupForm
from django import forms

class CustomSignupForm(SignupForm):
    es_empresa = forms.BooleanField(required=False, label="¿Eres una empresa?")
    telefono = forms.CharField(max_length=15, required=False, label="Teléfono")

    def save(self, request):
        user = super(CustomSignupForm, self).save(request)
        user.es_empresa = self.cleaned_data['es_empresa']
        user.telefono = self.cleaned_data['telefono']
        user.save()
        return user
```

#### b. Formulario de Inicio de Sesión (LoginForm)

El formulario de inicio de sesión (`LoginForm`) también se puede personalizar para agregar campos adicionales o modificar su comportamiento.

```python
# users/forms.py
from allauth.account.forms import LoginForm
from django import forms

class CustomLoginForm(LoginForm):
    telefono = forms.CharField(max_length=15, required=False, label="Teléfono")

    def login(self, *args, **kwargs):
        # Lógica personalizada
        return super(CustomLoginForm, self).login(*args, **kwargs)
```

#### c. Formulario de Restablecimiento de Contraseña (PasswordResetForm)

Para personalizar el formulario de restablecimiento de contraseña, puedes extender `ResetPasswordForm` y agregar campos o validaciones adicionales.

```python
# users/forms.py
from allauth.account.forms import ResetPasswordForm
from django import forms

class CustomPasswordResetForm(ResetPasswordForm):
    confirm_email = forms.EmailField(label="Confirma tu email", required=True)

    def clean(self):
        cleaned_data = super().clean()
        email = cleaned_data.get("email")
        confirm_email = cleaned_data.get("confirm_email")

        if email != confirm_email:
            raise forms.ValidationError("Los correos no coinciden.")
        return cleaned_data
```

---

### 4. Modificar Otros Modelos en Django-allauth

Además del modelo de usuario, `django-allauth` tiene otros modelos que se pueden personalizar, como `EmailAddress` y `SocialAccount`. Estos modelos almacenan información adicional relacionada con las direcciones de correo electrónico de los usuarios o las cuentas sociales conectadas mediante OAuth.

#### a. EmailAddress

Este modelo permite asociar múltiples direcciones de correo a un usuario. Puedes extenderlo si necesitas almacenar información adicional sobre las direcciones de correo, como un alias o el tipo de dirección.

```python
# users/models.py
from allauth.account.models import EmailAddress
from django.db import models

class CustomEmailAddress(EmailAddress):
    alias = models.CharField(max_length=100, blank=True)
```

#### b. SocialAccount

El modelo `SocialAccount` gestiona las cuentas de redes sociales asociadas con el usuario. Puedes extender este modelo para capturar información adicional, como un identificador de usuario específico de la red social.

```python
# users/models.py
from allauth.socialaccount.models import SocialAccount
from django.db import models

class CustomSocialAccount(SocialAccount):
    social_handle = models.CharField(max_length=100, blank=True)
```

---

### 5. Consideraciones al Modificar Modelos y Formularios

Cuando personalizas los formularios y modelos en `django-allauth`, hay algunos aspectos importantes que debes tener en cuenta para asegurar el correcto funcionamiento de tu aplicación.

#### a. Migraciones y Gestión de la Base de Datos

Cada vez que modifiques un modelo (ya sea agregando nuevos campos o extendiendo modelos existentes), debes asegurarte de generar y aplicar las migraciones correspondientes para reflejar estos cambios en la base de datos.

- **Generar migraciones**:

    ```bash
    python manage.py makemigrations
    ```

- **Aplicar migraciones**:

    ```bash
    python manage.py migrate
    ```

**Nota**: Es importante que verifiques en todos los entornos (desarrollo, producción) que las migraciones se apliquen correctamente.

#### b. Compatibilidad con OAuth

Si tu proyecto utiliza autenticación mediante proveedores OAuth (como Google o Facebook), asegúrate de probar completamente cualquier cambio que hagas en los modelos de `SocialAccount`. La estructura de los datos que recibes de los proveedores de OAuth es muy específica, y cambios inesperados pueden romper la autenticación externa.

#### c. Dependencias de Aplicaciones Externas

`django-allauth` puede estar vinculado a otras aplicaciones de tu proyecto o a paquetes externos. Cualquier modificación en los modelos o formularios puede afectar a esas dependencias, por lo que es crucial verificar que todos los componentes de tu proyecto sigan funcionando correctamente después de los cambios.

---

### 6. Solución de Problemas Comunes

#### a. Migraciones No Detectadas

Si `makemigrations` no detecta los cambios que realizaste en un modelo, asegúrate de que:

- La aplicación correspondiente esté registrada en `INSTALLED_APPS`.
- No haya errores tipográficos en los modelos o formularios.
- Has ejecutado `makemigrations` y `migrate` en el entorno correcto.

#### b. Campos No Guardados Correctamente

Si los campos adicionales no se guardan en la base de datos, verifica que:

- El modelo de usuario esté correctamente extendido con los nuevos campos.
- El método `save()` del formulario personalizado esté bien configurado para guardar los datos correctamente.

#### c. Incompatibilidad con Proveedores OAuth

Si modificas los modelos relacionados con OAuth, prueba siempre la integración con cada proveedor (Google, Facebook, etc.) para asegurarte de que los cambios no afecten la autenticación. Un pequeño cambio en la estructura de los modelos puede romper la integración.

---

### Resumen

Extender los modelos y formularios de `django-allauth` te permite personalizar la autenticación y el manejo de usuarios para adaptarlos a las necesidades específicas de tu proyecto. En esta guía, hemos cubierto:

1.

 **Cómo extender el modelo de usuario** para agregar campos personalizados.
2. **Cómo personalizar los formularios** de registro, inicio de sesión y restablecimiento de contraseñas.
3. **Cómo modificar otros modelos importantes** como `EmailAddress` y `SocialAccount`.
4. **Las consideraciones esenciales** al modificar modelos, como la gestión de migraciones y la compatibilidad con OAuth.
5. **La solución de problemas comunes** que pueden surgir al hacer estas modificaciones.

Con este enfoque, podrás adaptar `django-allauth` para ofrecer una experiencia de autenticación personalizada en tu aplicación.

