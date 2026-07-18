# Example 08 — Personal Life-OS (a day run through your agent)

*Grounded in `../personal-use-cases/`. Same discipline as business: honest integrations,
approval before any send/purchase/booking, no medical/legal/financial advice without a
professional caveat, never type secrets or 2FA.*

## Goal
Run a normal day through your agent: morning brief, inbox triage, task/calendar wrangling, one
booking or purchase (gated), and an evening wrap — all approval-gated.

## Prereqs (honest wiring)
- Calendar/email/notes: native where possible; Google Workspace / Apple (memo, remindctl) /
  Notion / Obsidian via their skills — **bring the account**.
- Anything that sends, books, or buys needs the relevant account AND your approval.
- `approvals.mode: manual`.

## Workflow map
```
Morning brief (calendar + tasks + inbox highlights) → 🚦approve any replies →
triage tasks → draft one booking/purchase → 🚦approve → evening wrap + tomorrow's plan
```

## Step-by-step
1. "Morning brief: today's calendar, top 3 tasks, anything in my inbox that needs me."
2. "Draft replies to the 2 emails that need one." → **🚦 gate** before any send.
3. "Reschedule my 2pm to Thursday and add a reminder to prep for it." (Calendar/reminders via
   your wired account.)
4. "Find 3 flight options for [trip] and draft the booking." → agent researches, shows options.
   **🚦 gate:** you approve before any booking/payment. (Agent never enters your card/2FA.)
5. "Evening wrap: what got done, what slipped, and a plan for tomorrow." Log it.

## Approval gates (must fire)
- Before sending any message. Before any booking or purchase. Before sharing personal data.

## Safety guardrails (personal-specific)
- No medical/legal/financial *advice* — surface options and say "consult a professional."
- Never type passwords, card numbers, or 2FA codes. If a flow needs them, hand back to you.
- Don't touch clearly-personal accounts (banking, health) unless that's the explicit task.

## Verification checklist
- [ ] Brief pulled from your real calendar/inbox (wired), not invented
- [ ] Every reply/booking/purchase required your approval
- [ ] No secrets/2FA were typed by the agent
- [ ] Evening wrap logged for continuity

## Common pitfalls
- Assuming calendar/email access it doesn't have → wire the account first or say so.
- Auto-booking → always gate money and commitments.
- Over-pinging → batch into a morning brief + evening wrap, not per-event noise.
