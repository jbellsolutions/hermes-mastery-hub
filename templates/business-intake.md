# Business Intake Template

> The coach fills this out WITH the user before building any role. Real context here is what
> turns a generic role card into an agent that actually fits the business. Keep answers short.

## Business basics
- Business name / what you sell:
- Who buys it (ICP — industry, size, title):
- Price points / rate card (or "config-driven, I'll provide"):
- Primary channel you operate from (Telegram / Slack / Discord / email / CLI):
- Brand voice in 3 words:

## Current stack (what's already wired — be honest)
| System | Tool | Wired? | Key location |
|---|---|---|---|
| Email send/receive | e.g. Gmail API / GoDaddy IMAP (himalaya) / Resend | ☐ | `.env` var name |
| Payments | e.g. Stripe | ☐ | `STRIPE_SECRET_KEY` |
| CRM | e.g. crm-lite / HubSpot / Airtable | ☐ | |
| Ads | e.g. Meta/Google Ads API | ☐ | |
| Social posting | e.g. X/LinkedIn API | ☐ | |
| Web research/scrape | native browser / rtrvr MCP | ☐ | |
| Accounting | e.g. QuickBooks / CSV export | ☐ | |

## Roles you want (rank the top 3 to build first)
- [ ] operator  [ ] marketer  [ ] cold-email  [ ] sales  [ ] media-buyer  [ ] copywriter
- [ ] email-reply  [ ] customer-success  [ ] finance-ops  [ ] research-analyst
- [ ] automation-engineer  [ ] founder-chief-of-staff  [ ] orchestrator

## Guardrails
- What must NEVER happen without your explicit approval? (default: all send/spend/publish)
- Daily caps? (e.g. max cold emails/day, max ad spend/day):
- Words/claims to avoid in outbound copy:

## Success in 30 days looks like
- One sentence:

---
*Once filled, the coach maps answers → the relevant `../business-roles/*.md` cards → builds the
top-ranked role as a skill and runs its first workflow, gating every side effect.*
