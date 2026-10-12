# 03 — Configuration, Profiles & Security Toggles

*Source: installed `hermes-agent` skill v2.3.0. Verify with `hermes config`, `hermes config edit`, and the [Configuration docs](https://hermes-agent.nousresearch.com/docs/user-guide/configuration).*

## Editing config

```bash
hermes config                 # view current config
hermes config edit            # open config.yaml in $EDITOR
hermes config set KEY VALUE   # set one value, e.g. hermes config set approvals.mode smart
hermes config check           # find missing/outdated options
hermes config migrate         # add new options after an update
```

## Config sections you will actually touch

| Section | Useful keys |
|---|---|
| `model` | `default`, `provider`, `base_url`, `api_key`, `context_length` |
| `agent` | `max_turns` (now `null` = unlimited by default — set a cap for unattended agents), `run_budget_seconds`, `tool_use_enforcement` |
| `terminal` | `backend` (local/docker/ssh/modal/daytona/singularity), `cwd`, `timeout` (180) |
| `compression` | `enabled`, `threshold` (0.50), `target_ratio` (0.20) |
| `display` | `skin`, `interface` (cli/tui), `show_reasoning`, `show_cost`, `language` |
| `stt` / `tts` | voice in/out providers |
| `memory` | `memory_enabled`, `user_profile_enabled`, `provider` |
| `security` | `redact_secrets`, `website_blocklist`, `allow_private_urls` (Tirith was removed) |
| `delegation` | `model`, `provider`, `max_iterations` (250), `max_concurrent_children` (10), `max_spawn_depth` (1) |
| `curator` | `enabled`, `consolidate` (off by default), `interval_hours` (168), `stale_after_days` (14), `archive_after_days` (30) |
| `approvals` | `mode` (smart default / manual / off), `cron_mode`, `single_query_mode`, `unattended_mode` (all `deny`), `deny` globs, `timeout` (300) |
| `gateway` | `multiplex_profiles` (on), `profile_routes`, `bot_loop_guard` |
| `skills` | `external_dirs`, `create_dir`, `write_approval` |
| `plugins` | `enabled`, `isolation` |

## Profiles — run several isolated agents on one machine

Each profile has its own config, sessions, skills, and memory under `~/.hermes/profiles/<name>/`.

```bash
hermes profile list
hermes profile create acme --clone        # new profile, clone current config
hermes profile use acme                    # sticky default
hermes profile show acme
hermes -p acme                             # run one session under a profile
hermes profile export acme                 # tar.gz for backup/migration
```

This is the mechanism behind a multi-agent team. Profile names follow
`^[a-z0-9][a-z0-9_-]{0,63}$` — lowercase letters, digits, hyphens and underscores (so `hermes-cmo` **is** valid;
the v1.0 hub was wrong about this). Reserved: `hermes`, `default`, `test`, `tmp`, `root`, `sudo`.

> **Changed since v1.0:** one multiplexed gateway per machine now serves every profile; profiles can be packaged as
> git-repo **distributions** and installed per client; clones no longer copy bot tokens unless `--clone-channels`.
> Full guide: `15-profiles-deep-dive.md`.

## Security toggles that matter for a business agent

**Command approval** — the single most important guardrail for money/side-effects:
```bash
hermes config set approvals.mode smart    # DEFAULT since v0.19.0: aux-LLM reviews risky commands
hermes config set approvals.mode manual   # always prompt (safest; recommended on work machines)
hermes config set approvals.mode off      # bypass everything (== --yolo; NOT for business use)
```
Per-invocation bypass: `hermes --yolo`. `approvals.deny` globs still block in YOLO, and unattended
surfaces (cron, single-query, unattended platforms) default to **deny**. **Keep approvals ON for any agent that can spend
money or send outbound messages.** The Mom-n-Pop team's explicit rule: *"annoying-but-safe
beats fast-but-loose."*

**Secret redaction** (on by default) — scrubs API-key-like strings out of tool output before
it enters context/logs. Snapshotted at startup; change it from a terminal then restart:
```bash
hermes config set security.redact_secrets true
```

**PII redaction in gateway** — hashes user IDs, strips phone numbers before they reach the model:
```bash
hermes config set privacy.redact_pii true
```

## When changes take effect

- **Tools/skills:** next session (`/reset`) — not mid-conversation (protects prompt caching).
- **Config values:** gateway → `/restart`; CLI → exit and relaunch.
- **MCP servers:** restart the agent (or `/reload-mcp`).

`hermes --ignore-rules` starts a session with **no** project context files, SOUL.md, user
config, plugins, or MCP — the clean-room switch for "is it my setup or Hermes itself?"
