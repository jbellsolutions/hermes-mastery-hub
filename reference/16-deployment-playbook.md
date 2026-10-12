# 16 — Deployment Playbook (laptop → VPS → cloud computer → enterprise)

*Source: `getting-started/installation.md`, `updating.md`, `user-guide/docker.md`, `desktop.md`, `multi-connection-desktop.md`,
`features/bot-screen.md`, `features/computer-use.md`, `egress/*.md`, `secrets/*.md`, `features/credential-vault.md`,
`security.md`, `managed-scope.md`, `guides/secure-hermes-on-a-work-machine.md`, `guides/manage-hermes-cloud-with-mcp.md`,
`developer-guide/stable-releases.md`, `developer-guide/gateway-monitoring.md` (main, 2026-10-12). Commands confirmed in
`hermes_cli/subcommands/`.*

## Pick a target

| Target | Best for | Install | Runs the gateway as | Updates |
|---|---|---|---|---|
| **Laptop + Desktop app** | Personal agent, building and testing | Signed DMG (Apple Silicon) / Windows MSIX, or `curl -fsSL https://hermes-agent.nousresearch.com/install.sh \| bash` then `hermes desktop` | `hermes gateway install` → launchd / systemd user / Windows Scheduled Task | Desktop update control, or `hermes update` |
| **VPS (systemd)** | Always-on business agents you reach from Slack/Telegram | Run the installer **as the service user**, then `hermes doctor` | `hermes gateway install` + `sudo loginctl enable-linger $USER` (preferred) or `sudo hermes gateway install --system` | `hermes update` (pauses + restarts gateways) |
| **Docker / compose** | Repeatable client deploys, many profiles per container | `docker run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup` | s6 inside the container (one slot per profile) | Pull + recreate. **Never `hermes update` inside a container** (exit 2) |
| **Cloud computer w/ GUI** (Orgo-style, or a VPS with a desktop) | Agents that need a real browser, logins, computer use | Same as VPS; add **Bot Screen**: `hermes computer-use screen install` / `start` | Same as VPS | Same as VPS |
| **Hermes Cloud** | Zero-ops hosted agents | portal.nousresearch.com/cloud | Platform-managed | Platform-managed |
| **Remote terminal backends** | Keep the gateway on one box, run commands elsewhere | `terminal.backend: docker \| modal \| daytona \| ssh \| vercel_sandbox` | Gateway stays on host | — |
| **Work machine / enterprise** | Company laptops, regulated teams | Normal install + **managed scope** (`/etc/hermes/config.yaml` + `.env`) + hardening guide | Same | Pinned channel + backups |
| **Mac mini fleet** | Your own hardware cluster | Installer per box; `hermes serve --host <tailscale-ip> --port 9119` | launchd; keep awake with `caffeinate -dis` | Desktop "Update all instances" or `hermes update --plan` |

Sizing (Docker docs): minimum 1 GB RAM, 2–4 GB recommended, at least 2 GB with a browser. Bot Screen: ~1.1–1.5 GB per
open screen with a browser; 8 GB is comfortable. Idle screens stop after 30 minutes.

## The standard VPS deploy (what you'll repeat for clients)

```bash
# 1. as the service user, not root
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc
hermes doctor

# 2. model + identity
hermes model                       # or: hermes setup
$EDITOR ~/.hermes/SOUL.md

# 3. profiles (one per role/employee)
hermes profile create ops --description "Operations lead"
hermes -p ops model

# 4. one gateway for all profiles
hermes gateway install
sudo loginctl enable-linger $USER
hermes gateway status
journalctl --user -u hermes-gateway -f

# 5. lock it down (see Security below), then back it up
hermes backup
```

Each profile that talks on Slack/Telegram needs **its own bot token** in its own `.env`. For Slack: subscribe
`message.channels`/`message.groups`, turn on the Messages tab, and **reinstall the app after any scope change**.
After Hermes updates, `hermes slack manifest --write` regenerates the manifest to paste in.

## Release channels, updates, rollback

- **Stable is the default** for source installs since v0.21.7 — you get the latest *published release tag*, not every
  commit. Follow main with `hermes update --set-channel main` (or `--channel main` for one run).
- **Canary** is a separate Desktop app (`hermes-canary`) — it **shares the same Hermes home**, so only run it against a backup.
- Safe update routine:
  ```bash
  hermes update --check          # what would change
  hermes update --plan           # read-only fleet inventory
  hermes config set updates.pre_update_backup full
  hermes update
  hermes doctor && hermes --version && hermes gateway status
  hermes config check            # then: hermes config migrate (if prompted)
  ```
- Receipts: `~/.hermes/logs/update_receipts/latest.json`.
- **Rollback = restore a backup.** Checking out older code does not roll back data — newer releases migrate config and DBs.
  Docker: pin a digest. Nix: `nix profile rollback`.
- If an update is triggered from *inside* the gateway (a cron job), use `--no-gateway-restart` and restart separately.

## Security for business and enterprise

**Approvals** (`approvals.*`)
- `mode: smart` (default — aux-LLM reviews risky commands), `manual` (always ask), `off` (= YOLO).
- `cron_mode`, `single_query_mode`, `unattended_mode` default to `deny` — unattended runs refuse dangerous commands
  instead of hanging. Approval prompts time out after 300s → denied.
- `approvals.deny: [...]` globs block commands **even in YOLO**. Add force-push, pipe-to-shell, `rm -rf` patterns.
- `hermes approvals suggest` turns your approval history into allowlist proposals.
- `skills.write_approval` / `memory.write_approval` stage the agent's self-edits for review.

**Secrets**
- Settings in `config.yaml`, secrets in `.env` (0600). Never the reverse.
- Secret sources: `hermes secrets onepassword setup` (service account token for headless),
  `hermes secrets bitwarden setup` (`BWS_ACCESS_TOKEN`), or `secrets.command` (helper, 3s timeout).
- **Credential vault** ("Passwords & Logins"): Fernet-encrypted at `~/.hermes/vault/`, bound to a site origin and filled
  over the browser protocol so the **model never sees the password**; generates TOTP codes. Card fills always need
  approval; headless sessions are refused.
- Redaction: `security.redact_secrets: true` (default); `privacy.redact_pii` for WhatsApp/Signal/Telegram.

**Network**
- SSRF guard on by default; `security.website_blocklist` for browsing.
- **iron-proxy** (`hermes egress install|setup|start|status`): the sandbox only holds opaque tokens, the host proxy swaps
  in real keys and denies unknown hosts. Docker backend only today; does not stop exfiltration to an allowed host.

**Who can talk to the agent**
- Gateways are deny-all by default. Use `*_ALLOWED_USERS` / `GATEWAY_ALLOWED_USERS` or DM pairing
  (`hermes pairing approve <platform> <code>`). Admin vs user tiers: `allow_admin_from`, `user_allowed_commands`.
  Slack: `allowed_channels`. Check yourself with `/whoami`.

**Enterprise lock-down**
- **Managed scope:** root-owned `/etc/hermes/config.yaml` + `.env` pin keys users can't override (even via shell env).
  It's v1: enforced by file permissions only, Linux-first, and the managed `.env` is world-readable — don't put secrets there.
- Pin `HERMES_MANAGED_DIR` in the unit/image so users can't repoint it.
- `plugins.isolation: host` runs third-party plugins out of process; lockable via managed scope.
- `hermes security audit` checks dependencies against OSV.dev.
- API server: `API_SERVER_KEY` is required even on loopback. Dashboards bound off-loopback always require auth.

## Operations

| Job | Command |
|---|---|
| Health | `hermes gateway status` (shows Degraded / stale heartbeat), `hermes doctor` |
| Logs | `hermes logs`, `~/.hermes/logs/` (gateway.log, errors.log, update.log) |
| Monitoring | `monitoring.gateway_health_export.enabled` + OTLP; alert on `absent_over_time(hermes_gateway_up[5m])` |
| Backups | `hermes backup` (safe while running; exits 1 if incomplete — alert on it). Restore: `hermes import` |
| Cron health | `hermes cron doctor`; global stop: `hermes pause` |
| state.db trouble | stop **all** Hermes processes → `hermes doctor` until no holders → `hermes sessions repair --check-only` |
| Checkpoints | `checkpoints.enabled: true`, then `/rollback`, `/rollback diff N` |
| Stack dump | `kill -USR2 <gateway-pid>` → `logs/gateway_faulthandler.log` |

## Cost guardrails (do these before you walk away)

- `agent.max_turns` is unlimited by default on current main — set a cap, or `agent.run_budget_seconds`.
- Cap fan-out: `delegation.max_concurrent_children` (default 10), `kanban.max_in_progress_per_profile`.
- Use a cheap worker model: `delegation.model`, per-card `--model` in kanban, per-job model in cron.
- Every `/loop` tick, heartbeat and bot-chat delivery is a **full agent turn** — price them.
- Fallbacks and key rotation reset the prompt cache.
