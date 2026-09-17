# When a Filed Design Meets the Build, the Judgment Survives and the Mechanism Doesn't

**Source:** Comparing card 070 as filed (2026-09-17, morning) against the plugin Dave then built at `~/watchtower/labs/release-notes-plugin` (same day).
**Type:** pattern
**Verified:** `ran-it` for this instance — one design, filed then built, with the diff visible in both cards. `inferred` for the generalisation; it's one case, and cards 058 and 063 are the supporting ones.
**Relevant to:** general, 5 (building with Claude Code)

## The worked example

Card 070 was filed with three rulings, then built. The diff:

| Ruling | As filed | After building |
|---|---|---|
| 1 — the model must not guess the release tier | **Held.** | |
| 1's *mechanism* — "make them commands, not skills" | | **Wrong.** Commands are merged into skills; the choice is `disable-model-invocation: true` |
| 2 — three skill files, not one with branches | **Held.** | Built that way |
| 3 — screenshots are the human's job | **Held.** | Built as TODO placeholders plus a checklist |
| Card 069's file structure | | **Wrong.** `.claude-plugin/plugin.json`, not root |
| Card 069's "skills are markdown only" | | **Wrong.** `allowed-tools`, `$1` args, executable blocks |

Every *judgment* survived. Every *mechanism* claim that hadn't been run was either wrong or dissolved.

## The pattern

**Judgment is durable; mechanism is perishable.**

Judgment answers *what should be true* — the model shouldn't guess something you already know; a patch shouldn't pay for major's context; a skill shouldn't promise what the host can't do. These come from reasoning about the problem, and the problem doesn't change when the product ships a release.

Mechanism answers *how the product does it today* — file layouts, field names, which primitives exist. These are somebody else's implementation decisions, and you have no access to them except by looking.

The failure mode is stating the second with the confidence you earned on the first. Card 070 said *"tiers are commands, not auto-fired skills"* — one clause of judgment welded to one clause of mechanism, in a sentence that reads as a single claim.

## What follows from it

- **Write the judgment as the ruling; write the mechanism as an implementation note.** Then a build corrects one without discrediting the other.
- **A wrong mechanism often makes the judgment sharper.** Here it improved: *"should the model be allowed to fire this?"* is one teachable frontmatter knob instead of an architectural choice. The question the design was really asking got a cleaner answer than the design knew to propose.
- **Expect questions to dissolve, not just resolve.** *"Does a command reliably load its paired skill?"* had no answer because it had no referent — same primitive, no hop. A dissolved question is a sign the model of the system was wrong, which is more valuable than a yes.
- **Bootstrapping gates are cheap; blocked gates aren't.** 070's prerequisite was building the thing. Card 068's is product behaviour nobody here controls. Sort pending work by which kind it is.

## Why this matters

It's the argument for the verification discipline stated positively. `labs/README.md` bars labs built from `inferred` cards, which reads like bureaucracy until you see what it caught: a lab teaching a `commands/` directory that would have collided on its own names, built on a manifest path that doesn't exist.

Nothing wrong got taught, because the card was honest about not having been run. **The discipline isn't there to slow down filing — it's there to make filing early safe.** Card 070 was filed the same morning it was wrong, and that was fine, because it said so.

See card 058 (write the spec to kill it — the same willingness, one step earlier), card 063 (harvest the sequence, then ship the script — spend the flexible tool once to learn), card 056 (conventions outliving their constraints — mechanism going stale the slow way instead of the fast way), and cards 069/070/071 (the instance).
