# 14 — What's New Since the v1.0 Snapshot (v0.18.0 → v0.21.7)

*Source: GitHub release pages for every tag from v2026.7.7 to v0.21.7, `hermes_cli/config_migrations.py`,
and a docs diff of `website/docs/` between tag `v2026.7.1` and `main` on 2026-10-12. See `../SOURCE_LOCK.md`.*

The v1.0 hub was built against **Hermes Agent v0.18.0 (2026.7.1)**. By 2026-10-12 the current stable
release is **v0.21.7** (published Oct 11–12). Nineteen releases shipped in between. Read this file first
if you used the v1.0 hub — several of its defaults are now wrong.

## Release timeline

| Tag | Date | Version | Headlines |
|---|---|---|---|
| v2026.7.7 / .7.2 | Jul 7 | 0.18.1–0.18.2 | Big fix rollup; Windows installer self-heal; WhatsApp bridge pinned |
| v2026.7.20 | Jul 20 | 0.19.0 "Quicksilver" | ~80% faster time-to-first-token; **smart approvals on by default**; Bitwarden/1Password secret sources; **multi-profile gateway**; Hermes Cloud mode in Desktop |
| v2026.7.30 | Jul 30 | 0.19.1 | Infra rollup; Buzz/Nostr channel |
| v2026.8.3 | Aug 3 | 0.20.0 "Herald" | Streaming voice + wake words; **A2A v1.0**; signed outbound webhooks; Desktop plugin SDK (Kanban is plugin #1); `hermes import-agent`; **Anthropic prompt caching now covers tool schemas** |
| v2026.8.13 | Aug 13 | 0.20.1 | Stabilization |
| v2026.8.16 / .16.2 | Aug 16 | 0.20.2–0.20.3 | Desktop multi-gateway Connections; `/loop`; MCP 2.x SDK; **Bot Mode plugin** bundled; plugin-install security scan |
| v2026.8.18 / .19 | Aug 18–19 | 0.20.4–0.20.5 | Skill-install scan; kanban notifications; cron jobs get memory + per-job reasoning effort; `hermes worktree` |
| v2026.8.27 | Aug 27 | 0.20.6 | 50+ remote MCP catalog servers; OS-keychain secret encryption; updates pause gateways instead of killing them |
| v2026.8.31 | Aug 31 | 0.21.0 "Pantheon" | **Bot Mode on by default**; **`hermes peer`**; live subagent steer/stop + JSON-schema output; protected writes to AGENTS.md/skills/memory |
| v2026.9.7 | Sep 7 | 0.21.1 | Modularization (internal import paths moved) |
| v2026.9.11 | Sep 11 | 0.21.2 | Fixes state.db corruption from 0.21.0; cross-profile secret-leak fixes; **credential vault**; **curated SHA-pinned plugin catalog**; Nous free tier |
| v2026.9.14 | Sep 14 | 0.21.3 | Read-only state.db readers; Agent Sessions API; MCP OAuth bound to issuer |
| v2026.9.21 | Sep 21 | 0.21.4 | Host-wide gateway singleton lock; `skills.auto_load`; `mcp.discovery_concurrency` |
| v2026.9.24 | Sep 24 | 0.21.5 | Connectors page replaces MCP tab; per-profile gateway park/start; kanban redesign; **Claude Opus 5.5** in catalog |
| **v0.21.6** | Oct 8 | 0.21.6 | First semver/stable-pipeline release; `plugins.isolation: host`; **Claude Sonnet 5.5 / Haiku 5.5**; dashboard-auth fixes |
| **v0.21.7** | Oct 11 | 0.21.7 | **Source installs now update to tagged releases, not every commit on main** |

> **Tag scheme changed.** Releases after v2026.9.24 are `vX.Y.Z`. Any script that watches for `v2026.*`
> tags will miss them. Full curated notes (breaking changes, credits) are deferred to **v0.22.0**.

## Defaults the v1.0 hub got wrong (now)

| Setting | v1.0 hub said | Now |
|---|---|---|
| `approvals.mode` | `manual` is the default | **`smart`** is the default (aux-LLM reviewer); `cron_mode`, `single_query_mode`, `unattended_mode` all default to **`deny`** |
| `delegation.max_concurrent_children` | 3 | **10** (no hard ceiling) |
| `delegation.max_iterations` | 50 | **250** |
| Delegation behavior | blocking unless background | **async by default** — returns a handle, result arrives as a new turn |
| `agent.max_turns` | 90 | **`null` = unlimited** on current `main` (release notes for 0.21.x mention a 500 limit; the source default is now unlimited). Set a cap or `agent.run_budget_seconds` yourself for unattended agents |
| Curator | stale 30d / archive 90d | **stale 14d / archive 30d**; `prune_builtins` false |
| Profile names | "lowercase alphanumeric only" | `^[a-z0-9][a-z0-9_-]{0,63}$` — **hyphens and underscores are allowed** (this was already true in July) |
| Install | PyPI + Homebrew + shell | PyPI/Homebrew **retired**; shell installer, Desktop bundles, Docker, Nix, Termux |
| Gateway per profile | one `gateway install` per profile | **one multiplexed gateway per machine serves every profile**; named-profile `gateway install` exits 78 |
| Security scanner | `security.tirith_enabled` | Tirith **removed**; plugin/skill install scans + iron-proxy instead |
| Writes to AGENTS.md, skills, memory | ungated | **always require approval** |

## Breaking config changes (auto-migrated on `hermes update`)

Config schema went from version 32 → 50. Migrations run automatically; check with `hermes config check`.

- `delegation.max_async_children` → folded into `max_concurrent_children`
- `gateway.multiplex_profile_allowlist` → **removed**: the host gateway serves *every* directory under `profiles/`. Archive profiles you don't want served.
- MCP `disabled: true` → `enabled: false`
- `model_catalog.ttl_hours` → `ttl_minutes`
- `display.tool_progress_overrides` → `display.platforms`
- list-form `custom_providers` → ignored until converted to `providers: {}`
- `security.tirith_*`, `cron.model_drift_guard` → removed
- "Messaging other agents" section stripped from SOUL.md (migration v41)
- Tool renames: `todo` → `todo_list`, `process` → `process_manage`, `cronjob` → `cronjob_manage`
- Docs: `guides/build-a-hermes-plugin` removed → `developer-guide/plugins`
- Skills: 5 GitHub skills merged into `software-development/github`; many creative/mlops skills moved to optional

If you ran 0.21.0/0.21.1 and see session errors: `hermes doctor`, then `hermes sessions recover --inspect-only`.

## The ten capabilities that matter most for business agent teams

1. **Bot Mode** (default on) — each profile is a named bot with its own role, model, memory, skills; group rooms of 2–6 bots. → `17-teams-and-orchestration.md`
2. **Multiplexing gateway** — one host gateway, per-profile adapters, `profile_routes`, park/start per profile. → `15-profiles-deep-dive.md`
3. **Profile distributions** — a profile packaged as a git repo, installed per client. → `15`
4. **Kanban** — durable, cross-profile work queue with review, notify+wake, per-card model. → `17`
5. **`hermes peer` + A2A** — agent-to-agent across machines. → `17`
6. **Cron with memory/continuity**, `/loop`, `/heartbeat`, `/goal`. → `17`
7. **Plugin catalog** (570 SHA-pinned entries) + per-profile plugin host isolation. → `18-plugins-mcp-and-extension-map.md`
8. **Credential vault + secret sources** (1Password, Bitwarden, command). → `16-deployment-playbook.md`
9. **Desktop multi-connection + Hermes Cloud** — one app controls local, SSH, remote and Cloud agents. → `16`
10. **Security hardening** — deny rules that apply even in YOLO, iron-proxy egress, protected instruction files. → `19-edge-cases-dos-and-donts.md`

## For Anthropic-model operators specifically

- Native Anthropic prompt caching covers tool schemas (v0.20.0). Mid-session `/model` switches, fallbacks and key rotation **reset the cache** — on long sessions it can be cheaper to start fresh on the right model.
- Claude Opus 5.5 (v0.21.5) and Sonnet 5.5 / Haiku 5.5 (v0.21.6) are in the model catalog.
- Signed reasoning replays on `--resume` (v0.21.7) fix resumed sessions on Bedrock.
- `hermes import-agent claude-code` migrates an existing Claude Code setup (skills, instructions).
- Anthropic Pro/Max OAuth logins live in the **root** `~/.hermes/auth.json` and are shared across profiles; give a profile its own login with `hermes -p <name> auth add <provider>`.
- Official-tier catalog plugin `claude-subscription-directsdk` runs Hermes turns on a Pro/Max subscription through the Claude Code CLI. It is labeled **experimental**; check Anthropic's current terms before using it for business workloads.
- Delegation docs recommend a strong planner with cheaper workers: pin `delegation.model` (global), or set a model per kanban card with `--model` / `set-model`.

## Ecosystem (as of 2026-10-12)

- **Hermes Cloud** (portal.nousresearch.com/cloud): hosted always-on agents billed per second from Nous credit. Manage it conversationally with the Portal MCP server (`guides/manage-hermes-cloud-with-mcp.md`).
- **Nous Portal** has a free tier (`/login`) and paid plans with credits. Prices change; check the portal.
- Nous announced a Series B in early October 2026 aimed at "Hermes for Business" (enterprise controls). Treat roadmap claims as unverified until they ship in a release.
