# A Swapped Connector Silently Repoints Every Scheduled Task — Account Drift Fails With Nobody Watching

**Source:** Beta, Claude Boot Camp session 2026-09-17, raising the consequence of card 065's one-account limit once card 066's recurring task is an *unattended* one.
**Type:** gotcha
**Verified:** `inferred` — the failure follows from card 065's `ran-it` swap behaviour plus unattended execution, but **nobody has hit it**. *[confirm-this: does a scheduled task in Claude for Mac resolve the connector at fire time (drift bites) or bind the account when the schedule is created (drift doesn't)? This single question decides whether the card is a live risk or a non-issue — test before relying on either answer.]*
**Relevant to:** 2 (operating Claude products), 4 (integrating), 3 (administering), 5 (building with Claude Code)

## The failure

The Google connector holds one account, and signing in with another **replaces** it (card 065). That's a minor annoyance in live chat. Under a schedule it becomes a silent fault:

1. Tuesday: you swap the connector to a personal Gmail to check something.
2. You forget to swap it back.
3. Wednesday morning: a scheduled task that expects the client Drive account fires.

It either fails, or — **worse — quietly succeeds against the wrong account.**

## Why unattended is a different risk class

In live chat you'd catch this instantly. The wrong inbox is right there on screen, and you're present at the moment of the mistake.

A scheduled task inverts every one of those properties:

| | Live chat | Scheduled task |
|---|---|---|
| Who's watching | You, at the moment it runs | Nobody |
| Wrong account visible | Immediately | Only if something later surfaces it |
| Time to detection | Seconds | Until someone notices the output is wrong |
| Failure mode you'd prefer | — | A loud error, which is **not** the one you might get |

The dangerous outcome isn't the failure. It's the success — a weekly update written to the wrong Drive, or a summary generated from the wrong inbox, with no error anywhere.

**The state a scheduled task depends on is mutable by an unrelated action.** Swapping a connector to answer a one-off question on Tuesday is not obviously an edit to Wednesday's automation, and nothing in the moment tells you it is.

## What to do about it

- **Establish the binding semantics first.** Everything below depends on the `confirm-this` above. If a schedule pins the account at creation time, this whole card is moot; if it resolves live, every scheduled task shares one mutable global.
- **Treat the connected account as dedicated** once anything is scheduled against it. Swap for ad-hoc work on a different surface, or accept that the swap is a change to your automation.
- **Make the task assert its own context** — have it check that the account or a known file/folder is the expected one and fail loudly otherwise. This is card 020's external verifier, narrowed to one precondition: *verify the input, not just the output.*
- **Prefer a loud failure to a quiet success.** If the task can't confirm which account it's on, not running is the better outcome.

## Why this matters

It's a specific instance of a general trap worth naming: **automation inherits whatever ambient state it runs in, and ambient state is edited by people who aren't thinking about the automation.** That person is usually you, a day earlier, doing something unrelated.

It also sharpens card 066's advice. "Scheduling answers the connector question for you" is true, and it comes with a condition — a scheduled task doesn't merely *justify* connecting an account, it takes a dependency on that account staying connected.

See card 065 (the one-account limit this is downstream of), card 066 (when to connect at all), card 020 (unattended work needs a verifier standing in for you), and card 014 (the model trusts its own snapshot — close the gap structurally rather than by remembering).
