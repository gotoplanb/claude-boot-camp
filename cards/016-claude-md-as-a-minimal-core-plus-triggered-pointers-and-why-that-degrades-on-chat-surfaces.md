# CLAUDE.md as a Minimal Core Plus Triggered Pointers — and Why the Same Pattern Degrades on Chat Surfaces

**Source:** Dave, Claude Boot Camp session 2026-09-11, on structuring context for long-running projects. The chat-surface caveat is Beta's, same session.
**Type:** preference
**Verified:** ran-it — the structure Dave uses across long-running repos. The chat-surface half is `inferred` from card 011's passive/active distinction, not measured. `[confirm-this: how reliably does a Projects chat actually follow a pointer to a connector-backed doc?]`
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products), general

## Content

On a large, long-running project, `CLAUDE.md` should hold **only what every single session needs.** Everything else becomes a pointer.

The shape is a table of contents with **triggers attached**:

> *Preparing a version release? Read `docs/release-checklist.md` first — it covers pre-release checks and the documentation writing.*
> *Patch release? Don't bother.*

The release checklist is long and full of detailed examples. It's exactly right when cutting a release, and pure waste when knocking out patch versions or building a feature. Loading it every session to be safe means paying for it every session.

### Why this works where "always re-verify" doesn't

Card 014 showed that a standing instruction to re-check external state mostly loses. This looks similar but behaves completely differently, and the distinction is worth being precise about:

- **"Always check X"** — a standing vigilance instruction with no trigger. Competes with the model's default posture on every turn and usually loses.
- **"When doing Y, read path Z"** — a concrete action bound to a recognisable condition. Claude Code reads local files cheaply and reliably, and "open this path" is unambiguous.

**Triggered concrete action beats standing vigilance.** That's the reusable form of the lesson.

### The same pattern is weaker on chat surfaces

In Claude Code, "go read this doc" resolves to a guaranteed-available local file read. In a Claude Project it resolves to *maybe* — the pointer only works if a connector (Google Drive, a URL fetch, an MCP server) is attached **and** the model chooses to call it. That's active retrieval (card 011), with its omission failure mode: no error, just an answer produced without the document.

So the design rule for Projects:

> **Progressive disclosure is only as reliable as the fetch step under it.**

Prefer putting genuinely-needed material in the knowledge base *passively*, where it's present without a tool call, and reserve pointers for the long-tail material where an occasional miss is acceptable. A TOC pointing at uploaded files is stronger than one pointing at things that require a live fetch.

## Why this matters

Two problems at once. Context budget: heavyweight reference material shouldn't tax every session for the few that need it. And staleness: a pointer resolves to the current document, while pasted content is frozen at upload time — which matters more on chat surfaces, where knowledge bases are snapshots (card 007).

The trap is assuming the pattern transfers unchanged across surfaces. It's the same *idea* everywhere and a different *guarantee* on each one.
