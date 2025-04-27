## Guía: Combinando PyMongo, Flask-WTF y `marshmallow` para Desarrollar Aplicaciones Web

### Tabla de Contenidos
1. [Introducción](#1-introducción)
2. [Instalación](#2-instalación)
3. [Configuración Inicial](#3-configuración-inicial)
4. [Definición de Modelos con `marshmallow`](#4-definición-de-modelos-con-marshmallow)
5. [Generación de Formularios con Flask-WTF](#5-generación-de-formularios-con-flask-wtf)
6. [Uso de PyMongo para la Persistencia de Datos](#6-uso-de-pymongo-para-la-persistencia-de-datos)
7. [Métodos HTTP: GET y POST](#7-métodos-http-get-y-post)
8. [Ejemplo Completo: Página Web de Búsqueda de Trabajo](#8-ejemplo-completo-página-web-de-búsqueda-de-trabajo)
9. [Conclusión](#9-conclusión)

---

### 1. Introducción

En esta guía aprenderás a combinar PyMongo, Flask-WTF y `marshmallow` para crear modelos, generar formularios y manejar la persistencia de datos en una aplicación Flask. Además, discutiremos la diferencia entre los métodos HTTP GET y POST y cómo usarlos correctamente. Para ilustrar estos conceptos, desarrollaremos un ejemplo de una página web de búsqueda de trabajo.

---

### 2. Instalación

Primero, instala las bibliotecas necesarias:

```bash
pip install flask pymongo flask-wtf marshmallow flask-bootstrap
```

### Explicación:
- **Flask**: Un microframework de Python para construir aplicaciones web. Es ligero y flexible, ideal para proyectos pequeños y medianos.
- **PyMongo**: Una biblioteca que proporciona herramientas para interactuar con MongoDB desde Python.
- **Flask-WTF**: Una extensión de Flask que facilita la creación y validación de formularios web.
- **marshmallow**: Una biblioteca para la serialización y deserialización de datos, útil para validar y transformar datos.
- **Flask-Bootstrap**: Una extensión de Flask que facilita la integración de Bootstrap (un marco de CSS) para el diseño de páginas web.

---

### 3. Configuración Inicial

Configura tu aplicación Flask y conecta PyMongo:

```python
from flask import Flask, render_template, redirect, url_for, flash
from flask_wtf import FlaskForm
from flask_bootstrap import Bootstrap
from wtforms import StringField, IntegerField, SubmitField
from wtforms.validators import DataRequired, NumberRange
from marshmallow import Schema, fields, ValidationError
from flask_pymongo import PyMongo

app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey'
app.config['MONGO_URI'] = 'mongodb://localhost:27017/job_search_db'
mongo = PyMongo(app)
Bootstrap(app)
```

### Explicación:
- **Flask App**: Se crea una instancia de Flask que será la aplicación principal.
- **SECRET_KEY**: Una clave secreta utilizada por Flask-WTF para proteger los formularios contra ataques CSRF (Cross-Site Request Forgery).
- **MONGO_URI**: La URI de conexión a la base de datos MongoDB.
- **PyMongo**: Se configura para conectar la aplicación Flask con la base de datos MongoDB.
- **Bootstrap**: Se inicializa para usar Bootstrap en las plantillas HTML.

---

### 4. Definición de Modelos con `marshmallow`

Define tus modelos utilizando `marshmallow` para la validación y serialización de datos:

```python
class JobSchema(Schema):
    title = fields.Str(required=True)
    company = fields.Str(required=True)
    location = fields.Str(required=True)
    description = fields.Str()
    salary = fields.Int()
```

### Explicación:
- **JobSchema**: Un esquema de `marshmallow` que define los campos que esperamos para un trabajo (job).
- **fields.Str**: Define un campo de tipo cadena (string).
- **fields.Int**: Define un campo de tipo entero (integer).
- **required=True**: Indica que el campo es obligatorio.

---

### 5. Generación de Formularios con Flask-WTF

Crea un formulario Flask-WTF basado en tu modelo:

```python
class JobForm(FlaskForm):
    title = StringField('Job Title', validators=[DataRequired()])
    company = StringField('Company', validators=[DataRequired()])
    location = StringField('Location', validators=[DataRequired()])
    description = StringField('Description')
    salary = IntegerField('Salary', validators=[NumberRange(min=0)])
    submit = SubmitField('Submit')
```

### Explicación:
- **FlaskForm**: Una clase base para definir formularios en Flask-WTF.
- **StringField**: Define un campo de entrada de texto.
- **IntegerField**: Define un campo de entrada de número entero.
- **SubmitField**: Define un botón de envío para el formulario.
- **DataRequired**: Un validador que asegura que el campo no esté vacío.
- **NumberRange**: Un validador que asegura que el número esté dentro de un rango específico (por ejemplo, mayor o igual a 0).

---

### 6. Uso de PyMongo para la Persistencia de Datos

Configura tu vista para manejar la creación de trabajos y su almacenamiento en MongoDB:

```python
@app.route('/job_form', methods=['GET', 'POST'])
def job_form():
    form = JobForm()
    if form.validate_on_submit():
        job_schema = JobSchema()
        job_data = {
            'title': form.title.data,
            'company': form.company.data,
            'location': form.location.data,
            'description': form.description.data,
            'salary': form.salary.data
        }
        try:
            job = job_schema.load(job_data)
            mongo.db.jobs.insert_one(job)
            flash('Job successfully created!', 'success')
            return redirect(url_for('job_form'))
        except ValidationError as

 err:
            flash(err.messages, 'danger')
    return render_template('job_form.html', form=form)
```

### Explicación:
- **Rutas Flask**: Define una ruta en la aplicación que maneja tanto GET como POST.
- **GET**: Cuando se carga la página, se presenta el formulario vacío.
- **POST**: Cuando se envía el formulario, se valida y los datos se serializan usando `marshmallow`.
- **mongo.db.jobs.insert_one**: Inserta los datos validados en la colección `jobs` de MongoDB.
- **flash**: Muestra mensajes a los usuarios.

---

### 7. Métodos HTTP: GET y POST

#### Método GET
- **Uso**: Obtener datos del servidor, como cargar una página o obtener información.
- **Ejemplo**: Cargar una página con un formulario vacío.

```python
@app.route('/job_form', methods=['GET'])
def get_job_form():
    form = JobForm()
    return render_template('job_form.html', form=form)
```

#### Método POST
- **Uso**: Enviar datos al servidor para ser procesados, como enviar un formulario.
- **Ejemplo**: Enviar los datos del formulario para ser validados y guardados.

```python
@app.route('/job_form', methods=['POST'])
def post_job_form():
    form = JobForm()
    if form.validate_on_submit():
        job_data = {
            'title': form.title.data,
            'company': form.company.data,
            'location': form.location.data,
            'description': form.description.data,
            'salary': form.salary.data
        }
        # Procesar y guardar datos
    return render_template('job_form.html', form=form)
```

### Explicación:
- **GET**: Método utilizado para solicitar datos del servidor. Es idempotente, lo que significa que llamar al mismo recurso varias veces no tiene efectos secundarios.
- **POST**: Método utilizado para enviar datos al servidor. No es idempotente, lo que significa que enviar la misma solicitud varias veces puede tener efectos secundarios (como crear múltiples registros).

---

### 8. Ejemplo Completo: Página Web de Búsqueda de Trabajo

#### Plantilla HTML (`templates/job_form.html`)

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Job Form</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        <h1>Job Form</h1>
        <form method="POST" action="">
            {{ form.hidden_tag() }}
            <div class="form-group">
                {{ form.title.label }} 
                {{ form.title(class="form-control") }}
            </div>
            <div class="form-group">
                {{ form.company.label }} 
                {{ form.company(class="form-control") }}
            </div>
            <div class="form-group">
                {{ form.location.label }} 
                {{ form.location(class="form-control") }}
            </div>
            <div class="form-group">
                {{ form.description.label }} 
                {{ form.description(class="form-control") }}
            </div>
            <div class="form-group">
                {{ form.salary.label }} 
                {{ form.salary(class="form-control") }}
            </div>
            <div class="form-group">
                {{ form.submit(class="btn btn-primary") }}
            </div>
        </form>
    </div>
</body>
</html>
```

### Explicación:
- **HTML con Bootstrap**: Se usa Bootstrap para dar estilo a la página y a los formularios.
- **Flask-WTF**: Los campos del formulario se representan utilizando el objeto form proporcionado por Flask-WTF.
- **hidden_tag**: Un campo oculto que incluye un token CSRF para proteger contra ataques CSRF.

#### Configuración Completa de Flask

```python
from flask import Flask, render_template, redirect, url_for, flash
from flask_wtf import FlaskForm
from flask_bootstrap import Bootstrap
from wtforms import StringField, IntegerField, SubmitField
from wtforms.validators import DataRequired, NumberRange
from marshmallow import Schema, fields, ValidationError
from flask_pymongo import PyMongo

app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey'
app.config['MONGO_URI'] = 'mongodb://localhost:27017/job_search_db'
mongo = PyMongo(app)
Bootstrap(app)

class JobSchema(Schema):
    title = fields.Str(required=True)
    company = fields.Str(required=True)
    location = fields.Str(required=True)
    description = fields.Str()
    salary = fields.Int()

class JobForm(FlaskForm):
    title = StringField('Job Title', validators=[DataRequired()])
    company = StringField('Company', validators=[DataRequired()])
    location = StringField('Location', validators=[DataRequired()])
    description = StringField('Description')
    salary = IntegerField('Salary', validators=[NumberRange(min=0)])
    submit = SubmitField('Submit')

@app.route('/job_form', methods=['GET', 'POST'])
def job_form():
    form = JobForm()
    if form.validate_on_submit():
        job_schema = JobSchema()
        job_data = {
            'title': form.title.data,
            'company': form.company.data,
            'location': form.location.data,
            'description': form.description.data,
            'salary': form.salary.data
        }
        try:
            job = job_schema.load(job_data)
            mongo.db.jobs.insert_one(job)
            flash('Job successfully created!', 'success')
            return redirect(url_for('job_form'))
        except ValidationError as err:
            flash(err.messages, 'danger')
    return render_template('job_form.html', form=form)

if __name__ == '__main__':
    app.run(debug=True)
```

### Explicación:
- **job_form**: La vista que maneja el formulario de trabajo.
- **form.validate_on_submit()**: Verifica si el formulario fue enviado y es válido.
- **job_schema.load()**: Valida y deserializa los datos del formulario.
- **mongo.db.jobs.insert_one()**: Inserta los datos validados en la base de datos MongoDB.
- **flash()**: Muestra mensajes de éxito o error al usuario.

---

### 9. Conclusión

Esta guía te ha mostrado cómo combinar PyMongo, Flask-WTF y `marshmallow` para crear modelos, generar formularios y manejar la persistencia de datos en una aplicación Flask. También discutimos cómo usar correctamente los métodos HTTP GET y POST. Con estos conocimientos, puedes desarrollar aplicaciones web de manera más rápida y eficiente.