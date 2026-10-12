# 17 — Teams & Orchestration (one orchestrator, many employees)

*Source: `user-guide/features/delegation.md`, `guides/delegation-patterns.md`, `features/kanban*.md`, `messaging/a2a.md`,
`messaging/relay.md`, `bot-mode.md`, `features/bot-screen.md`, `features/goals.md`, `loops.md`, `heartbeat.md`, `cron.md`,
`messaging/telegram.md`, `messaging/discord.md`, `developer-guide/multiplexing-gateway.md` (main, 2026-10-12).
Defaults confirmed in `hermes_cli/config_defaults.py`. Items marked INFERRED are synthesis, not doc statements.*

## The menu — pick the smallest thing that works

| Pattern | What it is | Survives restart? | Crosses machines? | Use it for |
|---|---|---|---|---|
| **Skills on one agent** | One profile, many procedures | yes | no | Solo operator; roles that share memory |
| **Subagent delegation** (`delegate_task`) | Child agents forked in-process, fresh context, summary returns | **no** (children become `unknown`) | no | Research fan-out, reviews, parallel drafting |
| **Kanban board** | Durable SQLite work queue; cards assigned to profiles; gateway dispatches workers | yes | **no** (single host) | Orchestrator → employee work with audit trail, review, unblocking |
| **Bot Mode** | Each profile is a named Bot with a permanent "Bot Chat"; group rooms of 2–6 bots | yes | via Desktop courier | Team chat, @-mention handoffs, routines |
| **`hermes peer`** | Hermes-to-Hermes DMs and long runs over the peer's `api_server` | yes | **yes** | Your agents on different VPSs/Mac minis |
| **A2A v1.0** | Open Agent2Agent protocol over HTTP JSON-RPC/SSE | yes | **yes** | Talking to non-Hermes agents (LangChain, CrewAI, ADK) or across orgs |
| **Cron / `/loop` / `/heartbeat` / `/goal`** | Schedules and self-driving loops for one agent | cron yes | — | Standups, reports, monitors, "keep going until done" |
| Relay (experimental) | Holds platform bot tokens off-box; gateway dials out | — | — | Hosting/NAT — **not** agent-to-agent |

## 1. Delegation (fast, disposable)

- Async by default: the call returns a handle; the result lands as a new turn.
- Defaults: `max_concurrent_children: 10`, `max_spawn_depth: 1` (flat), `max_iterations: 250`,
  `subagent_auto_approve: false`, `worktree_isolation: false`, `max_summary_chars: 24000`.
- Children inherit the parent's tools but **cannot** use `clarify`, `memory`, `send_message`, `cronjob`, or delegate
  further unless given `role="orchestrator"`.
- Control: `delegate_task` actions `list` / `steer` / `stop`; `/agents` (Ctrl+T); `output_schema` for JSON results.
- Pin a cheap worker model globally with `delegation.model` / `delegation.provider`.
- **Children know nothing** — pass every fact they need in `context`.

## 2. Kanban (the durable team backbone)

```bash
hermes kanban init
hermes kanban create "Draft 3 cold-email variants for gyms" --assignee copy --skill cold-email
hermes kanban create "QA the variants" --assignee qa --model <cheap-model>
hermes kanban link <child> <parent>
hermes kanban watch | tail | runs | diagnostics
hermes kanban swarm ...            # workers → verifier → synthesizer
/kanban ...                        # from any chat
```

- The dispatcher runs inside the gateway (`kanban.dispatch_in_gateway: true`, ticks every 60s) and spawns
  `hermes -p <assignee> chat -q` as a real OS process.
- Every worker run must end with exactly one of `kanban_complete`, `kanban_request_review`, `kanban_block`.
- Orchestrator profile: enable the `kanban` toolset (`hermes -p boss tools enable kanban --platform slack`). Orchestrator
  guidance is injected automatically. It reads each employee's `profile describe` text to route work.
- Reporting back: subscriptions with `--delivery-mode notify+wake` wake the orchestrator for a real turn when a card
  completes, blocks, or requests review. `notify-subscribe … --chat-type group` posts events to a team group.
- Limits: `max_in_progress`, `max_in_progress_per_profile`, `failure_limit` (2), `dispatch_stale_timeout_seconds` (4h).
- Workers can't see sibling cards — **stamp shared decisions into every card body**.
- Scratch workspaces are deleted on completion unless files are declared as `artifacts`.

## 3. Bot Mode (the team chat layer)

- On by default in Desktop since v0.21.0. Each profile is a Bot with one permanent **Bot Chat**.
- Group rooms: 2–6 bots; hard caps of 3 serial rounds and 10 messages per send; `@user` escalates to you.
- `message_agent(target=…)` lets bots DM each other (fire-and-forget). Routines are cron jobs named `[bot:<name>]`.
- Cron can deliver to a bot: `deliver: bot-chat:<profile>` — the orchestrator processes reports as real turns.
- Headless setups: a chat only counts as a Bot Chat if it's titled exactly `Bot Chat` and some `profile.yaml` has
  `ui_meta: {hermes-bots: {}}`.
- **Bot Screen:** each bot can get its own Xfce desktop on a Linux host, streamed into Desktop so a human can take over
  for login, 2FA, CAPTCHA or payment.

## 4. Group chats with multiple bots (Telegram/Discord)

Put each profile's bot in one group and stop them talking in circles:

```yaml
telegram:
  require_mention: true
  exclusive_bot_mentions: true
  bots_require_mention: true
gateway:
  bot_loop_guard: {enabled: true, max_events: 20, window_seconds: 300, cooldown_seconds: 600}  # the defaults
```
Plus `TELEGRAM_ALLOW_BOTS=mentions` in `.env`. Discord has `DISCORD_ALLOW_BOTS` and inline-mention handoffs.

## 5. Across machines

**`hermes peer`** (Hermes ↔ Hermes)
```bash
hermes peer add spark --url http://spark.lan:8377 --key <peer's API_SERVER_KEY>   # key stored in ~/.hermes/.env
hermes peer dm spark "status?"
hermes peer run spark "..." --idempotency-key weekly-report-2026-10-12
hermes peer status | stop
```
Then `message_agent(target="spark/researcher")`. NAT is one-way: laptop → VPS works; VPS → home needs Tailscale/VPN.

**A2A** (Hermes ↔ anything)
- Outbound: `hermes tools enable a2a --platform …` → `a2a_discover`, `a2a_call`, `a2a_orchestrate` (all/first/best).
- Inbound: `gateway.platforms.a2a` on port 9900; Agent Card at `/.well-known/agent-card.json`.
- No token → localhost only. Remote exposure needs `A2A_BEARER_TOKEN` or `A2A_PEER_TOKENS` **and** `A2A_HOST`.
  Rate limit 60/min, HMAC-signed push webhooks, audit log `~/.hermes/a2a_audit.jsonl`.
- `A2A_MAX_PINGPONG_TURNS` defaults to 5 (hard max 20). Long tasks: raise `A2A_REPLY_TIMEOUT` (300s) or use push.

**Kanban does not span hosts.** Use one board per machine, and peers/A2A between machines.

## 6. Blueprint: a "pod" (one orchestrator + N employees)

INFERRED synthesis of the native pieces above:

```
one machine (VPS / cloud computer / Docker)
└── one multiplexed gateway
    ├── profile: boss       ← orchestrator. toolsets: kanban, memory, gateway. Strong model.
    ├── profile: sdr        ← employee. own SOUL, skills, bot token, cheap model
    ├── profile: content    ← employee
    ├── profile: ops        ← employee
    └── profile: qa         ← independent checker (reviews other employees' cards)

work:      boss creates kanban cards → employees claim → complete / request_review / block
reporting: notify+wake subscriptions → boss; cron "[bot:x] daily report" → deliver: bot-chat:boss
team chat: one Slack/Telegram group, all bots, mention-only + loop guard
human:     you DM any bot directly, or talk to boss in its Bot Chat
cross-pod: hermes peer / A2A between machines
template:  each employee is a profile distribution (git repo) → `hermes profile install … --name client-x-sdr`
```

What still needs your glue (INFERRED): routing a human's group-chat request into cards (orchestrator SOUL/prompt), daily
roll-ups (cron prompts you write), cost reporting per employee, and onboarding interviews. External CLIs (Codex/Claude
Code) as kanban workers are "not yet a paved path."

## Footguns

- **Spend multiplies:** depth 3 × 3 children = 27 leaves. Every loop tick and bot-chat delivery is a full turn.
- `/goal`'s judge **fails open** — the turn budget (`goals.max_turns`, 20) is the real backstop.
- Two gateways with `kanban.dispatch_in_gateway: true` on one board race for claims. Exactly one dispatcher.
- Worker exit codes: 75 = rate-limited (requeued), 78 = bad credential/model (blocked first try).
  3 protocol violations auto-block a card. Linking a support card under a blocked parent deadlocks both.
- systemd installs need `loginctl enable-linger` or workers die with your SSH session.
- Delivery is at-least-once. Don't resend `message_agent`/`peer dm` when the outcome is unknown — use idempotency keys.
- Unattended approvals default to deny; subagents never get an interactive approval prompt.
- Cron jobs can't create cron jobs unless `cron.allow_agent_scheduling: true`.
- Bot-room members need history compression or they start returning empty replies.
