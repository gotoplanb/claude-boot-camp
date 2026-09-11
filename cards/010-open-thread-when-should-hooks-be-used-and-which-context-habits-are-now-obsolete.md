# Open Thread — When Should Hooks Be Used, and Which Context-Warming Habits Are Now Obsolete?

**Source:** Dave, Claude Boot Camp planning session 2026-09-11, flagging his own possibly-stale assumptions.
**Type:** question
**Verified:** confirm-this — nothing here is settled. This card exists to hold the questions, not to answer them.
**Relevant to:** 2 (operating Claude products), 5 (building with Claude Code)

## The questions

**1. Which context-warming habits are obsolete?**

Dave's habits formed over the last couple of years, when you had to be careful not to pollute a small context window. Concretely: preferring pointers into other documents over inlining, keeping CLAUDE.md lean, rationing what gets loaded.

Card 008 has a partial answer — relevance still beats volume, but size alone is much less of a constraint. What's still missing is anything empirical:

- At what rough size does a CLAUDE.md start costing more attention than it earns?
- When is a pointer genuinely better than inlining, now that file reads are cheap and on demand?
- Does a large, mostly-irrelevant knowledge base measurably degrade retrieval in a Project, or is that folklore?

These want a real experiment, not a plausible-sounding answer. They're the kind of question an LLM will confidently pattern-match and get wrong.

**Partially answered (2026-09-11), see card 014.** One sub-question is now settled: *can a standing `CLAUDE.md` instruction fix staleness?* No — an instruction to "always re-verify external state" competes with the model's default trust in its own snapshot on every turn, and mostly loses. That class of drift has to be closed structurally or made deterministic with a hook, not prompted away. The sizing questions above remain open.

**2. When should hooks be used?**

Barely explored. Open sub-questions:

- Which parts of a workflow genuinely want a hook versus a project instruction? (Hooks are deterministic — the harness runs them — so they're the right answer when "sometimes the model forgets" is unacceptable.)
- What are the real use cases beyond formatting-on-save: test gates, notifications, guardrails, audit logging?
- What's the equivalent story *outside* Claude Code — on claude.ai, in Projects, on mobile? Is there one, or is determinism a Claude Code-only capability?
- Where's the line at which a hook becomes a worse version of a git pre-commit hook or a CI check?

## Why this matters

Both questions are exactly what a center-of-excellence lead will be asked, and both are places where confident-sounding wrong answers are cheap to produce. Holding them as an explicit open card — rather than letting a plausible answer get carded as fact — is the verification discipline working as intended.

When these resolve, they should become several cards, not one: the hooks material in particular will split by use case.
