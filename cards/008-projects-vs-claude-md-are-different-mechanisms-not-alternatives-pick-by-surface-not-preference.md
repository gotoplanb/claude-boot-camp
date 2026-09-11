# Projects vs CLAUDE.md Are Different Mechanisms, Not Competing Options — You Pick by Surface, Not by Preference

**Source:** Beta seminar session 2026-09-11, answering Dave's question about whether his context-loading habits are outdated. Beta explicitly flagged this comparison as worth its own card.
**Type:** concept
**Verified:** docs for the Projects half (see card 007); `ran-it` for the CLAUDE.md half — Dave has run CLAUDE.md-driven Claude Code for months across several repos.
**Relevant to:** 2 (operating Claude products), 5 (building with Claude Code)

## Content

These get discussed as if you choose between them. You don't — they belong to different surfaces and can't substitute for each other:

| | **Projects** | **CLAUDE.md** |
|---|---|---|
| Surface | claude.ai / Claude Desktop | Claude Code |
| Context source | Uploaded knowledge base + custom instructions | The file, plus whatever files Claude reads from disk |
| Freshness | Snapshot — you re-upload when things change | Always current; read from the working tree at run time |
| Versioned | No | Yes, it's in git with the code |
| Scales by | Automatic RAG at capacity | Reading files on demand; pointers to other docs |

The practical consequence: **if the material is a repo, CLAUDE.md wins**, because it can never be stale relative to the code — it lives in the same commit. If the material is stable reference documents you want available across many exploratory chats on a phone, Projects wins, because Claude Code isn't there.

### For the two-session split specifically

This maps cleanly onto card 001's pattern:

- **Code session** → CLAUDE.md. The repo is the context; nothing to upload.
- **Seminar chat** → a Project is a genuine fit. Load it with the stable reference material for the topic being learned, so exploratory chats across several days stay grounded without re-pasting.

So the answer for the boot camp isn't "Projects or CLAUDE.md" — it's **CLAUDE.md for the code session, and a Project for the seminar chat**, holding the curriculum outline and whatever docs are being studied.

## On the "outdated concepts" worry

The instinct to be sparing with context — hoarding the window, preferring pointers over inlining — comes from a real constraint that has substantially relaxed. Windows are far larger, and both surfaces now handle overflow themselves (RAG in Projects, on-demand file reads in Claude Code).

What hasn't changed: **relevance still beats volume.** A large irrelevant knowledge base makes retrieval worse, and a bloated CLAUDE.md spends attention on instructions that don't apply. The old habit was right for the wrong reason — the enemy was never size, it was noise.

`[confirm-this: the practical ceiling before a CLAUDE.md starts hurting rather than helping — is there a rough size where pointers beat inlining? Needs a real experiment, not a guess.]`

## Why this matters

"Which context mechanism do I use" is the first question anyone leading Claude adoption hits, and the wrong framing (treating them as rivals) produces bad advice — like uploading a repo snapshot into a Project, which is stale the moment someone commits.
