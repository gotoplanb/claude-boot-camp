# Extended Case Studies Belong Beside Labs — the Brokerage Build as the First One

**Source:** Dave, Claude Boot Camp session 2026-09-12, on curriculum structure. The case study is [AI at Build Time, Determinism at Run Time](https://davestanton.com/blog/ai-at-build-time-determinism-at-run-time).
**Type:** decision
**Verified:** `n/a` for the curriculum stance. `ran-it` for the project it points at — six months of fractional CTO work, live on Cloud Run.
**Relevant to:** general (curriculum structure), 6 (Claude + Salesforce), 4 (integrating), 3 (administering)

## The decision

The curriculum carries **extended case studies alongside labs**. Not as a nicer lab — as a different teaching instrument.

A lab is scoped to an hour and proves one mechanism works. A case study shows **what six months of compounding decisions produces**, which is the thing a participant is actually trying to get to and cannot experience in a session.

The framing for participants: *this is what you'd ideally build for yourself as part of your go-to-market planning.* It's a target, not an exercise.

## What this one demonstrates that a lab structurally can't

**Architectural bets that only pay off over months.**
- Split the agent experience from the system of record. Agents use a portal; the portal writes to Salesforce; nobody learns Salesforce and nobody buys a seat to order a postcard.
- **Never write to the vendor's managed package.** *"A managed package is someone else's contract, and automating writes into it turns their upgrade into my outage."* Own your own objects.
- MCP as a **second front door onto one system** — the same domain logic as the web portal, not a parallel implementation. *"That's why it didn't double the maintenance burden."*

**Work that wasn't on the brief and turned out to be the job.** Rebuilding billing surfaced four live defects, each quietly mischarging for months — placeholder prices outranking real ones, postage priced but never charged, a flat-charge path ignoring quantity. *"Volume doesn't create defects like these. It makes them expensive."* No lab produces that discovery, because no lab runs long enough to have accumulated the mess.

**Restraint, itemised.** A whole section on what wasn't built: no Salesforce replacement, no chat interface for everything (*"a chat box that takes ninety seconds is a downgrade dressed as innovation"*), no automated writes into the managed package, no general document-processing platform, no per-agent AI budgets for a case that doesn't exist yet. Restraint is invisible in a lab, where the scope is given to you.

**The gate that makes AI-assisted speed safe.** 1,775 tests at a 92% branch-coverage floor, enforced every push alongside lint, secrets scan, static analysis, dependency audit, and a browser smoke test. *"Generated code is fast to produce and easy to over-trust. The gate is what converts speed into something you can deploy on a Friday."* That's card 020's harness argument at production scale.

**Judgment about release, not just build.** The finished billing rework is deliberately **held out of production** pending user-acceptance testing, with an audit queued to run in report mode first. *"I'd rather know the size of the problem than discover it while fixing it."*

**The asset nobody asks for at kickoff.** The compounding value isn't the flyer generator — it's *"six months and counting of structured operational data that didn't exist before and couldn't be reconstructed after the fact."*

## How to use it in the curriculum

- As the **closing exemplar** for a section, after labs have proven the mechanisms — here's those mechanisms at ten hours a week for six months.
- As a **source of carded principles**. Cards 048, 050, and 051 all came straight out of it — build-time economics, the managed-package boundary, and the two-books billing separation.
- As the **go-to-market prompt**: what is *your* equivalent? Which system of record, which population that won't adopt it, which deterministic transformation is currently being done by a person?

## Why this matters

Labs teach mechanism; case studies teach **judgment about scope**. Participants leaving with only mechanisms will build the impressive demo — the one where a model renders every flyer. What separates that from a system worth running is a set of decisions that only show up at length: what not to build, what to fix before volume arrives, what to hold back from production, and what to start capturing on day one.

It's also honest about pace. Ten hours a week for six months is the real shape of this work, and a curriculum made only of one-hour labs quietly implies otherwise.
