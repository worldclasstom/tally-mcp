# Connect Tally to your AI

Five steps, about two minutes. Full guided version with copy buttons:
https://tally.markets/connect

## 1 · Add the connector

URL for every client: `https://tally.markets/api/mcp`

- **Claude**: Settings → Connectors → Add custom connector → name it
  Tally, paste the URL. Leave the OAuth fields empty.
- **ChatGPT**: Plugins → New plugin → name it Tally, paste the URL as
  the Server URL, Authentication: OAuth. Requires developer mode.
  Optional icon: https://tally.markets/logo-256.png
- **Claude Code**:
  `claude mcp add --transport http tally https://tally.markets/api/mcp`
  then `/mcp` to sign in.

## 2 · Sign in and approve

Your AI opens Tally's sign-in (free account, no card:
https://tally.markets/sign-up). Reads are safe to always-allow; keep
writes on approval if you want a human click before the record changes.

## 3 · Say hello

> Get started with Tally, then run my checkup.

The server returns the full protocol; your AI briefs you on your
journal.

## 4 · Install the skill (optional, recommended)

https://tally.markets/tally-skill.md → upload in Claude → Settings →
Skills. Makes the AI proactive: float a trade idea and it offers to
pre-register; mention "taking profits" and it checks your written rules
first.

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
own journal. The server reaches no third-party service on the user's
behalf, browses nothing, and cannot read another account's data. It
never queries the AI client's memory, chat history, or files.

**It cannot trade.** Read-only toward markets by architecture. No tool
places, modifies, or cancels an order on any venue, and none moves money
or crypto. A linked brokerage goes through SnapTrade on a read-only
connection; broker credentials never reach Tally.

**Tools.** Ten, split five read and five write. Reads carry
`readOnlyHint` and run without per-call confirmation; the three one-way
operations — arming a thesis, firing a criterion, closing a thesis — are
marked destructive so the client always asks first.

**Data and retention.** What the user writes: theses, criteria, readings,
positions, notes. Any venue session key is encrypted at rest
(AES-256-GCM) before it touches the database. Receipts are public only
for theses the owner has closed. Export or deletion on request at
support@tally.markets; detail in https://tally.markets/privacy.

**Revoking access.** The grant is per user and can be revoked at any time
from the AI client's connector settings, which invalidates the token
immediately. Revoking does not delete the journal.

**Operator.** Tally is operated by Prosperity Labs, LLC.
support@tally.markets · https://tally.markets/terms ·
https://tally.markets/privacy
