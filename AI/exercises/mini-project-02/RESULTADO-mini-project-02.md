# Mini-proyecto 2 — Resultado esperado (autocomprobación)

Úsalo para **verificar** que cumples los **pasos B y C** de la espina con el extra de dificultad (BD + historial para Ollama). No es una solución línea a línea.

**Rutas:** este `.md` está en `exercises/`; enlaces a la guía con **`../`**.

---

## 1. Criterios de aceptación

| # | Criterio |
|---|----------|
| 1 | **`PromptForm`** (o equivalente) valida longitud y rechaza vacíos solo espacios; los errores se ven en la plantilla. |
| 2 | Usas **`django.contrib.messages`** al menos para un caso de éxito y uno de error (p. ej. Ollama caído). |
| 3 | Existen modelos **`ChatSession`** y **`Message`** con migraciones aplicadas. |
| 4 | Cada envío válido **persiste** el mensaje del usuario y la respuesta del asistente en BD (orden temporal coherente). |
| 5 | La llamada a Ollama usa **`/api/chat`** con una lista **`messages`** construida desde BD (no solo el último `prompt` suelto), con un **límite** claro de cuántos mensajes o turnos envías (para no disparar VRAM/tokens). |
| 6 | **“Nueva conversación”** crea una nueva `ChatSession` y el usuario ve un hilo vacío o solo mensajes de la nueva sesión. |
| 7 | Tras **recargar** la página o **reiniciar** `runserver`, el historial sigue visible (datos en BD, no solo en memoria de sesión). |
| 8 | CSRF activo en los POSTs que modifican datos. |

---

## 2. Comportamiento que puedes probar

1. Varias preguntas en la misma conversación: la respuesta del modelo **tiene en cuenta** contexto reciente (según cuántos mensajes envíes en `messages`).
2. **Nueva conversación:** el hilo anterior no se mezcla con el nuevo (salvo que hayas diseñado una lista de chats; lo mínimo es separar por sesión).
3. Parar Ollama: mensaje de error amable + **sin** traceback al usuario.

---

## 3. Esquema de datos (referencia, no único diseño válido)

```text
ChatSession
  id, created_at [, title opcional]

Message
  id, session (FK), role, content, created_at
```

`role` puede ser `CharField` con choices o valores acotados.

---

## 4. Política de contexto (debes haberla documentado)

Ejemplos de decisión válida (elige una y aplícala):

- Últimos **10** mensajes de la sesión ordenados por `created_at`.
- Últimos **5** turnos usuario+asistente.
- Incluir un **`system`** fijo al inicio si lo usas.

---

## 5. Relación con la guía

- Pasos **B** y **C** de la [tabla checklist](../AI-STUDY-GUIDE.md#step-checklist-a-to-e).
- Modelos de chat: [appendix-django-integration §8](../appendix-django-integration.md#appendix-int-sec-8-chat-models).

---

*Si cumples la §1 y el comportamiento de la §2, el ejercicio está logrado.*

---

**Siguiente:** [EJERCICIO-mini-project-03.md](EJERCICIO-mini-project-03.md) · [RESULTADO-mini-project-03.md](RESULTADO-mini-project-03.md).
