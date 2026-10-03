# Claude Code's Native OTel Export → Local LGTM First, a Filtered Subset to Sumo Logic Only for Team Prompt Tuning

**Source:** Dave, Claude Boot Camp session 2026-10-03. Mechanics read from the Claude Code monitoring and mods docs the same day. The Alloy-as-router design was worked out in that session.
**Type:** pattern
**Verified:** `docs` for every product claim below (code.claude.com/docs/en/monitoring-usage and /plugins/mods/overview, read 2026-10-03). `ran-it` for the surrounding practice: Watchtower as a local Grafana feedback loop (card 022). `inferred` for the prompt-tuning loop itself. **Nothing in this card has been run end to end.** `[confirm-this: see the open items at the bottom.]`
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 3 (administering — governance), 1 (foundational concepts)

## The shape

Claude Code can export its own telemetry over OpenTelemetry. No plugin, no hook, no custom code. Point it at a collector and it emits metrics, events (through the logs pipeline), and, in beta, traces.

> **Send everything to a collector you own. Decide what leaves the machine at the collector, not at the client.**

For Dave's setup that means:

1. **Local stage (the default, the one that matters).** Claude Code → Grafana Alloy → the LGTM stack in Watchtower (Loki for events, Mimir or Prometheus for metrics, Tempo for traces). Turn every content gate on. It's all on his own box, and the content is the learning material.
2. **Team stage (optional, later).** A second Alloy pipeline that strips content-bearing attributes and forwards a subset to Sumo Logic. Its job is *collaboration* — several developers comparing prompt and eval results — not production monitoring. There is no production need to send to Sumo.

This is card 022's feedback loop, with Claude Code's own behavior as the thing being observed.

## Turning it on

Env vars, set on the machine where `claude` actually runs:

```bash
export CLAUDE_CODE_ENABLE_TELEMETRY=1
export OTEL_METRICS_EXPORTER=otlp
export OTEL_LOGS_EXPORTER=otlp
export OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf   # no default — you must set it
export OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318   # Alloy's OTLP/HTTP port
export OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE=cumulative
```

Traces additionally need `CLAUDE_CODE_ENHANCED_TELEMETRY_BETA=1` and `OTEL_TRACES_EXPORTER=otlp`.

Two things that bite:

- **Protocol has no default.** Set `OTEL_EXPORTER_OTLP_PROTOCOL` or the per-signal variant, or nothing exports.
- **Temporality defaults to delta.** Prometheus-style backends expect cumulative. Without the `cumulative` setting above, counters will look wrong in Mimir.

## What you get

- **Metrics:** session count, cost (USD), tokens by type (input, output, cache read, cache creation), lines changed, commits, PRs, active time, and edit-tool accept/reject decisions.
- **Events:** `user_prompt`, `assistant_response`, `api_request` (cost and tokens), `api_error`, `tool_result` (duration and success), `tool_decision` (accept/reject and who decided), `compaction`, `subagent_completed`, plus inventory events like `plugin_loaded` and `hook_registered`.
- **Correlation:** `prompt.id` ties every event from one prompt together. `tool_use_id` matches the ID in hook payloads, so OTel records and anything a hook captured can be joined.
- **Traces:** one root span per prompt, with LLM requests and tool calls as children. Hook spans appear only under detailed beta tracing, which needs extra setup (and, in interactive sessions, org allowlisting).

## Content is redacted unless you opt in

Prompt text, assistant responses, tool arguments, and tool output are all off by default. Each has its own gate: `OTEL_LOG_USER_PROMPTS`, `OTEL_LOG_ASSISTANT_RESPONSES`, `OTEL_LOG_TOOL_DETAILS`, `OTEL_LOG_TOOL_CONTENT`. `OTEL_LOG_RAW_API_BODIES` dumps full request and response bodies, including the whole conversation, and implies consent to all of the others.

Identity is **not** redacted. `user.email` and `user.id` ride on every metric and event.

Locally, turn the gates on. That is the point of the local stage. The consequence is that **the data in Loki is now sensitive** (prompts, shell commands, file contents). Which leads to:

## The Sumo branch is a redaction policy, not just a router

Alloy fans the same OTLP stream to two destinations. The Sumo branch goes through an attributes or filter processor first.

- **Drop:** `prompt`, `response`, `tool_input`, `tool_parameters`, `full_command`, `bash_command`, `body`, `body_ref`, and any `file_path`.
- **Keep:** metrics, plus selected events (`api_request`, `tool_decision`, `plugin_loaded`) and the identifying and experiment attributes.

Doing this at the collector puts the privacy decision in one reviewable config file. Doing it per developer, in their shell profile, puts it in as many places as you have developers. If Sumo is ever used, that one file is the thing to review.

I haven't verified that Sumo's hosted collector accepts OTLP/HTTP directly. See open items.

## Using it for prompt and skill tuning

This is the `inferred` part.

Tag each experimental run with `OTEL_RESOURCE_ATTRIBUTES`, for example `experiment=claude-md-v2,variant=b`. The values go onto every metric datapoint and event. They can't contain spaces, so use underscores or percent-encoding. Then compare variants in Grafana on things the telemetry actually measures:

- tokens and cost per `prompt.id`
- retries and `api_error` rate
- `tool_result` failure rate and tool-call counts per prompt
- `tool_decision` rejects (how often a human said no)
- compaction frequency
- time to completion

What this does **not** measure is whether the answer was *right*. Telemetry shows how a variant behaved, not whether it was correct. For that you still need an eval or a verifier (card 021's second kind of harness). The telemetry makes the cost and behavior side of tuning observable; it doesn't replace the correctness check.

It also covers **Claude Code sessions only**. Customer prompts running through the API in an application need their own OTel instrumentation. Claude Code's exporter says nothing about them.

## Native telemetry versus a hook, and versus a mod

This partially answers card 010's open question about audit logging as a hook use case.

- **Usage, cost, and audit data:** use native OTel. It already exists, it is richer than a hook payload, and (see below) managed settings can enforce it.
- **A hook is still right** when you need a custom payload, want to *block* or *rewrite* something, or want a webhook into your own service.
- **Mods** are the third option and don't solve this problem. They draw only in the terminal and the Desktop app's Code tab. Under Remote Control from the phone, the mod's hooks run but what it draws appears only in the terminal on the host machine.

## Governance if a team does adopt it

- Put the `OTEL_*` variables in the `env` block of managed settings. Claude Code then removes developer-set endpoints and credentials that conflict, so users can't redirect the export.
- A repo's own `.claude/settings.json` cannot turn telemetry on, pick a destination, or capture content. It can only turn a signal off, and not even that if managed settings set the selector.
- Dynamic auth headers (`otelHeadersHelper`) work with the HTTP protocols only, not gRPC.
- `OTEL_*` variables aren't passed to subprocesses (Bash tool, hooks, MCP servers), so an instrumented app you run through Claude Code won't inherit the exporter config.

## Gotchas

- **Don't put Grafana behind the ngrok tunnel without authentication once content gates are on.** Card 040's rule is that the tunnel carries UI, not keys. A Grafana full of prompts and commands is a lot more than UI. If it needs to be viewable from the phone, put real auth in front of it first.
- **Version-gated attributes.** Many of the attributes above need Claude Code v2.1.2xx or later, and mods need v2.1.287. Check `claude --version` before concluding something is missing.
- **The Prometheus exporter is metrics only.** It serves a scrape endpoint on the session. It's not a path to events or traces.

## Open items

`[confirm-this: wire Claude Code → Alloy → Loki/Mimir/Tempo end to end and record what actually lands, including whether the cumulative-temporality setting is enough for Mimir.]`

`[confirm-this: does Sumo Logic's hosted collector accept OTLP/HTTP directly? Not verified. If not, the Sumo branch needs a different exporter.]`

`[confirm-this: a Remote Control session runs on the host machine, so presumably it inherits that process's environment, but no doc states that the OTel variables apply there. Test it from the phone.]`

`[confirm-this: do the content-bearing trace attributes survive Tempo's attribute size limits at the 60 KB default content cap?]`

`[confirm-this: the tagging-by-experiment loop. Run two variants of one CLAUDE.md or skill through the same task and see whether the telemetry differences are legible or just noise.]`

## Related cards

- **022** — the feedback-loop principle this implements (tests, browser, traces).
- **010** — the open hooks thread; this card settles the audit-logging sub-question.
- **021** — telemetry observes behavior; verification checks ground truth. You need both.
- **039 / 042** — `user.email` on every record is the attribution-in-the-logs idea made concrete.
- **040** — the tunnel rule that limits where Grafana can safely be exposed.
