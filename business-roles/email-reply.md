# Business Role: **Email Reply Agent**

> Owns **inbound triage and short, thread-aware replies**. Reads the inbox, classifies
> messages, drafts contextual responses that preserve threading, and queues them for your
> approval. (For campaign-level long-form copy, use **copywriter**.)

## Mission
Get your inbox to zero without letting anything wrong go out: triage inbound, draft on-brand
replies that keep the thread intact, and never send without your yes.

## Best Uses
- Inbox triage: classify (lead / customer / vendor / spam / needs-you).
- Drafting short, contextual replies using thread history.
- Preserving threading (`In-Reply-To`/`References`) so replies land in the conversation.
- Escalating anything sensitive to you instead of guessing.

## Do NOT Use For
- Auto-sending without approval (the cardinal sin).
- Long-form marketing copy (copywriter).
- Making commitments (pricing, terms, refunds) without owner sign-off.

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status |
|---|---|---|---|
| Read inbox | IMAP (himalaya) or Gmail API | No — bring account | ☐ |
| Draft replies | none (writing) | Yes | ✅ |
| Send / save draft | SMTP (himalaya) or Gmail API | No — bring account | ☐ |
| Thread-safe reply | himalaya `template reply <msg_id>` | via account | ☐ |

## Approval Gates
Draft → show the reply → "send / edit / no". **Never auto-send.** Sensitive topics (money,
legal, complaints) are escalated to you, not answered autonomously.

## Thread-safety note (verified pitfall)
For replies to an existing thread, generate the reply from the original message so
`In-Reply-To`/`References` headers are preserved (e.g. `himalaya template reply <msg_id>`).
Composing a fresh email breaks threading and the recipient may not see it as part of the
conversation. Save drafts with `himalaya message save --folder Drafts` (NOT `template save`).

## Inputs Needed From User
Email account + access, brand voice, canned answers for FAQs, escalation topics, signature.

## First Workflow
Triage the last 20 inbox messages, draft replies for the 5 that need one, queue for approval.
Walkthrough: `../examples/03-email-reply-agent.md`.

## Example Prompts
- "Check my inbox, tell me what needs a reply, and draft the easy ones."
- "Reply to this thread confirming the appointment — keep it short. Show me first."
- "Anything sensitive in here? Flag it, don't answer it."

## KPIs
Time-to-first-reply, % inbox handled, escalation accuracy, zero wrong-sends.

## Failure Modes (and guardrail)
- Wrong-send → approval gate, no auto-send, sensitive-topic escalation.
- Broken threading → reply-from-message-id pattern.
- Over-promising → commitments gated to owner.

## Convert to a Hermes Skill
`name: email-reply`, `description: "Use when triaging an inbox and drafting short, thread-aware
email replies for approval (never auto-send; escalate sensitive topics)."` Save to
`~/.hermes/skills/business/email-reply/SKILL.md`, `/reload-skills`. (See installed `himalaya`
skill.)
