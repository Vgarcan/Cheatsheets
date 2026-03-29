# Appendix — Tooling Stack (Ollama-first)

**Default path:** **Ollama** CLI + HTTP API on your dedicated server. **Advanced:** PyTorch, Hugging Face, Docker + NVIDIA—only when you outgrow “Django calls HTTP.”

---

## Table of Contents

- [1. Ollama CLI essentials](#1-ollama-cli-essentials)
- [2. HTTP API essentials](#appendix-tool-sec-2-http-api)
- [3. Environment variables](#3-environment-variables)
- [4. Troubleshooting](#4-troubleshooting)
- [5. Advanced track (optional)](#5-advanced-track-optional)
- [6. API request options (context, keep-alive)](#6-api-request-options-context-keep-alive)

---

## 1. Ollama CLI essentials

```bash
ollama --version
ollama list
ollama pull llama3.2
ollama run llama3.2 "Hello in one sentence."
```

```bash
ollama ps
```

Shows running models.

```bash
ollama show llama3.2
```

Displays parameters, architecture, and capabilities.

---

<a id="appendix-tool-sec-2-http-api"></a>

## 2. HTTP API essentials

**Base URL:** `http://HOST:11434` (default port **11434**).

| Endpoint | Purpose |
|----------|---------|
| `GET /api/tags` | List local models |
| `POST /api/generate` | Single prompt generation |
| `POST /api/chat` | Chat-style messages |
| `POST /api/embeddings` | Embeddings (when you need semantic search) |

**Chat (non-streaming) example:**

```bash
curl -s http://127.0.0.1:11434/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3.2",
    "messages": [{"role": "user", "content": "Explain Django MVT in one paragraph."}],
    "stream": false
  }'
```

**Streaming:** set `"stream": true`; response is **NDJSON** (one JSON object per line). Parse line-by-line in Python or forward to SSE—see [appendix-django-integration.md](appendix-django-integration.md).

**Generate (single prompt):**

```bash
curl -s http://127.0.0.1:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model":"llama3.2","prompt":"Why use timeouts in HTTP clients?","stream":false}'
```

---

## 3. Environment variables

Useful on the **server** (Ollama reads some of these; see `ollama --help` / upstream docs for your version):

| Variable | Typical use |
|----------|-------------|
| `OLLAMA_HOST` | Bind address (e.g. `0.0.0.0:11434` to listen on all interfaces—**firewall carefully**) |
| `OLLAMA_MODELS` | Model storage location |

On **Django**, use `OLLAMA_BASE_URL` in your app (see main guide).

---

## 4. Troubleshooting

| Symptom | Checks |
|---------|--------|
| Connection refused | Is Ollama running? Correct `OLLAMA_HOST` / port? Firewall? |
| Slow first request | Model **loading** into VRAM; normal after idle. |
| OOM / crash | Model too large for VRAM; try smaller model or more aggressive [quantization](appendix-hardware-gpu.md). |
| Empty response | Wrong model name; inspect JSON error in response body. |

```bash
nvidia-smi
```

Watch GPU memory during a request.

---

## 5. Advanced track (optional)

Use when you need **custom training**, **non-Ollama** inference servers, or **on-GPU** batch jobs:

- **PyTorch** + **CUDA**: install matching versions from official docs.
- **Hugging Face Transformers**: load models in Python, not only via Ollama.
- **Docker + NVIDIA Container Toolkit**: run GPU containers; useful for reproducible training/inference environments.

**Rule:** Keep **Django** talking to **HTTP** (Ollama or another server). Do not import heavy ML stacks into every Django process unless you deliberately colocate and understand memory implications.

---

## 6. API request options (context, keep-alive)

Ollama’s JSON body often supports (names may vary slightly by version—check [Ollama API docs](https://github.com/ollama/ollama/blob/main/docs/api.md)):

| Option | Purpose |
|--------|---------|
| `num_ctx` | Context window size (larger → more VRAM and slower). |
| `keep_alive` | How long to keep the model loaded after a request (reduces **cold start** if set appropriately). |

Example idea for `/api/chat` payload:

```json
{
  "model": "llama3.2",
  "messages": [{"role": "user", "content": "Hi"}],
  "stream": false,
  "options": {
    "num_ctx": 4096
  }
}
```

Use **smaller** `num_ctx` when you can to save VRAM on 12GB hardware. See [appendix-hardware-gpu.md](appendix-hardware-gpu.md).

---

*Extend this file with version-specific Ollama flags as your deployment stabilizes.*
