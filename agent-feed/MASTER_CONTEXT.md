# MASTER CONTEXT — the compressed map

> The coach loads this to navigate the hub fast. It's the index + the doctrine in one page.

## What Hermes is (30 seconds)

Open-source, self-improving AI agent by Nous Research. Runs in terminal, desktop app,
messaging platforms (Telegram/Discord/Slack/…), and IDEs. Provider-agnostic (20+). Learns via
**skills** (Markdown+YAML procedural memory), remembers via **memory** + **session search**,
lives anywhere (local/Docker/SSH/Modal/Daytona), reacts via **webhooks**, runs unattended via
**cron**, parallelizes via **delegation**, scales via **profiles** + **kanban** + Control Room.

## The five doctrines (everything reduces to these)

1. **Draft → approve → execute.** Human gates every real-world side effect.
2. **Encode expertise as skills.** Do it twice → make it a skill. Facts→memory, procedures→skills.
3. **Honest capability.** Only claim what's wired. "Coming later" beats a silent failure.
4. **Start manual, automate later.** Prove by hand → cron → webhook → maybe a second agent.
5. **Verify before "done."** Real output, not a description of it.

## Reference index (`../reference/`)

| File | Covers |
|---|---|
| 00-ground-truth | anti-hallucination contract; skill format |
| 01-hermes-overview | what Hermes is, surfaces, mental model |
| 02-installation-and-setup | install, `hermes setup --portal`, key paths |
| 03-configuration-and-profiles | config sections, profiles, security toggles, approvals |
| 04-skills-system | the learning loop; how to author skills |
| 05-memory-and-learning-loop | memory vs session-search vs skills; curator |
| 06-gateway-and-messaging | Telegram/Discord/Slack; approval over chat |
| 07-webhooks | event-driven runs; Stripe/lead patterns |
| 08-native-mcp | connect external tool ecosystems; security |
| 09-cron-and-durable-systems | cron, delegation, curator, kanban |
| 10-multi-agent-control-room | Level 1→4, task bus, naming, control-plane vs runtime |
| 11-best-practices | the operating doctrine |
| 12-troubleshooting | doctor-first, gateway, business pitfalls, Windows |

## Business roles (`../business-roles/`)

operator · marketer · cold-email · sales · media-buyer · copywriter · email-reply ·
customer-success · finance-ops · research-analyst · automation-engineer ·
founder-chief-of-staff · orchestrator. Each card: mission, allowed/forbidden, approval gates,
required integrations (honest), example prompts, first workflow, convert-to-skill, KPIs,
failure modes.

## Course (`../course/`)

`weekend-roadmap.md` + modules 01–09: setup → skills → memory → gateway → business workflows →
roles → webhooks/cron/mcp → control room → capstone.

## Case studies (`../case-studies/`)

01 Mom-n-Pop (owner-gated small-business pack, hackathon build) · 02 Control Room (multi-agent
levels) · 03 Master Class pointer (login-gated; study guide only).

## Integrations honesty table (memorize)

| Role wants to… | Needs external… | Hermes-native? |
|---|---|---|
| Send email | SMTP/IMAP (himalaya), Gmail API, Resend/Mailgun | No — bring account |
| Charge cards | Stripe API key | No — bring account |
| Run ads | Meta/Google Ads API or browser automation | No — bring account |
| Post social | X/LinkedIn API or browser | Partial (x_search); bring account for posting |
| CRM | SQLite (crm-lite) native; QuickBooks/HubSpot = API | crm-lite yes; others bring account |
| Web research/scrape | rtrvr MCP / browser toolset | browser native; rtrvr = bring key |

Everything a role "does" that touches the outside world requires the account/API/MCP to be
configured AND passes through an approval gate. Never imply otherwise.
