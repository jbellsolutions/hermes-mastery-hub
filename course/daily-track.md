# The Daily Track — 30 lessons, one a day

The weekend roadmap gets you running. This track makes you an expert: one focused lesson a day, each ending in one
thing you actually do on a real agent. Lessons are ordered; skip one only if you can already pass its check.

Each lesson = **Learn** (5 min, the reference section) → **Do** (≤20 min) → **Check** (how you know it worked).
A scheduled task can deliver one lesson per day; log results in `../agent-feed/DAILY_LEARNING_LOG.md`.

## Week 1 — Foundations, refreshed

| # | Lesson | Do | Check |
|---|---|---|---|
| 1 | What changed since July | Read ref 14; run `hermes --version`, `hermes config check` | You can name 5 wrong-now defaults |
| 2 | Update safely | `hermes update --check`, `--plan`, set `pre_update_backup: full` | Update receipt exists |
| 3 | Config vs secrets vs SOUL | Read `which-file-does-what`; move any key out of `config.yaml` | `grep -i key config.yaml` is clean |
| 4 | Approvals | Set `smart` vs `manual`; add 3 `approvals.deny` rules | A denied command fails in YOLO |
| 5 | Skills, the modern way | `/learn` a skill from one of your SOPs; chain two skills | It triggers from natural language |
| 6 | Shared skills | Set `skills.external_dirs` to one shared folder on two profiles | Both profiles list the same skill |
| 7 | Curator | `hermes curator status`, `adopt`, `pin` your keepers | You know what will archive next |

## Week 2 — Profiles & deployment

| # | Lesson | Do | Check |
|---|---|---|---|
| 8 | Profiles = employees | Create 2 profiles with distinct SOUL + model | Isolation table from ref 15 explained in your words |
| 9 | One gateway, many bots | Host gateway + one bot per profile | `gateway status` serves both |
| 10 | Shared bot routing | `gateway.profile_routes` on one Slack/Discord bot | Two channels hit two profiles |
| 11 | Distributions | Package a profile as a repo; install it under a new name | `.env.EXAMPLE` written, no secrets shipped |
| 12 | Deploy to a VPS | Non-root user, linger, journald logs | Survives SSH logout and a reboot |
| 13 | Cloud computer + Bot Screen | Bot Screen on a Linux box; human takeover for a login | Agent completes a browser task after takeover |
| 14 | Desktop multi-connection | Add local + remote + SSH connections | "Update all instances" dry run |

## Week 3 — Teams

| # | Lesson | Do | Check |
|---|---|---|---|
| 15 | Delegation | Fan out 3 subagents with `output_schema` | Structured results; you steered one mid-run |
| 16 | Kanban basics | Orchestrator creates cards for 2 employees | Cards complete with no protocol violations |
| 17 | Review & QA | Add a `qa` profile; `request_review` flow | QA catches one real issue |
| 18 | Wake the boss | `notify+wake` subscriptions; bot-chat cron reports | Orchestrator summarizes without being asked |
| 19 | Group chat without chaos | Mention-only + loop guard in one team group | Bots never reply to each other unprompted |
| 20 | Across machines | `hermes peer add` / `peer run` with idempotency key | A long run on box B reported back to box A |
| 21 | A2A | Expose one agent with a bearer token; call it from another | Audit log shows the call |

## Week 4 — Extension, security, scale

| # | Lesson | Do | Check |
|---|---|---|---|
| 22 | MCP done right | Add one server with `include` filters + `lazy`; turn on Tool Search | Context stays small with 5+ servers |
| 23 | Plugins | Install one catalog plugin; read capabilities + scan | You can explain what it's allowed to do |
| 24 | Hooks | A shell hook that blocks a risky tool call (exit 2) | Block fires and logs |
| 25 | Secrets & vault | 1Password/Bitwarden source + one vault login | Password never appears in transcript |
| 26 | Egress | iron-proxy on a Docker-backend agent | Unknown host denied |
| 27 | Ops | Backups, OTLP or cron health checks, `cron doctor` | An alert fires on a forced failure |
| 28 | Cost control | Turn caps, run budget, worker models, per-card models | A week's spend per profile you can explain |
| 29 | Claude in Hermes | Opus planner + cheap workers; observe prompt-cache behavior | Cache hits stay high across a long session |
| 30 | Capstone: ship a pod | One orchestrator + 3 employees + QA, templated as distributions | Installs fresh on a new box in under an hour |

After day 30, repeat weeks 3–4 on a real client build, and keep the weekly release check running.
