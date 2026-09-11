# Artifacts Are an Output Tray, Not a Knowledge Base — There Is No Path From an Artifact Into Project Context

**Source:** Dave asked whether an artifact can be fed into a Project's knowledge base (Claude Boot Camp session 2026-09-11). Beta answered; verified 2026-09-11 against [Publish and share artifacts](https://support.claude.com/en/articles/9547008-publish-and-share-artifacts) and [Share session output as artifacts](https://code.claude.com/docs/en/artifacts).
**Type:** product-behavior
**Verified:** `docs`. **The central claim is a negative** — see the shelf-life warning below.
**Relevant to:** 2 (operating Claude products), 4 (integrating)

## The answer

**No.** There is no documented action that sends or links an artifact into a Project's knowledge base. Download and re-upload is the path.

Artifacts and Project knowledge are **two separate systems that don't talk to each other.** An artifact lives in your artifacts gallery ([claude.ai/code/artifacts](https://claude.ai/code/artifacts)) as a published page; project knowledge is uploaded files scoped to a project's chats (card 007). Nothing bridges them.

> ⚠️ **This is a negative claim, and negative claims about product features have the shortest shelf life in this corpus.** "Feature X does not exist" becomes wrong the moment it ships, silently, with no error to catch it. Re-check before relying on this; do not cite it as settled a year from now.

## What the artifacts space actually is

An **output tray**, not working memory: a place to find the finished thing and hand it off.

- A gallery of everything you've created, across chats and sessions.
- **Sharing is what it's built around.** On Pro/Max, a public link is the *only* sharing option. On Team/Enterprise, you grant access to specific people or the whole org, and public sharing is off until an Owner enables it.
- Versions: each publish is a version, and you choose which one viewers see.

Two corrections to the description that circulated in the session:

- **"Remix" is gone.** The docs state plainly that *"The 'Remix' button is no longer available."*
- **An "Inspiration tab" for browsing others' published artifacts is unconfirmed.** It appears in neither article I read. `[confirm-this: does a public discovery/browse surface exist? If so it isn't in the publish-and-share docs.]`

## The consequence

If your workflow is *"generate reference material as artifacts, then use it to ground future chats,"* that loop doesn't close. You'd be exporting and re-importing by hand every time the material changes — a snapshot problem on top of a manual step (card 007).

**Artifacts are the wrong home for a permanent reference library.** They're a scratch space for something you're actively iterating on in one session, or a finished deliverable you hand to someone. Anything meant to ground future work belongs somewhere with a real retrieval path: a repo, a Project knowledge base, or an MCP server (card 011, card 012).

This is exactly the call already made for the boot camp cards — they live in git, publish to the site, and serve over MCP. An artifact of a card would be a photograph of it.

## Why this matters

It's a clean case of **two surfaces that look adjacent and aren't connected**, which is where people lose time. Both hold content, both live in claude.ai, both are things Claude made — and the absence of a link between them is invisible until you go looking for the button.

The general lesson from card 011 applies: ask what the *retrieval path* is. Artifacts have a sharing path, not a retrieval path. That single distinction predicts everything above.
