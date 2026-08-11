# Tally — the risk officer for AI-assisted trading

> Every trading connector makes your AI more capable. **Tally makes it
> accountable.**

Tally ([tally.markets](https://tally.markets)) is a trading-discipline
layer that mounts inside Claude, ChatGPT, Claude Code, and any
MCP-capable agent. Your AI is the analyst — it researches, monitors, and
gathers evidence. Tally holds the record neither you nor the model can
rewrite: pre-registered theses, exit rules that freeze before capital
moves, a 24-hour cooling-off, an override ledger for rule-breaking
exits, and a public receipt for every closed trade.

**Tally never executes trades.** Every broker and venue connection is
read-only at the API level — no order placement, no custody, by
architecture and by policy.

## Connect

**Server URL (Streamable HTTP · OAuth 2.0 with dynamic client
registration):**

```
https://tally.markets/api/mcp
```

| Client | How |
|---|---|
| **Claude** | Settings → Connectors → Add custom connector → name it `Tally`, paste the URL. Leave the OAuth fields empty — Tally registers itself. |
| **ChatGPT** | Plugins → New plugin → name it `Tally`, paste the URL as the Server URL, Authentication: OAuth. (Requires developer mode.) |
| **Claude Code** | `claude mcp add --transport http tally https://tally.markets/api/mcp`, then `/mcp` to sign in. |
| **Any MCP agent** | Point it at the URL above; OAuth discovery does the rest. |

Then say: **"Get started with Tally, then run my checkup."** The server
teaches your AI the whole protocol on first contact.

Full walkthrough (skill, scheduled morning checkup, broker linking):
[tally.markets/connect](https://tally.markets/connect)

## Tools

| Tool | What it does |
|---|---|
| `get_started` | Returns the discipline protocol — the AI teaches itself |
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

Market-data connectors inform your AI. Execution connectors let it
trade. Journal connectors show it your past. Tally is the one built to
constrain — it runs alongside all of them:
[tally.markets/compare](https://tally.markets/compare)

## Privacy & security

- OAuth per user; your AI sees your journal only, under a grant you can
  revoke any time.
- Brokerage data (via SnapTrade) is read-only; broker credentials never
  touch Tally.
- Details: [privacy](https://tally.markets/privacy) ·
  [terms](https://tally.markets/terms) · support@tally.markets

---

*Tally is a hosted service. This repository is its documentation and
public manifest, not its source — there is nothing here to install or
run. Point your AI at the connector URL above; OAuth does the rest.*
