# 06 — Gateway & Messaging Platforms

*Source: installed `hermes-agent` skill v2.3.0, official README, Mom-n-Pop case study. Verify with `hermes gateway --help` and the [Messaging docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/).*

The gateway is what lets the *same* agent run on messaging platforms with full tool access.
This is the backbone of every business use case: the Mom-n-Pop team ran a real service
business entirely over **Telegram** because it's "the same app the owners already use to text."

## Commands

```bash
hermes gateway setup       # configure platforms (interactive)
hermes gateway run         # start in foreground
hermes gateway install     # install as a background service
hermes gateway start/stop/restart/status
```

## Supported platforms (20+)

Telegram, Discord, Slack, WhatsApp (Baileys bridge + official Business Cloud API), iMessage
(Photon — `hermes photon setup`), Signal, Email, SMS, Matrix, Mattermost, Microsoft Teams,
LINE, SimpleX, ntfy, Google Chat, Home Assistant, DingTalk, Feishu, WeCom, Weixin, Raft,
API Server, Webhooks. Open WebUI connects via the API Server adapter.

## The Telegram path (recommended first gateway)

Telegram is the lowest-friction, most-reliable starting platform (it's what the case study used):

1. Create a bot with @BotFather, get the token.
2. `hermes gateway setup` → choose Telegram → paste token.
3. `hermes gateway run` (or `install` for always-on).
4. DM your bot. Use `/sethome` to set the current chat as the home channel.
5. Voice memos are auto-transcribed if STT is configured.

## Gateway slash commands (in-chat)

```
/approve /deny        Approve or deny a pending command (this is your money guardrail)
/restart              Restart the gateway
/sethome              Set current chat as home channel
/platforms (/gateway) Show platform connection status
/topic                Telegram DM topic sessions
/status /commands     Session info / browse all commands
```

## Approval over chat (the whole business pattern)

With `approvals.mode: manual`, when the agent wants to run something flagged risky, it posts
a prompt; you reply `/approve` or `/deny`. This is exactly how the Mom-n-Pop flow gates
sending estimates, payment links, and outbound email. **Keep it on for business agents.**

## Common platform gotchas (verified)

- **Discord bot silent** → enable **Message Content Intent** in the bot's Privileged Gateway Intents.
- **Slack bot only works in DMs** → subscribe to the `message.channels` event.
- **Gateway dies on SSH logout** → `sudo loginctl enable-linger $USER`.
- **Gateway dies on WSL2 close** → set `systemd=true` in `/etc/wsl.conf`.
- **Crash loop** → `systemctl --user reset-failed hermes-gateway`.
- Check logs first: `grep -i "failed to send\|error" ~/.hermes/logs/gateway.log | tail -20`.
