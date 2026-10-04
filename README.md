# Tally — anomaly detection for AI trading agents

> Every trading connector makes your AI more capable. **Tally makes it
> accountable.**

Tally ([tally.markets](https://tally.markets)) is anomaly detection for
the AI agent that trades a brokerage account. It reads the account
read-only through SnapTrade, builds the agent's own baseline (symbols,
size, frequency, hours, cancels), scans the account against it every
hour, and emails the owner the hour the agent deviates. Prompt
injection, a poisoned page, a bad update: a compromised agent shows up
first as an order it would never have placed yesterday, and that is
what Tally flags. Most days it sends nothing.

This connector is how an AI running inside Claude, ChatGPT, Claude Code,
or any MCP-capable client takes part in that. Four things it does:

- **The AI reads the monitoring** (`get_checkup`): the status word per
  account, orders in the last 24 hours, anomaly alerts of the last 7
  days and whether the owner answered them, the broker connection. The
  morning checkup leads with it.
- **The agent declares its mandate** (`declare_mandate`): what it
  trades, the largest order as a share of the account, cadence, hours,
  usual symbols, as structure. Tally runs it from the first scan and
  never loosens it; a later change notifies the owner.
- **The agent reports what it researched, considered, and proposed**
  (`log_agent_activity`): symbols and kinds only, no text accepted or
  stored. Tally judges the account by its actual orders; the reports
  sharpen an alert (an order in a symbol the agent never mentioned) and
  never replace one.
- **The record**, built from the broker's orders without anyone typing:
  every completed round trip with its realised P&L, and the numbers
  (win rate, expectancy, profit factor, by symbol, setup, weekday). The
  agent's `setup` tag on a proposal lands on the trip.
- **Pre-registered theses**, optional, for trades the user makes
  themselves: a statement, exit rules that freeze at creation, an
  optional 24-hour cooling-off, an override ledger for rule-breaking
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
| `declare_mandate` | Declare what the agent is supposed to do — asset classes, largest order, cadence, hours, symbols; a change notifies the owner |
| `log_agent_activity` | Report what the agent researched, considered, or proposed — symbols, kinds, and a setup tag; no text |
| `get_checkup` | Morning briefing: the monitoring status per account and any anomaly alert awaiting an answer, then active theses, deadlines, evidence, closed history and lifetime R |
| `list_theses` / `get_thesis` | Read the journal |
| `create_thesis` | Pre-register a trade: statement, mechanism, risk budget, exit criteria; armed at once, or `cooling_off: true` for the 24h gate |
| `arm_thesis` | Arm a thesis whose cooling-off has passed — from here the criteria fire, never edit |
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
| `morning-checkup` | The daily run — the monitoring status and any anomaly alert awaiting an answer first, then fresh readings against every active thesis's criteria and anything that fired. This is the one to put on a schedule. |
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
to check the account against the agent's own baseline, independently of
all three: [tally.markets/compare](https://tally.markets/compare)

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
