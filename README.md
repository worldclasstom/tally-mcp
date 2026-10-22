# Tally MCP

Your AI risk officer for AI-assisted trading. Tally keeps your private thesis, evidence, risk budget, exit criteria and decision history across conversations in Claude, ChatGPT and other compatible MCP clients.

## Connect

See [the setup guide](https://tally.markets/connect). Endpoint: `https://tally.markets/api/mcp`, Streamable HTTP with OAuth. Tool availability depends on the AI app, plan and workspace permissions.

Start with: “Get started with Tally, then run my checkup.”

Free includes 3 active theses (including drafts) and 10 total created. Closing frees an active slot but does not reset the total. Saved records remain readable, reviewable and closable. Sentinel is $9/month or $49/year with no creation cap and supported monitoring across the active journal.

Connected is an optional $89/month tier adding read-only broker snapshots after
payment, provider activation and explicit owner consent. Every plan includes the
AI connection. If a paid customer's broker service is awaiting initial setup,
`get_checkup.brokerage.status` is `activation_pending`; explain that state and
continue the journal workflow. A Tally check time is separate from provider data
freshness. Transaction history can be delayed; this is not a live trading feed.

## Tools

| Tool | Purpose |
| --- | --- |
| `get_execution_evidence` | Read archived orders, fills and decisions for an optional Propr account; no live protection claim |
| `get_started` | Current discipline protocol and tool boundaries |
| `get_checkup` | Active theses, evidence, deadlines, issues and history; optional read-only Connected broker snapshots or Propr state |
| `list_theses`, `get_thesis` | Private journal records |
| `create_thesis` | Save a thesis and criteria; enforce the shared free allowance |
| `arm_thesis` | Change journal readiness after optional cooling-off; no trade authorization |
| `record_reading` | Add sourced evidence to the record |
| `record_decision_event` | Save a user-approved step, decision, personal boundary, rule deviation or linked correction |
| `get_decision_history` | Read session entries and watcher records chronologically, with source labels and pagination |
| `submit_product_feedback` | Send the user’s chosen product feedback to the Tally team |
| `fire_criterion` | Record an objectively triggered criterion |
| `close_thesis` | Close the journal record, with a reason; no brokerage execution |
| `get_receipt_link` | Retrieve an explicitly published receipt link |
| `declare_mandate`, `log_agent_activity` | Preserve optional mandate/activity records; never control trading |

The `morning-checkup`, `pre-register-trade` and `close-out` prompts support reviews. Scheduling is client-dependent, not a guarantee of unattended operation. Keep user confirmation controls in place.

## Execution boundary

No connector tool places, cancels, closes or modifies orders, changes protective stops, or resumes autonomous trading. Existing optional Propr controls remain in Tally Settings. Journal closure must never be represented as position closure. OpenAI execution capabilities remain disabled under current policy guidance.

Records are owner-scoped and private unless the user explicitly publishes a receipt. Read [privacy](https://tally.markets/privacy) and [terms](https://tally.markets/terms).

This repository contains public documentation and registry metadata only. Server implementation and secrets are not published here. Method: 2026-10-21.1. Server manifest: 1.8.0 (publication is a separate release step).

In `get_checkup`, `brokerage` is null without an eligible Connected subscription;
it can describe activation pending before a broker is connected. The retired
`monitoring` section is null. The optional `agent` section is null without a
Propr account. Journal users do not need either connection.

Transport safeguards: requests are limited to 128 KiB before JSON parsing, and
tool arguments to 5,000 combined object keys/array entries. Split larger evidence
updates into smaller calls. Each request gets its own MCP server instance.

## Decision history

With the user’s agreement, save a concise sequence of what happened, what they decided, why, and which rule they followed or crossed. A `boundary` entry states the user’s line in the sand and their intended response. Corrections append and link to an earlier event. This works on every plan and does not consume a thesis slot.

Use `get_decision_history` alongside checkup data for process reviews. Records are oldest first; follow `next_cursor` until null for a complete period. Keep filters unchanged between pages. Date-range queries combine user reports and watcher alerts/readings; a session filter returns reported entries only because watcher records have no inferred session assignment. `occurred_at` and `recorded_at` distinguish an event from a retrospective report.

Tally does not receive the full Claude or ChatGPT transcript. Save only relevant summaries the user approved. Do not infer mental health, label a trading style from a small sample, or mistake a reported decision for a verified fill. Compare process with the rule that existed at the time, separately from P&L. Review possible tipping points with the user; a boundary entry does not control execution.

Product feedback is separate: show the exact feedback wording, get the user’s agreement to send it to Tally’s team, then call `submit_product_feedback`. Never include private journal content by default. Signed-in users can also use Feedback in their app footer.
