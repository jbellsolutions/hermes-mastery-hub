# 08 — Native MCP (connect external tool ecosystems)

*Source: installed `hermes-agent` skill reference `native-mcp.md`. Verify with `hermes mcp --help`.*

Hermes has a **built-in MCP client**. Point it at MCP servers in config; on startup it
connects, discovers their tools, and exposes them as first-class tools alongside `terminal`,
`read_file`, etc. No bridge CLI needed. This is how you give a business agent capabilities
Hermes doesn't ship natively (a vendor's API, a database, a specialized web agent).

## Prerequisites

```bash
pip install mcp        # optional dependency; without it MCP is silently disabled
# Node.js for npx servers, uv for uvx servers
```

## Configure servers

Add to `~/.hermes/config.yaml` under `mcp_servers`:

```yaml
mcp_servers:
  # stdio transport (command-based)
  filesystem:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/docs"]
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env: { GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_..." }   # only this var is passed through
  # HTTP transport (remote)
  company_api:
    url: "https://mcp.example.com/mcp"
    headers: { Authorization: "Bearer sk-..." }
    timeout: 180
```

Also usable: `hermes mcp add NAME --url/--command`, `hermes mcp list`, `hermes mcp test NAME`,
`hermes mcp configure NAME`. Adding/removing servers requires a restart (or `/reload-mcp`).

## Tool naming

Discovered tools are registered as `mcp_{server}_{tool}` (hyphens/dots → underscores).
Example: server `github`, tool `list-issues` → `mcp_github_list_issues`.

## Security (important for business use)

- Stdio servers get a **filtered** environment — only `PATH/HOME/USER/LANG/TERM/SHELL/TMPDIR`
  and `XDG_*` are inherited. Your API keys are NOT passed unless you add them under `env:`.
  This prevents credential leakage to untrusted servers.
- Credential-like patterns in MCP error messages are auto-redacted before reaching the LLM.
- Disable sampling for untrusted servers: `sampling: { enabled: false }`.

## Third-party servers relevant to the business roles

These are **third-party integrations**, not core Hermes features — install and authenticate
separately. Examples the machine this hub was built on already has as installed skills:

- **rtrvr / Retriever** (`retriever` skill) — DOM-native web agent (scrape, extract, fill
  forms, browser workflows) via MCP at `mcp.rtrvr.ai`. Useful for the research-analyst and
  lead-gen work. Requires a rtrvr API key.
- **notebooklm**, **super-browser**, **vapi**, **elevenlabs-agents** — other installed
  MCP/tool skills. Only claim a capability if the server is actually configured and authed.
