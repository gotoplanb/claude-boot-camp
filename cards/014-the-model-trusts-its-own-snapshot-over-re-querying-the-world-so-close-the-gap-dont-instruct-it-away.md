# The Model Trusts Its Own Snapshot Over Re-Querying the World — Close the Gap Structurally, Don't Try to Instruct It Away

**Source:** Dave, Claude Boot Camp session 2026-09-11, on why `CLAUDE.md` instructions to "check git before asserting branch state" never stick. Mechanism refined by Beta in the same session.
**Type:** gotcha
**Verified:** ran-it — Dave has hit this repeatedly across projects. Also reproduced live *during this session*: he edited a blog post directly on GitHub, and the Claude Code session had no idea until he said "be sure to pull main to get my change first." The *mechanism* below is `inferred`. The behaviour is not.
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products), general

## The behaviour

Work outside the session and the model's picture of the world goes stale, confidently. Commit and push from a second terminal, and Claude Code will later announce "we have these uncommitted changes on the branch" — for work that is already committed, pushed, built, and deployed.

The obvious fix fails. Putting a standing instruction in `CLAUDE.md` — *"before asserting what's on the branch, run `git log` and confirm"* — does not reliably work. Not occasionally-unreliable: mostly-doesn't-work.

## Why instructions lose

The model builds a working snapshot of the world as it goes, and treats its own earlier turns as high-confidence ground truth. Re-verifying costs tool calls and returns, nearly always, the same answer it already believed — so there's no signal pushing it to re-check.

A standing instruction has to win that argument on *every single turn*, against a default posture of trusting its own running state. It loses most of them. This isn't disobedience; it's a bad fit between the instruction and how the context is maintained.

## The fix: don't let state diverge

**Keep the changes inside the session.** Even a one-line edit that would be faster to type in VS Code is usually better made through Claude, because it keeps the session's picture current. The cost of the extra round trip is much lower than the cost of a divergence you then have to notice and repair.

When you do work outside — and sometimes you must — **say so explicitly**: "I edited X on GitHub, pull first." Explicit and in-turn beats standing and ambient, every time.

## Why this matters

The general principle is bigger than git:

> **Some classes of drift can't be prompted away, only designed away.**

If a failure mode requires the model to remember to check something on every turn, an instruction is the wrong tool. Either close the gap structurally (keep the work in-session), make the check deterministic (a hook, which the harness runs rather than the model choosing to), or accept that you'll have to say it out loud each time.

Card 017 is the deterministic version of this fix: a `SessionStart` hook on the `compact` matcher injects real git state as context, so the harness supplies the truth rather than the model remembering to ask for it.

This is the practical, load-bearing version of "write better instructions" advice — which is often just an invitation to keep losing the same argument more verbosely. See card 015 for the sibling case, where the mismatch is between the model and its *previous version* rather than the world.
