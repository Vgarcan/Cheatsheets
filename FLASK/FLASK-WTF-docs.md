# Guía Completa de Flask-WTF

## Tabla de Contenidos

1. [Introducción a Flask-WTF](#introducción-a-flask-wtf)
2. [Instalación](#instalación)
3. [Versión Lanzada](#versión-lanzada)
4. [Desarrollo](#desarrollo)
5. [Inicio Rápido](#inicio-rápido)
6. [Creación de Formularios](#creación-de-formularios)
7. [Validación de Formularios](#validación-de-formularios)
8. [Formularios Seguros](#formularios-seguros)
9. [Subida de Archivos](#subida-de-archivos)
10. [Recaptcha](#recaptcha)
11. [Protección CSRF](#protección-csrf)
    - [Configuración](#configuración)
    - [Formularios HTML](#formularios-html)
    - [Solicitudes JavaScript](#solicitudes-javascript)
    - [Personalizar la Respuesta de Error](#personalizar-la-respuesta-de-error)
    - [Excluir Vistas de la Protección](#excluir-vistas-de-la-protección)
12. [Documentación de la API](#documentación-de-la-api)
13. [Interfaz de Desarrollador](#interfaz-de-desarrollador)
    - [Formularios y Campos](#formularios-y-campos)
    - [Protección CSRF](#protección-csrf-desarrollador)

## Introducción a Flask-WTF

Flask-WTF es una extensión de Flask que integra WTForms con Flask, lo que facilita la creación y validación de formularios web. Proporciona una serie de características avanzadas como validación de formularios, protección CSRF, Recaptcha, y más. Esta guía te ayudará a entender y utilizar Flask-WTF en tus proyectos de Flask.

## Instalación

Para instalar Flask-WTF, utiliza pip:

```bash
pip install flask-wtf
```

## Versión Lanzada

Para verificar la versión instalada de Flask-WTF, ejecuta:

```bash
pip show flask-wtf
```

## Desarrollo

Si deseas contribuir al desarrollo de Flask-WTF, clona el repositorio desde GitHub:

```bash
git clone https://github.com/wtforms/flask-wtf.git
cd flask-wtf
```

## Inicio Rápido

Aquí hay un ejemplo rápido de cómo usar Flask-WTF para crear un formulario simple:

### Código del Servidor (app.py)

```python
from flask import Flask, render_template, request
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key'

class MyForm(FlaskForm):
    name = StringField('Name', validators=[DataRequired()])
    submit = SubmitField('Submit')

@app.route('/', methods=['GET', 'POST'])
def index():
    form = MyForm()
    if form.validate_on_submit():
        return f'Hello, {form.name.data}!'
    return render_template('index.html', form=form)

if __name__ == '__main__':
    app.run(debug=True)
```

### Plantilla HTML (templates/index.html)

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Flask-WTF Quickstart</title>
  </head>
  <body>
    <form method="POST">
      {{ form.hidden_tag() }}
      <p>
        {{ form.name.label }}<br>
        {{ form.name(size=32) }}<br>
        {% for error in form.name.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>{{ form.submit() }}</p>
    </form>
  </body>
</html>
```

Este código crea un formulario simple que solicita un nombre y lo muestra en una nueva página si la validación es exitosa.

## Creación de Formularios

Para crear formularios en Flask-WTF, debes definir una clase que herede de `FlaskForm`. Aquí tienes un ejemplo más detallado:

### Código del Formulario (forms.py)

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField, SubmitField
from wtforms.validators import DataRequired, Email, EqualTo

class RegistrationForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    confirm_password = PasswordField('Confirm Password', validators=[DataRequired(), EqualTo('password')])
    remember_me = BooleanField('Remember Me')
    submit = SubmitField('Register')
```

### Uso del Formulario en la Aplicación (app.py)

```python
from flask import Flask, render_template
from forms import RegistrationForm

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key'

@app.route('/register', methods=['GET', 'POST'])
def register():
    form = RegistrationForm()
    if form.validate_on_submit():
        # Procesar los datos del formulario
        return 'Registration Successful'
    return render_template('register.html', form=form)

if __name__ == '__main__':
    app.run(debug=True)
```

### Plantilla HTML (templates/register.html)

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Register</title>
  </head>
  <body>
    <form method="POST">
      {{ form.hidden_tag() }}
      <p>
        {{ form.username.label }}<br>
        {{ form.username(size=32) }}<br>
        {% for error in form.username.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>
        {{ form.email.label }}<br>
        {{ form.email(size=32) }}<br>
        {% for error in form.email.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>
        {{ form.password.label }}<br>
        {{ form.password(size=32) }}<br>
        {% for error in form.password.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>
        {{ form.confirm_password.label }}<br>
        {{ form.confirm_password(size=32) }}<br>
        {% for error in form.confirm_password.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>
        {{ form.remember_me() }} {{ form.remember_me.label }}
      </p>
      <p>{{ form.submit() }}</p>
    </form>
  </body>
</html>
```

## Validación de Formularios

La validación es una parte crucial de cualquier formulario web. Flask-WTF proporciona validadores que puedes usar para asegurar que los datos introducidos sean válidos.

### Validadores Comunes

- `DataRequired`: Asegura que el campo no esté vacío.
- `Email`: Verifica que el campo contiene una dirección de correo válida.
- `EqualTo`: Verifica que dos campos tengan el mismo valor.

### Ejemplo de Uso

```python
from wtforms.validators import Length, Regexp

class LoginForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired(), Length(min=4, max=25)])
    password = PasswordField('Password', validators=[DataRequired(), Regexp('^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,20}$', message="Password must be between 6 and 20 characters long, and include at least one number, one uppercase and one lowercase letter.")])
    submit = SubmitField('Login')
```

### Uso del Formulario en la Aplicación

```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        # Procesar los datos del formulario
        return 'Login Successful'
    return render_template('login.html', form=form)
```

### Plantilla HTML

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Login</title>
  </head>
  <body>
    <form method="POST">
      {{ form.hidden_tag() }}
      <p>
        {{ form.username.label }}<br>
        {{ form.username(size=32) }}<br>
        {% for error in form.username.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>
        {{ form.password.label }}<br>
        {{ form.password(size=32) }}<br>
        {% for error in form.password.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>{{ form.submit() }}</p>
    </form>
  </body>
</html>
```

## Formularios Seguros

Para asegurar tus formularios y protegerte contra ataques CSRF (Cross-Site Request Forgery), Flask-WTF utiliza una clave secreta

 configurada en tu aplicación Flask:

```python
app.config['SECRET_KEY'] = 'your_secret_key'
```

Esta clave se utiliza para generar tokens CSRF, que aseguran que las solicitudes provienen de formularios generados por tu aplicación.

### Implementación de Protección CSRF

Flask-WTF maneja automáticamente la protección CSRF si se configura `SECRET_KEY`. Asegúrate de incluir `form.hidden_tag()` en tus plantillas HTML:

```html
<form method="POST">
  {{ form.hidden_tag() }}
  <!-- otros campos del formulario -->
  {{ form.submit() }}
</form>
```

## Subida de Archivos

Para manejar la subida de archivos, Flask-WTF proporciona el campo `FileField` junto con validadores específicos para archivos.

### Ejemplo de Formulario de Subida de Archivos

```python
from flask_wtf.file import FileField, FileRequired, FileAllowed

class UploadForm(FlaskForm):
    upload = FileField('Upload File', validators=[FileRequired(), FileAllowed(['jpg', 'png'], 'Images only!')])
    submit = SubmitField('Submit')
```

### Uso del Formulario en la Aplicación

```python
@app.route('/upload', methods=['GET', 'POST'])
def upload():
    form = UploadForm()
    if form.validate_on_submit():
        filename = form.upload.data.filename
        form.upload.data.save(f'uploads/{filename}')
        return 'File uploaded successfully'
    return render_template('upload.html', form=form)
```

### Plantilla HTML

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Upload File</title>
  </head>
  <body>
    <form method="POST" enctype="multipart/form-data">
      {{ form.hidden_tag() }}
      <p>
        {{ form.upload.label }}<br>
        {{ form.upload() }}<br>
        {% for error in form.upload.errors %}
          <span style="color: red;">[{{ error }}]</span><br>
        {% endfor %}
      </p>
      <p>{{ form.submit() }}</p>
    </form>
  </body>
</html>
```

## Recaptcha

Añadir un Recaptcha a tu formulario puede ayudar a prevenir el spam y el abuso.

### Configuración

Primero, necesitas configurar las claves de Recaptcha en tu aplicación Flask:

```python
app.config['RECAPTCHA_PUBLIC_KEY'] = 'your_public_key'
app.config['RECAPTCHA_PRIVATE_KEY'] = 'your_private_key'
```

### Ejemplo de Formulario con Recaptcha

```python
from flask_wtf import RecaptchaField

class MyForm(FlaskForm):
    recaptcha = RecaptchaField()
    submit = SubmitField('Submit')
```

### Uso del Formulario en la Aplicación

```python
@app.route('/recaptcha', methods=['GET', 'POST'])
def recaptcha():
    form = MyForm()
    if form.validate_on_submit():
        return 'Recaptcha verified successfully'
    return render_template('recaptcha.html', form=form)
```

### Plantilla HTML

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Recaptcha</title>
  </head>
  <body>
    <form method="POST">
      {{ form.hidden_tag() }}
      {{ form.recaptcha }}
      <p>{{ form.submit() }}</p>
    </form>
  </body>
</html>
```

## Protección CSRF

La protección CSRF (Cross-Site Request Forgery) es una característica importante de seguridad que previene ataques donde una solicitud malintencionada se realiza desde otro sitio en el que el usuario está autenticado.

### Configuración

La protección CSRF está habilitada por defecto en Flask-WTF. Puedes personalizar la configuración si es necesario:

```python
app.config['WTF_CSRF_ENABLED'] = True
app.config['WTF_CSRF_SECRET_KEY'] = 'your_csrf_secret_key'
```

### Formularios HTML

Para incluir tokens CSRF en tus formularios HTML, asegúrate de usar `form.hidden_tag()` en tus plantillas:

```html
<form method="POST" action="/">
    {{ form.hidden_tag() }}
    {{ form.name.label }} {{ form.name }}
    {{ form.submit }}
</form>
```

### Solicitudes JavaScript

Para solicitudes AJAX, incluye el token CSRF en los encabezados de tus peticiones. Aquí hay un ejemplo utilizando JavaScript:

```javascript
function getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}

const csrftoken = getCookie('csrf_token');

fetch('/some-url/', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-CSRFToken': csrftoken
    },
    body: JSON.stringify({data: 'your data'})
});
```

### Personalizar la Respuesta de Error

Para personalizar la respuesta cuando la verificación CSRF falla, puedes usar el siguiente código en tu aplicación Flask:

```python
from flask_wtf.csrf import CSRFError

@app.errorhandler(CSRFError)
def handle_csrf_error(e):
    return render_template('csrf_error.html', reason=e.description), 400
```

### Excluir Vistas de la Protección

Para excluir ciertas vistas de la protección CSRF, puedes usar el decorador `@csrf.exempt`:

```python
from flask_wtf.csrf import CSRFProtect

csrf = CSRFProtect(app)

@app.route('/no_csrf', methods=['POST'])
@csrf.exempt
def no_csrf():
    return 'This view is not protected by CSRF'
```

## Documentación de la API

Flask-WTF tiene una extensa documentación de la API. Para más detalles sobre funciones, clases y métodos específicos, consulta [la documentación oficial](https://flask-wtf.readthedocs.io/).

## Interfaz de Desarrollador

### Formularios y Campos

Flask-WTF extiende WTForms, por lo que todos los campos y validadores de WTForms están disponibles. Aquí hay algunos ejemplos adicionales:

```python
from wtforms import TextAreaField, RadioField, SelectField

class FeedbackForm(FlaskForm):
    feedback = TextAreaField('Feedback', validators=[DataRequired()])
    rating = RadioField('Rating', choices=[('1', '1'), ('2', '2'), ('3', '3')])
    submit = SubmitField('Submit')
```

### Protección CSRF (Desarrollador)

Para más detalles sobre la protección CSRF, incluyendo cómo extender y personalizar su comportamiento, consulta la sección [CSRF en la documentación oficial](https://flask-wtf.readthedocs.io/en/latest/csrf.html).

Esta guía cubre los aspectos esenciales de Flask-WTF para que puedas comenzar a crear formularios robustos y seguros en tus aplicaciones Flask.