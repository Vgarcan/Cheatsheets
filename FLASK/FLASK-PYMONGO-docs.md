### Guía Completa de Flask con PyMongo

## Tabla de Contenidos

- [Tabla de Contenidos](#tabla-de-contenidos)
- [Introducción a Flask-PyMongo](#introducción-a-flask-pymongo)
- [Instalación](#instalación)
- [Configuración Básica](#configuración-básica)
- [Modelos en Flask-PyMongo](#modelos-en-flask-pymongo)
- [Tipos de Fields en Flask-PyMongo](#tipos-de-fields-en-flask-pymongo)
  - [StringField](#stringfield)
  - [IntField](#intfield)
  - [FloatField](#floatfield)
  - [BooleanField](#booleanfield)
  - [DateTimeField](#datetimefield)
  - [ListField](#listfield)
  - [ReferenceField](#referencefield)
  - [EmbeddedDocumentField](#embeddeddocumentfield)
  - [DynamicField](#dynamicfield)
- [Operaciones CRUD](#operaciones-crud)
  - [Crear](#crear)
  - [Leer](#leer)
  - [Actualizar](#actualizar)
  - [Eliminar](#eliminar)
- [Consultas Avanzadas](#consultas-avanzadas)
- [Validación y Schemas](#validación-y-schemas)
- [Relaciones entre Documentos](#relaciones-entre-documentos)
- [Paginación](#paginación)
- [Serialización](#serialización)
- [Integración con Formularios WTForms](#integración-con-formularios-wtforms)
- [Configuración de Middleware](#configuración-de-middleware)
- [Manejo de Errores](#manejo-de-errores)
- [Documentación de la API](#documentación-de-la-api)
- [Interfaz de Desarrollador](#interfaz-de-desarrollador)

---

## Introducción a Flask-PyMongo

**Flask-PyMongo** es una extensión para Flask que facilita la integración de MongoDB en tus aplicaciones Flask. Proporciona una manera simple y eficiente de interactuar con bases de datos MongoDB, permitiendo realizar operaciones CRUD y consultas avanzadas de manera intuitiva.

## Instalación

Para instalar Flask y Flask-PyMongo, usa pip:

```bash
pip install Flask Flask-PyMongo
```

## Configuración Básica

Configura tu aplicación Flask para que se conecte a una base de datos MongoDB. Aquí tienes un ejemplo básico:

```python
from flask import Flask
from flask_pymongo import PyMongo

app = Flask(__name__)

# Configuración de la conexión a MongoDB
app.config["MONGO_URI"] = "mongodb://localhost:27017/mydatabase"
mongo = PyMongo(app)

@app.route('/')
def index():
    return 'Hello, MongoDB!'

if __name__ == '__main__':
    app.run(debug=True)
```

## Modelos en Flask-PyMongo

A diferencia de otros ORM como MongoEngine, PyMongo no utiliza clases de modelo. En cambio, se trabaja directamente con colecciones y documentos de MongoDB. A continuación, veremos cómo definir y utilizar los "campos" directamente en las operaciones CRUD.

## Tipos de Fields en Flask-PyMongo

En PyMongo, no se definen los tipos de campos de la misma manera que en ORMs como MongoEngine. Sin embargo, puedes almacenar diversos tipos de datos en documentos de MongoDB. Aquí hay algunos ejemplos de los tipos más comunes que se pueden utilizar:

### StringField

```python
user = {"name": "John Doe"}
mongo.db.users.insert_one(user)
```

### IntField

```python
user = {"age": 30}
mongo.db.users.insert_one(user)
```

### FloatField

```python
product = {"price": 19.99}
mongo.db.products.insert_one(product)
```

### BooleanField

```python
user = {"is_active": True}
mongo.db.users.insert_one(user)
```

### DateTimeField

```python
from datetime import datetime

event = {"event_date": datetime.utcnow()}
mongo.db.events.insert_one(event)
```

### ListField

```python
user = {"hobbies": ["reading", "swimming", "gaming"]}
mongo.db.users.insert_one(user)
```

### ReferenceField

En PyMongo, puedes usar referencias manualmente almacenando el ObjectId de un documento en otro documento.

```python
from bson.objectid import ObjectId

user_id = mongo.db.users.insert_one({"name": "John Doe"}).inserted_id
post = {"title": "My Post", "author_id": ObjectId(user_id)}
mongo.db.posts.insert_one(post)
```

### EmbeddedDocumentField

Puedes almacenar documentos anidados directamente en MongoDB.

```python
address = {"city": "New York", "zipcode": "10001"}
user = {"name": "John Doe", "address": address}
mongo.db.users.insert_one(user)
```

### DynamicField

MongoDB es schema-less, lo que significa que puedes agregar cualquier tipo de campo en cualquier momento.

```python
dynamic_doc = {"field1": "value1", "field2": 2, "field3": [1, 2, 3]}
mongo.db.dynamic.insert_one(dynamic_doc)
```

## Operaciones CRUD

### Crear

```python
user = {"name": "John Doe", "age": 30}
mongo.db.users.insert_one(user)
```

### Leer

```python
user = mongo.db.users.find_one({"name": "John Doe"})
print(user)
```

### Actualizar

```python
mongo.db.users.update_one({"name": "John Doe"}, {"$set": {"age": 31}})
```

### Eliminar

```python
mongo.db.users.delete_one({"name": "John Doe"})
```

## Consultas Avanzadas

Puedes realizar consultas más complejas utilizando operadores de MongoDB.

```python
# Consulta con operador $gt (mayor que)
users = mongo.db.users.find({"age": {"$gt": 25}})
for user in users:
    print(user)
```

## Validación y Schemas

Aunque MongoDB es schema-less, puedes usar bibliotecas como `jsonschema` para validar documentos antes de insertarlos en la base de datos.

```python
from jsonschema import validate, ValidationError

schema = {
    "type": "object",
    "properties": {
        "name": {"type": "string"},
        "age": {"type": "number"},
    },
    "required": ["name", "age"]
}

user = {"name": "John Doe", "age": 30}

try:
    validate(instance=user, schema=schema)
    mongo.db.users.insert_one(user)
except ValidationError as e:
    print(f"Validation error: {e.message}")
```

## Relaciones entre Documentos

Puedes crear relaciones entre documentos utilizando referencias.

```python
from bson.objectid import ObjectId

user_id = mongo.db.users.insert_one({"name": "Jane Doe"}).inserted_id
post = {"title": "My Post", "author_id": ObjectId(user_id)}
mongo.db.posts.insert_one(post)

# Consultar el post y obtener el autor
post = mongo.db.posts.find_one({"title": "My Post"})
author = mongo.db.users.find_one({"_id": post["author_id"]})
print(author)
```

## Paginación

Para paginar resultados, puedes usar los métodos `skip` y `limit` de MongoDB.

```python
page_number = 1
page_size = 10
users = mongo.db.users.find().skip((page_number - 1) * page_size).limit(page_size)
for user in users:
    print(user)
```

## Serialización

Puedes utilizar bibliotecas como `marshmallow` para serializar y deserializar documentos de MongoDB.

```python
from marshmallow import Schema, fields

class UserSchema(Schema):
    name = fields.Str()
    age = fields.Int()

user_schema = UserSchema()
user = mongo.db.users.find_one({"name": "John Doe"})
serialized_user = user_schema.dump(user)
print(serialized_user)
```

## Integración con Formularios WTForms

Puedes utilizar WTForms para crear formularios en tu aplicación Flask y luego procesar los datos con PyMongo.

```python
from flask import Flask, render_template, request
from flask_wtf import FlaskForm
from wtforms import StringField, IntegerField, SubmitField
from wtforms.validators import DataRequired

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key'

class UserForm(FlaskForm):
    name = StringField('Name', validators=[DataRequired()])
    age = IntegerField('Age', validators=[DataRequired()])
    submit = SubmitField('Submit')

@app.route('/new_user', methods=['GET', 'POST'])
def new_user():
    form = UserForm()
    if form.validate_on_submit():
        user = {"name": form.name.data, "age": form.age.data}
        mongo.db.users.insert_one(user)
        return 'User created!'
    return render_template('new_user.html', form=form)

if __name__ == '__main__':
    app.run(debug=True)
```

## Configuración de Middleware

Puedes utilizar middleware en Flask para añadir funcionalidades a tu aplicación.

```python
@app.before_request
def before_request():
    print("Esta función se ejecutará antes de cada petición")

@app.after_request
def after_request(response):
    print("Esta función se ejecutará después de cada petición")
    return response
```

## Manejo de Errores

Para manejar errores en Flask, puedes utilizar decoradores de error.

```python
@app.errorhandler(404)
def not_found(error):
    return "Página no encontrada", 404

@app.errorhandler(500)
def server_error(error):
    return "Error interno del servidor", 500
```

## Documentación de la API

Puedes documentar tu API utilizando Swagger con la extensión Flask-Swagger.

```bash
pip install flask-swagger
```

```python
from flask_swagger import swagger

@app.route("/spec")
def spec():
    return jsonify(swagger(app))
```

## Interfaz de Desarrollador

Para crear una interfaz de desarrollador, puedes utilizar herramientas como Flask-Admin.

```bash
pip install flask-admin
```

```python
from flask_admin import Admin, BaseView, expose

admin = Admin(app, name='MyApp', template_mode='bootstrap3')

class MyView(BaseView):
    @expose('/')
    def index(self):
        return self.render('admin/index.html')

admin.add_view(MyView(name='Hello'))

if __name__ == '__main__':
    app.run(debug=True)
```

---

Esta guía te proporciona una comprensión completa de cómo utilizar Flask con PyMongo, cubriendo desde la instalación y configuración básica hasta operaciones avanzadas y la integración con herramientas adicionales.