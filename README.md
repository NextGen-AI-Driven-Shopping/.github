# RecommendMe Community Docs

This folder contains the GitHub-facing documentation for RecommendMe. It is the first stop for contributors, reviewers, and anyone trying to understand the project before opening the code.

RecommendMe is a conversational product recommendation system. A user describes what they need in plain language, the backend clarifies intent when needed, and the frontend presents ranked product options with live product data and direct purchase links.

## What belongs here

- [CONTRIBUTING.md](CONTRIBUTING.md) for local setup, workflow expectations, and pull request hygiene
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for collaboration standards
- [SECURITY.md](SECURITY.md) for private vulnerability reporting
- [profile/README.md](profile/README.md) for the public GitHub profile landing page
- [PULL_REQUEST_TEMPLATE.md](PULL_REQUEST_TEMPLATE.md) for consistent review submissions

## Project at a glance

RecommendMe is split into two applications:

- `RecommendMe-API` is the FastAPI backend that orchestrates query understanding, clarification, recommendation generation, session snapshots, auth, and profile data.
- `RecommendMe-APP` is the React + Vite frontend that provides the chat experience, authentication screens, profile management, and recommendation rendering.

Together they support a single workflow:

1. A user enters a shopping need in natural language.
2. The backend checks whether the request is clear or needs clarification.
3. If needed, the app asks follow-up questions and stores the session state.
4. Once enough context exists, the backend generates product categories and product types, then fetches real listings.
5. The frontend renders the results and lets the user continue the conversation.

## Fast start

If you are onboarding to the project, start here:

1. Read [CONTRIBUTING.md](CONTRIBUTING.md).
2. Review the backend and frontend README files in `RecommendMe-API/README.md` and `RecommendMe-APP/README.md`.
3. Run both apps locally:

```bash
# Backend
cd RecommendMe-API
python -m pip install -r requirements.txt
python -m pip install -r Requirements/requirements-dev.txt
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload

# Frontend
cd RecommendMe-APP
npm install
npm run setup
npm run dev
```

4. Open the frontend, send a query, and confirm the backend responds through the full chat flow.

## Documentation style

The docs in this folder aim to be:

- accurate to the current codebase
- concise enough to scan quickly
- specific enough to help a new contributor move without guesswork
- consistent in tone and formatting across all files

If the implementation changes, update the docs here at the same time.