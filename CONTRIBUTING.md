# Contributing to RecommendMe

This guide is for contributors working on either half of the product. Read it before you start a change so your work matches the current architecture and the review process stays predictable.

## What you can contribute

- Bug fixes in the backend or frontend
- Documentation updates
- UI improvements
- API integration fixes
- Tests and small refactors
- Prompt or recommendation flow improvements

## Before you start

1. Check whether the issue is already open or being worked on.
2. Read the relevant repo README and any feature-specific docs before editing code.
3. Pull the latest `main` branch before making changes.
4. Keep the scope of the work small enough to review in one pass.

## Local setup

### Backend

```bash
cd RecommendMe-API
python -m pip install -r requirements.txt
python -m pip install -r Requirements/requirements-dev.txt
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Useful validation commands:

```bash
python -m compileall app
python -m pytest -q
```

### Frontend

```bash
cd RecommendMe-APP
npm install
npm run setup
npm run dev
```

Useful validation commands:

```bash
npm run build
npm run test
npm run lint
```

## Working rules

- Read the files you are changing before you edit them.
- Keep changes inside the area you are responsible for unless the maintainer has approved a broader change.
- If you need to touch both repos, mention that in the issue or PR so reviewers can follow the full path.
- Prefer the smallest change that correctly fixes the problem.
- Add or update tests when the behavior changes.

## Branching

Use a descriptive branch name:

```bash
git checkout -b fix/session-hydration
git checkout -b feat/chat-resume-flow
git checkout -b docs/update-security-policy
```

## Commit messages

Use a short type prefix followed by a present-tense summary:

```text
feat: add session recovery after login
fix: handle empty avatar upload response
docs: refresh contributor setup steps
```

Keep the message focused on what changed and why. Avoid vague summaries such as `update` or `fix stuff`.

## Pull requests

Every PR should make it easy for a reviewer to answer three questions:

1. What changed?
2. Why was it needed?
3. How was it verified?

Include the following in the PR body:

- a short summary
- the problem or motivation
- the files or behaviors that changed
- the validation you ran locally
- screenshots for UI work, when relevant

If the PR closes an issue, link it at the end of the description with `Closes #<number>`.

## Review checklist

- I pulled the latest `main` before starting.
- I read the relevant docs and source files.
- I kept the change scoped and easy to review.
- I ran the relevant backend or frontend checks.
- I attached screenshots or recordings for visual changes.

## If something is unclear

Open an issue or leave a clear note in the PR. The goal is to keep the codebase understandable, not to make contributors guess.