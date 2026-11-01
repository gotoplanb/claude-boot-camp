# Parallelism Has Two Sources — and the Contended Resource Is Rarely the Files

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** concept
**Verified:** `ran-it` for local contention — Dave runs many parallel tmux sessions daily and hits port, database and attention conflicts. `inferred` for the shared-org claim and the tracker-vs-spreadsheet point; neither has been run with multiple agents against one Salesforce org.
**Relevant to:** 5 (building with Claude Code), 6 (Claude + Salesforce), 3 (administering)

## Two different things called parallelism

- **On a team, the humans supply it.** Five people, five machines, five sessions. Nothing special is required.
- **Solo, the cloud supplies it.** One person fanning out across several sessions (card 082).

These feel like the same capability and they aren't. The first scales with headcount and needs *coordination*; the second scales with money and needs *isolation*.

## What actually collides

**Locally, parallel sessions fight over** ports, databases, tunnels — and the human's attention, which is the scarcest of the four and the one nobody budgets for.

**On a platform, the files aren't the problem.** This is the part worth carrying:

> In Salesforce work the contended resource is **the shared org**, not the source tree.

Several agents deploying into one sandbox clobber each other, and git can't help — the conflict isn't in the repo, it's in the system the repo deploys to. Which means the unit of isolation is **one scratch org or dev sandbox per agent or developer**, not one branch per agent.

That generalizes past Salesforce: for anything that deploys into a shared mutable environment, ask what the agents are actually contending for before assuming version control has it covered.

## Claiming

Agents working from the same backlog need an explicit claim convention or they build the same thing twice (card 081).

**A spreadsheet is racy** — two readers see "unclaimed," both write "claimed," and neither sees the other. Fine when humans are slow and talkative; not fine when agents are fast and silent.

A real tracker with atomic state transitions is the safer substrate once agents are involved. **The race existed before, but humans covered it with conversation**, and agents don't do that.

## Why this matters

It stops two bad instincts. The first is treating "we can run things in parallel now" as unambiguously good — parallelism converts a scarce resource into a contended one, and the useful question is *which* resource. The second is assuming source control makes concurrent agents safe, which is only true when the thing being changed is the source.

See card 081 (the handoffs that force claiming), card 082 (where fan-out comes from), card 026 (scratch orgs and sandboxes — the isolation unit), and card 024 (what a half-deployed shared org looks like afterwards).
