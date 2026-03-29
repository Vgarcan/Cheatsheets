# Appendix — Django + Ollama Integration

Primary technical appendix for **[AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md)**. Recipes and expert notes: HTTP clients, settings, sync/async, queues, streaming, persistence, testing, production.

For **repo layout, environments, product rollout, and deployment topology**, see **[appendix-django-ai-project.md](appendix-django-ai-project.md)**.

---

## Table of Contents

- [1. Configuration](#1-configuration)
- [2. HTTP client choice](#appendix-int-sec-2-http-client)
- [3. Thin service layer](#appendix-int-sec-3-service)
- [4. Synchronous views (first milestone)](#appendix-int-sec-4-sync-view)
- [5. Async views (optional)](#5-async-views-optional)
- [6. Background jobs (Celery / RQ)](#appendix-int-sec-6-background-jobs)
- [7. Streaming (SSE-style)](#appendix-int-sec-7-streaming)
- [8. Data models for chat history](#appendix-int-sec-8-chat-models)
- [9. RAG-shaped patterns (minimal)](#9-rag-shaped-patterns-minimal)
- [10. Authentication and per-user limits](#10-authentication-and-per-user-limits)
- [11. Errors, timeouts, retries](#11-errors-timeouts-retries)
- [12. Testing (mocks)](#12-testing-mocks)
- [13. Production checklist](#13-production-checklist)
- [14. Security (CSRF, prompts, output)](#appendix-sec-14-security)
- [15. Health check (Ollama reachability)](#appendix-int-sec-15-health)

---

## 1. Configuration

**Environment variables** (never hardcode hostnames in code committed to git):

```bash
OLLAMA_BASE_URL=http://192.168.1.50:11434
OLLAMA_DEFAULT_MODEL=llama3.2
OLLAMA_REQUEST_TIMEOUT_S=120
```

**`settings.py`:**

```python
import os

OLLAMA_BASE_URL = os.environ.get("OLLAMA_BASE_URL", "http://127.0.0.1:11434").rstrip("/")
OLLAMA_DEFAULT_MODEL = os.environ.get("OLLAMA_DEFAULT_MODEL", "llama3.2")
OLLAMA_REQUEST_TIMEOUT_S = int(os.environ.get("OLLAMA_REQUEST_TIMEOUT_S", "120"))
```

**Trailing slash:** Ollama paths are like `/api/chat`; store base URL **without** trailing slash and concatenate explicitly to avoid double slashes.

---

<a id="appendix-int-sec-2-http-client"></a>

## 2. HTTP client choice

| Library | Use when |
|---------|----------|
| **`httpx`** | Sync or async; good timeouts; easy `json=` payloads. Prefer for new code. |
| **`requests`** | Ubiquitous; fine for sync-only MVP. |
| **`urllib`** | No extra dependency; verbose. |

Example sync call with **httpx**:

```python
import httpx
from django.conf import settings

def ollama_chat(messages: list[dict], model: str | None = None) -> str:
    model = model or settings.OLLAMA_DEFAULT_MODEL
    url = f"{settings.OLLAMA_BASE_URL}/api/chat"
    payload = {"model": model, "messages": messages, "stream": False}
    with httpx.Client(timeout=settings.OLLAMA_REQUEST_TIMEOUT_S) as client:
        r = client.post(url, json=payload)
        r.raise_for_status()
        data = r.json()
    return (data.get("message") or {}).get("content") or ""
```

---

<a id="appendix-int-sec-3-service"></a>

## 3. Thin service layer

Keep views dumb; put Ollama calls in **`services/ollama.py`** (or `ai/ollama_client.py`).

**Benefits:** one place for timeouts, logging, and future swap to another provider (OpenAI-compatible API, etc.).

```python
# services/ollama.py
import logging
import httpx
from django.conf import settings

logger = logging.getLogger(__name__)

class OllamaError(Exception):
    pass

def chat(messages: list[dict], *, model: str | None = None) -> str:
    model = model or settings.OLLAMA_DEFAULT_MODEL
    url = f"{settings.OLLAMA_BASE_URL}/api/chat"
    try:
        with httpx.Client(timeout=settings.OLLAMA_REQUEST_TIMEOUT_S) as client:
            r = client.post(url, json={"model": model, "messages": messages, "stream": False})
            r.raise_for_status()
            data = r.json()
        text = (data.get("message") or {}).get("content")
        if not text:
            raise OllamaError("Empty response from Ollama")
        return text
    except httpx.HTTPError as e:
        logger.exception("Ollama HTTP error")
        raise OllamaError("Inference service unavailable") from e
```

---

<a id="appendix-int-sec-4-sync-view"></a>

## 4. Synchronous views (first milestone)

```python
# views.py
from django.shortcuts import render
from django.views import View
from .forms import PromptForm
from .services.ollama import chat, OllamaError

class ChatDemoView(View):
    template_name = "ai/chat_demo.html"

    def get(self, request):
        return render(request, self.template_name, {"form": PromptForm(), "reply": None})

    def post(self, request):
        form = PromptForm(request.POST)
        reply = None
        if form.is_valid():
            prompt = form.cleaned_data["prompt"]
            try:
                reply = chat([{"role": "user", "content": prompt}])
            except OllamaError:
                reply = "Sorry, the AI service is unavailable. Try again later."
        return render(request, self.template_name, {"form": form, "reply": reply})
```

**Do not** block forever: `OLLAMA_REQUEST_TIMEOUT_S` is mandatory.

---

## 5. Async views (optional)

Use **async views** only if your Django stack supports ASGI and you use **`httpx.AsyncClient`**. Same rules: timeouts, error mapping.

```python
import httpx
from django.conf import settings
from django.http import JsonResponse
from django.views import View

class AsyncPingView(View):
    async def get(self, request):
        url = f"{settings.OLLAMA_BASE_URL}/api/tags"
        async with httpx.AsyncClient(timeout=10.0) as client:
            r = await client.get(url)
        return JsonResponse(r.json())
```

If you are on **WSGI + sync views only**, use threads or **background queues** for long calls instead of pretending to be async.

---

<a id="appendix-int-sec-6-background-jobs"></a>

## 6. Background jobs (Celery / RQ)

**When:** generation regularly exceeds **a few seconds** and you use **sync workers** (Gunicorn sync, etc.). Offload to Celery or RQ so HTTP workers stay free.

**Pattern:**

1. View creates a `GenerationJob` row with status `pending`, enqueues task with `job_id`.
2. Client polls `GET /api/jobs/<id>/` or uses HTMX refresh until `done` / `failed`.
3. Task calls `chat(...)` and saves result on the model.

**Celery** sketch:

```python
# tasks.py
from celery import shared_task
from .models import GenerationJob
from .services.ollama import chat

@shared_task
def run_generation(job_id: int):
    job = GenerationJob.objects.get(pk=job_id)
    job.status = "running"
    job.save(update_fields=["status"])
    try:
        text = chat(job.messages_payload)
        job.result_text = text
        job.status = "done"
    except Exception:
        job.status = "failed"
    job.save()
```

---

<a id="appendix-int-sec-7-streaming"></a>

## 7. Streaming (SSE-style)

Ollama supports **`"stream": true`** and returns **newline-delimited JSON** chunks. Django can stream with **`StreamingHttpResponse`**.

**Caveats:**

- Many hosted setups buffer responses; test with your server (Gunicorn `--worker-class gevent` or ASGI).
- You must parse Ollama’s stream format in a generator.

Conceptual pattern:

```python
from django.http import StreamingHttpResponse
import httpx
from django.conf import settings

def stream_chat(request):
    def event_stream():
        payload = {"model": settings.OLLAMA_DEFAULT_MODEL, "messages": [...], "stream": True}
        with httpx.Client(timeout=None) as client:
            with client.stream("POST", f"{settings.OLLAMA_BASE_URL}/api/chat", json=payload) as r:
                r.raise_for_status()
                for line in r.iter_lines():
                    if line:
                        yield f"data: {line}\n\n"
    return StreamingHttpResponse(event_stream(), content_type="text/event-stream")
```

Adjust headers (`Cache-Control: no-cache`) and CORS if the frontend is on another origin. For production, prefer **async ASGI** + proper SSE libraries if you hit buffering issues.

---

<a id="appendix-int-sec-8-chat-models"></a>

## 8. Data models for chat history

Minimal sketch:

```python
from django.db import models
from django.conf import settings

class ChatSession(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True, on_delete=models.CASCADE)
    title = models.CharField(max_length=200, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)

class Message(models.Model):
    session = models.ForeignKey(ChatSession, related_name="messages", on_delete=models.CASCADE)
    role = models.CharField(max_length=16)  # user, assistant, system
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
```

Build `messages` for Ollama from the last **N** messages (cap context length). See hardware appendix for token limits.

---

## 9. RAG-shaped patterns (minimal)

1. User question → **retrieve** relevant chunks (full-text search in PostgreSQL, or embeddings + pgvector later).
2. Build prompt: system instruction + **quoted chunks** + user question.
3. Call `chat(...)`.

Sanitize and **cite** chunk sources in the UI so users trust answers. This appendix stays minimal; expand when you add a vector store.

---

## 10. Authentication and per-user limits

- **LoginRequiredMixin** / DRF `IsAuthenticated` on AI endpoints.
- **Throttle** with DRF `DEFAULT_THROTTLE_RATES` or `django-ratelimit`.
- **Per-user quotas:** store token counts or request counts per day in DB or cache.

---

## 11. Errors, timeouts, retries

- **Map** `httpx.HTTPError` to a **single** user-facing message; log stack traces server-side.
- **Retries:** use sparingly for **idempotent** health checks; for chat POSTs, retries can **duplicate** side effects—prefer **one** try unless you have request ids.
- **Circuit breaker:** if Ollama fails N times, short-circuit and show maintenance message (optional pattern).

---

## 12. Testing (mocks)

**Do not** call real Ollama in unit tests by default.

```python
from unittest.mock import patch

@patch("myapp.services.ollama.chat", return_value="mocked reply")
def test_chat_view(self, mock_chat):
    response = self.client.post("/ai/demo/", {"prompt": "hi"})
    self.assertContains(response, "mocked reply")
```

Integration tests against a **real** Ollama can run in CI only if you provision a runner with Ollama (often skipped).

---

## 13. Production checklist

- [ ] `OLLAMA_BASE_URL` points to a **reachable** address from Django (same host private IP, Docker network name, etc.).
- [ ] Ollama **not** exposed publicly without TLS and auth.
- [ ] Timeouts set on every client call.
- [ ] Logging without leaking full PII prompts (policy-dependent).
- [ ] Rate limits on AI endpoints.
- [ ] Background workers for long jobs if using sync Gunicorn.
- [ ] Health check endpoint or management command that pings Ollama `/api/tags` (see [§15](#appendix-int-sec-15-health)).

---

<a id="appendix-sec-14-security"></a>

## 14. Security (CSRF, prompts, output)

### CSRF and forms

- **Template forms:** ensure `{% csrf_token %}` inside `<form>` and use Django’s `POST` handling so cross-site request forgery is blocked.
- **DRF:** use session auth + CSRF for browser clients, or token auth for APIs—do not expose a **state-changing** AI endpoint as anonymous `POST` without throttling and abuse controls.

### Prompt injection (application-level)

- Users will try to override system instructions (“ignore previous…”). Mitigations: **fixed system prompts** server-side only; **strip or limit** role fields if you expose structured chat; never **execute** model output as code or SQL.
- Treat model text as **untrusted** when rendering in HTML: use Django **auto-escaping**; avoid `|safe` on model output unless you fully control sanitization.

### Secrets and logging

- Do not send API keys to the browser. Ollama on a private network still deserves **network controls** (see remote appendix).
- Log **metadata** (latency, model, status code); avoid logging full prompts in production if they contain PII.

---

<a id="appendix-int-sec-15-health"></a>

## 15. Health check (Ollama reachability)

Use this in **deploy scripts**, **load balancer** custom checks, or **uptime** monitors.

### Option A — management command

`myapp/management/commands/check_ollama.py`:

```python
from django.core.management.base import BaseCommand
from django.conf import settings
import httpx

class Command(BaseCommand):
    help = "Verify Ollama is reachable (GET /api/tags)."

    def handle(self, *args, **options):
        url = f"{settings.OLLAMA_BASE_URL}/api/tags"
        try:
            r = httpx.get(url, timeout=10.0)
            r.raise_for_status()
        except Exception as e:
            self.stderr.write(self.style.ERROR(f"Ollama unreachable: {e}"))
            raise SystemExit(1)
        self.stdout.write(self.style.SUCCESS("Ollama OK"))
```

Run: `python manage.py check_ollama`

### Option B — internal URL (protect!)

A small **staff-only** or **IP-restricted** view that returns `200` if `/api/tags` succeeds and `503` otherwise. **Do not** expose this on the public internet without authentication.

---

*Add your own project-specific tricks here as the stack grows.*
