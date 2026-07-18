# 12 — Troubleshooting

*Source: installed `hermes-agent` skill v2.3.0 (Troubleshooting section) + Mom-n-Pop / Control Room verified pitfalls. Always check `hermes doctor` first.*

## First moves

```bash
hermes doctor            # dependencies + config; add --fix to auto-repair
hermes status --all      # component status
grep -i "error\|failed to send" ~/.hermes/logs/gateway.log | tail -20
hermes --ignore-rules    # clean-room session: is it your setup or Hermes itself?
```

## Model / provider

- `hermes doctor` for config/deps; `hermes auth` (or `hermes auth add <provider>`) to re-auth OAuth.
- Check `.env` has the right key.
- **GitHub Copilot 403:** `gh auth login` tokens do NOT work for Copilot API — use the
  Copilot device-code flow via `hermes model` → GitHub Copilot.
- **Auxiliary tasks fail silently** (vision/compression/session_search): the `auto` provider
  can't find a backend. Set `OPENROUTER_API_KEY` or `GOOGLE_API_KEY`, or configure each
  auxiliary task's provider explicitly.

## Changes not taking effect

- **Tools/skills:** `/reset` (new session) — never mid-conversation (protects prompt caching).
- **Config:** gateway → `/restart`; CLI → exit and relaunch.
- **MCP:** restart the agent or `/reload-mcp`.
- **Code changes:** restart the process.

## Skills not showing

`hermes skills list` (installed?) → `hermes skills config` (platform-enabled?) → load
explicitly `/skill name` or `hermes -s name` → `/reload-skills` after adding files manually.

## Gateway

- Silent on Discord → enable **Message Content Intent**.
- Slack ignores channels → subscribe to `message.channels`.
- Dies on SSH logout → `sudo loginctl enable-linger $USER`.
- Dies on WSL2 close → `systemd=true` in `/etc/wsl.conf`.
- Crash loop → `systemctl --user reset-failed hermes-gateway`.

## Business-workflow pitfalls (from the case studies)

- **Stripe amounts are in cents.** $3,300 = 330000. Use `sk_test_` keys for demos; test cards
  only work in test mode. Match the charge to the estimator output exactly.
- **Never auto-send customer emails.** Draft → show owner → explicit "send" → then send.
- **Email is often GoDaddy IMAP/SMTP (himalaya), not M365.** Don't assume Microsoft Graph.
  `himalaya message save --folder Drafts` for drafts (NOT `template save` — doesn't exist);
  `himalaya template reply <msg_id>` for thread replies (preserves In-Reply-To headers).
- **Link/approval holds can expire** (~6 min in the Link CLI). Set up the demo/camera first,
  then trigger the spend — don't race the timer.
- **Region/pricing config must be consistent** across estimator, website, and CRM. One
  canonical source; the rest follow.

## Windows quirks (verified)

- **Alt+Enter** doesn't insert a newline → use **Ctrl+Enter**.
- **HTTP 400 "No models provided"** on first run → config.yaml saved with a UTF-8 BOM
  (Notepad). Re-save UTF-8 without BOM, or use `hermes config edit`.
- **Defender flags `uv.exe`** → false positive; whitelist the `bin` folder (see README).

## When you're truly stuck

`/debug` uploads a debug report (system info + logs) and returns shareable links. Ask in the
[Discord](https://discord.gg/NousResearch) or file a [GitHub issue](https://github.com/NousResearch/hermes-agent/issues).
