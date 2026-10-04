# VeriTeknik plugin for Cursor

Connects Cursor to the VeriTeknik MCP server so the agent can work with your VeriTeknik infrastructure.

## What it can do

- Check live server health: load, memory, disks, services, listening ports and firewall
- Read logs, configuration files and backup status to troubleshoot an outage
- Read and change DNS records in zones your company owns
- Open support tickets and reply to existing ones
- Start, stop, restart and snapshot servers, install TLS certificates and publish static sites

## Setup

1. Install the plugin from the Cursor Marketplace.
2. Cursor connects to `https://veriteknik.com/api/mcp` and opens VeriTeknik's sign-in page (OAuth 2.1 with PKCE). No API key is needed.
3. On VeriTeknik's consent screen, choose the permission groups the agent receives and approve.

Requires an active VeriTeknik customer account. Server tools need Morpheus access enabled on the server in the VeriTeknik hub.

## Safety

Write tools are annotated, destructive actions ask for confirmation, and server commands are filtered by each server's own policy. Ordering a VPS or registering a domain is paid from your existing prepaid VeriTeknik balance and is capped by a per-connection monthly spend limit that defaults to 0.

## Links

- Documentation: https://veriteknik.com/docs/en/mcp/
- Support: https://veriteknik.com/en/support
- Privacy: https://veriteknik.com/en/privacy
- Official MCP Registry entry: `com.veriteknik/mcp`

## License

MIT
