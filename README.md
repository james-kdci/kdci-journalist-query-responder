# KDCI Journalist Query Responder

An AI-assisted **drafting engine** for answering journalist / HARO-style editorial queries as KDCI.ai's SME. It reads an inbound query, drafts a response grounded in a persistent "brain" (company facts + the SME's real positions and voice), and hands the SME a draft to personally review, edit, and send.

**It does not send anything, and it does not invent anything the brain doesn't actually know.** Every draft goes through the SME before it reaches a journalist.

## Status: spec + brain, skill not yet built

This repo is currently the **design spec and the SME's knowledge base**, not yet a runnable automation. That's deliberate — building the drafting pipeline against an empty brain just produces generic AI thought-leadership, which defeats the point. Read `SPEC.md` §8 (Plan) for the full build sequence; short version:

1. **Bootstrap the persona** (next step, not yet done) — run `sme-brain/bootstrap-interview.md` with Emman once, in conversation. Answers get written into `sme-brain/sme-persona.md` (voice, tone, credibility framing) and `sme-brain/topics-position-bank.md` (his real, stated positions on recurring topics).
2. Seed/confirm the company profile — `sme-brain/kdci-ai-profile.md` is already drafted from KDCI.ai's real positioning; worth a quick SME read-through for accuracy.
3. Build the actual drafting skill against the now-populated brain, test on real example queries.
4. Package it as a self-contained, runnable skill.
5. Run it for real, and keep feeding real responses back into the brain so it gets sharper over time.

## Start here

- **`CLAUDE.md`** — what this is, why it exists, and the non-negotiable rules (no fabrication, mandatory human sign-off, never auto-send).
- **`SPEC.md`** — the full design: how the "brain" is structured, the 10-step drafting pipeline, output schemas, and the build plan above in detail.
- **`sme-brain/`** — the knowledge base:
  - `bootstrap-interview.md` — **run this first.** The interview script that turns this from a cold-start template into something grounded in Emman's real opinions and voice.
  - `sme-persona.md` — who's speaking (identity filled in; voice/tone still pending the interview).
  - `kdci-ai-profile.md` — KDCI.ai's company facts, positioning, and what is/isn't Emman's actual lane to speak on.
  - `topics-position-bank.md` — Emman's real, stated positions on recurring topics (empty until the interview runs — this file is the single highest-leverage piece of the whole system).

## Why the brain matters more than the pipeline

The hard part of this project was never "can an LLM draft a press response" — it's whether the response sounds like a specific person with real, defensible opinions, or like generic AI-company boilerplate that any editor filters out immediately. That's why the knowledge base gets built and populated with real material *before* any drafting automation, and why the pipeline is designed to explicitly flag (never fabricate) anything the brain doesn't actually know.

## License

MIT — see `LICENSE`.
