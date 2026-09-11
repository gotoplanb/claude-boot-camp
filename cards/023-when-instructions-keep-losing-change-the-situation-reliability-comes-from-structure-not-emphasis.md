# When Instructions Keep Losing, Change the Situation — Reliability Comes From Structure, Not Emphasis

**Source:** Named in the Claude Boot Camp session of 2026-09-11, after the same principle turned up independently in four places (state drift, context structure, harnesses, observability). Beta spotted the through-line; this card exists because three other cards were each restating it locally.
**Type:** pattern
**Verified:** `ran-it` — every instance below is something Dave hit and solved in practice. The generalisation is `inferred` from those cases, not measured.
**Relevant to:** general (the principle the practical cards keep rediscovering)

## The principle

> **If you find yourself writing longer, louder, or more insistent instructions to get reliable behaviour, the instruction is the wrong tool. Change the situation instead.**

A standing instruction has to win an argument on every single turn, against whatever the model's default posture is. Sometimes it wins. Over hundreds of turns, "sometimes" is indistinguishable from "no."

Restructuring doesn't argue. It removes the choice, binds the action to a trigger the model can recognise, or arranges for something other than the model to catch the failure.

## The three levers

| Lever | What it does | Reach for it when | Instance |
|---|---|---|---|
| **Constrain** | Makes the wrong answer unavailable | Output is inconsistent; conventions drift; you're writing style guidance | Tailwind's closed vocabulary instead of a CSS style guide — card 021 |
| **Trigger** | Binds a concrete action to a recognisable condition | You're asking for standing vigilance ("always check…") | *"When cutting a release, read `docs/release-checklist.md`"* — card 016. Or a hook, which the harness runs rather than the model remembering — card 017 |
| **Verify** | Lets something other than the model confirm the result | You can't tell whether it worked; you want to walk away | Tests, Tempo traces, driving the real browser — cards 020, 022 |

They compose, and they fail differently. Constraint can't tell you the feature is broken. Verification can't stop twelve naming conventions that all pass.

## The diagnostic

**Notice the escalation.** The tell isn't that something went wrong once — it's that your fix was *more words*, and the next fix was *more words in capitals*.

`CRITICAL: You MUST always verify X before asserting Y` is a sentence that has already lost twice. It's a signal to stop editing prose and ask which of the three levers applies.

## Where instructions are still right

This isn't "instructions don't work" — most of what you write *is* instructions, and they're fine. The principle is narrower:

- **Instructions work well for one-shot direction.** "Write this in Go," "keep it under 200 words," "use a warmer tone" — stated once, acted on once, done.
- **Instructions work well for taste and judgment** where no tool could encode the preference anyway.
- **Instructions degrade for standing vigilance** — anything phrased "always," "never forget to," "before every X" is asking the model to win the same argument indefinitely.

The dividing line is roughly: *does this need to hold on turn 200 as reliably as on turn 1?* If yes, structure it. If it's a one-time steer, just say it.

## Why this matters

This is the single most transferable idea in the corpus so far, and it keeps getting rediscovered locally because each domain makes it look like a domain-specific fix — a git problem, a CLAUDE.md problem, a testing problem. It's one problem.

It also predicts where to spend effort. Time spent on constraints and verifiers compounds across every future task; time spent sharpening an instruction pays out once and decays as models change disposition (card 015). When something matters and must not silently stop working, don't write it more forcefully — build the thing that makes forcefulness unnecessary.
