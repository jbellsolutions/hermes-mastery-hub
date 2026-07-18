# Module 07 — Webhooks, Cron & MCP (make it react and run unattended)

## Outcome
Your agent does something useful on a schedule and (optionally) reacts to an external event,
plus you know how to add external tools via MCP.

## Required reading
- ../reference/09-cron-and-durable-systems.md
- ../reference/07-webhooks.md
- ../reference/08-native-mcp.md

## Exercise
1. **Cron (do this):** turn a *proven* manual workflow into a scheduled job. Follow
   `../examples/04-daily-pipeline-cron.md` — a daily standup/pipeline report delivered to your
   gateway (Telegram). Confirm delivery targets a real platform, or it won't reach you.
   ```bash
   hermes cron list
   hermes cron create "0 8 * * *"   # then set prompt + deliver telegram in the flow
   ```
2. **Webhook (optional):** enable the webhook platform (`hermes gateway setup`), then subscribe
   a new-lead or Stripe-payment notification. Test without the real service:
   ```bash
   hermes webhook list
   hermes webhook test <name> --payload '{"lead":{"name":"Test","email":"t@e.com"}}'
   ```
3. **MCP (optional):** add one MCP server to `~/.hermes/config.yaml` under `mcp_servers`,
   restart, and confirm its tools loaded (`hermes mcp list`). Remember: stdio servers only get
   the env vars you pass under `env:`.

## Artifact
- One cron job delivering to your gateway. (Optional: one webhook route, one MCP server.)

## Verification checklist
- [ ] `hermes cron list` shows your job and it delivered to your chat
- [ ] (If done) `hermes webhook test` triggered a run/delivery
- [ ] (If done) `hermes mcp list` shows the server and its `mcp_<server>_*` tools

## Coach notes
Automate only what already works manually (the Control Room rule). Cron delivery must target a
gateway-connected platform to actually reach you. Keep secrets in `.env` / MCP `env:`, never in
config values or git.

## Next action
Proceed to Module 08 — Control Room (advanced/optional).
