<p align="center">
  <img src="assets/logo-400.png" alt="VeriTeknik" width="96" height="96">
</p>

<h1 align="center">VeriTeknik MCP server</h1>

<p align="center">
  Manage your VeriTeknik VPS, dedicated servers, DNS, domains, invoices and support tickets
  from your AI agent: Cursor, Claude, ChatGPT, Gemini CLI, VS Code or any MCP client.
</p>

<p align="center">
  <a href="https://veriteknik.com/docs/en/mcp/">Documentation</a> ·
  <a href="https://veriteknik.com/docs/mcp/">Türkçe belge</a> ·
  <a href="https://registry.modelcontextprotocol.io/v0/servers?search=com.veriteknik">MCP Registry: <code>com.veriteknik/mcp</code></a> ·
  <a href="https://veriteknik.com/en/support">Support</a>
</p>

---

[VeriTeknik](https://veriteknik.com) is an infrastructure and managed hosting provider in Turkey: VPS, dedicated servers, domains, DNS, monitoring and managed services.
The **VeriTeknik MCP server** (`https://veriteknik.com/api/mcp`) lets your AI agent look at your infrastructure, troubleshoot it and act on it on your behalf, inside limits you set.
In a coding agent you can fix the code and the server it runs on from the same chat: read the production error log, find the bug in your repo, deploy, check the service came back.

There is no API key to copy.
You sign in with OAuth, pick what the agent may touch, set a monthly spend cap, and revoke the connection with one click whenever you like.

## Ask your agent things like

- *"List my VeriTeknik servers. Which ones are running?"*
- *"web-01 feels slow. Check load, memory, disks and failed services."*
- *"Show me the last 200 lines of the nginx error log on web-01 and tell me what is wrong."*
- *"Find where `TypeError` appears under /var/www/app."*
- *"Is MySQL on db-01 reachable from the internet?"*
- *"When was the last backup taken on db-01, and how big is it?"*
- *"Point `staging.example.com` to 203.0.113.10 and add the Microsoft verification TXT record."*
- *"Is `mybrand.com.tr` available? How much is it for two years?"*
- *"Create a 2 vCPU / 4 GB Ubuntu VPS, install nginx and a Let's Encrypt certificate for example.com, and publish this static site on it."*
- *"Do I have any unpaid invoices? When does my contract renew?"*
- *"Open a support ticket with this error and a summary of the logs."*

## What it can do

The server exposes **46 tools**, grouped into permission groups you approve on the consent page.
A tool from a group you did not grant never appears in your agent's tool list.

### Servers and VPS

| Tool | What it does | Permission group |
|---|---|---|
| `list_vps` | List your VPS with status and IP addresses | Servers: view |
| `get_vps_details` | One server in full: IPs, OS, CPU/RAM/disk/bandwidth, Morpheus access state | Servers: view |
| `get_vps_bandwidth_usage` | Bandwidth used against the included quota, and any overage | Servers: view |
| `get_vps_monitor_status` | Uptime monitoring: up right now, 24h/7d/30d uptime, ping, recent events | Servers: view |
| `check_service_status` | Live up/down status of every monitored server and domain | Servers: view |
| `list_my_dedicated_servers` | Your dedicated servers with IPs, resources and linked contract | Servers: view |
| `list_dedicated_servers` | Dedicated servers for sale, with prices | Servers: view |
| `list_vps_catalog` | Orderable VPS sizes, OS images and one-click applications | Servers: view |
| `estimate_cost` | Price quote for a VPS configuration, and whether your balance covers it | Servers: view |
| `start_vps`, `stop_vps`, `restart_vps` | Power actions | Servers: manage |
| `list_vps_snapshots`, `create_vps_snapshot` | List snapshots, take a new one | Servers: manage |
| `create_vps` | Order a new VPS, paid from your prepaid balance | Servers: create and delete |
| `delete_vps` | Delete a VPS created through MCP, after a refund preview and exact hostname confirmation | Servers: create and delete |

### Inside your server (read-only)

These tools connect to your server over SSH through Morpheus, VeriTeknik's DevOps agent.
They read, they never change anything.

| Tool | What it does | Permission group |
|---|---|---|
| `inspect_server` | Health snapshot: load, memory, disks, running and failed services, containers, listening ports, firewall, exposed database ports | Servers: view |
| `read_server_log` | The last lines of a known log (nginx, Apache, PHP, MySQL, syslog...), with search | Server investigation |
| `read_server_file` | A window of lines from a text file | Server investigation |
| `list_server_dir` | Directory listing | Server investigation |
| `search_server_files` | Text search under a directory | Server investigation |
| `check_backups` | When the newest backup was taken, its size, and whether a schedule exists | Server investigation |

Readable places are limited to `/var/log`, `/var/www`, `/srv`, `/opt`, `/home` and web, PHP and database configuration directories.
Credential files (`.env`, private keys, `.ssh`, `.aws`, password and database files) are never opened, and every returned line passes through a secret masker.

### Server tasks

| Tool | What it does | Permission group |
|---|---|---|
| `run_command` | Run a diagnostic command from an allowlist (`systemctl status`, `journalctl`, `df`, `ss`, `docker ps`, `nginx -t`, `dig`, `curl -I`...) | Servers: execute tasks |
| `setup_ssl` | Install nginx and a TLS certificate, after checking DNS points at the server | Servers: execute tasks |
| `deploy_static_site` | Publish static files under `/var/www/<domain>/` and reload nginx | Servers: execute tasks |
| `run_admin_command` | Changing commands: install packages, configure and restart services, write files, add firewall rules | Servers: admin commands |

Every command is still judged by the server's own Morpheus policy (BIOS rules, safety classification, autonomy level).
The destructive class (mass deletion, disk formatting, user removal, reinstall) never runs over MCP.

### Domains and DNS

| Tool | What it does | Permission group |
|---|---|---|
| `list_my_domains` | Your domains with status and expiry dates | Domains: view |
| `search_domain` | Is a name available, and what does it cost | Domains: view |
| `get_domain_pricing` | Registration, renewal and transfer prices for an extension (`.com`, `.com.tr`...) | Domains: view |
| `get_dns_zone` | A domain's DNS zone and all of its records | Domains: view |
| `list_domain_monitors`, `get_domain_monitor_status` | Domain uptime monitors, certificate lifetime included | Servers: view |
| `add_dns_record`, `update_dns_record`, `delete_dns_record` | Change DNS records; a value is never overwritten unless you name it | DNS: manage |
| `register_domain` | Register a domain (1-10 years), paid from your prepaid balance | Domains: register and manage |

### Billing, support and team

| Tool | What it does | Permission group |
|---|---|---|
| `get_balance` | Available balance, reserved advance, uninvoiced usage, connection budget | Billing: view |
| `list_my_invoices`, `get_invoice_details` | Invoices and their line items | Billing: view |
| `list_my_contracts`, `get_contract_details` | Contracts, items and renewal terms | Billing: view |
| `list_my_tickets`, `get_ticket_details` | Your support tickets and their conversation | Support: view your own tickets |
| `create_ticket`, `add_ticket_comment` | Open a ticket, reply to one | Support: open and reply to tickets |
| `list_company_members` | Team members, roles and status | Team: view |

The full, always current reference is in the [documentation](https://veriteknik.com/docs/en/mcp/#what-your-agent-can-do-today).

## Install

The server is hosted: there is nothing to clone, build or run.
Add the URL `https://veriteknik.com/api/mcp` to your client, and the first tool call opens VeriTeknik's sign-in and consent page in your browser.

### Cursor

Install **VeriTeknik** from the Cursor Marketplace, or add this to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "veriteknik": {
      "url": "https://veriteknik.com/api/mcp"
    }
  }
}
```

Cursor marks the server **Needs login**; click it to sign in.

### Claude Desktop and claude.ai

**Settings > Connectors > Add custom connector**, paste `https://veriteknik.com/api/mcp`, then sign in and approve.

### Claude Code

```bash
claude mcp add --transport http veriteknik https://veriteknik.com/api/mcp
```

Then type `/mcp`, choose `veriteknik` and sign in.

### ChatGPT

Turn on **Developer mode** (Settings > Security and login), then **Settings > Connectors > Create** and put `https://veriteknik.com/api/mcp` in the MCP server URL field.
In ChatGPT the two ordering tools (`create_vps`, `register_domain`) are not offered; order in the VeriTeknik hub instead.

### Gemini CLI

```bash
gemini extensions install https://github.com/VeriTeknik/veriteknik-mcp-plugin
```

Then run `/mcp auth veriteknik` inside a session to sign in.

### VS Code, Windsurf, Codex and others

The endpoint is a single Streamable HTTP URL.
Any client that supports remote MCP servers with OAuth 2.1 and dynamic client registration can connect.
Exact configuration for VS Code / GitHub Copilot, Windsurf, Codex, Devin, Kilo Code, LM Studio, Cline and JetBrains is in the [setup guide](https://veriteknik.com/docs/en/mcp/#setup).
If an agent is installing this for you, point it at [`llms-install.md`](llms-install.md).

## Before you start

1. **A VeriTeknik customer account** that belongs to a company.
2. **MCP access switched on for your company.** A company owner or admin turns on **Enable MCP access** under **Settings > Agent Connections > AI Agent Access (MCP)** in the VeriTeknik hub. It is off by default.
3. **Two-factor authentication** is optional but strongly recommended; with it on, approving or widening a connection asks for a fresh code.
4. **For the server tools**, Morpheus access must be enabled on that server (**Servers > the server > Morpheus** in the hub). A VPS created with `create_vps` gets it automatically once it has booted.

## You draw the boundaries

On the consent page you choose, per connection:

- **Which company** the connection acts for.
- **Which permission groups** it receives. Read-only groups are on by default; anything that changes, orders or deletes is off until you switch it on. You can never grant more than your own role allows.
- **A monthly spend cap in USD.** The default is **0**, so a new connection cannot order anything until you raise it. Orders are paid only from your existing prepaid VeriTeknik balance; the connection never sees, stores or uses card details, and it cannot top up your balance.
- **A 90-day lifetime.** After that the connection expires and asks you again.

You can narrow, widen or revoke a connection at any time under **Settings > Agent Connections > Connected Apps (MCP)**.
Narrowing applies to the agent's very next call.

Other guardrails:

- Every state-changing call carries a request id, so a retried call never creates a second server, ticket or snapshot.
- Tool annotations mark which tools are read-only, which change things and which are destructive, so your client can ask before running them.
- The company always comes from the connection itself; an agent cannot name a different tenant.
- Reinstalling a server, resetting a root password, resizing, paying invoices and managing cards stay in the hub on purpose.

## Troubleshooting

| You see | Do this |
|---|---|
| *MCP access is disabled for this company* | Ask a company owner or admin to switch it on in the hub |
| *This connection lacks the permission ...* | Widen the connection with **Edit** on the Connected Apps card |
| *Monthly cap ... USD* | Raise the cap with **Edit** |
| *Morpheus SSH access is not enabled on this server* | Enable it under **Servers > the server > Morpheus** |
| *This connection has expired* | Reconnect from your client; you get a fresh 90 days |
| A tool you approved does not show up | Reload the tool list or start a new chat; some clients only read it at start |

More in the [documentation](https://veriteknik.com/docs/en/mcp/#when-something-goes-wrong), or [open a support ticket](https://veriteknik.com/en/support).

## Türkçe

VeriTeknik MCP sunucusu Cursor, Claude, ChatGPT, Gemini CLI, VS Code ve MCP destekleyen diğer yapay zekâ araçlarını VeriTeknik hesabınıza bağlar.
Ajanınız VPS ve fiziksel sunucularınızın durumuna bakabilir, logları okuyup sorunu bulabilir, DNS kayıtlarını değiştirebilir, alan adı sorgulayıp kaydedebilir, bakiyeden VPS açabilir, faturalarınızı ve sözleşmelerinizi okuyabilir, destek talebi açabilir.

API anahtarı yoktur: OAuth ile giriş yaparsınız, onay ekranında hangi izin gruplarını vereceğinizi ve bağlantının aylık harcama tavanını (varsayılan 0) siz seçersiniz, bağlantı 90 gün sonra kendiliğinden düşer.
Başlamadan önce bir şirket sahibi ya da yöneticinin panelde **Ayarlar > Ajan Bağlantıları > AI Ajanı Erişimi (MCP)** kartından **MCP erişimini etkinleştir** anahtarını açması gerekir.

Kurulum: istemcinize `https://veriteknik.com/api/mcp` adresini ekleyin; ilk araç çağrısında tarayıcıda giriş ve onay ekranı açılır.
İstemci başına kurulum adımları ve tüm araçların listesi: [veriteknik.com/docs/mcp](https://veriteknik.com/docs/mcp/).

## Links

- Documentation: <https://veriteknik.com/docs/en/mcp/> ([Türkçe](https://veriteknik.com/docs/mcp/))
- MCP endpoint: `https://veriteknik.com/api/mcp` (Streamable HTTP, OAuth 2.1 with PKCE and dynamic client registration)
- Official MCP Registry: `com.veriteknik/mcp`
- Support: <https://veriteknik.com/en/support>
- Privacy: <https://veriteknik.com/en/privacy>

## Repository layout

| Path | Used by |
|---|---|
| `.cursor-plugin/plugin.json` | Cursor plugin manifest |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension and the context it gives the model |
| `mcp.json` | The MCP server definition the Cursor plugin loads |
| `llms-install.md` | Install instructions written for an AI agent doing the setup |
| `assets/logo.png`, `assets/logo-400.png` | Logo |

This repository holds only the client plugins; the MCP server itself runs at VeriTeknik.

## License

MIT
