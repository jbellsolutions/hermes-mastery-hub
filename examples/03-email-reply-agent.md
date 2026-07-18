# Example 03 — Email Reply Agent (inbox triage + thread-safe drafts)

*Grounded in `../business-roles/email-reply.md` and the installed `himalaya` skill.*

## Goal
Triage your inbox, draft short contextual replies that preserve threading, and queue them for
approval — with zero auto-sends and sensitive topics escalated to you.

## Prereqs
- Email read/send wired: himalaya (IMAP/SMTP) or Gmail API. **Bring your account.**
- Brand/personal voice + a short list of escalation topics (money, legal, complaints).

## Workflow map
```
Read inbox → classify (lead/customer/vendor/spam/needs-you) → draft replies for the easy ones →
🚦approve each → send or save draft (thread-safe) → escalate sensitive items to you
```

## Step-by-step
1. "Check my inbox and tell me what needs a reply." Agent lists + classifies recent messages.
2. "Draft replies for the straightforward ones; flag anything sensitive instead of answering."
3. For each draft, agent shows the text. **🚦 Approval gate:** "send / edit / no."
4. On "send": agent replies **from the original message** so `In-Reply-To`/`References`
   headers are preserved (e.g. `himalaya template reply <msg_id>`), keeping it in-thread.
   On "save": `himalaya message save --folder Drafts`.
5. Sensitive items (pricing commitments, legal, complaints) are summarized to you with a
   recommended action — not answered autonomously.

## Approval gates (must fire)
- Before every reply is sent.
- Sensitive topics escalate instead of auto-replying.

## Verification checklist
- [ ] Inbox classified correctly
- [ ] Replies preserved threading (landed in the conversation)
- [ ] No reply sent without your "send"
- [ ] Sensitive items escalated, not answered

## Common pitfalls
- Composing a fresh email for a thread breaks threading → reply from the message id.
- `himalaya template save` does **not** exist → use `message save --folder Drafts`.
- Over-promising in replies → keep commitments gated to you.
