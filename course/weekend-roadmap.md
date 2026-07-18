# The Weekend Roadmap — from zero to top-tier operating system

This is the central path. The coach follows it to sequence your weekend. Each block links to a
course module and produces a concrete artifact. **Do the exercises — reading alone doesn't
compound; running real workflows does.**

## Honest framing (read once)

"Top 1% by the weekend" is the goal. The truthful version: **in a focused weekend you build
the operating system of a top-tier Hermes user** — a working setup, a gateway you talk to from
your phone, 3+ approval-gated workflows, your first custom skills, and a roadmap. Genuine
world-class expertise comes from the weeks after, as your agent's memory and skills accumulate
around *your* business. This weekend gets you the scaffold and the momentum.

Time estimates are rough — go at your pace. Skip nothing in Friday/Saturday-morning; those are
load-bearing.

---

## Friday night — Foundation (≈1–2 hrs)
**Module:** `module-01-setup.md`
- Install Hermes, run `hermes doctor`, pick a model (`hermes model` or `hermes setup --portal`).
- After GitHub authentication, clone the private hub into
  `~/.hermes/skills/hermes-mastery-hub`, then run `hermes --skills hermes-mastery-hub`.
  If you already have a local checkout, copy it into that skill directory instead.
- Say: **"Be my Hermes coach. Start the weekend bootcamp."**
- **Artifact:** a working agent that can run a terminal command + a web search.
- **Checkpoint:** `hermes doctor` is green; the coach has confirmed your setup.

## Saturday morning — The learning loop (≈2–3 hrs)
**Modules:** `module-02-skills.md`, `module-03-memory.md`, `module-04-gateway.md`
- Install and inspect a real skill pack; write your first tiny skill from the template.
- Understand memory vs. session-search vs. skills.
- Connect a gateway (Telegram recommended) and test `/approve`.
- **Artifacts:** one custom `SKILL.md` that loads; a phone you can talk to your agent from.
- **Checkpoint:** you can install, load, and author a skill; approvals work over chat.

## Saturday afternoon — Your first business workflow (≈2–3 hrs)
**Modules:** `module-05-business-workflows.md`, `module-06-business-roles.md` (operator)
- Fill `../templates/business-intake.md` with your real business.
- Run **lead → estimate → approve → payment link → draft email → track** by hand (Example 01).
- Build the **operator** role as a skill.
- **Artifacts:** the operator skill; one real (or realistic dry-run) lead processed end-to-end.
- **Checkpoint:** you drove a full owner-gated workflow; nothing sent without your yes.

## Saturday evening — A second role (≈1–2 hrs)
**Module:** `module-06-business-roles.md` (email-reply or cold-email)
- Build **email-reply** (inbox triage + thread-safe drafts) *or* **cold-email** (compliant,
  capped outbound). Pick the one that removes your biggest pain.
- **Artifact:** a second role skill + its first workflow (Example 03 or 02).
- **Checkpoint:** two roles live; you understand the approval gate cold.

## Sunday morning — Automation & integrations (≈2 hrs)
**Module:** `module-07-webhooks-cron-mcp.md`
- Turn one *proven* workflow into a **cron** job (daily standup → your phone; Example 04).
- Optionally wire a **webhook** (e.g. new-lead notification) and/or an **MCP** server.
- **Artifact:** one scheduled job delivering to your gateway.
- **Checkpoint:** your agent does something useful unattended.

## Sunday afternoon — The role library + custom skills (≈2 hrs)
**Modules:** `module-06-business-roles.md`, `module-02-skills.md` (author track)
- Add the roles you actually need (finance-ops, marketer, customer-success…).
- Author one skill from scratch that's specific to your business; patch it after a test.
- **Artifact:** 3–5 role skills + one bespoke skill.
- **Checkpoint:** the agent is measurably tuned to your business.

## Sunday evening — Capstone & operating plan (≈1–2 hrs)
**Module:** `module-09-capstone.md`
- Assemble your roles into a daily/weekly operating rhythm (founder-chief-of-staff optional).
- Fill `../agent-feed/ASSESSMENT.md` to see your level.
- Write your 30-day plan into `../agent-feed/DAILY_LEARNING_LOG.md`.
- **Artifact:** a written operating plan + honest self-assessment.
- **Checkpoint:** you can run your business through the agent and know what to build next.

---

## After the weekend (where mastery actually happens)
- Run the workflows daily; patch skills when they're wrong; let the agent accumulate memory.
- Graduate to a Control Room (`module-08-control-room.md`) only when one agent isn't enough.
- Every time you do something twice, make it a skill. That's the compounding that makes you elite.
