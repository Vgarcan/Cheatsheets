

1. Introducción a los Diccionarios en Python
2. Métodos de Diccionarios
    - `dict.clear()`
    - `dict.copy()`
    - `dict.fromkeys()`
    - `dict.get()`
    - `dict.items()`
    - `dict.keys()`
    - `dict.pop()`
    - `dict.popitem()`
    - `dict.setdefault()`
    - `dict.update()`
    - `dict.values()`
3. Dictionary Comprehension

---

## 1. Introducción a los Diccionarios en Python

Un diccionario en Python es una colección desordenada de elementos. Cada elemento es un par de clave y valor. Los diccionarios se definen con llaves `{}` y cada par clave-valor se separa por comas.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
```

---

## 2. Métodos de Diccionarios

### `dict.clear()`
El método `clear()` elimina todos los elementos del diccionario.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
mi_diccionario.clear()
print(mi_diccionario)  # Output: {}
```

### `dict.copy()`
El método `copy()` devuelve una copia superficial del diccionario.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
copia_diccionario = mi_diccionario.copy()
print(copia_diccionario)  # Output: {'nombre': 'Juan', 'edad': 30, 'ciudad': 'Madrid'}
```

### `dict.fromkeys()`
El método `fromkeys()` crea un nuevo diccionario con claves de un iterable y valores establecidos en un valor específico.

```python
claves = ["a", "b", "c"]
nuevo_diccionario = dict.fromkeys(claves, 0)
print(nuevo_diccionario)  # Output: {'a': 0, 'b': 0, 'c': 0}
```

### `dict.get()`
El método `get()` devuelve el valor para la clave especificada. Si la clave no existe, devuelve un valor predeterminado (por defecto `None`).

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
print(mi_diccionario.get("nombre"))  # Output: Juan
print(mi_diccionario.get("pais", "España"))  # Output: España
```

### `dict.items()`
El método `items()` devuelve una vista de objetos con pares clave-valor del diccionario.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
print(mi_diccionario.items())  # Output: dict_items([('nombre', 'Juan'), ('edad', 30), ('ciudad', 'Madrid')])
```

### `dict.keys()`
El método `keys()` devuelve una vista de objetos con todas las claves del diccionario.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
print(mi_diccionario.keys())  # Output: dict_keys(['nombre', 'edad', 'ciudad'])
```

### `dict.pop()`
El método `pop()` elimina y devuelve el valor para la clave especificada. Si la clave no se encuentra, genera una excepción `KeyError`.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
edad = mi_diccionario.pop("edad")
print(edad)  # Output: 30
print(mi_diccionario)  # Output: {'nombre': 'Juan', 'ciudad': 'Madrid'}
```

### `dict.popitem()`
El método `popitem()` elimina y devuelve el último par clave-valor insertado en el diccionario. Antes de Python 3.7, eliminaba un par clave-valor aleatorio.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
ultima_clave_valor = mi_diccionario.popitem()
print(ultima_clave_valor)  # Output: ('ciudad', 'Madrid')
print(mi_diccionario)  # Output: {'nombre': 'Juan', 'edad': 30}
```

### `dict.setdefault()`
El método `setdefault()` devuelve el valor de una clave si está en el diccionario. Si no, inserta la clave con un valor predeterminado y devuelve ese valor.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30}
valor = mi_diccionario.setdefault("ciudad", "Madrid")
print(valor)  # Output: Madrid
print(mi_diccionario)  # Output: {'nombre': 'Juan', 'edad': 30, 'ciudad': 'Madrid'}
```

### `dict.update()`
El método `update()` actualiza el diccionario con pares clave-valor de otro diccionario u otro iterable de pares clave-valor.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30}
mi_diccionario.update({"ciudad": "Madrid", "pais": "España"})
print(mi_diccionario)  # Output: {'nombre': 'Juan', 'edad': 30, 'ciudad': 'Madrid', 'pais': 'España'}
```

### `dict.values()`
El método `values()` devuelve una vista de objetos con todos los valores del diccionario.

```python
mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}
print(mi_diccionario.values())  # Output: dict_values(['Juan', 30, 'Madrid'])
```

---

## 3. Dictionary Comprehension

El Dictionary Comprehension es una forma concisa de crear diccionarios. Similar a las listas por comprensión, te permite construir diccionarios en una sola línea de código.

### Ejemplo Básico
```python
cuadrados = {x: x*x for x in range(6)}
print(cuadrados)  # Output: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### Ejemplo con Condicionales
```python
pares_cuadrados = {x: x*x for x in range(6) if x % 2 == 0}
print(pares_cuadrados)  # Output: {0: 0, 2: 4, 4: 16}
```

### Ejemplo de Transformación de Valores
```python
original = {"a": 1, "b": 2, "c": 3}
transformado = {k: v*2 for k, v in original.items()}
print(transformado)  # Output: {'a': 2, 'b': 4, 'c': 6}
```


