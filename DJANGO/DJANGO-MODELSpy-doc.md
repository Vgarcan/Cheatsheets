# Guía Completa y Detallada sobre cómo usar `models.py` en Django

### **Tabla de Contenidos**
1. [¿Qué es el archivo `models.py` y por qué es importante?](#1-qué-es-el-archivo-modelspy-y-por-qué-es-importante)
2. [Creación de un modelo en Django](#2-creación-de-un-modelo-en-django)
3. [Tipos de campos en Django](#3-tipos-de-campos-en-django)
   - 3.1 [Campos básicos](#31-campos-básicos)
   - 3.2 [Campos numéricos](#32-campos-numéricos)
   - 3.3 [Campos de fecha y hora](#33-campos-de-fecha-y-hora)
   - 3.4 [Campos de archivo y multimedia](#34-campos-de-archivo-y-multimedia)
   - 3.5 [Campos relacionales](#35-campos-relacionales)
4. [Opciones avanzadas de campos](#4-opciones-avanzadas-de-campos)
5. [Relaciones entre modelos](#5-relaciones-entre-modelos)
   - 5.1 [Relación Uno a Muchos (`ForeignKey`)](#51-relación-uno-a-muchos-foreignkey)
   - 5.2 [Relación Muchos a Muchos (`ManyToManyField`)](#52-relación-muchos-a-muchos-manytomanyfield)
   - 5.3 [Relación Uno a Uno (`OneToOneField`)](#53-relación-uno-a-uno-onetoonefield)
6. [Métodos dentro de los modelos](#6-métodos-dentro-de-los-modelos)
   - 6.1 [Métodos especiales (`__str__`, `__unicode__`)](#61-métodos-especiales-__str__-__unicode__)
   - 6.2 [Métodos personalizados](#62-métodos-personalizados)
   - 6.3 [Sobrescribir métodos (`save`, `delete`)](#63-sobrescribir-métodos-save-delete)
7. [Campos opcionales, valores por defecto y validadores](#7-campos-opcionales-valores-por-defecto-y-validadores)
8. [Uso de la clase `Meta` para personalizar el comportamiento de los modelos](#8-uso-de-la-clase-meta-para-personalizar-el-comportamiento-de-los-modelos)
9. [Cómo hacer que los datos aparezcan correctamente en el admin](#9-cómo-hacer-que-los-datos-aparezcan-correctamente-en-el-admin)
10. [Campos personalizados y cómo extender `models.Model`](#10-campos-personalizados-y-cómo-extender-modelsmodel)
11. [Modelos proxy y herencia de modelos](#11-modelos-proxy-y-herencia-de-modelos)
    - 11.1 [Herencia abstracta](#111-herencia-abstracta)
    - 11.2 [Herencia multi-tabla](#112-herencia-multi-tabla)
    - 11.3 [Modelos proxy](#113-modelos-proxy)
12. [Consultas y cómo usar el ORM de Django](#12-consultas-y-cómo-usar-el-orm-de-django)
    - 12.1 [Consultas básicas](#121-consultas-básicas)
    - 12.2 [Filtros y consultas avanzadas](#122-filtros-y-consultas-avanzadas)
    - 12.3 [Métodos de QuerySet comunes](#123-métodos-de-queryset-comunes)
13. [Gestores de modelos (Managers) y QuerySets personalizados](#13-gestores-de-modelos-managers-y-querysets-personalizados)
14. [Migraciones: Creación y aplicación de migraciones](#14-migraciones-creación-y-aplicación-de-migraciones)
15. [Optimización y rendimiento de los modelos](#15-optimización-y-rendimiento-de-los-modelos)
    - 15.1 [Uso de índices](#151-uso-de-índices)
    - 15.2 [Optimización de consultas](#152-optimización-de-consultas)
    - 15.3 [Select Related y Prefetch Related](#153-select-related-y-prefetch-related)
16. [Validación y limpieza de datos en modelos](#16-validación-y-limpieza-de-datos-en-modelos)
17. [Uso de señales (signals) en modelos](#17-uso-de-señales-signals-en-modelos)
18. [Transacciones y manejo de errores](#18-transacciones-y-manejo-de-errores)
19. [Buenas prácticas para trabajar con modelos en Django](#19-buenas-prácticas-para-trabajar-con-modelos-en-django)
20. [Recursos adicionales](#20-recursos-adicionales)

---

### 1. ¿Qué es el archivo `models.py` y por qué es importante?

El archivo **`models.py`** es el lugar donde defines las **clases de modelos** de tu aplicación Django. Los modelos son la representación de las estructuras de datos de tu aplicación y se encargan de:

- **Definir la estructura de la base de datos**: Cada modelo corresponde a una tabla en la base de datos.
- **Proporcionar una interfaz de alto nivel para interactuar con la base de datos**: Gracias al **ORM (Object-Relational Mapping)** de Django, puedes realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) utilizando código Python en lugar de SQL.
- **Mantener la lógica de negocio**: Los modelos pueden contener métodos y lógica que representan las reglas y procesos de tu aplicación.

#### Ventajas de utilizar modelos en Django:

- **Abstracción de la base de datos**: Puedes cambiar el motor de base de datos (por ejemplo, de SQLite a PostgreSQL) sin tener que reescribir tu código.
- **Productividad**: El ORM te permite escribir menos código y centrarte en la lógica de tu aplicación.
- **Seguridad**: El uso del ORM reduce el riesgo de inyección SQL y otros problemas de seguridad relacionados con las consultas directas a la base de datos.

---

### 2. Creación de un modelo en Django

Un **modelo** en Django es una clase que hereda de `models.Model`. Los atributos de la clase corresponden a campos en la base de datos.

#### Paso 1: Crear o editar el archivo `models.py`

Cada aplicación en Django tiene su propio archivo `models.py`. Si tu aplicación no tiene uno, créalo en el directorio de la aplicación:

```bash
myapp/
    __init__.py
    models.py    # Aquí defines tus modelos
    views.py
    admin.py
```

#### Paso 2: Definir un modelo básico

Vamos a crear un modelo llamado `Producto` para una aplicación de e-commerce.

```python
# models.py
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    descripción = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    disponible = models.BooleanField(default=True)
    fecha_creación = models.DateTimeField(auto_now_add=True)
    fecha_actualización = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.nombre
```

#### Explicación:

- **`nombre`**: Un campo de texto con una longitud máxima de 200 caracteres.
- **`descripción`**: Un campo de texto sin límite de longitud.
- **`precio`**: Un campo decimal con hasta 10 dígitos en total y 2 decimales.
- **`stock`**: Un campo entero que indica la cantidad disponible.
- **`disponible`**: Un campo booleano que indica si el producto está disponible para la venta.
- **`fecha_creación`**: Almacena la fecha y hora en que se creó el objeto.
- **`fecha_actualización`**: Se actualiza automáticamente cada vez que se guarda el objeto.

---

### 3. Tipos de campos en Django

Django ofrece una amplia variedad de campos que puedes utilizar para definir tus modelos. Estos campos representan diferentes tipos de datos y proporcionan validación automática.

#### 3.1 Campos básicos

- **`CharField`**: Almacena cadenas de texto de longitud limitada.
  ```python
  nombre = models.CharField(max_length=255)
  ```
- **`TextField`**: Almacena cadenas de texto de longitud ilimitada.
  ```python
  descripción = models.TextField()
  ```
- **`SlugField`**: Almacena cadenas de texto utilizadas en URLs amigables.
  ```python
  slug = models.SlugField(unique=True)
  ```
- **`EmailField`**: Almacena direcciones de correo electrónico y realiza validación básica.
  ```python
  email = models.EmailField()
  ```
- **`URLField`**: Almacena URLs y realiza validación básica.
  ```python
  sitio_web = models.URLField()
  ```
- **`UUIDField`**: Almacena identificadores únicos universales (UUID).
  ```python
  uuid = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
  ```

#### 3.2 Campos numéricos

- **`IntegerField`**: Almacena números enteros.
  ```python
  edad = models.IntegerField()
  ```
- **`FloatField`**: Almacena números de punto flotante.
  ```python
  puntuación = models.FloatField()
  ```
- **`DecimalField`**: Almacena números decimales con precisión fija.
  ```python
  precio = models.DecimalField(max_digits=10, decimal_places=2)
  ```
- **`PositiveIntegerField`**: Almacena números enteros positivos.
  ```python
  cantidad = models.PositiveIntegerField()
  ```

#### 3.3 Campos de fecha y hora

- **`DateField`**: Almacena fechas.
  ```python
  fecha_nacimiento = models.DateField()
  ```
- **`TimeField`**: Almacena horas.
  ```python
  hora_apertura = models.TimeField()
  ```
- **`DateTimeField`**: Almacena fechas y horas.
  ```python
  fecha_publicación = models.DateTimeField(auto_now_add=True)
  ```

#### 3.4 Campos de archivo y multimedia

- **`FileField`**: Almacena rutas a archivos cargados por el usuario.
  ```python
  archivo = models.FileField(upload_to='archivos/')
  ```
- **`ImageField`**: Especialización de `FileField` para archivos de imagen.
  ```python
  imagen = models.ImageField(upload_to='imagenes/')
  ```

#### 3.5 Campos relacionales

- **`ForeignKey`**: Establece una relación uno a muchos.
- **`ManyToManyField`**: Establece una relación muchos a muchos.
- **`OneToOneField`**: Establece una relación uno a uno.

Se explicarán en detalle en la sección [5. Relaciones entre modelos](#5-relaciones-entre-modelos).

---

### 4. Opciones avanzadas de campos

Los campos en Django pueden tener opciones adicionales que te permiten personalizar su comportamiento.

#### **`choices`**

Define un conjunto de opciones predefinidas para un campo.

```python
class Producto(models.Model):
    TIPO_PRODUCTO = [
        ('ELE', 'Electrónica'),
        ('MOD', 'Moda'),
        ('ALI', 'Alimentos'),
    ]
    tipo = models.CharField(max_length=3, choices=TIPO_PRODUCTO)
```

#### **`unique`**

Establece que el valor del campo debe ser único en la tabla.

```python
email = models.EmailField(unique=True)
```

#### **`primary_key`**

Define el campo como clave primaria del modelo. Si no se especifica, Django crea un campo `id` automático.

```python
uuid = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
```

#### **`default`**

Establece un valor por defecto para el campo.

```python
stock = models.IntegerField(default=0)
```

#### **`null` y `blank`**

- **`null=True`**: Permite que el campo acepte valores `NULL` en la base de datos.
- **`blank=True`**: Permite que el campo sea opcional en los formularios.

```python
segundo_nombre = models.CharField(max_length=100, null=True, blank=True)
```

#### **`validators`**

Permite especificar funciones de validación personalizadas para un campo.

```python
from django.core.validators import MinValueValidator

precio = models.DecimalField(max_digits=10, decimal_places=2, validators=[MinValueValidator(0.01)])
```

---

### 5. Relaciones entre modelos

Las relaciones entre modelos son fundamentales en cualquier base de datos relacional. Django facilita la definición de estas relaciones.

#### 5.1 Relación Uno a Muchos (`ForeignKey`)

Una relación donde un registro de un modelo puede estar asociado con muchos registros de otro modelo, pero cada registro del segundo modelo está asociado con un solo registro del primero.

**Ejemplo**: Una categoría puede tener muchos productos, pero cada producto pertenece a una sola categoría.

```python
class Categoria(models.Model):
    nombre = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

class Producto(models.Model):
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE)
    nombre = models.CharField(max_length=200)
    # Otros campos...
```

- **`on_delete`**: Especifica el comportamiento cuando el objeto relacionado es eliminado.
  - **`models.CASCADE`**: Elimina los objetos relacionados.
  - **`models.SET_NULL`**: Establece el campo a `NULL`.
  - **`models.PROTECT`**: Previene la eliminación del objeto relacionado.

#### 5.2 Relación Muchos a Muchos (`ManyToManyField`)

Una relación donde múltiples registros de un modelo pueden estar relacionados con múltiples registros de otro modelo.

**Ejemplo**: Un producto puede tener múltiples etiquetas, y una etiqueta puede aplicarse a múltiples productos.

```python
class Etiqueta(models.Model):
    nombre = models.CharField(max_length=50)

    def __str__(self):
        return self.nombre

class Producto(models.Model):
    etiquetas = models.ManyToManyField(Etiqueta)
    nombre = models.CharField(max_length=200)
    # Otros campos...
```

#### 5.3 Relación Uno a Uno (`OneToOneField`)

Una relación donde un registro de un modelo está relacionado con un solo registro de otro modelo.

**Ejemplo**: Un usuario puede tener un perfil, y cada perfil pertenece a un solo usuario.

```python
from django.contrib.auth.models import User

class Perfil(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)
    biografía = models.TextField()
    # Otros campos...

    def __str__(self):
        return f'Perfil de {self.usuario.username}'
```

---

### 6. Métodos dentro de los modelos

Los modelos en Django pueden contener métodos que proporcionan funcionalidad adicional.

#### 6.1 Métodos especiales (`__str__`, `__unicode__`)

- **`__str__()`**: Define la representación en cadena de un objeto en Python 3.
- **`__unicode__()`**: Se usa en Python 2 para cadenas Unicode.

```python
def __str__(self):
    return self.nombre
```

Este método es crucial para que los objetos se representen de manera legible en el administrador de Django y en otros contextos.

#### 6.2 Métodos personalizados

Puedes definir métodos que realicen cálculos o devuelvan información específica.

```python
class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()

    def calcular_precio_con_impuesto(self):
        return self.precio * 1.21  # Asumiendo un 21% de IVA

    def hay_stock(self):
        return self.stock > 0
```

#### 6.3 Sobrescribir métodos (`save`, `delete`)

Puedes sobrescribir los métodos `save()` y `delete()` para personalizar el comportamiento al guardar o eliminar objetos.

```python
def save(self, *args, **kwargs):
    # Lógica antes de guardar
    self.nombre = self.nombre.capitalize()
    super().save(*args, **kwargs)
    # Lógica después de guardar
```

- **Precaución**: Si sobrescribes `save()`, siempre debes llamar a `super().save(*args, **kwargs)` para mantener el comportamiento predeterminado.

---

### 7. Campos opcionales, valores por defecto y validadores

#### Campos opcionales

- **`blank=True`**: Permite que el campo sea opcional en formularios y validaciones.
- **`null=True`**: Permite que el campo acepte `NULL` en la base de datos.

```python
descripción = models.TextField(blank=True, null=True)
```

#### Valores por defecto

Puedes establecer un valor por defecto utilizando el parámetro `default`.

```python
disponible = models.BooleanField(default=True)
```

#### Validadores

Puedes agregar validadores personalizados a los campos.

```python
from django.core.validators import MinLengthValidator

nombre = models.CharField(max_length=100, validators=[MinLengthValidator(2)])
```

#### Ejemplo completo:

```python
from django.core.validators import MaxValueValidator, MinValueValidator

class Producto(models.Model):
    nombre = models.CharField(max_length=200, validators=[MinLengthValidator(2)])
    precio = models.DecimalField(
        max_digits=10, decimal_places=2,
        validators=[MinValueValidator(0.01)]
    )
    stock = models.PositiveIntegerField(default=0)
    disponible = models.BooleanField(default=True)
```

---

### 8. Uso de la clase `Meta` para personalizar el comportamiento de los modelos

La clase `Meta` dentro de un modelo te permite especificar opciones de comportamiento.

#### Opciones comunes de `Meta`

- **`ordering`**: Define el orden predeterminado de los objetos al realizar consultas.
  ```python
  class Meta:
      ordering = ['nombre']
  ```
- **`verbose_name` y `verbose_name_plural`**: Especifica nombres legibles para el modelo.
  ```python
  class Meta:
      verbose_name = 'Producto'
      verbose_name_plural = 'Productos'
  ```
- **`db_table`**: Especifica el nombre de la tabla en la base de datos.
  ```python
  class Meta:
      db_table = 'mi_tabla_productos'
  ```
- **`unique_together`**: Establece restricciones de unicidad combinada.
  ```python
  class Meta:
      unique_together = [('nombre', 'categoria')]
  ```
- **`indexes`**: Crea índices en la base de datos para optimizar consultas.
  ```python
  class Meta:
      indexes = [
          models.Index(fields=['nombre'], name='idx_nombre'),
      ]
  ```

---

### 9. Cómo hacer que los datos aparezcan correctamente en el admin

El administrador de Django es una herramienta poderosa para gestionar tus modelos. Personalizar cómo se presentan los datos mejora la usabilidad.

#### Registro básico en `admin.py`

```python
from django.contrib import admin
from .models import Producto

admin.site.register(Producto)
```

#### Personalización con `ModelAdmin`

Para personalizar la interfaz del admin, crea una clase que herede de `admin.ModelAdmin`.

```python
class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'precio', 'stock', 'disponible')
    list_editable = ('precio', 'stock')
    search_fields = ('nombre',)
    list_filter = ('disponible', 'fecha_creación')
    ordering = ('-fecha_creación',)

admin.site.register(Producto, ProductoAdmin)
```

#### Explicación:

- **`list_display`**: Campos que se muestran en la lista de objetos.
- **`list_editable`**: Campos que se pueden editar directamente desde la lista.
- **`search_fields`**: Campos en los que se puede realizar búsquedas.
- **`list_filter`**: Añade filtros en la barra lateral.
- **`ordering`**: Define el orden predeterminado.

#### Mostrar relaciones en el admin

Si tu modelo tiene relaciones, puedes mostrarlas en el admin.

```python
class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'categoria', 'precio')
    list_filter = ('categoria',)
```

#### Inlines

Para editar objetos relacionados directamente desde el modelo principal.

```python
from .models import ImagenProducto

class ImagenProductoInline(admin.TabularInline):
    model = ImagenProducto
    extra = 1

class ProductoAdmin(admin.ModelAdmin):
    inlines = [ImagenProductoInline]
```

---

### 10. Campos personalizados y cómo extender `models.Model`

Puedes crear tus propios campos personalizados si necesitas un comportamiento específico.

#### Creación de un campo personalizado

```python
from django.db import models
from django.core import validators

class NombrePropioField(models.CharField):
    def __init__(self, *args, **kwargs):
        kwargs['max_length'] = 100
        super().__init__(*args, **kwargs)
        self.validators.append(validators.RegexValidator(
            regex='^[A-Z][a-z]+$', message='Debe comenzar con mayúscula y contener solo letras.'
        ))
```

Uso en un modelo:

```python
class Persona(models.Model):
    nombre = NombrePropioField()
```

#### Extender `models.Model`

Puedes crear una clase base que otros modelos hereden.

```python
class TiempoModelo(models.Model):
    creado = models.DateTimeField(auto_now_add=True)
    actualizado = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True

class Artículo(TiempoModelo):
    título = models.CharField(max_length=200)
    contenido = models.TextField()
```

---

### 11. Modelos proxy y herencia de modelos

Django soporta diferentes tipos de herencia en modelos.

#### 11.1 Herencia abstracta

Se utiliza para compartir campos y métodos entre modelos.

```python
class BaseModelo(models.Model):
    creado = models.DateTimeField(auto_now_add=True)

    class Meta:
        abstract = True

class ModeloA(BaseModelo):
    campo_a = models.CharField(max_length=100)

class ModeloB(BaseModelo):
    campo_b = models.IntegerField()
```

#### 11.2 Herencia multi-tabla

Cada modelo tiene su propia tabla en la base de datos.

```python
class Vehículo(models.Model):
    fabricante = models.CharField(max_length=100)

class Coche(Vehículo):
    num_puertas = models.IntegerField()
```

#### 11.3 Modelos proxy

Permiten cambiar el comportamiento de un modelo sin cambiar su estructura.

```python
class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    # Otros campos...

class ProductoProxy(Producto):
    class Meta:
        proxy = True
        ordering = ['nombre']

    def __str__(self):
        return f'Producto: {self.nombre}'
```

---

### 12. Consultas y cómo usar el ORM de Django

El ORM de Django te permite interactuar con la base de datos usando Python.

#### 12.1 Consultas básicas

- **Obtener todos los objetos**:
  ```python
  productos = Producto.objects.all()
  ```
- **Filtrar objetos**:
  ```python
  productos_disponibles = Producto.objects.filter(disponible=True)
  ```
- **Obtener un objeto**:
  ```python
  producto = Producto.objects.get(id=1)
  ```
- **Crear un objeto**:
  ```python
  nuevo_producto = Producto.objects.create(nombre='Nuevo', precio=9.99)
  ```

#### 12.2 Filtros y consultas avanzadas

- **Filtros de campos de texto**:
  ```python
  productos = Producto.objects.filter(nombre__icontains='laptop')
  ```
- **Filtros de campos de fecha**:
  ```python
  from django.utils import timezone
  productos_recientes = Producto.objects.filter(fecha_creación__gte=timezone.now() - timezone.timedelta(days=30))
  ```
- **Ordenar resultados**:
  ```python
  productos_ordenados = Producto.objects.order_by('-precio')
  ```

#### 12.3 Métodos de QuerySet comunes

- **`count()`**: Devuelve el número de objetos.
  ```python
  total_productos = Producto.objects.count()
  ```
- **`exists()`**: Devuelve `True` si existen objetos que cumplen la condición.
  ```python
  existe = Producto.objects.filter(nombre='Laptop').exists()
  ```
- **`first()` y `last()`**: Devuelve el primer o último objeto.
  ```python
  primer_producto = Producto.objects.first()
  ```
- **`values()` y `values_list()`**: Devuelve diccionarios o listas de valores.
  ```python
  nombres = Producto.objects.values_list('nombre', flat=True)
  ```

---

### 13. Gestores de modelos (Managers) y QuerySets personalizados

Los **managers** controlan cómo interactúas con el ORM.

#### Creación de un manager personalizado

```python
class ProductoManager(models.Manager):
    def disponibles(self):
        return self.filter(disponible=True)

class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    disponible = models.BooleanField(default=True)

    objects = ProductoManager()
```

Uso:

```python
productos_disponibles = Producto.objects.disponibles()
```

#### QuerySets personalizados

Puedes extender `models.QuerySet` para crear métodos de consulta más complejos.

```python
class ProductoQuerySet(models.QuerySet):
    def disponibles(self):
        return self.filter(disponible=True)

class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    disponible = models.BooleanField(default=True)

    objects = ProductoQuerySet.as_manager()
```

---

### 14. Migraciones: Creación y aplicación de migraciones

Las migraciones son cambios incrementales en la estructura de la base de datos.

#### Crear migraciones

```bash
python manage.py makemigrations
```

#### Aplicar migraciones

```bash
python manage.py migrate
```

#### Ver el estado de las migraciones

```bash
python manage.py showmigrations
```

#### Consideraciones:

- Siempre revisa las migraciones antes de aplicarlas.
- Usa el control de versiones para rastrear cambios en los modelos y migraciones.

---

### 15. Optimización y rendimiento de los modelos

#### 15.1 Uso de índices

Los índices mejoran el rendimiento de las consultas.

```python
class Producto(models.Model):
    nombre = models.CharField(max_length=200, db_index=True)
```

O usando `Meta`:

```python
class Meta:
    indexes = [
        models.Index(fields=['nombre'], name='idx_nombre'),
    ]
```

#### 15.2 Optimización de consultas

Evita el acceso innecesario a la base de datos.

- **No uses consultas dentro de bucles**.
- **Usa `bulk_create()` y `bulk_update()`** para operaciones masivas.

#### 15.3 `select_related` y `prefetch_related`

- **`select_related`**: Para relaciones **ForeignKey** y **OneToOneField**.
  ```python
  productos = Producto.objects.select_related('categoria').all()
  ```
- **`prefetch_related`**: Para relaciones **ManyToManyField** y **ForeignKey inversas**.
  ```python
  categorias = Categoria.objects.prefetch_related('producto_set').all()
  ```

---

### 16. Validación y limpieza de datos en modelos

Puedes validar datos antes de guardarlos.

#### Método `clean()`

```python
class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    precio = models.DecimalField(max_digits=10, decimal_places=2)

    def clean(self):
        if self.precio <= 0:
            raise ValidationError('El precio debe ser mayor que cero')
```

#### Uso de `full_clean()`

Llama a `full_clean()` para validar el modelo.

```python
producto = Producto(nombre='Laptop', precio=-1000)
producto.full_clean()  # Lanzará ValidationError
```

---

### 17. Uso de señales (signals) en modelos

Las señales permiten ejecutar código en respuesta a ciertos eventos.

#### Ejemplo: Enviar un correo al crear un usuario

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

@receiver(post_save, sender=User)
def enviar_correo_bienvenida(sender, instance, created, **kwargs):
    if created:
        # Lógica para enviar correo
        pass
```

---

### 18. Transacciones y manejo de errores

Puedes usar transacciones para asegurar la integridad de los datos.

```python
from django.db import transaction

with transaction.atomic():
    # Operaciones que deben ser atómicas
    producto = Producto.objects.create(nombre='Tablet', precio=300)
    inventario = Inventario.objects.create(producto=producto, cantidad=50)
```

Si ocurre una excepción dentro del bloque, todas las operaciones se revertirán.

---

### 19. Buenas prácticas para trabajar con modelos en Django

1. **Mantén los modelos simples**: Separa la lógica de negocio compleja en otros componentes.
2. **Usa nombres descriptivos**: Para campos y métodos.
3. **Documenta tu código**: Especialmente si sobrescribes métodos o usas lógica personalizada.
4. **Optimiza consultas**: Evita consultas innecesarias y usa las herramientas que Django proporciona.
5. **Valida los datos**: Utiliza validadores y métodos de limpieza.
6. **Usa migraciones correctamente**: No edites migraciones generadas automáticamente y usa el control de versiones.
7. **Seguridad**: Protege los datos sensibles y maneja excepciones.

---

### 20. Recursos adicionales

- **Documentación oficial de Django**: [https://docs.djangoproject.com/](https://docs.djangoproject.com/)
- **Django ORM Cookbook**: Una guía para aprovechar al máximo el ORM de Django.
- **Django Patterns and Best Practices**: Para aprender patrones comunes y prácticas recomendadas.

---

### Conclusión

El archivo **`models.py`** es fundamental en cualquier proyecto Django. Define cómo se estructuran tus datos, cómo se relacionan y cómo interactúas con ellos. Al comprender profundamente los modelos, las relaciones, las consultas y las mejores prácticas, podrás construir aplicaciones robustas, eficientes y fáciles de mantener. Esta guía detallada te proporciona una base sólida para aprovechar al máximo el poder de los modelos en Django.