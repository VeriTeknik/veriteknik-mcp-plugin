# Installing the VeriTeknik MCP server

VeriTeknik is a hosted (remote) MCP server. There is nothing to clone, build or run locally.

## Configuration

Add a remote MCP server with this URL:

```
https://veriteknik.com/api/mcp
```

- Transport: Streamable HTTP
- Authentication: OAuth 2.1 with PKCE. The server answers `401` with a `WWW-Authenticate` header that points to its OAuth metadata, and it supports Dynamic Client Registration and Client ID Metadata Documents, so the client registers itself. No API key or header is needed.

Example for clients that use an `mcpServers` JSON file:

```json
{
  "mcpServers": {
    "veriteknik": {
      "url": "https://veriteknik.com/api/mcp"
    }
  }
}
```

Some clients name the field differently for Streamable HTTP (for example `httpUrl` in Gemini CLI, or `"type": "streamableHttp"` with `"url"`).

## First use

1. On the first tool call the client opens VeriTeknik's sign-in page in the browser.
2. The user signs in with their VeriTeknik account and selects the permission groups the agent receives on the consent screen.
3. After approval the tools are available.

Requires an active VeriTeknik customer account (https://veriteknik.com).

## Verify

Ask: "List my VeriTeknik servers." The agent should call `list_vps` and return the company's servers.
