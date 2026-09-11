# Knowledge-Elicitation Harnesses — Mine Tacit Gotchas by Rebuilding From Your Own Notes and Diffing the Orgs

**Source:** Dave, Claude Boot Camp session 2026-09-11. The verification-vs-elicitation distinction and the sampling caveat are Beta's, same session. The safe method below is Dave's revision after the policy problem with the original approach surfaced.
**Type:** pattern
**Verified:** `ran-it` for the underlying technique producing real gotchas — it's how Dave's Salesforce lessons-learned corpus started. `inferred` for the specific note-and-diff protocol as written here, which is the cleaned-up version. `[confirm-this: run the full protocol once end-to-end and record how many gotchas the diff surfaces that the notes missed.]`
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), general

## A third kind of harness

Card 021 split harnesses into **constraint** (narrow the space before generation) and **verification** (check ground truth after). There's a third job that looks like verification and isn't:

| | Verification harness | **Knowledge-elicitation harness** |
|---|---|---|
| Question it answers | *Did this build work?* | *What don't we know that we don't know?* |
| Value is in | The pass | **The divergences** |
| Output | A green build | **A corpus of gotchas** |

You expect divergence. The point is that **the gap between what you wrote down and what actually had to happen surfaces tacit dependencies neither you nor Claude would have thought to ask about.**

This is the counter to card 024's problem: you can't instruct Claude to be careful about invisible platform dependencies, because neither of you can enumerate them yet. You have to go find them.

## The method — rebuild from notes, then diff

The trick is that **the model never solves the original challenge.** It re-implements from *your* artifacts, somewhere else, and the learning happens in the comparison.

**1. Solve it yourself, manually, and take notes as you go.**
A Trailhead superbadge is a good scenario: a plain-language use case with automated checks that tell you *where* you failed. Work it legitimately.

**Prefer one you haven't completed before.** A first solve produces better raw material than a replay: the notes are written while you're actually stuck, so they capture the struggle rather than a reconstruction from memory. On a badge you already know, the tacit steps are *more* invisible — you do them reflexively and never think to write them down, which is exactly the material being hunted. It also moots the policy question a second way: there's nothing to reset.

While you work, keep the notes you'd keep anyway:
- the **error messages as Salesforce wrote them** — verbatim, they're the most useful artifact
- what you had to go fix, and in what order
- a running document of the sequencing and gotchas you hit

**2. Hand Claude your notes — not the challenge — and point it at a clean org.**
A separate sandbox or a fresh Trailhead playground. The input is your own write-up of the requirement, not the superbadge page.

**3. Diff the two orgs.** Side by side manually, and with the `sf` CLI: does what Claude built match what you built by hand? Retrieve metadata from both and compare.

**4. Iterate.** Every divergence is a gotcha — something that was obvious to you in the moment and absent from your notes, which means it's also absent from any instruction you'd have written for Claude.

That last point is the whole mechanism. **The gaps in your own notes are the tacit knowledge**, and the diff is what makes them visible.

## Why this shape, and not the obvious one

The obvious version — reset the badge and let Claude attempt the challenge directly — has a policy problem. Trailblazer Community guidance states that getting outside help to complete a superbadge violates the certification agreement, and **there is no published carve-out for a badge you already earned**. "I passed it myself first" is reasonable in spirit but isn't sanctioned in the text, and a certification-standing question is not one you want to have with someone's L&D team.

The notes-and-diff method sidesteps it entirely: the credential was earned by your own unaided work, and Claude only ever sees your notes and a clean org. **Same gotchas, no conversation with Salesforce's learning team.**

It's also a better harness. The direct version tests Claude against Salesforce's checker; this version tests Claude against *your understanding*, which is the thing you're actually trying to find holes in.

## Other sources of the same pattern

Superbadges give you a well-specified scenario for free, but any ground truth works:

- **The `sf` CLI as the checker.** Deploy to a scratch org and mine the *errors*. Same validation engine you'll ship against.
- **Non-credential Trailhead content** — projects and modules with hands-on challenges, no certification agreement attached.
- **Real client work, carded as you go.** Slower and unsystematic, but the gotchas are the ones that matter.

## Be honest about what the corpus covers

Mark provenance on every gotcha. The generalisation is uneven:

- **Platform-structural** — field-level security before visibility, tab assignment, permission-set sequencing — true everywhere, and this method surfaces them reliably.
- **Org-specific accumulated weirdness** — the actual long tail of consulting — **cannot** be surfaced this way by construction, because playgrounds and fresh sandboxes are clean.

A corpus that silently mixes the two fails exactly when someone points it at a real org with fifteen years of cruft in it.

## The payoff — a library you select from per project

The accumulated documents become a **capability-indexed reference library**. Starting a project, the question becomes: *what platform features does this need?* — identity and access, custom objects and field types, flows, integrations — and you feed **only that subset** into context.

That's card 016's minimal-core-plus-pointers applied to domain knowledge rather than repo structure, and card 012's curated-subset idea applied to your own work rather than distribution. The corpus is worth building precisely because it's selectable; a single monolithic gotchas file would be too big to load and too diluted to help.

## Why this matters

It reframes early platform work: the first deliverable isn't a working feature, it's **the map of what makes features fail** — and the map is reusable across every later task.

It also supplies the missing piece for card 023. Constrain, trigger, and verify all assume you already know what needs constraining. Elicitation is how you find that out.
