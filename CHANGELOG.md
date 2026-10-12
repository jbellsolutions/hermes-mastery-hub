# Changelog

## v2.0.0 — 2026-10-12 (targets Hermes Agent v0.21.7)

Refresh from Hermes v0.18.0 (July 1) to v0.21.7 (October 11): 19 upstream releases.

**New reference chapters**
- `reference/14-whats-new-since-july.md` — release timeline, wrong-now defaults, breaking config, Claude-specific notes
- `reference/15-profiles-deep-dive.md` — isolation table, commands, multiplexed gateway, distributions, cross-machine
- `reference/16-deployment-playbook.md` — targets matrix, VPS recipe, channels/update/rollback, security, ops, cost guardrails
- `reference/17-teams-and-orchestration.md` — delegation, kanban, Bot Mode, group chats, `hermes peer`, A2A, pod blueprint
- `reference/18-plugins-mcp-and-extension-map.md` — skill vs MCP vs plugin vs hook, plugin catalog, Tool Search
- `reference/19-edge-cases-dos-and-donts.md` — do/don't lists and a symptom → cause → fix table

**New course material**
- Modules 10 (profiles & deployment), 11 (teams & orchestration), 12 (security & ops)
- `course/daily-track.md` — 30 one-a-day lessons

**New templates and cards**
- `templates/profile-distribution/` — a ready-to-copy distribution (manifest, SOUL, config, .gitignore, cron)
- `personal-use-cases/always-on-personal-agent.md`
- `maintenance/WEEKLY_UPDATE_CHECK.md`

**Corrections to v1.0**
- `approvals.mode` default is `smart` (was documented as `manual`); unattended surfaces default to deny
- Delegation defaults: 10 concurrent children, 250 iterations, async by default
- `agent.max_turns` now defaults to unlimited
- Profile names allow hyphens and underscores (v1.0 said alphanumeric only)
- PyPI/Homebrew installs retired; Tirith scanner removed; curator 14/30 days
- One multiplexed gateway per machine replaces per-profile gateway services

## v1.0.0 — 2026-07-18 (targeted Hermes Agent v0.18.0)

Initial release: 14 reference docs, 9-module weekend course, 3 case studies, 13 business roles, 7 personal use
cases, 6 templates, 8 examples, agent-feed coach.
