# Tally — the risk officer for AI trading

> Every trading connector makes your AI more capable. **Tally makes it
> accountable.**

Tally ([tally.markets](https://tally.markets)) is an independent,
read-only watcher for the AI agent that trades a brokerage account. It
reads the account through SnapTrade, learns how the agent normally
trades, and emails the owner on the day the agent breaks its own
pattern. Most days it sends nothing.

This connector is how an agent running inside Claude, ChatGPT, Claude
Code, or any MCP-capable client takes part in that. Two things it does:

- **The agent reports what it researched, considered, and proposed**
  (`log_agent_activity`): symbols and kinds only, no text accepted or
  stored. Tally judges the account by its actual orders; the reports
  sharpen an alert (an order in a symbol the agent never mentioned) and
  never replace one.
- **The thesis journal**, for trades the user makes themselves: a
  pre-registered thesis per trade, exit rules that freeze before capital
  moves, a 24-hour cooling-off, an override ledger for rule-breaking
  exits, and a public receipt for every closed trade.

**Tally never executes trades.** Every broker and venue connection is
read-only at the API level — no order placement, no custody, by
architecture and by policy. Nothing in this connector can reach a
brokerage.

## Connect

**Server URL (Streamable HTTP · OAuth 2.0 with dynamic client
registration):**

```
https://tally.markets/api/mcp
```

| Client | How |
|---|---|
| **Claude** | Settings → Connectors → Add custom connector → name it `Tally`, paste the URL. Leave the OAuth fields empty — Tally registers itself. |
| **ChatGPT** | Listed app — [one click from the directory](https://chatgpt.com/plugins/plugin_asdk_app_6a7b8e021b148191aa85f69711529637), no developer mode. (Manual fallback: developer mode → Plugins → New plugin, the URL as Server URL, OAuth.) |
| **Claude Code** | `claude mcp add --transport http tally https://tally.markets/api/mcp`, then `/mcp` to sign in. |
| **Any MCP agent** | Point it at the URL above; OAuth discovery does the rest. |

Then say: **"Get started with Tally, then run my checkup."** The server
teaches your AI the whole protocol on first contact. The connector
needs a subscribed Tally account (Sentinel, $4.99 a month or $49 a
year); every tool but `get_started` answers only for one.

Full walkthrough (skill, scheduled morning checkup, broker linking):
[tally.markets/connect](https://tally.markets/connect)

## Tools

| Tool | What it does |
|---|---|
| `get_started` | Returns the protocol — the AI teaches itself |
| `log_agent_activity` | Report what the agent researched, considered, or proposed — symbols and kinds only, no text |
| `get_checkup` | Morning briefing: active theses, deadlines, evidence, closed history and lifetime R |
| `list_theses` / `get_thesis` | Read the journal |
| `create_thesis` | Pre-register a trade: statement, mechanism, risk budget, exit criteria (starts the 24h cooling-off) |
| `arm_thesis` | Freeze the criteria after cooling-off — from here they fire, never edit |
| `record_reading` | Log evidence against a criterion (the generic sensor primitive) |
| `fire_criterion` | Mark a rule objectively triggered, with cited evidence — one-way |
| `close_thesis` | Close a trade; discretionary closes require a written justification, logged forever |
| `get_receipt_link` | The public receipt for a closed thesis |

Reads are safe to always-allow; writes are guarded by the protocol
itself (cooling-off, criteria minimums, override justifications).

## Prompts

Clients with a prompt picker get the three jobs Tally exists for, ready
to run:

| Prompt | What it does |
|---|---|
| `morning-checkup` | The daily run — what needs action, fresh readings against every active thesis's criteria, anything that fired. This is the one to put on a schedule. |
| `pre-register-trade` | Turns an idea into a pre-registered thesis, interviewing you until it is falsifiable |
| `close-out` | Walks a thesis to its exit — what the written rules demand, and the justification the override ledger requires if you are closing early |

## Proof

A real receipt — pre-registered thesis, venue-verified entry and exit,
−0.22R, graded Process B / Outcome D, closing note verbatim:
[*"Wrong on thesis, right on
process."*](https://tally.markets/receipt/b3bb0a14-3467-4d55-9c9d-3763f31c6b9f)

## How it compares

Broker notifications confirm each fill. The agent's own reports grade
the agent. Portfolio trackers show the balance. Tally is the one built
to judge the account against its own normal, independently of all
three: [tally.markets/compare](https://tally.markets/compare)

## Privacy & security

- OAuth per user; your AI sees your own record only, under a grant you
  can revoke any time.
- What the agent reports is stored as a symbol, a kind, and a time, for
  90 days. No text field exists.
- Brokerage data (via SnapTrade) is read-only and is read by Tally's own
  service, never through this connector; broker credentials never touch
  Tally.
- Details: [privacy](https://tally.markets/privacy) ·
  [terms](https://tally.markets/terms) · [support](https://tally.markets/support)

---

*Tally is a hosted service. This repository is its documentation and
public manifest, not its source — there is nothing here to install or
run. Point your AI at the connector URL above; OAuth does the rest.*
