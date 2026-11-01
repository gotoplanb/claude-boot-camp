# The Discovery Questions That *Decide* the Design — Ask the Go/No-Go Ones Before Drawing Anything

**Source:** Assembled 2026-11-01 from the open questions scattered across cards 081–091. Beta spotted that they had no home; the go/no-go-versus-shapes split is the reason they needed one.
**Type:** pattern
**Verified:** `inferred` as a consolidated checklist — **this list has not been run against a customer.** The individual questions are real and sourced from the cards cited beside them. The underlying practice (establish constraints before designing) is ordinary consulting discipline, not a finding. *[confirm-this: run it once and record which questions turned out to change a design and which were merely interesting — the split below is predicted.]*
**Relevant to:** 6 (Claude + Salesforce), 3 (administering), 4 (integrating), general

## Why these need collecting

Scattered through an engagement's design are questions whose answers **remove options**. They aren't preferences to gather sympathetically. They're constraints, and asking them late means designing twice.

The distinction that matters:

> **Some answers are go/no-go — a design dies. Others merely shape.** Ask the first kind before drawing anything.

## Go/no-go — the design depends on the answer

| Question | What dies if the answer is no | Card |
|---|---|---|
| Are **repo-level plugins** permitted by enterprise settings? | The whole floor/ceiling structure. Fallback is markdown a session is pointed at — real, but weaker. | 090 |
| What are the **Enterprise settings for cloud sessions, environments, secrets and connectors**? | Unattended execution, and with it recipe graduation. | 082, 084 |
| Does the **network allowlist** reach the target systems from the execution environment? | Screenshot evidence, and possibly the deploy path itself. | 088 |
| Is there a **real tracker with atomic state transitions**, or a spreadsheet? | Safe claiming once agents are involved. | 083 |
| Can **environments be separated by role** — tracker access without org credentials? | The least-privilege split in the package. | 086 |

## Shapes the design, doesn't kill it

- **What monitoring do you use today? Who would actually look at run data? What can't you see now?** (089) — discovery questions rather than a recommendation, and the card's point is to resist leading with a product.
- **Does GitHub licensing include code and secret scanning on private repos?** Changes what the pipeline has to do itself.
- **Is a Jira CLI maintained and approved here?** Thin REST calls are the lower-risk fallback.
- **What is the real recipe edit rate?** (091) — the rate-of-change argument rests on it. Monthly rather than fortnightly and the org channel becomes viable.

## How to ask them

**Ask early, and ask them as constraints rather than requests.** *"Are repo-level plugins permitted?"* gets an answer. *"Can we have repo-level plugins?"* gets routed to someone's queue as a request for permission.

That's card 058's security-ask discipline pointed slightly differently: there you arrive with a demonstrated, specific ask; here you arrive *before* designing, to find out which asks are even worth preparing.

**And record the answers with dates.** Several of these are settings an administrator can change, and a "no" from six months ago isn't a no now. Card 056's warning applies — the convention outlives the constraint that justified it, and an engagement can carry a dead restriction for a year because nobody re-checked.

## Why this matters

The expensive failure isn't getting a no. It's getting the no **after** the architecture assumed a yes — which is how a week of design becomes a fallback nobody chose on purpose.

Collecting them also makes the first customer conversation concrete. It's a short list of answerable questions rather than a general request for access, and that's the same reason card 084's staged path works: specific and bounded beats general and open-ended.

See card 090 (the go/no-go that matters most here), card 082 (where the agent can run), card 089 (the monitoring questions, and the stance against selling a backend), card 058 (arriving with a specific ask), card 056 (re-check constraints you recorded), and card 084 (the staged path these questions unblock).
