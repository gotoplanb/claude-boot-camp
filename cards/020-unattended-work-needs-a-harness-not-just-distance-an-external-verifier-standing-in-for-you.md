# Unattended Work Needs a Harness, Not Just Distance — an External Verifier Standing In for You

**Source:** Beta, Claude Boot Camp session 2026-09-11, refining Dave's attended/unattended axis after comparing two Fable tasks that felt different despite both being large and autonomous.
**Type:** pattern
**Verified:** `ran-it` for the observation that prompted it — Dave's SonarQube/test-coverage review on Fable felt like a poor fit despite being exactly the "big autonomous task" the axis predicted. `inferred` for the generalisation. `[confirm-this: run the Python-prototype → Fable-ports-to-Rust task and see whether the test suite really does substitute for watching.]`
**Relevant to:** 5 (building with Claude Code), 1 (foundational concepts), general

## Content

"Attended vs. unattended" (card 018) is the right axis but an incomplete test. Task *size* doesn't decide whether you can walk away. **The presence of an external verifier does.**

> **Unattended works when something other than you can tell whether it worked.**

Tests, a build, a lint pass, a type-checker, a diff against a golden output — any objective pass/fail signal that stands in for your judgment.

### The two Fable tasks that look alike and aren't

**Port a Python prototype to Rust.** Autonomous, long-running, and *bounded*: the existing test suite is an objective pass/fail signal. You don't have to watch the tool calls to know whether it worked — you run the tests. Genuine fire-and-forget.

**Review SonarQube findings and test coverage.** Also autonomous and long-running, but the deliverable is a **judgment**: did it review this well? There is no test that answers that. The only available verifier is *you reading the output* — which is precisely the supervision you were trying to avoid.

Same model, same scale, opposite fit. The difference isn't capability, it's whether the "did it work" question has an answer outside your head.

### The diagnostic

Before handing something off unattended, ask: **what will tell me this succeeded, other than my own reading of it?**

- Concrete answer (the suite goes green, the build passes, output matches the golden file) → safe to walk away.
- No answer → you're going to end up supervising regardless, so plan for that: pick an attended model and workflow, or **build the verifier first** and only then hand it off.

That second option is the interesting one. A task without a harness is often a task where the harness is the missing work — writing the characterization tests before the refactor converts a supervise-me job into a walk-away job.

### Why the SonarQube case felt like friction

Card 018 recorded that Fable "goes into a hole" on some work. That framing was uncharitable and slightly wrong: it reads as a capability limit. It isn't — Fable can do the review fine. The friction was a **fit mismatch**: watching it closely for a task the model is tuned to run unattended. The discomfort was structural, not a shortfall.

## Why this matters

The naive version of autonomy is "give the big job to the strong model and leave." That produces the worst outcome: you don't watch, nothing else checks, and you discover the problem late — or you *do* watch, and get no benefit from the autonomy you paid for.

Reframing it as *where is the verifier* also makes the harness a first-class design decision rather than an afterthought. It's the same shape as card 014's lesson — some problems can't be prompted away, only designed away — applied to trust rather than state. If you want to walk away, build the thing that watches in your place.
