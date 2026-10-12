# Module 04 — Gateway (talk to your agent from your phone)

## Outcome
Your agent runs on a messaging platform (Telegram recommended) and you can approve actions over chat.

## Required reading
- ../reference/06-gateway-and-messaging.md

## Exercise
1. Create a Telegram bot with @BotFather; copy the token.
2. `hermes gateway setup` → choose Telegram → paste the token.
3. Start it: `hermes gateway run` (or `hermes gateway install` for always-on).
4. DM your bot. Send `/sethome` in the chat you'll use.
5. Confirm approvals: the default is now `smart` (an aux-LLM reviews risky commands). For this
   exercise set `hermes config set approvals.mode manual`, ask the agent to do something flagged
   risky, and approve it with `/approve` in chat.
   Also set an allowlist: `TELEGRAM_ALLOWED_USERS=<your id>` in `.env` (gateways are deny-all by default).
6. (Optional) Send a voice memo to confirm transcription if STT is configured.

## Artifact
- A phone (or desktop Telegram) you can operate your agent from, with working approvals.

## Verification checklist
- [ ] The bot responds to your DM
- [ ] `/sethome` set your home channel
- [ ] A risky action prompted for approval; `/approve` executed it, `/deny` would block it
- [ ] `/platforms` shows Telegram connected

## Coach notes
Telegram is the lowest-friction platform; Slack is better for business (files, threads). Keep
`approvals.mode: manual` (or `smart` plus `approvals.deny` rules) for any agent that spends money or
sends messages. One gateway per machine now serves every profile — see `../reference/15-profiles-deep-dive.md`. Gateway dies on
SSH logout? `sudo loginctl enable-linger $USER`.

## Next action
Proceed to Module 05 — Business Workflows.
