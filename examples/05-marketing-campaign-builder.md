# Example 05 — Marketing Campaign Builder (draft → test → approve → send → report)

*Grounded in `../business-roles/marketer.md` and the real `marketing-campaign-builder` skill.*

## Goal
Turn a dormant email list into an active channel: agent drafts the email + subject A/B
variants, you approve, agent creates the campaign, sends a test to you, then sends/schedules on
your final yes — and reports afterward.

## Prereqs
- `marketing-campaign-builder` + `mailchimp-integration` skills (or another ESP).
- ESP wired: `MAILCHIMP_API_KEY`, `MAILCHIMP_AUDIENCE_ID`, `MAILCHIMP_SERVER_PREFIX` in `.env`.
- Brand voice + forbidden claims (from `../templates/business-intake.md`).

## Workflow map
```
Owner describes campaign → agent drafts body + 2-3 subject lines → 🚦owner picks subject →
create campaign (regular or A/B) → send TEST to owner → 🚦owner approves → send/schedule → report
```

## Step-by-step
1. "Send a summer re-engagement campaign to lapsed contacts — mention [offer], free estimates."
2. Agent extracts audience/topic/tone and drafts: 2–3 subject lines (<50 chars), preview text,
   HTML body, plain-text version.
3. **🚦 Gate:** agent shows the draft + subject options. You pick A/B/C or write your own.
4. Agent creates the campaign in the ESP (regular, or `variate` for a subject A/B test).
5. Agent sends a **test email to you** and says "check your inbox; reply 'send' or tell me what to change."
6. **🚦 Gate:** you reply "send" (immediate) or "schedule Tuesday 10am ET" (best B2B open time).
7. After 48h: "pull the campaign report." Agent summarizes opens/clicks/unsubs/best segment.

## Approval gates (must fire)
- Subject-line selection (you choose).
- After the test send, before the full send.

## Verification checklist
- [ ] Draft + subject variants shown before anything was created
- [ ] A **test to you** happened before the full send
- [ ] Full send required your explicit "send"
- [ ] Post-send report delivered

## Common pitfalls
- **Never send without a test** — one bad link to the whole list can't be undone.
- CAN-SPAM: unsubscribe footer required (ESP adds `*|UNSUB|*`).
- Don't A/B everything at once — test subject lines first, then timing.
- Merge tags need exact match (`*|FNAME|*`). Avoid Friday-afternoon sends.
