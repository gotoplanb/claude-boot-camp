# Put the Agent Next to the Thing It Manipulates — Three Variables Decide Local vs. Cloud

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** concept
**Verified:** `ran-it` for the local side — it's how Dave works today (cards 009, 040). `inferred` for the cloud side: the reasoning is sound and the variables are the right ones, but he has not run an unattended cloud agent against a client system. *[confirm-this: the customer's Enterprise settings for cloud sessions, environments, secrets and connectors — what's actually available there decides whether half this card is usable.]*
**Relevant to:** 5 (building with Claude Code), 3 (administering — governance), 4 (integrating), general

## The rule

> **Put the agent next to the thing it manipulates.**

Local versus cloud isn't a matter of taste. Three questions decide it:

1. **Is a human present, and do they need to be?**
2. **Whose credentials does the agent hold?**
3. **How heavy and privileged is the environment?**

## Which way each points

**Local wins when** a human is present, the environment is heavy, and the managed device already carries the organization's controls — VPN, device management, the existing security posture. You're borrowing controls that already exist.

**Cloud wins when:**

- **The work is unattended** — scheduled jobs, CI triage, drift checks (card 085). Nobody is sitting there, so "a human is present" stops being an argument.
- **One person wants to fan out** several sessions in parallel (card 083).
- **Isolation matters.** This is the one people get backwards: a managed laptop has good security controls, but *an autonomous agent running on it inherits everything that user can reach.* Device management protects the device, not the blast radius of an agent holding the user's ambient permissions — which is card 039's whole argument. A cloud sandbox can be given a least-privilege credential instead.
- **Environments need to be reproducible** and handed between people.

## The contrast that makes it concrete

**A local Python app runs best locally** — the app lives there. The agent is next to the thing.

**Salesforce work lives in the platform and its sandboxes.** The local machine contributes almost nothing beyond a CLI; the thing being manipulated is somewhere else entirely. So the gravitational argument for local is much weaker, and the isolation argument for cloud is stronger.

## Why this matters

It replaces a preference ("I like working locally") with a test you can apply to a specific task, and it surfaces the counterintuitive case: **the managed laptop is the less isolated option once the agent is autonomous.** Good device security and a bounded blast radius are different properties, and only one of them is improved by a locked-down laptop.

See card 039 (ambient permissions vs. scoped tokens — the mechanism under the isolation point), card 040 (keep compute and credentials private, expose only the control channel), card 009 (default to local tooling, and why), card 083 (fan-out), and card 084 (how a task earns its way from local to cloud).
