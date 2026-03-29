# Mini-proyecto 2 — Formularios, mensajes y memoria en BD (espina B–C, un poco más exigente)

**Prerrequisito:** haber completado el **mini-proyecto 1** (paso A): servicio HTTP a Ollama, vista + plantilla, CSRF, errores controlados.

**Objetivo:** subir un escalón de dificultad — **paso B** (UX seria con `Form` y `messages`) y **paso C** con **persistencia en base de datos** (no solo sesión en memoria), enviando a Ollama **varios turnos** de conversación para que el modelo “recuerde” dentro de la misma charla.

**No abras `RESULTADO-mini-project-02.md`** hasta haber intentado el ejercicio o querer contrastar criterios.

**Rutas:** este `.md` está en `exercises/`; los enlaces usan **`../`**. Abre el workspace en la carpeta **`AI`**.

---

## Qué parte de la guía estás practicando

| Idea | Dónde leerla |
|------|--------------|
| Pasos **B** y **C** de la espina | [§5 Progressive integration](../AI-STUDY-GUIDE.md#5-progressive-django-integration-the-spine) · [Step checklist](../AI-STUDY-GUIDE.md#step-checklist-a-to-e) |
| Formularios, colas, modelos (idea) | [appendix-django-integration §4](../appendix-django-integration.md#appendix-int-sec-4-sync-view) · [§8](../appendix-django-integration.md#appendix-int-sec-8-chat-models) |
| Límites de contexto / VRAM | [appendix-hardware-gpu](../appendix-hardware-gpu.md) (no mandes miles de tokens sin criterio) |
| Proyecto real con apps y datos | [appendix-django-ai-project](../appendix-django-ai-project.md) (secciones de modelos y flujos) |

---

## En qué sube la dificultad (respecto al MP1)

1. **Validación en servidor** con `Form` (longitud máxima del prompt, mensajes de error por campo).
2. **`django.contrib.messages`** para feedback claro (éxito, error de inferencia, etc.).
3. **Modelos `ChatSession` + `Message`** en BD, migraciones, y **orden cronológico** de mensajes.
4. Construir la lista `messages` para **`/api/chat`** a partir de los **últimos N** mensajes guardados (usuario + asistente), no un solo prompt suelto.
5. **Botón “Nueva conversación”** que crea una nueva `ChatSession` y deja de mostrar la anterior hasta que el usuario elija otra (puedes usar **clave en `request.session`** para “la conversación activa” o un parámetro en la URL — tú decides, pero debe quedar claro en el flujo).

---

## Preguntas para pensar

1. Si guardas 50 mensajes en BD pero solo envías los **últimos 10** a Ollama, ¿qué “sabe” el modelo y qué queda solo en tu historial visual?
2. ¿Qué pasa si dos pestañas usan la misma “conversación activa” en sesión?
3. ¿Por qué limitar `max_length` del prompt en el `Form` además del timeout HTTP?

---

## Tareas (orden sugerido)

### 1) Modelo de datos

- Define **`ChatSession`** (al menos `created_at`; opcional `title` generado después del primer mensaje).
- Define **`Message`** con `ForeignKey` a `ChatSession`, campos `role` (`user` / `assistant` / `system` si lo usas), `content` (TextField), `created_at`.
- Ejecuta migraciones.

### 2) Formulario

- `PromptForm` con un campo de texto del usuario; **`max_length`** razonable (p. ej. 4000 caracteres o menos según tu plan; documenta el número en un comentario o en el README).
- `clean()` o validación para rechazar solo espacios en blanco.

### 3) Vista(s) y sesión “activa”

- Al enviar un mensaje válido: crea o reutiliza la `ChatSession` activa; guarda el mensaje **user**; llama a Ollama con `messages` = historial recortado (últimos N intercambios o últimos M mensajes — fija N o M y **documenta** la elección).
- Guarda la respuesta del asistente como **`Message`** con `role=assistant`.
- Si Ollama falla, **no** rompas la página: muestra mensaje al usuario (`messages.error`) y, si quieres, guarda o no el fallo — elige una política y sé consistente.

### 4) Plantilla

- Muestra **errores de campo** del formulario.
- Lista el hilo de la conversación activa (autor + texto).
- Incluye **CSRF** y botón **“Nueva conversación”** que crea sesión nueva y actualiza la sesión activa.

### 5) Criterio de calidad

- Recarga la página: el historial **sigue** (viene de BD).
- Reinicia el servidor Django **sin** borrar la BD: el historial **sigue** (no dependas solo de `request.session` para el texto de los mensajes).

---

## Qué no es obligatorio (pero puedes añadir)

- Login de usuario (todo anónimo o un solo usuario está bien).
- DRF, HTMX, streaming.
- Paginar conversaciones antiguas (lista de chats pasados).

---

## Siguiente paso

1. Contrasta con **[RESULTADO-mini-project-02.md](RESULTADO-mini-project-02.md)** (autocomprobación).
2. Cuando cumplas los criterios, pasa al **[EJERCICIO-mini-project-03.md](EJERCICIO-mini-project-03.md)** (D–E: colas, polling, streaming). Carpeta código: [mini-project-03](mini-project-03/).
