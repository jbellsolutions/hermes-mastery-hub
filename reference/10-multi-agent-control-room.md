# 10 — Multi-Agent Control Room

*Source: `shannhk/hermes-agent-control-room` (MIT) — README, docs/architecture.md, docs/levels.md, docs/orchestrator.md, docs/naming.md, docs/starter-guide.md, task-bus templates, and the bundled skills. Also the installed `hermes-remote-db` skill for the VPS+local pattern.*

When one agent isn't enough, you don't build "one giant agent with every credential." You
build a **Control Room**: a side control plane that documents and governs a set of focused
agents. This is the architecture for running your business roles as a real operating system.

> Key idea from the source: **set up the Control Room first, then plug agents into it.**
> The Control Room is *not* an agent — it's the system map, operating manual, registry,
> runbook library, and recovery notebook.

## The four levels (grow into this, don't start at the top)

- **Level 1 — Control Room + one agent.** Document one agent. No orchestrator, no task bus.
  Best first milestone: the Control Room exists, one agent is documented, no raw secrets in
  the repo, and you can restart/recover that agent from its runbook.
- **Level 2 — direct specialists.** Add role-specific agents you talk to directly:
  `hermes-life`, `hermes-seo`, `hermes-dev`, `hermes-cmo`, `hermes-ops`. Add a specialist
  only when the role is clear enough to separate tools, memory, credentials, and crons.
- **Level 3 — orchestrator + specialists.** Add one `hermes-orchestrator` as an optional
  front door. It reads the Control Room, writes task briefs, routes through the task bus,
  reviews results, and synthesizes. It must NOT hold every credential or bypass the
  specialist that owns a domain.
- **Level 4 — automated team.** Recurring workflows, audits, backup checks, task routing.
  **Only automate after the manual workflow works.**

## Access paths (you're never locked into one)

```
Control path:      You → Agent Control Room (edit docs/rules)
Direct path:       You → hermes-seo / hermes-dev / hermes-cmo
Orchestrated path: You → hermes-orchestrator → Task Bus → Specialist → Orchestrator → You
```

## Control plane vs runtime (keep them separate)

```
/root/agent-control-room     docs, templates, runbooks, registry — NO raw secrets
/srv/<agent-name>/data       live Hermes runtime: .env, memory, skills, sessions, crons, logs
```
One Docker container per long-running agent. Never run two gateway processes against the same
data directory. In Hermes terms, each specialist can be a **profile** (single box, simplest)
or a **container** (isolation, scale).

## The task bus (orchestrator → specialist handoff)

A filesystem "handoff desk": `inbox / working / outbox / archive`. Tasks are markdown files
with frontmatter declaring `assigned_to`, `priority`, `status`, and — critically —
`requires_approval_before: [publishing, pushing commits, rotating credentials, destructive
operations]`. Specialists return: findings, files changed, risks/assumptions, recommended
next action. (Hermes's native **kanban** is the built-in equivalent — see `09-...`.)

## Naming (one stable slug everywhere)

```
Agent slug: hermes-seo   Container: hermes-seo   Data: /srv/hermes-seo/data
Docs: agents/hermes-seo  Keys: hermes-seo:<purpose>   Bot: Hermes SEO
```
Recommended base names: `hermes-orchestrator, hermes-life, hermes-seo, hermes-dev,
hermes-cmo, hermes-ops, hermes-finance`. Avoid `main, agent1, bot, assistant, test`.

Those are **Control Room slugs** for containers, data directories, docs, and bot labels.
Hermes **profile names are a separate namespace** and the observed CLI requires lowercase
alphanumeric names, so map `hermes-cmo` → profile `cmo` or `hermescmo`.

## Bundled skills in the source repo

`create-vps`, `setup-control-room`, `agent-control-room`, `agent-task-router`,
`agent-registry-manager`, `agent-backup-manager`, `agent-security-auditor`,
`agent-team-cron-planner`. Point your agent at the repo: *"Read this repo and help me set up
an Agent Control Room. Start with docs/starter-guide.md and the setup-control-room skill."*

## Security rule (verbatim from source)

Never commit raw secrets. The Control Room may record secret **names, provider, scope,
location, rotation date** — never key values, OAuth refresh tokens, passwords, or private keys.

## When a solo operator should — and shouldn't — use this

- **Should:** you're running 3+ genuinely distinct roles with different credentials and
  schedules, and remembering "which agent do I ask?" has become friction.
- **Shouldn't (yet):** you have one agent and a handful of skills. Stay at Level 1–2. A
  single Hermes agent with multiple *skills* (your business roles as skills) covers most of a
  solo operator's needs. Reach for multi-agent only when isolation or parallelism forces it.

## Mapping to this hub's business roles

Each role in `business-roles/` can start life as a **skill** on one agent (Level 1–2). When a
role needs its own credentials, schedule, and memory, promote it to a **profile/container**
specialist (Level 2–3), and let an orchestrator route to it (Level 3). The role cards are
written so this promotion is a copy-paste, not a rewrite.
