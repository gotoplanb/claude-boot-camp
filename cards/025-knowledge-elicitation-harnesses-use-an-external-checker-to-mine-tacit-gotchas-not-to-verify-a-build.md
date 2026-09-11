# Knowledge-Elicitation Harnesses — Use an External Checker to *Mine* Tacit Gotchas, Not to Verify a Build

**Source:** Dave's Trailhead superbadge technique for building a Salesforce gotcha corpus, Claude Boot Camp session 2026-09-11. The distinction from verification harnesses, and the sampling-bias caveat, are Beta's from the same session.
**Type:** pattern
**Verified:** `ran-it` for the technique producing real gotchas — it's how Dave's Salesforce lessons-learned corpus got started. `docs` for the policy position below (see the caution).
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), general

## A third kind of harness

Card 021 split harnesses into **constraint** (narrow the space before generation) and **verification** (check ground truth after). There's a third job that looks like verification and isn't:

| | Verification harness | **Knowledge-elicitation harness** |
|---|---|---|
| Question it answers | *Did this build work?* | *What don't we know that we don't know?* |
| Whose system | Yours | Someone else's, deliberately |
| Value is in | The pass | **The failures** |
| Output | A green build | **A corpus of gotchas** |

You already expect to fail. The point is that **the gap between "what the prompt asked for" and "what the checker demands" surfaces tacit dependencies neither you nor Claude would have thought to ask about.** The checker is a teacher, not a gate.

This is the counter to card 024's problem: you can't instruct Claude to be careful about invisible platform dependencies, because neither of you can enumerate them yet. You have to go find them.

## The Salesforce instance — and its policy problem

Trailhead **superbadges** are an unusually good elicitation harness on paper: a plain-language use case, and four or five automated checks that tell you *where* you failed. Run the prompt at Claude in a playground, collect every failure, and each one is a gotcha for the corpus — identity and access management, field types, tab visibility, permission-set sequencing.

> ⚠️ **Caution — this collides with Salesforce's policy.** Trailblazer Community guidance states plainly that getting outside help to complete a superbadge violates the certification agreement. **There is no published carve-out for a badge you already earned**, and searching for one didn't find text distinguishing "already-passed" from "in-progress."
>
> Dave's own safeguard — pass it legitimately first, only replay against a reset instance for private practice, never to earn or re-earn the credential — is sound risk management **in spirit**, but it is not something the policy explicitly sanctions.
>
> **Do not make this a lab.** What's defensible as a personal judgment call about your own credential is a different thing when a curriculum recommends it to consultants whose certifications are employment-relevant. See the safer variants below.

## Safer instances of the same pattern

The pattern is the valuable part; superbadges are one source of ground truth, and a legally fraught one. Others:

- **Non-credential Trailhead content** — projects and modules with hands-on org challenges, which carry the checker without the certification agreement.
- **The `sf` CLI itself as the checker.** Deploy to a scratch org and mine the *errors*. Salesforce's deployment validation is a rich, honest source of dependency and ordering rules, and it's the same org you'll actually ship to.
- **Real client work, carded as you go.** Slower and less systematic, but the gotchas are the ones that matter (see the sampling caveat).
- **Write the checker yourself** for a workflow you already understand — then any model failure against it is an elicited gotcha.

## The sampling caveat — be honest about what this corpus covers

A verification harness over *your* system tells you about your system. An elicitation harness over **someone else's fixed scenario** tells you about that scenario, and the generalisation is uneven:

- **Platform-structural gotchas** — field-level security before visibility, tab assignment, permission-set sequencing — are true everywhere. Superbadges surface these reliably.
- **Org-specific accumulated weirdness** — the actual long tail of consulting work — **superbadges cannot surface by construction**, because they run in clean environments.

So mark the provenance on every gotcha you file. A corpus that silently mixes "always true on the platform" with "true in a pristine playground" will fail exactly when someone points it at a real org with fifteen years of cruft in it.

## Why this matters

It reframes early platform work: the first deliverable isn't a working feature, it's **the map of what makes features fail**. That map is reusable across every later task, which is why it's worth spending deliberate effort to mine rather than accumulating by accident.

It also supplies the missing piece for card 023. Constrain, trigger, and verify all assume you know what needs constraining. Elicitation is how you find that out.
