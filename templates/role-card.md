# Role Card Template — `<role-name>`

> Copy this to define a business role. Fill every section with the user's real context (use
> `business-intake.md` to gather it). When done, convert it into a Hermes skill with the
> instructions at the bottom.

## Mission
One or two sentences: what outcome this role owns.

## Best Uses
- The 3–5 tasks this role is genuinely great at.

## Do NOT Use For
- Adjacent work that belongs to a different role (prevents overreach).

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status for this user |
|---|---|---|---|
| e.g. send email | SMTP/IMAP (himalaya) or Gmail/Resend/Mailgun API | No — bring account | ☐ not wired |
| e.g. store contacts | crm-lite (SQLite) | Yes | ☐ |

> Rule: the role can only *do* something that touches the outside world if the account/API/MCP
> is actually configured. If it isn't, the honest answer is "you'll need to wire X first."

## Approval Gates (mandatory)
Before any of these, the agent drafts, shows you exactly what will happen, and waits for "yes":
- [ ] Sending any outbound message (email/DM/SMS)
- [ ] Spending money / creating a charge / launching an ad
- [ ] Publishing or deploying anything public
- [ ] Deleting or overwriting data

## Inputs Needed From User
- The specific business context this role needs (offer, ICP, brand voice, rate card, etc.).

## First Workflow (the one to run on day one)
A single, end-to-end, approval-gated task that proves the role works. Link to an `examples/` file.

## Example Prompts (say these to the agent)
- "…"
- "…"

## KPIs (how you know it's working)
- Metric 1, metric 2. Keep them few and honest.

## Failure Modes (and the guardrail)
- Failure → guardrail. (e.g. "over-sends → daily cap + approval gate".)

## Convert to a Hermes Skill
1. Open `../templates/SKILL.md`.
2. Set `name: <role-name>`, write a "Use when …" `description`, add `tags`.
3. Paste Mission → Approval Gates → Example Prompts into the body (Overview / Flow / Patterns).
4. Save to `~/.hermes/skills/business/<role-name>/SKILL.md`.
5. Run `/reload-skills`, then test the trigger phrasing.
6. Patch the skill with any pitfall you hit while running the first workflow.
