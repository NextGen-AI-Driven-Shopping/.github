# NextGen AI-Driven Shopping

> Building **RecommendMe** — a conversational AI that understands what you need and recommends the best products. No cart. No checkout. Just clarity.

---

## What is RecommendMe?

RecommendMe is an AI-powered product discovery engine built to solve one problem: **decision fatigue in online shopping.**

Instead of opening 12 tabs, reading conflicting reviews, and still second-guessing yourself — you describe what you need in plain language. RecommendMe figures out what you actually require, asks smart follow-up questions if needed, and returns a curated list of the best products with real prices and direct buy links.

**We are not a store. We don't sell products. We don't run ads.**
We are purely a recommendation engine — the knowledgeable friend who did the research so you don't have to.

---

## How It Works

```
User: "I want to go trekking"
            │
            ▼
  ┌──────────────────────┐
  │  Is the query clear? │  ← Tier 1 AI (Ollama — runs locally)
  └──────────┬───────────┘
     VAGUE   │   CLEAR
       │     │
       ▼     └──────────────────────────┐
  Ask 2–3 follow-up questions           │
  to gather more context                │
       │                                │
       └───────────────────────────────►▼
                            ┌───────────────────────┐
                            │  Understand intent    │  ← Tier 2 AI (GPT-4o)
                            │  Generate categories  │
                            │  Rank + explain picks │
                            └───────────┬───────────┘
                                        │
                                        ▼
                            ┌───────────────────────┐
                            │  Fetch real products  │  ← SerpAPI
                            │  Prices, ratings,     │    Google Shopping
                            │  and buy links        │
                            └───────────────────────┘
```

---

## Two-Tier AI Design

The system uses two AI models deliberately — one cheap and fast, one powerful and precise.

| | Tier 1 | Tier 2 |
|---|---|---|
| **Model** | Ollama — Phi-3 Mini | OpenAI GPT-4o |
| **Runs on** | Local server | Cloud API |
| **Job** | Check if query is vague. Generate follow-up questions if needed. | Understand full intent. Identify product categories. Rank and explain recommendations. |
| **Cost** | Free | Pay-per-use (~$0.01–0.03 per session) |
| **Fallback** | Switches to GPT-4o-mini if Ollama is unavailable | — |

This keeps costs low without sacrificing quality where it matters.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Tailwind CSS |
| Backend | Python 3.11, FastAPI |
| Tier 1 AI | Ollama (Phi-3 Mini / Llama 3.2) |
| Tier 2 AI | OpenAI GPT-4o |
| Product Data | SerpAPI — Google Shopping |
| HTTP Client | httpx (async) |
| Validation | Pydantic v2 |

---

## Status

🟡 **Active development — MVP in progress**

---

<sub>MIT License · Built with curiosity and a genuine hatred of decision fatigue</sub>
