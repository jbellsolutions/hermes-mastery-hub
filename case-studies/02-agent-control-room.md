# Case Study 02 — The Agent Control Room: from one agent to a team

*Source: `shannhk/hermes-agent-control-room` (MIT) — README, docs (architecture, levels,
orchestrator, naming, starter-guide), task-bus templates, and bundled skills. Grounds
`../reference/10-multi-agent-control-room.md`.*

## The one-line story

Instead of building "one giant agent with every credential," you build a **Control Room** —
a side control plane that documents and governs a growing set of focused agents — and scale
from one agent → direct specialists → orchestrator → automated team, only advancing when the
manual layer works.

## The problem it solves

People end up with "a pile of disconnected bots": no map of what runs where, no runbooks, no
recovery plan, secrets scattered, and no clear answer to "which agent do I even ask?" The
Control Room makes agents run **like an operating system**.

> Core idea, verbatim: **"Set up the Control Room first, then plug agents into it."** It is
> *not* an agent — it's the system map, operating manual, registry, runbook library, and
> recovery notebook.

## The four levels (grow into it)

| Level | What you add | When |
|---|---|---|
| **1** | Control Room + **one** agent | Starting with a single personal/work assistant |
| **2** | **Direct specialists** (`hermes-seo`, `hermes-dev`, `hermes-cmo`, `hermes-ops`, …) | Roles are clear enough to separate tools/memory/creds/crons |
| **3** | **Orchestrator** front door + task bus | It's annoying to remember which specialist to ask |
| **4** | **Automated team** (recurring workflows, audits, backups, routing) | Only after the manual workflow works |

**Recommended first milestone (don't skip):** Control Room exists on a box; one agent is
documented in `agents/<name>/`; no raw secrets in the repo; you can restart/recover that one
agent from its runbook.

## The three access paths (you're never locked in)

```
Control path:      You → Agent Control Room (edit docs/rules)
Direct path:       You → hermes-seo / hermes-dev / hermes-cmo
Orchestrated path: You → hermes-orchestrator → Task Bus → Specialist → Orchestrator → You
```

## Control plane vs runtime (the key separation)

```
/root/agent-control-room     docs, templates, runbooks, registry — NO raw secrets
/srv/<agent-name>/data       live runtime: .env, memory, skills, sessions, crons, logs
```
One container per long-running agent. Never run two gateway processes against the same data
directory. In Hermes terms, a specialist is either a **profile** (single box, simplest) or a
**container** (isolation/scale).

## The task bus (how the orchestrator delegates)

A filesystem handoff desk — `inbox / working / outbox / archive` — where tasks are markdown
files with frontmatter. The safety-critical field:

```yaml
requires_approval_before:
  - publishing
  - pushing commits
  - rotating credentials
  - destructive operations
```

Specialists return findings, files changed, risks/assumptions, recommended next action.
Hermes's built-in **kanban** (`../reference/09-...`) is the native equivalent.

## Naming discipline (one slug everywhere)

`hermes-seo` = container = `/srv/hermes-seo/data` = `agents/hermes-seo` = `hermes-seo:<key>`
= bot "Hermes SEO". Recommended: `hermes-orchestrator, hermes-life, hermes-seo, hermes-dev,
hermes-cmo, hermes-ops, hermes-finance`. Avoid `main, agent1, bot, assistant, test`.

## Bundled skills in the source repo

`create-vps`, `setup-control-room`, `agent-control-room`, `agent-task-router`,
`agent-registry-manager`, `agent-backup-manager`, `agent-security-auditor`,
`agent-team-cron-planner`. You can literally point an agent at the repo: *"Read this repo and
help me set up an Agent Control Room. Start with docs/starter-guide.md and the
setup-control-room skill."*

## Security rule (verbatim)

Never commit raw secrets. Record secret **names, provider, scope, location, rotation date** —
never key values, OAuth refresh tokens, passwords, or private keys.

## How this hub uses it

Your business roles (`../business-roles/`) start as **skills on one agent** (Level 1–2). When
a role needs its own credentials, schedule, and memory, promote it to a **profile/container
specialist** (Level 2–3), and let `hermes-orchestrator` route to it (Level 3). Example 07
(`../examples/07-control-room-task-routing.md`) walks the orchestrator → task bus → specialist
loop. **Do not start here** — most solo operators live happily at Level 1–2 with a
multi-skill single agent.
