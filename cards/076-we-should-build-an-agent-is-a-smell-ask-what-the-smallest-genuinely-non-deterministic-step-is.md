# "We Should Build an Agent" Is a Smell — Ask What the Smallest Genuinely Non-Deterministic Step Is

**Source:** Dave, from a walk-and-talk with friends now using LLMs heavily in programming, 2026-09-26. The build-time reframe and the smallest-step framing are Beta's, same session.
**Type:** pattern
**Verified:** `ran-it` for the underlying split — card 048 is a shipped system built this way. `inferred` for the sorting case below, which is an illustration nobody has run.
**Relevant to:** 1 (foundational concepts), 5 (building with Claude Code), 3 (administering), general

## The phrase

*"We should build an agent to do X"* has become a catchphrase — it turns up in requirements documents as though it were a specification. It isn't one. It names a technology, not a problem.

The question it skips:

> **Is X constrainable and deterministic — or close enough that what you actually want is for the model to *build you* the deterministic thing?**

Cards 048 and 033 already answer *why* that matters (cost shape; debuggability). This card is about the **trigger** — the sentence that should make you ask, before anyone starts building.

## The worked example, and why it's the sorting one

The arithmetic version is a strawman: *"you could prompt a model for 1+1, but a function is faster and cheaper."* Nobody actually proposes that, so it convinces no one.

**"Build an agent that sorts our data" is a sentence people genuinely write.** And the naive reading produces something that works, costs per call, and is far slower than the built-in sort.

But the interesting answer isn't "just use the built-in sort" either. Your language's sort is a *generalized* sort, carrying assumptions about data it knows nothing about. So:

> Have the model **analyze a representative dataset and tell you which sort is right for this data** — then ship that deterministic implementation.

You are not asking the model to sort your data. You are asking it **which algorithm to use**, once, offline — and then running the answer forever at no per-call cost.

That's a third flavor of build-time use, distinct from the two in card 048. There the model *generates an artifact* (text) or *is replaced by* a transformation. Here it does the **optimization reasoning** and hands back an algorithm choice.

**The eval is not optional, and it's easy to design wrong — see card 077.**

## The answer is three-way, not binary

This is not "agents bad, scripts good."

| If the task is | Then |
|---|---|
| Statable as a rule | Encode the rule. No model at run time (card 048). |
| Statable *once you've thought hard about your data* | Use the model **at build time** to do that thinking; ship the deterministic result. |
| Irreducibly a judgment | Keep the model — **for that step only.** |

The third row is where most real systems land, and it's the one the catchphrase obscures. The mature version is the pipe-and-filter shape from card 070: **find the smallest genuinely non-deterministic step and wrap it**, with deterministic code upstream and downstream.

"Should we build an agent" usually has the middle answer — yes, keep a model in the loop, but only at the one point that actually needs judgment.

## Why this matters

The phrase is doing real damage in requirements, because it specifies a *mechanism* before anyone has asked what kind of work the task is. Teams end up with a per-call model invocation wrapped around something a function could have done, and the inefficiency is invisible — it works, so nobody revisits it.

The reframe is cheap: *what is the smallest part of this that genuinely cannot be written down?* Everything else compiles.

See card 048 (the cost economics), card 033 (why determinism is worth having even when it's less accurate), card 077 (**how the eval for this goes wrong**), card 070 (chaining — orchestration vs. pipeline), and card 063 (the same instinct applied to a UI: use the flexible tool once, keep the deterministic artifact).
