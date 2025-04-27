# Guía Completa de Pillow para la Manipulación de Imágenes en Python

## Tabla de Contenidos

1. [¿Qué es Pillow?](#qué-es-pillow)
2. [Instalación de Pillow](#instalación-de-pillow)
3. [Abrir y Mostrar Imágenes](#abrir-y-mostrar-imágenes)
4. [Guardado de Imágenes](#guardado-de-imágenes)
5. [Manipulación Básica de Imágenes](#manipulación-básica-de-imágenes)
    - [Cambiar el Tamaño de las Imágenes](#cambiar-el-tamaño-de-las-imágenes)
    - [Rotar Imágenes](#rotar-imágenes)
    - [Voltear Imágenes](#voltear-imágenes)
6. [Manipulación Avanzada de Imágenes](#manipulación-avanzada-de-imágenes)
    - [Recortar Imágenes](#recortar-imágenes)
    - [Aplicar Filtros](#aplicar-filtros)
    - [Convertir Entre Modos de Color](#convertir-entre-modos-de-color)
7. [Operaciones con Texto en Imágenes](#operaciones-con-texto-en-imágenes)
8. [Trabajar con Canales y Transparencia](#trabajar-con-canales-y-transparencia)
9. [Manipulación de Formatos de Imágenes](#manipulación-de-formatos-de-imágenes)
10. [Combinar y Superponer Imágenes](#combinar-y-superponer-imágenes)
11. [Creación de Miniaturas](#creación-de-miniaturas)
12. [Solución de Problemas y Errores Comunes](#solución-de-problemas-y-errores-comunes)
13. [Conclusión](#conclusión)

---

## 1. ¿Qué es Pillow?

**Pillow** es una biblioteca de Python utilizada para la manipulación y edición de imágenes. Es una continuación y mejora de la antigua biblioteca **PIL (Python Imaging Library)**. Con Pillow, puedes realizar una amplia gama de operaciones sobre imágenes, desde abrir y guardar archivos hasta aplicar transformaciones complejas, filtros y cambios en los modos de color. Pillow es ideal para proyectos de procesamiento de imágenes y desarrollo web donde se requiere manipulación dinámica de imágenes.

---

## 2. Instalación de Pillow

Para instalar Pillow, debes usar el administrador de paquetes **pip**:

```bash
pip install Pillow
```

Para verificar que la instalación se realizó correctamente, puedes ejecutar el siguiente código:

```python
from PIL import Image
print(Image.__version__)
```

Si todo está bien, verás la versión de Pillow instalada.

---

## 3. Abrir y Mostrar Imágenes

Abrir y mostrar imágenes con Pillow es sencillo. Utilizamos la función `Image.open()` para cargar la imagen y el método `show()` para visualizarla.

```python
from PIL import Image

# Abrir una imagen
imagen = Image.open('ruta/a/tu-imagen.jpg')

# Mostrar la imagen
imagen.show()
```

Este comando abrirá la imagen con la aplicación predeterminada de visualización de imágenes del sistema.

### Trabajar con la Ruta de la Imagen

En ocasiones, es útil conocer la ruta de la imagen y su formato. Puedes obtener esta información fácilmente usando los atributos `format`, `size` y `mode`.

```python
print(imagen.format)  # Ejemplo: JPEG
print(imagen.size)    # Ejemplo: (1920, 1080)
print(imagen.mode)    # Ejemplo: RGB
```

---

## 4. Guardado de Imágenes

Para guardar imágenes, puedes utilizar el método `save()`, especificando el formato de la imagen si es necesario. Pillow soporta una amplia variedad de formatos como **JPEG**, **PNG**, **BMP**, **GIF**, entre otros.

```python
# Guardar la imagen en un formato diferente
imagen.save('nueva-imagen.png')

# Guardar con calidad específica (para JPEG)
imagen.save('nueva-imagen.jpg', quality=90)
```

Además, puedes comprimir la imagen al guardarla:

```python
# Guardar en formato JPEG optimizado
imagen.save('nueva-imagen-optimizada.jpg', quality=85, optimize=True)
```

---

## 5. Manipulación Básica de Imágenes

Pillow permite realizar varias manipulaciones básicas como cambiar el tamaño, rotar y voltear las imágenes.

### Cambiar el Tamaño de las Imágenes

El método `resize()` te permite redimensionar las imágenes a las dimensiones que especifiques.

```python
# Redimensionar la imagen a 200x200 píxeles
imagen_redimensionada = imagen.resize((200, 200))
imagen_redimensionada.show()
```

Para mantener la relación de aspecto de la imagen, puedes calcular las nuevas dimensiones usando proporciones.

```python
ancho_original, alto_original = imagen.size
relacion_aspecto = ancho_original / alto_original
nuevo_ancho = 400
nuevo_alto = int(nuevo_ancho / relacion_aspecto)

# Redimensionar manteniendo la proporción
imagen_redimensionada = imagen.resize((nuevo_ancho, nuevo_alto))
imagen_redimensionada.show()
```

### Rotar Imágenes

Puedes rotar las imágenes en grados con el método `rotate()`. Ten en cuenta que el ángulo se mide en sentido antihorario.

```python
# Rotar la imagen 90 grados
imagen_rotada = imagen.rotate(90)
imagen_rotada.show()
```

Si no quieres perder parte de la imagen debido al recorte durante la rotación, puedes usar `expand=True` para ajustar el tamaño del lienzo automáticamente.

```python
imagen_rotada_expandida = imagen.rotate(45, expand=True)
imagen_rotada_expandida.show()
```

### Voltear Imágenes

Para voltear las imágenes horizontal o verticalmente, puedes usar los métodos `transpose()` junto con las constantes `FLIP_LEFT_RIGHT` o `FLIP_TOP_BOTTOM`.

```python
# Voltear horizontalmente
imagen_volteada_h = imagen.transpose(Image.FLIP_LEFT_RIGHT)
imagen_volteada_h.show()

# Voltear verticalmente
imagen_volteada_v = imagen.transpose(Image.FLIP_TOP_BOTTOM)
imagen_volteada_v.show()
```

---

## 6. Manipulación Avanzada de Imágenes

### Recortar Imágenes

Pillow te permite recortar una parte de la imagen utilizando las coordenadas `(left, top, right, bottom)`.

```python
# Recortar una región de la imagen
imagen_recortada = imagen.crop((50, 50, 400, 400))
imagen_recortada.show()
```

Este método es útil para enfocar una parte específica de la imagen.

### Aplicar Filtros

Pillow incluye una variedad de filtros predefinidos a través de la clase `ImageFilter`. Algunos ejemplos comunes son el desenfoque, contorno, y suavizado.

```python
from PIL import ImageFilter

# Aplicar un filtro de desenfoque
imagen_desenfocada = imagen.filter(ImageFilter.BLUR)
imagen_desenfocada.show()

# Aplicar filtro de contorno
imagen_contorno = imagen.filter(ImageFilter.CONTOUR)
imagen_contorno.show()
```

Algunos filtros adicionales incluyen:

- **SHARPEN**: Agudiza los detalles de la imagen.
- **DETAIL**: Mejora los detalles de la imagen.
- **EDGE_ENHANCE**: Resalta los bordes.

### Convertir Entre Modos de Color

Pillow permite convertir entre diferentes modos de color como **RGB**, **L** (escala de grises), y **RGBA** (transparencia). Esto es útil cuando necesitas cambiar el modo de la imagen para ciertas operaciones.

```python
# Convertir a escala de grises
imagen_gris = imagen.convert('L')
imagen_gris.show()

# Convertir a RGBA (para trabajar con transparencia)
imagen_rgba = imagen.convert('RGBA')
imagen_rgba.show()
```

---

## 7. Operaciones con Texto en Imágenes

Puedes agregar texto a las imágenes utilizando la clase `ImageDraw`. También es posible personalizar el estilo de la fuente utilizando `ImageFont`.

```python
from PIL import ImageDraw, ImageFont

# Crear un objeto para dibujar en la imagen
draw = ImageDraw.Draw(imagen)

# Definir la fuente y el tamaño
fuente = ImageFont.truetype('arial.ttf', 36)

# Agregar texto en la imagen
draw.text((50, 50), 'Hola Mundo', font=fuente, fill='white')

# Mostrar la imagen con el texto añadido
imagen.show()
```

Si no tienes una fuente personalizada, puedes utilizar la fuente por defecto que proporciona Pillow con `ImageFont.load_default()`.

---

## 8. Trabajar con Canales y Transparencia

Pillow permite trabajar con imágenes con transparencia utilizando el modo **RGBA**. Cada canal (rojo, verde, azul, alfa) puede ser manipulado por separado.

```python
# Abrir una imagen con canal alfa
imagen_rgba = Image.open('imagen-con-transparencia.png')

# Separar los canales
rojo, verde, azul, alfa = imagen_rgba.split()

# Mostrar el canal alfa
alfa.show()
```

Puedes manipular los valores de transparencia ajustando el canal alfa y luego volviendo a combinar los canales.

```python
# Modificar la transparencia al 50%
nuevo_alfa = alfa.point(lambda p: p // 2)

# Combinar los canales

 de nuevo
nueva_imagen_rgba = Image.merge('RGBA', (rojo, verde, azul, nuevo_alfa))
nueva_imagen_rgba.show()
```

---

## 9. Manipulación de Formatos de Imágenes

Pillow soporta una variedad de formatos como **JPEG**, **PNG**, **BMP**, **GIF** y **TIFF**. Puedes convertir entre estos formatos fácilmente al guardar la imagen.

```python
# Convertir de PNG a JPEG
imagen.save('imagen_convertida.jpg', format='JPEG')
```

Además, puedes obtener información sobre el formato, tamaño y modo de la imagen utilizando atributos simples.

```python
print(imagen.format)  # Formato original
print(imagen.size)    # Tamaño de la imagen
print(imagen.mode)    # Modo de la imagen (RGB, L, etc.)
```

---

## 10. Combinar y Superponer Imágenes

Pillow permite combinar varias imágenes en una sola o superponer imágenes usando `paste()`.

```python
# Cargar dos imágenes
imagen_fondo = Image.open('fondo.png')
imagen_superpuesta = Image.open('logo.png')

# Pegar la imagen 'logo' sobre la imagen 'fondo'
imagen_fondo.paste(imagen_superpuesta, (50, 50), imagen_superpuesta)

# Mostrar la imagen resultante
imagen_fondo.show()
```

En este caso, la imagen `logo.png` debe tener un canal alfa para que funcione correctamente como una superposición transparente.

---

## 11. Creación de Miniaturas

Pillow ofrece una forma sencilla de generar miniaturas utilizando el método `thumbnail()`. Esto redimensionará la imagen manteniendo la relación de aspecto.

```python
imagen.thumbnail((128, 128))
imagen.show()
```

El método `thumbnail()` modifica la imagen en el lugar (in-place), lo que significa que no devuelve una nueva imagen.

---

## 12. Solución de Problemas y Errores Comunes

### Error: `OSError: cannot identify image file`
Este error ocurre si el archivo no es una imagen válida o si Pillow no admite su formato. Asegúrate de que el archivo es una imagen compatible o usa `Image.verify()` para verificar el archivo.

```python
try:
    imagen.verify()
except Exception as e:
    print(f"Error: {e}")
```

### Error al trabajar con fuentes: `OSError: cannot open resource`
Este error ocurre cuando no se encuentra el archivo de fuente. Asegúrate de especificar la ruta completa al archivo `.ttf` o utiliza la fuente predeterminada con `ImageFont.load_default()`.

---

## 13. Conclusión

Pillow es una herramienta poderosa y versátil para la manipulación de imágenes en Python. Te permite realizar desde tareas básicas como cambiar el tamaño y rotar imágenes, hasta operaciones avanzadas como trabajar con transparencia, canales y filtros. Con esta guía extendida y detallada, ahora estás equipado para manejar imágenes de manera eficiente en tus proyectos de Python.
