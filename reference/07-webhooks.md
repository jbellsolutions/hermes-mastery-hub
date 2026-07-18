# 07 — Webhooks (event-driven agent runs)

*Source: installed `hermes-agent` skill reference `webhooks.md`. Verify with `hermes webhook --help`.*

Webhooks let external services (GitHub, Stripe, CI, monitoring, IoT, form backends) trigger a
Hermes agent run by POSTing an event to a URL. This is how a business agent reacts to the
world instead of waiting to be asked — e.g. a Stripe `payment_intent.succeeded` event pings
Telegram and updates the CRM automatically.

## Enable it first

```bash
hermes webhook list         # says "not enabled"? set it up:
hermes gateway setup        # enable webhooks, set port + HMAC secret
```
Or config:
```yaml
platforms:
  webhook:
    enabled: true
    extra: { host: "0.0.0.0", port: 8644, secret: "strong-secret" }
```
Verify: `curl http://localhost:8644/health` → `{"status":"ok"}`.

## Create a subscription

```bash
hermes webhook subscribe <name> \
  --prompt "Template using {payload.fields}" \
  --events "event1,event2" \
  --skills "skill1,skill2" \
  --deliver telegram --deliver-chat-id "12345" \
  --secret "optional-custom-secret"
```
Prompts support `{dot.notation}` into the JSON payload (`{issue.title}`,
`{data.object.amount}`). Returns the URL + HMAC secret to paste into the service.

## Business-relevant patterns

**Stripe payment events → Telegram alert:**
```bash
hermes webhook subscribe stripe-payments \
  --events "payment_intent.succeeded,payment_intent.payment_failed" \
  --prompt "Payment {data.object.status}: {data.object.amount} cents from {data.object.receipt_email}" \
  --deliver telegram --deliver-chat-id "-100123456789"
```

**Direct delivery (no LLM, zero cost)** — forward a verbatim notification:
```bash
hermes webhook subscribe new-lead \
  --deliver telegram --deliver-chat-id "123456789" --deliver-only \
  --prompt "New website lead: {lead.name} — {lead.email} — {lead.message}"
```
`--deliver-only` makes the rendered prompt the literal message body. Use it for form-backend
lead notifications, monitoring alerts, and inter-agent pings.

## Security & ops

- Each subscription gets an HMAC-SHA256 secret; signatures validated on every POST.
- Subscriptions persist to `~/.hermes/webhook_subscriptions.json` (hot-reloaded).
- For local dev, expose the port with a tunnel (ngrok/cloudflared).
- Test without the real service: `hermes webhook test <name> --payload '{"k":"v"}'`.
