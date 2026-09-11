# Never Point Claude at a Production Salesforce Org — Use a Scratch Org, Developer Edition, or Sandbox

**Source:** Dave, Claude Boot Camp session 2026-09-11: *"definitely make sure you're using a developer account or a sandbox, and you're not running this stuff against production to start, because you will break things unintentionally."*
**Type:** preference
**Verified:** `ran-it` — standing practice across Dave's Salesforce work.
**Relevant to:** 6 (Claude + Salesforce), 3 (administering — governance)

## The rule

**Claude gets a disposable org. Always, at least until you've built the gotcha corpus.**

This is not the generic "be careful in prod" reflex. Two things make Salesforce specific:

1. **The `sf` CLI is genuinely powerful**, which is exactly why it's good to work with (card 024) and exactly why it's dangerous. It can deploy metadata, modify permissions, and change org configuration in one command.
2. **Claude acts confidently through it**, and much of the damage isn't a failed command — it's a *successful* one with consequences nobody enumerated. Deleting a field takes data with it. Changing a permission set changes who can see what, right now, for real users.

The failure mode from card 024 compounds this. Claude reports accurate success on a step that doesn't add up to the intended outcome — and in production, the intermediate states aren't harmless.

## The progression

| Environment | Use it for |
|---|---|
| **Scratch org** | Day-to-day development. Disposable by design, defined in source, recreate in minutes when you wreck it — and you should expect to wreck it. |
| **Developer edition** | Learning, Trailhead work, elicitation runs (card 025). Persistent, free, and yours to break. |
| **Sandbox** | Client work and integration testing, once the change is understood. Real-ish data shapes without real consequences. |
| **Production** | Reviewed, promoted changes only. Not an environment Claude explores in. |

The corollary Dave runs in practice: **when development happens in a scratch org, the promotion to the parent/production org is its own tracked task.** Work isn't done because the scratch org looks right — the change still has to make a supervised crossing.

## Why this matters

For a consultant or SI, this is table stakes governance, and it's the first question a client's admin will ask. Getting it wrong once is unrecoverable in a way that outweighs every efficiency gain the tooling provides.

It's also the cheapest possible form of card 020's harness: a disposable org means the blast radius of an unsupervised mistake is "recreate the org," not "restore from backup and explain yourself." You can let Claude work freely *because* the environment is worthless.
