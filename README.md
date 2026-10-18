# Tally MCP

Your AI risk officer for AI-assisted trading. Tally keeps your private thesis, evidence, risk budget, exit criteria and decision history across conversations in Claude, ChatGPT and other compatible MCP clients.

## Connect

See [the setup guide](https://tally.markets/connect). Endpoint: `https://tally.markets/api/mcp`, Streamable HTTP with OAuth. Tool availability depends on the AI app, plan and workspace permissions.

Start with: “Get started with Tally, then run my checkup.”

Free includes 3 active theses (including drafts) and 10 total created. Closing frees an active slot but does not reset the total. Saved records remain readable, reviewable and closable. Sentinel is $4.99/month or $49/year with no creation cap and supported monitoring across the active journal.

## Tools

| Tool | Purpose |
| --- | --- |
| `get_execution_evidence` | Read archived orders, fills and decisions for an optional Propr account; no live protection claim |
| `get_started` | Current discipline protocol and tool boundaries |
| `get_checkup` | Active theses, evidence, deadlines, issues and history; optional connected Propr state is read-only |
| `list_theses`, `get_thesis` | Private journal records |
| `create_thesis` | Save a thesis and criteria; enforce the shared free allowance |
| `arm_thesis` | Change journal readiness after optional cooling-off; no trade authorization |
| `record_reading` | Add sourced evidence to the record |
| `fire_criterion` | Record an objectively triggered criterion |
| `close_thesis` | Close the journal record, with a reason; no brokerage execution |
| `get_receipt_link` | Retrieve an explicitly published receipt link |
| `declare_mandate`, `log_agent_activity` | Preserve optional mandate/activity records; never control trading |

The `morning-checkup`, `pre-register-trade` and `close-out` prompts support reviews. Scheduling is client-dependent, not a guarantee of unattended operation. Keep user confirmation controls in place.

## Execution boundary

No connector tool places, cancels, closes or modifies orders, changes protective stops, or resumes autonomous trading. Existing optional Propr controls remain in Tally Settings. Journal closure must never be represented as position closure. OpenAI execution capabilities remain disabled under current policy guidance.

Records are owner-scoped and private unless the user explicitly publishes a receipt. Read [privacy](https://tally.markets/privacy) and [terms](https://tally.markets/terms).

This repository contains public documentation and registry metadata only. Server implementation and secrets are not published here. Method: 2026-10-18.1. Server manifest: 1.7.0 (publication is a separate release step).

In `get_checkup`, optional `agent` and `monitoring` sections are null when no corresponding account is connected. Journal users do not need a brokerage or Propr connection.
