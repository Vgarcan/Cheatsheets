# Mini-proyecto 1 — Resultado esperado (autocomprobación)

Úsalo para **verificar** que tu trabajo cumple el **paso A** de la espina. Si algo falla, vuelve a la guía y al apéndice de integración antes de copiar nada de aquí.

**Norma:** esto describe **comportamiento y estructura**, no sustituye tu propio código letra por letra.

**Rutas:** Este `.md` está en `exercises/`; enlaces a la guía usan **`../`** hasta la carpeta **`AI/`**.

---

## 1. Criterios de aceptación (todos sí)

| # | Criterio |
|---|----------|
| 1 | Con Ollama **encendido** y modelo disponible, envías un prompt desde el navegador y ves **texto generado** en la respuesta HTML. |
| 2 | `settings` obtiene **`OLLAMA_BASE_URL`** y **`OLLAMA_DEFAULT_MODEL`** (u equivalente) desde variables de entorno o `.env`, sin credenciales inventadas en el repo. |
| 3 | La llamada a Ollama está **centralizada** (p. ej. `services/ollama.py` o similar), no repetida en varias vistas. |
| 4 | La petición HTTP al backend de inferencia usa **timeout** explícito (no espera infinita). |
| 5 | Si Ollama **no** responde (apagado, URL mala), la página muestra un **mensaje controlado** al usuario, no un traceback en producción. |
| 6 | El formulario POST incluye protección **CSRF** estándar de Django. |

---

## 2. Comportamiento que puedes probar

1. **Feliz:** `curl` a Ollama funciona desde tu PC; la web también devuelve texto.
2. **Triste:** detén Ollama o cambia temporalmente `OLLAMA_BASE_URL` a un puerto vacío → la app Django sigue arrancando; la vista devuelve error amable.

---

## 3. Estructura de archivos (referencia mínima)

No es obligatorio que los nombres coincidan al 100 %; la idea es **separación clara**.

```text
tu_proyecto/
  manage.py
  config/ o nombre_del_proyecto/
    settings.py
    urls.py
  ai/   (o el nombre de tu app)
    services/
      ollama.py    # chat(), timeout, errores
    views.py
    forms.py       # opcional: puedes usar Form mínimo aquí o en el mismo archivo al principio
    templates/
      ai/...
```

La **url** raíz o `/chat/` apunta a tu vista.

---

## 4. Contrato HTTP que debes respetar

- **POST** a `{OLLAMA_BASE_URL}/api/chat` con cuerpo JSON al menos: `model`, `messages`, `"stream": false`.
- Respuesta: parseas el JSON y extraes el contenido del mensaje del asistente (forma actual de Ollama: objeto con `message.content`; si tu versión difiere, ajusta según [appendix-tooling-stack — HTTP API](../appendix-tooling-stack.md#appendix-tool-sec-2-http-api)).

---

## 5. Errores comunes (checklist rápido)

| Síntoma | Qué revisar |
|---------|-------------|
| `Connection refused` | Ollama no corre, puerto, firewall, o `OLLAMA_BASE_URL` mal en el entorno donde ejecutas `runserver`. |
| 404 desde Ollama | Nombre de **modelo** no instalado (`ollama pull ...`). |
| CSRF 403 | Falta `{% csrf_token %}` o estás haciendo POST sin cookie de sesión. |
| Cuelgue eterno | Falta **timeout** en el cliente HTTP. |

---

## 6. Relación con la guía (check mental)

- Paso **A** cumplido → puedes marcar la primera fila de la [tabla checklist](../AI-STUDY-GUIDE.md#step-checklist-a-to-e) y revisar [§5.1 What step A means](../AI-STUDY-GUIDE.md#guide-spine-step-a).
- Siguiente ampliación natural: **[EJERCICIO-mini-project-02.md](EJERCICIO-mini-project-02.md)** (pasos B–C); carpeta código: **[mini-project-02](mini-project-02/)**.

---

*Si tu implementación cumple la tabla de la §1 y el comportamiento de la §2, el ejercicio está logrado aunque el código difiera en detalles de estilo.*
