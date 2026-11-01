# Plugins Are the Floor and the CLI Is the Ceiling — So an Unmaintained Tool Degrades Instead of Failing

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01, extending card 086's package question. Supersedes nothing; it adds the layering those cards left open.
**Type:** decision
**Verified:** `n/a` for the stance. Two supporting claims are **not** settled: that repo-committed plugins load for anyone who checks out the repo is still `docs` (card 073's open question), and whether the customer's enterprise settings permit repo-level plugins at all is **unknown and decisive** — see the precondition. *[confirm-this: both of the above, and specifically whether a `doctor` command can meaningfully detect skill/CLI parameter drift or only surface-level mismatches.]*
**Relevant to:** 5 (building with Claude Code), 6 (Claude + Salesforce), 4 (integrating), 3 (administering)

## The structure

**Ship the process with the code.** Plugins — skills and recipes — live at the root of the repo, so any session that checks the repo out has them. A custom CLI (card 087) sits alongside and adds determinism.

> **The plugins are the floor. The CLI is the ceiling.**

Which produces the property the whole design is for: **if the CLI goes unmaintained, the system degrades instead of failing.**

## Why this structure

**Distribution rides with the code.** Pushing plugins through a separate org-level channel is slow, and it becomes a bottleneck on development itself. Anything that changes at the pace of the code should ship at the pace of the code.

That's a *second* argument for repo-committed skills, and a stronger one than card 073's. There the deciding factor was audience — who can reach the thing. Here it's **velocity**: even when both channels reach everyone, the org channel throttles iteration.

**Maintainability matches the audience.** Skills are mostly markdown, so a semi-technical admin can read and edit them through a pull request. The CLI is code and needs an engineer. The payoff is the alignment: **the layer that changes most often — the recipes — is also the layer non-engineers can edit.**

**It answers the handover problem.** This is the consulting insight, and it's easy to miss until it bites:

> A customer can *depend on* a custom CLI without being able to *support* it.

Build only the CLI and you've handed over a dependency with a maintainer-shaped hole in it. With the plugins as the floor, the process still works when nobody is maintaining the tool — slower and less deterministic, but working.

## How the two layers meet

- **The skill is the spec; the CLI is the accelerator.** Each recipe is *fully described* in its skill. The CLI implements the deterministic parts, and the skill calls it when it's available. Missing or broken CLI, and the skill still says how to proceed — with less determinism, not with nothing.
- **One source of truth where it's possible.** Two descriptions of one process drift (card 085's problem turned inward). Define recipe parameters once and consume them from both sides; have `doctor` (card 087) flag skill/CLI mismatches.
- **Version them together.** One repo, one version, one review — a recipe change and its code change land in the same pull request.
- **Changes go through review.** Recipe edits arrive as PRs, which gives the customer an audit trail and a natural place for admins to participate in their own process.

## The precondition — ask it in discovery

**Repo-level plugins must be permitted by the customer's enterprise settings.** There's no good workaround; routing distribution through an org-level channel is too slow to be the primary path.

So this is a **go/no-go question, asked early**, because the rest of the structure depends on the answer. If it's blocked, the recipes still exist as markdown in the repo and a session can be pointed at them — a real fallback, but weaker and less ergonomic.

Worth noticing that the fallback is *the floor without the automatic loading*. The layering survives even when the mechanism doesn't, which is some evidence the layering is the right idea.

## The takeaway

> **As autonomy increases, move more of the process from prompts into code. Keep the prompts as a working fallback, so the process never depends entirely on code someone has to maintain.**

That's card 076's build-time/run-time instinct with a durability argument attached. Moving work into deterministic code is right — and the version of it that survives contact with a client is the one that leaves a prose floor underneath.

See card 086 (the recipe this layers), card 087 (the CLI that is the ceiling), card 073 (repo-root distribution — this adds the velocity argument to its audience argument), card 076 (push determinism down, keep judgment up), card 085 (why one source of truth matters), and card 054 (the other handover question — what the customer is left holding).
