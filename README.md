# Dinero MCP setup & connection checker

Prepare Dinero for an AI workflow: understand the required credentials, generate MCP client configuration and inspect a hosted endpoint’s available tools.

**[Open Dinero in PipMCP](https://pipmcp.com/github/start?app=dinero&integration=dinero-mcp&utm_source=github&utm_medium=readme&utm_campaign=github-dinero-mcp&utm_content=readme)** · [Connection guide](https://jannikco.github.io/pipmcp-connectors/integrations/dinero/) · [Quick start](#quick-start)

> **Included:** provider setup inventory, MCP client configuration and hosted tool discovery. Provider API execution takes place through the connected service, not this utility.

## A clear path from account to AI client

Use this project when you need to connect an authorized Dinero account and want to check the setup before using it in an MCP client. It keeps the provider’s credential prerequisites and the client’s hosted endpoint configuration in one inspectable place.

PipMCP provides a managed path for API import, credential configuration and MCP access. Use its hosted onboarding to connect the provider, then bring the resulting endpoint into your AI client.

A managed catalog template is listed. Connect your authorized provider account, review the available tools and copy the hosted MCP endpoint.

## Quick start

Requires **Node.js 22 or later** and npm. The first commands run offline and need no account or credentials.

```bash
git clone https://github.com/jannikco/dinero-mcp.git
cd dinero-mcp
npm ci --ignore-scripts
npm test
npm start -- --setup
```

The setup command prints this app’s recorded authentication field names, provider documentation, setup link and required client environment variables. It never asks for provider secret values. The output includes:

```json
{
  "app": "Dinero",
  "appSlug": "dinero",
  "requiredEnvironment": [
    "PIPMCP_ENDPOINT",
    "PIPMCP_TOKEN"
  ]
}
```

## Connect a hosted endpoint

1. [Open Dinero in PipMCP](https://pipmcp.com/github/start?app=dinero&integration=dinero-mcp&utm_source=github&utm_medium=readme&utm_campaign=github-dinero-mcp&utm_content=readme) and complete the provider connection or manual import described there.
2. Copy your hosted MCP endpoint and its access token from the product. A provider API key and an MCP endpoint token serve different purposes.
3. In Bash, read the values interactively so the token is not embedded in a command or committed file:

```bash
read -r -p "Hosted MCP endpoint: " PIPMCP_ENDPOINT
read -r -s -p "Hosted MCP token: " PIPMCP_TOKEN
printf '\n'
export PIPMCP_ENDPOINT PIPMCP_TOKEN
npm start
npm start -- --live
```

`npm start` prints MCP client configuration when an endpoint is set; without one it prints the setup inventory. `--live` performs protocol initialization and read-only tool discovery. Its JSON reports `state`, `toolCount`, `toolNames` and `providerLiveValidation: "not-run"`. Use the emitted configuration in your MCP client, then test your intended provider operation there.

## Capabilities and evidence

| Capability | What the released code does | Evidence boundary |
| --- | --- | --- |
| Provider setup inventory | Displays Dinero prerequisites and source links | Documented provider metadata; credentials are not validated locally |
| MCP client configuration | Builds configuration for the supplied hosted endpoint | Offline configuration tests |
| Hosted endpoint discovery | Initializes MCP and lists available tools with `--live` | Protocol access only; no `tools/call` |
| Dinero API execution | Not implemented in this setup utility | No provider operation or Live outcome is claimed |
| Managed provider operations | Depend on the imported/listed integration, account and permissions | Documented; verify each required operation in the hosted account |

## Dinero authentication prerequisites

- Recorded authentication type: **token exchange**.
- Provider credential field names: `provider_exchange_credentials`.
- Account or tenant identifiers: `organization_id`.

Enter provider credential values only in the intended hosted credential flow. This utility reads the hosted endpoint/token; it does not read or upload the provider’s secret fields. Use the provider’s official guide to confirm scopes, account tier and current prerequisites.

- [Provider API documentation](https://api.dinero.dk/openapi/v1/swagger.json)
- [Authentication / source reference 2](https://developer.dinero.dk/documentation/personal-integration/)

[Visit Dinero](https://www.dinero.dk/)

## Frequently asked questions

**Does installing this package let an AI call Dinero immediately?**
Complete the hosted connection and configure its MCP endpoint first; this repository handles setup and discovery.

**What should I check if discovery fails?**
Confirm that the endpoint uses HTTPS, the MCP token is current and the hosted connection is enabled. Include sanitized command output when requesting support.

**Where do my credentials go?**
The local utility reads the endpoint and MCP token from the environment. Provider credentials belong in the hosted connection flow. Do not paste either type into issues, source code or an API definition.

**What if a provider has no managed preset?**
Use the manual import guide and a supported API definition. Authentication and required operations must be reviewed before use; a preset is not implied by this repository’s name.

## Related projects

- [Typed LeadAdapter API client](https://github.com/jannikco/linkedin-typescript-sdk) — JavaScript runtime and generated TypeScript request types for the LeadAdapter REST API.
- [LeadAdapter Python API client](https://github.com/jannikco/linkedin-python-sdk) — Standard-library Python client for the LeadAdapter REST API.
- [LinkedIn API quickstart](https://github.com/jannikco/linkedin-api) — Inspect connected profile status and read stored leads through LeadAdapter.
- [LinkedIn people and post search](https://github.com/jannikco/linkedin-search-api) — Build asynchronous people and post search requests with explicit idempotency.

## Support and maintenance

[Report a reproducible issue](https://github.com/jannikco/dinero-mcp/issues) with command versions and sanitized output. For sensitive issues, use [PipMCP support](https://pipmcp.com/contact). Never publish tokens, connection URLs containing credentials or customer records.

Maintained by jannikco. MIT applies to this repository’s code; provider services and runtime dependencies have their own terms. Provider names do not imply endorsement. Recorded source revision: `6ccb875db90d6ed2e1471ddabfe7d007724b7d70fe30b0d9958bec5bef403785`. See [integration.json](./integration.json), [LICENSE](./LICENSE) and [SECURITY.md](./SECURITY.md).
