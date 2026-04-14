# Security Policy

RecommendMe handles user conversations, authentication, session state, and external API integrations. If you find a vulnerability, report it privately so it can be fixed before public disclosure.

## Supported versions

Only the current `main` branch is actively maintained and receives security fixes.

## How to report a vulnerability

Do not open a public issue for a security problem.

Send a private report to `teamlead@gmail.com` and include:

- a short description of the issue
- the affected endpoint, page, or component
- steps to reproduce
- the impact you observed or expect
- any proof-of-concept details that help us verify it

If you would like attribution after a fix ships, mention that in your report.

## What is in scope

Security review is appropriate for the parts of the system that actually ship as part of RecommendMe:

- the FastAPI backend and its request validation
- authentication and bearer-token handling
- profile creation, update, and avatar upload flows
- chat session storage and recovery
- the `/v1/query` recommendation pipeline, including clarification and fallback behavior
- environment variables and secret handling
- CORS configuration and browser-origin restrictions
- frontend storage of auth and chat state
- integrations with OpenAI, Groq, Gemini, Ollama, and SerpAPI as used by this application

## What is out of scope

- vulnerabilities in third-party services themselves
- missing features that are not security issues
- speculative attacks without a reproducible example
- social engineering or physical attacks
- hosting-provider incidents outside the RecommendMe codebase

## Current security posture

The codebase already includes a few defensive controls worth knowing about:

- user input is sanitized and validated before it reaches the AI or product-fetch pipeline
- CORS is configured from settings and allows local development and approved deployed origins only
- auth tokens are stored client-side and protected endpoints check them server-side
- profile avatar uploads are constrained by file type and size
- chat state is stored per browser tab in `sessionStorage`, while auth state is stored in `localStorage`
- the backend keeps sessions in memory and hydrates them through dedicated session endpoints
- the API records request correlation IDs and request timing for observability

## Response targets

| Step | Target |
|------|--------|
| Initial acknowledgement | within 48 hours |
| Triage | within 3 business days |
| Critical or high severity fix | within 7 days |
| Medium severity fix | within 14 days |
| Low severity fix | within 30 days |

## Safe reporting note

If a report involves secrets, never paste real production credentials into a public issue or a shared document. Redact them and send the full details privately.