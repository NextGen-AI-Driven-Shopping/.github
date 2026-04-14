# 🛍️ RecommendMe

**A conversational product recommendation engine that replaces the research spiral with one focused conversation.**

The average online shopper spends 45–90 minutes across multiple tabs before purchasing. RecommendMe cuts that to a few targeted questions and a curated result set grounded in real product data and live prices.

### The Problem We Solve

- Search engines return thousands of products. You want the three best ones.
- Review platforms are gamed. You need trustworthy signals.
- Shopping forms assume you know what you're looking for. You often don't.
- Comparison grids are overwhelming. You want to talk through it.

### What RecommendMe Does

1. **Listens** — understands your need in plain language, no forms required
2. **Asks smart questions** — narrows down context only when necessary
3. **Generates options** — creates product categories and types at runtime (no hardcoded lists)
4. **Fetches real data** — pulls live listings, prices, and ratings from shopping APIs
5. **Keeps the conversation going** — lets you ask follow-ups without restarting

### Who It Is For

- **Shoppers** tired of endless tabs and sponsored listings
- **Contributors** looking for a clean example of a conversational AI product
- **Teams** exploring how to build recommendation systems that scale with context
- **Developers** interested in FastAPI + React patterns for real-time chat flows

### Why This Matters

RecommendMe is not just a feature—it's a proof of concept that **AI can replace friction, not create it**. Every recommendation is grounded in real user context. Every question is there for a reason. Every product is real.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      FRONTEND (React + Vite)                    │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Chat Interface                                          │  │
│  │  - Session Management (sessionStorage)                   │  │
│  │  - Auth State (localStorage)                             │  │
│  │  - Message History & Rendering                           │  │
│  │  - Lazy-Loaded Pages (Chat, Profile, Login)             │  │
│  └────────────────┬─────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                         │
                         ▼
           ┌─────────────────────────┐
           │  API Bridge Layer       │
           │  - Query Requests       │
           │  - Auth Endpoints       │
           │  - Session Hydration    │
           │  - Profile Updates      │
           └────────────┬────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    BACKEND (FastAPI)                             │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Query Pipeline                                          │  │
│  │  ┌──────────────────────────────────────────────────┐   │  │
│  │  │ 1. Vagueness Check (AI)                          │   │  │
│  │  │    ↓                                              │   │  │
│  │  │ 2. Follow-up Questions (AI) — 5 questions max   │   │  │
│  │  │    ↓                                              │   │  │
│  │  │ 3. Recommendation Plan (AI)                       │   │  │
│  │  │    ↓                                              │   │  │
│  │  │ 4. Product Fetch (SerpAPI)                        │   │  │
│  │  │    ↓                                              │   │  │
│  │  │ 5. Response Formatting & Session Save            │   │  │
│  │  └──────────────────────────────────────────────────┘   │  │
│  │                                                           │  │
│  │  AI Provider Chain (Fallback):                           │  │
│  │  Groq → OpenAI → Gemini → Ollama                        │  │
│  │                                                           │  │
│  │  Session Store: In-memory + Redis (optional)             │  │
│  │  Auth: CSV + JWT Tokens                                  │  │
│  │  Profiles: JSON-backed + Avatar Upload                   │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
         │                            │                    │
         ▼                            ▼                    ▼
    ┌─────────┐            ┌──────────────────┐   ┌──────────────┐
    │ OpenAI  │            │   SerpAPI        │   │   Ollama     │
    │ (GPT-4o)│            │ (Google Shopping)│   │  (Local LLM) │
    └─────────┘            └──────────────────┘   └──────────────┘
```

---

## Key Features

- **Runtime AI** — no hardcoded templates, every question and recommendation is generated fresh
- **Multi-turn clarification** — exactly 5 follow-ups gathered across 2 intelligent rounds
- **Real product data** — live prices, ratings, and purchase links fetched on demand
- **Smart fallbacks** — if one AI provider fails, the next takes over automatically
- **Session persistence** — users can resume conversations across browser sessions
- **Bearer token auth** — simple JWT authentication without external OAuth
- **Mobile-responsive** — works seamlessly on desktop, tablet, and phone

---

## Getting Started

Want to see it in action? Clone the repo and run both services locally:

```bash
# Terminal 1: Backend
cd RecommendMe-API
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload

# Terminal 2: Frontend
cd RecommendMe-APP
npm install
npm run dev
```

Open `http://localhost:5173` and describe a shopping need. The system will handle the rest.

---

## Why Contribute?

- **Learn real patterns** — multi-turn conversation flows, AI orchestration, session management
- **Small, readable codebase** — not a massive framework, just solid engineering
- **Active development** — new features ship weekly, PRs reviewed promptly
- **Clear problems to solve** — from UX polish to new AI providers to performance optimization
- **Good docs** — every area has architecture notes, not just comments in code

## How it works

1. The user enters a shopping need in the frontend chat interface.
2. The backend checks whether the query is clear or needs clarification.
3. If the request is vague, the app asks targeted questions to gather missing context.
4. The recommendation engine generates a category, product types, and product explanations.
5. The system fetches real product data and renders the results in the chat UI.

## Getting started

If you want to run the project locally, start with the contributor guide in the repository root and then launch both apps:

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

## Learn more

- [Contributing](../CONTRIBUTING.md)
- [Security](../SECURITY.md)
- [Code of Conduct](../CODE_OF_CONDUCT.md)# NextGen AI-Driven Shopping

> Building **RecommendMe** — a conversational AI that understands what you need and recommends the best products.
> No cart. No checkout. Just clarity.

---

## The Problem

Online shopping is broken in a specific, underappreciated way.

It's not that products are hard to find — it's that there are *too many*, spread across too many platforms, buried under sponsored listings and manipulated reviews. The average shopper spends **45–90 minutes** across multiple tabs before a single purchase. Many don't buy at all.

That's not a search problem. That's a **decision fatigue** problem — and search engines weren't built to solve it.

---

## What We're Building

**RecommendMe** is an AI-powered product discovery engine with a single job: replace the multi-tab research spiral with one honest conversation.

You describe what you need in plain language — the way you'd explain it to a knowledgeable friend. RecommendMe figures out what you actually require, asks targeted follow-up questions if your intent is unclear, and returns a curated shortlist of the best-matched products with real prices and direct purchase links.

```
User: "I'm planning a 3-day trek to Himachal Pradesh next month, camping in the wild."

RecommendMe understands: waterproof tent · cold weather sleeping bag · lightweight backpack
                         headlamp · portable stove · trekking poles

→ Returns top 3 picks per category with prices, ratings, and a reason why each fits your situation.
```

**We are not a store. We don't sell products. We don't run ads. We have no inventory.**

We are purely a recommendation engine — built to be the knowledgeable friend who did the research so you don't have to.

---

## How It Works

```
You type your need in plain language
            │
            ▼
  ┌─────────────────────────┐
  │   Is the query clear?   │  ◄── Tier 1 AI  (Ollama · runs locally · free)
  └────────────┬────────────┘
       VAGUE   │   CLEAR
         │     │
         ▼     └─────────────────────────────┐
    Ask 2–3 targeted follow-up               │
    questions to gather context              │
         │                                   │
         └──────────────────────────────────►▼
                               ┌─────────────────────────┐
                               │   Extract full intent   │  ◄── Tier 2 AI  (GPT-4o · cloud)
                               │   Generate categories   │
                               │   Rank & explain picks  │
                               └─────────────┬───────────┘
                                             │
                                             ▼
                               ┌─────────────────────────┐
                               │   Fetch real products   │  ◄── SerpAPI · Google Shopping
                               │   Live prices, ratings, │
                               │   platform & buy links  │
                               └─────────────────────────┘
```

---

## Two-Tier AI Architecture

The system uses two models deliberately — one lightweight and free for fast triage, one powerful for the part that actually matters.

|  | Tier 1 | Tier 2 |
|--|--------|--------|
| **Model** | Ollama — Phi-3 Mini / Llama 3.2 3B | OpenAI GPT-4o |
| **Runs on** | Local server | Cloud API |
| **Responsibility** | Classify vagueness. Generate follow-up questions. | Extract intent. Identify product categories. Rank and explain recommendations. |
| **Cost** | Free | ~$0.01–$0.03 per session |
| **Fallback** | Auto-switches to GPT-4o-mini if Ollama is unavailable | — |

This keeps operating costs low at scale without compromising the quality of the final recommendation — which is the only output the user ever sees.

---

## Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Frontend** | React 18, Tailwind CSS | Conversational chat UI, mobile-responsive |
| **Backend** | Python 3.11, FastAPI | Async, stateless, horizontally scalable |
| **Tier 1 AI** | Ollama (Phi-3 Mini / Llama 3.2) | Runs locally on backend server |
| **Tier 2 AI** | OpenAI GPT-4o | Cloud inference for final recommendations |
| **Product Data** | SerpAPI — Google Shopping | Live listings, prices, ratings, buy links |
| **Caching** | Redis (optional) | Reduces SerpAPI call volume on repeated queries |
| **Validation** | Pydantic v2 | Request/response schema enforcement |
| **HTTP Client** | httpx (async) | External API calls from backend |

---

## Repositories

| Repository | Description | Status |
|------------|-------------|--------|
| `recommendme-app` | React frontend — chat interface and landing page | 🟡 In development |
| `recommendme-api` | FastAPI backend — query orchestration and AI layer | 🟡 In development |
| `.github` | Org-wide defaults — contributing, code of conduct, security policy | ✅ Live |

---

## Project Status

🟡 **MVP in active development** — targeting initial deployment within weeks.

The MVP covers the full end-to-end flow: natural language input → vagueness detection → follow-up questions → intent extraction → real product results with buy links. User accounts, saved lists, and personalization are scoped for v2.

---

## What's Coming

A few things on the roadmap after MVP:

- **Price history signals** — surface "lowest price this month" context per product
- **Review trust scoring** — cross-reference Fakespot to flag unreliable review patterns
- **Budget distribution engine** — when you give a total budget, AI splits it optimally across all required items
- **Reddit signal layer** — pull real community opinions from relevant subreddits
- **Voice input** — hands-free query on mobile via Web Speech API
- **Chrome extension** — sidebar overlay while browsing Amazon or Flipkart

---

## Get Involved

We follow standard open-source contribution practices across all repositories.

- Read [`CONTRIBUTING.md`](../CONTRIBUTING.md) before opening a PR
- Follow our [`CODE_OF_CONDUCT.md`](../CODE_OF_CONDUCT.md)
- Report security issues privately per [`SECURITY.md`](../SECURITY.md) — please don't open public issues for vulnerabilities

Questions, ideas, or feedback? Open an issue in the relevant repository.

---

<sub>MIT License · Built by NextGen AI-Driven Shopping · Fuelled by genuine frustration with 12-tab shopping spirals</sub>
