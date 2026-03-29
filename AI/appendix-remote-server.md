# Appendix — Remote Server & Access

**Goal:** Your **Django** app (dev laptop or production) calls **Ollama** on a **dedicated GPU server** safely. This appendix is about **connectivity and exposure**, not Django code (see [appendix-django-integration.md](appendix-django-integration.md)).

---

## Table of Contents

- [1. Network layout](#1-network-layout)
- [2. SSH basics](#2-ssh-basics)
- [3. Ollama bind address](#3-ollama-bind-address)
- [4. Firewall](#4-firewall)
- [5. Do not expose Ollama raw to the internet](#5-do-not-expose-ollama-raw-to-the-internet)
- [6. Reverse proxy + TLS (outline)](#6-reverse-proxy--tls-outline)
- [7. Jupyter / VS Code Remote (optional)](#7-jupyter--vs-code-remote-optional)
- [8. Django `OLLAMA_BASE_URL` examples](#8-django-ollama_base_url-examples)

---

## 1. Network layout

- **Same LAN:** Django uses `http://192.168.x.x:11434` (example).
- **VPN:** treat like LAN; use VPN-assigned IP or hostname.
- **SSH tunnel:** Django can use `http://127.0.0.1:11434` on the **client** while forwarding to the server (see below).

---

## 2. SSH basics

```bash
ssh user@your-server
```

**Key-based auth:** prefer SSH keys over passwords; disable password login in production.

**Port forwarding (local tunnel):** expose remote Ollama on your laptop’s localhost:

```bash
ssh -L 11434:127.0.0.1:11434 user@your-server
```

Then Django (on the laptop) can use `OLLAMA_BASE_URL=http://127.0.0.1:11434`. The tunnel encrypts traffic inside SSH.

---

## 3. Ollama bind address

Default is often **localhost only** (`127.0.0.1`), which is **safe** but unreachable from other machines.

To allow **LAN** access (example—verify with current Ollama docs):

- Set `OLLAMA_HOST=0.0.0.0:11434` (or equivalent) so the daemon listens on all interfaces.

**Only do this** if you also **firewall** so only trusted IPs reach the port.

---

## 4. Firewall

- **Linux (`ufw` example):** allow SSH; allow `11434` **only** from your Django host’s IP or VPN subnet.

```bash
sudo ufw allow OpenSSH
sudo ufw allow from 192.168.1.0/24 to any port 11434 proto tcp
sudo ufw enable
```

Adjust subnets and policies to your environment.

---

## 5. Do not expose Ollama raw to the internet

Ollama’s API is **not** a public auth layer. If you must reach inference from the internet:

- Put **Django** (or another API you control) behind **HTTPS + authentication**.
- **Do not** port-forward Ollama directly without VPN, IP allowlists, or mTLS.

---

## 6. Reverse proxy + TLS (outline)

Typical pattern:

- Nginx or Caddy on the server terminates TLS.
- Proxies `/ollama/` to `127.0.0.1:11434` with **auth** (basic auth, JWT, or internal network only).

Django should still prefer **private** network calls to Ollama (same VPC, Docker network) rather than looping through the public internet.

---

## 7. Jupyter / VS Code Remote (optional)

- **Jupyter:** bind to localhost + SSH tunnel, or run behind a reverse proxy with auth—**never** leave an open Jupyter port on `0.0.0.0` without protection.
- **VS Code Remote SSH:** edit code on the server; run Ollama and tests there; no need to expose Jupyter at all.

---

## 8. Django `OLLAMA_BASE_URL` examples

| Scenario | Value |
|----------|--------|
| Django on same machine as Ollama | `http://127.0.0.1:11434` |
| Django on LAN, Ollama on GPU box | `http://192.168.1.50:11434` |
| Django on laptop, SSH tunnel | `http://127.0.0.1:11434` (with `-L` active) |

---

*Keep this appendix updated with your actual firewall and VPN names.*
