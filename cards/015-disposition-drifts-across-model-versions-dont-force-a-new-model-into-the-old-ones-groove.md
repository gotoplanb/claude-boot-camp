# Disposition Drifts Across Model Versions — Don't Force a New Model Into the Old One's Groove

**Source:** Dave, Claude Boot Camp session 2026-09-11, on working across Opus 4.5 → 4.6 → 4.7 → 4.8 and into 5.
**Type:** preference
**Verified:** ran-it — lived across several model releases on long-running projects (the book, this website, client work).
**Relevant to:** 1 (foundational concepts), 3 (administering — model policy), general

## Content

Capability climbs roughly monotonically with each release. **Disposition does not.** Models differ in how they approach a problem, not only in how well — the shape of the answer, how much they volunteer, what they reach for first.

Two consequences follow, and the second is the one people resist.

### 1. Your context has to move with the model

Project instructions, `CLAUDE.md`, and uploaded knowledge bases are all **snapshots tuned against one model's disposition**. Guidance that steered 4.5 beautifully can be neutral or counterproductive on 4.8 — some of it is correcting a tendency the new model no longer has, which means it's now pushing against nothing, or pushing the wrong way.

So when a new model arrives and things feel worse: the honest first hypothesis is *my context is tuned for the last one*, not *this model is worse*. Re-read the instructions with fresh eyes and delete the ones that were fixing yesterday's problem.

Approach a new model with curiosity rather than correction. If it's solving something differently, look at *why* before you steer it back — on the assumption that capability is improving, its way may be better than the groove you'd worn.

### 2. Staying on an older model mid-project is a legitimate engineering call

If a task runs for days or weeks, you are allowed to finish it on the model you started with — even if you're a compulsive updater. The mid-project switch costs re-tuning exactly when you're deepest in a groove, and the friction arrives disguised as "the new model is worse."

This is the same reasoning as not bumping a major dependency in the middle of a sprint. Not stubbornness: sequencing. Upgrade at a seam, not mid-stride.

## Why this matters

The failure mode is subtle and self-inflicted: force-upgrade mid-project, keep the old context, hit friction, and misattribute it to the model. You then spend real effort steering a more capable model back toward an older model's habits — paying for the upgrade twice and getting worse output than either would give alone.

It also argues for treating project instructions as **living and dated**, not written once. Same logic as the card corpus: a note from fourteen months ago against a product that has moved should look suspicious, not authoritative. (See card 005 — staying flexible as the tools shift is the stance; this is its operational form.)

Sibling to card 014: there the mismatch is between the model's snapshot and the world, here it's between the model and its own previous version. In both cases, standing instructions are the wrong tool — close the gap or accept the model on its current terms.
