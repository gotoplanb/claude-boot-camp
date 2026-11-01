# Ship the Process as a Package — the Recipe Pairs the Intake Questions With the Build Steps

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** decision
**Verified:** `n/a` for the stance. The mechanics it assumes are `ran-it` from the release plugin (cards 069, 070): skills, packaging, and distribution all behave as described. The engagement-specific parts — role-separated environments, version-stamped tickets — are **not yet built**.
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), 4 (integrating), 3 (administering)

## The unit

**Distribute recipes, templates and tooling together. The environment itself is the package.**

The load-bearing decision is what a *recipe* contains:

> One recipe defines **both** the intake template — what to ask the requester — **and** the build-and-verify steps.

Keeping them in one artifact is what stops them drifting. Split them and the questions stop matching what the build actually needs, slowly, without anyone noticing until a ticket arrives that can't be built from its own answers.

## The pieces

- **A skill handles conversational intake** and plain-language explanation. It asks the clarifying questions, so ambiguity is caught *before* build time. That's grooming (card 081's front) made cheap and repeatable — and it's a correct use of a skill, because deciding which question to ask next is judgment.
- **A custom CLI handles the deterministic steps**: validating tickets, calling the tracker, running platform commands. See card 087 for what that CLI should look like.
- **Stamp the recipe version on each ticket.** Without it, a ticket built last month is unreproducible — you can't tell which version of the process produced it, and card 085's drift problem now applies to your own tooling.
- **Separate environments by role.** The requester-facing environment has tracker access and *no org credentials*; the build environment has sandbox access. That's card 039's least-privilege argument applied to the two halves of your own pipeline rather than to a vendor's product.

## The division that runs through all of it

> **Prompts are for judgment. Code is for determinism.**

The skill asks the questions because the right follow-up depends on the answers. The CLI runs the deploy because a deploy should do the same thing every time. Card 076 is the general form; this is what it looks like once it's a shipped package rather than an argument.

## Why this matters

It's the difference between a consultant who knows how to do the work and a consultant who leaves behind something that does the work. The second one is a package — and the package is only coherent if intake and execution ship as one thing.

See card 087 (the CLI half, in detail), card 076 (judgment vs. determinism — the general rule), card 081 (what the intake skill is doing in the larger loop), card 073 (how a package like this reaches people), card 069 (what a skill actually is), and card 039 (why the environments are split by role).
