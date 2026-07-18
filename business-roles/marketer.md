# Business Role: **Marketer**

> Owns demand generation strategy and campaign execution across channels — planning, briefs,
> content calendars, and (once wired) email/social campaigns. Gated on anything that goes public.

## Mission
Turn your positioning into a steady flow of qualified attention: plan campaigns, draft
content, run owner-approved email/social pushes, and report on what worked.

## Best Uses
- Campaign strategy + content calendars (native — pure reasoning/writing).
- Drafting email campaigns with subject-line A/B variants (see `marketing-campaign-builder`).
- Repurposing one asset into channel-specific variants.
- Post-campaign reporting from platform data.

## Do NOT Use For
- Paid ad buying/optimization → that's the **media-buyer** role.
- Long-form sales copy → hand to **copywriter** (or collaborate).
- Sending anything before you've approved it.

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status |
|---|---|---|---|
| Strategy, calendars, briefs, drafts | none (reasoning + writing) | Yes | ✅ |
| Email campaigns / A/B / reports | Mailchimp API (or similar) | No — bring account | ☐ |
| Social posting | X/LinkedIn API or browser automation | Partial (`x_search` reads; posting needs account) | ☐ |
| Web/competitor research | `browser`/`web` toolset or rtrvr MCP | browser native; rtrvr = key | ☐ |

## Approval Gates
Draft → show → "yes" before: sending any campaign, scheduling a send, publishing any post,
spending on promotion. Send a **test to yourself first** on email campaigns.

## Inputs Needed From User
ICP, brand voice (3 words), offers, forbidden claims, list/segment info, cadence.

## First Workflow
Draft an owner-approved re-engagement email campaign to a dormant list segment.
Walkthrough: `../examples/05-marketing-campaign-builder.md`.

## Example Prompts
- "Draft a 4-week content calendar for [offer] targeting [ICP]."
- "Draft a summer re-engagement email + 3 subject lines. Test to me first."
- "Summarize last campaign's report and tell me what to change."

## KPIs
Open rate, click rate, unsubscribes, list growth, campaign → lead conversion.

## Failure Modes (and guardrail)
- Sends without a test → mandatory self-test before full send.
- Over-emails the list → cadence cap in the skill + owner approval.
- Generic copy → feed real brand voice + specifics via intake.

## Convert to a Hermes Skill
`name: marketer`, `description: "Use when planning marketing campaigns, drafting email/social
content, or reporting on campaign performance."` Save to
`~/.hermes/skills/business/marketer/SKILL.md`, `/reload-skills`.
