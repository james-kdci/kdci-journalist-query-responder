# Journalist Query Responder Project

## What this is

An AI-assisted **drafting engine** for answering journalist / HARO-style editorial queries as KDCI.ai's SME. It reads an inbound query, drafts a response grounded in a persistent "brain" (company facts + the SME's real positions and voice), and hands the SME a draft to personally review, edit, and send. **It does not send anything, and it does not invent anything the brain doesn't actually know.**

This lives alongside the AI Link Building Prospecter Project (same parent folder) because both are earned-media / domain-authority plays — but the mechanism is fundamentally different, so it's a separate skill with its own rules, not a third "Build."

## Why it exists

- **Journalist queries are high-value earned media** — a used quote in a real outlet is worth more than a directory listing, but arrives on a tight deadline and under a real person's name.
- **Drafting is the automatable slice.** Reading a query, pulling the SME's relevant known positions, and producing a first-pass draft in their voice is where AI saves real time. Sending, editing for accuracy, and owning what gets attributed to the SME is not automatable — and shouldn't be.
- **Brand scope (confirmed):** `kdci.ai` only for now. `kdci.co` may be added later if the SME starts fielding outsourcing/BPO press queries too.

## Core rules (do not violate)

- **No fabrication, ever.** If the brain has no real, grounded position, stat, or anecdote for a sub-question, the draft says so explicitly (`[SME INPUT NEEDED: ...]`) instead of inventing one. This is stricter than the parent project's "no fabrication" rule — there, a bad row gets dropped; here, a fabricated claim goes out under a real person's byline.
- **Mandatory human sign-off, every single time.** No batch approval, no "looks fine, ship it." The SME personally reviews and edits every draft before anything is sent. This is a per-item gate, not a per-run gate (unlike Build A/B's batch approval).
- **Never auto-send.** Output is always a draft document. Sending happens through whatever channel the query arrived on (email, HARO/Connectively/Qwoted platform, PR agency), done by a human.
- **The brain is living, not static.** Every real, SME-approved response that goes out gets fed back into the position bank and persona file. This is the mechanism that fixes the cold-start problem (see SPEC §6) — the brain gets less generic and less dependent on flags every cycle.
- **Voice comes from real material only.** Never approximate the SME's voice from a personality description — only from actual writing samples, past quotes, or SME-edited drafts that accumulate over time.

## Files

- `SPEC.md` — the how: brain structure, pipeline, schemas, plan, open decisions, progress log. **Read it before building anything, and update its Progress Log at the end of every significant session.**
- `sme-brain/` — the persistent knowledge base (see SPEC §2 for what goes in each file).
- `runs/` — per-query drafts and the response log (created once the pipeline runs for real).

## Relationship to the parent project

Shares the folder and the underlying philosophy (draft/prospect only, no fabrication, human acts on it) with the AI Link Building Prospecter Project, but does **not** share its ledger, its Apify/Semrush stack, or its authority-scoring logic — those don't apply here. Treat this as a sibling project that happens to live in the same workspace folder, not a third build of the same pipeline.

## Working guidelines

Follow `/karpathy-guidelines`: simplest thing that works, surgical changes, state assumptions, define verifiable success criteria. Don't build the skill (plan step 3) before the persona bootstrap (plan step 1) is done — a drafting engine with an empty brain just produces generic thought-leadership, which defeats the purpose.
