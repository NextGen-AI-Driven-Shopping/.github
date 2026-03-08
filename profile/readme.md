# NextGen AI-Driven Shopping

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
