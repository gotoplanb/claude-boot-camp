# Connector-Backed Artifacts Execute as the *Viewer*, Not the Author — Two People Open the Same Dashboard and See Different Data

**Source:** Found while verifying artifact capabilities, Claude Boot Camp session 2026-09-11. Verified against [Share session output as artifacts](https://code.claude.com/docs/en/artifacts) → *Pull live data with MCP connectors*.
**Type:** gotcha
**Verified:** `docs` — read in full. **Not yet run.** `[confirm-this: publish a connector-backed artifact and have a second person open it; confirm the per-viewer permission prompt and the differing data.]`
**Relevant to:** 4 (integrating — MCP and APIs), 2 (operating Claude products), 3 (administering)

## The behaviour

A published artifact can call **MCP connectors at view time**, so the page shows current data rather than a snapshot from the session that built it. Ask for it in the prompt — *"pull the live list through my GitHub connector when the page loads"* — and Claude declares which connectors the page may call as part of publishing.

Here's the part that surprises people:

> **The call uses the account of the person viewing the page, not the person who published it.**

Consequences, all documented:

- **Two viewers can see different data** from the same dashboard, depending on what their accounts can reach.
- **Each viewer approves access first** — claude.ai prompts before the page's first connector call. Decline, or lack the connection, and you get the page with its live sections empty.
- **Actions run as the viewer too.** A page can offer controls that invoke connector tools *with side effects* — posting a message, updating an issue — and the side effect is attributed to whoever clicks.
- The page never sees anyone's credentials; claude.ai makes the calls on its behalf.

## Why this is a gotcha rather than a footnote

The natural mental model for "I built a dashboard and shared it" is that the dashboard shows *what I see*. It doesn't. It's closer to shipping a **query template** than a report — the recipient runs it against their own access.

That's arguably the right security design (no credential leakage, no privilege escalation through a shared page), but it inverts the expectation in ways that matter:

- A dashboard that looked complete when you built it can be **empty for the person you sent it to**, and the page won't say why unless you asked for a fallback message naming the connector. The docs recommend exactly that — do it.
- "Everyone on the team sees the same view" is **false by default**, which undermines a status board's whole purpose if access differs across the team.
- A control with side effects is a button that acts *as the clicker*. Worth thinking about before shipping one.

## Related limits

- A connector-backed artifact **can't be shared to a public link on any plan.** On Pro/Max — where a public link is the only sharing option (card 028) — that means it stays private to you, full stop.
- Only connectors from your claude.ai account qualify. **Local MCP servers from `.mcp.json` can supply data while Claude builds the page, but the published page can't call them.**
- Orgs can disable this independently of artifacts, via an **Enable artifact connectors** toggle.

## Why this matters

It's a clean example of a feature whose *security* model and whose *expected* model point in opposite directions — and the gap only shows up when someone else opens the page, which is after you've already shared it.

For anyone standing up a Claude practice, the second bullet is the operational one: **a locally-configured MCP server won't power a published page.** If the plan is "build an internal dashboard on top of our MCP server and share it," that requires the connector to exist on claude.ai accounts, not in a developer's `.mcp.json`. Worth knowing before designing around it.
