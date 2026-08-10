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
