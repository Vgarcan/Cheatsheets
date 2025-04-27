Sí, tienes razón. Si ya estás utilizando el decorador `@login_required` proporcionado por Flask-Login, no necesitas verificar manualmente si el usuario está autenticado en cada decorador adicional que implementes. El decorador `@login_required` se encarga de redirigir a los usuarios no autenticados a la página de inicio de sesión.

Sin embargo, si deseas crear un decorador adicional para roles, debes asumir que `@login_required` ya se ha aplicado o incluir la verificación de autenticación en tu decorador de roles si no se garantiza que siempre se use junto con `@login_required`.

### Decorador `role_required` sin Verificación de Autenticación

Puedes asumir que `@login_required` se aplicará siempre junto con `role_required`, y por lo tanto, eliminar la verificación de autenticación del decorador de roles:

```python
import functools
from flask import redirect, url_for, flash
from flask_login import current_user

def role_required(role):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            if current_user.role != role:
                flash('You do not have permission to access this page.', 'danger')
                return redirect(url_for('home'))
            return func(*args, **kwargs)
        return wrapper
    return decorator
```

### Uso del Decorador con `@login_required`

Cuando uses estos decoradores en tus vistas, simplemente asegúrate de aplicar ambos decoradores en las rutas protegidas:

#### Ejemplo de Uso

##### Para Usuarios Normales (`users/views.py`)

```python
from flask import Blueprint, render_template
from flask_login import login_required
from .decorators import role_required

users_bp = Blueprint('users', __name__)

@users_bp.route('/user_home')
@login_required
@role_required('user')
def user_home():
    return render_template('user_home.html')
```

##### Para Usuarios de Compañías (`companies/views.py`)

```python
from flask import Blueprint, render_template
from flask_login import login_required
from .decorators import role_required

companies_bp = Blueprint('companies', __name__)

@companies_bp.route('/company_home')
@login_required
@role_required('company')
def company_home():
    return render_template('company_home.html')
```

### Explicación

- **@login_required**: Asegura que solo los usuarios autenticados pueden acceder a la vista.
- **@role_required('user')**: Asegura que solo los usuarios con el rol 'user' pueden acceder a la vista `user_home`.
- **@role_required('company')**: Asegura que solo los usuarios con el rol 'company' pueden acceder a la vista `company_home`.

### Guía Completa sobre Decoradores en Python y su Uso en Flask

#### Tabla de Contenidos
1. [Introducción a los Decoradores](#introducción-a-los-decoradores)
2. [Creación de un Decorador Básico](#creación-de-un-decorador-básico)
3. [Uso de `functools.wraps`](#uso-de-functoolswraps)
4. [Decoradores con Argumentos](#decoradores-con-argumentos)
5. [Decoradores en Flask](#decoradores-en-flask)
6. [Control de Acceso Basado en Roles](#control-de-acceso-basado-en-roles)
7. [Ejemplos Prácticos](#ejemplos-prácticos)
8. [Recursos Adicionales](#recursos-adicionales)

### Introducción a los Decoradores

Un decorador en Python es una función que permite añadir funcionalidad adicional a otra función o método sin modificar su estructura. Los decoradores son útiles para seguir el principio de la responsabilidad única, mejorando la reutilización del código y su legibilidad.

### Creación de un Decorador Básico

Para crear un decorador, defines una función que toma otra función como argumento y devuelve una nueva función que envuelve a la función original.

#### Ejemplo Básico

```python
def my_decorator(func):
    def wrapper():
        print("Algo que se hace antes de llamar a func()")
        func()
        print("Algo que se hace después de llamar a func()")
    return wrapper

@my_decorator
def say_hello():
    print("Hola!")

say_hello()
```

Salida:
```
Algo que se hace antes de llamar a func()
Hola!
Algo que se hace después de llamar a func()
```

### Uso de `functools.wraps`

El uso de `functools.wraps` es una buena práctica para preservar la información de la función original, como su nombre, módulo y cadena de documentación.

#### Ejemplo con `functools.wraps`

```python
import functools

def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print("Algo que se hace antes de llamar a func()")
        result = func(*args, **kwargs)
        print("Algo que se hace después de llamar a func()")
        return result
    return wrapper

@my_decorator
def say_hello(name):
    print(f"Hola, {name}!")

say_hello("Mundo")
print(say_hello.__name__)  # Output: say_hello
```

### Decoradores con Argumentos

A veces, necesitarás pasar argumentos al decorador. Para ello, encapsulas el decorador dentro de otra función.

#### Ejemplo de Decorador con Argumentos

```python
import functools

def repeat(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def say_hello(name):
    print(f"Hola, {name}!")

say_hello("Mundo")
```

Salida:
```
Hola, Mundo!
Hola, Mundo!
Hola, Mundo!
```

### Decoradores en Flask

En Flask, los decoradores son especialmente útiles para la autenticación, autorización y otras funcionalidades relacionadas con la seguridad y el acceso a las vistas.

#### Decorador para Autenticación

```python
from functools import wraps
from flask import redirect, url_for, request
from flask_login import current_user

def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if not current_user.is_authenticated:
            return redirect(url_for('login', next=request.url))
        return f(*args, **kwargs)
    return decorated_function
```

### Control de Acceso Basado en Roles

Para implementar control de acceso basado en roles, puedes crear un decorador que verifique el rol del usuario antes de permitir el acceso a una vista.

#### Decorador `role_required`

```python
import functools
from flask import redirect, url_for, flash
from flask_login import current_user

def role_required(role):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            if current_user.role != role:
                flash('You do not have permission to access this page.', 'danger')
                return redirect(url_for('home'))
            return func(*args, **kwargs)
        return wrapper
    return decorator
```

### Ejemplos Prácticos

#### Aplicación de Decoradores en Vistas de Flask

##### Para Usuarios Normales (`users/views.py`)

```python
from flask import Blueprint, render_template
from flask_login import login_required
from .decorators import role_required

users_bp = Blueprint('users', __name__)

@users_bp.route('/user_home')
@login_required
@role_required('user')
def user_home():
    return render_template('user_home.html')
```

##### Para Usuarios de Compañías (`companies/views.py`)

```python
from flask import Blueprint, render_template
from flask_login import login_required
from .decorators import role_required

companies_bp = Blueprint('companies', __name__)

@companies_bp.route('/company_home')
@login_required
@role_required('company')
def company_home():
    return render_template('company_home.html')
```

### Recursos Adicionales

1. **Documentación Oficial de Python**:
   - [Decoradores](https://docs.python.org/3/glossary.html#term-decorator)
   - [functools.wraps](https://docs.python.org/3/library/functools.html#functools.wraps)

2. **Tutoriales y Artículos**:
   - [Real Python: Understanding Python Decorators](https://realpython.com/primer-on-python-decorators/)
   - [Geeks for Geeks: Python Decorators](https://www.geeksforgeeks.org/decorators-in-python/)

3. **Documentación Oficial de Flask**:
   - [Extensión Flask-Login](https://flask-login.readthedocs.io/en/latest/)
   - [Flask Documentation: View Decorators](https://flask.palletsprojects.com/en/2.0.x/patterns/viewdecorators/)

4. **Libros Recomendados**:
   - *"Flask Web Development"* por Miguel Grinberg
   - *"Python Cookbook"* por David Beazley y Brian K. Jones

5. **Cursos en Línea**:
   - [Coursera: Python for Everybody Specialization](https://www.coursera.org/specializations/python)
   - [Udemy: The Complete Python Course](https://www.udemy.com/course/the-complete-python-course/)
   - [Udemy: Flask Bootcamp](https://www.udemy.com/course/flask-bootcamp/)

Esta guía proporciona una base sólida sobre cómo