# The Discovery Questions That *Decide* the Design — Ask the Go/No-Go Ones Before Drawing Anything

**Source:** Assembled 2026-11-01 from the open questions scattered across cards 081–091. Beta spotted that they had no home; the go/no-go-versus-shapes split is the reason they needed one.
**Type:** pattern
**Verified:** `inferred` as a consolidated checklist — **this list has not been run against a customer.** The individual questions are real and sourced from the cards cited beside them. The underlying practice (establish constraints before designing) is ordinary consulting discipline, not a finding. *[confirm-this: run it once and record which questions turned out to change a design and which were merely interesting — the split below is predicted.]*
**Relevant to:** 6 (Claude + Salesforce), 3 (administering), 4 (integrating), general

> **Relationship to card 093.** This card is the *lens* — which answers are fatal and which merely shape. Card 093 is the *instrument*: the full kickoff interview, grouped by what each answer blocks, written to be run as a session.

## Why these need collecting

Scattered through an engagement's design are questions whose answers **remove options**. They aren't preferences to gather sympathetically. They're constraints, and asking them late means designing twice.

The distinction that matters:

> **Some answers are go/no-go — a design dies. Others merely shape.** Ask the first kind before drawing anything.

## Correction — two of these gate phase two, not day one

Card 093 sorts the same questions by *when* they bite, and that exposed an error here. The first two rows below were originally filed as fatal to the whole design. They aren't:

> **While the only users are developers pulling from GitHub, enterprise cloud-session settings and repo-level plugin permissions block nothing.** Developers control their own configuration and get the skills by checking out the repo. Those settings gate **phase two** — admins working in cloud sessions on the customer's tenant.

That matters because a "no" on either was being treated as a reason not to start. It isn't. It's a reason phase two needs a different answer.

## Go/no-go — the design depends on the answer

| Question | What dies if the answer is no | When it bites | Card |
|---|---|---|---|
| Are **repo-level plugins** permitted by enterprise settings? | Automatic skill loading for customer admins. Fallback is markdown a session is pointed at — real, but weaker. | **Phase two** | 090 |
| What are the **Enterprise settings for cloud sessions, environments, secrets and connectors**? | Unattended execution, and with it recipe graduation. | **Phase two** | 082, 084 |
| Does the **network allowlist** reach the target systems from the execution environment? | Screenshot evidence, and possibly the deploy path itself. | Day one | 088 |
| Is there a **real tracker with atomic state transitions**, or a spreadsheet? | Safe claiming once agents are involved. | Day one | 083 |
| Can **environments be separated by role** — tracker access without org credentials? | The least-privilege split in the package. | Day one | 086 |

## Shapes the design, doesn't kill it

Monitoring and run-data readership (089), GitHub scanning licensing, the Jira CLI question, and the real recipe edit rate (091). Card 093's Groups 3 and 4 carry these in full, with the rest of the interview.

## How to ask them

**Ask early, and ask them as constraints rather than requests.** *"Are repo-level plugins permitted?"* gets an answer. *"Can we have repo-level plugins?"* gets routed to someone's queue as a request for permission.

That's card 058's security-ask discipline pointed slightly differently: there you arrive with a demonstrated, specific ask; here you arrive *before* designing, to find out which asks are even worth preparing.

**And record the answers with dates.** Several of these are settings an administrator can change, and a "no" from six months ago isn't a no now. Card 056's warning applies — the convention outlives the constraint that justified it, and an engagement can carry a dead restriction for a year because nobody re-checked.

## Why this matters

The expensive failure isn't getting a no. It's getting the no **after** the architecture assumed a yes — which is how a week of design becomes a fallback nobody chose on purpose.

Collecting them also makes the first customer conversation concrete. It's a short list of answerable questions rather than a general request for access, and that's the same reason card 084's staged path works: specific and bounded beats general and open-ended.

See card 093 (the full checklist, and the phase correction above),
card 090 (the precondition this sharpens), card 082 (where the agent can run), card 089 (the monitoring questions, and the stance against selling a backend), card 058 (arriving with a specific ask), card 056 (re-check constraints you recorded), and card 084 (the staged path these questions unblock).
