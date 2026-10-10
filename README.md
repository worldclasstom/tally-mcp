# Tally — an AI agent for prop firm challenges

> Building an AI agent to pass prop firm challenges, hosted. **Tally
> runs it for you.**

Tally ([tally.markets](https://tally.markets)) runs a trading agent on
your Propr challenge, free trial or paid. You start at Propr, paste one API key
into Tally, choose the markets, and authorize the agent by name. It then
trades the challenge on fixed rules, with one scan a day after the daily
close and a tick every hour, a reduce-only protective stop on every
position resting at Propr, and a halt before Propr's limits. You see
every order on the status screen and in the daily report, and you stop
it with one click. Tally never buys a challenge, moves funds or requests
a payout. Propr accounts are simulated.

This connector is how an AI running inside Claude, ChatGPT, Claude Code,
or any MCP-capable client reads that, and runs the journal beside it:

- **The AI reads the agent** (`get_checkup`): its status (Running, Paused
  for the day, Stopping, Stopped, Halted, Needs attention), equity, the
  distance to the daily limit and to the drawdown limit, open positions
  with their stops, the last tick, the sentence that says what halted or
  what needs you, and the record so far (closed positions with why each
  opened and closed, win rate, net P&L). The morning checkup leads with it.
- **The AI cannot touch the agent.** No tool places an order, changes a
  rule, or stops the agent. Stop and Start again live on the status
  screen, and only you press them.
- **Pre-registered theses**, optional, for trades you make yourself: a
  statement, exit rules that freeze at creation, an optional 24-hour
  cooling-off, an override ledger for rule-breaking exits, and a public
  receipt for every closed trade.
- **The agent's own records** for an AI that trades elsewhere, optional:
  `declare_mandate` and `log_agent_activity` keep structure only, never
  text, for the journal's sake.

**Nothing in this connector can trade.** Orders on your Propr account
are placed only by Tally's own agent service, on the one account you
authorized by name, and never through this connector or at an AI's
request.

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

Full walkthrough (skill, scheduled morning checkup, the four setup
steps): [tally.markets/connect](https://tally.markets/connect)

## Tools

| Tool | What it does |
|---|---|
| `get_started` | Returns the protocol — the AI teaches itself |
| `get_checkup` | Morning briefing: the agent's status first (running or not, equity, the distances, positions with stops, anything halted), then active theses, deadlines, evidence, closed history and lifetime R |
| `list_theses` / `get_thesis` | Read the journal |
| `create_thesis` | Pre-register a trade: statement, mechanism, risk budget, exit criteria; armed at once, or `cooling_off: true` for the 24h gate |
| `arm_thesis` | Arm a thesis whose cooling-off has passed — from here the criteria fire, never edit |
| `record_reading` | Log evidence against a criterion (the generic sensor primitive) |
| `fire_criterion` | Mark a rule objectively triggered, with cited evidence — one-way |
| `close_thesis` | Close a trade; discretionary closes require a written justification, logged forever |
| `get_receipt_link` | The public link for a receipt the owner explicitly published |
| `declare_mandate` | Optional record for an AI that trades elsewhere: what it is supposed to do, as structure |
| `log_agent_activity` | Optional record for an AI that trades elsewhere: symbols, kinds and a setup tag; no text |

Reads are safe to always-allow; writes are guarded by the protocol
itself (cooling-off, criteria minimums, override justifications).

## Prompts

Clients with a prompt picker get the three jobs Tally exists for, ready
to run:

| Prompt | What it does |
|---|---|
| `morning-checkup` | The daily run — the agent's status first, then fresh readings against every active thesis's criteria and anything that fired. This is the one to put on a schedule. |
| `pre-register-trade` | Turns an idea into a pre-registered thesis, interviewing you until it is falsifiable |
| `close-out` | Walks a thesis to its exit — what the written rules demand, and the justification the override ledger requires if you are closing early |

## What the agent does

One scan a day after the daily close. A close above the prior 20 days'
highs opens a long; below the prior 20 days' lows opens a short; a close
back through the prior 10 days' range closes it. Each position risks
0.4% of the starting balance with a stop two ATR from the fill resting
at Propr; at most three positions, gross exposure never above twice the
equity. The agent stops opening positions for the day at a 2% loss of
equity (inside Propr's 3%) and closes everything and stops for good at
4.5% below the starting balance (inside Propr's 6%). Passing is the aim,
not a result anyone can promise.

## How it compares

Trading the challenge by hand, following a signal group, running an
agent on your own computer, or Tally:
[tally.markets/compare](https://tally.markets/compare)

## Privacy & security

- OAuth per user; your AI sees your own record only, under a grant you
  can revoke any time.
- Your Propr API key is encrypted at rest and decrypted in memory by the web app to list accounts and by the
  agent service to read or trade the selected account; it never passes through this connector.
- Details: [privacy](https://tally.markets/privacy) ·
  [terms](https://tally.markets/terms) · [support](https://tally.markets/support)

---

*Tally is a hosted service. This repository is its documentation and
public manifest, not its source — there is nothing here to install or
run. Point your AI at the connector URL above; OAuth does the rest.*

Receipts are private until their owner selects Publish receipt in the journal.
The connector returns private journal links for ordinary thesis reads and exits;
`get_receipt_link` returns a public URL only after that explicit opt-in.
Trading status reflects pending Stop, failed ticks, and stale observations even
when billing ends. Stop and receipt privacy controls remain available in Tally.
