# KDCI Journalist Query Responder

An AI-assisted **drafting engine** for answering journalist / HARO-style editorial queries as KDCI.ai's SME. It reads an inbound query, drafts a response grounded in a persistent "brain" (company facts + the SME's real positions and voice), and hands the SME a draft to personally review, edit, and send.

**It does not send anything, and it does not invent anything the brain doesn't actually know.** Every draft goes through the SME before it reaches a journalist.

## Status: brain populated with real material; drafting pipeline not yet built

`SKILL.md` is a real, installable skill (`/journalist-query-respond`). The brain is no longer cold-start: Emman supplied real answers to all 3 launch example queries directly, and `sme-brain/` has been populated from that material — 16 real, traceable positions in `topics-position-bank.md`, evidence-backed tone/voice notes in `sme-persona.md`, and the raw source preserved in `sme-brain/writing-samples/`. Two clusters (explicit boundaries, default credibility framing) are still open but non-blocking — see `sme-persona.md`'s status line.

What's still deliberately not built: the actual drafting pipeline (decompose → retrieve → gap-check → draft → flag) that runs once a real query comes in. Building that against an empty brain would've produced generic AI thought-leadership; now that the brain has real content, that's the natural next step. Read `SPEC.md` §8 (Plan) for the full build sequence; short version:

1. ~~Bootstrap the brain~~ — done, via real sample material rather than a live interview (`SKILL.md` Step 1 supports either path, and will run the live interview automatically for a fresh install with no existing material).
2. Company profile — `sme-brain/kdci-ai-profile.md` is drafted from KDCI.ai's real positioning and Emman's confirmed expertise; worth a periodic SME read-through for accuracy.
3. **Not yet built:** the drafting pipeline itself. `SKILL.md` explicitly stubs this rather than half-implementing it.
4. Run it for real on actual journalist queries (via HARO, Connectively, SOS, or Qwoted), and keep feeding real responses back into the brain so it gets sharper over time.

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
