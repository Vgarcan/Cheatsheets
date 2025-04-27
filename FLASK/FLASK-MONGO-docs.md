# Guía Completa de Flask-MongoEngine

## Tabla de Contenidos

Entendido, aquí está la tabla de contenidos con los tipos de campos (fields) incluidos:

1. [Introducción a Flask-MongoEngine](#introducción-a-flask-mongoengine)
2. [Instalación](#instalación)
3. [Configuración Básica](#configuración-básica)
4. [Modelos en Flask-MongoEngine](#modelos-en-flask-mongoengine)
    - [Tipos de Fields en Flask-MongoEngine](#tipos-de-fields-en-flask-mongoengine)
        - [StringField](#stringfield)
        - [IntField](#intfield)
        - [FloatField](#floatfield)
        - [BooleanField](#booleanfield)
        - [DateTimeField](#datetimefield)
        - [ListField](#listfield)
        - [ReferenceField](#referencefield)
        - [EmbeddedDocumentField](#embeddeddocumentfield)
        - [DynamicField](#dynamicfield)
5. [Operaciones CRUD](#operaciones-crud)
6. [Consultas Avanzadas](#consultas-avanzadas)
7. [Validación y Schemas](#validación-y-schemas)
8. [Relaciones entre Documentos](#relaciones-entre-documentos)
9. [Paginación](#paginación)
10. [Serialización](#serialización)
11. [Integración con Formularios WTForms](#integración-con-formularios-wtforms)
12. [Configuración de Middleware](#configuración-de-middleware)
13. [Manejo de Errores](#manejo-de-errores)
14. [Documentación de la API](#documentación-de-la-api)
15. [Interfaz de Desarrollador](#interfaz-de-desarrollador)

Espero que esta versión sea lo que buscas.
## Introducción a Flask-MongoEngine

Flask-MongoEngine es una extensión de Flask que facilita la integración de MongoDB en tus aplicaciones Flask usando MongoEngine, un ORM (Object-Relational Mapping) para MongoDB. Flask-MongoEngine simplifica la conexión a la base de datos, la creación de modelos y la realización de operaciones CRUD.

En esta guía, usaremos Flask-MongoEngine para crear una aplicación de búsqueda de empleo, cubriendo desde la configuración inicial hasta las operaciones avanzadas.

## Instalación

Para instalar Flask-MongoEngine, usa pip:

```bash
pip install flask-mongoengine
```

## Configuración Básica

Para comenzar a usar Flask-MongoEngine, debes configurar la conexión a la base de datos en tu aplicación Flask. A continuación, te muestro cómo hacerlo:

### Código del Servidor (app.py)

```python
from flask import Flask
from flask_mongoengine import MongoEngine

app = Flask(__name__)
app.config['MONGODB_SETTINGS'] = {
    'db': 'jobsearchdb',
    'host': 'localhost',
    'port': 27017
}

db = MongoEngine()
db.init_app(app)

if __name__ == '__main__':
    app.run(debug=True)
```

## Modelos en Flask-MongoEngine

Los modelos en Flask-MongoEngine se definen como clases que heredan de `db.Document`. Para nuestra aplicación de búsqueda de empleo, crearemos modelos para `Job`, `Employer` y `Applicant`.

### Definición de Modelos (models.py)

```python
from flask_mongoengine import MongoEngine
from datetime import datetime

db = MongoEngine()

class Employer(db.Document):
    name = db.StringField(required=True, unique=True)
    industry = db.StringField()
    location = db.StringField()

class Job(db.Document):
    title = db.StringField(required=True)
    description = db.StringField()
    requirements = db.ListField(db.StringField())
    employer = db.ReferenceField(Employer, reverse_delete_rule=db.CASCADE)
    posted_at = db.DateTimeField(default=datetime.utcnow)

class Applicant(db.Document):
    name = db.StringField(required=True)
    email = db.StringField(required=True, unique=True)
    resume = db.StringField()
    applied_jobs = db.ListField(db.ReferenceField(Job, reverse_delete_rule=db.PULL))
```
## Tipos de Fields en Flask-MongoEngine

MongoEngine proporciona una variedad de campos (fields) que puedes usar para definir tus modelos. A continuación, te mostramos algunos de los campos más comunes, su descripción y usos típicos.

### StringField

`StringField` es un campo para almacenar cadenas de texto.

```python
class ExampleModel(db.Document):
    name = db.StringField(required=True, max_length=200)
```

- **Descripción**: Almacena una cadena de texto.
- **Usos comunes**: Nombres, descripciones, direcciones de correo electrónico, etc.
- **Parámetros comunes**:
  - `required`: Si se establece en `True`, el campo es obligatorio.
  - `max_length`: Define la longitud máxima de la cadena.

### IntField

`IntField` es un campo para almacenar enteros.

```python
class ExampleModel(db.Document):
    age = db.IntField(min_value=0)
```

- **Descripción**: Almacena un número entero.
- **Usos comunes**: Edades, cantidades, identificadores numéricos, etc.
- **Parámetros comunes**:
  - `min_value`: Define el valor mínimo permitido.
  - `max_value`: Define el valor máximo permitido.

### FloatField

`FloatField` es un campo para almacenar números de punto flotante.

```python
class ExampleModel(db.Document):
    score = db.FloatField()
```

- **Descripción**: Almacena un número de punto flotante.
- **Usos comunes**: Puntuaciones, precios, medidas, etc.
- **Parámetros comunes**:
  - `min_value`: Define el valor mínimo permitido.
  - `max_value`: Define el valor máximo permitido.

### BooleanField

`BooleanField` es un campo para almacenar valores booleanos (`True` o `False`).

```python
class ExampleModel(db.Document):
    is_active = db.BooleanField(default=True)
```

- **Descripción**: Almacena un valor booleano.
- **Usos comunes**: Indicadores de estado, banderas de activación/desactivación, etc.
- **Parámetros comunes**:
  - `default`: Define el valor predeterminado si no se proporciona uno.

### DateTimeField

`DateTimeField` es un campo para almacenar fechas y horas.

```python
from datetime import datetime

class ExampleModel(db.Document):
    created_at = db.DateTimeField(default=datetime.utcnow)
```

- **Descripción**: Almacena un objeto de fecha y hora.
- **Usos comunes**: Marcas de tiempo de creación/modificación, fechas de eventos, etc.
- **Parámetros comunes**:
  - `default`: Define un valor predeterminado, que puede ser una función como `datetime.utcnow`.

### ListField

`ListField` es un campo para almacenar listas de elementos.

```python
class ExampleModel(db.Document):
    tags = db.ListField(db.StringField())
```

- **Descripción**: Almacena una lista de elementos de un tipo específico.
- **Usos comunes**: Etiquetas, listas de valores, colecciones de elementos relacionados, etc.
- **Parámetros comunes**:
  - `field`: Define el tipo de los elementos dentro de la lista.

### ReferenceField

`ReferenceField` es un campo para almacenar una referencia a otro documento.

```python
class ExampleModel(db.Document):
    referenced_doc = db.ReferenceField('OtherModel')
```

- **Descripción**: Almacena una referencia a otro documento en una colección diferente.
- **Usos comunes**: Relaciones uno a uno o muchos a uno entre documentos, como una referencia de un trabajo a un empleador.
- **Parámetros comunes**:
  - `document_type`: Define el tipo de documento al que se referencia.
  - `reverse_delete_rule`: Define el comportamiento de eliminación en cascada.

### EmbeddedDocumentField

`EmbeddedDocumentField` es un campo para almacenar un documento embebido.

```python
class Address(db.EmbeddedDocument):
    street = db.StringField()
    city = db.StringField()

class ExampleModel(db.Document):
    address = db.EmbeddedDocumentField(Address)
```

- **Descripción**: Almacena un documento embebido dentro de otro documento.
- **Usos comunes**: Datos estructurados que pertenecen lógicamente a un único documento, como una dirección dentro de un perfil de usuario.
- **Parámetros comunes**:
  - `document_type`: Define el tipo de documento embebido.

### DynamicField

`DynamicField` es un campo para almacenar datos sin un tipo fijo.

```python
class ExampleModel(db.Document):
    dynamic_field = db.DynamicField()
```

- **Descripción**: Almacena datos que pueden ser de cualquier tipo.
- **Usos comunes**: Campos que pueden contener diferentes tipos de datos, campos que requieren flexibilidad en la estructura.
- **Parámetros comunes**:
  - No requiere parámetros adicionales específicos.

## Ejemplos Prácticos en una Aplicación de Búsqueda de Empleo

Para ilustrar el uso de estos campos en una aplicación de búsqueda de empleo, a continuación se presentan algunos ejemplos:

### Modelo de Empleador

```python
class Employer(db.Document):
    name = db.StringField(required=True, unique=True)
    industry = db.StringField()
    location = db.StringField()
    founded = db.IntField(min_value=1800)
    is_hiring = db.BooleanField(default=True)
    created_at = db.DateTimeField(default=datetime.utcnow)
```

### Modelo de Trabajo

```python
class Job(db.Document):
    title = db.StringField(required=True)
    description = db.StringField()
    requirements = db.ListField(db.StringField())
    employer = db.ReferenceField(Employer, reverse_delete_rule=db.CASCADE)
    salary = db.FloatField(min_value=0)
    posted_at = db.DateTimeField(default=datetime.utcnow)
```

### Modelo de Solicitante

```python
class Applicant(db.Document):
    name = db.StringField(required=True, min_length=2)
    email = db.EmailField(required=True, unique=True)
    resume = db.StringField(required=True)
    skills = db.ListField(db.StringField())
    applied_jobs = db.ListField(db.ReferenceField(Job, reverse_delete_rule=db.PULL))
```

Con esta explicación detallada de los tipos de fields en Flask-MongoEngine, puedes elegir y usar el campo adecuado según las necesidades de tu aplicación y asegurarte de que los datos se almacenen de manera eficiente y efectiva en MongoDB.

## Operaciones CRUD

A continuación, te muestro cómo realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en nuestros modelos de búsqueda de empleo.

### Crear un Documento

```python
@app.route('/add_employer')
def add_employer():
    employer = Employer(name='TechCorp', industry='Software', location='San Francisco')
    employer.save()
    return 'Employer added!'

@app.route('/add_job')
def add_job():
    employer = Employer.objects(name='TechCorp').first()
    if employer:
        job = Job(title='Software Engineer', description='Develop and maintain software.', requirements=['Python', 'Flask'], employer=employer)
        job.save()
        return 'Job added!'
    return 'Employer not found!'
```

### Leer Documentos

```python
@app.route('/jobs')
def get_jobs():
    jobs = Job.objects()
    return str(jobs)

@app.route('/job/<job_id>')
def get_job(job_id):
    job = Job.objects(id=job_id).first()
    return str(job) if job else 'Job not found!'
```

### Actualizar un Documento

```python
@app.route('/update_job/<job_id>')
def update_job(job_id):
    job = Job.objects(id=job_id).first()
    if job:
        job.update(description='Develop, maintain, and deploy software.')
        return 'Job updated!'
    return 'Job not found!'
```

### Eliminar un Documento

```python
@app.route('/delete_job/<job_id>')
def delete_job(job_id):
    job = Job.objects(id=job_id).first()
    if job:
        job.delete()
        return 'Job deleted!'
    return 'Job not found!'
```

## Consultas Avanzadas

### Filtrado y Ordenación

Puedes filtrar y ordenar los documentos en MongoEngine de manera sencilla. Aquí tienes un ejemplo de cómo hacerlo en la página de búsqueda de empleo:

```python
@app.route('/search_jobs')
def search_jobs():
    keyword = request.args.get('keyword', '')
    jobs = Job.objects(title__icontains=keyword).order_by('-posted_at')
    return str(jobs)
```

### Paginación

Para manejar la paginación, puedes usar el método `paginate` de MongoEngine.

### Ejemplo de Paginación

```python
@app.route('/jobs_paginated')
def get_paginated_jobs():
    page = int(request.args.get('page', 1))
    per_page = int(request.args.get('per_page', 10))
    jobs = Job.objects.paginate(page=page, per_page=per_page)
    return str(jobs.items)
```

## Validación y Schemas

MongoEngine permite agregar validaciones a los campos de los modelos para asegurarse de que los datos sean correctos.

### Validadores Comunes

- `required=True`: Asegura que el campo no esté vacío.
- `unique=True`: Asegura que el valor del campo sea único en la colección.
- `min_length`: Especifica la longitud mínima para el campo.

### Ejemplo de Validación

```python
class Applicant(db.Document):
    name = db.StringField(required=True, min_length=2)
    email = db.EmailField(required=True, unique=True)
    resume = db.StringField(required=True)
```

## Relaciones entre Documentos

### Documentos Embebidos

Los documentos embebidos permiten almacenar subdocumentos directamente dentro de un documento principal.

```python
class Address(db.EmbeddedDocument):
    street = db.StringField()
    city = db.StringField()

class Employer(db.Document):
    name = db.StringField(required=True, unique=True)
    address = db.EmbeddedDocumentField(Address)
```

### Documentos Referenciados

Los documentos referenciados permiten crear relaciones entre documentos de diferentes colecciones.

```python
class Job(db.Document):
    title = db.StringField(required=True)
    employer = db.ReferenceField(Employer)
```

## Paginación

Para manejar la paginación en MongoEngine, puedes usar el método `paginate`.

### Ejemplo de Paginación

```python
@app.route('/paginated_jobs')
def paginated_jobs():
    page = int(request.args.get('page', 1))
    per_page = int(request.args.get('per_page', 10))
    jobs = Job.objects.paginate(page=page, per_page=per_page)
    return render_template('jobs.html', jobs=jobs.items)
```

### Plantilla HTML para Paginación (templates/jobs.html)

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Jobs</title>
</head>
<body>
    <ul>
        {% for job in jobs %}
            <li>{{ job.title }} at {{ job.employer.name }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

## Serialización

Para serializar documentos MongoEngine en JSON, puedes usar el método `to_json`.

### Ejemplo de Serialización

```python
@app.route('/job_json/<job_id>')
def job_json(job_id):
    job = Job.objects(id=job_id).first()
    return job.to_json() if job else 'Job not found!'
```

## Integración con Formularios WTForms

Para integrar WTForms con MongoEngine y crear formularios, puedes definir formularios personalizados y usarlos en tus rutas.

### Definición del Formulario (forms.py)

```python
from flask_wtf import FlaskForm
from wtforms import StringField, TextAreaField, SubmitField
from wtforms.validators import DataRequired

class JobForm(FlaskForm):
    title = StringField('Job Title', validators=[DataRequired()])
    description = TextAreaField('Job Description', validators=[DataRequired()])
    submit = SubmitField('Post Job')
```

### Uso del Formulario en la Aplicación

```python
@app.route('/post_job', methods=['GET', 'POST'])
def post_job():
    form = JobForm()
    if form.validate_on_submit():
        employer = Employer.objects(name='TechCorp').first()
        if employer:
            job = Job(title=form.title.data, description=form.description.data, employer=employer)
            job.save()
            return 'Job

 posted!'
    return render_template('post_job.html', form=form)
```

### Plantilla HTML (templates/post_job.html)

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Post Job</title>
</head>
<body>
    <form method="POST">
        {{ form.hidden_tag() }}
        <p>
            {{ form.title.label }}<br>
            {{ form.title(size=32) }}<br>
            {% for error in form.title.errors %}
                <span style="color: red;">[{{ error }}]</span><br>
            {% endfor %}
        </p>
        <p>
            {{ form.description.label }}<br>
            {{ form.description(cols=32, rows=4) }}<br>
            {% for error in form.description.errors %}
                <span style="color: red;">[{{ error }}]</span><br>
            {% endfor %}
        </p>
        <p>{{ form.submit() }}</p>
    </form>
</body>
</html>
```

## Configuración de Middleware

Puedes agregar middleware para realizar tareas específicas antes o después de cada solicitud.

### Middleware de Logging

```python
import logging

@app.before_request
def before_request():
    logging.info(f"Request: {request.method} {request.url}")

@app.after_request
def after_request(response):
    logging.info(f"Response: {response.status_code}")
    return response
```

## Manejo de Errores

Puedes personalizar el manejo de errores en tu aplicación Flask.

### Personalización de Manejo de Errores

```python
@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404

@app.errorhandler(500)
def internal_server_error(e):
    return render_template('500.html'), 500
```

### Plantillas de Errores (templates/404.html y templates/500.html)

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Page Not Found</title>
</head>
<body>
    <h1>404 - Page Not Found</h1>
</body>
</html>
```

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Internal Server Error</title>
</head>
<body>
    <h1>500 - Internal Server Error</h1>
</body>
</html>
```

## Documentación de la API

Para más detalles sobre funciones, clases y métodos específicos, consulta la [documentación oficial de Flask-MongoEngine](https://flask-mongoengine.readthedocs.io/).

## Interfaz de Desarrollador

### Definición de Modelos Avanzados

Puedes definir modelos más complejos que incluyan documentos embebidos y referenciados.

```python
class Skill(db.EmbeddedDocument):
    name = db.StringField(required=True)
    level = db.StringField(choices=['Beginner', 'Intermediate', 'Advanced'])

class Applicant(db.Document):
    name = db.StringField(required=True)
    email = db.EmailField(required=True, unique=True)
    resume = db.StringField()
    skills = db.EmbeddedDocumentListField(Skill)
    applied_jobs = db.ListField(db.ReferenceField(Job, reverse_delete_rule=db.PULL))
```

### Consultas Complejas

Puedes realizar consultas más avanzadas que incluyan múltiples condiciones y ordenaciones.

```python
@app.route('/search_applicants')
def search_applicants():
    skill_name = request.args.get('skill', '')
    applicants = Applicant.objects(skills__name=skill_name).order_by('-name')
    return str(applicants)
```

Esta guía cubre los aspectos esenciales de Flask-MongoEngine para que puedas comenzar a construir aplicaciones Flask con MongoDB de manera eficiente y robusta. La aplicación de ejemplo se centra en una plataforma de búsqueda de empleo, pero puedes adaptar estos conceptos y ejemplos a cualquier tipo de aplicación.