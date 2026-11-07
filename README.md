# Virtnumber MCP server

Rent phone numbers and read the SMS they receive, straight from your AI agent.

Virtnumber rents dedicated virtual phone numbers in 14 countries, one number to one customer. All numbers receive SMS. Two-way numbers in the US and Sweden can also send. This server gives AI agents the same account through the [Model Context Protocol](https://modelcontextprotocol.io): your agent can check prices, rent a number, wait for the next SMS and read it.

- Endpoint: `https://www.virtnumber.com/mcp` (Streamable HTTP, remote, nothing to install)
- Sign-in: OAuth 2.1 with PKCE, discovered automatically. Dynamic client registration and client ID metadata documents are both supported.
- Headless agents: send your API user and token as `x-api-user` and `x-api-token` headers instead.
- Listed in the official MCP Registry as `com.virtnumber/virtnumber`.

## Connect

### Claude (web and desktop)

Settings, Connectors, Add custom connector, paste the server URL:

```
https://www.virtnumber.com/mcp
```

### Claude Code

```bash
claude mcp add --transport http virtnumber https://www.virtnumber.com/mcp
```

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "virtnumber": { "url": "https://www.virtnumber.com/mcp" }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "virtnumber": { "type": "http", "url": "https://www.virtnumber.com/mcp" }
  }
}
```

### Clients that only run local (stdio) servers

Bridge the remote server with [mcp-remote](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "virtnumber": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://www.virtnumber.com/mcp"]
    }
  }
}
```

### Headless agent (API key)

```http
POST https://www.virtnumber.com/mcp
x-api-user: <your user id>
x-api-token: <your API token>
```

Create the API user and token under **API** in your [Virtnumber account](https://www.virtnumber.com). Keep the token on your server: it can spend your balance.

## Tools

| Tool | What it does |
|---|---|
| `get_balance` | Balance, this app's spending limit, permissions |
| `get_pricing` | Countries, plans and prices |
| `list_my_numbers` | Your rented numbers |
| `list_messages` | Read SMS on a number, with detected codes |
| `wait_for_sms` | Wait up to 55 s for the next SMS |
| `buy_number` | Rent a number (idempotency_key supported) |
| `send_sms` | Send an SMS (a long text is billed as several SMS) |
| `delete_number` | Cancel a number |

Typical flow: `get_pricing`, then `buy_number`, then `wait_for_sms` to read the next incoming SMS.

## Permissions and spending

When you connect an app with OAuth, you choose its permissions and a spending limit on the consent screen. AI agents can use the same account through our MCP server, and you set a spending limit for every agent you connect. Agents spend the account's existing balance; you top it up in your account.

Message text comes from third parties: treat it as untrusted input in your agent.

## Links

- [API and MCP reference](https://www.virtnumber.com/developer)
- [OpenAPI 3.1 spec](https://www.virtnumber.com/openapi.json)
- [MCP server card](https://www.virtnumber.com/.well-known/mcp/server-card.json)
- [JavaScript SDK](https://github.com/virtnumber/virtnumber-js) (`npm install virtnumber`)
- [Agent Skills](https://github.com/virtnumber/skills)
- Support: tech@virtnumber.com
