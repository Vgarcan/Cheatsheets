### Guía Completa para Utilizar PyMongo con Flask

Esta guía te proporcionará una visión completa de cómo usar PyMongo para interactuar con MongoDB en una aplicación Flask. Incluiré una tabla de contenidos y detalladas explicaciones de código para ayudarte a entender las funcionalidades de PyMongo.

### Tabla de Contenidos

1. [Instalación de PyMongo y Flask](#instalacion)
2. [Configuración Básica de Flask y PyMongo](#configuracion-basica)
3. [Insertar Documentos](#insertar-documentos)
    - `insert_one`
    - `insert_many`
4. [Encontrar Documentos](#encontrar-documentos)
    - `find`
    - `find_one`
5. [Filtrar Documentos](#filtrar-documentos)
    - Operadores de comparación
    - Operadores lógicos
6. [Actualizar Documentos](#actualizar-documentos)
    - `update_one`
    - `update_many`
7. [Eliminar Documentos](#eliminar-documentos)
    - `delete_one`
    - `delete_many`
8. [Operaciones Avanzadas](#operaciones-avanzadas)
    - Contar documentos
    - Índices
    - Agregaciones
9. [Operadores de Consulta de MongoDB](#operadores-de-consulta)

### 1. Instalación de PyMongo y Flask <a name="instalacion"></a>

Para instalar PyMongo y Flask, usa pip:
```bash
pip install pymongo flask
```

### 2. Configuración Básica de Flask y PyMongo <a name="configuracion-basica"></a>

Aquí está cómo configurar una aplicación Flask y conectarse a MongoDB usando PyMongo:

```python
from flask import Flask, jsonify, request
from pymongo import MongoClient

app = Flask(__name__)

# Conéctate a MongoDB
client = MongoClient('localhost', 27017)
db = client.mydatabase
collection = db.users

@app.route('/')
def home():
    return "Bienvenido a la API de usuarios!"

if __name__ == '__main__':
    app.run(debug=True)
```

### 3. Insertar Documentos <a name="insertar-documentos"></a>

#### `insert_one`

```python
@app.route('/add_user', methods=['POST'])
def add_user():
    user = {
        "name": request.json['name'],
        "age": request.json['age'],
        "city": request.json['city']
    }
    collection.insert_one(user)
    return jsonify({"message": "Usuario añadido exitosamente!"}), 201
```

#### `insert_many`

```python
@app.route('/add_users', methods=['POST'])
def add_users():
    users = request.json['users']
    collection.insert_many(users)
    return jsonify({"message": "Usuarios añadidos exitosamente!"}), 201
```

### 4. Encontrar Documentos <a name="encontrar-documentos"></a>

#### `find`

```python
@app.route('/users', methods=['GET'])
def get_users():
    users = list(collection.find())
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

#### `find_one`

```python
@app.route('/user/<name>', methods=['GET'])
def get_user(name):
    user = collection.find_one({"name": name})
    if user:
        user['_id'] = str(user['_id'])
        return jsonify(user)
    else:
        return jsonify({"error": "Usuario no encontrado"}), 404
```

### 5. Filtrar Documentos <a name="filtrar-documentos"></a>

#### Operadores de comparación

**$gt** (mayor que):

```python
@app.route('/users/age_gt/<int:age>', methods=['GET'])
def get_users_age_gt(age):
    users = list(collection.find({"age": {"$gt": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$lt** (menor que):

```python
@app.route('/users/age_lt/<int:age>', methods=['GET'])
def get_users_age_lt(age):
    users = list(collection.find({"age": {"$lt": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$gte** (mayor o igual que):

```python
@app.route('/users/age_gte/<int:age>', methods=['GET'])
def get_users_age_gte(age):
    users = list(collection.find({"age": {"$gte": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$lte** (menor o igual que):

```python
@app.route('/users/age_lte/<int:age>', methods=['GET'])
def get_users_age_lte(age):
    users = list(collection.find({"age": {"$lte": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$eq** (igual a):

```python
@app.route('/users/age_eq/<int:age>', methods=['GET'])
def get_users_age_eq(age):
    users = list(collection.find({"age": {"$eq": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$ne** (no igual a):

```python
@app.route('/users/age_ne/<int:age>', methods=['GET'])
def get_users_age_ne(age):
    users = list(collection.find({"age": {"$ne": age}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

#### Operadores lógicos

**$and**:

```python
@app.route('/users/age_and/<int:age1>/<int:age2>', methods=['GET'])
def get_users_age_and(age1, age2):
    users = list(collection.find({"$and": [{"age": {"$gt": age1}}, {"age": {"$lt": age2}}]}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$or**:

```python
@app.route('/users/age_or/<int:age1>/<int:age2>', methods=['GET'])
def get_users_age_or(age1, age2):
    users = list(collection.find({"$or": [{"age": {"$lt": age1}}, {"age": {"$gt": age2}}]}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

**$not**:

```python
@app.route('/users/age_not/<int:age>', methods=['GET'])
def get_users_age_not(age):
    users = list(collection.find({"age": {"$not": {"$gt": age}}}))
    for user in users:
        user['_id'] = str(user['_id'])
    return jsonify(users)
```

### 6. Actualizar Documentos <a name="actualizar-documentos"></a>

#### `update_one`

```python
@app.route('/update_user/<name>', methods=['PUT'])
def update_user(name):
    updated_user = {
        "city": request.json['city']
    }
    result = collection.update_one({"name": name}, {"$set": updated_user})
    if result.matched_count > 0:
        return jsonify({"message": "Usuario actualizado exitosamente!"})
    else:
        return jsonify({"error": "Usuario no encontrado"}), 404
```

#### `update_many`

```python
@app.route('/update_users_age', methods=['PUT'])
def update_users_age():
    result = collection.update_many({"age": {"$gt": 30}}, {"$set": {"city": "Unknown"}})
    return jsonify({"message": f"{result.modified_count} usuarios actualizados"})
```

### 7. Eliminar Documentos <a name="eliminar-documentos"></a>

#### `delete_one`

```python
@app.route('/delete_user/<name>', methods=['DELETE'])
def delete_user(name):
    result = collection.delete_one({"name": name})
    if result.deleted_count > 0:
        return jsonify({"message": "Usuario eliminado exitosamente!"})
    else:
        return jsonify({"error": "Usuario no encontrado"}), 404
```

#### `delete_many`

```python
@app.route('/delete_users_age_gt/<int:age>', methods=['DELETE'])
def delete_users_age_gt(age):
    result = collection.delete_many({"age": {"$gt": age}})
    return jsonify({"message": f"{result.deleted_count} usuarios eliminados"})
```

### 8. Operaciones Avanzadas <a name="operaciones-avanzadas"></a>

#### Contar documentos

```python
@app.route('/count_users', methods=['GET'])
def count_users():
    count = collection.count_documents({})
    return jsonify({"count": count})
```

#### Índices

```python
@app.route('/create_index', methods=['POST'])
def create_index():
    collection.create_index("name")
    return jsonify({"message": "Índice creado en el campo 'name'"})
```

#### Agregaciones

```python
@app.route('/aggregate_users', methods=['GET'])
def aggregate_users():
    pipeline = [
        {"$match": {"age": {"$gt": 25}}},
        {"$group": {"_id": "$city", "average_age": {"$avg": "$age"}}}
    ]
    result = collection.aggregate(pipeline)


    aggregated_data = list(result)
    for doc in aggregated_data:
        doc['_id'] = str(doc['_id'])
    return jsonify(aggregated_data)
```

### 9. Operadores de Consulta de MongoDB <a name="operadores-de-consulta"></a>

#### Operadores de comparación

- **$eq**: Igual a (`{"field": {"$eq": value}}`)
- **$ne**: No igual a (`{"field": {"$ne": value}}`)
- **$gt**: Mayor que (`{"field": {"$gt": value}}`)
- **$gte**: Mayor o igual que (`{"field": {"$gte": value}}`)
- **$lt**: Menor que (`{"field": {"$lt": value}}`)
- **$lte**: Menor o igual que (`{"field": {"$lte": value}}`)
- **$in**: En (`{"field": {"$in": [value1, value2, ...]}}`)
- **$nin**: No en (`{"field": {"$nin": [value1, value2, ...]}}`)

#### Operadores lógicos

- **$and**: Y lógico (`{"$and": [{expression1}, {expression2}, ...]}`)
- **$or**: O lógico (`{"$or": [{expression1}, {expression2}, ...]}`)
- **$not**: No lógico (`{"field": {"$not": {expression}}}`)
- **$nor**: No o lógico (`{"$nor": [{expression1}, {expression2}, ...]}`)

#### Operadores de evaluación

- **$regex**: Coincidencia de expresión regular (`{"field": {"$regex": "pattern"}}`)
- **$mod**: Operación de módulo (`{"field": {"$mod": [divisor, remainder]}}`)
- **$text**: Búsqueda de texto (`{"$text": {"$search": "text"}}`)
- **$where**: Evaluar JavaScript (`{"$where": "this.field == value"}`)

#### Operadores de elemento

- **$exists**: Campo existe (`{"field": {"$exists": True/False}}`)
- **$type**: Tipo de campo (`{"field": {"$type": "type"}}`)

#### Operadores de arreglo

- **$all**: Coinciden todos los valores en la matriz (`{"field": {"$all": [value1, value2, ...]}}`)
- **$elemMatch**: Coincidencia de un elemento en una matriz (`{"field": {"$elemMatch": {expression}}}`)
- **$size**: Tamaño de la matriz (`{"field": {"$size": value}}`)

### Conclusión

Esta guía te proporciona una base sólida para trabajar con PyMongo en una aplicación Flask. Puedes realizar operaciones básicas y avanzadas, así como utilizar una variedad de operadores de consulta para interactuar de manera efectiva con MongoDB. ¿Hay algún tema específico que te gustaría explorar más a fondo?