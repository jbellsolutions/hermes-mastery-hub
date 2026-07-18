# Workflow Map Template

> Sketch any end-to-end business workflow before automating it. The arrows are where approval
> gates and integrations live. Fill in your real steps. (Shape mirrors the Mom-n-Pop
> lead-to-payment flow.)

## Template

```
Trigger (how the workflow starts: inbound email / Telegram msg / website form / webhook / cron)
    ↓
Agent step 1  ──────────────  [integration needed? / native?]
    ↓
Agent step 2  ──────────────  [ … ]
    ↓
🚦 APPROVAL GATE  ───────────  user says yes/edit/no   ← every side effect passes through here
    ↓
Execute (send / charge / publish)  ──  [integration + credentials required]
    ↓
Log + follow-up (CRM update, learning log, cron reminder if unpaid/unanswered)
```

## Worked reference: lead → payment (from the case study)

```
Lead arrives (email / Telegram / website form)
    ↓
Agent qualifies + summarizes            (native)
    ↓
Agent prices it — estimator-engine      (native, rate card in skill config)
    ↓
Agent logs to CRM — crm-lite            (native, SQLite)
    ↓
🚦 APPROVAL GATE: owner approves estimate in Telegram
    ↓
Agent creates Stripe payment link       (needs STRIPE_SECRET_KEY; amounts in CENTS)
    ↓
Agent drafts customer email             (needs SMTP/IMAP e.g. himalaya) — DRAFT ONLY
    ↓
🚦 APPROVAL GATE: owner approves email → agent sends
    ↓
Customer pays → agent confirms → CRM updated → follow-up cron if unpaid in 3 days
```

## Fill-in checklist for YOUR workflow
- [ ] What triggers it?
- [ ] Which steps are Hermes-native vs need an external account?
- [ ] Where are the approval gates? (list every side effect)
- [ ] What gets logged, and where?
- [ ] What's the follow-up (cron/reminder) if the workflow stalls?
