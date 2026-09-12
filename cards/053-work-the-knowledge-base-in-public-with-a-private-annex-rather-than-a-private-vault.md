# Work the Knowledge Base in Public, With a Private Annex — Rather Than a Private Vault

**Source:** Dave, Claude Boot Camp session 2026-09-12: *"Many people put this into a private knowledge repository like Obsidian. I prefer to just work in public."*
**Type:** preference
**Verified:** `ran-it` — this corpus is the instance. Public repo, CC BY-SA, rendered on the site, served over MCP, with a separate private repo for anything that can't be public. The reasoning below is Dave's own, given in his words.
**Relevant to:** general, 2 (operating Claude products), 4 (integrating)

## The fork

Most people building a personal knowledge base reach for a **private vault** — Obsidian, Notion, a local folder. Perfectly reasonable, and the default.

The alternative taken here: **the corpus is a public git repo**, licensed CC BY-SA (card 013), rendered on a public site, and served over a public MCP endpoint. Working in the open is the default; privacy is the exception.

## What it isn't

**Not "everything is public."** The arrangement is public-by-default **with a private annex**:

- **Public** — the card corpus, the curriculum, the labs, the book.
- **Private** — a separate repo for unpublished drafts, client material, and personal documents that have no business being readable. (This website's own repo is private for exactly that reason.)

The discipline is choosing the right home *at filing time*, not treating one as the overflow of the other. A card that can't be public is a signal to put it in the private annex — not a reason to abandon public-by-default.

## The reasoning — receiver-side bias

Dave's own framing, and it borrows vocabulary from his book's Shannon-Weaver work on **sender-side encoding** and the **prepared receiver**:

> *"I am just very receiver-side biased. If I do all of my work in process privately and generate a single output in the way that I want it to look, that's my sender-side bias of how I want people to read me."*

Working privately and publishing one polished artefact is a **sender-side** act: you control the framing, the sequencing, the impression. That's the correct posture *if the goal is to push a specific framing of yourself.*

His goal is different — **share knowledge, and discuss knowledge and perspectives with others.** Once that's the goal, sender-side polish stops being value-added and starts being overhead, because it's optimising a variable the reader doesn't care about.

Two things follow, and the second is the one that actually drives the behaviour:

**The cringe threshold is set deliberately low.** Rough presentation or a few errors don't bother him, because *"the overall value of the information outweighs any structural criticisms by orders of magnitude."* That's a judgment about relative magnitudes, not indifference to quality — and it's what makes publishing-before-polish tolerable.

**Deliberation is friction, and friction costs output.** *"By working in public and just putting my perspective out there, it keeps me going and talking and doing things faster. If I was very deliberate in the final image, I'm just gonna do less, because I'm putting a psychological and emotional friction on myself that I feel is silly."*

That's the claim that actually drives it: the cost of a private-then-polish workflow isn't the polishing time, it's **everything that never gets written because the polishing step is waiting at the end of it.** A private vault has no such gate — which is precisely why the gate is invisible until you notice your output rate.

## What the public choice also buys

Consequences this corpus demonstrates, independent of the reasoning above:

- **It's already an interface.** A public corpus can be rendered on a site, searched, and exposed over MCP (cards 011, 012) without an export step. A private vault would need a publishing pipeline bolted on to reach any of that — the thing you'd build later is already built.
- **No migration problem.** Markdown in git outlives whichever tool is fashionable. Vault formats are portable in theory and sticky in practice.
- **Others can actually use it.** A licence plus a URL means a card can be cited, forked, or dropped into someone's Project. A vault can only be described.
- **A quality floor.** Writing that might be read gets written more carefully. Worth noting this cuts against the friction argument above — the trick is letting it raise the floor without becoming the gate.

## The honest costs

- **Self-censorship is real.** You will hesitate to file things you'd have written freely in private — half-formed opinions, unflattering mistakes, client specifics. The private annex is what keeps that from silently narrowing the corpus, but only if you actually use it.
- **Public writing is slower per card.** This corpus's cards are longer and more carefully sourced than private notes would be. That's mostly good and definitely not free.
- **Confidentiality is a standing judgment call**, made per card, forever. A vault asks it once.

## Why this matters

The choice is usually framed as a personality trait — "some people share, some don't." It's better understood as **choosing where the publishing cost lands**: pay it continuously by writing in the open, or pay it in a lump later when you want the knowledge to reach anyone but yourself.

This corpus is the argument for paying continuously. Fifty-plus cards in, it's simultaneously a private reference, a public resource, a curriculum source, and a retrieval endpoint available from any project — with no export step between those uses, because there was never a private-only version to convert.
