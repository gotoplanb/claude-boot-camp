# Workflow Conventions Outlive the Constraints That Justified Them — Re-Check When the Surface Changes

**Source:** Dave retiring the "post LinkedIn drafts as GitHub issues" rule, Claude Boot Camp session 2026-09-12.
**Type:** pattern
**Verified:** `ran-it` — the rule was live in Claude Code's memory for about a month and was applied one last time on the day it was retired.
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products), general

## The instance

A standing instruction told Claude Code: when a blog post is published, put the LinkedIn draft and headline options in a **GitHub issue**, not inline in the conversation.

Sensible when written. The reason was narrow and purely mechanical: **copying from a terminal scrollback is annoying.** An issue gave Dave a clean page to copy from on any device.

Then the surface changed. He started doing enough remote control through the **Claude for iOS app**, where copying straight out of the conversation is easy. The friction the rule existed to route around was gone — and the rule kept running anyway, generating an issue nobody needed.

Nothing failed. That's what makes it worth a card: **an obsolete convention doesn't announce itself.** It keeps producing plausible output, and the only signal is somebody eventually noticing it isn't buying anything.

## Why this bites instruction files specifically

`CLAUDE.md`, memory files, project conventions, and system prompts all accumulate rules the same way — each added at a moment when it solved something real. But:

- **The rule is durable; the constraint is not.** A tool upgrade, a new client app, a changed permission, or a fixed bug can dissolve the reason while the text stays put.
- **The reason usually isn't written down.** A rule recorded as *"post LinkedIn drafts as issues"* is unfalsifiable later — you can't tell whether it still applies, because you can't see what it was for.
- **Compliance masks obsolescence.** A model following a stale rule looks exactly like a model following a good one. There's no error to trip over.

## What to do about it

**Write the constraint, not just the rule.** Every convention should record *why* in a form that can be checked:

> ~~Post LinkedIn drafts as GitHub issues.~~
> Post LinkedIn drafts as GitHub issues **because copying from terminal scrollback is painful.** If that stops being true, deliver them inline.

That version carries its own expiry condition. The moment copying gets easy, the rule visibly no longer applies — no audit required.

**Re-read instruction files when the surface changes**, not on a calendar. The trigger isn't age, it's a change in how you work: a new device, a new client, a new access path, a newly capable model. Those are the moments when the largest number of workarounds silently stop earning their keep.

**Rename, don't just rewrite.** When a rule reverses, a file still named `linkedin-drafts-as-github-issues` now asserts the opposite of its own contents. Retrieval systems match on names and descriptions, so a stale name is a live hazard — delete and re-file under a name that states the current rule.

## Why this matters

Most guidance about instruction files is about what to put in them. The failure mode in practice is what stays in them.

A model is extremely good at following a rule you no longer need, and will do it indefinitely without complaint. The corrective isn't discipline — it's **making rules carry the condition that would falsify them**, so obsolescence shows up as a contradiction instead of as quiet, well-executed waste.
