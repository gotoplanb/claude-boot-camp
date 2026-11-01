# Screenshots Are Evidence for a Human, Not Proof — the Deterministic Check Is the Proof

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** pattern
**Verified:** `ran-it` for browser screenshot capture as a verification channel (card 022, and this session's own use of it). `inferred` for the reviewer flow end to end, and for the pre-authenticated URL approach specifically — **not yet run against a client sandbox.** *[confirm-this: whether the cloud environment's network allowlist actually reaches the target org. Check before designing around it.]*
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), 3 (administering)

## The split

**Primary verification is deterministic**: metadata checks, tests, queries. That's what decides whether the work is correct.

**Screenshots are for the human reviewer.** They're evidence, in the courtroom sense — they help a person form a judgment. They are not proof, and nothing should pass on their strength alone.

Collapsing those two is the mistake. A screenshot showing the right thing is compatible with the underlying change being wrong, missing, or applied to the wrong record.

## The flow

1. Agent builds and verifies deterministically.
2. Agent captures screenshots.
3. Agent replies with the screenshots plus a link to the sandbox.
4. **Reviewer clicks through with their own login** for final acceptance.

Step 4 is card 081's back bookend, and the reviewer using *their own* login matters — it's the same property card 029 describes for connector-backed artifacts. What the reviewer sees is what their access shows, not what the agent's did.

## Cautions

- **Confirm the network allowlist reaches the target systems.** A cloud sandbox that can't see the org produces a confident empty result, not an error.
- **Prefer a CLI-generated pre-authenticated URL over driving a login page.** Login flows are brittle and involve handling credentials you'd rather not handle. **Treat that URL as a credential** — keep it out of logs and out of the screenshots themselves.
- **UI selectors are brittle.** This is the mechanical reason a screenshot can't be the only proof: the capture can fail, succeed on the wrong element, or silently shift when the page changes.
- **Screenshots may contain real data.** Check what's being attached and who can see it before it's attached to a ticket forever.

## Why it matters

For reviewers who aren't developers — and admins acting as reviewers usually aren't — **a before-and-after screenshot communicates more than a metadata diff does.** That's not a failure on their part; it's the correct instrument for the question they're being asked, which is "is this what I wanted?" rather than "is this what was specified?"

So the screenshot isn't decoration for a non-technical audience. It's the part of the handoff that makes acceptance possible at all. It just isn't the part that establishes correctness.

See card 022 (observe reality — and which browser tool does it), card 081 (the acceptance bookend this serves), card 020 (the deterministic verifier that is the actual proof), card 029 (whose access governs what a viewer sees), and card 063 (the browser as oracle rather than actuator).
