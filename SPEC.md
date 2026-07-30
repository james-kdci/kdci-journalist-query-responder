# SPEC — Journalist Query Responder

The technical spec: brain structure, pipeline, schemas, plan, and progress. See `CLAUDE.md` for the what/why and the non-negotiable rules.

---

## 1. Why this isn't "Build C" of the parent project

The parent project (directory + editorial link prospecting) is a **discovery** problem: find many candidates, dedup, score, rank, output a list. This is a **drafting** problem: one query in, one high-stakes answer out, under a real person's name, usually on a deadline. No discovery fan-out, no authority scoring, no ledger-dedup of "candidates." The only thing carried over is the philosophy (AI does the labor, human owns the output, never fabricate) and the folder.

---

## 2. The brain (`sme-brain/`)

Three files, each with a different job and a different growth path:

### 2.1 `kdci-ai-profile.md` — company facts (mostly known today)
Seeded from the parent project's `kdci-profile.md` Brand B block (kdci.ai description, positioning, service lines, keywords), expanded with:
- Any real stats/proof points KDCI.ai can point to (e.g. specialist ramp time, replacement guarantee, industries served) — only ones already documented, nothing new invented here.
- A running "expertise areas" list — the topics KDCI.ai's SME is actually positioned to speak on (e.g. AI staffing/workforce augmentation, offshore AI operations, AI adoption for SMBs) vs. topics that are adjacent but not KDCI's lane (e.g. foundation-model research, chip supply chains) — used by the relevance gate (Step 1) to catch queries KDCI shouldn't weigh in on.

### 2.2 `sme-persona.md` — who's speaking (starts cold, per your answer)
Since there's no existing corpus yet, this file starts as a **bootstrap questionnaire artifact**, not a finished persona:
- Bio, title, credentials, years in AI/outsourcing, what makes them a credible voice on this beat.
- Tone/voice notes — but explicitly marked low-confidence until real writing samples or SME-edited drafts exist to back them up.
- A **writing samples** section — empty at start, filled in as real material appears (LinkedIn posts, interviews, bylined pieces, or just SME edits to early drafts).
- A **calibration log** — every early response logs draft-vs-SME-final as a diff/note, so voice drift gets corrected instead of repeated.

### 2.3 `topics-position-bank.md` — what they actually think (starts empty, grows forever)
A living list, one entry per recurring topic:
```
## Topic: AI agents vs. traditional automation
**Position:** <SME's real stated view, in their words>
**Reasoning:** <why — the actual argument, not marketing copy>
**Source:** <which response/date this came from>
**Last updated:** <date>
```
This is the single highest-leverage file in the whole system — it's what turns generic AI output into something that sounds like a specific person with a specific opinion. It is deliberately empty at launch. Seed topic categories to watch for (from the three example queries, expect more over time): AI agents vs. automation, big-tech AI competition (Microsoft/OpenAI/Google/Meta), AI ethics/adoption risk, digital legacy/AI avatars, AI staffing/workforce trends, AI in specific industries KDCI serves.

---

## 3. Pipeline

**Step 0 — Intake.** Manual paste of the query text (confirmed: no fixed platform yet — revisit if the SME adopts Connectively/Qwoted/a PR agency feed later).

**Step 1 — Relevance/legitimacy gate.** Fast judgment call: is this a real, on-topic-for-KDCI.ai query worth the SME's time (not spam, not wildly outside `kdci-ai-profile.md`'s expertise list)? If not, flag and stop — don't spend drafting effort on a bad fit.

**Step 2 — Decompose.** Break the query into its discrete sub-questions/angles (e.g. query #2 above is really 6 sub-questions, not one).

**Step 3 — Retrieve.** For each sub-question, pull matching material from `kdci-ai-profile.md` + `topics-position-bank.md` + any persona writing samples.

**Step 4 — Gap-check.** Classify each sub-question as **grounded** (a real position/stat/anecdote exists) or **ungrounded** (brain has nothing). This classification is the core safeguard — it's what stands between "drafted from real material" and "invented to sound complete."

**Step 5 — Draft.** Write the response in the SME's voice (per persona notes/samples), sub-question by sub-question:
- Grounded sub-questions → drafted fully from the brain.
- Ungrounded sub-questions → either an honestly-scoped shorter answer, or an explicit inline `[SME INPUT NEEDED: <what's missing>]` placeholder. **Never fabricate a stat, client story, or opinion to fill the gap.**

**Step 6 — Output.** A single draft doc: the response text + a flags section listing every `SME INPUT NEEDED` placeholder + a note on suggested length/format matching what the query asked for (word count, quote-only vs. full commentary, etc.).

**Step 7 — Human gate (mandatory, every time).** SME reviews, edits, and personally sends via whatever channel the query arrived on. No exceptions, no batch-approval shortcut — this is a per-item gate by design (see `CLAUDE.md`).

**Step 8 — Feedback capture.** Once the SME has sent (and/or the piece is published), the actually-sent version gets folded back in:
- New/updated entries in `topics-position-bank.md` for whatever topics it touched.
- A calibration note in `sme-persona.md` comparing the draft to the SME's final edit (what changed, why, tone patterns).
This is the mechanism that fixes cold start — each real response makes the next one less generic.

**Step 9 — Log.** Append a row to `runs/response_log.csv`.

---

## 4. Output artifacts

```
runs/<YYYY-MM-DD>/<query-slug>/
  query.md        # the original inbound query, verbatim
  draft.md        # the drafted response + flags section
response_log.csv  # one row per query, append-only
```

## 5. `response_log.csv` schema

`date_received, outlet_or_reporter, topic_tags, sub_questions_count, grounded_count, ungrounded_flagged_count, draft_status(drafted|sme_edited|sent|published|declined), published_url, notes`

---

## 6. Cold-start plan (no existing corpus — confirmed)

The brain starts empty on the two files that matter most (`sme-persona.md`, `topics-position-bank.md`). Expect early drafts to carry more flags and need heavier SME editing — that's the honest starting state, not a bug to hide.

1. **Bootstrap the persona.** Run a short structured interview with the SME (bio + tone + 5-8 recurring topics with their real opinion in their own words) to seed `sme-persona.md` and give `topics-position-bank.md` its first real entries, instead of launching on an empty file and hoping Step 8's feedback loop does all the work from zero.
2. **First 3-5 real queries** (the three already supplied are a good first batch) will surface which topics recur and which flags come up repeatedly — that's signal for what to prioritize asking the SME about next.
3. **Target quality curve:** not "perfect on query one" — expect the ratio of grounded-to-flagged sub-questions to climb steadily as `topics-position-bank.md` accumulates real entries over the first 5-10 real responses. Track this ratio in the response log; it's the system's actual quality metric.

---

## 7. Folder layout

```
Journalist Query Responder Project/
  CLAUDE.md
  SPEC.md
  sme-brain/
    kdci-ai-profile.md
    sme-persona.md
    topics-position-bank.md
  runs/
    <YYYY-MM-DD>/<query-slug>/
      query.md
      draft.md
    response_log.csv
```

---

## 8. Plan (sequenced, each step independently verifiable)

1. **Bootstrap persona** → run the structured interview with the SME → verify `sme-persona.md` has a real bio + tone notes + at least 5 topics with real stated positions (not placeholders), and `topics-position-bank.md` has those same 5+ entries.
2. **Seed the company profile** → pull `kdci-ai-profile.md` from the parent `kdci-profile.md` Brand B block + any additional stats/expertise-areas the SME supplies. *Verify: contains description, positioning, service lines, and an explicit expertise-areas list.*
3. **Build the skill** (draft pipeline, Steps 0-9 above) → test on the three example queries already supplied. *Verify: zero fabricated stats/claims across all three drafts; every ungrounded sub-question is flagged, not invented; output is reviewed against the persona for voice fit.* — **Partially built:** `SKILL.md` now implements Step 0 (self-install) and Step 1 (bootstrap-interview gate + execution) as a real, runnable skill. Step 2 (the actual decompose→retrieve→gap-check→draft pipeline) is explicitly stubbed — not yet coded — until bootstrap has run for real and there's a populated brain to test the drafting logic against.
4. **Package as a shareable skill** for the SME, mirroring the parent project's Build A/B precedent (self-contained `SKILL.md`, genericized, no hardcoded workspace path, asks for its own config on first run). *Verify: SME can run it standalone without this workspace's files.* — Self-install step written into `SKILL.md` (copies itself + all of `sme-brain/` together, since the brain is stateful and must travel with the skill, unlike Build A/B's stateless pattern).
5. **Run for real** on 5-10 actual queries, capture Step 8 feedback loop each time. *Verify: grounded/flagged ratio measurably improves by query #10 vs. query #1.*

Do **not** start step N+1 until step N verifies — in particular, don't build the skill (step 3) against an empty brain; the bootstrap (step 1) is what makes the output worth shipping at all.

---

## 9. Open decisions (need input before/at build time)

- **SME identity** — name, title, credentials not yet supplied. Needed to fill `sme-persona.md` beyond placeholders (plan step 1).
- ~~**Intake channel**~~ — resolved 2026-07-30: **HARO, Connectively, SOS, and Qwoted** (confirmed by the user, recorded in `kdci-ai-profile.md`). Still manual paste for now — no platform API integration — but the source platforms are known, which is useful context for Step 2's eventual query-parsing logic (typical format/length/tone per platform).
- **kdci.co scope** — out of scope for now (confirmed AI-only); revisit if the SME starts fielding outsourcing/BPO press queries too.
- **Second approval stage** — confirmed single-gate (SME reviews every draft personally); revisit only if query volume grows enough to need a PR/comms triage layer in front of the SME.
- ~~**Skill trigger name**~~ — resolved: `/journalist-query-respond` (see `SKILL.md`).

---

## 10. Progress Log

Append newest last. Format: `## YYYY-MM-DD — title` then Discussed / Decisions / Built / Open.

## 2026-07-29 — Project spec'd
**Discussed:** SME wants a skill that answers journalist/HARO-style editorial queries representing KDCI.ai, using a "brain" of company + SME knowledge, in the SME's voice. Evaluated feasibility first: process design is sound and mirrors existing PR-tech patterns, but quality is gated entirely by whether the brain holds real, grounded opinions/data (vs. company boilerplate) — a brain with no real positions in it produces generic thought-leadership that reads as AI and gets filtered by editors. Fabrication risk here is categorically higher than the parent project's (output is published under a real person's byline, not a dropped CSV row).
**Decisions (user-confirmed via AskUserQuestion):** (1) Brand scope = kdci.ai only. (2) No existing voice corpus — starting cold, so the spec includes a persona-bootstrap interview step before any skill gets built, plus a calibration log to track voice drift over early runs. (3) Intake channel unconfirmed/ad hoc for now (no HARO/Connectively/Qwoted integration yet). (4) Approval = single gate, SME personally reviews and edits every draft before sending — no batch approval, no auto-send, ever.
**Built:** Project folder `Journalist Query Responder Project/` (sibling to the parent project) + `CLAUDE.md` (what/why/rules) + this `SPEC.md` (brain structure, 10-step pipeline, schemas, cold-start plan, 5-step build plan). `sme-brain/` folder created with `kdci-ai-profile.md` (seeded from parent profile) and `sme-persona.md` (template).
**Open:** SME identity still needed. Bootstrap interview (plan step 1) not yet run. No skill code written yet (plan step 3) — deliberately sequenced after the brain has real content, not before.

## 2026-07-29 — SME identity confirmed (Emman Umali) + bootstrap interview drafted
**Discussed:** User supplied Emman Umali's real bio. Key finding: his actual specialty is production LLM/agent engineering (reliability, anti-hallucination safety, MLOps, computer vision, AI enablement) — not generic "AI staffing" commentary, which is what the first-pass `kdci-ai-profile.md` expertise list had assumed. This reshapes query fit: strong fit for queries #1 (big-tech AI competition) and #2 (agents vs. automation) given his hands-on production experience; weak/uncertain fit for query #3 (AI avatars/digital legacy) since that's digital-human/avatar product design, not agentic backend infrastructure — flagged for explicit confirm rather than silent decline, since there may be an adjacent angle (persona-memory consistency in agentic systems) worth offering.
**Decisions:** Split the bootstrap content into two files: `bootstrap-interview.md` (the first-run interview script/process, tailored to Emman's real specialty, clustered into technical stances / boundaries / voice-tone / credibility-framing) and `sme-persona.md` (the output artifact the interview fills in). Kept them separate rather than one file, since script vs. data store are different concerns.
**Built:** `sme-persona.md` Identity section filled with Emman's real bio + a "real specialty" note used for query-fit judgment. `bootstrap-interview.md` — 14-question interview across 4 clusters, each question tied to a specific gap (an example query, a boundary, a voice trait, or a credibility angle) rather than generic. `kdci-ai-profile.md` expertise-areas and not-KDCI's-lane lists rewritten to match the real bio, with AI avatars explicitly called out as a fit-question rather than auto-included/excluded.
**Open:** `bootstrap-interview.md` not yet run with Emman — `topics-position-bank.md` still empty, `sme-persona.md` tone/voice/writing-samples still template. This is plan step 1's remaining work before the skill (plan step 3) should be built.

## 2026-07-29 — Repo published + first-run bootstrap interview built as a real skill step
**Discussed:** User asked to publish the project as a GitHub repo the SME (Emman) can clone, and separately asked about turning the bootstrap interview from a static doc into an actual runnable first-run function — not just something a human reads and manually copies answers from.
**Decisions (user-confirmed via AskUserQuestion):** Repo visibility = **public** (diverges from the private-leaning default I recommended, since this repo commits Emman's real name/bio and KDCI.ai's real positioning directly, unlike Build A/B's genericized+gitignored pattern) — user chose public to match the existing precedent of the two prior link-prospecting repos. Published under `james-kdci` (not personal `japresbitero-del`), same account-switch-then-restore pattern as prior sessions.
**Built:** (1) `README.md` + `LICENSE` (MIT, KDCI Outsourcing 2026) added for the SME-facing repo entry point. (2) Git repo initialized in this project folder and pushed to **https://github.com/james-kdci/kdci-journalist-query-responder** (public). (3) `SKILL.md` written — the first real runnable piece of this project: Step 0 (self-install — copies itself AND the entire `sme-brain/` folder together, since unlike Build A/B this skill's value is the stateful brain, not just the pipeline logic) and Step 1 (bootstrap gate — checks `sme-persona.md`'s status line; if not bootstrapped, conducts the 4-cluster interview conversationally, one cluster at a time with follow-ups, never all 14 questions at once; writes real answers into `sme-persona.md`/`topics-position-bank.md`/`kdci-ai-profile.md`; marks bootstrap complete). Step 2 (the actual drafting pipeline) is explicitly stubbed in `SKILL.md` rather than half-built. Resolved the "skill trigger name" open decision: `/journalist-query-respond`.
**Open:** The interview still hasn't been run with Emman for real — that's the next actual value-adding step. Step 2 (drafting pipeline) remains unbuilt by design until the brain has real content to test against (plan step 3, in progress).

## 2026-07-30 — Brain populated from real material (Path B bootstrap) + intake channels confirmed
**Discussed:** Rather than running the live conversational interview, Emman supplied his own real answers to all 3 original launch example queries (Microsoft/OpenAI/Google/Meta competition, AI agents vs. automation, Ronald Wayne digital avatar). User also confirmed the actual journalist-query intake platforms: HARO, Connectively, SOS, Qwoted.
**Decisions:** Generalized the bootstrap mechanism in `SKILL.md` into two valid paths — Path A (live conversational interview, as originally designed) and **Path B (real material supplied directly)** — since this is likely to recur (more sample responses, past quotes, articles) and is often richer than a live Q&A. Raw source material is saved verbatim and never overwritten (`sme-brain/writing-samples/`), with everything else distilled from it and cited back to it. Confirmed: query 3 (AI avatars) is **in-lane after all** — Emman answered it with real technical specificity (knowledge base + retrieval grounding, voice cloning, likeness capture, real-time orchestration, naming Delphi/ElevenLabs/HeyGen) — moved from "not KDCI's lane" to a confirmed expertise area in `kdci-ai-profile.md`, reversing the original assumption from the 2026-07-29 session.
**Built:** `sme-brain/writing-samples/2026-07-30-sample-query-responses.md` (verbatim source). `topics-position-bank.md` populated with 16 real, traceable position entries across all 3 queries (previously empty). `sme-persona.md` status upgraded to `BOOTSTRAPPED: 2026-07-30` with an explicit note that Clusters B/D remain open; Tone/voice notes section rewritten with 8 evidence-backed observations (balanced/dialectic structure, closes with a clean reduction, sparing first-person conviction, root-causes failure to process not technology, comfortable naming real companies/tools, metrics-grounded, labeled sub-answers, jargon-to-plain-English translation). `kdci-ai-profile.md` expertise list gained the digital-avatar item (moved out of not-my-lane); added a confirmed "Intake channels" section. `SKILL.md` Step 1 rewritten to document Path A/B explicitly, so future real material drops in the same way without needing a redesign each time. Resolved the "intake channel" open decision.
**Open:** Cluster B (explicit competitors/topics to avoid) and Cluster D (default credibility-angle) still not directly asked — flagged as a light follow-up in `sme-persona.md`, not blocking. Step 2 (drafting pipeline) still not built — the brain is now populated enough to make that the natural next step.

## 2026-07-30 — Step 2 (drafting pipeline) built and mechanics-tested
**Discussed:** User asked to build Step 2 (the actual drafting pipeline) now that the brain has real content, and asked whether it needed anything from them. It didn't — the pipeline logic was writable directly from this SPEC's §3 design.
**Decisions:** Wrote `SKILL.md` Step 2 as 9 concrete substeps (relevance gate → decompose → retrieve → gap-check → draft → output → human gate → feedback capture → log), matching `SPEC.md` §3's design but as literal, repeatable instructions (mirroring Build A/B's style of concrete substeps + exact file paths). Then test-ran it against all 3 launch example queries to satisfy plan step 3's verify bar. **Important caveat surfaced and recorded rather than glossed over:** these 3 queries are exactly what the brain was built FROM (Emman's own real answers), so this test proves the pipeline's *mechanics* — decomposition, grounding classification, zero added fabrication, correct output format, and (for query 3) that the relevance gate surfaces a fit-check rather than silently deciding — but does **not** prove generalization to a genuinely new, unseen query. Every draft carries an explicit note saying so.
**Built:** `SKILL.md` Step 2 (full pipeline). Test outputs: `runs/2026-07-30/{microsoft-ai-competition,ai-agents-vs-automation,ronald-wayne-digital-avatar}/{query.md,draft.md}` — all 3 came back 100% grounded (5/5, 6/6, 4/4 sub-questions) with 0 fabricated claims and 0 flags, which is the expected result given the circularity noted above. `runs/response_log.csv` created with all 3 rows logged `draft_status=drafted`.
**Open:** The real test that matters — a genuinely new query the brain wasn't built from — hasn't happened yet. Step 2.8 (feedback capture) is written but inert until Emman actually sends a real response. Cluster B/D from the earlier session still open.
