# 18 — Skills, Plugins, MCP & Hooks: the Extension Map

*Source: `user-guide/features/skills.md`, `developer-guide/creating-skills.md`, `features/curator.md`, `features/plugins.md`,
`features/plugin-catalog.md`, `developer-guide/plugins/*.md`, `features/mcp.md`, `reference/mcp-config-reference.md`,
`features/tool-search.md`, `features/hooks.md`, `developer-guide/middleware.md`, `features/context-files.md`,
`reference/toolsets-reference.md`, `plugin-catalog/` (main, 2026-10-12). Commands confirmed in `hermes_cli/subcommands/`.*

## Which one do I reach for?

| You want… | Use | Lives in |
|---|---|---|
| The agent to *know how* to do something (procedure, checklist, playbook) | **Skill** (`SKILL.md`) | `~/.hermes/skills/`, `skills.external_dirs`, project `.hermes/skills` |
| Always-on identity and tone | **SOUL.md** | profile home only |
| Always-on project rules | **Context file** (`.hermes.md` → `AGENTS.override.md` → `AGENTS.md` → `CLAUDE.md` → `.cursorrules`, first match) | project tree |
| A tool that already exists as a server, or isn't Python | **MCP server** | `mcp_servers:` in the profile's `config.yaml` |
| Custom tools in code, guardrails, a new model provider, a new chat channel, Desktop UI | **Plugin** (`register(ctx)`) | `~/.hermes/plugins/<name>/` or the catalog |
| Block/modify a tool call, inject context each turn — without Python | **Shell hook** (`hooks:` in config; exit 2 blocks) | `config.yaml` |
| To limit what a profile/platform can call | **Toolsets** (`platform_toolsets`, `custom_toolsets`) | `config.yaml` |

Rule of thumb: **skill first** (cheap, the agent maintains it), **MCP second** (reuse what exists), **plugin last**
(code you own and must trust).

## Skills — what changed and what matters

**Format is still Markdown + YAML frontmatter.** Fields: `name`, `description`, `version`, `author`, `license`,
`platforms`, `required_environment_variables`, and under `metadata.hermes`: `tags`, `related_skills`,
`requires_toolsets`, `requires_tools`, `fallback_for_toolsets`, `fallback_for_tools`, `config`, `blueprint`.
New since July: **`requires_apps`** (hide the skill unless an app is present on this host, fails closed).
`blueprint` suggests a cron job (reviewed via `/suggestions`, never auto-scheduled). Descriptions cap at 1024 chars.

```bash
hermes skills browse | search | inspect <id> | install <id|url|owner/repo/path>
hermes skills list | check | update | audit | uninstall <name>
hermes skills diff <name> | reset <name> --restore | list-modified
hermes skills tap add owner/repo
hermes skills trust <project>        # project-local skills only load once trusted
hermes skills snapshot export|import
hermes bundles create|list|show      # one slash command loads several skills
```

In chat: `/learn <dir|url|notes>` writes a skill from material; chain up to 5 skills: `/a /b do X`;
`/skills pending|approve|reject` when `skills.write_approval: true`.

**Sharing skills across your whole fleet**
```yaml
skills:
  external_dirs: [~/.agents/skills, /shared/team-skills]   # scanned by every profile that sets this
  create_dir: /shared/team-skills                         # new agent-written skills go here
```
External dirs are **writable by the agent** — any profile can patch a shared skill in place. Name precedence:
project → local → `create_dir` → `external_dirs` (higher silently shadows lower). `--clone` copies skills once; it
doesn't keep profiles linked.

**Curator** (background skill janitor): idle-triggered, every 168h, stale after 14 days, archive after 30, never
deletes (`skills/.archive/`). Only curates `created_by: agent` skills (from background review). Skills made with
`/learn` or in the foreground are **not** curated unless you `hermes curator adopt`. Pinning blocks archiving but the
background review can still edit pinned skills. Every change is in `.curator_ledger.jsonl`;
`hermes curator rollback <entry-id>` undoes one.

## Plugins

A plugin's `register(ctx)` can add tools, hooks (44 hook points), middleware, slash and CLI commands, skills
(read-only, namespaced `plugin:skill`), system-prompt sections (cache-safe, ≤4k chars each), model/image/TTS/STT/
computer-use providers, memory providers, chat platforms, secret sources and Desktop UI.

```bash
hermes plugins search <q> | browse | show <name>
hermes plugins install <catalog-name | owner/repo> [--ref <40-char sha>] [--enable]
hermes plugins enable|disable|list|update|remove|capabilities|check-updates|doctor
```

- **Catalog:** 570 entries on 2026-10-12 (12 official, the rest community), every entry pinned to an exact commit SHA.
  Categories: tools, desktop, memory, models, automation, general, web, platform, voice.
- **Installed ≠ enabled.** Third-party plugins only load if listed in `plugins.enabled`.
- Capabilities (`tools.override`, `llm.*_override`, `gateway.platform_actions`) need consent at install and again if they grow.
- Install-time scan returns safe / caution / dangerous; `--force` cannot override dangerous. `removed.yaml` blocks known-bad plugins.
- `plugins.isolation: host` runs plugins in a separate process per profile.
- **A catalog listing is not an audit.** The docs say plainly that plugins are not a sandbox.
- Plugins that imported internal Hermes modules broke after the September decomposition — pin versions.

## MCP — connect lots of tools without drowning the context

```yaml
mcp_servers:
  crm:
    url: https://mcp.example.com/mcp
    auth: oauth
    lazy: true                       # start on first call (after one live connect has cached the schema)
    trust: untrusted                 # approval for any tool not marked readOnlyHint
    tools:
      include: [search_*, get_*]     # original tool names, globs
      resources: false
      prompts: false
```
```bash
hermes mcp catalog | install <name>   # 65 Nous-reviewed servers
hermes mcp add | list | test | login | reauth | remove
hermes mcp serve                      # expose Hermes itself as an MCP server
/reload-mcp                           # gateway also watches config.yaml (~1 min)
```

- Tools are named `mcp__<server>__<tool>`.
- **Tool Search** replaces deferred MCP/plugin tools with `tool_search`, `tool_describe`, `tool_call` and keeps a small
  listing (≤ 5% of context / 4000 tokens). Turn it on once you have more than a handful of servers.
- Composio-style aggregators: filter hard with `include` — a thousand tools in context will wreck tool choice.

## Hooks & middleware

- **Shell hooks** (`hooks:` block): any language, run as a subprocess, follow Claude Code conventions (exit 2 blocks
  the call). `fail_closed` makes a crashing hook block instead of pass.
- **Gateway hooks** (`~/.hermes/hooks/<name>/HOOK.yaml` + `handler.py`) load **just by being placed there** — no
  enable list. Treat that folder like `/etc/sudoers.d`.
- **Outbound webhooks** (`hooks.outbound`) push events out, signed.
- **Middleware** (plugins only) rewrites provider kwargs or tool args; observer hooks are read-only.

## When changes take effect

- New/removed skills show up as a note on your next message; the system prompt updates at the next compaction.
- Plugin tools and prompt sections enabled mid-session apply from the next session; disabled handlers stay wired until restart.
- Adding/removing tools mid-session invalidates the prompt cache.
- MCP: `/reload-mcp`, or the gateway picks up `config.yaml` changes within ~1 minute.
