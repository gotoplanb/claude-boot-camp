# The Baseline Is Only True Until Someone Clicks Something — Agent Work Builds on a Drifting Snapshot

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** gotcha
**Verified:** `inferred` for the drift job and the capture-tool fallback — proposed, not built. The underlying problem is `ran-it`: Dave has hit hand-made org changes invalidating assumed state across his Salesforce work (cards 024, 025). *[confirm-this: current capabilities of Salesforce DevOps Center — the source was a secondary article and may be dated. Re-check before ruling it in or out.]*
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), 3 (administering)

## The problem

Agent work builds on a **snapshot** of the system. Humans keep changing the system by hand. The snapshot is therefore correct at the moment it's taken and decaying immediately afterwards.

In an org with two or three admins who click things, that decay is continuous and invisible. Nothing announces it. The agent's next change is built on a picture that stopped being true at some unknown point.

This is card 014's failure one level out. There, the *model* trusts its own stale snapshot of the world. Here, **the repo** is the stale snapshot — and the fix is the same in shape: close the gap structurally rather than asking anyone to remember.

## Practices

- **Bootstrap by pulling existing metadata into source control before anything else.** Nothing can be said about drift until there's a baseline to drift from. This also happens to be a visible early win for the customer, which is a good property for the first thing you do.
- **Run a scheduled job that retrieves current state and flags divergence** between the org and the repo. This is a natural unattended cloud task (cards 082, 084) — it's verifiable without a human by construction, since the output is a diff.
- **A change-capture tool is a reasonable fallback** if admins keep working by hand. Meeting the behaviour where it is beats a policy that assumes it stops.

## On visual pipeline tools

Judged not worth the overhead early on, **given that Claude is the primary developer and works through CLI workflows.** The tracker's statuses plus the CI pipeline already form the state machine; a second visual one is duplicate state with a sync problem.

Stated as a decision rather than a fact, and with a shelf life: **these products change, so re-check before ruling one out.** The `confirm-this` above exists because the information behind this call came from a secondary source.

## Why this matters

It names the thing that makes platform automation different from application development. In a repo, the code is the system — if it's committed, it's true. In a configured platform, **the repo is a model of the system, and the system has other authors.**

Any plan that assumes the repo is authoritative is making a claim about human behaviour, not about tooling.

See card 014 (the same drift, one layer in), card 024 (what a half-configured org does to an agent's assumptions), card 025 (mining what the admins know but haven't written down), card 084 (criterion 2 — the hidden prerequisite this creates), and card 022 (observe reality rather than reasoning about what should be there).
