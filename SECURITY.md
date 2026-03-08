# Security Policy

> This document outlines the security policy for **RecommendMe** — an AI-powered conversational product recommendation engine. It covers supported versions, vulnerability reporting, scope, and our internal security practices.

---

## Supported Versions

Only the latest version of RecommendMe (tracked on the `main` branch) is actively maintained and receives security patches.

| Version        | Supported          |
|----------------|--------------------|
| Latest (`main`) | ✅ Active           |
| Feature branches | ⚠️ Best-effort     |
| Older / archived branches | ❌ Not supported |

---

## Reporting a Vulnerability

> **Please do NOT open a public GitHub issue for security vulnerabilities.**
> Public disclosure before a fix is available puts all users at risk.

If you discover any security issue — including but not limited to exposed API keys, prompt injection risks, input validation bypasses, data leaks, or insecure API behaviour — please report it privately via the channel below.

**📧 Email:** `teamlead@gmail.com` *(or your Team Lead's address)*

### What to Include in Your Report

To help us triage quickly, please provide as much of the following as possible:

- **Description** — A clear summary of the vulnerability and what it affects
- **Steps to Reproduce** — Detailed, minimal steps to trigger the issue
- **Impact Assessment** — What could an attacker achieve? (data exposure, system compromise, etc.)
- **Environment** — Version, endpoint, or component affected
- **Suggested Fix** — Optional, but always appreciated

### Our Response Commitment

| Milestone | Target Timeframe |
|-----------|-----------------|
| Initial acknowledgement | Within **48 hours** |
| Severity assessment & triage | Within **3 business days** |
| Fix for Critical / High severity | Within **7 days** |
| Fix for Medium severity | Within **14 days** |
| Fix for Low severity | Within **30 days** |

We follow responsible disclosure practices. If you'd like, we'll credit you by name (or handle) in the fix commit and release notes.

---

## Scope

### ✅ In Scope

The following are part of the RecommendMe codebase and are valid targets for security research:

| Area | Examples |
|------|----------|
| **Backend API** | FastAPI endpoints (`POST /query`, `GET /health`), request validation, rate limiting logic |
| **AI Layer** | Prompt injection or manipulation via user inputs, context poisoning across conversation turns |
| **API Key Handling** | Leaked or hardcoded keys in source, logs, or error responses |
| **Input Handling** | HTML/script injection in user messages, oversized payloads bypassing character limits |
| **Session Management** | Session fixation, session ID guessing, expired session handling |
| **CORS & Transport** | Misconfigured CORS that allows unauthorised origins, HTTP downgrade attacks |
| **Rate Limiting** | Bypasses to `POST /api/query` rate limit (10 req/min/IP) |
| **Environment Config** | `.env` exposure, secrets in version control or build artifacts |

### ❌ Out of Scope

The following are **not** part of the RecommendMe codebase. Please report issues directly to the respective providers:

| Service | Report To |
|---------|-----------|
| OpenAI (GPT-4o / GPT-4o-mini) | [openai.com/security](https://openai.com/security) |
| SerpAPI | [serpapi.com](https://serpapi.com) |
| Ollama | [github.com/ollama/ollama](https://github.com/ollama/ollama) |
| Vercel / Railway / Render (hosting) | Their respective security teams |

Also out of scope:
- Bugs with no security impact — please open a regular issue instead
- Theoretical vulnerabilities without a reproducible proof-of-concept
- Social engineering attacks targeting team members
- Denial-of-service attacks against production infrastructure

---

## Security Architecture Overview

For context, here is a summary of the security controls currently implemented in RecommendMe. This is intended to help researchers understand the existing posture.

### API Key Management
- All keys (`OPENAI_API_KEY`, `SERPAPI_KEY`) are stored in a `.env` file loaded via `python-dotenv`
- `.env` is listed in `.gitignore` and is never committed to version control
- In production, keys are injected as environment variables via the hosting platform dashboard

### Input Validation
- All user messages validated server-side: minimum 3 characters, maximum 500 characters
- Input is stripped of HTML and script tags before being passed to any AI model or external API
- Conversation history is capped at 20 messages per session to mitigate prompt injection via accumulated context

### Rate Limiting
- `POST /api/query` is rate-limited to **10 requests per minute per IP**
- Implemented via `slowapi` middleware
- Clients exceeding the limit receive `HTTP 429` with a `Retry-After` header

### Transport & CORS
- All production traffic served over **HTTPS with TLS 1.2+**
- HTTP requests are redirected to HTTPS at the load balancer level
- CORS is restricted to whitelisted frontend origins only — no wildcard (`*`) in production

### Session Handling
- Sessions are ephemeral in the MVP — no persistent user data is stored
- Session IDs are UUIDs generated client-side; no sensitive data is encoded in them
- Sessions expire after 30 minutes of inactivity

---

## Severity Classification

We use the following rubric to classify reported vulnerabilities:

| Severity | Description | Example |
|----------|-------------|---------|
| 🔴 Critical | Full system compromise or data breach possible | Hardcoded production API key in public repo |
| 🟠 High | Significant security control bypassed | Rate limit bypass enabling unlimited API calls |
| 🟡 Medium | Limited impact or requires user interaction to exploit | Reflected XSS in chat input |
| 🟢 Low | Minor issue with minimal real-world impact | Missing security header (e.g. `X-Frame-Options`) |

---

## Acknowledgements

We are grateful to the security researchers and community members who help keep RecommendMe secure. Responsible disclosures will be acknowledged here (with your permission).

*No disclosures logged yet — be the first!*

---

*Last updated: March 2026 — RecommendMe v1.0 MVP*
