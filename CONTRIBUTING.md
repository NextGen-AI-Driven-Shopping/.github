# Contributing to RecommendMe

Thanks for your interest in contributing. This document covers everything
you need to get started.

---

## Ways to Contribute

- Fix a bug
- Improve the AI prompts in `prompt-library/`
- Add or improve documentation
- Suggest or build a new feature
- Report an issue clearly and in detail

---

## Before You Start

- Check existing issues before opening a new one — it may already be reported
- For large changes, open an issue first and describe what you want to build
- For small fixes (typos, minor bugs), go straight to a PR

---

## How to Submit a PR

1. Fork the repository
2. Create a branch with a descriptive name
```bash
   git checkout -b fix/ollama-fallback
   git checkout -b feature/budget-distribution
```
3. Make your changes
4. Write a clear commit message
```
   fix: handle empty SerpAPI response gracefully
   feat: add budget distribution across categories
   docs: update API endpoint reference
```
5. Open a Pull Request against `main`
6. Fill in the PR description — what changed and why
7. Link the issue it closes: `Closes #42`

---

## Code Style

**Python (backend)**
- Follow PEP 8
- Use type hints on all functions
- Keep functions focused and under 40 lines where possible

**JavaScript / React (frontend)**
- Functional components and hooks only
- One component per file
- Tailwind utility classes only — no inline styles

---

## Prompts (`prompt-library/`)

If you're improving or adding AI prompts:
- Explain what the prompt does and why you changed it
- If possible, include a before/after example of the AI output
- Keep prompts focused — one job per prompt

---

## Questions?

Open a [GitHub Discussion](https://github.com/orgs/NextGen-AI-Driven-Shopping/discussions)
before starting work on anything large. We're happy to align before you invest time.
