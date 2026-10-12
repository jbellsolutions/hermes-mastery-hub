# 09 — Cron & Durable Background Systems

*Source: installed `hermes-agent` skill v2.3.0. Verify with `hermes cron --help`.*

Four systems run alongside the main conversation loop. Know which one to reach for.

## Cron (durable scheduled jobs) — the always-on backbone

Survives restarts. This is how a business agent sends a daily pipeline report, runs a nightly
backup, or polls Stripe for unpaid invoices — unattended.

```bash
hermes cron list                       # --all includes disabled
hermes cron create "0 9 * * *"         # daily 9am; also "30m", "every 2h", ISO timestamp
hermes cron edit ID                    # schedule / prompt / delivery
hermes cron pause|resume|run|remove ID
hermes cron status
```

Per-job knobs that matter: `skills` (preload), `model`/`provider` override, `script`
(pre-run data collection; `no_agent=True` makes the script the whole job — the classic
watchdog), `context_from` (chain job A's output into job B), `workdir` (run in a repo with its
`AGENTS.md` loaded), and multi-platform delivery.

**Delivery reality check:** a cron job only *messages* you if its `deliver` targets a
gateway-connected platform (e.g. `telegram`, `all`). A CLI/TUI-scheduled job with default
delivery is saved but not pushed into your terminal.

**Business examples:**
- Daily pipeline summary → Telegram at 8am (`crm-lite` weekly-report pattern).
- Poll Stripe for `unpaid` estimates older than 3 days → nudge the owner.
- Nightly `crm.db` backup to cloud storage.

## Delegation (`delegate_task`) — parallel subagents

Spawn an isolated subagent with its own context + terminal. Single or batch (parallel, capped
by `delegation.max_concurrent_children`, **default 10** since v0.21). **Async by default**: the call
returns a handle and the child's result re-enters the conversation when done. Steer or stop live
children with `delegate_task` `list/steer/stop` or `/agents`. Roles: `leaf` (default, can't
re-delegate) vs `orchestrator`. **Not durable** — if the parent process exits, the child is
lost. For work that must outlive the process, use cron or `terminal(background=True,
notify_on_complete=True)`.

## Curator (skill lifecycle) — covered in `04-skills-system.md`

Background maintenance of agent-created skills. Never deletes (max action = archive). Free
inactivity sweep; the LLM "consolidate overlapping skills" pass is opt-in.

## Kanban (multi-agent work queue)

Durable SQLite board for multi-profile/multi-worker collaboration. `hermes kanban <init|
create|assign|link|comment|unblock|watch|tail|runs|diagnostics|swarm>`. The gateway dispatches
workers; each run ends with `kanban_complete`, `kanban_request_review` or `kanban_block`; the
orchestrator is woken by `notify+wake` subscriptions. Single-host. Full guide:
`17-teams-and-orchestration.md`.

## New since v1.0: cron upgrades, `/loop`, `/heartbeat`, `/goal`

- Cron jobs now have **memory and continuity**, per-job reasoning effort, `deliver: bot-chat:<profile>`,
  `catch_up_missed`, `hermes cron doctor`, and a global `hermes pause`. Jobs can't create jobs unless
  `cron.allow_agent_scheduling: true`. Unattended approvals default to deny.
- `/loop` re-runs a prompt on a timer or self-paced (`loops.max_ticks` 100).
- `/heartbeat` one recurring idle-only prompt per session (min 60s).
- `/goal` judge-driven loop in one session (`goals.max_turns` 20; the judge fails open).

## Choosing between them

| Need | Use |
|---|---|
| Runs on a schedule, must survive restarts | **cron** |
| React to an external event | **webhooks** (`07-...`) |
| Parallelize a bounded subtask now | **delegate_task** |
| Long autonomous mission | spawn a `hermes` process (see skill's spawning section) |
| Coordinate a team of agents on one machine | **kanban** + Bot Mode |
| Talk to an agent on another machine | **`hermes peer`** or **A2A** |
