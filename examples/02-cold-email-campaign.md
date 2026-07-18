# Example 02 — Cold Email Campaign (compliant, capped, approved)

*Grounded in `../business-roles/cold-email.md`. The rule: fewer, better, compliant emails —
and **never mass-send without approval + a daily cap**.*

## Goal
Build a small targeted segment, draft a personalized 3-touch sequence, and produce an
owner-approved send queue capped at 20/day.

## Prereqs
- Research: `web`/`browser` toolset (native) or rtrvr MCP (needs key).
- Email verification (optional): a verification API/skill (e.g. `verify-leads` if installed).
- Sending: SMTP/IMAP (himalaya) or an ESP API (Resend/Mailgun/SES) — **bring your account**.
- A suppression list (opt-outs) and a real physical address for the footer (CAN-SPAM).

## Workflow map
```
ICP → build 25-prospect list → verify → personalize opener → draft 3-touch sequence →
🚦approve queue → send capped 20/day → track replies → 🚦approve replies
```

## Step-by-step
1. "Build a 25-company list matching [ICP/firmographics]. Show me the list."
2. "Verify these emails; drop undeliverables." (If a verifier is wired.)
3. "For each prospect, draft a 1-line personalized opener referencing something specific."
4. "Draft a 3-touch sequence for [offer]: opener + 2 follow-ups. Include an opt-out line."
5. **🚦 Approval gate:** agent shows the full send queue. Review for accuracy, tone, compliance.
   Say "approve, cap at 20/day."
6. Agent queues sends (or sends the first 20 if wired), honoring the cap and suppression list.
7. "Who replied? Draft replies for me to approve." → **🚦 gate** before any reply goes out.

## Approval gates (must fire)
- Before the first send (whole queue reviewed).
- Before any reply is sent.

## Compliance guardrails (non-negotiable)
- Working unsubscribe + real physical address in every email.
- Honor opt-outs immediately; never re-contact the suppression list.
- Warm up new domains/inboxes; keep volume conservative.

## Verification checklist
- [ ] List matches the ICP; emails verified
- [ ] Each email has a personalized opener (not generic spray)
- [ ] Daily cap enforced; suppression list respected
- [ ] Every email includes opt-out + address
- [ ] No send happened without your approval

## Common pitfalls
- Deliverability tanks with volume spikes or spammy formatting → cap + warmup + plain text.
- Buying dubious lists → don't. Provenance matters legally and for deliverability.
