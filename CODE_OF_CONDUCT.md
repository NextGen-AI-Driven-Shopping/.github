# Code of Conduct

This document covers two things: how we treat each other, and how we work together as a team. Both matter equally. A respectful team that ships broken, undocumented code is just as problematic as a technically excellent team that is hostile to work with.

---

## Our Pledge

We are committed to making participation in this project a respectful, harassment-free experience for everyone — regardless of age, background, experience level, gender, nationality, or any other personal characteristic.

---

## Our Standards

### Expected Behavior

- Be respectful and constructive in all interactions
- Give and receive feedback graciously — critique the work, not the person
- Focus on what is best for the project and its users
- Acknowledge and learn from mistakes openly
- Ask questions early — silence is not a substitute for clarity

---

## Engineering Standards

These are not suggestions. Every team member is expected to follow these practices on every contribution. They exist to protect the team's time, keep the codebase healthy, and make collaboration possible across 11 people.

---

### 1. Always Read Before You Touch

Before writing a single line of code, read the relevant documentation.

**What to read:**
- `README.md` of the repo you are working in
- Any `ARCHITECTURE.md` or design document related to the feature
- The docstring or inline comments of any file you plan to modify
- The SDD section that covers your area of work

This is non-negotiable. Most bugs introduced in team projects come from someone assuming they understand how a system works without verifying. Two minutes of reading prevents two hours of debugging.

---

### 2. Never Work Outside Your Allocated Directory Without Permission

Every team member has an assigned ownership area (documented in the backend and frontend READMEs). This boundary exists to prevent conflicts, duplicate work, and unintentional breakage.

**The rule:**
- Do not modify files outside your assigned directory without explicit approval from the person who owns that area
- If you discover a bug outside your area, open an issue or flag it in discussion — do not fix it unilaterally
- If your work genuinely requires a change in another area, coordinate with that owner first, discuss the change, and let them implement it or explicitly hand off ownership

**Why this matters:**
On an 11-person team, uncoordinated edits to shared files are the single most common source of merge conflicts, regressions, and broken builds. Ownership boundaries only work if everyone respects them.

---

### 3. Always Pull Before You Start

Before beginning any work session, always sync with the latest state of the repository.

```bash
git checkout main
git pull origin main
git checkout your-branch
git rebase main       # or git merge main
```

**Never start work on a stale branch.** If you push code that was written against an outdated base, you risk:
- Conflicts that are painful to resolve
- Reintroducing bugs that were already fixed
- Overwriting someone else's work silently

If you have been away from the codebase for more than a day, pull before you write anything.

---

### 4. Write Detailed Commit Messages

A commit message is documentation. Future contributors — including yourself six months from now — will read your commits to understand what changed and why.

**Format:**

```
type: short summary in present tense (under 72 characters)

Body — explain WHAT changed and WHY. Not how (the code shows how).
Include context that the diff cannot show:
- What problem does this solve?
- Why was this approach chosen over alternatives?
- Any side effects or follow-up work needed?

Refs: #issue-number (if applicable)
```

**Types:**
```
feat:     new feature or capability
fix:      bug fix
refactor: code change that is not a feature or fix
docs:     documentation only
test:     adding or updating tests
chore:    dependencies, config, tooling
perf:     performance improvement
```

**Good example:**
```
feat: add Redis caching to SerpAPI product fetch

Previously every query triggered a fresh SerpAPI call even for
identical search terms. Popular queries (e.g. "trekking tent India")
were being fetched repeatedly at $0.002/call.

Added query hash as cache key with 6-hour TTL. Cache hit rate in
testing was ~40% for repeated category searches within a session.

Falls back to live fetch if Redis is unavailable — no hard dependency.

Refs: #34
```

**Bad examples (do not do this):**
```
fix stuff
wip
updated file
changes
asdfgh
```

A commit message that does not explain why a change was made has no value.

---

### 5. Write Detailed Pull Request Descriptions

A pull request is a formal proposal to change shared code. It needs to be reviewable by someone who was not involved in writing it. Vague PRs get rejected or merged with bugs.

**Required sections in every PR:**

---

**Summary**
What does this PR do? Two sentences maximum.

**Problem / Motivation**
What issue, bug, or need does this address? Link the issue if one exists.

**Changes Made**
A bullet list of every meaningful change. Be specific.
- Added `app/services/cache.py` — Redis get/set wrapper with TTL
- Modified `app/services/products.py` — checks cache before calling SerpAPI
- Updated `requirements.txt` — added `redis==5.0.1`

**How to Test**
Step-by-step instructions for a reviewer to verify the change works.
```
1. Start Redis locally: docker run -p 6379:6379 redis
2. Run the server: uvicorn main:app --reload
3. Send POST /v1/query with a trekking query
4. Send the same query again
5. Check server logs — second request should show "cache hit"
```

**Screenshots or Screen Recordings (required for any UI change)**
Before and after screenshots for every visual change. A reviewer should not have to run the code to understand what changed on screen. Annotate screenshots if needed to highlight what changed.

| Before | After |
|--------|-------|
| *(screenshot)* | *(screenshot)* |

**Does this break anything?**
Explicitly state if this change affects existing behavior, APIs, or other team members' areas. If it does, list what and who needs to know.

**Checklist**
- [ ] I have pulled from `main` and rebased before opening this PR
- [ ] I have read the documentation for every file I modified
- [ ] I have not modified files outside my allocated directory without approval
- [ ] All tests pass locally (`pytest` or `npm run lint`)
- [ ] I have added tests for new logic where applicable
- [ ] Screenshots or recordings are attached for any UI change
- [ ] The relevant issue is linked

---

## Scope

This Code of Conduct applies in all project spaces — GitHub issues, pull requests, discussions, commit messages, code reviews, and any other forum used by this organization.

---

## Enforcement

Instances of unacceptable behavior — whether interpersonal or engineering standards violations — can be reported to:

**Email:** teamlead@gmail.com

All reports will be reviewed and investigated. We will respond within 72 hours. The project maintainers are obligated to maintain confidentiality regarding the reporter.

Repeated violations of the engineering standards (committing without reading docs, working outside allocated directories, vague commit messages, PRs without descriptions) will be treated as a conduct issue after two warnings.

Maintainers who do not follow or enforce this Code of Conduct may face consequences as determined by other members of the organization's leadership.

---

## Attribution

The interpersonal conduct section is adapted from the [Contributor Covenant](https://www.contributor-covenant.org), version 2.1.
The engineering standards sections are original and specific to this team and project.
