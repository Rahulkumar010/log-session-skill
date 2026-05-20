# Examples

Real-world examples of `log-session` input triggers and output.

---

## Example 1 — End-of-day handoff

**Trigger phrase**

> "Okay I think that's good enough for tonight. Can you log this session so I can pick it up tomorrow?"

**Generated output (abbreviated)**

```markdown
# Session Summary

* **Goal:** Set up authentication middleware for the Express API
* **Outcome:** JWT verification middleware implemented and tested; refresh token endpoint pending
* **Status:** partial

---

# Context

* **Project:** express-api-starter
* **Files involved:** `src/middleware/auth.ts`, `src/routes/auth.ts`
* **Technologies:** Node.js, Express, TypeScript, jsonwebtoken

---

# Steps Taken

1. Reviewed existing route structure to identify where auth middleware should attach
2. Implemented `verifyToken` middleware with error handling for expired and malformed tokens
3. Registered middleware on protected routes in `src/routes/index.ts`
4. Wrote manual curl tests to verify 401 responses on missing/invalid tokens

---

# Key Decisions

* **Decision:** Used RS256 over HS256 for token signing
  **Reason:** Public/private key pair required for multi-service verification without sharing secrets

---

# Next Steps

* [ ] Implement refresh token endpoint
* [ ] Add token rotation logic
* [ ] Write integration tests for auth flow

---

# Bootstrap Prompt

> "I'm continuing work on express-api-starter. Last session I built JWT
> verification middleware. Status: partial. Next I need to implement the
> refresh token endpoint. Stack: Node/Express/TypeScript, files:
> src/middleware/auth.ts, src/routes/auth.ts. Please help me continue."
```

---

## Example 2 — Research session with no code

**Trigger phrase**

> "Save session — I want to remember what we found about vector DB options."

**Generated output (abbreviated)**

```markdown
# Session Summary

* **Goal:** Evaluate vector database options for a RAG pipeline
* **Outcome:** Compared Pinecone, Weaviate, and pgvector across latency, cost, and ops complexity
* **Status:** completed

---

# Context

* **Project:** not specified
* **Files involved:** none identified
* **Technologies:** Python, LangChain (planned)

---

# Steps Taken

1. Defined selection criteria: query latency < 50ms, managed hosting preferred, cost < $200/mo at 10M vectors
2. Evaluated Pinecone — managed, fast, but expensive at scale
3. Evaluated Weaviate — open-source with managed tier, flexible schema
4. Evaluated pgvector — lowest ops overhead if already on Postgres, limited ANN performance

---

# Key Decisions

* **Decision:** Shortlisted pgvector for MVP, Weaviate for scale
  **Reason:** pgvector avoids new infrastructure; Weaviate is the upgrade path if latency becomes a constraint

---

# Code Changes

No code changes in this session.

---

# Next Steps

* [ ] Run latency benchmarks with pgvector on target dataset size
* [ ] Price out Weaviate Cloud at projected vector count
* [ ] Prototype retrieval chain with LangChain + pgvector

---

# Bootstrap Prompt

> "I'm continuing a vector DB evaluation for a RAG pipeline. Last session I compared
> Pinecone, Weaviate, and pgvector — shortlisted pgvector for MVP and Weaviate for scale.
> Status: completed research, next is benchmarking. Please help me set up latency tests
> with pgvector."
```

---

## Example 3 — Very short session

**Trigger phrase**

> "Actually I need to run — log this."

**Generated output**

```markdown
# Session Summary

* **Goal:** Debug failing CI pipeline on the data-ingestion repo
* **Outcome:** Identified likely cause (missing env var in CI config); fix not yet applied
* **Status:** partial

---

# Context

* **Project:** data-ingestion
* **Files involved:** `.github/workflows/ci.yml`
* **Technologies:** GitHub Actions, Python

---

# Steps Taken

1. Reviewed CI failure logs — error traced to missing `DB_CONNECTION_STRING` env var
2. Confirmed var is set locally but not in GitHub Secrets

---

# Next Steps

* [ ] Add `DB_CONNECTION_STRING` to GitHub Secrets
* [ ] Re-run CI and confirm green

---

# Bootstrap Prompt

> "I'm continuing debug work on data-ingestion CI. Last session I found the failure is
> caused by a missing `DB_CONNECTION_STRING` GitHub Secret. Status: partial. Next I need
> to add the secret and re-run CI. File: .github/workflows/ci.yml. Please help me continue."
```

---

## Trigger phrase reference

| Phrase | Behaviour |
|---|---|
| `/log-session` | Explicit invocation — always triggers |
| `save session` | Explicit — always triggers |
| `capture context` | Explicit — always triggers |
| `log this session` | Explicit — always triggers |
| `let's pick this up tomorrow` | Auto-detected — triggers log |
| `I need to take a break` | Auto-detected — triggers log |
| `summarize what we did` | Intent-checked — triggers if user wants to reuse output |
| `what did we accomplish?` | Intent-checked — triggers near end of session |
