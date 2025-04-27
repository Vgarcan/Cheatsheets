# Tkinter Tutorial

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Instalación](#instalación)
   - [En Windows y macOS](#en-windows-y-macos)
   - [En Linux](#en-linux)
3. [Creando una Ventana Simple](#creando-una-ventana-simple)
4. [Elementos de Interfaz Comunes](#elementos-de-interfaz-comunes)
   - [Etiquetas](#etiquetas)
   - [Botones](#botones)
   - [Cuadros de Texto](#cuadros-de-texto)
5. [Otros Elementos de Interfaz](#otros-elementos-de-interfaz)
   - [Tablas](#tablas)
6. [Disposición de los Elementos](#disposición-de-los-elementos)
   - [Método pack()](#método-pack)
   - [Método grid()](#método-grid)
   - [Método place()](#método-place)
7. [Personalización de Colores y Fuentes](#personalización-de-colores-y-fuentes)
8. [Diferencias entre `tk` y `ttk`](#diferencias-entre-tk-y-ttk)
   - [Personalización con ttk](#personalización-con-ttk)
9. [Ventanas y Menús](#ventanas-y-menús)
   - [Creando Ventanas Adicionales](#creando-ventanas-adicionales)
   - [Creando Menús](#creando-menús)
10. [Navegación entre Ventanas](#navegación-entre-ventanas)
11. [Ejemplo Completo](#ejemplo-completo)
12. [Recursos Adicionales](#recursos-adicionales)

## Introducción

Tkinter es el paquete estándar de Python para crear interfaces gráficas de usuario (GUI). Proporciona un conjunto de herramientas robusto y flexible para crear aplicaciones con ventanas, botones, cuadros de texto, menús y más. Tkinter es fácil de usar y viene incluido con Python, lo que lo convierte en una excelente opción para desarrolladores principiantes y avanzados.

## Instalación

Tkinter viene incluido con Python, por lo que no necesitas instalarlo por separado si ya tienes Python en tu sistema. Sin embargo, en algunas distribuciones de Linux, puede ser necesario instalar Tkinter manualmente.

### En Windows y macOS

Tkinter viene preinstalado con Python, así que no necesitas hacer nada adicional.

### En Linux

Para instalar Tkinter en una distribución de Linux basada en Debian (como Ubuntu), usa el siguiente comando:

```sh
sudo apt-get install python3-tk
```

## Creando una Ventana Simple

Aquí tienes un ejemplo básico de cómo crear una ventana simple con Tkinter:

```python
import tkinter as tk

# Crear la ventana principal
root = tk.Tk()

# Establecer el título de la ventana
root.title("Mi Primera Ventana con Tkinter")

# Establecer el tamaño de la ventana
root.geometry("400x300")

# Iniciar el bucle principal de la aplicación
root.mainloop()
```

Este script crea una ventana con el título "Mi Primera Ventana con Tkinter" y un tamaño de 400x300 píxeles.

## Elementos de Interfaz Comunes

A continuación, añadiremos algunos elementos de interfaz a la ventana, como etiquetas, botones y cuadros de texto. También veremos la diferencia entre los widgets de `tk` y `ttk`.

### Etiquetas

Las etiquetas se utilizan para mostrar texto o imágenes en la ventana.

#### Con `tk`

```python
label = tk.Label(root, text="Hola, Tkinter!")
label.pack()
```

#### Con `ttk`

```python
from tkinter import ttk

label = ttk.Label(root, text="Hola, Tkinter!")
label.pack()
```

### Botones

Los botones se utilizan para ejecutar comandos cuando se hacen clic.

#### Con `tk`

```python
def on_button_click():
    label.config(text="¡Botón clicado!")

button = tk.Button(root, text="Haz clic aquí", command=on_button_click)
button.pack()
```

#### Con `ttk`

```python
button = ttk.Button(root, text="Haz clic aquí", command=on_button_click)
button.pack()
```

### Cuadros de Texto

Los cuadros de texto permiten la entrada de texto por parte del usuario.

#### Con `tk`

```python
entry = tk.Entry(root)
entry.pack()
```

#### Con `ttk`

```python
entry = ttk.Entry(root)
entry.pack()
```
## Otros Elementos de Interfaz

### Tablas

#### 1. Configurar el `Treeview` para Mostrar Datos en Forma de Tabla

Define el `Treeview`, sus columnas y el encabezado de la tabla.

```python
# Definir las columnas
columns = ("#1", "#2", "#3")

# Crear el Treeview
tree = ttk.Treeview(root, columns=columns, show='headings')

# Definir los encabezados
tree.heading("#1", text="ID")
tree.heading("#2", text="Nombre")
tree.heading("#3", text="Edad")

# Definir el ancho de las columnas
tree.column("#1", width=100, anchor=tk.CENTER)
tree.column("#2", width=250, anchor=tk.W)
tree.column("#3", width=100, anchor=tk.CENTER)

# Agregar datos a la tabla
data = [
    (1, "John Doe", 25),
    (2, "Jane Smith", 30),
    (3, "Mike Johnson", 35)
]

for item in data:
    tree.insert("", tk.END, values=item)

# Empaquetar el Treeview en la ventana
tree.pack(expand=True, fill=tk.BOTH)
```

#### 2. Añadir un Scrollbar (Opcional)

Para manejar muchos datos, es útil agregar una barra de desplazamiento.

```python
# Crear una barra de desplazamiento vertical
vsb = ttk.Scrollbar(root, orient="vertical", command=tree.yview)
vsb.pack(side='right', fill='y')

# Asociar la barra de desplazamiento con el Treeview
tree.configure(yscrollcommand=vsb.set)
```

### Explicación del Código

1. **Importaciones**: Importa `tkinter` y `ttk` para crear la interfaz gráfica.
2. **Ventana Principal**: Crea la ventana principal y establece el título y tamaño.
3. **Treeview**:
   - Define las columnas que tendrá la tabla.
   - Crea el widget `Treeview` y especifica que debe mostrar solo los encabezados (`show='headings'`).
   - Establece los encabezados de las columnas y el ancho de cada una.
   - Inserta datos en la tabla usando el método `insert`.
4. **Scrollbar**: Agrega una barra de desplazamiento vertical y la asocia con el `Treeview`.
5. **Bucle Principal**: Inicia el bucle principal de la aplicación con `root.mainloop()`.

## Disposición de los Elementos

Tkinter ofrece varias maneras de disponer los elementos dentro de la ventana. Los tres métodos principales son `pack()`, `grid()` y `place()`.

### Método pack()

El método `pack()` coloca los widgets en la ventana en el orden en que se añaden, uno tras otro.

```python
label = tk.Label(root, text="Etiqueta con pack")
label.pack(side="top", fill="x")
```

### Método grid()

El método `grid()` organiza los widgets en una cuadrícula.

```python
label = tk.Label(root, text="Etiqueta con grid")
label.grid(row=0, column=0)
button = tk.Button(root, text="Botón con grid")
button.grid(row=1, column=1)
```

### Método place()

El método `place()` coloca los widgets en coordenadas específicas dentro de la ventana.

```python
label = tk.Label(root, text="Etiqueta con place")
label.place(x=50, y=50)
```

## Personalización de Colores y Fuentes

Puedes personalizar los colores y las fuentes de los elementos de la interfaz.

### Personalización de Colores

```python
label = tk.Label(root, text="Hola, Tkinter!", bg="yellow", fg="blue")
label.pack()

button = tk.Button(root, text="Haz clic aquí", command=on_button_click, bg="red", fg="white")
button.pack()
```

### Personalización de Fuentes

```python
from tkinter import font

custom_font = font.Font(family="Helvetica", size=16, weight="bold")

label = tk.Label(root, text="Texto con Fuente Personalizada", font=custom_font)
label.pack()
```

## Diferencias entre `tk` y `ttk`

Tkinter ofrece dos conjuntos de widgets: `tk` y `ttk`.

- `tk`: Widgets tradicionales de Tkinter.
- `ttk`: Widgets mejorados con soporte para estilos y una apariencia más moderna.

### Personalización con ttk

Los widgets `ttk` pueden personalizarse utilizando estilos.

```python
style = ttk.Style()
style.configure("TButton", font=("Helvetica", 12), padding=10)

button = ttk.Button(root, text="Botón Estilizado")
button.pack()
```

## Ventanas y Menús

### Creando Ventanas Adicionales

Puedes crear ventanas adicionales (ventanas secundarias) en tu aplicación.

```python
def open_new_window():
    new_window = tk.Toplevel(root)
    new_window.title("Nueva Ventana")
    new_window.geometry("200x150")
    tk.Label(new_window, text="Esta es una nueva ventana").pack()

button = tk.Button(root, text="Abrir Nueva Ventana", command=open_new_window)
button.pack()
```

### Creando Menús

Puedes añadir menús a tu aplicación usando el widget `Menu`.

```python
menu_bar = tk.Menu(root)

file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Abrir")
file_menu.add_command(label="Guardar")
file_menu.add_separator()
file_menu.add_command(label="Salir", command=root.quit)

menu_bar.add_cascade(label="Archivo", menu=file_menu)
root.config(menu=menu_bar)
```

## Navegación entre Ventanas

Para navegar entre diferentes ventanas, puedes ocultar la ventana actual y mostrar otra.

```python
def show_window1():
    window2.withdraw()
    window1.deiconify()

def show_window2():
    window1.withdraw()
    window2.deiconify()

window1 = tk.Toplevel(root)
window1.title("Ventana 1")
window1.geometry("400x300")
window1.withdraw()

window2 = tk.Toplevel(window1)
window2.title("Ventana 2")
window2.geometry("400x300")
window2.withdraw()

button_to_window2 = tk.Button(window1, text="Ir a Ventana 2", command=show_window2)
button_to_window2.pack()

button_to_window1 = tk.Button(window2, text="Volver a Ventana 1", command=show_window1)
button_to_window1.pack()
```

## Ejemplo Completo

Aquí tienes un ejemplo completo que incorpora todos los elementos mencionados anteriormente:

```python
import tkinter as tk
from tkinter import ttk, font

def on_button_click():
    label.config(text="¡Botón clicado!")

def show_entry_text():
    text = entry.get()
    label.config(text=text)

def open_new_window():
    new_window = tk.Toplevel(root)
    new_window.title("Nueva Ventana")
    new_window.geometry("200x150")
    tk.Label(new_window, text="Esta es una nueva ventana").pack()

def show_window1():
    window2.withdraw()
    window1.deiconify()

def show_window2():
    window1.withdraw()
    window2.deiconify()

# Crear la ventana principal
root = tk.Tk()
root.title("Ejemplo Completo de Tkinter")
root.geometry("400x300")

# Crear una barra de menús
menu_bar = tk.Menu(root)
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Abrir")
```python
file_menu.add_command(label="Guardar")
file_menu.add_separator()
file_menu.add_command(label="Salir", command=root.quit)
menu_bar.add_cascade(label="Archivo", menu=file_menu)
root.config(menu=menu_bar)

# Crear y colocar una etiqueta con color y fuente personalizada
custom_font = font.Font(family="Helvetica", size=16, weight="bold")
label = tk.Label(root, text="Hola, Tkinter!", bg="yellow", fg="blue", font=custom_font)
label.pack(pady=10)

# Crear y colocar un botón
button = tk.Button(root, text="Haz clic aquí", command=on_button_click, bg="red", fg="white")
button.pack(pady=10)

# Crear y colocar un cuadro de texto
entry = tk.Entry(root)
entry.pack(pady=10)

# Crear y colocar un botón para mostrar el texto del cuadro de texto
show_text_button = tk.Button(root, text="Mostrar Texto", command=show_entry_text)
show_text_button.pack(pady=10)

# Crear y colocar un botón para abrir una nueva ventana
open_window_button = tk.Button(root, text="Abrir Nueva Ventana", command=open_new_window)
open_window_button.pack(pady=10)

# Ventanas adicionales para navegación
window1 = tk.Toplevel(root)
window1.title("Ventana 1")
window1.geometry("400x300")
window1.withdraw()

window2 = tk.Toplevel(root)
window2.title("Ventana 2")
window2.geometry("400x300")
window2.withdraw()

# Botón para mostrar la ventana 2 desde la ventana 1
button_to_window2 = tk.Button(window1, text="Ir a Ventana 2", command=show_window2)
button_to_window2.pack(pady=10)

# Botón para mostrar la ventana 1 desde la ventana 2
button_to_window1 = tk.Button(window2, text="Volver a Ventana 1", command=show_window1)
button_to_window1.pack(pady=10)

# Botones para navegar a las ventanas adicionales desde la ventana principal
nav_button1 = tk.Button(root, text="Ir a Ventana 1", command=show_window1)
nav_button1.pack(pady=10)

nav_button2 = tk.Button(root, text="Ir a Ventana 2", command=show_window2)
nav_button2.pack(pady=10)

# Iniciar el bucle principal de la aplicación
root.mainloop()
```

## Recursos Adicionales

Para obtener más información sobre Tkinter, consulta los siguientes recursos:

- [Documentación oficial de Tkinter](https://docs.python.org/3/library/tkinter.html)
- [Tutorial de Tkinter en Python.org](https://docs.python.org/3/tutorial/gui.html)
- [Tkinter Wiki](https://wiki.python.org/moin/TkInter)

Con estos conceptos básicos y ejemplos completos, deberías estar listo para comenzar a crear tus propias aplicaciones GUI con Tkinter. ¡Diviértete programando!

## Ejemplo Completo con Navegación entre Ventanas

Para una implementación completa que también incluya la navegación entre ventanas, aquí tienes un ejemplo extendido:

```python
import tkinter as tk
from tkinter import ttk, font

def on_button_click():
    label.config(text="¡Botón clicado!")

def show_entry_text():
    text = entry.get()
    label.config(text=text)

def open_new_window():
    new_window = tk.Toplevel(root)
    new_window.title("Nueva Ventana")
    new_window.geometry("200x150")
    tk.Label(new_window, text="Esta es una nueva ventana").pack()

def show_window1():
    window2.withdraw()
    window1.deiconify()

def show_window2():
    window1.withdraw()
    window2.deiconify()

# Crear la ventana principal
root = tk.Tk()
root.title("Ejemplo Completo de Tkinter")
root.geometry("400x300")

# Crear una barra de menús
menu_bar = tk.Menu(root)
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Abrir")
file_menu.add_command(label="Guardar")
file_menu.add_separator()
file_menu.add_command(label="Salir", command=root.quit)
menu_bar.add_cascade(label="Archivo", menu=file_menu)
root.config(menu=menu_bar)

# Crear y colocar una etiqueta con color y fuente personalizada
custom_font = font.Font(family="Helvetica", size=16, weight="bold")
label = tk.Label(root, text="Hola, Tkinter!", bg="yellow", fg="blue", font=custom_font)
label.pack(pady=10)

# Crear y colocar un botón
button = tk.Button(root, text="Haz clic aquí", command=on_button_click, bg="red", fg="white")
button.pack(pady=10)

# Crear y colocar un cuadro de texto
entry = tk.Entry(root)
entry.pack(pady=10)

# Crear y colocar un botón para mostrar el texto del cuadro de texto
show_text_button = tk.Button(root, text="Mostrar Texto", command=show_entry_text)
show_text_button.pack(pady=10)

# Crear y colocar un botón para abrir una nueva ventana
open_window_button = tk.Button(root, text="Abrir Nueva Ventana", command=open_new_window)
open_window_button.pack(pady=10)

# Ventanas adicionales para navegación
window1 = tk.Toplevel(root)
window1.title("Ventana 1")
window1.geometry("400x300")
window1.withdraw()

window2 = tk.Toplevel(window1)
window2.title("Ventana 2")
window2.geometry("400x300")
window2.withdraw()

# Botón para mostrar la ventana 2 desde la ventana 1
button_to_window2 = tk.Button(window1, text="Ir a Ventana 2", command=show_window2)
button_to_window2.pack(pady=10)

# Botón para mostrar la ventana 1 desde la ventana 2
button_to_window1 = tk.Button(window2, text="Volver a Ventana 1", command=show_window1)
button_to_window1.pack(pady=10)

# Botones para navegar a las ventanas adicionales desde la ventana principal
nav_button1 = tk.Button(root, text="Ir a Ventana 1", command=show_window1)
nav_button1.pack(pady=10)

nav_button2 = tk.Button(root, text="Ir a Ventana 2", command=show_window2)
nav_button2.pack(pady=10)

# Iniciar el bucle principal de la aplicación
root.mainloop()
```
## Recursos Adicionales

Para obtener más información sobre Tkinter, consulta los siguientes recursos:

- [Documentación oficial de Tkinter](https://docs.python.org/3/library/tkinter.html)
- [Tutorial de Tkinter en Python.org](https://docs.python.org/3/tutorial/gui.html)
- [Tkinter Wiki](https://wiki.python.org/moin/TkInter)

Con estos conceptos básicos y ejemplos completos, deberías estar listo para comenzar a crear tus propias aplicaciones GUI con Tkinter. ¡Diviértete programando!

## Elementos de Interfaz Adicionales

Además de las etiquetas, botones y cuadros de texto, Tkinter y ttk proporcionan una variedad de otros elementos que puedes utilizar en tus aplicaciones.

### Listbox

El Listbox se utiliza para mostrar una lista de elementos.

#### Con `tk`

```python
listbox = tk.Listbox(root)
listbox.pack()
listbox.insert(1, "Elemento 1")
listbox.insert(2, "Elemento 2")
```

#### Con `ttk`

```python
listbox = tk.Listbox(root)
listbox.pack()
listbox.insert(1, "Elemento 1")
listbox.insert(2, "Elemento 2")
```

### Combobox

El Combobox se utiliza para crear un cuadro de selección desplegable.

```python
combobox = ttk.Combobox(root)
combobox['values'] = ("Opción 1", "Opción 2", "Opción 3")
combobox.pack()
```

### Checkbutton

El Checkbutton se utiliza para crear casillas de verificación.

#### Con `tk`

```python
checkbutton = tk.Checkbutton(root, text="Aceptar Términos y Condiciones")
checkbutton.pack()
```

#### Con `ttk`

```python
checkbutton = ttk.Checkbutton(root, text="Aceptar Términos y Condiciones")
checkbutton.pack()
```

### Radiobutton

El Radiobutton se utiliza para crear botones de opción.

#### Con `tk`

```python
radio_var = tk.IntVar()
radiobutton1 = tk.Radiobutton(root, text="Opción 1", variable=radio_var, value=1)
radiobutton2 = tk.Radiobutton(root, text="Opción 2", variable=radio_var, value=2)
radiobutton1.pack()
radiobutton2.pack()
```

#### Con `ttk`

```python
radio_var = tk.IntVar()
radiobutton1 = ttk.Radiobutton(root, text="Opción 1", variable=radio_var, value=1)
radiobutton2 = ttk.Radiobutton(root, text="Opción 2", variable=radio_var, value=2)
radiobutton1.pack()
radiobutton2.pack()
```

### Scale

El Scale se utiliza para crear una barra deslizante.

#### Con `tk`

```python
scale = tk.Scale(root, from_=0, to=100, orient=tk.HORIZONTAL)
scale.pack()
```

#### Con `ttk`

```python
scale = ttk.Scale(root, from_=0, to=100, orient=tk.HORIZONTAL)
scale.pack()
```

### Progressbar

La Progressbar se utiliza para mostrar el progreso de una operación.

```python
progressbar = ttk.Progressbar(root, length=200, mode='determinate')
progressbar.pack()
progressbar['value'] = 50
```

### Separator

El Separator se utiliza para crear una línea de separación horizontal o vertical.

```python
separator = ttk.Separator(root, orient='horizontal')
separator.pack(fill='x')
```

## Personalización de Estilos con ttk

### Cambiando el Estilo de un Widget

```python
style = ttk.Style()
style.configure("TButton", font=("Helvetica", 12), padding=10)

button = ttk.Button(root, text="Botón Estilizado", style="TButton")
button.pack()
```

### Temas en ttk

ttk proporciona varios temas predefinidos que puedes utilizar para cambiar la apariencia general de tu aplicación.

```python
style.theme_use('clam')
```

## Ejemplo Completo con ttk y Widgets Adicionales

```python
import tkinter as tk
from tkinter import ttk, font

def on_button_click():
    label.config(text="¡Botón clicado!")

def show_entry_text():
    text = entry.get()
    label.config(text=text)

def open_new_window():
    new_window = tk.Toplevel(root)
    new_window.title("Nueva Ventana")
    new_window.geometry("200x150")
    tk.Label(new_window, text="Esta es una nueva ventana").pack()

def show_window1():
    window2.withdraw()
    window1.deiconify()

def show_window2():
    window1.withdraw()
    window2.deiconify()

# Crear la ventana principal
root = tk.Tk()
root.title("Ejemplo Completo de Tkinter")
root.geometry("400x500")

# Crear una barra de menús
menu_bar = tk.Menu(root)
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Abrir")
file_menu.add_command(label="Guardar")
file_menu.add_separator()
file_menu.add_command(label="Salir", command=root.quit)
menu_bar.add_cascade(label="Archivo", menu=file_menu)
root.config(menu=menu_bar)

# Crear y colocar una etiqueta con color y fuente personalizada
custom_font = font.Font(family="Helvetica", size=16, weight="bold")
label = tk.Label(root, text="Hola, Tkinter!", bg="yellow", fg="blue", font=custom_font)
label.pack(pady=10)

# Crear y colocar un botón
button = tk.Button(root, text="Haz clic aquí", command=on_button_click, bg="red", fg="white")
button.pack(pady=10)

# Crear y colocar un cuadro de texto
entry = tk.Entry(root)
entry.pack(pady=10)

# Crear y colocar un botón para mostrar el texto del cuadro de texto
show_text_button = tk.Button(root, text="Mostrar Texto", command=show_entry_text)
show_text_button.pack(pady=10)

# Crear y colocar un botón para abrir una nueva ventana
open_window_button = tk.Button(root, text="Abrir Nueva Ventana", command=open_new_window)
open_window_button.pack(pady=10)

# Ventanas adicionales para navegación
window1 = tk.Toplevel(root)
window1.title("Ventana 1")
window1.geometry("400x300")
window1.withdraw()

window2 = tk.Toplevel(root)
window2.title("Ventana 2")
window2.geometry("400x300")
window2.withdraw()

# Botón para mostrar la ventana 2 desde la ventana 1
button_to_window2 = tk.Button(window1, text="Ir a Ventana 2", command=show_window2)
button_to_window2.pack(pady=10)

# Botón para mostrar la ventana 1 desde la ventana 2
button_to_window1 = tk.Button(window2, text="Volver a Ventana 1", command=show_window1)
button_to_window1.pack(pady=10)

# Botones para navegar a las ventanas adicionales desde la ventana principal
nav_button1 = tk.Button(root, text="Ir a Ventana 1", command=show_window1)
nav_button1.pack(pady=10)

nav_button2 = tk.Button(root, text="Ir a Ventana 2", command=show_window2)
nav_button2.pack(pady=10)

# Añadir elementos adicionales de interfaz
listbox = tk.Listbox(root)
listbox.pack(pady=10)
listbox.insert(1, "Elemento 1")
listbox.insert(2, "Elemento 2")

combobox = ttk.Combobox(root)
combobox['values'] = ("Opción 1", "Opción 2", "Opción 3")
combobox.pack(pady=10)

checkbutton = ttk.Checkbutton(root, text="Aceptar Términos y Condiciones")
checkbutton.pack(pady=10)

radio_var = tk.IntVar()
radiobutton1 = ttk.Radiobutton(root, text="Opción 1", variable=radio_var, value=1)
radiobutton2 = ttk.Radiobutton(root, text="Opción 2", variable=radio_var, value=2)
radiobutton1.pack(pady=10)
radiobutton2.pack(pady=10)

scale = ttk.Scale(root, from_=0, to=100, orient=tk.HORIZONTAL)
scale.pack(pady=10)

progressbar = ttk.Progressbar(root, length=200, mode='determinate')
progressbar.pack(pady=10)
progressbar['value'] = 50

separator = ttk.Separator(root, orient='horizontal')
separator.pack(fill='x', pady=10)

# Iniciar el bucle principal de la aplicación
root.mainloop()
```

Este ejemplo completo incluye una variedad de widgets y características de Tkinter y ttk, mostrando cómo personalizar y organizar elementos en tu aplicación de GUI. ¡Disfruta explorando y creando con Tkinter!