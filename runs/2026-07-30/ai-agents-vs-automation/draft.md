> **Test-run note:** this query is one of the 3 the brain was originally built from — Emman's real answer to this exact question is already in `topics-position-bank.md`. This draft verifies the pipeline's mechanics, not generalization to an unseen query. See `SPEC.md` progress log, 2026-07-30 entry.

## Decomposition (Step 2.2) — this query explicitly lists its 6 sub-questions

1. Where traditional automation still outperforms AI agents.
2. Business processes where AI agents have delivered measurable value.
3. Common mistakes companies make when choosing between the two.
4. Cost, implementation, and maintenance differences.
5. How to determine which approach is the right fit.
6. Lessons learned from real-world deployments.

## Grounding (Steps 2.3–2.4)

| # | Sub-question | Status | Source |
|---|---|---|---|
| 1 | Automation still wins | GROUNDED | "Where traditional automation still wins" |
| 2 | Measurable agent value | GROUNDED | "How to measure whether an AI agent is actually delivering value" |
| 3 | Common mistake | GROUNDED | "The most common mistake companies make choosing AI vs. automation" |
| 4 | Cost/implementation/maintenance | GROUNDED | "Cost, implementation, and maintenance differences (AI vs. automation)" |
| 5 | Right fit | GROUNDED | "How to determine the right fit between AI and automation" |
| 6 | Real deployment lessons | GROUNDED | "Lessons learned from real AI deployments" |

## Draft response

In practice this is rarely a straight either/or choice — the strongest systems usually combine both.

**Where traditional automation still wins.** When the work is rule-based and predictable, traditional automation is the better tool: more reliable, cheaper, easier to trust. Bring in AI agents only when a task calls for judgment or has to interpret messy, human kinds of input — images, audio, language, meaning. When a simple rule can handle the job, that's usually the right way to do it.

**Where AI agents prove their value.** An agent has to prove its value in terms you can trace and measure. In customer service, for example: response speed and response rate, how many tickets it resolves on its own, and how consistently it holds the right tone. Most important is accuracy against the company's own standards — correct information, right sources, respecting confidentiality — plus the approval rating from customers, whether from real feedback or the system's own self-evaluation. Without those measures in place, there's no real way to stand behind the results.

**The most common mistake.** Companies reach for AI on every problem, and that's the trap. If the process is routine and the decisions are structured, automation is often the smarter, cheaper answer. Save AI for genuinely complex inputs — unstructured documents, multimedia, anything that doesn't fit neatly into a spreadsheet. In most real projects the best result comes from using both together rather than choosing one over the other.

**Cost, implementation, and maintenance.** AI generally costs more across the board. Applied AI — a model shaped to your specific process — involves fine-tuning, testing, iterating on feedback, and the ongoing cost of running frontier models. The build has to be engineered with care: guardrails against hallucination, careful context management, turning messy input into something structured, and a disciplined, quality-checked process. Unlike traditional automation, AI also needs watching after launch, because the world shifts, your data shifts, and performance quietly drifts when no one's monitoring it.

**Choosing the right fit.** It comes down to the value you're after. When the job means handling genuinely complex data, or producing real insight — analysis, prediction, understanding meaning and themes — that's where AI belongs. When it doesn't, you probably don't need it.

**Lessons from real deployments.** Two stand out. First, in computer vision — any system working with images or video — there's a real gap between the lab and the real world. Lighting, camera angle, weather, and glare all change what the model sees, and no amount of clean training data fully closes that gap until you deploy and learn from live conditions. Second, for Applied AI, your data is your real advantage. Every company has its own voice, policies, customers, and know-how, and a deployment only works once you feed that in, so the AI can replicate, support, or extend what your people already do. The point I'd most want a business to hear: when these projects fail, the cause is rarely the model itself. It usually traces back to unclear goals and weak oversight — and once those are sorted out, the technology tends to hold up well.

## Flags

None — 6 of 6 sub-questions grounded.

## Grounding summary

6 of 6 sub-questions grounded. 0 fabricated claims.
