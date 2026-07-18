# START HERE — Orientation

You are looking at the **Hermes Mastery Hub**: a verified reference library, a 9-module
weekend course, real case studies, a 13-role business library, templates, and worked
examples. It exists to be **fed to your Hermes agent** so the agent can coach you from
beginner to a top-tier operator.

## The one-command way to use this

```bash
# copy the hub into your skills dir so the agent can load it
cp -R ~/Desktop/hermes-mastery-hub ~/.hermes/skills/hermes-mastery-hub
hermes -s hermes-mastery-hub
```
Then say: **"Be my Hermes coach. Start the weekend bootcamp."**

Or, in any Hermes session: **"Load the hermes-mastery-hub skill and walk me through it step by step."**

## What the agent does when you say that

It reads `HERMES_COACH_PROMPT.md` (its operating contract), `MASTER_CONTEXT.md` (the map),
and `../course/weekend-roadmap.md` (the path). Then it:

1. Verifies your setup is working (`hermes doctor`, model, gateway).
2. Teaches **one module at a time**, with a hands-on exercise each time.
3. Helps you **build one business role at a time** as a real skill.
4. Gates every real-world action (send/spend/publish) behind your approval.
5. Keeps a **learning log** (`DAILY_LEARNING_LOG.md`) so progress compounds.
6. Ends each session with a clear **next action**.

## Honest expectation setting

"Top 1% by the weekend" is the goal you set. Here's the truthful version the coach will hold
you to: **by the end of a focused weekend you will have the operating system of a top-tier
Hermes user** — a working setup, a gateway you talk to from your phone, 3+ approval-gated
business workflows, your first custom skills, and a roadmap for the compounding that turns
into genuine mastery over the following weeks. Reading makes you informed; *running real
workflows* is what makes the agent (and you) elite. The hub is the fast-path scaffold; the
compounding is yours to earn.

## Map of the hub

- `../reference/` — 13 ground-truth docs (start at `00-ground-truth.md`)
- `../case-studies/` — Mom-n-Pop small business, Control Room multi-agent, Master Class pointer
- `../business-roles/` — 13 role cards + README
- `../course/` — 9 modules + `weekend-roadmap.md`
- `../templates/` — copy-paste artifacts (SKILL.md, role-card, task-brief, …)
- `../examples/` — 7 concrete approval-gated workflows
- `MASTER_CONTEXT.md` — the compressed map the coach loads
- `HERMES_COACH_PROMPT.md` — the coach's operating contract
- `DAILY_LEARNING_LOG.md` — your running progress log
- `ASSESSMENT.md` — the skills self-check / levels
