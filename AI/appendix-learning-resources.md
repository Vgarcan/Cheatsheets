# Appendix — Learning resources

Curated **entry points** for expanding knowledge while staying aligned with **Django + HTTP inference** (Ollama). Add your own links as you go; prefer **official docs** first.

---

## Table of Contents

- [Django and Python web stack](#django-and-python-web-stack)
- [Ollama and local LLMs](#ollama-and-local-llms)
- [HTTP clients and async](#http-clients-and-async)
- [Background jobs](#background-jobs)
- [Databases and search](#databases-and-search)
- [ML / AI (broader, optional)](#ml--ai-broader-optional)
- [Security and operations](#security-and-operations)

---

## Django and Python web stack

- [Django documentation](https://docs.djangoproject.com/en/stable/) — especially **deployment**, **async**, **security**, **testing**.
- [Django REST framework](https://www.django-rest-framework.org/) — if your AI features are **JSON APIs** first.

---

## Ollama and local LLMs

- [Ollama](https://ollama.com/) — download, model library, blog.
- [Ollama GitHub](https://github.com/ollama/ollama) — issues, release notes, API discussions.

---

## HTTP clients and async

- [httpx](https://www.python-httpx.org/) — timeouts, sync/async, streaming.
- [Django async views](https://docs.djangoproject.com/en/stable/topics/async/) — when you are on ASGI.

---

## Background jobs

- [Celery](https://docs.celeryq.dev/en/stable/) or [RQ](https://python-rq.org/) — long-running inference without blocking web workers.

---

## Databases and search

- [PostgreSQL full-text search](https://www.postgresql.org/docs/current/textsearch.html) — simple RAG without vectors.
- [pgvector](https://github.com/pgvector/pgvector) — if you move to **embeddings** in Postgres.

---

## ML / AI (broader, optional)

- [Hugging Face Course](https://huggingface.co/learn/nlp-course/chapter1/1) — NLP concepts (not Django-specific).
- [PyTorch](https://pytorch.org/docs/stable/index.html) — only if you leave “HTTP-only” inference.

---

## Security and operations

- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — mindset for anything public-facing.
- [Mozilla Web Security](https://infosec.mozilla.org/guidelines/web_security) — practical headers and patterns.

---

*Keep this list short; deep reading belongs in the domain you are implementing next (e.g. RAG, embeddings, monitoring).*
