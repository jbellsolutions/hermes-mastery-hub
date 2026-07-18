# 01 — What Hermes Agent Actually Is

*Source: official README (`NousResearch/hermes-agent`), docs landing page, installed `hermes-agent` skill v2.3.0.*

Hermes Agent is an open-source AI agent framework by **Nous Research**. It runs in your
terminal, a native desktop app, messaging platforms, and IDEs. It is in the same category
as Claude Code and OpenAI Codex — an autonomous, tool-calling task-execution agent — but
with a design center that is different in four ways that matter for this hub.

## The four things that make Hermes different

1. **Self-improving through skills.** When Hermes solves a complex problem, discovers a
   workflow, or gets corrected, it can persist that knowledge as a *skill* document that
   loads into future sessions. Skills accumulate, so the agent gets better at *your*
   specific tasks and environment over time. This is the single most important feature for
   the business use cases in this hub — your roles literally become skills the agent owns.

2. **Persistent memory across sessions.** It remembers who you are, your preferences,
   environment details, and lessons learned. Pluggable backends (built-in, Honcho, Mem0).

3. **Multi-platform gateway.** The *same* agent runs on Telegram, Discord, Slack, WhatsApp,
   iMessage, Signal, Matrix, Teams, Email, SMS and more — with full tool access, not just
   chat. This is what let the Mom-n-Pop hackathon team run a real business over Telegram.

4. **Provider-agnostic.** Swap models/providers mid-workflow with `hermes model` — no code
   changes. 20+ providers (Nous Portal, OpenRouter, Anthropic, OpenAI, Google, DeepSeek,
   xAI, GLM/Z.AI, MiniMax, Kimi, local, and more). Credential pools rotate across keys.

## Surfaces (the same agent core, many front doors)

- **CLI / TUI** — `hermes` (interactive), `hermes chat -q "..."` (one-shot)
- **Native desktop app** — `hermes desktop` (alias `hermes gui`)
- **Web dashboard** — `hermes dashboard` (admin panel + embedded chat)
- **Gateway** — `hermes gateway run` (messaging platforms)
- **ACP server** — `hermes acp` (VS Code / Zed / JetBrains)
- **OpenAI-compatible proxy** — `hermes proxy` (point Aider/Cline/Codex at your OAuth model)

## Where it runs

Six terminal backends: **local, Docker, SSH, Singularity, Modal, Daytona**. Modal and
Daytona give serverless persistence — the environment hibernates when idle and wakes on
demand. Practical translation: you can run an always-on business agent on a $5 VPS, talk to
it from your phone over Telegram, and it keeps working while your laptop is closed.

## The mental model for this hub

> Hermes is not a chatbot you prompt. It is an **operator** you employ.
> You give it roles (skills), tools (toolsets + MCP), a place to live (a backend + gateway),
> a schedule (cron), event triggers (webhooks), and guardrails (approval gates). Then it
> runs your workflows — drafting work, asking for approval on anything with real-world
> side effects, and executing once you say go.

Everything else in `reference/` unpacks one piece of that model. Read them in order:

- `02-installation-and-setup.md` — get a working agent
- `03-configuration-and-profiles.md` — models, toolsets, profiles, security toggles
- `04-skills-system.md` — the learning loop and how to author skills
- `05-memory-and-learning-loop.md` — memory, session search, curator
- `06-gateway-and-messaging.md` — run it on Telegram/Discord/Slack
- `07-webhooks.md` — event-driven agent runs
- `08-native-mcp.md` — connect external tool ecosystems
- `09-cron-and-durable-systems.md` — scheduling, delegation, kanban
- `10-multi-agent-control-room.md` — scale from one agent to a team
- `11-best-practices.md` — the operating doctrine
- `12-troubleshooting.md` — when things break

## UNVERIFIED / out of scope

The "Hermes Agent Master Class" article series by Tony Simons on X is a real, referenced
resource (the public card confirms *"Part 9: Browser and Computer Use"*), but the article
body is behind an X login wall and was **not** readable when this hub was built. This hub
therefore does **not** reproduce or paraphrase its contents. Treat it as a recommended
external read, not a source this hub is derived from. See `case-studies/03-master-class-series.md`.
