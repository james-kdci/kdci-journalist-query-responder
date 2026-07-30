# KDCI Journalist Query Responder

An AI-assisted **drafting engine** for answering journalist / HARO-style editorial queries as KDCI.ai's SME. It reads an inbound query, drafts a response grounded in a persistent "brain" (company facts + the SME's real positions and voice), and hands the SME a draft to personally review, edit, and send.

**It does not send anything, and it does not invent anything the brain doesn't actually know.** Every draft goes through the SME before it reaches a journalist.

## Status: pipeline built and mechanics-tested; not yet proven on a real, unseen query

`SKILL.md` is a real, installable skill (`/journalist-query-respond`) with both halves working: the brain is populated with real material (16 traceable positions in `topics-position-bank.md`, evidence-backed voice notes in `sme-persona.md`, raw source in `sme-brain/writing-samples/`), and Step 2 (the drafting pipeline — relevance gate → decompose → retrieve → gap-check → draft → output → human gate → feedback capture → log) is fully written and test-run against all 3 launch example queries — see `runs/2026-07-30/`.

**Read this caveat before trusting the test results:** those 3 queries are exactly what the brain was built from, so the test proves the pipeline's mechanics (no fabrication, correct sub-question grounding, proper output format) — it does **not** prove the pipeline generalizes to a genuinely new query the brain hasn't seen. Every test draft says so explicitly. Two persona clusters (explicit boundaries, default credibility framing) also remain open non-blocking — see `sme-persona.md`'s status line.

Read `SPEC.md` §8 (Plan) and its Progress Log for full detail; short version of where things stand:

1. ~~Bootstrap the brain~~ — done, via real sample material (`SKILL.md` Step 1 supports either a live interview or real material supplied directly).
2. Company profile — `sme-brain/kdci-ai-profile.md` is drafted from KDCI.ai's real positioning and Emman's confirmed expertise; worth a periodic SME read-through for accuracy.
3. ~~Build the drafting pipeline~~ — done and mechanics-tested (`SKILL.md` Step 2, `runs/2026-07-30/`).
4. **Next real test:** run it on a genuine, unseen journalist query (via HARO, Connectively, SOS, or Qwoted) to see how it performs when the brain doesn't already contain the answer.
5. Run it for real, and keep feeding real, sent responses back into the brain so it gets sharper over time (Step 2.8).

## Start here

- **`SKILL.md`** — the runnable skill. Install it (project- or user-level `.claude/skills/`) and it self-installs on first run, then walks straight into the bootstrap interview if the brain isn't seeded yet.
- **`CLAUDE.md`** — what this is, why it exists, and the non-negotiable rules (no fabrication, mandatory human sign-off, never auto-send).
- **`SPEC.md`** — the full design: how the "brain" is structured, the 10-step drafting pipeline, output schemas, and the build plan above in detail.
- **`sme-brain/`** — the knowledge base (this travels with the skill on install, not just `SKILL.md` itself):
  - `bootstrap-interview.md` — the interview script `SKILL.md` runs on first use. Turns this from a cold-start template into something grounded in Emman's real opinions and voice.
  - `sme-persona.md` — who's speaking (identity filled in; voice/tone/writing-samples populated by the interview).
  - `kdci-ai-profile.md` — KDCI.ai's company facts, positioning, and what is/isn't Emman's actual lane to speak on.
  - `topics-position-bank.md` — Emman's real, stated positions on recurring topics (empty until the interview runs — this file is the single highest-leverage piece of the whole system).

## Why the brain matters more than the pipeline

The hard part of this project was never "can an LLM draft a press response" — it's whether the response sounds like a specific person with real, defensible opinions, or like generic AI-company boilerplate that any editor filters out immediately. That's why the knowledge base gets built and populated with real material *before* any drafting automation, and why the pipeline is designed to explicitly flag (never fabricate) anything the brain doesn't actually know.

## License

MIT — see `LICENSE`.
