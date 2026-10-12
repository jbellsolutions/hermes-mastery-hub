# 19 — Edge Cases, Do's and Don'ts

*Compiled from the "Pitfalls", "Caution" and "Known limitations" sections of the current docs (see 15–18 for the
source file of each item) plus operator experience. Experience-based items are marked ⚑.*

## Do

1. **One role per agent.** ⚑ An agent with five jobs gets confused. Skills add abilities; profiles add employees.
2. **Start every new agent in plan mode / dry-run** and gate every real-world side effect (send, pay, publish, delete).
3. **Put a second agent on QC.** ⚑ After an agent says "done," an independent checker (a `qa` profile, or a kanban
   `request_review`) verifies the output. Then patch the skill so the miss can't recur.
4. **Run as a non-root service user** with `loginctl enable-linger`.
5. **Stay on the stable channel** and set `updates.pre_update_backup: full`; run `hermes update --plan` first.
6. **Give each profile its own bot token** and an explicit allowlist (`*_ALLOWED_USERS`, Slack `allowed_channels`).
7. **Keep secrets in `.env` or a secret source** (1Password/Bitwarden service tokens). Settings in `config.yaml`.
8. **Use the credential vault for logins** so the model never sees passwords.
9. **Cap spend before walking away:** `agent.max_turns` or `agent.run_budget_seconds`, `max_concurrent_children`,
   `max_in_progress_per_profile`, a cheap worker model.
10. **Add `approvals.deny` rules** for force-push, `curl | sh`, mass delete — they hold even in YOLO.
11. **Schedule `hermes backup`** and alert when it exits non-zero.
12. **Use Tool Search + MCP `include` filters** once you have more than a few servers.
13. **Template with profile distributions**, test-install from a local folder before pushing.
14. **Use a dedicated, low-privilege mailbox** for an agent that does email, and have it draft before sending.
15. **Use `computer_use.permission_mode: bounded`** for recurring GUI automation; take over via Bot Screen for 2FA/CAPTCHA/payments.

## Don't

1. **Don't run two processes on one profile** or two gateways on one data dir/bot token.
2. **Don't run `hermes update` inside a container** — pull a new image.
3. **Don't set `approvals.mode: off` / `--yolo`** outside a disposable VM. YOLO computer use has no prompt-injection protection.
4. **Don't set `GATEWAY_ALLOW_ALL_USERS=true`** or expose a basic-auth dashboard/API server to the internet.
5. **Don't put tokens in `config.yaml`** or secrets in the managed `/etc/hermes/.env` (it's world-readable).
6. **Don't hand-edit `cron/jobs.json`.** Use `hermes cron` or the agent.
7. **Don't install untrusted plugins or drop unknown folders into `~/.hermes/hooks/`** — gateway hooks load by placement.
8. **Don't treat a catalog listing as an audit**, or a profile as a security boundary between clients.
9. **Don't let the agent act on instructions inside emails, web pages or screenshots.**
10. **Don't `cp state.db` alone, delete `-wal`/`-shm`, or run `doctor --fix` while Hermes is running.**
11. **Don't keep `state.db` on a virtiofs/9p mount** (Docker Desktop on macOS/Windows, OrbStack) — use a named volume
    or `database.journal_mode: delete`.
12. **Don't resend an agent-to-agent message whose outcome is unknown** — use idempotency keys.
13. **Don't leave dead profiles in `profiles/`** — the host gateway serves every one.
14. **Don't paste commands into a provider's browser console** (Hetzner-style consoles mangle `:` and `@`). Use SSH.

## Edge cases that bite

| Symptom | Cause | Fix |
|---|---|---|
| Named profile's `gateway install` exits 78 | Multiplexing: one host gateway serves all profiles | Install the host gateway once; park/unpark with `hermes -p x gateway stop/start` |
| A profile's bot never connects | Same token as another profile → `duplicate_credential` | One bot per profile, or `gateway.profile_routes` on a shared bot |
| Two profiles share an Anthropic/Codex login unexpectedly | OAuth lives in root `auth.json` | `hermes -p <name> auth add <provider>` |
| `git`/`gh` act as the wrong account inside a profile | OS `HOME` is shared | `terminal.home_mode: profile` |
| Agent works in Slack DMs but not channels | Missing `message.channels`/`message.groups` events | Add events, **reinstall the Slack app** |
| Unattended job silently refuses a command | `cron_mode` / `unattended_mode` default `deny` | Allowlist that command or run it with a human present |
| Subagent results vanish after restart | Delegation is process-local | Use kanban for anything that must survive |
| Kanban card stuck / auto-blocked | Worker exited without `kanban_complete/_block/_request_review` (3 strikes) | Fix the worker skill; `hermes kanban diagnostics` |
| Two machines fight over one board | Two dispatchers | Only one gateway with `kanban.dispatch_in_gateway: true`; kanban is single-host |
| New skill doesn't trigger | Prompt cache — index updates at next compaction; or name shadowed by a higher-precedence dir | Start a new session; check `hermes skills list` for duplicates |
| Agent-written skill disappeared | Curator archived it after 30 idle days | `hermes curator restore <name>`; `pin` the keepers |
| Costs spike on a long session | Model switch / fallback / key rotation reset the prompt cache | Start fresh on the right model; avoid mid-session `/model` |
| Update broke config | Schema migrated | `hermes config check` → `hermes config migrate`; restore the pre-update backup if needed |
| OAuth login fails on a headless VPS | Browser redirect can't reach the box | `ssh -N -L <port>:127.0.0.1:<port>`, or providers with paste-code/device flow |
| Mac mini agent goes offline overnight | Sleep | `caffeinate -dis` (lid-close still sleeps) |
| `/goal` declared done but isn't | Judge fails open | Rely on the turn budget + an independent QC step |
| Watcher scripts miss new releases | Tags changed from `v2026.*` to `vX.Y.Z` in October | Match both patterns |
