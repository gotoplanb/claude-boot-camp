# Chat vs. Cowork Is a Task-Shape Question, Not an Access Question

**Source:** Dave asking where the line sits, Claude Boot Camp session 2026-09-11. Gmail behaviour verified against [Use Google Workspace connectors](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors) — and it corrects what was said in session.
**Type:** concept
**Verified:** `docs` for the Gmail correction. `inferred` for the task-shape split — reasoned from stated capabilities, not measured. **Not run.**
**Relevant to:** 2 (operating Claude products), 1 (foundational concepts)

## The line isn't access

The same Google Workspace connectors are available in both. Whatever Chat can reach, Cowork can reach. Choosing between them on access grounds is a category error — the difference is **the shape of the task**.

## Chat, when the work is a question

- **A lookup or single turn.** *"What's on my calendar Thursday?"* *"Find Sarah's email about the contract."* One question, one connector call, an answer.
- **The answer belongs in the conversation**, not in a file.
- **Nothing needs to touch your machine.** Chat reaches systems through connector APIs; it has no local file access.

## Cowork, when the work is a job

- **Multiple chained steps.** Check Calendar for open slots → cross-reference a Drive doc for attendee context → draft the invite → produce a prep doc. Not one call answering one question.
- **It should run unattended.** Close the laptop and it continues, including on a schedule for recurring work.
- **The output is a real file**, and Cowork has **local file access** — it reads, creates, and organises files on your machine as well as in Drive.
- **Independent work can run in parallel** rather than as sequential chat turns.
- **You want to start it from your phone** and have it work on your desktop while you're away — functionally a managed version of the tunnel pattern in card 040.

## The test

> If you'd normally do it by **typing a question and reading the answer** — stay in Chat.
>
> If you'd normally do it by **opening three tabs, copying between them, and coming back in an hour** — that's Cowork's lane.

## Correction: Gmail is not drafts-only

Stated in session, and wrong. The docs are explicit:

> *"Claude can search and read emails using natural language queries, and **send, reply to, and forward emails** from Gmail."*
>
> *"Claude can send, reply to, and forward emails, but **only does so with your explicit approval by default**."*

So the guardrail is **human-in-the-loop approval, not a capability ceiling.** That's a meaningfully different safety property, and the difference matters in both directions:

- Better than drafts-only in practice — it can complete the task rather than leaving you a half-finished one.
- Weaker than it sounds — *"by default"* implies the approval step is configurable. Anyone reasoning about blast radius should check whether it's still on, not assume the ceiling is structural.

Assuming a capability limit where there's actually an approval prompt is the kind of mistake that survives right up until someone changes a setting.

## Why this matters

The task-shape framing scales past these two products: it's the same attended-vs-unattended axis as card 018, and the same *does-an-external-verifier-exist* question as card 020. Cowork's unattended mode is only as safe as what checks its work.

And the Gmail correction is a small lesson with a general shape — **"it can't" and "it asks first" are different guarantees**, and product summaries blur them constantly. Only one of them survives a configuration change.
