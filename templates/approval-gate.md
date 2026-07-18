# Approval Gate Template — draft → approve → execute

> The core pattern of the whole hub (from the Mom-n-Pop build: *"the agent drafts and proposes;
> the human approves; then the agent executes."*). Reuse this checklist for anything with a
> real-world side effect.

## The pattern

```
1. DRAFT     Agent produces the exact artifact (email text, charge amount, ad, post, deploy).
2. SHOW      Agent shows the user the full draft + the precise action it will take.
3. WAIT      Agent stops. No side effect happens yet.
4. DECIDE    User replies: "yes" / "send" / "approve"  |  "edit: …"  |  "no".
5. EXECUTE   Only on explicit yes does the agent perform the action.
6. LOG       Agent records what was done (CRM, learning log) and confirms.
```

## What must be gated (never skip)
- [ ] Any outbound message to a third party (email, DM, SMS, WhatsApp)
- [ ] Any movement of money (charge, refund, payout, ad spend, subscription)
- [ ] Anything published or deployed to the public
- [ ] Any deletion / overwrite / irreversible change
- [ ] Any credential rotation

## The approval message shape (what the agent posts)
```
🧾 Ready to <ACTION>. Here is exactly what I'll do:
<full draft / amount / target>

Reply: yes  |  edit <what to change>  |  no
```

## Config that enforces it
```bash
hermes config set approvals.mode manual   # always prompt (safest for money/comms)
# or 'smart' to auto-approve low-risk and prompt on high-risk
```
Over a gateway (Telegram/Discord/Slack), you approve with `/approve` and reject with `/deny`.

## Gotcha
Some approval holds expire (the Link CLI's were ~6 minutes in the case study). For
time-sensitive real-money flows, set up first, then trigger the action — don't race the timer.
