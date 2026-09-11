# Salesforce: "Created" Does Not Mean "Usable" — Claude Reports True Success and the Admin Sees Nothing

**Source:** Dave, Claude Boot Camp session 2026-09-11, on why Salesforce work with Claude needs more scaffolding than the API docs suggest.
**Type:** gotcha
**Verified:** `ran-it` — hit repeatedly across Dave's Salesforce work; the accumulated fixes live in his `docs/salesforce/lessons-learned.md` (43 documented gotchas at last count).
**Relevant to:** 6 (Claude + Salesforce)

## The setup

Salesforce is genuinely good to build against with Claude Code: the APIs are well documented, and the `sf` CLI is both capable and unusually informative — it gives back real, assessable feedback about org state rather than opaque success codes.

What is **not** in the published documentation is the part that actually bites: **dependencies and sequences.** Every org is different, and the ordering constraints that make a change *visible and usable* are tribal knowledge.

## The canonical example

Ask for a custom object. Claude creates it via the CLI and reports success. A newcomer assumes they can now log in and use it.

An admin knows that's maybe a fifth of the job:

1. Create the custom object.
2. Create the custom field(s) — and **field type choice matters** in ways that are painful to reverse.
3. Create a **tab**, or nobody can see it.
4. **Assign the tab to apps.**
5. Grant **field-level security** through a profile or permission set. (Dave prefers permission sets.)

Miss any of the last four and the human logs in to find *nothing*.

## Why this is worse than a hallucination

**Claude's report is accurate.** The object was created. The CLI said so, truthfully. There is no false claim to catch.

That's what makes it dangerous — the usual defenses don't fire. It isn't a confabulated success you could catch by checking; it's a **true statement about a step that doesn't add up to the outcome**. "Created" and "usable" are different states, and only one of them has a CLI command.

Claude won't think to ask about tab assignment or FLS for the same reason a new admin wouldn't: nothing in the request or the docs indicates that a created object is invisible by default.

## The consequence

**You will not get an efficiency gain from Salesforce + Claude until the gotchas are written down somewhere the model can read.** Without that corpus, every task costs a round of "it says it's done" → "I can't see it" → manual setup archaeology, and that round trip eats the speedup.

The corpus is the actual deliverable of early Salesforce work — see card 025 for how to mine it, and card 023 for why writing the gotchas down beats instructing Claude to be careful.

## Why this matters

It generalises past Salesforce to any platform with **configuration layered on top of creation** — CRMs, identity providers, cloud consoles, anything with a permissions model. The API call succeeds, the entity exists, and it's inert until three other things are also true.

For a consultant or SI, this is the difference between demoing an efficiency gain and demoing an embarrassment: the client logs in, sees an empty app, and the fact that the metadata deployed cleanly is no comfort at all.
