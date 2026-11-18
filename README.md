# Tally MCP

Your AI risk officer for AI-assisted trading. Check a trade's size and what you could lose against your chosen limits. Private drafts, evidence and decision history are available when useful; no journal is required to check a trade.

## Connect

See [the setup guide](https://tally.markets/connect). Endpoint: `https://tally.markets/api/mcp`, Streamable HTTP with OAuth. Tool availability depends on the AI app, plan and workspace permissions.

Start with: “Check this trade for me. Use the details we already have.” Read the server's tool list to confirm the deployed version supports `check_trade`.

Free includes 3 active trade plans (including drafts) and 10 total created. Closing frees an active slot but does not reset the total. Saved records remain readable, reviewable and closable. Sentinel starts at the founding price of $9/month or $49/year with no plan-creation cap and up to 3 active evidence watches. Connected supports up to 10. Automatic monitoring requires active paid-tier access.

Connected is an optional $89/month tier adding read-only broker snapshots after
payment, provider activation and explicit owner consent. Every plan includes the
AI connection. If a paid customer's broker service is awaiting initial setup,
`get_checkup.brokerage.status` is `activation_pending`; explain that state and
continue the journal workflow. A Tally check time is separate from provider data
freshness. Transaction history can be delayed; this is not a live trading feed.

## Tools

| Tool | Purpose |
| --- | --- |
| `check_trade` | Check fees, whole-unit size, additions, giveback and known expiry/event conflicts against an explicit loss limit; no orders or journal prerequisite |
| `get_execution_evidence` | Read archived orders, fills and decisions for an optional Propr account; no live protection claim |
| `get_started` | Current method, first-use guidance and tool boundaries |
| `try_tally` | Conversational fictional example: compare two and three contracts; no brokerage reads or trading records |
| `view_trade_check` | Optional branded visual card, only when the user requests an interactive or visual view |
| `get_checkup` | Active theses, evidence, deadlines, issues and history; optional read-only Connected broker snapshots or Propr state |
| `list_theses`, `get_thesis` | Private journal records |
| `save_thesis_draft` | Save or revise a private conversational draft; no monitoring or trading |
| `activate_idea_watch` | Activate a confirmed, one-time hourly Coinbase USD price watch for BTC, ETH, SOL or XRP |
| `list_idea_watches` | Review your conditions, timestamps, errors and triggered state |
| `stop_idea_watch` | Stop a watch while preserving its evidence |
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

This repository contains public documentation and registry metadata only. Server 1.12.0 uses method 2026-11-18.4. Hosted tool availability can lag behind deployment while an AI app scans and approves updates; a registry version does not establish marketplace approval.

`check_trade` accepts supported fully paid USD long calls, puts and whole shares, excluding same-day expiry. Use known conversation facts and authorized sources, never invented limits or market observations. Data can come from Connected via SnapTrade, a broker connector, both or neither. Preserve source identity and timestamp; do not sum duplicate positions or combine cash and holdings from different accounts. The server does not independently authenticate data relayed by the host from another connector. Connected snapshots are context, not verified current funding. Supporting MCP Apps hosts can render the optional result view; complete text output works without it. Validate actual host rendering before release.

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

## Ideas and supported watches

Save an incomplete idea with `save_thesis_draft` after confirming its summary. Reuse its ID for revisions. Every watch is attached to an owned trade plan; a completed formal thesis is optional. `activate_idea_watch` requires an explicit USD threshold, why it matters, an expiry within 30 days, a retry-stable request UUID and user confirmation. These legacy price watches require active paid-tier access and support up to ten active watches per owner.

Checks run hourly against Coinbase Exchange last-trade observations. They can miss moves between checks and are not stop orders, continuous monitoring or daily-close rules. Conditions include equality; the first fresh check can already meet the condition. Each watch triggers once. Missing or stale data never counts as an unchanged market. Inspect `list_idea_watches` or https://tally.markets/workspace for source timestamps and errors. Emails follow existing notification preferences. Free-text macroeconomic criteria are not automatically monitored by this feature.

No private conversations are copied automatically. No watch authorizes trading.


## Conversation-led evidence watches

The main flow is **discuss the trade → examine the case and risk → preview a supported watch → confirm once → review meaningful changes**. A thesis is the why inside a trade plan. No formal thesis questionnaire is required; every watch belongs to a saved trade plan. `get_started` supplies the current conversation guidance.

- `save_trade_plan`, `get_trade_plan`, `close_trade_plan`: map the conversation into a private game plan; preserve revisions and record user-reported exits without trading.
- `preview_evidence_watch`: read-only scope, paid entitlement, source readiness and confirmation hash.
- `activate_evidence_watch`: confirmed trade plan and attached watch in one idempotent transaction, or a watch for an existing owned plan.
- `list_evidence_watches`: dated observations, coverage and recent private events, optionally by watch ID.
- `stop_evidence_watch`: stop checks and suppress unsent notifications; retain history.

Initial scope: Circle/USDC company evidence, with optional qualified end-of-session CRCL call quotes when separately configured. Sentinel and higher paid tiers only. These tools do not execute trades. A broker MCP session does not grant background access. See the [hosted guidance](https://tally.markets/tally-skill.md) and use the read-only preview to check current deployment readiness. Optional contract-price coverage is separately gated; do not promise it from a broker connector in the chat.
