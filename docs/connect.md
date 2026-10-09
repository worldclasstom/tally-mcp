# Connect Tally to your AI

Five steps, about two minutes. Full guided version with copy buttons:
https://tally.markets/connect

The agent itself is set up on Tally's status screen in four steps:
subscribe, start a free trial at Propr and create an API key, paste the
key into Tally and choose the markets, authorize the agent by name. The
connector is optional and reads the result.

## 1 · Add the connector

URL for every client: `https://tally.markets/api/mcp`

- **Claude**: Settings → Connectors → Add custom connector → name it
  Tally, paste the URL. Leave the OAuth fields empty.
- **ChatGPT**: Tally Markets is a listed app — one click from the
  directory, no developer mode:
  https://chatgpt.com/plugins/plugin_asdk_app_6a7b8e021b148191aa85f69711529637
  (Manual fallback if your workspace hides the directory: developer
  mode → Plugins → New plugin → paste the URL as the Server URL,
  Authentication: OAuth.)
- **Claude Code**:
  `claude mcp add --transport http tally https://tally.markets/api/mcp`
  then `/mcp` to sign in.

## 2 · Sign in and approve

Your AI opens Tally's sign-in (https://tally.markets/sign-up; the
connector needs a subscribed account, $4.99 a month). Reads are safe to
always-allow; keep writes on approval if you want a human click before
the journal changes.

## 3 · Say hello

> Get started with Tally, then run my checkup.

The server returns the full protocol; your AI gives you the agent's
status, the word, equity, the distance to each limit, open positions and
anything that halted, and briefs you on your journal.

## 4 · Install the skill (optional, recommended)

https://tally.markets/tally-skill.md — in Claude: upload under
Settings → Skills. In ChatGPT: add it to a Project's files, so it loads
in every chat inside that project. Makes the AI proactive: float a
trade idea and it offers to pre-register; mention "taking profits" and
it checks your written rules first.

## 5 · Schedule the morning checkup

Daily scheduled task with exactly this prompt:

> Run my Tally checkup: give me the agent's status first (the word,
> equity, the distance to each limit, open positions, and anything
> halted or needing me); then, for each active thesis, gather fresh
> readings for its criteria, log them with record_reading, and flag any
> criterion that has objectively fired.

The agent itself ticks on Tally's server every hour whether or not this
schedule exists; the schedule is how you hear the morning read.

## For security review

What an administrator evaluating this connector needs. Every claim is
checkable against the connector itself.

**Endpoint.** One hosted endpoint, `https://tally.markets/api/mcp`, over
streamable HTTP. Same URL for every user; identity comes from the access
token, never the URL. Nothing to install, no local process.

**Authentication.** OAuth 2.0 with dynamic client registration and PKCE —
no shared secret to distribute. Consent is hosted at
accounts.tally.markets. Scopes requested: `openid`, `profile`, `email`,
and nothing else.

**What it can reach.** Every tool is scoped to the authenticated user's
own record: the agent's status (the word, equity and the distance to
each limit, open positions by market and side, the last tick, anything
halted) and their journal. The server reaches no third-party service on
the user's behalf, browses nothing, cannot read another account's data,
and never holds or returns the user's Propr API key. It never queries
the AI client's memory, chat history, or files.

**The connector cannot trade.** No tool places, modifies, or cancels an
order, and none moves money or crypto. Orders on a user's Propr account
are placed only by Tally's own agent service, on the one account the
user authorized by name on the status screen, and never through this
connector or at an AI's request.

**Tools.** Twelve, split five read and seven write. Reads carry
`readOnlyHint` and run without per-call confirmation; the checkup
returns the agent's status and the journal; the three one-way
operations — arming a thesis, firing a criterion, closing a thesis — are
marked destructive so the client always asks first.

**Data and retention.** What the user writes: theses, criteria, readings,
positions, notes. What the agent service writes back: the account's
status and the daily report. The user's Propr API key is encrypted at
rest (AES-256-GCM) before it touches the database and is decrypted only
by the agent service. Receipts are public only for theses the owner has
closed. Export or deletion on request through
https://tally.markets/support; detail in https://tally.markets/privacy.

**Revoking access.** The grant is per user and can be revoked at any time
from the AI client's connector settings, which invalidates the token
immediately. Revoking does not delete the journal or stop the agent; the
agent is stopped from the status screen.

**Operator.** Tally is operated by [Prosperity Labs, LLC](https://prosperitylabs.co/).
https://tally.markets/support · https://tally.markets/terms ·
https://tally.markets/privacy
