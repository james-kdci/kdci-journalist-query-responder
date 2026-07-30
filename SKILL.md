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

- Starts with **`Status: BOOTSTRAPPED: <date>`** → the brain has real content. Note any "still open" caveat on that line (e.g. specific clusters not yet covered) and treat those as light follow-ups, not blockers — then skip to Step 2.
- Anything else (`NOT YET BOOTSTRAPPED`, `IDENTITY FILLED, INTERVIEW NOT YET RUN`, etc.) → **this is a first run.** Bootstrap now, via whichever path fits (below). Do **not** proceed to drafting a real query on an unbootstrapped brain — if the user pastes a query before bootstrap is done, say so plainly and offer to bootstrap first.

There are two valid ways to bootstrap — use whichever the SME actually gives you:

### Path A — Conversational interview

1. Read `sme-brain/bootstrap-interview.md` in full — it has the question set, tailored to Emman's real specialty, grouped into 4 clusters.
2. Ask the clusters **one at a time, in normal conversation** — not as a multiple-choice tool, since these need real, unscripted answers. Present a cluster's questions together, wait for Emman's actual reply, and follow up briefly if an answer is thin ("can you give a concrete example?") before moving to the next cluster. Do not dump all 14 questions into one message — it produces rushed, shallow answers, which defeats the entire point of this file.
3. If Emman explicitly defers or declines a question, record **"deferred"** — never invent an answer to fill the gap, including during bootstrap.
4. Hold answers in working memory through all 4 clusters; don't write partial files mid-interview.

### Path B — Real material supplied directly (e.g. sample responses, past quotes, articles)

If the SME hands over real written material instead of doing a live interview — sample answers to example queries, past press quotes, LinkedIn posts, transcripts — that's equally valid, often richer, source material. Process it the same way an interview's answers get processed:
1. Save the raw material verbatim, unedited, under `sme-brain/writing-samples/<date>-<short-label>.md` — this is the ground truth to re-derive from later, never overwrite or summarize over it.
2. Distill it into the same destinations as Path A: real positions with reasoning into `topics-position-bank.md` (cite the source file + date), concrete tone/voice cues actually observable in the writing into `sme-persona.md` (cite specific phrases/patterns as evidence, don't just assert a tone), and any lane/boundary implications into `kdci-ai-profile.md`.
3. Real material like this typically only covers some clusters (usually A/C — technical stances and voice show through naturally; B/D — explicit boundaries and default credibility framing — usually don't, since nobody volunteers "topics I'd avoid" unprompted). That's fine: mark the file **partially bootstrapped**, explicitly listing which clusters are still open, rather than either blocking on the gap or silently treating it as fully resolved.

### After either path — write the results

- **`sme-brain/sme-persona.md`** — tone/voice notes (with evidence, not assertion), Identity gaps if Cluster D was covered, Writing Samples pointing at the source file(s).
- **`sme-brain/topics-position-bank.md`** — one new `## Topic:` entry per real position surfaced, in the SME's own words for Position and Reasoning, with a traceable `Source`.
- **`sme-brain/kdci-ai-profile.md`** — update "not KDCI's lane" per whatever boundaries or demonstrated fit surfaced (e.g. a topic once assumed out-of-lane may turn out to be in-lane if the SME answers it substantively — move it, cite why).
- Update `sme-brain/sme-persona.md`'s status line to `Status: BOOTSTRAPPED: <date>`, noting which path was used and which clusters (if any) remain open.
- Report back a short summary of what got written where, and confirm the brain is now ready for real queries (or still partially open, and on what).

## Step 2 — Drafting pipeline

Runs once the brain is bootstrapped (Step 1) and the user pastes a real journalist query. Implements `SPEC.md` §3 as concrete, repeatable substeps.

### 2.1 — Relevance/legitimacy gate
Check the query's topic(s) against `sme-brain/kdci-ai-profile.md`'s expertise-areas list and "not KDCI's lane" list. If it's a clean fit, proceed. If it's clearly outside both the expertise list and any demonstrated adjacent angle (see how the AI-avatar topic got reclassified — check for a real angle before assuming a decline), stop and tell the user plainly: this looks outside Emman's lane, confirm before drafting anything. Don't spend a full draft on a bad fit.

### 2.2 — Decompose
Read the query text and list its discrete sub-questions/angles explicitly, numbered, before drafting anything. A query that reads as one paragraph often has 3-6 real sub-questions bundled in it (query 2 from the launch examples had 6).

### 2.3 — Retrieve
For each numbered sub-question, search `sme-brain/topics-position-bank.md` for a matching `## Topic:` entry, `sme-brain/kdci-ai-profile.md` for relevant company facts/proof points, and `sme-brain/writing-samples/` for phrasing/voice precedent on this exact topic if it exists. Note which source(s) matched per sub-question, or "no match."

### 2.4 — Gap-check
Classify every sub-question as:
- **GROUNDED** — a real position bank entry, proof point, or writing-sample precedent exists.
- **UNGROUNDED** — nothing in the brain addresses it.

This classification must be shown in the output (Step 2.6), not just used silently — it's the core audit trail against fabrication.

### 2.5 — Draft
Write the response, sub-question by sub-question:
- **Grounded** → draft from the matched `topics-position-bank.md` entry's Position + Reasoning, adapted into natural flowing prose (not pasted verbatim as bullet points) and matched to the tone/voice cues in `sme-persona.md` (balanced framing, closes with a clean reduction, sparing first-person conviction, names companies/tools directly, metrics over vague enthusiasm, jargon translated to plain English). Do not add any claim, number, or example beyond what the position-bank entry actually says.
- **Ungrounded** → insert `[SME INPUT NEEDED: <one-line description of what's missing>]` inline where that sub-answer would go. Never fill it with an invented stat, anecdote, or opinion — this is the single most important rule in this entire skill.
- Match the query's requested format/length if stated (word count, quote-only vs. full commentary); if unstated, default to a length proportional to how many sub-questions were asked.

### 2.6 — Output
Write two files under `runs/<YYYY-MM-DD>/<query-slug>/`:
- `query.md` — the original inbound query, verbatim, plus which platform it came from if known (HARO/Connectively/SOS/Qwoted).
- `draft.md` — the drafted response, followed by a **Flags** section listing every `[SME INPUT NEEDED]` placeholder plus a one-line **Grounding summary** (`N of M sub-questions grounded`).

### 2.7 — Human gate (mandatory, no exceptions)
Present the draft to the user in-chat. State plainly: this is a draft only — Emman must personally review, edit, and send it himself through whichever platform the query came from. Do not represent it as final or ready-to-send.

### 2.8 — Feedback capture (after Emman actually sends something)
Once Emman has sent his real, edited final version (not this draft), fold it back in:
- Any sub-question where his final differs meaningfully from the draft → note it in `sme-brain/sme-persona.md`'s calibration log (draft vs. final, what changed, pattern).
- Any genuinely new position that emerged in his edit → new `## Topic:` entry in `topics-position-bank.md`, `Source: response to <query slug>, <date>`.
This step only fires on a real sent response — never on an unreviewed draft.

### 2.9 — Log
Append a row to `runs/response_log.csv` (create with header if missing): `date_received,outlet_or_reporter,topic_tags,sub_questions_count,grounded_count,ungrounded_flagged_count,draft_status,published_url,notes`. Set `draft_status=drafted` at this point; the human updates it to `sme_edited`/`sent`/`published`/`declined` later.

## Hard rules (from `CLAUDE.md` — never violate)

1. **No fabrication, ever.** No grounded position/stat/anecdote for a sub-question → say so (`[SME INPUT NEEDED: ...]`), never invent one.
2. **Mandatory human sign-off, every single draft.** No batch approval. Emman personally reviews and edits before anything is sent.
3. **Never auto-send.** Output is always a draft document; sending is a human action through whatever channel the query arrived on.
4. **The brain is living.** Every real, SME-approved response that goes out gets folded back into `topics-position-bank.md` and `sme-persona.md`'s calibration log.
5. **Voice from real material only** — never approximate from a personality description.

## Files

- `CLAUDE.md` — what/why/rules.
- `SPEC.md` — full design spec, pipeline detail, schemas, plan, progress log. Append a Progress Log entry after any significant run of this skill (bootstrap or drafting).
- `sme-brain/bootstrap-interview.md` — the first-run interview script (Path A in Step 1).
- `sme-brain/sme-persona.md` — identity, voice/tone, writing samples pointer, calibration log.
- `sme-brain/kdci-ai-profile.md` — company facts, positioning, expertise/not-my-lane lists, confirmed intake channels.
- `sme-brain/topics-position-bank.md` — Emman's real, stated positions — the highest-leverage file in the system.
- `sme-brain/writing-samples/` — raw, unedited real material (sample responses, quotes, transcripts) that `sme-persona.md` and `topics-position-bank.md` are distilled from. Never overwrite; append new dated files as more material comes in.
