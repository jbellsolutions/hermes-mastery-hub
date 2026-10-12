# 15 — Profiles Deep Dive (one machine, many agents)

*Source: `website/docs/user-guide/profiles.md`, `reference/profile-commands.md`, `user-guide/multi-profile-gateways.md`,
`user-guide/profile-distributions.md`, `user-guide/which-file-does-what.md`, `user-guide/managed-scope.md`,
`user-guide/multi-connection-desktop.md`; commands confirmed in `hermes_cli/subcommands/profile.py` (main, 2026-10-12).*

## The mental model

**A profile is a complete, separate Hermes home.** The default profile is `~/.hermes`. Named profiles live at
`~/.hermes/profiles/<name>/`. Hermes sets `HERMES_HOME` to that folder and everything follows it.

Think of a profile as **one employee**: its own identity (SOUL.md), memory, skills, schedule, credentials and chat
identity. The *code* is shared; the *person* is not.

A directory only counts as a profile if it holds at least one identity file: `config.yaml`, `.env`, `SOUL.md`,
`profile.yaml`, `auth.json` or `state.db`.

## What is isolated vs shared

| Isolated per profile | Shared across profiles |
|---|---|
| `config.yaml` (model, toolsets, approvals, MCP servers) | Hermes code + Python environment (`hermes update` runs once) |
| `.env` (API keys, **bot tokens**) | Bundled skills (synced into every profile on update) |
| `SOUL.md` (identity, slot #1 of the system prompt) | **OAuth logins** — Anthropic Pro/Max, Codex, xAI live in root `~/.hermes/auth.json` |
| `memories/MEMORY.md`, `USER.md`, memory provider config | Your OS `HOME` — so `git`, `ssh`, `gh`, Claude/Codex CLI credentials are shared unless you set `terminal.home_mode: profile` |
| Sessions + `state.db` | Plugin Python dependencies (one bad config can break dependency prep for all) |
| Skills | Filesystem access — **a profile is not a sandbox** |
| `cron/` jobs, logs, gateway state | Managed scope (`/etc/hermes/`) pins keys for every profile on the box |
| `plugins/`, `profile.yaml` (description, display name) | |
| MCP connections (`auth: oauth` MCP keeps its own `mcp-tokens/`) | |

## Commands you will actually use

```bash
# create
hermes profile create sales --clone            # copy config, .env, SOUL, skills, memory files, memory-provider config
hermes profile create sales --clone-from ops   # clone from a specific profile
hermes profile create sales --clone-all        # everything except history, cron and bot channels
hermes profile create sales --clone --clone-channels   # ALSO copy bot tokens/allowlists (collision risk; needs a clone flag)
hermes profile create sales --description "Inbound sales SDR"

# use
hermes profile list | show <name>
hermes profile use sales          # sticky default (careful — see pitfalls)
hermes -p sales                   # one session as that profile
hermes -p sales gateway stop      # "park" just that profile in the host gateway
hermes profile describe sales --text "..."   # description the kanban orchestrator reads

# lifecycle
hermes profile rename old new     # removes the old service; identity is migrated
hermes profile alias sales --name s
hermes profile delete sales -y
hermes profile export sales -o sales.tar.gz   # one-off move (INCLUDES memories and sessions)
hermes profile import sales.tar.gz --name sales2

# distributions (templates)
hermes profile install <git-url|local-dir> --name client-acme --alias
hermes profile update client-acme             # re-pull; user data preserved
hermes profile info client-acme

# migrate an existing setup in
hermes import-agent claude-code --dry-run
```

Naming rule: `^[a-z0-9][a-z0-9_-]{0,63}$` — lowercase, digits, `-`, `_`. Reserved: `hermes`, `default`, `test`,
`tmp`, `root`, `sudo`. A Unicode `display_name` can be set separately for how the bot appears.

## Gateways: one per machine, serving every profile

Since v0.19–0.21, **one host gateway multiplexes all profiles** (`gateway.multiplex_profiles: true`, and `false` is
retired). This replaces the old "one gateway service per profile" pattern.

- Each profile still needs **its own bot token per platform**. A duplicate token is parked as `duplicate_credential`.
- New profiles are picked up live; the host rescans `profiles/` every 30 seconds.
- `hermes -p <name> gateway stop|start` parks/unparks that profile inside the host.
- HTTP-inbound platforms share one listener under `/p/<profile>/…` (e.g. `/p/coder/webhooks/<route>`). Secondary
  profiles must not enable their own `api_server`/`webhook`.
- **One shared bot, many profiles:** `gateway.profile_routes` matches on `user_id`, `thread_id`, `chat_id`, `guild_id`.
  Matches are scored by adding weights (user 16, thread 8, chat 4, guild 2); the highest score wins. Useful when one Slack/Discord bot fronts a whole team.
- Named-profile `gateway install/start/run` refuses (exit 78) unless `gateway.standalone: true` (temporary shim) or
  `--force` (fleets split across UNIX users).
- Service names: launchd `ai.hermes.gateway.plist`, systemd `hermes-gateway.service` (host); Docker uses s6 slots.
- Upgrading an old per-profile fleet: `hermes update` folds it automatically; rollback data in
  `~/.hermes/gateway_migration.json`; opt out with `gateway.auto_multiplex_migration: false`.

> **Known limitation:** inside the multiplexed gateway, MCP discovery, the terminal environment and the built-in tool
> registry are still process-global (`developer-guide/multiplexing-gateway.md`). Secrets fail closed
> (`UnscopedSecretError`), but don't treat co-hosted profiles as a security boundary between clients.

## Profile distributions — your "pod" template mechanism

A **distribution** is a git repo containing a profile's shareable parts. This is how you templatize an agent (or a
whole client pod of agents) and stamp out copies.

```
my-sdr-agent/
├── distribution.yaml      # name (required), version, description, hermes_requires, env_requires[...], author, license
├── SOUL.md
├── config.yaml
├── mcp.json
├── skills/
├── cron/jobs.json
└── README.md
```

```yaml
# distribution.yaml
name: sdr-agent
version: 1.0.0
description: Inbound SDR — qualifies leads, books calls, drafts follow-ups
hermes_requires: ">=0.21.6"
env_requires:
  - name: SLACK_BOT_TOKEN
    description: Bot token for this client's Slack app
    required: true
  - name: OPENROUTER_API_KEY
    required: true
```

- `hermes profile install <repo> --name client-acme` previews the manifest, checks required env vars, writes
  `.env.EXAMPLE`, strips `.git/`. Private repos work over your normal git auth. **Test from a local directory first.**
- `hermes profile update client-acme` overwrites SOUL.md and mcp.json, merges skills per skill and cron per job id
  (new jobs arrive **paused**), keeps `config.yaml` unless `--force-config`, never touches memory/sessions/auth/.env.
- The installer **never ships** `auth.json`, `.env`, `memories/`, `sessions/`, `state.db*`, `logs/`, `workspace/`,
  `plans/`, `home/`, caches, `local/`.
- Not yet available: pinning to a git ref, signing, lockfiles, `update --dry-run`.

**Export vs distribution:** `profile export` makes a tar.gz that *includes memories and sessions* (credentials are
filtered by filename only). Use it to move *your* agent. Use a distribution to hand a *template* to someone else.

## Profiles across machines

- **Desktop multi-connection** (Settings → Gateways): one app controls Local, Remote (`hermes serve` URL + token/OAuth),
  SSH, and Hermes Cloud connections. A profile name that exists on several machines shows as `@name-device`.
  "Update all instances" fans out updates. Tokens are stored 0600 in `connections.json`.
- Mac mini fleet: `hermes serve --host <tailscale-ip> --port 9119` on each box, add each as a Remote connection.

## Pitfalls (from the docs)

1. **Never run two agent processes on one profile** — memory writes compound each other.
2. `--clone` does **not** copy bot channels. Adding `--clone-channels` to a clone flag does, and risks token collisions (`profile list` warns).
3. OAuth refresh tokens are single-use, so clones share the root login. Give a profile its own: `hermes -p x auth add anthropic`.
4. `terminal.cwd: "."` means the *launch* directory, not the profile directory. Set it explicitly.
5. Deleting the sticky profile's folder by hand blocks commands until `hermes profile use default`.
6. `--clone-all` skips cron so jobs don't fire twice.
7. Exports can leak personal content in memories/sessions/USER.md — run `tar -tzf` before sending.
8. Distributions: add `.gitignore` **before** `git init`. Git does not strip secrets; the installer only refuses known paths.
9. `rename` removes the old gateway service — reinstall under the new name if you run standalone.
10. Webhook subscriptions under multiplexing use `--route-profile`, not `-p`.
11. Every directory under `profiles/` gets served by the host gateway — archive dead profiles.
12. A cloned `local_embedded` memory setup can still share the source's daemon and memory bank.

## When to make a new profile (vs. a new skill)

| Make a **skill** when… | Make a **profile** when… |
|---|---|
| It's a procedure the same agent should know | It needs its own credentials or bot identity |
| It shares the agent's memory and tone | Its memory must not mix with another role's |
| It runs inside existing schedules | It has its own cron schedule and reports |
| You're a solo operator with one agent | You're serving a client, or the role is a distinct "employee" |

Rule of thumb from experience: **one role per agent.** An agent juggling five jobs gets confused. Skills give one agent
more *abilities*; profiles give you more *employees*.
