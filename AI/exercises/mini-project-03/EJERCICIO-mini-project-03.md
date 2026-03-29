# Mini-proyecto 3 — Colas y streaming (espina D–E)

**Prerrequisito:** mini-proyecto 2 (o equivalente): chat con BD, servicio Ollama, formularios razonables.

**Objetivo:** dos saltos de producción típicos:

1. **Paso D** — La generación **no bloquea** el worker HTTP de Django: encolas el trabajo, respondes rápido al usuario y muestras **pendiente → listo** (o error).
2. **Paso E** — Ofreces **streaming** de tokens (SSE o respuesta en chunks) desde Ollama hacia el navegador, **o** documentas por qué en tu entorno lo dejas para más adelante (buffering con `runserver`, etc.).

**No abras `RESULTADO-mini-project-03.md`** hasta haber intentado el ejercicio o querer contrastar criterios.

**Rutas:** este `.md` está en `exercises/`; enlaces con **`../`**. Workspace: carpeta **`AI`**.

---

## Qué parte de la guía estás practicando

| Idea | Dónde leerla |
|------|--------------|
| Pasos **D** y **E** | [§5 Progressive integration](../AI-STUDY-GUIDE.md#5-progressive-django-integration-the-spine) · [Step checklist](../AI-STUDY-GUIDE.md#step-checklist-a-to-e) · [§5.3 What steps D and E mean](../AI-STUDY-GUIDE.md#guide-spine-step-de) |
| Colas (Celery / RQ) | [appendix-django-integration §6](../appendix-django-integration.md#appendix-int-sec-6-background-jobs) |
| Streaming / SSE | [appendix-django-integration §7](../appendix-django-integration.md#appendix-int-sec-7-streaming) |
| Timeouts y UX larga | [§3.3](../AI-STUDY-GUIDE.md#guide-sec-33-errors-timeouts) · [appendix-hardware-gpu](../appendix-hardware-gpu.md) |
| Proyecto con workers | [appendix-django-ai-project](../appendix-django-ai-project.md) (decisiones sync vs cola) |

---

## Subida de dificultad (respecto al MP2)

- Tienes que **levantar un proceso worker** además de `runserver` (Redis + **RQ** suele ser más corto que Celery; si ya dominas Celery, úsalo).
- Debes modelar **estado explícito** del trabajo (p. ej. `pending` / `running` / `done` / `failed`) en BD.
- El streaming obliga a pensar en **formato de Ollama** (`stream: true`, líneas NDJSON) y en **cómo** los empaquetas hacia el cliente (SSE mínimo).

---

## Preguntas para pensar

1. ¿Qué pasa si el usuario cierra la pestaña mientras el job sigue en cola?
2. ¿Por qué un worker aparte puede usar **más tiempo** de CPU/GPU sin castigar a otros usuarios que piden páginas estáticas?
3. Si `runserver` **bufferiza** toda la respuesta antes de enviarla, ¿significa que tu streaming “no funciona” o que el **stack de desarrollo** no es el adecuado para probarlo?

---

## Parte 1 — Paso D (obligatoria): cola + polling

### 1) Infra mínima

- **Redis** en marcha (local o Docker).
- Instala **django-rq** *o* **Celery + Redis** (elige uno; documenta cuál en un comentario o README del mini-proyecto).

### 2) Modelo de trabajo

- Algo equivalente a **`GenerationJob`**: `id`, `status`, `prompt` o FK a tu `Message`/`ChatSession`, `result_text` (nullable), `error_message` (nullable), `created_at`, `updated_at`.
- La vista que **antes** llamaba a Ollama de forma síncrona debe **encolar** una tarea y devolver **en segundos** una respuesta HTTP (redirect a detalle del job o JSON con `job_id`).

### 3) Tarea / task

- El worker ejecuta la llamada a Ollama (reutiliza tu **servicio** `chat(...)` del MP1/MP2).
- Actualiza el modelo al **empezar**, al **terminar bien** y al **fallar** (try/except; no dejes el job colgado en `running` para siempre si el proceso muere — un timeout en el job es un plus).

### 4) UX

- El usuario ve **estado pendiente** y puede **refrescar** o usar **polling** ligero (meta refresh, JavaScript `setInterval`, o HTMX si quieres) hasta `done` / `failed`.
- Mensaje claro si Ollama falla (igual filosofía que en MPs anteriores).

---

## Parte 2 — Paso E (elige una vía)

### Vía A — Streaming “de verdad”

- Endpoint (vista) que llama a Ollama con **`stream: true`**, lee el cuerpo en **streaming** con `httpx` (u otra API compatible) y devuelve **`StreamingHttpResponse`** con tipo **`text/event-stream`** (SSE mínimo) o chunks que tu cliente consuma con `fetch` + `ReadableStream`.
- Página o script mínimo que **añada** texto al DOM conforme llegan trozos.

### Vía B — Consciente y documentada

- Si tras probar concluyes que en **tu** stack el streaming no se aprecia (buffering), escribe **4–6 frases** en un `STREAMING-NOTES.md` (o en el README del MP3) explicando qué probaste y qué usarías en producción (ASGI, otro servidor, etc.). En ese caso, **completa la Parte 1 al máximo nivel** (cola robusta + estados + manejo de error) para compensar.

---

## Qué no es obligatorio

- Despliegue en producción real, HTTPS, Kubernetes.
- Autenticación por usuario en los jobs (puedes asumir un solo operador).
- Reintentos automáticos sofisticados o dead-letter queues.

---

## Siguiente paso

1. Contrasta con **[RESULTADO-mini-project-03.md](RESULTADO-mini-project-03.md)** (autocomprobación).
2. Con la espina A–E cerrada en práctica, sigue con [appendix-topics-beyond-the-spine](../appendix-topics-beyond-the-spine.md) (RAG, embeddings, observabilidad).
