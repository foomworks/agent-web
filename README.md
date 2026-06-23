# agent-web

**URL → LLM-ready markdown.** A charter-clean, *polite, robots-respecting* web reader for AI
agents — honors `robots.txt`, identifies honestly, and **never** bypasses anti-bot/CAPTCHA or
paywalls. No account, no API key — free per-call access.

🌐 Live: **https://agent-web.foomworks.workers.dev** · 🔌 MCP: **`https://agent-web.foomworks.workers.dev/mcp`**

---

## What it gives an agent

Give it one publicly reachable URL; get back the page as clean, **LLM-ready markdown** —
title, word count, and the text — without parsing raw HTML yourself. Server-rendered (static)
HTML pages. Ideal for summarizing an article, extracting docs, or feeding a page into an LLM.

## Connect as an MCP server (recommended)

Streamable HTTP, JSON-RPC 2.0:

```
https://agent-web.foomworks.workers.dev/mcp
```

| Tool | Cost | Returns |
|---|---|---|
| `read_url` | free | fetch a URL → full LLM-ready markdown (title, word count, markdown) |
| `read_url_preview` | free | the first ~600 characters of the markdown — a cheap look before the full pull |

## Or call it over HTTP

```bash
BASE=https://agent-web.foomworks.workers.dev
curl -s "$BASE/read?url=https://example.com/"          # full markdown
curl -s "$BASE/read/preview?url=https://example.com/"  # first ~600 chars
```

Discovery: [`/.well-known/mcp.json`](https://agent-web.foomworks.workers.dev/.well-known/mcp.json) (MCP) ·
[`/.well-known/agent-card.json`](https://agent-web.foomworks.workers.dev/.well-known/agent-card.json) (A2A) ·
[`/openapi.json`](https://agent-web.foomworks.workers.dev/openapi.json) ·
[`/policy`](https://agent-web.foomworks.workers.dev/policy) (Acceptable-Use Policy)

## Charter-clean by design (the bright line)

This is a *polite* reader, not a scraper:

- Honors the origin's `robots.txt` for our user-agent — a disallowed path is **refused without fetching**; an unconfirmable `robots.txt` (5xx/error) is treated conservatively as disallowed.
- Identifies honestly with a descriptive user-agent — **never** spoofs a browser.
- Read-only `GET` of the **single** URL you supply — no crawl, no proxy rotation.
- **Never** bypasses anti-bot/CAPTCHA, paywalls, or login walls.
- SSRF-guarded: private/loopback/link-local/internal hosts are blocked and every redirect hop is re-validated.

You are responsible for your right to fetch the URL you submit.

## Limits

- **Static HTML only** for now — JavaScript-rendered pages, screenshots, and PDF are a later increment.
- Body cap ~2 MB, fetch timeout ~12 s.
- The service holds no keys and never initiates payments.

## Install the AgentSkill

[`SKILL.md`](./SKILL.md) is a portable AgentSkill (ClawHub / skills.sh / Claude Skills).

## About

agent-web is operated by **FOOM** — an AI-operated, human-supervised autonomous service.
License: [MIT](./LICENSE).
