# Appendix — Topics beyond the spine

After you complete **steps A–E** in [AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md), use this file to **choose** what to learn next. Everything stays in the **Django app builder** role: you orchestrate; heavy compute stays elsewhere.

**Related:** structuring a full codebase (apps, deploy, flags) is covered in **[appendix-django-ai-project.md](appendix-django-ai-project.md)**.

---

## Table of Contents

- [1. Embeddings and semantic search](#1-embeddings-and-semantic-search)
- [2. RAG beyond “paste chunks”](#topics-sec-2-rag-beyond)
- [3. Structured output and tools](#3-structured-output-and-tools)
- [4. Evaluation (evals)](#4-evaluation-evals)
- [5. Observability and cost](#5-observability-and-cost)
- [6. Fine-tuning and LoRA](#6-fine-tuning-and-lora)
- [7. Optional libraries](#7-optional-libraries)
- [8. Mental model for “what to learn next”](#topics-sec-8-mental-model)

---

## 1. Embeddings and semantic search

- **What:** Turn text into vectors; **similar** texts have **nearby** vectors in space.
- **Why:** Search by meaning (“find docs like this question”), not only keywords.
- **Django angle:** Store embeddings in **Postgres + pgvector**, or a dedicated vector DB; your views **query** then **pass top chunks** to Ollama (same RAG pattern as the main guide, with retrieval upgraded).

---

<a id="topics-sec-2-rag-beyond"></a>

## 2. RAG beyond “paste chunks”

- Chunking strategies (size, overlap), metadata filters, re-ranking, citing sources in the UI.
- **Django angle:** Models for `Document`, `Chunk`, `Citation`; admin for content; background tasks to re-embed when content changes.

---

## 3. Structured output and tools

- Asking the model for **JSON** (with validation) or **tool calls** (function names + arguments) for your app to execute safely **in code**—not free-form shell commands from the model.
- **Django angle:** Your **code** runs tools; the LLM only **proposes** structured actions you validate.

---

## 4. Evaluation (evals)

- Small **golden set** of questions with expected properties (correctness, format, refusal).
- **Why:** Track regressions when you change models or prompts.
- **Django angle:** Management command or CI job that calls Ollama and **asserts** metrics; optional dashboard.

---

## 5. Observability and cost

- Log **latency**, **token estimates** (if available), **model name**, **user id** (hashed if needed).
- Alerts when error rate or p95 latency spikes—same as any critical dependency.

---

## 6. Fine-tuning and LoRA

- Training **adapters** or small models on **your** data. Requires GPU time, data hygiene, and ML workflow—**not** required for most product features that only need a good prompt + RAG.
- **Django angle:** Still **HTTP** to a **served** model; training is offline.

---

## 7. Optional libraries

| Area | Notes |
|------|--------|
| **LangChain / LlamaIndex** | Abstractions for chains, agents, loaders—useful if complexity grows; avoid if you prefer plain Python services. |
| ** Instructor / Pydantic** | Structured JSON from LLMs with validation—pairs well with Django forms/serializers conceptually. |

Prefer **your own thin `services/` layer** until you feel real pain from boilerplate.

---

<a id="topics-sec-8-mental-model"></a>

## 8. Mental model for “what to learn next”

1. **Ship** one user-facing feature end-to-end (you already have the spine).
2. Add **one** capability that users ask for: usually **better retrieval** (embeddings) or **faster UX** (queues/streaming)—already partly in the django appendix.
3. Only then add **evals** or **fine-tuning** if the bottleneck is **model quality**, not product or data.

---

*Add subsections here as you explore (e.g. your first pgvector migration, your first eval harness).*
