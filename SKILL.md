---
name: journalist-query-respond
description: Draft KDCI.ai journalist/HARO-style press query responses in Emman Umali's real voice, grounded in his own stated positions in sme-brain/. First run conducts a one-time bootstrap interview to seed the knowledge base — do not skip it, and do not draft real queries against an unbootstrapped brain. Trigger with "/journalist-query-respond", "answer this journalist query", "draft a HARO response", "respond to this press query", or paste a journalist query directly.
---

# Journalist Query Responder

Drafting-only engine for KDCI.ai journalist/press queries. Reads an inbound query, drafts a response grounded in `sme-brain/` (company facts + Emman's real positions and voice), and hands back a draft for Emman to personally review, edit, and send. **Never sends anything. Never invents a stat, anecdote, or opinion the brain doesn't actually have** — see `CLAUDE.md` for the full rule set.

## Step 0 — First-run setup (self-install)

Run this check before anything else, every invocation.

1. **Self-install as a slash command.** This skill only becomes a real `/journalist-query-respond` command when `SKILL.md` sits under `.claude/skills/journalist-query-respond/` (project-level) or `~/.claude/skills/journalist-query-respond/` (user-level) — **and `sme-brain/` must travel with it**, copied into that same folder (`.claude/skills/journalist-query-respond/sme-brain/`). Unlike a stateless skill, this one's whole value is the brain — installing the SKILL.md without it produces a drafting engine with nothing to draft from.
   - If this run started from anywhere else (a fresh `git clone`, pasted content, a README), detect it and offer to install: **this project only** or **every project**.
   - Copy `SKILL.md` + the entire `sme-brain/` folder (all 4 files) to the chosen location.
   - Tell the user to reload/restart and `/journalist-query-respond` will be a direct slash command from then on. This run continues inline regardless.
   - If already correctly installed with `sme-brain/` present alongside it, skip silently.

2. Read `CLAUDE.md` in this same folder for the hard rules (no fabrication, mandatory human sign-off, never auto-send) if not already loaded this session.

## Step 1 — Bootstrap check (mandatory gate before any drafting)

Read the status line at the top of `sme-brain/sme-persona.md`.

- Starts with **`Status: BOOTSTRAPPED: <date>`** → the brain has real content. Skip straight to Step 2.
- Anything else (`NOT YET BOOTSTRAPPED`, `IDENTITY FILLED, INTERVIEW NOT YET RUN`, etc.) → **this is a first run.** Conduct the bootstrap interview now (below). Do **not** proceed to drafting a real query on an unbootstrapped brain — if the user pastes a query before the interview is done, say so plainly and offer to run the interview first.

### Conducting the bootstrap interview

1. Read `sme-brain/bootstrap-interview.md` in full — it has the actual question set, tailored to Emman's real specialty (production LLM/agent engineering, anti-hallucination, MLOps, computer vision, AI enablement), grouped into 4 clusters.
2. Ask the clusters **one at a time, in normal conversation** — not as a multiple-choice tool, since these need real, unscripted answers. Present a cluster's questions together, wait for Emman's actual reply, and follow up briefly if an answer is thin ("can you give a concrete example?") before moving to the next cluster. Do not dump all 14 questions into one message — it produces rushed, shallow answers, which defeats the entire point of this file.
3. If Emman explicitly defers or declines a question, record **"deferred"** — never invent an answer to fill the gap, including during bootstrap.
4. Hold answers in working memory through all 4 clusters; don't write partial files mid-interview.
5. Once all clusters are done (or explicitly deferred), write the results:
   - **`sme-brain/sme-persona.md`** — replace the `[LOW CONFIDENCE...]` tone/voice section with Cluster C's real answers. Fill any remaining Identity gaps from Cluster D (which credibility angle leads by default). Add anything real mentioned (Slack explanations, talks, the Q1 paper) to Writing Samples.
   - **`sme-brain/topics-position-bank.md`** — one new `## Topic:` entry per Cluster A/B question that got a real answer, in Emman's own words for Position and Reasoning, `Source: bootstrap interview, <date>`.
   - **`sme-brain/kdci-ai-profile.md`** — update the "not KDCI's lane" list per Cluster B's boundary answers, and explicitly resolve the AI-avatar fit-question (Cluster A Q8) — either move it into expertise areas with whatever angle Emman gave, or confirm it stays excluded.
   - Mark `sme-brain/bootstrap-interview.md`'s top line `bootstrapped: <date>` and `sme-brain/sme-persona.md`'s status line `Status: BOOTSTRAPPED: <date>`.
6. Report back a short summary of what got written where, and confirm the brain is now ready for real queries.

## Step 2 — Drafting pipeline (not yet built)

This is plan step 3 in `SPEC.md` — **not implemented as an automated pipeline yet.** If the brain is bootstrapped and the user pastes a real journalist query, say so plainly rather than attempting a half-built pipeline silently: the knowledge base is ready, but the full decompose → retrieve → gap-check → draft → flag automation described in `SPEC.md` §3 hasn't been coded into this skill as a repeatable step yet. Offer to either (a) draft this one response by hand right now, applying `SPEC.md` §3's logic manually and reading straight from `sme-brain/`, or (b) build Step 2 out properly first so it's repeatable. Either way, the hard rules below still apply in full.

## Hard rules (from `CLAUDE.md` — never violate)

1. **No fabrication, ever.** No grounded position/stat/anecdote for a sub-question → say so (`[SME INPUT NEEDED: ...]`), never invent one.
2. **Mandatory human sign-off, every single draft.** No batch approval. Emman personally reviews and edits before anything is sent.
3. **Never auto-send.** Output is always a draft document; sending is a human action through whatever channel the query arrived on.
4. **The brain is living.** Every real, SME-approved response that goes out gets folded back into `topics-position-bank.md` and `sme-persona.md`'s calibration log.
5. **Voice from real material only** — never approximate from a personality description.

## Files

- `CLAUDE.md` — what/why/rules.
- `SPEC.md` — full design spec, pipeline detail, schemas, plan, progress log. Append a Progress Log entry after any significant run of this skill (bootstrap or drafting).
- `sme-brain/bootstrap-interview.md` — the first-run interview script (this file drives Step 1).
- `sme-brain/sme-persona.md` — identity, voice/tone, writing samples, calibration log.
- `sme-brain/kdci-ai-profile.md` — company facts, positioning, expertise/not-my-lane lists.
- `sme-brain/topics-position-bank.md` — Emman's real, stated positions — the highest-leverage file in the system.
