# Two Kinds of Harness — Constraint Narrows the Space Before Generation, Verification Checks Ground Truth After

**Source:** Beta, Claude Boot Camp session 2026-09-11, separating two things Dave was using together in his stack and tooling choices.
**Type:** concept
**Verified:** `ran-it` for the constraint half — this is the stack guidance Dave gives on every project. `inferred` for the two-category framing itself (a useful cut, not a measured claim).
**Relevant to:** 5 (building with Claude Code), 1 (foundational concepts), general

## Content

"Harness" (card 020) covers two mechanisms that work at opposite ends of generation. Naming them separately makes it obvious which one a situation needs.

### Constraint harness — narrows the solution space *before* generation

Pick from a curated space instead of leaving the field wide open.

Dave's stack guidance is entirely this, and deliberately **framework-level, not library-level**:

- Need administrative management → **Django**. Don't → **FastAPI**.
- Front end → **htmx + Alpine.js + Tailwind**.

He gives the high-level framework and stops — no individual library picks. The framework is the harness; inside it, choice is fine.

**Tailwind is the clearest case.** Utility classes are a fixed, legible vocabulary. There's no room to invent an ad-hoc class-naming scheme or drift into inconsistent styling across files, because the vocabulary is closed. Compare the alternative: arbitrary hand-written CSS, where every file is a fresh opportunity to invent conventions that don't match the last ones.

> A constraint harness works by giving the model **an example of what good looks like**, encoded as a restricted vocabulary — rather than an open field plus instructions about taste.

This is why "use Tailwind" outperforms a long style guide saying how to write CSS well. The restriction is enforced by the tool; the style guide is enforced by the model remembering it. Same distinction as card 014 — designed-in beats instructed-for.

### Verification harness — checks ground truth *after* generation

Replace hypothesis with observation. Tests, traces, driving the real browser, a build, a lint pass. See card 022 for the full worked setup.

### Choosing between them

| Symptom | Reach for |
|---|---|
| Output is plausible but inconsistent across files; conventions drift | **Constraint** — close the vocabulary |
| Output looks right and you can't tell if it works | **Verification** — get ground truth |
| You want to walk away from the task | **Verification** — an external verifier must stand in for you (card 020) |
| You keep writing longer instructions about style or approach | **Constraint** — the instruction is losing; restrict the space instead |

They compose. A constraint harness shrinks the space of things that can go wrong; a verification harness catches what still does.

## Why this matters

Both get called "guardrails," which hides that they fail differently. A constraint harness can't tell you the feature is broken — Tailwind classes are perfectly consistent in a page that doesn't work. A verification harness can't stop the model producing twelve different naming conventions that all pass the tests.

The practical tell that you're missing a constraint harness: **you're writing increasingly detailed instructions about how to do something.** That's the same losing argument as card 014's standing-vigilance instruction. When you notice it, stop adding prose and go find the tool that makes the wrong answer unavailable. Card 023 names this pattern and its three levers.
