# RecommendMe — Team Roles & Contributions
**Project:** RecommendMe — AI-Powered Product Recommendation Engine
**Document Version:** 1.0 | March 2026
**Classification:** Internal / Company Confidential

---

## Table of Contents
1. [Project Leadership](#1-project-leadership)
2. [Individual Team Member Sections](#2-individual-team-member-sections)
3. [Team Responsibility Matrix](#3-team-responsibility-matrix)
4. [Integration Ownership](#4-integration-ownership)
5. [Complete File Ownership Index](#5-complete-file-ownership-index)

---

## 1. Project Leadership

**Varun K S — Team Lead · Integration Architect · UI/UX Design Lead**

Varun holds overall ownership of the RecommendMe project. As Team Lead, he is responsible for the full system's architectural integrity, cross-service integration, and the health of all external API contracts. He ensures that every module built by individual contributors connects correctly into the end-to-end flow — from the frontend through the FastAPI backend and out to OpenAI, Ollama, and SerpAPI.

In addition to his backend and infrastructure role, Varun leads all UI/UX design decisions for the frontend. This includes defining the visual identity of the product — colour palette, typography, font choices, spacing systems, and the overall design language. Sharan implements the frontend in code, but all design direction, aesthetic decisions, and UX review are driven and approved by Varun.

Beyond technical integration, Varun owns the GitHub organisation, governs the repository (branch strategy, PR reviews, merge authority), and manages CI/CD pipeline configuration. Any standalone infrastructure file, deployment configuration, or file that does not have a natural home in another team member's domain is owned and maintained by Varun.

---

## 2. Individual Team Member Sections

---

### Varun K S — Team Lead · API Integration · UI/UX Design Lead · Project Infrastructure

**Responsibilities**

*Backend & Infrastructure*
- Owns the top-level application entry point and overall request lifecycle
- Designs and enforces the `POST /v1/query` and `GET /v1/health` API contracts
- Wires all services together inside `app/api/` (route handlers, dependency injection)
- Manages the `deps.py` shared dependency layer consumed by all route handlers
- Owns all CI/CD workflow files and ensures automated testing and deployment pipelines are green
- Governs GitHub organisation settings, branch protection rules, access control, and code review policy
- Maintains all top-level project files (`main.py`, `Dockerfile`, `docker-compose.yml`, `requirements.txt`, `requirements-dev.txt`, `.env.example`, `.gitignore`, `README.md`)
- Handles any miscellaneous or cross-cutting file not explicitly owned by another member
- Final review and merge authority for all pull requests

*UI/UX Design*
- Defines the complete visual identity of RecommendMe — colour palette, brand colours, hover/active states, and dark/light mode decisions
- Owns typography system: font family selection (headings, body, monospace), font sizes, line heights, font weights, and letter spacing scale
- Establishes spacing, layout grid, and border-radius conventions used across all frontend components
- Designs the end-to-end user experience flow: landing page → chat initiation → follow-up question display → product card reveal
- Produces design references (wireframes, mockups, or annotated screenshots) that Sharan implements in code
- Reviews all frontend PRs for visual consistency and UX quality before merge
- Makes final calls on any design ambiguity or component-level visual decisions raised by Sharan

**Owned Modules**

```
recommendme-api/                        ← Project root
├── main.py                             ← Entry point: imports and runs app
├── Dockerfile                          ← Production container
├── docker-compose.yml                  ← Local dev: app + Redis together
├── requirements.txt                    ← Production dependencies
├── requirements-dev.txt                ← Dev + test dependencies
├── .env.example                        ← All env vars with placeholder values
├── .gitignore
└── README.md

app/                                    ← Main application package
├── __init__.py
├── main.py                             ← FastAPI app, lifespan, middleware registration
│
└── api/                                ← All route handlers
    ├── __init__.py
    ├── deps.py                         ← Shared dependencies (session, rate limiter injection)
    └── v1/
        ├── __init__.py
        ├── router.py                   ← Aggregates all v1 routes into one router
        ├── query.py                    ← POST /v1/query (core endpoint)
        └── health.py                   ← GET /v1/health (system status)

app/services/
└── __init__.py                         ← Package init (individual service files owned by respective engineers)

.github/                                ← CI/CD workflows
└── workflows/
    ├── ci.yml                          ← Run tests + lint on every PR
    └── deploy.yml                      ← Auto-deploy to Railway on merge to main
```

---

### Sharan — Frontend Engineer

**Responsibilities**
- Owns the React frontend codebase — translates Varun's UI/UX design direction into production-quality code
- Implements all pages (`/` Landing Page, `/chat` Chat Interface, `/chat/:id` Session Route)
- Builds and maintains all chat UI components: `Sidebar`, `ChatHeader`, `MessageList`, `FollowUpQuestion`, `CategoryList`, `ProductCard`, `ExpertTip`, `LoadingIndicator`, `InputBar`, `StarterPrompts`
- Applies Varun's design system in code using Tailwind CSS utility classes — colour tokens, typography scale, spacing, and layout grid as specified
- Manages React state via `useState` and `useReducer` for messages, loading state, input value, sessions, and current session UUID
- Integrates with the backend API using Axios (`POST /v1/query`, `GET /v1/health`)
- Ensures mobile-responsive layout using Tailwind CSS (FR-14)
- Owns client-side routing via React Router 6.x
- Responsible for rendering follow-up question flows and final product card results in the chat UI
- Raises design questions to Varun and implements approved resolutions — does not make independent visual or UX decisions

**Owned Modules**
*(Frontend repository — outside the `recommendme-api/` backend tree)*
```
React Frontend Application (separate repo / deployment)
├── pages/LandingPage
├── pages/ChatApp
├── components/Sidebar
├── components/ChatHeader
├── components/MessageList
├── components/FollowUpQuestion
├── components/CategoryList
├── components/ProductCard
├── components/ExpertTip
├── components/LoadingIndicator
├── components/InputBar
└── components/StarterPrompts
```

---

### Darshan — Vagueness Service Engineer

**Responsibilities**
- Owns the Tier 1 AI classification layer end-to-end
- Implements the `vagueness.py` service: sends queries to local Ollama, parses CLEAR/VAGUE classification, and generates 2–3 targeted follow-up questions when needed
- Implements the GPT-4o-mini automatic fallback when Ollama is unavailable (per FR-02, FR-03 and the reliability matrix in §6.3)
- Owns the vagueness check prompt template and ensures it correctly encodes the classification logic (use-case present, context present, budget signal present)
- Maintains the `check_ollama.py` developer utility script to verify Ollama availability during local development
- Responsible for keeping Tier 1 AI response time under 1.5 seconds (per NFR §6.1)

**Owned Modules**

```
app/services/                           ← Business logic (Darshan's files)
└── vagueness.py                        ← Tier 1 AI: Ollama check + GPT-4o-mini fallback

app/prompts/                            ← AI prompt templates (Darshan's files)
└── vagueness_check.py                  ← Tier 1 prompt: CLEAR / VAGUE classification

scripts/                                ← Developer utilities (Darshan's files)
└── check_ollama.py                     ← Verify Ollama is running and model is loaded
```

---

### Lahari — Product Recommendation Service & Cache/Performance Engineer

**Responsibilities**
- Owns the Tier 2 AI recommendation orchestration layer (`recommender.py`)
- Implements intent extraction: takes assembled session context and invokes GPT-4o to extract a structured list of required product categories with budget allocations and rationale
- Owns Redis caching (`cache.py`): implements `get` and `set` operations keyed by query hash with a 6-hour TTL, reducing redundant SerpAPI calls
- Responsible for the intent extraction prompt template used in GPT-4o calls
- Ensures GPT-4o recommendation generation completes within 4 seconds (per NFR §6.1)
- Monitors and tunes Redis cache hit rates to optimise SerpAPI quota usage
- Handles the `SERP_QUOTA_EXCEEDED` and partial-results fallback logic in coordination with Vrandha

**Owned Modules**

```
app/services/                           ← Business logic (Lahari's files)
├── recommender.py                      ← Tier 2 AI: GPT-4o intent + category generation
└── cache.py                            ← Redis get/set for product query results

app/prompts/                            ← AI prompt templates (Lahari's files)
└── intent_extraction.py                ← Tier 2 prompt: extract categories from context
```

---

### Sharanu — Ranking Service Engineer

**Responsibilities**
- Owns the GPT-4o product ranking layer (`ranking.py`)
- Implements the logic that takes raw SerpAPI product results per category and calls GPT-4o to select the top 3 products, generate personalised ranking justifications, and write expert tips
- Owns the product ranking prompt template, including the structured output format (title, price, rating, reason, expert_tip)
- Ensures ranking output conforms to the `QueryResponse` / `CategoryResult` / `ProductCard` schema defined by Sujay
- Responsible for maintaining ranking quality: explanations must be specific to the user's session context, not generic
- Handles the `OPENAI_RATE_LIMIT` retry-with-exponential-backoff logic within the ranking service

**Owned Modules**

```
app/services/                           ← Business logic (Sharanu's files)
└── ranking.py                          ← GPT-4o product ranking + explanation generation

app/prompts/                            ← AI prompt templates (Sharanu's files)
└── product_ranking.py                  ← Tier 2 prompt: rank products + write reasons
```

---

### Vrandha — Product & Data Service Engineer · Prompt Engineer

**Responsibilities**
- Owns the SerpAPI integration layer (`products.py`): constructs targeted Google Shopping search queries per product category, fetches real listings, and parses returned product data (title, price, rating, thumbnail, platform, link)
- Implements affiliate tag injection into product URLs (`formatters.py` in coordination with Sai Kumar)
- Sets SerpAPI query parameters: `gl=in`, `hl=en`, `num=5` per category
- Owns the `mock_serp.py` developer script for offline development without consuming SerpAPI quota
- Acts as Prompt Engineer: reviews and iterates on all three prompt templates (`vagueness_check.py`, `intent_extraction.py`, `product_ranking.py`) for output quality, token efficiency, and structured response formatting
- Runs prompt quality tests via `scripts/test_prompt.py`
- Responsible for keeping SerpAPI product fetch time under 2 seconds per category (per NFR §6.1)

**Owned Modules**

```
app/services/                           ← Business logic (Vrandha's files)
└── products.py                         ← SerpAPI fetch + affiliate tag injection

app/prompts/                            ← AI prompt templates (Vrandha owns this package)
├── __init__.py                         ← Package init for prompts module
├── vagueness_check.py                  ← (reviewed and iterated on as Prompt Engineer)
├── intent_extraction.py                ← (reviewed and iterated on as Prompt Engineer)
└── product_ranking.py                  ← (reviewed and iterated on as Prompt Engineer)

scripts/                                ← Developer utilities (Vrandha's files)
├── test_prompt.py                      ← Run a prompt against GPT-4o from terminal
└── mock_serp.py                        ← Generate mock SerpAPI responses for offline dev
```

---

### Aditi — Core Configuration & Application Infrastructure Engineer

**Responsibilities**
- Owns the `app/core/` package: all cross-cutting application concerns
- Implements `config.py` using Pydantic `BaseSettings` to load and validate all environment variables from `.env`
- Defines and registers all custom exception classes in `exceptions.py` and maps them to consistent HTTP error responses (using the error catalog from §10.2)
- Configures structured JSON logging in `logger.py` for production observability
- Implements request-level middleware in `middleware.py`: request logging, response timing, and correlation ID injection
- Owns `security.py`: CORS configuration (whitelisted origins), input sanitization rules, and rate limiting configuration for `slowapi`
- Ensures rate limiting of 10 requests/minute per IP on `POST /api/query` is correctly applied (FR from §11.3)

**Owned Modules**

```
app/core/                               ← App config, exceptions, cross-cutting concerns
├── __init__.py
├── config.py                           ← All settings loaded from .env via Pydantic BaseSettings
├── exceptions.py                       ← Custom exception classes + HTTP error handlers
├── logger.py                           ← Structured JSON logging setup
├── middleware.py                       ← Request logging, timing, correlation ID middleware
└── security.py                         ← Input sanitization, CORS config, rate limit rules
```

---

### Sujay — Models & Validation Engineer

**Responsibilities**
- Owns all Pydantic model definitions across the `app/models/` package
- Defines `QueryRequest` and `ConversationMessage` in `requests.py` — the schema for all incoming API payloads
- Defines `QueryResponse`, `ProductCard`, and `CategoryResult` in `responses.py` — the schema for all outgoing API responses (both `followup` and `recommendations` types)
- Defines internal-only types in `internal.py` not exposed to API consumers
- Ensures all models enforce the field constraints documented in the SDD (e.g., session_id as UUID, message role enum, price/rating types)
- Coordinates with Varun to ensure models satisfy the API contract defined in §7.2
- Reviews model changes from all team members before merge, as schema changes are cross-cutting

**Owned Modules**

```
app/models/                             ← All Pydantic models, split by purpose
├── __init__.py
├── requests.py                         ← QueryRequest, ConversationMessage
├── responses.py                        ← QueryResponse, ProductCard, CategoryResult
└── internal.py                         ← Internal types not exposed to API consumers
```

---

### Gagan & Bhagyaraj — Documentation & Testing Engineers

**Responsibilities**

**Gagan** — Lead on integration testing and test infrastructure
- Owns `tests/conftest.py`: shared pytest fixtures, mock API clients, and test app setup
- Implements integration test suite: `test_query_flow.py` (full vague → follow-up → recommendations flow) and `test_health.py`
- Ensures integration tests cover the error catalog scenarios from §10.2

**Bhagyaraj** — Lead on unit testing
- Implements unit tests for all individual service and utility modules: `test_vagueness.py`, `test_recommender.py`, `test_products.py`, `test_ranking.py`, `test_prompts.py`, `test_validators.py`, `test_formatters.py`
- Ensures test coverage meets project standards before each release

**Shared**
- Both members maintain the `tests/` directory structure
- Responsible for keeping the test suite passing in CI on every PR
- Maintain and update this SDD and any supplementary technical documentation as the project evolves

**Owned Modules**

```
tests/                                  ← Full test suite (Gagan & Bhagyaraj shared ownership)
├── __init__.py                         ← [Gagan]
├── conftest.py                         ← [Gagan] Shared pytest fixtures, mock clients, test app setup
│
├── unit/                               ← [Bhagyaraj] Unit tests
│   ├── __init__.py
│   ├── test_vagueness.py               ← Tests for vagueness service
│   ├── test_recommender.py             ← Tests for recommender service
│   ├── test_products.py                ← Tests for products service
│   ├── test_ranking.py                 ← Tests for ranking service
│   ├── test_prompts.py                 ← Tests for all prompt templates
│   ├── test_validators.py              ← Tests for input validators
│   └── test_formatters.py             ← Tests for response formatters
│
└── integration/                        ← [Gagan] Integration tests
    ├── __init__.py
    ├── test_query_flow.py              ← Full flow: vague → follow-up → recommendations
    └── test_health.py                  ← Health endpoint tests
```

---

### Sai Kumar — Utilities Engineer

**Responsibilities**
- Owns the `app/utils/` package: all stateless helper functions shared across services
- Implements `session.py`: the in-memory session store (Python dict keyed by `session_id`) with session creation, retrieval, update, and expiry logic (SESSION_EXPIRED after 30 minutes of inactivity)
- Implements `validators.py`: query length validation (minimum 3 characters, maximum 500 characters), HTML/script injection pattern detection, and conversation history length cap (20 messages)
- Implements `formatters.py`: response assembly helpers and affiliate URL tag injection for product links
- Coordinates with Vrandha on affiliate tag logic and with Aditi on input sanitization rules
- Responsible for keeping utility functions stateless, well-tested, and reusable across all services

**Owned Modules**

```
app/utils/                              ← Stateless helper functions
├── __init__.py
├── session.py                          ← In-memory session store (dict keyed by session_id)
├── validators.py                       ← Query length checks, injection pattern detection
└── formatters.py                       ← Response assembly, affiliate URL tagging
```

---

## 3. Team Responsibility Matrix

| Team Member | Role | Primary Domain | Modules Owned |
|---|---|---|---|
| **Varun K S** | Team Lead · UI/UX Design Lead | API Integration, UI/UX Design, Infrastructure, Project Leadership | `app/main.py`, `app/api/`, `main.py`, `Dockerfile`, `docker-compose.yml`, `requirements*.txt`, `.env.example`, `.gitignore`, `README.md`, `.github/workflows/` + UI/UX design system |
| **Sharan** | Frontend Engineer | React Implementation (per Varun's design direction) | Frontend application (React, Tailwind, Axios, React Router) |
| **Darshan** | Vagueness Service Engineer | Tier 1 AI Classification | `app/services/vagueness.py`, `app/prompts/vagueness_check.py`, `scripts/check_ollama.py` |
| **Lahari** | Recommendation & Cache Engineer | Tier 2 AI Orchestration, Redis Caching | `app/services/recommender.py`, `app/services/cache.py`, `app/prompts/intent_extraction.py` |
| **Sharanu** | Ranking Service Engineer | GPT-4o Product Ranking | `app/services/ranking.py`, `app/prompts/product_ranking.py` |
| **Vrandha** | Product/Data & Prompt Engineer | SerpAPI Integration, Prompt Quality | `app/services/products.py`, `scripts/mock_serp.py`, `scripts/test_prompt.py` |
| **Aditi** | Core & Config Engineer | App Config, Security, Middleware | `app/core/` (all files) |
| **Sujay** | Models & Validation Engineer | Pydantic Schemas, API Contracts | `app/models/` (all files) |
| **Gagan** | Testing Engineer (Integration) | Integration Tests, Test Infrastructure | `tests/conftest.py`, `tests/integration/` |
| **Bhagyaraj** | Testing Engineer (Unit) | Unit Tests, Documentation | `tests/unit/` |
| **Sai Kumar** | Utilities Engineer | Session Store, Validators, Formatters | `app/utils/` (all files) |

---

## 4. Integration Ownership

**Varun K S** owns the seams between all modules. Specifically:

**Frontend Design Oversight**
While Sharan owns the frontend codebase, Varun owns all visual and UX decisions — colour palette, typography, font choices, spacing, component aesthetics, and interaction patterns. Sharan implements; Varun directs and approves. No frontend PR that changes visual design is merged without Varun's sign-off.

**Control Flow Orchestration**
The end-to-end query lifecycle — from `POST /v1/query` arriving at `app/api/v1/query.py`, through vagueness check (Darshan's service), context assembly, recommendation generation (Lahari's service), product fetch (Vrandha's service), and ranking (Sharanu's service) — is wired together by Varun in the route handler and the `deps.py` dependency injection layer.

**External API Integration**
Varun holds final ownership of the three external API integrations at the contract level: OpenAI (GPT-4o / GPT-4o-mini), Ollama, and SerpAPI. While each service module implements calls to these APIs, Varun ensures authentication, error handling, rate limit configuration, and fallback routing are consistently applied across all three.

**GitHub Organisation & Repository Governance**
- GitHub organisation ownership and team permissions
- Branch protection rules on `main` (require PR, require passing CI)
- PR review and final merge authority
- Release tagging and version management

**Cross-Service Coordination**
When a change in one module affects another (e.g., a schema change in `models/responses.py` rippling into `ranking.py` and the frontend), Varun coordinates the cross-team communication and ensures all impacted modules are updated together in a single coherent PR.

**CI/CD Pipeline**
- `ci.yml`: runs linting and full test suite on every pull request
- `deploy.yml`: auto-deploys to Railway on merge to `main`

---

*Document maintained by Varun K S (Team Lead). Update this file whenever new modules are added or ownership changes.*
*RecommendMe · v1.0 · March 2026 · Internal / Confidential*
