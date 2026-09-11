# Claude Code Publishes Artifacts Too — and an API-Key Session Can't. The Availability Gates Are the Gotcha

**Source:** Surfaced while verifying a question about the artifacts space, Claude Boot Camp session 2026-09-11 — this capability wasn't in the answer being checked. Verified against [Share session output as artifacts](https://code.claude.com/docs/en/artifacts).
**Type:** product-behavior
**Verified:** `docs` — read in full. **Not yet run** by Dave. `[confirm-this: publish one from a real session and check the /artifacts flow and same-URL republish.]`
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products)

## What it is

Claude Code can publish a session's output as a **live interactive page on claude.ai** — not just the chat app. Useful when terminal text is the wrong medium: an annotated PR walkthrough, a dashboard from data the session already pulled, several design options side by side, a progress board that fills in during a long run.

Mechanics worth knowing:

- **Updates republish to the same URL**, in place, while viewers have it open. Each publish is a version; the Share control picks which one viewers see.
- **`/artifacts`** lists everything you own or that's shared with you — reads from your claude.ai account, so it survives `/clear` and new sessions. `Enter` attaches one to the session; `Ctrl+]` reopens the most recent.
- **To update from a different session**, give Claude the URL or attach it via `/artifacts`. Without that, a new session creates a *new* artifact instead.
- It's **one self-contained page**: no backend, no routes, no relative links, 16 MiB rendered, and a strict CSP that blocks external images and most external scripts.

## The gates — this is the part that bites

Artifacts require **every** condition below. Miss one and Claude quietly writes a local HTML file or says it can't publish:

| Requirement | Detail |
|---|---|
| **Auth** | A session signed in to a claude.ai account via `/login`. **Sessions using an API key, an LLM-gateway token, or a cloud-provider credential cannot publish — at all.** |
| Plan | Pro, Max, Team, or Enterprise. On Enterprise an Owner must enable it. |
| Model provider | Anthropic API only. **Not** Bedrock, Vertex, or Microsoft Foundry. |
| Org policy | Not available with CMEK, HIPAA, or Zero Data Retention enabled. |
| Version | Claude Code ≥ 2.1.183 (desktop ≥ 1.13576.0). Off by default in Agent SDK, GitHub Action, and MCP-server contexts. |

The auth gate is the one to internalise: **`/login` vs. API key changes what the tool can do**, not just how it bills. That's a surprising coupling, and it means artifacts are unavailable in exactly the automation contexts (CI, SDK, gateway) where you might have assumed they'd be most useful.

The ZDR/HIPAA/CMEK exclusion matters for anyone advising regulated clients — artifacts are off the table there, which is worth knowing before demoing them.

## A constraint-harness detail worth stealing

Claude applies a built-in design skill when building an artifact, and **it looks for an existing design system in your project first** — tokens recorded in `CLAUDE.md` or a theme file take precedence over its own choices:

```markdown
## Design system
- Colors: primary #1a4d8f, accent #f59e0b, surface #f8fafc
- Typography: Inter for body, JetBrains Mono for code
- Spacing: 8px scale, 6px border radius
```

That's a textbook constraint harness (card 021): a small closed vocabulary in a file the model already reads, replacing a long instruction about visual taste that would have to win an argument every time.

## Why this matters

It changes what a Claude Code session can hand back. A long refactor or investigation currently ends in a wall of terminal scrollback that nobody else will read; an artifact turns the same work into a link a reviewer can actually look at — and card 022's whole argument is that seeing beats reading a description.

It's also a distinct mental model from chat artifacts, which is why it's easy to miss: same word, different surface, and the Claude Code version is the one that fits a build-and-review loop.
