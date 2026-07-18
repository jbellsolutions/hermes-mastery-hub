# Business Role: **Operator** (start here)

> The foundational role. The operator is the general manager of your business agent — it owns
> intake, triage, coordination, and keeping the day moving. Every other role plugs into it.

## Mission
Keep the business running day-to-day: capture what comes in, triage it, route it to the right
workflow, keep records straight, and surface what needs your decision — all through chat.

## Best Uses
- Intake + triage of leads, emails, tasks, and requests from any channel.
- Running the lead → estimate → payment loop (with the sales/finance skills).
- Daily/weekly status: what's open, what's stuck, what needs approval.
- Coordinating handoffs to specialist roles (marketer, sales, copywriter, …).
- Keeping the CRM and the learning log current.

## Do NOT Use For
- Deep specialist work (ad buying, long-form copy, financial modeling) — delegate to the role.
- Anything with a side effect it hasn't shown you first (see Approval Gates).

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status |
|---|---|---|---|
| Read/triage inbound chat | gateway (Telegram/Slack/Discord) | Yes | ☐ wire gateway |
| Read email | SMTP/IMAP (himalaya) or Gmail API | No — bring account | ☐ |
| Track leads/customers | crm-lite (SQLite) | Yes | ☐ install skill |
| Price jobs | estimator-engine (config rate card) | Yes | ☐ |
| Send payment link | Stripe API key | No — bring account | ☐ |
| Schedule reports/reminders | cron | Yes | ☐ |

## Approval Gates (mandatory)
Draft → show → wait for "yes" before: sending any message, creating any charge, publishing
anything, deleting/overwriting data. Keep `approvals.mode: manual`.

## Inputs Needed From User
Business name, what you sell, primary channel, current stack (fill `../templates/business-intake.md`),
and your daily rhythm (when you want the standup/report).

## First Workflow
Run one real lead through **lead → estimate → approve → payment link → draft email → track**
by hand. Walkthrough: `../examples/01-lead-to-payment.md`.

## Example Prompts
- "New lead from Telegram: [paste]. Summarize and tell me what you'd do."
- "Give me the morning standup: open leads, unpaid estimates, anything waiting on me."
- "Draft the estimate for this job, show me before sending."
- "Update the CRM: mark Jane Doe as won, $3,300."

## KPIs
- Response time to new leads · % of leads with an estimate within 24h · unpaid-estimate aging
· owner minutes saved/day.

## Failure Modes (and guardrail)
- Acts without showing you → approval gate + `manual` mode.
- Loses context across days → CRM + learning log are the source of truth, not chat memory.
- Over-triages/spams you → batch into a scheduled standup instead of per-event pings.

## Convert to a Hermes Skill
Use `../templates/SKILL.md`. `name: operator`,
`description: "Use when triaging inbound business items, running daily standups, or
coordinating lead→payment for a small business."` Save to
`~/.hermes/skills/business/operator/SKILL.md`, `/reload-skills`, test the trigger.
