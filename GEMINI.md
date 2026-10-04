# VeriTeknik

The `veriteknik` MCP server manages a customer's VeriTeknik infrastructure: servers, DNS, backups, support tickets and the account.

- The server signs the user in with OAuth on first use; no API key is needed.
- Read a resource before changing it: `get_dns_zone` before a DNS write, `list_my_tickets` before opening a new ticket.
- Server tools (`inspect_server`, `read_server_log`, `check_backups`, `run_command`) need Morpheus access enabled on that server in the VeriTeknik hub.
- Destructive tools (deleting DNS records or servers, registering a domain) should be confirmed with the user first.
