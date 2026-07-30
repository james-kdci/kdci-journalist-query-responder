# SME Bootstrap Interview — First-Run Script

This is the interview the skill runs **once**, on first invocation (Step 0 bootstrap, before any query gets drafted — see `../SPEC.md` §6 plan step 1). Answers get written into `sme-persona.md` (identity, tone, calibration) and `topics-position-bank.md` (positions, one topic per cluster below). Once bootstrapped, the skill should skip this and go straight to the drafting pipeline — same "config exists → silent" pattern as the parent project's Build A/B first-run setup.

Tailored to **Emman Umali's actual specialty** — production LLM pipelines, agentic systems, anti-hallucination engineering, MLOps, computer vision, AI enablement — not a generic "AI expert" template. This matters: it's what separates a quote that sounds like a specific engineer from one that sounds like any AI-company spokesperson.

Ask in clusters, one at a time, in conversation (not a form dump) — his own words in each answer are the point, not a checkbox.

---

## Cluster A — Core technical stances (feeds `topics-position-bank.md`)

1. **Agents vs. automation.** When should a business actually reach for an AI agent instead of traditional rules-based automation — and where does plain automation still win? (Directly answers example query #2's opener.)
2. **Anti-hallucination, in plain English.** You focus on "anti-hallucination safety" in production LLM pipelines — how would you explain that risk and how you mitigate it to someone who isn't technical? What's the one thing companies get wrong here?
3. **Demo-to-production gap.** What's the #1 mistake you see when a company goes from "cool AI agent demo" to something that has to run reliably in production?
4. **A real KDCI example.** Across the systems you've built — the AI provider marketplace, agentic support agents, inbound voice AI, AI-driven lead gen — is there one (even anonymized/generalized) where an AI agent delivered clearly measurable value over a rules-based approach? What actually changed?
5. **A war story.** What's a lesson you learned the hard way building or maintaining one of these systems — something that only makes sense once it's broken on you in production?
6. **Big-tech AI competition.** Does the Microsoft/OpenAI/Google/Meta competition actually change anything for the businesses KDCI's clients run, or is it mostly noise at their scale? (Answers example query #1.)
7. **Cost efficiency.** "Cost efficiency" is explicitly part of your mandate — what's a concrete cost lever in AI agent adoption that most companies overlook?
8. **Fit check — AI avatars / digital legacy (example query #3).** Your background is production agent infrastructure, not digital-human/avatar creation. Is this genuinely in your lane, or should we flag and decline it? If there's an angle you *can* speak to (e.g. the underlying challenge of encoding a real person's knowledge/persona reliably, memory/consistency in agentic systems), say so — otherwise this one gets marked out-of-lane in `kdci-ai-profile.md`.

## Cluster B — Boundaries (feeds `kdci-ai-profile.md` "not KDCI's lane" list)

9. What topics do you want to explicitly avoid or only lightly caveat? (e.g. foundation-model research/training, AI policy or regulation, hardware/chip supply chains — confirm or correct the current draft list.)
10. Any companies or competitors you'd rather not comment on directly by name?

## Cluster C — Voice/tone (feeds `sme-persona.md` tone notes)

11. When you explain technical concepts to non-technical people — reporters, clients, new hires during AI enablement/onboarding — what's your natural style? Analogy-heavy, blunt/no-fluff, story-first, data-first?
12. Any phrases, framings, or analogies you find yourself repeating when explaining AI to others?
13. Do you have any existing real material we could pull actual phrasing from — Slack explanations, onboarding decks, your Q1 journal paper, NVIDIA AI Academy talks/slides? (Even informal internal writing counts — see `sme-persona.md` Writing Samples.)

## Cluster D — Credibility framing (feeds `sme-persona.md` identity)

14. Your bio has three credible angles — (a) production/reliability engineering (the systems you actually build and ship), (b) academic/research credibility (Q1 journal co-author, MS Data Science, 86% ECE board score), (c) enablement/leadership (NVIDIA AI Academy PH founding/technical lead, KDCI's company-wide AI onboarding). Different journalist queries will want different angles emphasized — which should lead by default when a query doesn't specify?

---

## After the interview

- Write clusters A + B's answers into `topics-position-bank.md` as new topic entries (one per question that got a real answer).
- Write clusters C + D's answers into `sme-persona.md` (replace the "[LOW CONFIDENCE]" tone section, fill Identity's credibility framing).
- Mark this file's status as "bootstrapped: <date>" at the top so the skill knows not to re-run it.
