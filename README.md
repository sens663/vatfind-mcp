# VATFind MCP

VAT and EIN lookup, company identity matching with source evidence, and company-record monitoring for AI agents.

VATFind is a hosted commercial service. This repository contains public connection instructions and discovery metadata for its remote MCP server. The service implementation is maintained separately.

## Connect

- **MCP endpoint:** https://vatfind.com/api/mcp
- **Transport:** Streamable HTTP (remote)
- **Authentication:** OAuth 2.0 authorization code with PKCE (S256)
- **Documentation:** https://vatfind.com/docs/mcp
- **Tool catalogue:** https://vatfind.com/docs/mcp/tools.json
- **Website:** https://vatfind.com/mcp-for-ai-agents
- **Official MCP Registry identity:** `com.vatfind/company-identity`

In an MCP client that supports remote Streamable HTTP and OAuth, add the endpoint above and complete VATFind's authorization flow. Sign in to a VATFind workspace and approve the connection. Available operations use that workspace's allowance.

For clients that read a remote `mcpServers` configuration:

```json
{
  "mcpServers": {
    "vatfind": {
      "type": "http",
      "url": "https://vatfind.com/api/mcp"
    }
  }
}
```

Client configuration formats vary; use the client's remote OAuth connector settings when it does not accept this format. A client limited to local stdio servers cannot connect directly.

## Gemini CLI

Install the extension from this public repository:

```sh
gemini extensions install https://github.com/sens663/vatfind-mcp
```

Restart Gemini CLI, then use `/mcp auth vatfind` to authorize the connection if prompted. Gemini CLI discovers VATFind's OAuth endpoints and registers a public client automatically. Sign in to your VATFind workspace and approve access. Use `/mcp list` to inspect the available tools.

The root `gemini-extension.json` connects to the hosted Streamable HTTP endpoint. It does not embed credentials or download a local VATFind server. VATFind account access and workspace allowance are required. Identifier matches and monitoring have the result boundaries described below.

## What agents can do

- Look up VAT numbers and US EINs, subject to country and record coverage.
- Match a supplier to company records and retain source evidence, match ambiguity, and warnings.
- Retrieve a previous check and inspect country capabilities and workspace usage.
- Create, list, update, run, and archive company-record monitors, and read their events.

### Tools

| Tool | Purpose |
| --- | --- |
| `check_vat_number` | Check a VAT number and its company-record association |
| `find_vat_number` | Find VAT identifiers from company information |
| `check_tax_identifier` | Check supported tax identifiers, including US EINs |
| `get_check` | Retrieve a previous check |
| `get_country_capabilities` | Inspect supported coverage and fields |
| `get_usage` | Read the workspace's usage |
| `create_vat_monitor` | Create a company-record monitor |
| `list_vat_monitors` | List monitors |
| `get_vat_monitor` | Retrieve a monitor |
| `update_vat_monitor` | Update a monitor |
| `run_vat_monitor` | Run a monitor |
| `archive_vat_monitor` | Archive a monitor |
| `list_vat_monitor_events` | List monitor events |
| `get_vat_monitor_event` | Retrieve a monitor event |

## Important result boundaries

VATFind checks identifier format and matches company records. It does **not** perform live tax-authority registration validation. A company-record match is not proof of current VAT registration status. Monitors detect company-record changes, not tax-authority VAT status changes.

Coverage and available fields vary by country. Preserve source evidence, ambiguity, and warnings when presenting results. MCP operations use live workspace data and allowance; REST sandbox fixtures are documented separately.

## Example requests

- “Find the VAT number for this supplier and show the source evidence and any ambiguous matches.”
- “Check whether this US EIN matches the company information I supplied.”
- “Show the available fields for this country before looking up the supplier.”
- “Monitor this supplier's company record and show the latest change events.”

## Discovery

- Server manifest: https://vatfind.com/.well-known/mcp.json
- Server card: https://vatfind.com/.well-known/mcp/server-card.json
- Agent guide: https://vatfind.com/agents
- Machine-readable documentation: https://vatfind.com/docs/mcp.md

## Support and service information

- Support: support@vatfind.com
- Pricing: https://vatfind.com/pricing
- Coverage: https://vatfind.com/coverage
- Security: https://vatfind.com/security
- Privacy: https://vatfind.com/privacy
- Service terms: https://vatfind.com/terms

The public documentation and configuration examples in this repository are available under the MIT license. VATFind's hosted service is governed by its service terms; this repository's license does not license the service implementation or grant rights to VATFind trademarks.
