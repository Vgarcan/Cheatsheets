# 📚 JavaScript Basics Cheat Sheet

## 📑 Table of Contents

- [📚 JavaScript Basics Cheat Sheet](#-javascript-basics-cheat-sheet)
  - [📑 Table of Contents](#-table-of-contents)
  - [Variables](#variables)
  - [Data Types](#data-types)
  - [Operators](#operators)
  - [Conditionals](#conditionals)
  - [Functions](#functions)
  - [Loops](#loops)
  - [Arrays](#arrays)
  - [Array Iteration Methods](#array-iteration-methods)
  - [Objects](#objects)
  - [Object Utilities](#object-utilities)
  - [String Methods](#string-methods)
  - [Template Literals](#template-literals)
  - [Destructuring](#destructuring)
  - [Spread and Rest Operators](#spread-and-rest-operators)
  - [Short-Circuiting and Nullish Coalescing](#short-circuiting-and-nullish-coalescing)
  - [Optional Chaining](#optional-chaining)
  - [Type Conversion](#type-conversion)
  - [Type Checking](#type-checking)
  - [Try...Catch (Error Handling)](#trycatch-error-handling)
  - [Timing Functions](#timing-functions)
  - [Math Methods](#math-methods)
  - [Date Methods](#date-methods)
- [📚 JavaScript Cheat Sheet for DOM Manipulation](#-javascript-cheat-sheet-for-dom-manipulation)
  - [Accessing Elements](#accessing-elements)
  - [Modifying Elements](#modifying-elements)
  - [Classes](#classes)
  - [Traversing the DOM](#traversing-the-dom)
  - [Creating Elements](#creating-elements)
  - [Removing Elements](#removing-elements)
  - [Events](#events)
  - [Event Object](#event-object)
  - [Event Delegation](#event-delegation)
  - [Form Events](#form-events)
  - [Changing Attributes Dynamically](#changing-attributes-dynamically)
  - [Dynamic Styling with Class Toggle](#dynamic-styling-with-class-toggle)
  - [Page Load and DOMContentLoaded](#page-load-and-domcontentloaded)
  - [Scroll Events](#scroll-events)
  - [Prevent Default Behavior](#prevent-default-behavior)
  - [Stop Propagation](#stop-propagation)
- [📚 JavaScript Cheat Sheet for Advanced DOM Manipulation](#-javascript-cheat-sheet-for-advanced-dom-manipulation)
  - [Cloning Elements](#cloning-elements)
  - [insertAdjacentHTML / insertAdjacentElement / insertAdjacentText](#insertadjacenthtml--insertadjacentelement--insertadjacenttext)
    - [Positions for `insertAdjacent*`](#positions-for-insertadjacent)
  - [Document Fragments (Performance optimization)](#document-fragments-performance-optimization)
  - [Template Elements (HTML Templates)](#template-elements-html-templates)
  - [Managing Data Attributes](#managing-data-attributes)
  - [CSS Variables with JavaScript](#css-variables-with-javascript)
  - [Drag \& Drop Basic](#drag--drop-basic)
  - [Intersection Observer API (Detect when elements are visible)](#intersection-observer-api-detect-when-elements-are-visible)
  - [Mutation Observer (Watch for changes in the DOM)](#mutation-observer-watch-for-changes-in-the-dom)
- [📚 JavaScript Asynchronous Programming Cheat Sheet](#-javascript-asynchronous-programming-cheat-sheet)
  - [¿Qué es la asincronía en JavaScript?](#qué-es-la-asincronía-en-javascript)
  - [Código Síncrono](#código-síncrono)
  - [Código Asíncrono](#código-asíncrono)
- [🛠️ Herramientas de Asincronía en JavaScript](#️-herramientas-de-asincronía-en-javascript)
  - [1. Callbacks](#1-callbacks)
    - [Problema: Callback Hell](#problema-callback-hell)
  - [2. Promises](#2-promises)
    - [Crear una Promise](#crear-una-promise)
  - [3. Async/Await](#3-asyncawait)
    - [Ejemplo simple](#ejemplo-simple)
- [🎯 Resumen Visual](#-resumen-visual)
- [🌟 Aplicaciones prácticas](#-aplicaciones-prácticas)
- [📚 JavaScript Asynchronous Practical Examples](#-javascript-asynchronous-practical-examples)
  - [1. Fetch básico (petición a API pública)](#1-fetch-básico-petición-a-api-pública)
  - [2. Varias peticiones al mismo tiempo con Promise.all](#2-varias-peticiones-al-mismo-tiempo-con-promiseall)
  - [3. 🛠️ Mini proyecto: App de usuarios (cargar y mostrar datos + manejar errores)](#3-️-mini-proyecto-app-de-usuarios-cargar-y-mostrar-datos--manejar-errores)
- [🎯 Buenas Prácticas](#-buenas-prácticas)
- [📚 JavaScript - Custom Promises and Retry Mechanisms](#-javascript---custom-promises-and-retry-mechanisms)
  - [1. Crear tu propia Promise personalizada](#1-crear-tu-propia-promise-personalizada)
    - [Promise que puede fallar o tener éxito](#promise-que-puede-fallar-o-tener-éxito)
  - [2. Retry automático: intentar varias veces si falla](#2-retry-automático-intentar-varias-veces-si-falla)
    - [Ejemplo práctico: Retry de petición a una API](#ejemplo-práctico-retry-de-petición-a-una-api)
- [🎯 Buenas prácticas al usar Promises y Retry](#-buenas-prácticas-al-usar-promises-y-retry)

## Variables

```javascript
let name = "John";
const age = 30;
var city = "New York"; // Older way
```

## Data Types

```javascript
let text = "Hello";           // String
let number = 100;             // Number
let isActive = true;          // Boolean
let empty = null;             // Null
let notAssigned;              // Undefined
let user = { name: "Jane" };  // Object
let colors = ["red", "blue"]; // Array
let id = Symbol("id");        // Symbol
let bigNumber = 123456789012345678901234567890n; // BigInt
```

## Operators

```javascript
// Arithmetic
+, -, *, /, %, **

// Assignment
=, +=, -=, *=, /=, %=

// Comparison
==, ===, !=, !==, >, <, >=, <=

// Logical
&&, ||, !

// Nullish Coalescing
??

// Ternary Operator
const status = score > 50 ? "Passed" : "Failed";
```

## Conditionals

```javascript
if (condition) {
  // code
} else if (anotherCondition) {
  // code
} else {
  // code
}
```

## Functions

```javascript
// Function declaration
function greet(name) {
  return `Hello, ${name}`;
}

// Function expression
const add = function(a, b) {
  return a + b;
};

// Arrow function
const multiply = (a, b) => a * b;
```

## Loops

```javascript
// For loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// For...of loop
for (const color of colors) {
  console.log(color);
}

// For...in loop (for objects)
for (const key in user) {
  console.log(key, user[key]);
}

// While loop
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// Do...while loop
let j = 0;
do {
  console.log(j);
  j++;
} while (j < 5);
```

## Arrays

```javascript
let fruits = ["apple", "banana", "cherry"];

// Access
console.log(fruits[0]);

// Add/Remove
fruits.push("orange");     // Add at end
fruits.pop();              // Remove from end
fruits.shift();            // Remove from start
fruits.unshift("strawberry"); // Add at start

// Search
fruits.indexOf("banana");
fruits.includes("apple");

// Modify
fruits.slice(1, 3);       // Returns new array
fruits.splice(1, 1, "grape"); // Modify original array
fruits.reverse();
fruits.sort();
```

## Array Iteration Methods

```javascript
fruits.forEach(fruit => console.log(fruit));

const upperFruits = fruits.map(fruit => fruit.toUpperCase());

const longFruits = fruits.filter(fruit => fruit.length > 5);

const firstLongFruit = fruits.find(fruit => fruit.length > 5);

const hasShortFruit = fruits.some(fruit => fruit.length < 5);

const allLongFruits = fruits.every(fruit => fruit.length > 5);

const totalLength = fruits.reduce((total, fruit) => total + fruit.length, 0);
```

## Objects

```javascript
let person = {
  name: "Alice",
  age: 30,
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

// Access
console.log(person.name);
console.log(person["age"]);

// Modify
person.city = "Paris";
delete person.age;

// Check
console.log("city" in person);

// Get keys, values, entries
Object.keys(person);
Object.values(person);
Object.entries(person);

// Clone
const newPerson = Object.assign({}, person);

// Spread operator
const anotherPerson = { ...person, job: "Engineer" };
```

## Object Utilities

```javascript
// Freeze object (immutable)
Object.freeze(person);

// Seal object (can't add or remove properties)
Object.seal(person);
```

## String Methods

```javascript
let text = " Hello World ";

// Basics
text.length;
text.trim();
text.toUpperCase();
text.toLowerCase();

// Operations
text.includes("World");
text.startsWith("Hello");
text.endsWith("World");
text.split(" ");
text.replace("World", "Everyone");
```

## Template Literals

```javascript
const greeting = `Hello, ${name}!`;
```

## Destructuring

```javascript
// Arrays
const [first, second] = colors;

// Objects
const { name: personName, city = "Unknown" } = person;
```

## Spread and Rest Operators

```javascript
// Spread
const allFruits = [...fruits, "mango"];

// Rest
const sum = (...numbers) => numbers.reduce((a, b) => a + b, 0);
```

## Short-Circuiting and Nullish Coalescing

```javascript
const username = "";
const displayName = username || "Guest";

const input = null;
const output = input ?? "Default";
```

## Optional Chaining

```javascript
const country = user?.address?.country;
```

## Type Conversion

```javascript
String(123);     // "123"
Number("123");   // 123
Boolean(0);      // false
parseInt("123px"); // 123
parseFloat("12.34"); // 12.34
```

## Type Checking

```javascript
typeof "hello";   // "string"
typeof 123;       // "number"
typeof true;      // "boolean"
typeof undefined; // "undefined"
typeof null;      // "object" (special case)
typeof [];        // "object"
typeof {};        // "object"
Array.isArray([]); // true
```

## Try...Catch (Error Handling)

```javascript
try {
  throw new Error("Something went wrong!");
} catch (error) {
  console.error(error.message);
} finally {
  console.log("Always runs");
}
```

## Timing Functions

```javascript
setTimeout(() => {
  console.log("Runs after 2 seconds");
}, 2000);

const intervalId = setInterval(() => {
  console.log("Runs every second");
}, 1000);

clearInterval(intervalId);
```

## Math Methods

```javascript
Math.round(4.6);  // 5
Math.floor(4.9);  // 4
Math.ceil(4.1);   // 5
Math.max(10, 20); // 20
Math.min(10, 20); // 10
Math.random();    // 0 to 1
```

## Date Methods

```javascript
const now = new Date();

now.getFullYear();
now.getMonth();    // 0-11
now.getDate();     // 1-31
now.getDay();      // 0-6 (Sunday - Saturday)
now.getHours();
now.getMinutes();
now.toISOString();
```

# 📚 JavaScript Cheat Sheet for DOM Manipulation

## Accessing Elements

```javascript
// By ID
const title = document.getElementById("title");

// By Class
const items = document.getElementsByClassName("item");

// By Tag
const paragraphs = document.getElementsByTagName("p");

// Query Selector (first match)
const button = document.querySelector(".btn");

// Query Selector All (all matches)
const allButtons = document.querySelectorAll(".btn");
```

## Modifying Elements

```javascript
// Change text content
title.textContent = "New Title";

// Change inner HTML
title.innerHTML = "<span>Highlighted Title</span>";

// Change attribute
button.setAttribute("disabled", "true");

// Remove attribute
button.removeAttribute("disabled");

// Change styles directly
button.style.backgroundColor = "blue";
button.style.fontSize = "20px";
```

## Classes

```javascript
// Add class
button.classList.add("active");

// Remove class
button.classList.remove("active");

// Toggle class
button.classList.toggle("active");

// Check if element has a class
button.classList.contains("active");
```

## Traversing the DOM

```javascript
// Parent element
const parent = title.parentElement;

// Child elements
const children = parent.children;

// First child
const firstChild = parent.firstElementChild;

// Last child
const lastChild = parent.lastElementChild;

// Sibling elements
const nextSibling = title.nextElementSibling;
const prevSibling = title.previousElementSibling;
```

## Creating Elements

```javascript
// Create a new element
const newDiv = document.createElement("div");

// Set content
newDiv.textContent = "I am a new div";

// Set attributes or classes
newDiv.setAttribute("id", "new-div");
newDiv.classList.add("highlight");

// Append to an existing element
document.body.appendChild(newDiv);

// Insert before another element
const container = document.getElementById("container");
const referenceNode = document.getElementById("reference");
container.insertBefore(newDiv, referenceNode);
```

## Removing Elements

```javascript
// Remove an element
newDiv.remove();

// Old way
parent.removeChild(newDiv);
```

## Events

```javascript
// Add event listener
button.addEventListener("click", function() {
  console.log("Button clicked!");
});

// Arrow function
button.addEventListener("mouseenter", () => {
  console.log("Mouse entered button");
});

// Remove event listener
function handleClick() {
  console.log("Clicked once");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## Event Object

```javascript
button.addEventListener("click", (event) => {
  console.log(event.target); // The clicked element
  console.log(event.type);   // "click"
});
```

## Event Delegation

```javascript
document.body.addEventListener("click", function(e) {
  if (e.target.matches(".dynamic-btn")) {
    console.log("Dynamic button clicked!");
  }
});
```

## Form Events

```javascript
const form = document.querySelector("form");

// Submit
form.addEventListener("submit", (e) => {
  e.preventDefault(); // Prevent page reload
  console.log("Form submitted");
});

// Input
const inputField = document.getElementById("username");

inputField.addEventListener("input", (e) => {
  console.log(e.target.value);
});
```

## Changing Attributes Dynamically

```javascript
const image = document.querySelector("img");

// Change image source
image.src = "new-image.jpg";

// Change alt text
image.alt = "A beautiful scenery";
```

## Dynamic Styling with Class Toggle

```javascript
button.addEventListener("click", () => {
  button.classList.toggle("highlight");
});
```

## Page Load and DOMContentLoaded

```javascript
// Full page including styles/images loaded
window.addEventListener("load", () => {
  console.log("Page fully loaded");
});

// Only DOM loaded (faster)
document.addEventListener("DOMContentLoaded", () => {
  console.log("DOM loaded");
});
```

## Scroll Events

```javascript
window.addEventListener("scroll", () => {
  console.log(window.scrollY); // Vertical scroll position
});
```

## Prevent Default Behavior

```javascript
const link = document.querySelector("a");

link.addEventListener("click", (e) => {
  e.preventDefault();
  console.log("Link click prevented");
});
```

## Stop Propagation

```javascript
const outer = document.getElementById("outer");
const inner = document.getElementById("inner");

outer.addEventListener("click", () => {
  console.log("Outer clicked");
});

inner.addEventListener("click", (e) => {
  e.stopPropagation();
  console.log("Inner clicked without bubbling");
});
```
# 📚 JavaScript Cheat Sheet for Advanced DOM Manipulation

## Cloning Elements

```javascript
const card = document.querySelector(".card");

// Clone element (deep clone with children)
const clonedCard = card.cloneNode(true);

// Insert the clone
document.body.appendChild(clonedCard);
```

## insertAdjacentHTML / insertAdjacentElement / insertAdjacentText

```javascript
const container = document.getElementById("container");

// Insert HTML
container.insertAdjacentHTML("beforeend", "<p>Inserted Paragraph</p>");

// Insert Element
const newButton = document.createElement("button");
newButton.textContent = "Click Me";
container.insertAdjacentElement("afterbegin", newButton);

// Insert Text
container.insertAdjacentText("beforebegin", "Start of Container");
```

### Positions for `insertAdjacent*`
- `"beforebegin"` – before the element itself
- `"afterbegin"` – just inside the element, before first child
- `"beforeend"` – just inside the element, after last child
- `"afterend"` – after the element itself

## Document Fragments (Performance optimization)

```javascript
const fragment = document.createDocumentFragment();

for (let i = 0; i < 5; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i + 1}`;
  fragment.appendChild(li);
}

document.querySelector("ul").appendChild(fragment);
```

## Template Elements (HTML Templates)

```html
<template id="card-template">
  <div class="card">
    <h2 class="title"></h2>
    <p class="description"></p>
  </div>
</template>
```

```javascript
const template = document.getElementById("card-template");
const clone = template.content.cloneNode(true);

clone.querySelector(".title").textContent = "Card Title";
clone.querySelector(".description").textContent = "Card Description";

document.body.appendChild(clone);
```

## Managing Data Attributes

```javascript
const box = document.querySelector(".box");

// Set a data attribute
box.dataset.color = "blue";

// Get a data attribute
console.log(box.dataset.color);

// Remove a data attribute
delete box.dataset.color;
```

## CSS Variables with JavaScript

```javascript
// Get root element
const root = document.documentElement;

// Change a CSS variable
root.style.setProperty("--main-color", "coral");

// Read a CSS variable
const mainColor = getComputedStyle(root).getPropertyValue("--main-color");
console.log(mainColor);
```

## Drag & Drop Basic

```html
<div id="drag-item" draggable="true">Drag me!</div>
<div id="drop-area">Drop here</div>
```

```javascript
const dragItem = document.getElementById("drag-item");
const dropArea = document.getElementById("drop-area");

dragItem.addEventListener("dragstart", (e) => {
  e.dataTransfer.setData("text/plain", dragItem.id);
});

dropArea.addEventListener("dragover", (e) => {
  e.preventDefault(); // Necessary to allow a drop
});

dropArea.addEventListener("drop", (e) => {
  e.preventDefault();
  const id = e.dataTransfer.getData("text");
  const draggedElement = document.getElementById(id);
  dropArea.appendChild(draggedElement);
});
```

## Intersection Observer API (Detect when elements are visible)

```javascript
const target = document.querySelector(".target");

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log("Element is visible");
    } else {
      console.log("Element is not visible");
    }
  });
});

observer.observe(target);
```

## Mutation Observer (Watch for changes in the DOM)

```javascript
const observedElement = document.getElementById("observed");

const observer = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    console.log("Mutation detected:", mutation);
  });
});

observer.observe(observedElement, {
  childList: true,       // Watch for added or removed children
  attributes: true,      // Watch for attribute changes
  subtree: true          // Watch entire subtree
});

// Example mutation
observedElement.setAttribute("data-status", "updated");
```

---

# 📚 JavaScript Asynchronous Programming Cheat Sheet

---

## ¿Qué es la asincronía en JavaScript?

JavaScript es un **lenguaje de un solo hilo** (*single-threaded*), lo que significa que solo puede ejecutar **una tarea a la vez**.

Sin embargo, muchas operaciones (como leer archivos, hacer peticiones HTTP, o esperar tiempos) **no necesitan bloquear** el hilo principal.  
Por eso JavaScript usa **programación asincrónica**: puede delegar tareas a eventos externos y seguir ejecutando otras instrucciones.

**Resultado:**  
JavaScript puede ser **rápido** y **eficiente** incluso si algunas tareas tardan en completarse.

---

## Código Síncrono

```javascript
console.log("Inicio");

function tareaPesada() {
  for (let i = 0; i < 1e9; i++) {} // Simula trabajo pesado
}

tareaPesada();
console.log("Fin");
```

**Salida:**
```
Inicio
Fin (después de que termine tareaPesada)
```

✅ Aquí **nada ocurre hasta que la tarea anterior termina**.

---

## Código Asíncrono

```javascript
console.log("Inicio");

setTimeout(() => {
  console.log("Tarea asíncrona terminada");
}, 2000);

console.log("Fin");
```

**Salida:**
```
Inicio
Fin
Tarea asíncrona terminada
```

✅ Aquí **JavaScript no espera** el `setTimeout`, sigue ejecutando.

---

# 🛠️ Herramientas de Asincronía en JavaScript

---

## 1. Callbacks

Un **Callback** es simplemente una **función** que se pasa como **argumento** a otra función, para que sea llamada después de terminar alguna tarea.

```javascript
function saludar(nombre, callback) {
  console.log(`Hola, ${nombre}`);
  callback();
}

saludar("Ana", () => {
  console.log("Callback ejecutado después de saludar");
});
```

### Problema: Callback Hell

Cuando encadenas muchos callbacks anidados, el código se vuelve **difícil de leer**:

```javascript
loginUser("user", "pass", (user) => {
  getUserProfile(user, (profile) => {
    updateUI(profile, () => {
      console.log("Todo listo");
    });
  });
});
```

---

## 2. Promises

Una **Promise** es un **objeto** que representa el eventual **resultado** de una operación asíncrona.

Una Promise puede estar en 3 estados:
- **Pending** (pendiente)
- **Fulfilled** (cumplida)
- **Rejected** (rechazada)

### Crear una Promise

```javascript
const promesa = new Promise((resolve, reject) => {
  const exito = true;
  
  if (exito) {
    resolve("¡Operación exitosa!");
  } else {
    reject("Algo salió mal.");
  }
});

promesa
  .then((resultado) => console.log(resultado))
  .catch((error) => console.error(error));
```

---

## 3. Async/Await

**Async/Await** es **azúcar sintáctica** sobre Promises: hace que el código asíncrono **luzca síncrono**.

### Ejemplo simple

```javascript
function tarea() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Tarea completada"), 2000);
  });
}

async function ejecutar() {
  console.log("Inicio");
  const resultado = await tarea();
  console.log(resultado);
  console.log("Fin");
}

ejecutar();
```

**Salida:**
```
Inicio
(después de 2 segundos)
Tarea completada
Fin
```

---

# 🎯 Resumen Visual

| Herramienta | Ventajas | Desventajas |
|:---|:---|:---|
| Callbacks | Simple y directa | Callback Hell (difícil de leer) |
| Promises | Mejor organización, encadenamiento fácil | Manejo de errores más formal necesario |
| Async/Await | Código limpio y legible | Siempre trabaja sobre Promises |

---

# 🌟 Aplicaciones prácticas

- **Petición HTTP a APIs:**  
  (fetch, axios)

- **Lectura y escritura de archivos:**  
  (fs/promises en Node.js)

- **Animaciones temporizadas:**  
  (setTimeout, setInterval)

- **Procesos en segundo plano:**  
  (Carga de imágenes, pagos en línea, validaciones)

---

# 📚 JavaScript Asynchronous Practical Examples

---

## 1. Fetch básico (petición a API pública)

```javascript
// Hacer una petición HTTP usando fetch
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    if (!response.ok) {
      throw new Error('Error en la respuesta');
    }
    return response.json();
  })
  .then(data => {
    console.log("Datos recibidos:", data);
  })
  .catch(error => {
    console.error("Hubo un problema con la petición:", error);
  });
```

✅ **Aprendes a:**
- Hacer peticiones
- Convertir la respuesta a JSON
- Manejar errores

---

## 2. Varias peticiones al mismo tiempo con Promise.all

```javascript
// Hacer múltiples peticiones a la vez
const urls = [
  'https://jsonplaceholder.typicode.com/posts/1',
  'https://jsonplaceholder.typicode.com/posts/2',
  'https://jsonplaceholder.typicode.com/posts/3'
];

Promise.all(urls.map(url => fetch(url).then(res => res.json())))
  .then(results => {
    results.forEach((result, index) => {
      console.log(`Resultado ${index + 1}:`, result);
    });
  })
  .catch(error => {
    console.error("Error en alguna de las peticiones:", error);
  });
```

✅ **Aprendes a:**
- Ejecutar varias tareas asincrónicas juntas
- Esperar a que todas terminen
- Capturar errores si alguna falla

---

## 3. 🛠️ Mini proyecto: App de usuarios (cargar y mostrar datos + manejar errores)

**HTML base:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mini User App</title>
</head>
<body>
  <h1>Lista de Usuarios</h1>
  <button id="load-users">Cargar Usuarios</button>
  <ul id="user-list"></ul>

  <script src="app.js"></script>
</body>
</html>
```

**JavaScript (app.js):**

```javascript
const loadButton = document.getElementById('load-users');
const userList = document.getElementById('user-list');

loadButton.addEventListener('click', async () => {
  userList.innerHTML = "Cargando...";

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const users = await response.json();
    displayUsers(users);
  } catch (error) {
    userList.innerHTML = `<li style="color:red;">Error al cargar usuarios: ${error.message}</li>`;
  }
});

function displayUsers(users) {
  userList.innerHTML = ""; // Limpiar lista antes de mostrar
  users.forEach(user => {
    const li = document.createElement('li');
    li.textContent = `${user.name} (${user.email})`;
    userList.appendChild(li);
  });
}
```

✅ **Aprendes a:**
- Usar `async/await` para escribir código claro
- Manejar errores de red
- Actualizar el DOM dinámicamente
- Pensar como un desarrollador profesional

---

# 🎯 Buenas Prácticas

- **Siempre usa try/catch** al trabajar con `async/await`
- **Verifica `response.ok`** antes de procesar una respuesta HTTP
- **Muestra mensajes claros al usuario** si ocurre un error
- **Prefiere `async/await`** sobre `.then/.catch` si puedes — es más fácil de leer

---

# 📚 JavaScript - Custom Promises and Retry Mechanisms

---

## 1. Crear tu propia Promise personalizada

```javascript
// Función que devuelve una Promise que se resuelve después de X milisegundos
function esperar(ms) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(`Esperado ${ms} milisegundos`);
    }, ms);
  });
}

// Usando la Promise
esperar(2000).then(mensaje => console.log(mensaje));
```

✅ **Aprendes a:**
- Crear Promises manualmente
- Controlar cuándo se resuelven

---

### Promise que puede fallar o tener éxito

```javascript
function tareaConExito(probabilidadExito = 0.5) {
  return new Promise((resolve, reject) => {
    const random = Math.random();
    if (random < probabilidadExito) {
      resolve("Tarea completada exitosamente");
    } else {
      reject("Tarea fallida");
    }
  });
}

// Usarlo
tareaConExito(0.7)
  .then(mensaje => console.log(mensaje))
  .catch(error => console.error(error));
```

✅ **Aprendes a:**
- Simular operaciones exitosas o fallidas
- Trabajar con resolve/reject manualmente

---

## 2. Retry automático: intentar varias veces si falla

**Función de reintento (`retry`)**

```javascript
async function retry(fn, retries = 3, delayMs = 1000) {
  try {
    return await fn();
  } catch (error) {
    if (retries > 0) {
      console.warn(`Error: ${error}. Reintentando en ${delayMs}ms...`);
      await new Promise(res => setTimeout(res, delayMs));
      return retry(fn, retries - 1, delayMs);
    } else {
      throw new Error(`Falló después de varios intentos: ${error}`);
    }
  }
}
```

✅ **Qué hace:**
- Intenta ejecutar `fn()`
- Si falla, espera un tiempo y reintenta
- Si se acaban los intentos, lanza el error final

---

### Ejemplo práctico: Retry de petición a una API

```javascript
async function pedirUsuario() {
  const respuesta = await fetch('https://jsonplaceholder.typicode.com/users/1');
  
  if (!respuesta.ok) {
    throw new Error(`HTTP Error ${respuesta.status}`);
  }
  
  return respuesta.json();
}

(async () => {
  try {
    const usuario = await retry(pedirUsuario, 3, 2000); // 3 intentos, 2 segundos entre cada uno
    console.log("Usuario obtenido:", usuario);
  } catch (error) {
    console.error(error.message);
  }
})();
```

✅ **Aprendes a:**
- Hacer tu código **más resistente a fallos de red**
- Implementar **reintentos automáticos** como en aplicaciones profesionales

---

# 🎯 Buenas prácticas al usar Promises y Retry

- No hagas retries infinitos sin control: siempre limita los intentos
- Usa un **delay progresivo** (por ejemplo, 1s, 2s, 4s...) en proyectos más avanzados (estrategia de "backoff")
- Informa al usuario si el proceso falló tras varios intentos

