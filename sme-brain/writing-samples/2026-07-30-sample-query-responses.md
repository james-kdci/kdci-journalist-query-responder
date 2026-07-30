# Writing Sample — Emman's real answers to the 3 launch example queries

**Source:** Emman Umali, supplied directly (not drafted by this skill), 2026-07-30. Verbatim — this is the raw source material `sme-persona.md` (tone/voice) and `topics-position-bank.md` (positions) are distilled from. Keep this file unedited as the ground truth to re-derive from if later distillation is ever questioned.

**Note on scope:** these are Emman's real answers to the same 3 queries used to originally illustrate this project's use case — meaning this project's cold-start problem is now substantially resolved for Clusters A (core technical stances) of `bootstrap-interview.md`. Clusters B (explicit boundaries) and D (credibility-framing default) are not covered by this material and remain open — see `sme-persona.md` status line.

---

## Query 1: How are Microsoft's latest AI updates changing the competition with OpenAI, Google, and Meta?

Across the industry, the big move right now is away from chat and code assistance and toward agentic workflows, where AI carries out multi-step tasks instead of just answering questions. The clearest sign of this is where the investment is going, with every major lab racing to expand how much a model can hold in working memory while it operates. The leading models from OpenAI, Google, and Anthropic have pushed that memory into the millions of tokens, so an agent can keep a lot of operational context in play. Microsoft's in-house models currently carry noticeably less, which may become a constraint for heavier, longer-running agent tasks.

What matters more than the spec is the strategic signal. By building its own models, Microsoft is showing that it wants to compete at the frontier and not just resell what another company makes. It is still an early entrant there, and OpenAI continues to power the backbone of Copilot. So the way I see it, Microsoft is adding options for itself while keeping OpenAI close.

I read these moves primarily as risk reduction. By loosening exclusivity and working across several model providers and initiatives, Microsoft opens the door to any capable lab, whether established or a startup, and avoids betting its whole future on a single partner. It appears that Microsoft is not closing the door on OpenAI, but rather giving itself room to pursue several approaches at once, which is generally what drives innovation rather than locking a company into one path.

The real head-to-head in the enterprise is Microsoft against Google. Google's advantage is reach, from its enormous base of everyday Workspace users to the fastest-growing cloud business, alongside its own frontier models. Meta is taking a very different path. It is spending heavily on chips and data centers and still shipping capable models, but it has stepped back from the open-weight frontier, and it lacks the workplace productivity suite that Microsoft and Google rely on to reach businesses. That missing piece is what keeps the enterprise contest between just the two of them.

For most people, I would not expect a dramatic near-term change in everyday AI use. The biggest beneficiaries will be organizations already working inside the Microsoft ecosystem, where these capabilities appear right where their teams already work.

---

## Query 2: AI agents vs. traditional automation, and how to choose between them

In practice this is rarely a straight choice between the two, because the strongest systems usually combine them.

**Where traditional automation still wins.** When the work is rule-based and predictable, traditional automation is the better tool. It is more reliable, cheaper, and easier to trust. You bring in AI agents only when a task calls for judgment or has to interpret the messy, human kinds of input like images, audio, language, and meaning. When a simple rule can handle the job, that is usually the right way to do it.

**Where AI agents prove their value.** An agent has to prove its value in terms you can actually trace and measure. In customer service, for example, I look at response speed and response rate, how many tickets it resolves on its own, and how consistently it holds the right tone. Most important is accuracy against the company's own standards, meaning whether the information is correct, whether it draws from the right sources, and whether it respects confidentiality. I also pay close attention to the approval rating from customers, whether that comes from real customer feedback or from the system's own self-evaluation. Without those measures in place, there is no real way to stand behind the results.

**The most common mistake.** Companies reach for AI on every problem, and that is the trap. If the process is routine and the decisions are structured, automation is often the smarter and cheaper answer. Save AI for genuinely complex inputs like unstructured documents, multimedia, and anything that does not fit neatly into a spreadsheet. In most real projects the best result comes from using both together rather than choosing one over the other.

**Cost, implementation, and maintenance.** AI generally costs more across the board, so it helps to be clear about what you are actually building. Applied AI, meaning a model shaped to your specific process, involves fine-tuning, testing, iterating on feedback, and the ongoing cost of running frontier models. The build itself has to be engineered with care, with guardrails against hallucination, careful context management, turning messy input into something structured, and a disciplined, quality-checked process so the result solves the original problem. Unlike traditional automation, AI also needs watching after launch, because the world shifts, your data shifts, and performance quietly drifts when no one is monitoring it.

**Choosing the right fit.** It comes down to the value you are after. When the job means handling genuinely complex data, or producing real insight such as analysis, prediction, or understanding meaning and themes, that is where AI belongs. When it does not, you probably do not need it.

**Lessons from real deployments.** Two lessons stand out. First, in computer vision, meaning any system that works with images or video, there is a real gap between the lab and the real world. Lighting, camera angle, weather, and glare all change what the model sees, and no amount of clean training data fully closes that gap until you deploy and learn from live conditions. Second, when it comes to Applied AI, your data is your real advantage. Every company has its own voice, policies, customers, and know-how, and a deployment only works once you feed that in, so the AI can replicate, support, or extend what your people already do. The point I would most want a business to hear is that when these projects fail, the cause is rarely the model itself. It usually traces back to unclear goals and weak oversight, and once those are sorted out, the technology tends to hold up well.

---

## Query 3: The future of AI-powered legacy preservation, and a digital avatar for Ronald G. Wayne

Digital avatars have moved out of the research lab and into something close to a working industry, and the tools are now mature enough that a project like this is genuinely achievable. Ronald Wayne is close to an ideal subject, and the main reason is simply that he is still alive.

That single fact resolves most of the hard questions. The toughest issues in this field, including consent, privacy, accuracy, dignity, and how the public receives a recreated person, all become far simpler when the subject can speak for himself. Wayne could give informed consent, sit for sessions that capture the insights he never put into his books, review and approve the knowledge the avatar draws on, and set clear limits on what it should and should not say. With him directly involved, we would be preserving his real legacy rather than reconstructing it from the outside.

It is also a story worth telling because it points AI in a hopeful direction, which is preserving history in order to educate. Future generations could ask a founding figure of the personal-computer era about his thinking, hear it in his own voice, and learn from it.

As for how you would actually build it, the core pieces are well understood. You start with a knowledge base of everything he knows and has done, paired with a retrieval system so the avatar can surface the right memory at the right moment. An approach like Delphi's works well here, since it grounds answers in a person's own timeline, so responses reflect not only what happened but what it meant to him at the time. You clone his voice for natural, high-quality speech, and tools like ElevenLabs do this well. You capture his likeness through a photo, a short video, or a full multi-camera scan for the most lifelike result, then render it convincingly, where something like HeyGen is strong. Finally, you tie it all together with an orchestration layer that runs the whole loop in real time, handling the listening, the reasoning, the knowledge retrieval, and the spoken response with minimal delay.

Encouragingly, you do not have to build all of this from scratch, since much of it exists off the shelf today. Once the core is in place, the experience can take whatever form fits the audience, whether that is a face-to-face video conversation, an interactive chat, a curated set of recorded answers, or a guided, presentation-style walkthrough of his life.

At its simplest, recreating Ronald Wayne comes down to three things: capturing his knowledge and personality, capturing his voice, and capturing his likeness. Everything else is craft built on top of those foundations. And because he is here to guide the process, it could be done in a way that is both impressive and genuinely responsible.
