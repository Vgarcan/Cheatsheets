# Guía de Métodos de Flask-PyMongo

## Tabla de Contenidos

1. [Introducción a Flask-PyMongo](#introducción-a-flask-pymongo)
2. [Métodos de la Clase `flask_pymongo.PyMongo`](#métodos-de-la-clase-flask_pymongopymongo)
   - [init_app(app)](#init_appapp)
   - [db](#db)
   - [cx](#cx)
   - [close()](#close)
   - [save(document, manipulate=True, check_keys=True, **kwargs)](#savedocument-manipulatetrue-check_keystue-kwargs)
   - [insert(document_or_documents, manipulate=True, check_keys=True, continue_on_error=False, **kwargs)](#insertdocument_or_documents-manipulatetrue-check_keystue-continue_on_errorfalse-kwargs)
   - [update(spec, document, upsert=False, manipulate=False, multi=False, check_keys=True, **kwargs)](#updatespec-document-upsertfalse-manipulatefalse-multifalse-check_keystue-kwargs)
   - [remove(spec_or_id=None, multi=True, **kwargs)](#removespec_or_idnone-multitrue-kwargs)
   - [find_one(spec_or_id=None, *args, **kwargs)](#find_one-spec_or_idnone-args-kwargs)
   - [find(spec=None, *args, **kwargs)](#findspecnone-args-kwargs)
   - [find_and_modify(query=None, update=None, upsert=False, sort=None, full_response=False, manipulate=False, **kwargs)](#find_and_modifyquerynone-updatenone-upsertfalse-sortnone-full_responsefalse-manipulatefalse-kwargs)
   - [count()](#count)
   - [create_index(keys, **kwargs)](#create_indexkeys-kwargs)
   - [drop_index(index_or_name)](#drop_indexindex_or_name)
   - [drop_indexes()](#drop_indexes)
   - [ensure_index(keys, **kwargs)](#ensure_indexkeys-kwargs)
   - [index_information()](#index_information)
   - [aggregate(pipeline, **kwargs)](#aggregatepipeline-kwargs)
   - [distinct(key, **kwargs)](#distinctkey-kwargs)
   - [group(key, condition, initial, reduce, finalize=None, **kwargs)](#groupkey-condition-initial-reduce-finalizenone-kwargs)
   - [map_reduce(map, reduce, out, full_response=False, **kwargs)](#map_reducemap-reduce-out-full_responsefalse-kwargs)
   - [inline_map_reduce(map, reduce, full_response=False, **kwargs)](#inline_map_reducemap-reduce-full_responsefalse-kwargs)
   - [options()](#options)
   - [rename(name, **kwargs)](#rename-name-kwargs)
   - [save_file(filename, content_type=None, content=None, meta=None, **kwargs)](#save_filefilename-content_tynone-contentnone-metanone-kwargs)
   - [get_file(filename_or_id)](#get_filefilename_or_id)
   - [delete(filename_or_id)](#deletefilename_or_id)
   - [list()](#list)
3. [Métodos de la Clase `flask_pymongo.wrappers.Collection`](#métodos-de-la-clase-flask_pymongowrapperscollection)
   - [initialize_ordered_bulk_op()](#initialize_ordered_bulk_op)
   - [initialize_unordered_bulk_op()](#initialize_unordered_bulk_op)
   - [bulk_write(requests, ordered=True, bypass_document_validation=False)](#bulk_writerequests-orderedtrue-bypass_document_validationfalse)
   - [count_documents(filter, session=None)](#count_documentsfilter-sessionnone)
   - [delete_many(filter, collation=None, session=None)](#delete_manyfilter-collationnone-sessionnone)
   - [delete_one(filter, collation=None, session=None)](#delete_onefilter-collationnone-sessionnone)
   - [distinct(key, filter=None, session=None, **kwargs)](#distinctkey-filternone-sessionnone-kwargs)
   - [drop(session=None)](#dropsessionnone)
   - [estimated_document_count(filter=None, **kwargs)](#estimated_document_countfilternone-kwargs)
   - [find(filter=None, *args, **kwargs)](#findfilternone-args-kwargs)
   - [find_one(filter=None, *args, **kwargs)](#find_onefilternone-args-kwargs)
   - [find_one_and_delete(filter, projection=None, sort=None, session=None, **kwargs)](#find_one_and_deletefilter-projectionnone-sortnone-sessionnone-kwargs)
   - [find_one_and_replace(filter, replacement, projection=None, sort=None, upsert=False, return_document=ReturnDocument.BEFORE, session=None, **kwargs)](#find_one_and_replacefilter-replacement-projectionnone-sortnone-upsertfalse-return_documentreturndocumentbefore-sessionnone-kwargs)
   - [find_one_and_update(filter, update, projection=None, sort=None, upsert=False, return_document=ReturnDocument.BEFORE, session=None, **kwargs)](#find_one_and_updatefilter-update-projectionnone-sortnone-upsertfalse-return_documentreturndocumentbefore-sessionnone-kwargs)
   - [insert_many(documents, ordered=True, bypass_document_validation=False, session=None)](#insert_manydocuments-orderedtrue-bypass_document_validationfalse-sessionnone)
   - [insert_one(document, bypass_document_validation=False, session=None)](#insert_onedocument-bypass_document_validationfalse-sessionnone)
   - [list_indexes(session=None)](#list_indexes-sessionnone)
   - [replace_one(filter, replacement, upsert=False, bypass_document_validation=False, collation=None, session=None)](#replace_onefilter-replacement-upsertfalse-bypass_document_validationfalse-collationnone-sessionnone)
   - [update_many(filter, update, upsert=False, bypass_document_validation=False, collation=None, array_filters=None, session=None)](#update_manyfilter-update-upsertfalse-bypass_document_validationfalse-collationnone-array_filtersnone-sessionnone)
   - [update_one(filter, update, upsert=False, bypass_document_validation=False, collation=None, array_filters=None, session=None)](#update_onefilter-update-upsertfalse-bypass_document_validationfalse-collationnone-array_filtersnone-sessionnone)
4. [Ejemplos de Uso](#ejemplos-de-uso)


---

## 1. Introducción a Flask-PyMongo

Flask-PyMongo es una extensión para Flask que facilita la integración de MongoDB en aplicaciones Flask. Proporciona una interfaz simplificada para interactuar con bases de datos MongoDB desde el código Python de Flask. A continuación, se detallan los métodos disponibles y cómo se utilizan en el contexto de desarrollo web.

## Métodos de la Clase `flask_pymongo.PyMongo`

La clase `flask_pymongo.PyMongo` proporciona una interfaz para interactuar con MongoDB en aplicaciones Flask mediante el paquete Flask-PyMongo. Esta clase facilita la conexión, configuración y gestión de bases de datos MongoDB dentro del contexto de una aplicación Flask, permitiendo operaciones avanzadas de base de datos y manipulación de datos estructurados de manera eficiente.

**Uso de los Métodos:**
Los métodos de `flask_pymongo.PyMongo` son fundamentales para configurar la conexión con una base de datos MongoDB y realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en las colecciones de la base de datos.

**Importancia y Aplicación:**
1. **Configuración de la Conexión:** `init_app`, `connect`, `close_db`, permiten establecer y gestionar la conexión con una instancia de MongoDB, asegurando que la aplicación Flask pueda acceder y manipular datos de manera efectiva.

2. **Acceso a Colecciones:** `db`, `cx`, `gridfs`, permiten obtener referencias a colecciones específicas dentro de la base de datos, lo que facilita la ejecución de operaciones CRUD en documentos almacenados.

3. **Operaciones CRUD:** Métodos como `insert_one`, `insert_many`, `find_one`, `find`, `update_one`, `update_many`, `delete_one`, `delete_many` permiten realizar operaciones de creación, lectura, actualización y eliminación en documentos MongoDB, asegurando la integridad y consistencia de los datos.

4. **Gestión de Sesiones:** `start_session`, `end_session`, permiten gestionar sesiones de MongoDB, lo que es crucial para transacciones y operaciones que requieren consistencia y atomicidad.

5. **Funcionalidades Avanzadas:** `aggregate`, `map_reduce`, `watch`, permiten realizar operaciones avanzadas como agregaciones, map-reduce y cambios en tiempo real (watch) en la base de datos MongoDB, ofreciendo capacidades analíticas y de seguimiento en tiempo real.

Estos métodos son esenciales para construir aplicaciones web escalables y robustas con Flask y MongoDB, proporcionando herramientas poderosas para el manejo eficiente de datos y la integración de capacidades avanzadas de base de datos en el desarrollo de aplicaciones.

### `init_app(app)`

**Uso:**
Inicializa la extensión Flask-PyMongo con una instancia de la aplicación Flask (`app`), configurando la conexión a la base de datos MongoDB.

**Atributos:**
- `app`: Instancia de la aplicación Flask.

**Ejemplo:**
```python
from flask import Flask
from flask_pymongo import PyMongo

app = Flask(__name__)
mongo = PyMongo()
mongo.init_app(app)
```

### `db`

**Uso:**
Proporciona acceso a la base de datos de MongoDB configurada para la aplicación Flask actual.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
users_collection = mongo.db.users
```

### `cx`

**Uso:**
Proporciona acceso al cliente MongoDB configurado para la aplicación Flask actual.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
client = mongo.cx
```

### `close()`

**Uso:**
Cierra la conexión activa al cliente de MongoDB.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
mongo.close()
```

### `save(document, manipulate=True, check_keys=True, **kwargs)`

**Uso:**
Guarda un documento en la colección especificada.

**Atributos:**
- `document`: Documento a guardar.
- `manipulate`: Indica si aplicar manipulaciones al documento (por defecto `True`).
- `check_keys`: Indica si verificar las claves del documento (por defecto `True`).

**Ejemplo:**
```python
new_user = {"name": "Alice", "age": 30}
mongo.db.users.save(new_user)
```

### `insert(document_or_documents, manipulate=True, check_keys=True, continue_on_error=False, **kwargs)`

**Uso:**
Inserta uno o varios documentos en la colección especificada.

**Atributos:**
- `document_or_documents`: Documento o lista de documentos a insertar.
- `manipulate`: Indica si aplicar manipulaciones a los documentos (por defecto `True`).
- `check_keys`: Indica si verificar las claves de los documentos (por defecto `True`).
- `continue_on_error`: Indica si continuar con otros documentos en caso de error (por defecto `False`).

**Ejemplo:**
```python
new_users = [{"name": "Bob", "age": 25}, {"name": "Charlie", "age": 35}]
mongo.db.users.insert(new_users)
```

### `update(spec, document, upsert=False, manipulate=False, multi=False, check_keys=True, **kwargs)`

**Uso:**
Actualiza documentos en la colección que coincidan con el criterio especificado.

**Atributos:**
- `spec`: Especificación de criterios de selección para la actualización.
- `document`: Documento con las actualizaciones a aplicar.
- `upsert`: Si es `True`, inserta un nuevo documento si no se encuentra ningún documento que coincida con el criterio de selección.
- `manipulate`: Si es `True`, permite manipular los documentos en la actualización.
- `multi`: Si es `True`, actualiza múltiples documentos que coincidan con el criterio de selección.
- `check_keys`: Si es `True`, comprueba las claves en el documento a actualizar.
- `**kwargs`: Otros argumentos opcionales para la operación de actualización.

**Ejemplo:**
```python
# Actualiza el documento con el nombre "Alice"
spec = {"name": "Alice"}
document = {"$set": {"age": 31}}
mongo.db.users.update(spec, document)

# Actualiza todos los documentos con la categoría "Young"
spec = {"category": "Young"}
document = {"$inc": {"age": 1}}
mongo.db.users.update(spec, document, multi=True)
```

### `remove(spec_or_id=None, multi=True, **kwargs)`

**Uso:**
Elimina documentos que coincidan con el filtro especificado.

**Atributos:**
- `spec_or_id`: Especificación o ID del documento a eliminar.
- `multi`: Indica si eliminar múltiples documentos que coincidan (por defecto `True`).

**Ejemplo:**
```python
mongo.db.users.remove({"age": {"$lt": 25}})
```

### `find_one(spec_or_id=None, *args, **kwargs)`

**Uso:**
Encuentra un documento que coincida con el filtro especificado.

**Atributos:**
- `spec_or_id`: Especificación o ID del documento a encontrar.
- Opciones adicionales como `projection`, `sort`, entre otros.

**Ejemplo:**
```python
user = mongo.db.users.find_one({"name": "Alice"})
print(user)
```

### `find(spec=None, *args, **kwargs)`

**Uso:**
Encuentra documentos que coincidan con el filtro especificado.

**Atributos:**
- `spec`: Especificación del filtro para encontrar documentos.
- Opciones adicionales como `projection`, `skip`, `limit`, `sort`, entre otros.

**Ejemplo:**
```python
users = mongo.db.users.find({"age": {"$gte": 25}})
for user in users:
    print(user)
```

### `find_and_modify(query=None, update=None, upsert=False, sort=None, full_response=False, manipulate=False, **kwargs)`

**Uso:**
Encuentra y modifica un documento que coincida con el filtro especificado.

**Atributos:**
- `query`: Filtro para encontrar el documento.
- `update`: Actualización a aplicar.
- `upsert`: Indica si insertar un nuevo documento si no se encuentra (por defecto `False`).
- `sort`: Opcional, criterio de ordenación.
- `full_response`: Indica si devolver una respuesta completa (por defecto `False`).
- `manipulate`: Indica si aplicar manipulaciones al documento (por defecto `False`).

**Ejemplo:**
```python
updated_user = mongo.db.users.find_and_modify({"name": "Alice"}, {"$set": {"age": 31}}, full_response=True)
print(updated_user)
```

### `count()`

**Uso:**
Devuelve el número de documentos en la colección.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
count = mongo.db.users.count()
print(f"Número de documentos: {count}")
```

### `create_index(keys, **kwargs)`

**Uso:**
Crea un índice en la colección.

**Atributos:**
- `keys`: Claves para el índice.
- Opciones adicionales como `name`, `unique`, `background`, entre otros.

**Ejemplo:**
```python
mongo.db.users.create_index([("name", pymongo.ASCENDING)], unique=True)
```

### `drop_index(index_or_name)`

**Uso:**
Elimina un índice de la colección.

**Atributos:**
- `index_or_name`: Índice o nombre del índice a eliminar.

**Ejemplo:**
```python
mongo.db.users.drop_index("index_name")
```

### `drop_indexes()`

**Uso:**
Elimina todos los índices de la colección.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
mongo.db.users.drop_indexes()
```

### `ensure_index(keys, **kwargs)`

**Uso:**
Asegura la existencia de un índice en la colección.

**Atributos:**
- `keys`: Claves para el índice.
- Opciones adicionales como `name`, `unique`, `background`, entre otros.

**Ejemplo:**
```python
mongo.db.users.ensure_index([("name", pymongo.ASCENDING)], unique=True)
```

### `index_information()`

**Uso:**
Devuelve información sobre los índices de la colección.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
indexes_info = mongo.db.users.index_information()
print(indexes_info)
```

### `aggregate(pipeline, **kwargs)`

**Uso:**
Realiza una operación de agregación en la colección.

**Atributos:**
- `pipeline`: Lista de etapas de la operación de agregación.
- Opciones adicionales como `allowDiskUse`, `batchSize`, entre otros.

**Ejemplo:**
```python
pipeline = [{"$group": {"_id": "$name", "total": {"$sum": 1}}}]
result = mongo.db.users.aggregate(pipeline)
for doc in result:
    print(doc)
```

### `distinct(key, **kwargs)`

**Uso:**
Devuelve valores distintos para la clave especificada en la colección.

**Atributos:**
- `key`: Clave para aplicar la operación `distinct`.
- Opciones adicionales como `filter`, `session`, entre otros.

**Ejemplo:**
```python
distinct_names = mongo.db.users.distinct("name")
print(distinct_names)
```

### `group(key, condition, initial, reduce, finalize=None, **kwargs)`

**Uso:**
Agrupa documentos en la colección según la clave y la condición dadas.

**Atributos:**
- `key`: Clave para agrupar documentos.
- `condition`: Condición para el grupo.
- `initial`: Valor inicial para el grupo.
- `reduce`: Función de reducción para el grupo.
- `finalize`: Función final para el grupo (opcional).
- Opciones adicionales como `keyf`, `finalize`, entre otros.

**Ejemplo:**
```python
key = {"age": 1}
condition = {"age": {"$gte": 30}}
initial = {"count": 0}
reduce = "function(obj, prev) { prev.count++; }"
result = mongo.db.users.group(key, condition, initial, reduce)
print(result)
```

### `map_reduce(map, reduce, out, full_response=False, **kwargs)`

**Uso:**
Aplica una operación de map-reduce en la colección.

**Atributos:**
- `map`: Función de mapeo para map-reduce.
- `reduce`: Función de reducción para map-reduce.
- `out`: Especificación para la salida de map-reduce.
- `full_response`: Indica si devolver una respuesta completa (por defecto `False`).
- Opciones adicionales como `query`, `sort`, `limit`, entre otros.

**Ejemplo:**
```python
from bson.code import Code

map_func = Code("function() { emit(this.name, 1); }")
reduce_func = Code("function(key, values) { return Array.sum(values); }")
result = mongo.db.users.map_reduce(map_func, reduce_func, "myresult")
print(result)
```

### `inline_map_reduce(map, reduce, full_response=False, **kwargs)`

**Uso:**
Aplica una operación de map-reduce en línea en la colección.

**Atributos:**
- `map`: Función de mapeo para map-reduce.
- `reduce`: Función de reducción para map-reduce.
- `full_response`: Indica si devolver una respuesta completa (por defecto `False`).
- Opciones adicionales como `query`, `sort`, `limit`, entre otros.

**Ejemplo:**
```python
from bson.code import Code

map_func = Code("function() { emit(this.name, 1); }")
reduce_func = Code("function(key, values) { return Array.sum(values); }")
result = mongo.db.users.inline_map_reduce(map_func, reduce_func, full_response=True)
print(result)
```

### `options()`

**Uso:**
Devuelve las opciones actuales para la colección.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
options = mongo.db.users.options()
print(options)
```

### `rename(name, **kwargs)`

**Uso:**
Renombra la colección actual.

**Atributos:**
- `name`: Nuevo nombre para la colección.
- Opciones adicionales como `dropTarget`, entre otros.

**Ejemplo:**
```python
mongo.db.users.rename("new_users")
```

### `save_file(filename, content_type=None, content=None, meta=None, **kwargs)`

**Uso:**
Guarda un archivo en la colección GridFS.

**Atributos:**
- `filename`: Nombre del archivo a guardar.
- `content_type`: Tipo de contenido del archivo.
- `content`: Contenido del archivo.
- `meta`: Metadatos adicionales para el archivo.
- Opciones adicionales como `chunk_size`, `aliases`, entre otros.

**Ejemplo:**
```python
with open("example.txt", "rb") as f:
    mongo.db.users.save_file("example.txt", content_type="text/plain", content=f)
```

### `get_file(filename_or_id)`

**Uso:**
Recupera un archivo de la colección GridFS.

**Atributos:**
- `filename_or_id`: Nombre de archivo o ID del archivo a recuperar.

**Ejemplo:**
```

python
output_stream = open("example_output.txt", "wb")
mongo.db.users.get_file("example.txt").seek(0)
shutil.copyfileobj(mongo.db.users.get_file("example.txt"), output_stream)
output_stream.close()
```

### `delete(filename_or_id)`

**Uso:**
Elimina un archivo de la colección GridFS.

**Atributos:**
- `filename_or_id`: Nombre de archivo o ID del archivo a eliminar.

**Ejemplo:**
```python
mongo.db.users.delete("example.txt")
```

### `list()`

**Uso:**
Devuelve una lista de los nombres de todas las colecciones en la base de datos.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
collections = mongo.db.list()
print(collections)
```

## Métodos de la Clase `flask_pymongo.wrappers.Collection`

La clase `flask_pymongo.wrappers.Collection` proporciona una interfaz conveniente para interactuar con colecciones MongoDB dentro de aplicaciones Flask que utilizan Flask-PyMongo. Estos métodos permiten realizar operaciones fundamentales de base de datos como inserción, consulta, actualización y eliminación de documentos. Cada método está diseñado para ofrecer flexibilidad y eficiencia en la gestión de datos, aprovechando las funcionalidades robustas de MongoDB a través de PyMongo.

**Uso de los Métodos:**
Los métodos de `flask_pymongo.wrappers.Collection` se utilizan para ejecutar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en documentos almacenados en una colección MongoDB. Desde la inserción inicial de datos hasta la consulta y actualización de información, estos métodos son esenciales para cualquier aplicación web que requiera almacenamiento persistente y manipulación de datos estructurados.

**Importancia y Aplicación:**
1. **Inserción de Datos:** `insert_one`, `insert_many` permiten agregar nuevos documentos a la colección, facilitando la captura y almacenamiento de información generada por los usuarios o por el sistema.
   
2. **Consulta y Lectura:** `find`, `find_one` permiten recuperar datos según criterios específicos, facilitando la visualización y presentación de información relevante para los usuarios o para procesos internos de la aplicación.

3. **Actualización de Datos:** `update_one`, `update_many` permiten modificar documentos existentes en respuesta a cambios en el estado de la aplicación o a nuevas entradas de datos, asegurando que la información se mantenga actualizada y precisa.

4. **Eliminación de Datos:** `delete_one`, `delete_many` permiten eliminar documentos que ya no son necesarios o que han sido marcados para su eliminación, asegurando la integridad de los datos y cumpliendo con las regulaciones de privacidad.

Estos métodos forman la columna vertebral de cualquier sistema de gestión de datos basado en MongoDB, ofreciendo herramientas poderosas para la administración eficiente y efectiva de datos en entornos web. Su integración con Flask-PyMongo facilita la construcción de aplicaciones robustas y escalables que pueden manejar grandes volúmenes de información de manera confiable.

### `initialize_ordered_bulk_op()`

**Uso:**
Inicializa una operación de escritura en bloque ordenada.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
bulk = mongo.db.users.initialize_ordered_bulk_op()
```

### `initialize_unordered_bulk_op()`

**Uso:**
Inicializa una operación de escritura en bloque desordenada.

**Atributos:**
- Ninguno.

**Ejemplo:**
```python
bulk = mongo.db.users.initialize_unordered_bulk_op()
```

### `bulk_write(requests, ordered=True, bypass_document_validation=False)`

**Uso:**
Ejecuta una operación de escritura en bloque.

**Atributos:**
- `requests`: Lista de solicitudes de escritura.
- `ordered`: Indica si la operación es ordenada (por defecto `True`).
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).

**Ejemplo:**
```python
requests = [
    pymongo.InsertOne({"name": "Alice", "age": 30}),
    pymongo.UpdateOne({"name": "Bob"}, {"$set": {"age": 28}}),
    pymongo.DeleteOne({"name": "Charlie"})
]
result = mongo.db.users.bulk_write(requests)
print(result.bulk_api_result)
```

### `count_documents(filter, session=None)`

**Uso:**
Cuenta los documentos que coinciden con el filtro especificado.

**Atributos:**
- `filter`: Filtro para contar documentos.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
count = mongo.db.users.count_documents({"age": {"$gte": 25}})
print(f"Número de documentos: {count}")
```

### `delete_many(filter, collation=None, session=None)`

**Uso:**
Elimina varios documentos que coincidan con el filtro especificado.

**Atributos:**
- `filter`: Filtro para eliminar documentos.
- `collation`: Opcional, opciones de colación para la comparación de cadenas.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
result = mongo.db.users.delete_many({"age": {"$lt": 25}})
print(f"Documentos eliminados: {result.deleted_count}")
```

### `delete_one(filter, collation=None, session=None)`

**Uso:**
Elimina un documento que coincida con el filtro especificado.

**Atributos:**
- `filter`: Filtro para eliminar un documento.
- `collation`: Opcional, opciones de colación para la comparación de cadenas.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
result = mongo.db.users.delete_one({"name": "Alice"})
print(f"Documento eliminado: {result.deleted_count}")
```

### `distinct(key, filter=None, session=None, **kwargs)`

**Uso:**
Devuelve valores distintos para la clave especificada en la colección.

**Atributos:**
- `key`: Clave para aplicar la operación `distinct`.
- `filter`: Filtro para la operación `distinct`.
- `session`: Opcional, sesión de MongoDB.
- Opciones adicionales.

**Ejemplo:**
```python
distinct_names = mongo.db.users.distinct("name")
print(distinct_names)
```

### `drop(session=None)`

**Uso:**
Elimina la colección actual.

**Atributos:**
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
mongo.db.users.drop()
```

### `estimated_document_count(filter=None, **kwargs)`

**Uso:**
Devuelve una estimación del número de documentos en la colección.

**Atributos:**
- `filter`: Filtro para estimar el número de documentos.
- Opciones adicionales como `skip`, `limit`, entre otros.

**Ejemplo:**
```python
count = mongo.db.users.estimated_document_count()
print(f"Número estimado de documentos: {count}")
```

### `insert_many(documents, ordered=True, bypass_document_validation=False, session=None)`

**Uso:**
Inserta varios documentos en la colección.

**Atributos:**
- `documents`: Lista de documentos a insertar.
- `ordered`: Indica si la operación es ordenada (por defecto `True`).
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
new_users = [{"name": "Bob", "age": 25}, {"name": "Charlie", "age": 35}]
result = mongo.db.users.insert_many(new_users)
print(result.inserted_ids)
```

### `insert_one(document, bypass_document_validation=False, session=None)`

**Uso:**
Inserta un documento en la colección.

**Atributos:**
- `document`: Documento a insertar.
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
new_user = {"name": "Alice", "age": 30}
result = mongo.db.users.insert_one(new_user)
print(result.inserted_id)
```

### `replace_one(filter, replacement, upsert=False, bypass_document_validation=False, collation=None, session=None)`

**Uso:**
Reemplaza un documento que coincida con el filtro especificado.

**Atributos:**
- `filter`: Filtro para encontrar el documento a reemplazar.
- `replacement`: Nuevo documento con el que reemplazar.
- `upsert`: Indica si insertar un nuevo documento si no se encuentra (por defecto `False`).
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).
- `collation`: Opcional, opciones de colación para la comparación de cadenas.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
filter = {"name": "Alice"}
replacement = {"name": "Alicia", "age": 31}
result = mongo.db.users.replace_one(filter, replacement)
print(f"Documentos reemplazados: {result.modified_count}")
```

### `update_many(filter, update, upsert=False, bypass_document_validation=False, collation=None, array_filters=None, session=None)`

**Uso:**
Actualiza varios documentos que coincidan con el filtro especificado.

**Atributos:**
- `filter`: Filtro para encontrar los documentos a actualizar.
- `update`: Actualización a aplicar.
- `upsert`: Indica si insertar un nuevo documento si no se encuentra (por defecto `False`).
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).
- `collation`: Opcional, opciones de colación para la comparación de cadenas.
- `array_filters`: Opcional, filtros para identificar elementos a actualizar en matrices.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
filter = {"age": {"$lt": 25}}
update = {"$set": {"category": "Young"}}
result = mongo.db.users.update_many(filter, update)
print(f"Documentos actualizados: {result.modified_count}")
```

### `update_one(filter, update, upsert=False, bypass_document_validation=False, collation=None, array_filters=None, session=None)`

**Uso:**
Actualiza un documento que coincida con el filtro especificado.

**Atributos:**
- `filter`: Filtro para encontrar el documento a actualizar.
- `update`: Actualización a aplicar.
- `upsert`: Indica si insertar un nuevo documento si no se encuentra (por defecto `False`).
- `bypass_document_validation`: Indica si omitir la validación de documentos (por defecto `False`).
- `collation`: Opcional, opciones de colación para la comparación de cadenas.
- `array_filters`: Opcional, filtros para identificar elementos a actualizar en matrices.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
filter = {"name": "Alice"}
update = {"$set": {"age": 31}}
result = mongo.db.users.update_one(filter, update)
print(f"Documentos actualizados: {result.modified_count}")
```

### `with_options(**kwargs)`

**Uso:**
Crea una copia de la colección con opciones especificadas.

**Atributos:**
- Opciones como `read_preference`, `write_concern`, `read_concern`, entre otros.

**Ejemplo:**
```python
users_collection = mongo.db.users.with_options(read_preference=ReadPreference.SECONDARY)
```

### `watch(pipeline=None, full_document='default', resume_after=None, start_after=None, max_await_time_ms=None, batch_size=None, collation=None, session=None, **kwargs)`

**Uso:**
Inicia un flujo de cambios en tiempo real en la colección.

**Atributos:**
- `pipeline`: Opcional, lista de etapas de agregación para el flujo de cambios.
- `full_document`: Tipo de documento completo para cambios (`'default'`, `'updateLookup'`).
- `resume_after`: Punto de resumen después del cual continuar.
- `start_after`: Punto de inicio después del cual comenzar.
- `max_await_time_ms`: Tiempo máximo de espera en milisegundos.
- `batch_size`: Tamaño del lote para resultados.
- `collation`: Opciones de colación para la comparación de cadenas.
- `session`: Opcional, sesión de MongoDB.

**Ejemplo:**
```python
with mongo.db.users.watch() as stream:
    for change in stream:
        print(change)
```

## Ejemplos de Uso

### Ejemplo 1: Inserción de un Nuevo Documento

```python
from flask import Flask
from flask_pymongo import PyMongo

app = Flask(__name__)
app.config["MONGO_URI"] = "mongodb://localhost:27017/mydatabase"
mongo = PyMongo(app)

# Inserta un nuevo usuario en la colección 'users'
@app.route('/insert_user')
def insert_user():
    new_user = {"name": "Alice", "age": 30, "city": "New York"}
    result = mongo.db.users.insert_one(new_user)
    return f"Usuario insertado con ID: {result.inserted_id}"

if __name__ == '__main__':
    app.run(debug=True)
```

### Ejemplo 2: Consulta de Documentos

```python
# Consulta todos los usuarios mayores de 25 años
@app.route('/find_users')
def find_users():
    users = mongo.db.users.find({"age": {"$gt": 25}})
    return render_template('users.html', users=users)
```

### Ejemplo 3: Actualización de Documentos

```python
# Actualiza la edad de un usuario específico
@app.route('/update_user')
def update_user():
    query = {"name": "Alice"}
    new_age = {"$set": {"age": 31}}
    result = mongo.db.users.update_one(query, new_age)
    return f"Usuario actualizado: {result.modified_count} documento(s) modificado(s)"
```

### Ejemplo 4: Eliminación de Documentos

```python
# Elimina un usuario por su nombre
@app.route('/delete_user')
def delete_user():
    query = {"name": "Alice"}
    result = mongo.db.users.delete_one(query)
    return f"Usuario eliminado: {result.deleted_count} documento(s) eliminado(s)"
```

### Ejemplo 5: Operaciones Avanzadas de Agregación

```python
# Calcula la cantidad de usuarios por ciudad
@app.route('/aggregate_users')
def aggregate_users():
    pipeline = [
        {"$group": {"_id": "$city", "count": {"$sum": 1}}}
    ]
    result = mongo.db.users.aggregate(pipeline)
    return render_template('aggregate.html', result=result)
```

Estos ejemplos te muestran cómo integrar los diferentes métodos de `flask_pymongo.PyMongo` en aplicaciones Flask para realizar operaciones desde simples inserciones y consultas hasta operaciones avanzadas de agregación. Puedes adaptar estos ejemplos según los requerimientos específicos de tu aplicación y explorar más sobre cada método para aprovechar al máximo las capacidades de MongoDB en tu proyecto.


## Ejemplos Avanzados de Uso

### Ejemplo 1: Actualización de Documentos con Operadores de Agregación

En este ejemplo, vamos a actualizar documentos utilizando operadores de agregación para calcular nuevos valores.

```python
from flask import Flask
from flask_pymongo import PyMongo

app = Flask(__name__)
app.config["MONGO_URI"] = "mongodb://localhost:27017/mydatabase"
mongo = PyMongo(app)

# Actualiza la edad de todos los usuarios sumándoles 1
@app.route('/update_users_age')
def update_users_age():
    query = {}
    update = [{"$set": {"new_age": {"$add": ["$age", 1]}}}]
    result = mongo.db.users.update_many(query, update)
    return f"Se actualizaron {result.modified_count} usuarios"

if __name__ == '__main__':
    app.run(debug=True)
```

### Ejemplo 2: Consulta con Índices y Opciones Avanzadas

Este ejemplo utiliza índices y opciones avanzadas para mejorar el rendimiento de una consulta específica.

```python
# Consulta todos los usuarios ordenados por edad descendente
@app.route('/find_users_sorted')
def find_users_sorted():
    users = mongo.db.users.find().sort("age", -1)
    return render_template('users.html', users=users)
```

### Ejemplo 3: Uso de Transacciones y Sesiones

En este ejemplo, vamos a utilizar transacciones y sesiones para asegurar operaciones atómicas en MongoDB.

```python
# Transacción para transferencia entre cuentas bancarias
@app.route('/transfer')
def transfer():
    with mongo.start_session() as session:
        with session.start_transaction():
            # Actualiza el saldo de la cuenta de origen
            result1 = mongo.db.accounts.update_one(
                {"_id": 1, "balance": {"$gte": 100}},
                {"$inc": {"balance": -100}},
                session=session
            )
            
            # Actualiza el saldo de la cuenta de destino
            result2 = mongo.db.accounts.update_one(
                {"_id": 2},
                {"$inc": {"balance": 100}},
                session=session
            )
            
            if result1.modified_count == 1 and result2.modified_count == 1:
                return "Transferencia realizada con éxito"
            else:
                session.abort_transaction()
                return "La transferencia no pudo ser completada"

if __name__ == '__main__':
    app.run(debug=True)
```

### Ejemplo 4: Uso de Operaciones de Agregación y Map-Reduce

En este ejemplo, vamos a utilizar operaciones de agregación y map-reduce para obtener estadísticas avanzadas.

```python
# Obtener el promedio de edad por ciudad utilizando agregación
@app.route('/average_age_by_city')
def average_age_by_city():
    pipeline = [
        {"$group": {"_id": "$city", "avg_age": {"$avg": "$age"}}}
    ]
    result = mongo.db.users.aggregate(pipeline)
    return render_template('average.html', result=result)
```

### Ejemplo 5: Gestión de Archivos Binarios con GridFS

Este ejemplo muestra cómo almacenar y recuperar archivos binarios utilizando GridFS en MongoDB.

```python
from flask import Flask, request, send_file
from flask_pymongo import PyMongo
from bson import ObjectId
from gridfs import GridFS

app = Flask(__name__)
app.config["MONGO_URI"] = "mongodb://localhost:27017/mydatabase"
mongo = PyMongo(app)
fs = GridFS(mongo.db)

# Almacenar archivo en GridFS
@app.route('/upload_file', methods=['POST'])
def upload_file():
    file = request.files['file']
    file_id = fs.put(file, filename=file.filename)
    return f"Archivo almacenado con ID: {file_id}"

# Descargar archivo desde GridFS
@app.route('/download_file/<file_id>')
def download_file(file_id):
    file = fs.get(ObjectId(file_id))
    return send_file(file, as_attachment=True, attachment_filename=file.filename)

if __name__ == '__main__':
    app.run(debug=True)
```

### Ejemplo 6: Consulta con Filtros Avanzados

```python
# Consulta usuarios con edad entre 25 y 40 años, ordenados por nombre
@app.route('/find_users_filtered')
def find_users_filtered():
    query = {"age": {"$gte": 25, "$lte": 40}}
    users = mongo.db.users.find(query).sort("name", 1)
    return render_template('users.html', users=users)
```

### Ejemplo 7: Actualización de Documentos con Agregación y Pipelines

```python
# Incrementa la edad de todos los usuarios en 1, utilizando una operación de agregación
@app.route('/increment_users_age')
def increment_users_age():
    pipeline = [
        {"$set": {"new_age": {"$add": ["$age", 1]}}}
    ]
    result = mongo.db.users.update_many({}, pipeline)
    return f"Se actualizaron {result.modified_count} usuarios"
```

### Ejemplo 8: Consulta con Proyección y Exclusión de Campos

```python
# Consulta usuarios excluyendo el campo 'city' y limitando a 10 resultados
@app.route('/find_users_projection')
def find_users_projection():
    projection = {"city": 0}  # Excluir el campo 'city'
    users = mongo.db.users.find({}, projection).limit(10)
    return render_template('users.html', users=users)
```

### Ejemplo 9: Consulta Utilizando Expresiones Regulares

```python
# Consulta usuarios cuyo nombre empieza con 'A'
@app.route('/find_users_regex')
def find_users_regex():
    query = {"name": {"$regex": "^A"}}
    users = mongo.db.users.find(query)
    return render_template('users.html', users=users)
```

### Ejemplo 10: Indexación y Consulta Utilizando Índices

```python
# Crea un índice en el campo 'name' y realiza una consulta utilizando ese índice
@app.route('/find_users_indexed')
def find_users_indexed():
    mongo.db.users.create_index([('name', pymongo.ASCENDING)])
    users = mongo.db.users.find().sort("name", 1)
    return render_template('users.html', users=users)
```

### Ejemplo 11: Operaciones de Agregación con Varios Niveles

```python
# Calcula la cantidad de usuarios por ciudad y por género utilizando una operación de agregación
@app.route('/users_count_by_city_and_gender')
def users_count_by_city_and_gender():
    pipeline = [
        {"$group": {"_id": {"city": "$city", "gender": "$gender"}, "count": {"$sum": 1}}}
    ]
    result = mongo.db.users.aggregate(pipeline)
    return render_template('aggregate.html', result=result)
```

### Ejemplo 12: Actualización Condicionada y Upsert

```python
# Actualiza la edad de un usuario específico, o lo inserta si no existe
@app.route('/update_or_insert_user')
def update_or_insert_user():
    query = {"name": "Alice"}
    update = {"$set": {"age": 30}}
    result = mongo.db.users.update_one(query, update, upsert=True)
    if result.upserted_id:
        return f"Nuevo usuario insertado con ID: {result.upserted_id}"
    else:
        return f"Usuario actualizado: {result.modified_count} documento(s) modificado(s)"
```

### Ejemplo 13: Consulta Utilizando Agregación y Operadores de Comparación

```python
# Consulta usuarios con edad mayor al promedio de edad de la colección
@app.route('/find_users_above_average_age')
def find_users_above_average_age():
    pipeline = [
        {"$group": {"_id": None, "avg_age": {"$avg": "$age"}}},
        {"$match": {"age": {"$gt": "$avg_age"}}}
    ]
    result = mongo.db.users.aggregate(pipeline)
    return render_template('users.html', users=result)
```

### Ejemplo 14: Consulta con Texto Completo (Text Search)

```python
# Realiza una búsqueda de texto completo en los campos 'name' y 'description'
@app.route('/text_search')
def text_search():
    query = {"$text": {"$search": "python web"}}
    projection = {"score": {"$meta": "textScore"}}
    users = mongo.db.users.find(query, projection).sort([("score", {"$meta": "textScore"})])
    return render_template('users.html', users=users)
```

### Ejemplo 15: Uso de Funciones JavaScript en Map-Reduce

```python
# Utiliza una función JavaScript para realizar map-reduce en los datos
@app.route('/map_reduce')
def map_reduce():
    map_function = Code("""
        function() {
            emit(this.city, this.age);
        }
    """)
    reduce_function = Code("""
        function(key, values) {
            return Array.avg(values);
        }
    """)
    result = mongo.db.users.map_reduce(map_function, reduce_function, "avg_age_by_city")
    return render_template('map_reduce.html', result=result)
```

Estos ejemplos avanzados te proporcionan una visión más profunda y práctica sobre cómo utilizar `flask_pymongo.PyMongo` junto con MongoDB para realizar operaciones complejas y avanzadas en el contexto de una aplicación Flask. Puedes adaptar estos ejemplos según tus necesidades específicas y explorar más sobre cada tema para fortalecer tus habilidades en desarrollo de aplicaciones web con bases de datos NoSQL.