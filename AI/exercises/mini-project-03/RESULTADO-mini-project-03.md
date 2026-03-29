# Mini-proyecto 3 — Resultado esperado (autocomprobación)

Comprueba **paso D** (cola) y **paso E** (streaming o nota técnica). No es código de referencia completo.

**Rutas:** este `.md` está en `exercises/`; enlaces con **`../`**.

---

## 1. Paso D — Criterios (todos sí)

| # | Criterio |
|---|----------|
| 1 | Tras enviar un prompt “largo” o simulado, la **respuesta HTTP inicial** del endpoint que encola vuelve en **poco tiempo** (orden de segundos o menos que una generación completa de Ollama), sin esperar a que termine el modelo. |
| 2 | Hay un **worker** separado (`rq worker`, `celery -A ... worker`, etc.) que procesa la cola y llama a tu servicio Ollama. |
| 3 | El estado del trabajo es **persistente** en BD (`pending` → `running` → `done` o `failed`). |
| 4 | La UI (o API) permite saber si el job **terminó** y muestra **resultado** o **error** entendible. |
| 5 | Un fallo de Ollama deja el registro en **`failed`** (o equivalente) con mensaje útil, no un zombie `running` eterno en el caso normal de excepción controlada. |

---

## 2. Paso E — Criterios (elige A o B)

### Vía A (streaming)

| # | Criterio |
|---|----------|
| 1 | Petición a Ollama con **`stream: true`**. |
| 2 | El cliente recibe **datos de forma incremental** (eventos SSE o chunks consumidos en JS), no solo un bloque al final en condiciones donde el streaming esté soportado. |
| 3 | Manejo básico de error (corte de conexión, respuesta no-200) sin romper el navegador. |

### Vía B (documentado)

| # | Criterio |
|---|----------|
| 1 | Archivo **`STREAMING-NOTES.md`** (o sección clara en README) con **qué probaste**, **qué observaste** (p. ej. buffering con `runserver`) y **qué stack probarías** después (ASGI, Daphne/Uvicorn, proxy). |
| 2 | La **Parte D** cumple todos los criterios de la tabla §1 sin atajos. |

---

## 3. Comportamiento que puedes probar

1. Encolar dos jobs seguidos: ambos **completan** (orden puede ser serial con un worker; es válido).
2. Parar Redis o el worker: los jobs no pasan a `done` sin intervención; la UI sigue coherente (sigue en `pending`/`running` o muestra mensaje de sistema caído si lo implementaste).
3. Streaming: escribe un prompt que genere varias frases y observa si el texto **crece** en pantalla.

---

## 4. Relación con la guía

- Filas **D** y **E** de la [tabla checklist](../AI-STUDY-GUIDE.md#step-checklist-a-to-e).
- [§5.3 What steps D and E mean](../AI-STUDY-GUIDE.md#guide-spine-step-de).

---

*Si cumples §1 y una de las vías de la §2, el mini-proyecto 3 está logrado.*
