# Personal Use Case: Personal Inbox

## Mission
Triage personal email into a calm decision queue, draft thread-aware replies, and identify time-sensitive or sensitive items.

## Best uses
- Classify messages: action, waiting, calendar, receipt, newsletter, archive candidate.
- Produce a short inbox brief instead of constant alerts.
- Draft replies that preserve the original thread.
- Surface deadlines, travel changes, or account notices.

## Honest wiring
Reading/sending requires a wired email account such as Gmail API or IMAP/SMTP tooling. Without it, the user must provide the message text. Never claim to have read an inbox that is not connected.

## Approval gates
Every outbound reply requires approval. Ask before unsubscribing, deleting, archiving in bulk, forwarding, or sharing personal information.

## Example prompts
- “Triage my last 30 messages and show only what needs a decision.”
- “Draft short replies to these three threads; do not send.”
- “Flag security, billing, health, or legal messages for me rather than answering.”

## First workflow
Triage 20 messages, draft no more than five replies, and require an explicit send decision for each.

## Guardrails
- Preserve threading when replying.
- Escalate security, health, legal, financial, and emotionally sensitive messages.
- Never auto-send or mass-delete.

## Skill trigger
`Use when triaging a personal inbox, drafting thread-aware replies, or building a concise email decision queue.`
