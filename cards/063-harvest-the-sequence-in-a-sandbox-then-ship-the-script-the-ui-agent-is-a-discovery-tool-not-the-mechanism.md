# Harvest the Sequence in a Sandbox, Then Ship the Script — the UI Agent Is a Discovery Tool, Not the Mechanism

**Source:** Dave, reacting to a Dreamforce 2026 keynote demo of supplier onboarding, 2026-09-16. The demo's friction point was setting a supplier up in SAP — many screens of clicking, weeks to train a new hire, boring and error-prone. The discovery-vs-execution framing and the incident-response reading are Beta's, same session.
**Type:** pattern
**Verified:** `ran-it` — *"that's how I build things today."* The demo description is Dave's live account of a keynote, not independently covered.
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 3 (administering — governance), 6 (Claude + Salesforce), general

## The method

1. **Work in a sandbox.** Never production. (The demo recommended this too, and it's the right call — card 026.)
2. **Let the UI-driving agent discover the sequence.** Claude in Chrome logs in, clicks through, and you watch what the process actually requires.
3. **Use the browser for *visual verification only*.** Dave's actual use: confirming that API requests succeeded, and that black-box backend workflows he can't see from the API fired correctly. The UI is the oracle, not the actuator.
4. **Write the real thing as a sequence of API calls — or better, CLI calls.** CLI is the preferred target because the whole thing then executes as one command.
5. **Narrow the credential, iteratively** (below).
6. **Codify the sequence as a script.** Optionally deliver it behind a custom MCP.

> *"I'm not going to be in prod driving clicks in a UI to do some process. I'm only going to harvest it in a sandbox and then turn it into a script."*

## Why this isn't just "UI-driving bad, API good"

The sharper claim is about **what the agent is for**:

**Spend an expensive, flexible, nondeterministic tool once to learn something. Encode what you learned into a cheap, rigid, deterministic tool you run forever after.**

The agent is a *discovery* instrument — it reverse-engineers an unknown sequence. Then you throw the agent away and keep only the derived artifact.

That makes it a distinct thing from the verification harnesses in cards 020 and 021, and the payoffs differ:

| | Verification harness | Discovery harvest |
|---|---|---|
| What you get | Confidence that a build did what it should | A permanent artifact |
| Agent afterwards | Still needed, every run | Not needed at all |
| Runs | Forever | Once |

Worth naming separately for that reason: a verification harness is infrastructure you keep paying for; a harvest is a one-time cost that leaves a deterministic script behind.

## The scope-narrowing loop

The credential work happens *during* harvesting, not after:

- First pass runs **as you**, on your local machine, against the sandbox — maximum access, because you're still learning what's required.
- Then mint a profile or access token with **only the scopes you think you need**, and run the same thing again.
- Keep cutting until you're at the **minimum scope that still completes the task**.

Dave's aside is worth keeping, because the vocabulary has gone soft:

> *"'Hardening' gets overused. The point is to reduce attack surface."*

The loop works precisely because the task is fixed and repeatable. You can't iteratively minimize scope against an interactive human session — there's no stable definition of "what it needed." A script gives you one.

## The security dividend — an *enumerable* incident-response surface

Card 039 already argues that scoped tokens bound the blast radius. The harvest adds something that argument can't get on its own: **the list of operations is knowable and finite, because the script enumerates it.**

If that credential ever leaks, *"what could it have done"* is a short, known list — not *"whatever that person's SSO session could reach."* That's a difference at incident time, not just at design time: a bounded, legible surface instead of an opaque one. Same instinct as dual-writing telemetry you control, applied to credentials instead of traces.

You only get this because the script exists. Scope minimization without a codified task gives you a smaller surface you still can't enumerate.

## Why this matters

It's the answer to the whole category of "AI clicks through your legacy system for you" demos. Those demos sizzle because the clicking is the visible pain — but automating the clicking keeps the nondeterminism, keeps the agent in the loop forever, and keeps a broad interactive credential live in production.

Harvesting inverts all three. The pain was never the clicking; it was that nobody had written down the sequence.

See card 026 (sandbox first), cards 020/021 (the harness kinds this sits beside), card 022 (observe reality rather than accepting the report — why the browser is the oracle), card 039 (what bounds the blast radius), card 064 (**the trap: the harvested sequence may not be equivalent**), and card 060 (when shipping it behind a custom MCP is the right call).
