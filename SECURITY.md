# Security Policy

## Supported Versions

Only the latest version of RecommendMe is actively maintained and receives security updates.

| Version | Supported |
|---------|-----------|
| Latest (main branch) | ✅ |
| Older branches | ❌ |

## Reporting a Vulnerability

**Please do NOT open a public GitHub issue for security vulnerabilities.**

If you discover a security issue — such as exposed API keys, data leaks,
prompt injection risks, or any other vulnerability — report it privately.

**Email:** @Team Lead

Include in your report:
- A clear description of the vulnerability
- Steps to reproduce it
- The potential impact
- Your suggested fix (optional but appreciated)

We will acknowledge your report within **48 hours** and aim to resolve
confirmed vulnerabilities within **7 days** depending on severity.

We appreciate responsible disclosure and will credit you in the fix
if you'd like.

## What is In Scope

- Backend API (FastAPI endpoints)
- AI prompt injection or manipulation
- Exposed or leaked API keys
- Insecure handling of user input

## What is Out of Scope

- Third-party services (OpenAI, SerpAPI, Ollama) — report those to them directly
- Bugs that do not have a security impact — open a regular issue for those
