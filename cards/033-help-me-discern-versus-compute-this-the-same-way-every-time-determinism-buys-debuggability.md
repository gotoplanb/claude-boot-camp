# "Help Me Discern" vs. "Compute This the Same Way Every Time" — Determinism Buys Debuggability, Not Correctness

**Source:** Beta, Claude Boot Camp session 2026-09-11, drawing the boundary on using chat + MCP instead of Salesforce rollups and reports.
**Type:** concept
**Verified:** `inferred` — a reasoned principle, not a measured finding. The underlying behaviour (a model reasons fresh each time and may frame the same question differently) is `ran-it` and unremarkable.
**Relevant to:** 1 (foundational concepts), 3 (administering — governance), 6 (Claude + Salesforce)

## The boundary

Ask a model *"who should I prioritise on this account?"* twice and you may get subtly different framing or emphasis. It's reasoning fresh each time, not running a fixed calculation.

That is **the feature** when you want discernment, and **a disqualifier** when you need reproducibility.

The line worth holding:

| | Reach for the model | Reach for a rollup / report / formula |
|---|---|---|
| The ask | *"Help me think about this account"* | *"Compute this the same way every time"* |
| Output feeds | A human's judgment | A decision, a metric, a downstream system |
| Varying answers are | Fine, often better | Unacceptable |
| Needs an audit trail | No | Yes |

## What determinism actually buys

Not correctness — **debuggability.**

> A brittle deterministic rollup is wrong in the *same* way every time, and that's what makes it fixable. You can point at the formula.

A model that reasons freshly is *usually* better and *occasionally* differently-framed, and there's no formula to point at. When someone asks "why did it say that?", the honest answer is a paragraph about context and emphasis, not a line of code. For a conversation that's fine. For anything that has to be defended, it isn't.

Note what this reverses: the deterministic option is often the *less accurate* one, and still the right choice. Consistency and inspectability are separate goods from accuracy, and governance usually cares about them more.

## The tell

Two questions settle it:

1. **Would anyone ever ask "why did it say that?" and need a defensible answer?**
2. **Does the output silently drive an action** — who gets called, what gets escalated, what a number in a dashboard says?

Either yes → it wants a real report or rollup. The danger isn't using a model for judgment; it's **judgment output quietly becoming an input to something that assumed determinism.** That drift happens without a decision: someone starts pasting the answer into a weekly update, and six weeks later it's a metric.

## Why this matters

It's the cleanest rule available for "when do I use an LLM at all," and it's more useful than accuracy comparisons because it doesn't move as models improve. A better model doesn't become reproducible; it becomes better at judgment. The boundary is a property of the *task's requirements*, not the model's quality.

For a center-of-excellence lead this is the governance answer worth having ready. "Where should we use this?" is usually asked as a capability question and is really this question — and the answer is stable enough to write into policy.
