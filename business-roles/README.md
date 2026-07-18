# Business Roles — index & how to use

This directory holds **13 role cards**. Each card is a self-contained definition of a business
function your Hermes agent can perform: mission, best uses, an honest integrations table,
mandatory approval gates, example prompts, KPIs, failure modes, and instructions to convert
the role into a real Hermes skill.

> **The core idea:** a "role" is just a skill your agent loads. Start every role as a **skill
> on one agent**. Only promote a role to its own **profile or container** (Control Room
> Level 2–3) when it genuinely needs isolated credentials, schedule, or memory. Most solo
> operators never need to leave one multi-skill agent.

## The 13 roles

| Role | Mission (one line) | External wiring burden |
|---|---|---|
| [operator](operator.md) | Run day-to-day: intake, triage, lead→payment, standups | Low (gateway + crm-lite native) |
| [email-reply](email-reply.md) | Triage inbox, draft thread-safe replies for approval | Med (email account) |
| [sales](sales.md) | Qualify leads, prep calls, draft proposals/follow-ups | Med (CRM + email) |
| [marketer](marketer.md) | Plan campaigns, draft content, run approved email/social | Med (ESP/social account) |
| [cold-email](cold-email.md) | Build lists, personalize sequences, approved send queue | High (email + verify + research) |
| [copywriter](copywriter.md) | Long-form brand/marketing copy (pages, ads, sequences) | Low (writing native) |
| [customer-success](customer-success.md) | Onboarding, check-ins, reviews, churn flags | Med (CRM + messaging) |
| [research-analyst](research-analyst.md) | Source-cited market/competitor/prospect research | Low–Med (browser native; rtrvr optional) |
| [media-buyer](media-buyer.md) | Ad strategy + analysis; live ads need external API | High (Ads API + real money) |
| [finance-ops](finance-ops.md) | Reconcile, summarize, track invoices, flag anomalies | Med (Stripe/QBO; not tax advice) |
| [automation-engineer](automation-engineer.md) | Turn workflows into skills; wire cron/webhooks/MCP | Low (mostly native) |
| [founder-chief-of-staff](founder-chief-of-staff.md) | Weekly review, prioritization, coordinated plans | Low (synthesis across roles) |
| [orchestrator](orchestrator.md) | Route tasks across a **team** of agents (Level 3+) | High (multi-agent setup) |

## Which role to build first (recommended order)

1. **operator** — the foundation. Everything else plugs into it. Build this first, always.
2. **email-reply** *or* **sales** — pick the one that removes your biggest daily pain.
3. **marketer** and/or **cold-email** — turn on demand generation (cold-email has the most
   compliance burden — do it carefully).
4. **finance-ops** — once money is flowing, get visibility.
5. **customer-success** / **copywriter** / **research-analyst** — add as needed.
6. **automation-engineer** — once 2–3 workflows are proven manually, automate them.
7. **founder-chief-of-staff** — when you want a weekly strategic layer over the roles.
8. **orchestrator** — **last, and only** if you've genuinely grown into a multi-agent Control
   Room (`../reference/10-multi-agent-control-room.md`). A solo operator does not need it.

## The honest integrations caveat (applies to every role)

A role can only *do* something that touches the outside world if the account/API/MCP it needs
is **actually configured**, and every real-world side effect passes through an **approval
gate**. Hermes ships the reasoning, writing, CRM (crm-lite), scheduling (cron), and browser
natively — but sending email, charging cards, running ads, and posting to social all require
*you* to bring and wire the account. If it isn't wired, the honest answer is "you'll need to
wire X first," not a silent failure. See the wiring table on each card.

## Convert a role to a skill (quick reference)

1. Open `../templates/SKILL.md` (canonical Markdown + YAML format).
2. Set `name:`, write a "Use when …" `description` (this is the trigger), add `tags`.
3. Paste the role's Mission / Approval Gates / Example Prompts into the body.
4. Save to `~/.hermes/skills/business/<role>/SKILL.md`.
5. Run `/reload-skills`, then test the trigger phrasing.
6. Patch the skill with any pitfall you hit running its first workflow.

Full teaching version: `../course/module-06-business-roles.md`.
