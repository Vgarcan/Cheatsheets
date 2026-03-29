# Mini-proyecto 1 — “Hola, Ollama” (Paso A de la espina)

**Objetivo:** Tener **un** flujo real: navegador → Django (POST) → **HTTP** a Ollama → respuesta visible en una página. Eso es el **paso A** de la guía.

**No leas el archivo `RESULTADO-mini-project-01.md` hasta que hayas intentado** implementar y probar por ti mismo (o hasta que te atasques del todo y quieras contrastar criterios).

**Rutas:** Este `.md` está en `exercises/`; los enlaces usan **`../`** para la carpeta **`AI/`**. Abre el workspace en **`AI`**.

---

## Qué parte de la guía estás practicando

Cada enlace debe abrir el fichero correcto con un clic (preview Markdown en Cursor/VS Code).

| Idea en la guía | Dónde leerla |
|-----------------|--------------|
| Por qué **HTTP** y no PyTorch en la vista | [§2.2 — Why HTTP](../AI-STUDY-GUIDE.md#guide-sec-22-why-http) |
| Quién usa la **GPU** (Ollama vs Django) | [§2.1 — Who holds the GPU?](../AI-STUDY-GUIDE.md#guide-sec-21-gpu) |
| Contrato con Ollama (`/api/chat`, base URL, timeouts) | [§3 — Ollama backend](../AI-STUDY-GUIDE.md#3-ollama-as-the-backend-developer-contract) · [§3.1 Endpoints](../AI-STUDY-GUIDE.md#guide-sec-31-http-endpoints) · [§3.3 Errores](../AI-STUDY-GUIDE.md#guide-sec-33-errors-timeouts) |
| Probar con **curl** antes de Django | [§3.2 Verify with curl](../AI-STUDY-GUIDE.md#guide-sec-32-curl) |
| **Nuevo proyecto** (venv, pip, startapp) | [§4.0 Local environment](../AI-STUDY-GUIDE.md#guide-sec-40-local-setup) |
| **`settings`** y variables de entorno | [§4.1 Settings](../AI-STUDY-GUIDE.md#guide-sec-41-settings) |
| App `ai` y capa de servicio | [§4.2 Optional `ai` app](../AI-STUDY-GUIDE.md#guide-sec-42-ai-app) |
| Paso **A** explicado | [§5.1 What step A means](../AI-STUDY-GUIDE.md#guide-spine-step-a) |
| Tabla espina A–E | [§5 Progressive integration](../AI-STUDY-GUIDE.md#5-progressive-django-integration-the-spine) |
| Criterio “done” del paso A | [Step checklist (A to E)](../AI-STUDY-GUIDE.md#step-checklist-a-to-e) |
| Cliente HTTP, vista sync, CSRF (detalle) | [Apéndice — §2 HTTP client](../appendix-django-integration.md#appendix-int-sec-2-http-client) · [§3 Service layer](../appendix-django-integration.md#appendix-int-sec-3-service) · [§4 Sync views](../appendix-django-integration.md#appendix-int-sec-4-sync-view) · [§14 Security](../appendix-django-integration.md#appendix-sec-14-security) |

Relaciona mentalmente: **vista** = punto de entrada HTTP; **servicio** = un solo sitio donde construyes la petición a Ollama; **plantilla** = lo que ve el usuario.

---

## Preguntas para pensar (antes de escribir código)

1. ¿Quién habla con la GPU: Django o Ollama? ¿Qué papel tiene Django entonces?
2. Si Ollama está en **otro servidor**, ¿qué valor tendrá `OLLAMA_BASE_URL` en tu máquina de desarrollo?
3. ¿Por qué tiene sentido un **timeout** en la llamada HTTP aunque “a veces vaya rápido”?
4. ¿Qué verías en el navegador si Ollama está apagado pero Django sigue vivo? ¿Debe el usuario ver un mensaje genérico o un traceback?

Escribe tus respuestas en un papel o nota (una o dos frases cada una). Te servirán al revisar `RESULTADO-mini-project-01.md`.

---

## Tareas (hazlas en orden)

### 0) Entorno

- Crea un **virtualenv** e instala Django y **`httpx`** (o `requests` si prefieres; la guía recomienda httpx). Pasos mínimos: [§4.0](../AI-STUDY-GUIDE.md#guide-sec-40-local-setup).
- Arranca **Ollama** y comprueba con `curl` que responde: [§3.2](../AI-STUDY-GUIDE.md#guide-sec-32-curl).

### 1) Proyecto Django mínimo

- `django-admin startproject` + `startapp` (nombre del app: por ejemplo `ai` o `demo`).
- En `settings`, define **`OLLAMA_BASE_URL`** y **`OLLAMA_DEFAULT_MODEL`** leyendo del entorno: [§4.1](../AI-STUDY-GUIDE.md#guide-sec-41-settings).

### 2) Capa de servicio (un solo módulo)

- Implementa una función del estilo `chat(messages) -> str` que haga **POST** a `{OLLAMA_BASE_URL}/api/chat` con `stream: false`.
- Centraliza **timeout** y manejo básico de error (aunque sea una excepción propia o un mensaje claro). Referencia: [apéndice §2](../appendix-django-integration.md#appendix-int-sec-2-http-client) y [§3](../appendix-django-integration.md#appendix-int-sec-3-service).

**Pista de diseño:** la vista no debería conocer la URL completa de Ollama; solo llamar al servicio.

### 3) Vista y plantilla

- Una **vista** que en GET muestre un formulario con **un campo de texto** (el prompt).
- En POST, si el formulario es válido, llama al servicio y muestra la **respuesta del modelo** en la misma página (o en un bloque debajo del formulario).
- Incluye **CSRF** en el formulario. Por qué: [§6 Product and safety](../AI-STUDY-GUIDE.md#6-product-and-safety-basics) (primer bullet) y [apéndice §14](../appendix-django-integration.md#appendix-sec-14-security).

### 4) URL

- Enlaza la vista en `urls.py` para que puedas abrirla en el navegador (por ejemplo `/` o `/chat/`).

### 5) Prueba manual

- Envía un prompt corto y comprueba que ves texto generado.
- Apaga Ollama o pon una URL incorrecta: tu app debe **fallar con gracia** (mensaje entendible, sin exponer detalles internos al usuario final). Idea: [§3.3](../AI-STUDY-GUIDE.md#guide-sec-33-errors-timeouts).

---

## Qué **no** hace falta en este mini-proyecto

- Celery, streaming, DRF, base de datos de historial, RAG, login (eso viene en pasos posteriores).
- Bonito CSS (opcional).

---

## Reflexión final (después de implementar)

1. ¿Qué capa cambiarías si mañana sustituyes Ollama por otro proveedor compatible con HTTP?
2. ¿Qué añadirías primero: validación más estricta del prompt (paso B) o guardar historial (paso C)? Justifica en una frase.

---

## Siguiente paso en la carpeta de ejercicios

1. Contrasta con **[RESULTADO-mini-project-01.md](RESULTADO-mini-project-01.md)** (autocomprobación).
2. Cuando cumplas los criterios, pasa al **[EJERCICIO-mini-project-02.md](EJERCICIO-mini-project-02.md)** (B–C: formularios, `messages`, BD, multi-turno). Carpeta opcional para código: [mini-project-02](mini-project-02/).
