# The Line That Decides Skills vs. Custom MCP Is Your Claude Org Boundary, Not Your Plan Tier

**Source:** Dave, Claude Boot Camp session 2026-09-14. Filed as a correction: the session opened on the assumption that custom MCP was the workaround for Enterprise-tier gating, and that turned out to be the wrong axis entirely.
**Type:** decision
**Verified:** `n/a` for the stance. The supporting product claim — that **Team** plan owners, not only Enterprise, can build and distribute plugin marketplaces org-wide — is `docs`. *[confirm-this: verify on a real Team tenant; it attaches to the Team/Enterprise session already queued in GAPS.md.]*
**Relevant to:** 4 (integrating — MCP and APIs), 3 (administering), 2 (operating Claude products), general

## The correction

The question *"Skills or a custom MCP server?"* looks like a budget question. It isn't.

The first framing was: Skills distribution is gated behind Enterprise, so a custom MCP is how you avoid paying for it. **That's wrong on the facts** — plugin marketplaces are available to Team plan owners too, not just Enterprise — and wrong in shape, because plan tier was never the thing doing the work.

**The actual dividing line is whether the people you're serving are inside your Claude org.**

| Who you're reaching | What to build |
|---|---|
| Just you | A personal skill. There is no distribution problem to solve. |
| Your team, same Claude org (Team *or* Enterprise) | Plugin marketplace — bundle skills, connectors and commands; distribute by ZIP upload or GitHub sync; control access per group. Officially supported; don't hand-roll around it. |
| People outside your Claude org entirely | Custom MCP server. It's just a URL, so it needs no org membership. |

That last row is the real gap a custom MCP fills. Not cost — **reach**.

## The trap: building a custom MCP to dodge per-seat cost

The tempting middle case is a large org with many light or occasional users: keep them off Team seats, serve them through a custom MCP instead.

**Probably don't.** It pencils out on a spreadsheet and then you own a server — auth, uptime, versioning, support, the security posture from cards 039–043 — to avoid a per-seat line item. The engineering and maintenance overhead will usually exceed the seats you saved. It's a "maybe" only when the usage pattern is extreme enough that the arithmetic isn't close.

The general form: *don't take on an ongoing engineering obligation to avoid a recurring bill of comparable size.* A bill is someone else's operational problem; a server is yours forever.

## The case where custom MCP is genuinely necessary

**External customers and clients** — people who are never going to be in your Claude org, no matter the budget. A support desk where clients file tickets, check status, and leave comments is the clean example.

Here custom MCP isn't the cheaper option, it's the *only* option. Which is what makes it the honest use case: the decision is forced by who the users are, not chosen to save money.

## Building it is not the same decision as listing it

Once built, distribution is a separate call. A public marketplace listing is available — and often you don't want it:

- **Too early** — discoverability before the thing is ready buys nothing but support load.
- **You already know exactly who needs it** — a fixed client list needs delivery, not discovery.

Unlisted-but-reachable is the default worth reaching for, and it's the same instinct as card 055: the venue already decided publishability, so the remaining question is only whether to advertise. Don't conflate "I built a server anyone could connect to" with "I want anyone to find it."

## Why this matters

The wrong axis produces the wrong build. Framing this as a cost question leads you to write a server you didn't need; framing it as an org-boundary question usually reveals that the supported path already covers you — and, in the one case it doesn't, that you have no alternative and should stop comparing.

Card 059 is the other half: once you know *which* to build, that card is the one that tells you what each costs you in context.

See also card 037 (org identity and admin-provisioned bundles — the same boundary from the access side), card 045 (Cowork solves access control *for* you, inside the org), card 012 (a curated Project upload — the distribution with no infrastructure at all), and card 035 (cost *shape*, the related trap about how pricing changes behavior).
