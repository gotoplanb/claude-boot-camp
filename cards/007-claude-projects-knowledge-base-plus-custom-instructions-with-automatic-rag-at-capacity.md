# Claude Projects — A Knowledge Base Plus Custom Instructions Scoped to a Set of Chats, with Automatic RAG at Capacity

**Source:** Beta seminar session 2026-09-11; claims independently verified against [support.claude.com "How can I create and manage projects"](https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects).
**Type:** product-behavior
**Verified:** docs — read the official article and confirmed each specific below. Dave has **not** used Projects himself. `[confirm-this: paid-tier project count limits are not documented — only the free cap of 5 is stated.]`
**Relevant to:** 2 (operating Claude products)

## Content

A **Project** is a claude.ai / Claude Desktop workspace bundling three things around one context:

1. **Knowledge base** — uploaded documents, text files, or code snippets, available as context to *every* chat in the project.
2. **Custom instructions** — project-level guidance on how Claude should behave, applied to all conversations inside it.
3. **The chats themselves** — grouped together rather than scattered in history.

The point is that you configure context **once** instead of re-pasting it into every new chat.

### Verified specifics

- **Automatic RAG at capacity.** When project knowledge approaches the capacity limit *on paid plans*, "Claude will automatically enable RAG mode to expand your project's capacity." So the knowledge base is not purely stuffed into the context window — past a threshold it switches to retrieval. You don't manage this.
- **Free plans cap at five projects.** Paid-tier limits aren't documented.
- **Team/Enterprise sharing:** projects can be private (invited members only) or shared with the broader org; invitees get "Can view" or "Can edit."

### When to reach for it

- Recurring work you return to over days or weeks where the same background material matters every time — client engagements, a research thread, a writing project with a consistent style guide.
- When Claude needs durable reference material (specs, transcripts, prior output) without re-uploading per chat.
- Team scenarios where colleagues should chat against the same grounded context.

### When not to

- One-off or exploratory chats — the setup overhead isn't repaid.
- **Anything code-heavy that lives in a repo** — that's Claude Code's territory. See card 008; these are different mechanisms and conflating them is the common error.
- When the material changes constantly enough that keeping the knowledge base current becomes its own chore. Re-uploading can cost more than the grounding is worth.

## A note for the walk-and-listen workflow

Two of the links that circulate for this topic have moved: `support.anthropic.com/...` now 301s to `support.claude.com/...`, and the "Intro to Projects" support article redirects to **academy.claude.com**, where it is a **7-minute video, not an article**.

That matters for the Eleven Reader loop in card 003 — a video can't be fed to a text reader. The `support.claude.com` article above *is* text and works fine. Worth checking a link resolves to prose before queuing it for a walk.

## Why this matters

Projects is the answer to "how do I stop re-pasting the same background into every chat," and the automatic-RAG behavior means it scales past a context window without the user doing anything. Knowing the free cap and the sharing model matters for a CoE lead deciding how a team should organize work.
