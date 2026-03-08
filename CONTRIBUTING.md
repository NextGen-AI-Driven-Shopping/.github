# Contributing to RecommendMe

Thanks for your interest in contributing. Read this document fully before writing a single line of code. It covers everything from how to set up your environment to how to write a commit message that won't get rejected.

---

## Table of Contents

- [Ways to Contribute](#ways-to-contribute)
- [Before You Start](#before-you-start)
- [Rule 1 — Always Read the Docs First](#rule-1--always-read-the-docs-first)
- [Rule 2 — Never Work Outside Your Allocated Directory](#rule-2--never-work-outside-your-allocated-directory)
- [Rule 3 — Always Pull Before You Start](#rule-3--always-pull-before-you-start)
- [How to Submit a PR](#how-to-submit-a-pr)
- [Commit Message Standards](#commit-message-standards)
- [Pull Request Standards](#pull-request-standards)
- [Code Style](#code-style)
- [Prompts](#prompts-prompt-library)
- [Questions](#questions)

---

## Ways to Contribute

- Fix a bug
- Improve AI prompts in `prompt-library/`
- Add or improve documentation
- Suggest or build a new feature
- Report an issue clearly and in detail
- Write or improve tests

---

## Before You Start

- Check existing issues before opening a new one — it may already be reported or in progress
- For large changes, open an issue first and describe what you want to build — do not invest days of work before alignment
- For small fixes (typos, minor bugs), go straight to a PR
- Make sure you know which repo and directory you are responsible for before touching anything

---

## Rule 1 — Always Read the Docs First

**Before writing any code, read the relevant documentation.**

This is mandatory, not optional. It applies to every contribution regardless of how small.

**What to read before starting:**

| You are working on | Read these first |
|--------------------|-----------------|
| Backend (`recommendme-api`) | `README.md`, `app/core/config.py`, the SDD section for your area |
| Frontend (`recommendme-ui`) | `README.md`, `src/services/api.js`, the component you are modifying |
| AI Prompts (`prompt-library`) | The prompt file you are changing, the SDD section on AI architecture |
| Any shared file | The docstring or inline comments at the top of that file |
| Any new feature | The relevant SDD section + any related existing service files |

**Why this rule exists:**

The majority of bugs introduced on team projects come from someone assuming they understand how a system works without verifying. Two minutes of reading prevents two hours of debugging and a broken build for eleven other people.

If the documentation does not exist or is unclear, write it as part of your contribution.

---

## Rule 2 — Never Work Outside Your Allocated Directory

Every team member has an assigned ownership area documented in the backend and frontend READMEs. These boundaries exist to prevent conflicts, duplicate work, and unintentional breakage.

**The rule:**

- Do not modify files outside your assigned directory without explicit approval from the person who owns that area
- If you discover a bug or issue outside your area, open a GitHub issue or raise it in Discussions — do not fix it unilaterally
- If your work genuinely requires a change in another area, coordinate with the owner first, discuss the change in the issue or PR thread, and let them implement it or explicitly hand off ownership in writing

**The one exception:**

Documentation files (`README.md`, comments, docstrings) in any directory can be improved by anyone as long as the change is documentation-only and does not touch logic.

**Why this rule exists:**

On an 11-person team, uncoordinated edits to shared files are the most common source of merge conflicts, regressions, and broken builds. Ownership boundaries only protect the team if everyone respects them.

---

## Rule 3 — Always Pull Before You Start

Before beginning any work session — every single time — sync with the latest state of the codebase.

```bash
# Step 1 — go to main and pull latest
git checkout main
git pull origin main

# Step 2 — go to your branch and rebase on top of latest main
git checkout your-branch-name
git rebase main
```

**If you do not have a branch yet:**

```bash
git checkout main
git pull origin main
git checkout -b feature/your-feature-name
```

**Never start work on a stale branch.** If you push code written against an outdated base you risk:

- Merge conflicts that are painful and time-consuming to resolve
- Reintroducing bugs that were already fixed by someone else
- Overwriting a teammate's work without realising it

If you have been away from the codebase for more than a day, pull before you write anything.

---

## How to Submit a PR

### 1. Fork and clone

```bash
git clone https://github.com/YOUR_USERNAME/REPO_NAME.git
cd REPO_NAME
```

### 2. Pull latest main (see Rule 3)

```bash
git checkout main
git pull origin main
```

### 3. Create a branch

Use a clear, descriptive name that tells reviewers what this branch is about:

```bash
git checkout -b fix/ollama-fallback-empty-response
git checkout -b feat/budget-distribution-across-categories
git checkout -b docs/update-api-endpoint-reference
git checkout -b refactor/split-ranking-from-products-service
```

**Branch naming format:** `type/short-description-in-kebab-case`

### 4. Make your changes

- Stay within your allocated directory (Rule 2)
- Read the files you are modifying before changing them (Rule 1)
- Keep each commit focused on one logical change

### 5. Write a detailed commit message (see Commit Message Standards below)

### 6. Push and open a PR

```bash
git push origin your-branch-name
```

Open a Pull Request against `main`. Fill in the full PR description (see Pull Request Standards below).

### 7. Link the issue

At the bottom of your PR description:
```
Closes #42
```

GitHub will automatically close the issue when the PR merges.

---

## Commit Message Standards

A commit message is documentation. Someone will read your commits to understand what changed and why — including you, six months from now.

**Format:**

```
type: short summary in present tense (under 72 characters)

Body — explain WHAT changed and WHY.
Not HOW (the code shows how).

- What problem does this solve?
- Why was this approach chosen over alternatives?
- Any side effects, limitations, or follow-up work needed?

Refs: #issue-number
```

**Types:**

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code change that is not a feature or fix |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `chore` | Dependencies, config, tooling |
| `perf` | Performance improvement |

**Good commit message — what to aim for:**

```
feat: add Redis caching to SerpAPI product fetch

Previously every query triggered a fresh SerpAPI call even for
identical search terms. Popular queries (e.g. "trekking tent India")
were fetched repeatedly at $0.002/call, adding up quickly.

Added query hash as cache key with 6-hour TTL in cache.py.
Cache hit rate in testing was ~40% for repeated category searches
within the same session.

Falls back to live SerpAPI fetch if Redis is unavailable — no
hard dependency introduced.

Refs: #34
```

**Bad commit messages — do not do this:**

```
fix stuff
wip
updated file
changes
asdfgh
did the thing
minor update
```

A commit message that does not explain why a change was made has zero documentation value. PRs with unexplained commits will be asked to rewrite them before merging.

---

## Pull Request Standards

A pull request is a formal proposal to change shared code. It must be reviewable by someone who was not involved in writing it. Incomplete PRs slow the team down and will be sent back.

**Every PR must include all of the following sections:**

---

### Summary

What does this PR do? Two to three sentences maximum.

---

### Problem / Motivation

What issue, bug, or need does this address? Link the GitHub issue.

---

### Changes Made

A specific bullet list of every meaningful change. Be explicit.

```
- Added `app/services/cache.py` — Redis get/set wrapper with 6-hour TTL
- Modified `app/services/products.py` — checks cache before SerpAPI call
- Updated `requirements.txt` — added redis==5.0.1
- Added `tests/unit/test_cache.py` — unit tests for cache hit/miss
```

---

### How to Test

Step-by-step instructions for a reviewer to verify the change works locally. Do not assume the reviewer knows how the feature should behave.

```
1. Start Redis: docker run -p 6379:6379 redis
2. Start the server: uvicorn main:app --reload
3. Send POST /v1/query with: { "user_message": "I need a trekking tent" }
4. Send the same request again
5. Check server logs — second request should show "cache hit: <hash>"
6. Run: pytest tests/unit/test_cache.py -v
```

---

### Screenshots or Recordings (required for any UI change)

Any change that affects what the user sees requires visual proof. A reviewer should not need to run the app to understand what changed on screen.

**Required for:**
- New components or UI elements
- Changes to layout, spacing, or colour
- New states — loading, error, empty
- Any change to product cards, chat flow, or landing page

**Provide before and after:**

| Before | After |
|--------|-------|
| *(screenshot)* | *(screenshot)* |

Annotate screenshots to highlight what changed if it is not obvious. Use Loom, Gifox, or any screen recorder for interaction flows. Attach files directly to the PR — do not link to external storage.

---

### Does This Break Anything?

Explicitly state whether this change affects existing behavior, other team members' directories, or the API contract. If it does, list what changed and who needs to know before this merges.

---

### PR Checklist

Before clicking "Open Pull Request":

- [ ] I have pulled from `main` and rebased before starting this work
- [ ] I have read the documentation for every file I modified
- [ ] I have not modified files outside my allocated directory without written approval
- [ ] All tests pass locally
- [ ] I have written tests for any new logic
- [ ] Commit messages follow the format above
- [ ] Screenshots or recordings are attached for any UI change
- [ ] The relevant issue is linked with `Closes #issue-number`

---

## Code Style

### Python (backend)

- Follow PEP 8
- Use type hints on all function signatures — no untyped parameters
- Keep functions focused on one job and under 40 lines where possible
- Docstring on every function that is not self-evident
- No commented-out code in PRs — delete it or explain it

### JavaScript / React (frontend)

- Functional components and hooks only — no class components
- One component per file, named to match the filename exactly
- Tailwind utility classes only — no inline styles, no CSS modules
- Logic in hooks, not in JSX — if a component exceeds ~100 lines, split it
- No `console.log` statements in PRs

---

## Prompts (`prompt-library/`)

If you are improving or adding AI prompts:

- Clearly explain what the prompt does and why you changed it
- Include a before/after example of the AI output so reviewers can evaluate the improvement without running it
- Keep prompts focused — one job per prompt
- Do not change a prompt that belongs to another team member's service without coordination

---

## Questions

Open a [GitHub Discussion](https://github.com/orgs/NextGen-AI-Driven-Shopping/discussions) before starting work on anything large. Five minutes of alignment upfront saves days of rework.

Keep all communication in GitHub — issues, PR threads, and discussions. Keeping context in GitHub makes it visible to everyone, not just the people in a private message.
