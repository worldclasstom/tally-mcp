# Connect Tally to your AI

Five steps, about two minutes. Full guided version with copy buttons:
https://tally.markets/connect

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
the record changes.

## 3 · Say hello

> Get started with Tally, then run my checkup.

The server returns the full protocol; your AI briefs you on your
journal.

## 4 · Install the skill (optional, recommended)

https://tally.markets/tally-skill.md — in Claude: upload under
Settings → Skills. In ChatGPT: add it to a Project's files, so it loads
in every chat inside that project. Makes the AI proactive: float a
trade idea and it offers to pre-register; mention "taking profits" and
it checks your written rules first.

## 5 · Schedule the morning checkup

Daily scheduled task with exactly this prompt:

> Run my Tally checkup, gather fresh readings for each active thesis's
> criteria, log them with record_reading, and flag any criterion that
> has objectively fired.

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
own record: their journal and, if their AI reports it, the symbols it
researched or considered. The server reaches no third-party service on
the user's behalf, browses nothing, cannot read another account's data,
and cannot reach the user's brokerage. It never queries the AI client's
memory, chat history, or files.

**It cannot trade.** Read-only toward markets by architecture. No tool
places, modifies, or cancels an order on any venue, and none moves money
or crypto. A linked brokerage goes through SnapTrade on a read-only
connection; broker credentials never reach Tally.

**Tools.** Eleven, split five read and six write. Reads carry
`readOnlyHint` and run without per-call confirmation; the three one-way
operations — arming a thesis, firing a criterion, closing a thesis — are
marked destructive so the client always asks first. The agent-activity
report accepts symbols and kinds and no text.

**Data and retention.** What the user writes: theses, criteria, readings,
positions, notes. What their AI reports: a symbol, a kind, a time, kept
90 days. Any venue session key is encrypted at rest (AES-256-GCM) before
it touches the database. Receipts are public only for theses the owner
has closed. Export or deletion on request through
https://tally.markets/support; detail in https://tally.markets/privacy.

**Revoking access.** The grant is per user and can be revoked at any time
from the AI client's connector settings, which invalidates the token
immediately. Revoking does not delete the journal.

**Operator.** Tally is operated by [Prosperity Labs, LLC](https://prosperitylabs.co/).
https://tally.markets/support · https://tally.markets/terms ·
https://tally.markets/privacy
