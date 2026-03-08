# .github

This repository holds **organization-wide community health defaults** for all repositories under **NextGen AI-Driven Shopping**.

Files placed here are automatically inherited by any repository in the organization that does not define its own version — keeping standards consistent across the entire codebase without duplicating files everywhere.

---

## Repository Structure

```
.github/
├── profile/
│   └── README.md               # Public organization homepage on GitHub
├── CONTRIBUTING.md             # Default contribution guidelines for all repos
├── CODE_OF_CONDUCT.md          # Behavioral expectations for all contributors
└── SECURITY.md                 # How to privately report a security vulnerability
```

---

## File Reference

| File | Applies To | Purpose |
|------|------------|---------|
| `profile/README.md` | GitHub org homepage | Publicly visible landing page shown on the organization's GitHub profile |
| `CONTRIBUTING.md` | All repos | Branching strategy, PR process, commit conventions, and local setup guidance |
| `CODE_OF_CONDUCT.md` | All repos | Standards for respectful collaboration; outlines enforcement procedures |
| `SECURITY.md` | All repos | Instructions for privately reporting vulnerabilities; scope and response SLA |

---

## How Inheritance Works

GitHub treats this `.github` repository as the **fallback source** for community health files across the organization.

```
Does repo X have its own CONTRIBUTING.md?
        │
        ├── YES → GitHub uses repo X's version
        │
        └── NO  → GitHub falls back to this .github repo's version
```

This means:
- Every repo automatically gets these defaults on day one — no manual setup needed
- Individual repos can **override** any file by adding their own version to their root or `.github/` folder
- Changes made here propagate to all repos that haven't overridden the file

> **Example:** `recommendme-api` does not have its own `SECURITY.md`, so GitHub surfaces the one from this repo on its Security tab and in its community profile checklist automatically.

---

## What Does NOT Live Here

Some files must be defined per-repository and cannot be inherited from this `.github` repo:

| File / Directory | Why it lives in each repo |
|------------------|--------------------------|
| `LICENSE` | License terms are specific to each project's codebase |
| `.github/ISSUE_TEMPLATE/` | Issue templates are scoped to the repo they live in |
| `.github/pull_request_template.md` | PR templates are scoped to the repo they live in |
| `.github/workflows/` | GitHub Actions workflows are always repo-specific |
| Project-specific code or scripts | This repo is config only — no application logic |

---

## Making Changes

This repository follows the same contribution process as all others under the organization.

1. Open an **issue** first if you're proposing a significant change to a shared default (e.g. rewriting `CODE_OF_CONDUCT.md`)
2. Fork or branch, make your changes, and open a **pull request** with a clear description of what's changing and why
3. Changes that affect all repositories should be reviewed by at least **one other maintainer** before merging
4. Once merged, the updated default takes effect immediately for all repos that inherit it

For minor corrections (typos, broken links, formatting), a PR without a preceding issue is fine.

---

## Maintained By

The core maintainer team of **NextGen AI-Driven Shopping**.

If something here is outdated, incorrect, or missing — open an issue or PR. This is a shared resource and contributions from any team member are welcome.

---

*Part of the NextGen AI-Driven Shopping GitHub organization — [RecommendMe](https://github.com/nextgen-ai-driven-shopping/recommendme)*
