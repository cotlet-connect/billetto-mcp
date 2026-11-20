# Billetto MCP server for ChatGPT and Claude

Billetto connects to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev, so your AI agent can work with events, orders and attendees in your own Billetto organiser account. This repository provides a Python MCP stdio adapter for the hosted Cotlet.dev browser tools.

Unofficial, not affiliated with, endorsed by or sponsored by Billetto.

## What can an AI agent do in Billetto?

The [live Billetto integration page](https://cotlet.dev/integrations/billetto-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=billetto) describes 14 hosted workflow tools:

- Read events, ticket types, campaigns, orders and attendees.
- Add order notes and request refunds for current events.
- Cancel free tickets or resend ticket PDFs.
- Email guest reports, create access codes and clone events.

The page lists seven read and seven write operations. Write operations ask for approval by default; approval settings can be changed per tool. Review recipients, ticket IDs, refund amounts and report contents before approving a change or send. A cloned event needs its date and review before publication.

## How do I connect Billetto to ChatGPT or Claude?

1. [Get access through Cotlet.dev](https://cotlet.dev/signup?app=billetto&utm_source=github&utm_medium=readme&utm_campaign=billetto).
2. Open `billetto.com` in your Cotlet.dev Cloud Browser and sign in to your organiser account yourself.
3. In ChatGPT, add an MCP connector. In Claude, add a custom connector. Use `https://cotlet.dev/mcp/v1/connect` and sign in through Cotlet.dev.
4. Enable Billetto for agents and ask your agent to list your events.

## How do I connect Codex or Claude Code?

Create an API key in the Cotlet.dev console and set `COTLET_API_KEY` in your environment.

Codex configuration:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

Claude Code:

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## How do I run the local MCP adapter?

Use Python 3.10 or newer. The adapter uses the standard library.

```bash
git clone https://github.com/cotlet-connect/billetto-mcp.git
cd billetto-mcp
python3 billetto_agent.py serve
```

For a client that starts local MCP servers, use an absolute path to the downloaded adapter:

```json
{"mcpServers":{"billetto":{"command":"python3","args":["/absolute/path/billetto-mcp/billetto_agent.py","serve"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

The local adapter exposes seven browser helpers: `billetto_guide`, `billetto_status`, `billetto_open`, `billetto_read`, `billetto_click`, `billetto_type` and `billetto_close`. These are browser helpers, distinct from the hosted workflow catalog on the integration page. Call the guide first. Without an API key, listing tools works and tool calls return setup instructions.

```bash
python3 billetto_agent.py connect
python3 billetto_agent.py guide
python3 billetto_agent.py status
```

The adapter forwards calls to `https://cotlet.dev/mcp/v1`; it requires a Cotlet.dev account and a signed-in Billetto Cloud Browser. At a sign-in page, sign in yourself. Close the browser session when finished.

## Does Billetto have an official API or MCP server?

The live Cotlet.dev page reports a public Billetto API v3 that reads events, orders and attendees and sends webhooks, but no official MCP server or refund, cancellation or ticket resend endpoints, checked 27 October 2026. Use Billetto's own documented integration when it covers your task.

## What should I ask first?

Start with a read, such as "List my upcoming events" or "Show attendees for the event I select". Refunds apply to current events; free tickets use cancellation. Ask your agent to prepare the proposed changes and review them before approval.

Source: [Billetto on Cotlet.dev](https://cotlet.dev/integrations/billetto-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=billetto). Source page retrieved 20 November 2026.
