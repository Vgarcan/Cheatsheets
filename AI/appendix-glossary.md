# Appendix — Glossary (Web Developer Angle)

Short definitions for terms used in **[AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md)**. Expand this file as you encounter new jargon.

---

## Table of Contents

- [A–E](#a--e)
- [F–L](#f--l)
- [M–R](#m--r)
- [S–Z](#s--z)

---

## A–E

| Term | Meaning |
|------|---------|
| **API** | Here: HTTP surface (Ollama `/api/chat`, etc.) your Django app calls. |
| **Chat completion** | Model returns an assistant message given a list of messages (roles + content). |
| **Context / context window** | Maximum tokens (prompt + completion) the model can attend to at once. |
| **Embedding** | Vector representing text (or image) for similarity search; often a separate API call. |
| **Eval (evaluation)** | Systematic checks that model outputs still meet quality after you change prompts/models. |
| **Inference** | Running a trained model to produce outputs (vs training). |

---

## F–L

| Term | Meaning |
|------|---------|
| **Fine-tuning** | Training a pre-trained model further on your data; heavier than “call Ollama.” |
| **GPU / VRAM** | Hardware where big matrix ops run; VRAM holds weights during inference. |
| **Hallucination** | Model outputs plausible but false text; mitigate with RAG, citations, and UI disclaimers. |
| **LLM** | Large language model; predicts text given a prompt. |
| **LoRA** | Low-rank adaptation: a smaller way to **fine-tune** a subset of weights; still mostly offline training. |
| **Agent (in apps)** | Pattern where the model proposes **steps** or **tool calls**; your code executes them after validation—not autonomous root access. |

---

## M–R

| Term | Meaning |
|------|---------|
| **Model** | In Ollama: a named artifact you `pull` (e.g. `llama3.2`) with a specific size/quant. |
| **Ollama** | Local runner that serves models over HTTP on port 11434 by default. |
| **Prompt** | Input text (or message list) you send to the model. |
| **Quantization** | Storing weights in fewer bits to save VRAM; may slightly reduce quality. |
| **RAG** | Retrieval-augmented generation: fetch relevant documents/chunks, then ask the LLM with that context. |

---

## S–Z

| Term | Meaning |
|------|---------|
| **SSE** | Server-Sent Events; one way to stream tokens to a browser (see Django appendix). |
| **Streaming** | Receiving output token-by-token (or chunk-by-chunk) instead of one final blob. |
| **System prompt** | Instruction message (role `system`) shaping behavior; optional but useful. |
| **Token** | Model’s text unit; billing, length limits, and speed are often expressed in tokens. |
| **Training** | Building/updating model weights from data; not required for basic Django+Ollama apps. |
| **Vector database** | Store optimized for **embedding** vectors and similarity queries (or use pgvector inside Postgres). |

---

*Add rows alphabetically or by section as needed.*
