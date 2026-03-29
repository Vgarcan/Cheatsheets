# Appendix — Django + AI in a real project

How to think about **structure, boundaries, and operations** when AI is not a demo but a **product feature**. Complements **[appendix-django-integration.md](appendix-django-integration.md)** (code recipes) and **[AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md)** (learning path).

---

## Table of Contents

- [1. Repository and layout](#1-repository-and-layout)
- [2. Settings and environments](#2-settings-and-environments)
- [3. App boundaries](#3-app-boundaries)
- [4. Service layer (where AI lives)](#4-service-layer-where-ai-lives)
- [5. Data you will probably model](#5-data-you-will-probably-model)
- [6. Sync vs async vs queues (project-level)](#6-sync-vs-async-vs-queues-project-level)
- [7. Uploads, documents, and RAG-shaped features](#7-uploads-documents-and-rag-shaped-features)
- [8. API design: who calls whom](#8-api-design-who-calls-whom)
- [9. Deployment topology](#9-deployment-topology)
- [10. Observability in one project](#10-observability-in-one-project)
- [11. Testing strategy](#11-testing-strategy)
- [12. Feature flags and rollout](#12-feature-flags-and-rollout)
- [13. Example directory tree](#13-example-directory-tree)

---

## 1. Repository and layout

- **One Django project** is enough for many products: keep **AI-specific code** in dedicated modules (`services/`, `tasks/`) so it is easy to grep and test.
- **Avoid** scattering `httpx.post(...)` across dozens of views—centralize in a small number of service functions (see integration appendix).
- If the codebase grows, split **by bounded context**: e.g. `documents` for ingestion, `chat` for conversations, both calling shared `ai/services/ollama.py`.

---

## 2. Settings and environments

| Concern | Practice |
|---------|----------|
| **Ollama URL** | `OLLAMA_BASE_URL` per environment (dev may use SSH tunnel; prod uses private IP or service name). |
| **Model default** | `OLLAMA_DEFAULT_MODEL` per env (dev can use a smaller/faster model). |
| **Timeouts** | Shorter in tests; production values tuned for UX and worker limits. |
| **Feature flags** | e.g. `AI_CHAT_ENABLED` to disable AI in staging without removing code. |

Use `django-environ` or your existing `.env` pattern; never commit real hostnames for production GPUs.

---

## 3. App boundaries

Typical split (names are examples):

| App | Responsibility |
|-----|----------------|
| **`users` / `accounts`** | Auth, profiles, **usage quotas** if you bill or limit AI. |
| **`ai` or `assistant`** | Prompts, chat UI, calls to `services/ollama.py`, maybe Webhooks. |
| **`documents`** (optional) | Uploads, chunking, indexing jobs, **retrieval** for RAG. |

**Rule:** views are thin; **domain rules** (“can this user run another generation?”) live in models/services, not in templates.

---

## 4. Service layer (where AI lives)

- **`ai/services/ollama.py`** — `chat()`, optional `stream()`, `embed()` later.
- **`ai/services/prompts.py`** — string templates and system prompts (versioned if you care about regressions).
- **`ai/tasks.py`** — Celery/RQ jobs that call services (never duplicate HTTP logic in tasks).

This makes it trivial to **swap** Ollama for another OpenAI-compatible endpoint in one place.

---

## 5. Data you will probably model

Even before full RAG:

- **`ChatSession`** / **`Message`** — history, auditing, “resume conversation.”
- **`AiUsage`** (optional) — user id, date, approximate tokens or request count for quotas and debugging.
- **`Document` / `Chunk`** (later) — file metadata, text chunks, embedding references.

Index foreign keys you query often (`user`, `session`, `created_at`).

---

## 6. Sync vs async vs queues (project-level)

| Pattern | When |
|---------|------|
| **Sync view** | MVP, internal tools, low traffic. |
| **Celery/RQ** | Generations regularly **> few seconds** or you must not block workers. |
| **Streaming** | Chat UX; validate buffering with your actual WSGI/ASGI stack. |

Document the **team decision** in `README` so new contributors do not add blocking calls to hot paths by accident.

---

## 7. Uploads, documents, and RAG-shaped features

- **Storage:** `MEDIA_ROOT` + S3-compatible backend in production; **virus scanning** policy if users upload arbitrary files.
- **Pipeline:** upload → validate → **async job** to chunk → store chunks → (optional) embed → query at question time.
- **Django’s role:** orchestration and **permission checks** (“user owns this document”); embedding batch jobs can stay in workers.

See [appendix-topics-beyond-the-spine.md](appendix-topics-beyond-the-spine.md) for embeddings depth.

---

## 8. API design: who calls whom

| Client | Typical pattern |
|--------|-----------------|
| **Browser** | Session auth, CSRF, HTML or HTMX; same-origin. |
| **Mobile / SPA** | DRF + token/JWT; **rate limits**; never expose Ollama URL to the client. |
| **Partner integration** | API keys, webhooks, separate throttle tiers. |

The **only** public AI surface should be **your** Django (or API gateway), not raw Ollama.

---

## 9. Deployment topology

**Common setups:**

1. **Django + Ollama on same private network** (LAN or VPC): low latency; `OLLAMA_BASE_URL=http://gpu-host:11434`.
2. **Django in PaaS, Ollama on your metal** — SSH tunnel or VPN; monitor **packet loss** and **latency**; timeouts may need tuning.
3. **Multiple web workers, one Ollama** — Ollama becomes a **shared resource**: watch GPU memory and queue depth; consider **request limits** at Django.

---

## 10. Observability in one project

- **Structured logs:** `request_id`, `user_id`, `model`, `latency_ms`, `status`.
- **Metrics:** p95 latency to Ollama, error rate, queue length.
- **Health:** `manage.py check_ollama` or internal health URL (see integration appendix).

Alert when errors spike after a **deploy** or **model** change.

---

## 11. Testing strategy

- **Unit tests:** mock `chat()` at the service boundary.
- **Integration tests:** optional subset hitting a **real** Ollama in CI (or nightly) if stable enough.
- **Smoke test after deploy:** health check + one canned prompt in staging.

Keep **golden prompts** (expected shape, not always exact text) if you care about regressions—see topics-beyond appendix for evals.

---

## 12. Feature flags and rollout

- Ship AI behind a **flag** (`AI_ENABLED`) or **per-group** (staff only → beta users → all).
- **Dark launch:** log would-be AI calls without showing output to users (rarely needed for small teams, useful at scale).

---

## 13. Example directory tree

Illustrative only—adapt names to your project.

```text
myproject/
  manage.py
  config/
    settings/
      base.py
      local.py
      production.py
    urls.py
    wsgi.py
  ai/
    services/
      ollama.py
      prompts.py
    tasks.py
    views.py
    urls.py
    models.py
  documents/          # optional RAG
    models.py
    tasks.py
  templates/ai/
  static/
  tests/
    ai/
      test_services.py
      test_views.py
```

---

*Extend this appendix with your real deployment names, queue backend, and on-call playbooks.*
