# Appendix — Hardware & GPU (12GB VRAM)

Companion to **[AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md)**. Rules of thumb for **Ollama** on a **12GB** consumer/workstation GPU—not exact benchmarks (those change with drivers and model builds).

---

## Table of Contents

- [1. VRAM vs system RAM](#1-vram-vs-system-ram)
- [2. What 12GB is good for](#2-what-12gb-is-good-for)
- [3. Model sizes and Ollama](#3-model-sizes-and-ollama)
- [4. Quantization (plain language)](#4-quantization-plain-language)
- [5. Context length and speed](#5-context-length-and-speed)
- [6. Red lines (when to stop expecting local magic)](#6-red-lines-when-to-stop-expecting-local-magic)
- [7. What to tell Django users](#7-what-to-tell-django-users)

---

## 1. VRAM vs system RAM

- **VRAM** on the GPU holds model weights and activations during inference. If the model does not fit, Ollama may offload parts to **system RAM** (slower) or fail.
- **12GB VRAM** is a solid **local inference** budget for small/medium open models in **quantized** form. It is **not** a training cluster for frontier models.

---

## 2. What 12GB is good for

- Running **7B–8B** class models in **Q4** quantization comfortably for many workloads.
- Running **smaller** models (1B–3B) very fast for classification, short answers, or high throughput.
- **Experimenting** with 13B+ only when heavily quantized and with shorter context; may be tight or spill to RAM.

---

## 3. Model sizes and Ollama

Ollama names models by upstream family (e.g. `llama3.2`, `mistral`, `phi3`). The **tag** often implies size (e.g. `:3b`, `:7b`, `:8b`). Always check `ollama show <model>` on your server for **parameter count** and **quantization**.

**Practical approach:**

1. Pull a **known-small** model and verify `nvidia-smi` during a long generation.
2. Increase size until latency or OOM (out-of-memory) pushes you back down.

---

## 4. Quantization (plain language)

- Full-precision weights use more VRAM; **quantization** stores weights in fewer bits (e.g. 4-bit, **Q4**).
- **Tradeoff:** slightly lower quality vs much smaller memory. For web-app chat, **Q4** is often the sweet spot on 12GB.

---

## 5. Context length and speed

- **Longer prompts** (big RAG dumps, pasted documents) increase memory and time.
- In Django, enforce **max prompt length** and **max retrieved chunks** so a single request does not accidentally blow VRAM or stall workers.

---

## 6. Red lines (when to stop expecting local magic)

| Expectation | Reality on 12GB + Ollama |
|-------------|-------------------------|
| Run the largest flagship models at full context | Usually **no**; use cloud APIs or bigger hardware. |
| Fine-tune a 70B model locally | **No** for typical setups. |
| Train foundation models | **Out of scope** for this hardware. |
| Low-latency streaming for huge contexts | May need **smaller models** or **shorter** context. |

---

## 7. What to tell Django users

- Show **progress** (spinner, job queue) for long generations.
- Return **friendly errors** when the inference service is overloaded or unavailable.
- Optionally display **“model is loading”** if Ollama cold-starts a model (first request after idle).

---

*Revisit when you upgrade GPU or move inference to a dedicated multi-GPU box.*
