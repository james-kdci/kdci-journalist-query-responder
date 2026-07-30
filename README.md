# KDCI Journalist Query Responder

An AI-assisted **drafting engine** for answering journalist / HARO-style editorial queries as KDCI.ai's SME. It reads an inbound query, drafts a response grounded in a persistent "brain" (company facts + the SME's real positions and voice), and hands the SME a draft to personally review, edit, and send.

**It does not send anything, and it does not invent anything the brain doesn't actually know.** Every draft goes through the SME before it reaches a journalist.

## Status: runnable first-run interview; drafting pipeline not yet built

`SKILL.md` is a real, installable skill (`/journalist-query-respond`) — but right now it only implements self-install and the **first-run bootstrap interview**. That's deliberate — building the drafting pipeline against an empty brain just produces generic AI thought-leadership, which defeats the point. Read `SPEC.md` §8 (Plan) for the full build sequence; short version:

1. **Clone this repo and run `/journalist-query-respond` (or ask Claude Code to follow `SKILL.md`).** First run detects the brain isn't bootstrapped yet and conducts the interview in `sme-brain/bootstrap-interview.md` conversationally with Emman — one cluster of questions at a time, real answers only, nothing invented. It writes the results straight into `sme-brain/sme-persona.md` (voice, tone, credibility framing) and `sme-brain/topics-position-bank.md` (his real, stated positions).
2. Seed/confirm the company profile — `sme-brain/kdci-ai-profile.md` is already drafted from KDCI.ai's real positioning; worth a quick SME read-through for accuracy.
3. **Not yet built:** the actual drafting pipeline (decompose → retrieve → gap-check → draft → flag) that runs once the brain has real content. `SKILL.md` explicitly stubs this rather than half-implementing it.
4. Run it for real on actual journalist queries, and keep feeding real responses back into the brain so it gets sharper over time.

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
