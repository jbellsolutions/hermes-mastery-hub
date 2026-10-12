# SOURCES

Hermes feature, CLI, and source-repository claims in this hub trace to the sources below.
Course sequencing, templates, role cards, personal use cases, assessments, examples, and
recommendations are original hub-authored synthesis informed by those sources—not direct
claims copied from them. Provenance is separated by type so you can weigh confidence correctly.

For exact repository revisions and the observed build environment, see [`SOURCE_LOCK.md`](SOURCE_LOCK.md).

## A. Official Nous Research (highest authority)

- **Hermes Agent docs** — https://hermes-agent.nousresearch.com/docs
  Docs landing, structure, install commands, product positioning, canonical links.
- **Hermes Agent source + README** — https://github.com/NousResearch/hermes-agent
  Product positioning, install one-liners, terminal backends, feature summary.
- **Skills Hub** — https://hermes-agent.nousresearch.com/docs/skills
  Skills catalog surface (verified to exist; catalog loads dynamically).

### Added for v2.0.0 (read at `main` @ ee36aed, 2026-10-12)

- `website/docs/user-guide/`: profiles, multi-profile-gateways, profile-distributions, which-file-does-what,
  managed-scope, multi-connection-desktop, bot-mode, docker, desktop, security, egress/*, secrets/*, messaging/a2a,
  messaging/relay, messaging/slack, messaging/telegram, features/{kanban*, delegation, cron, goals, loops, heartbeat,
  plugins, plugin-catalog, skills, curator, mcp, tool-search, hooks, credential-vault, computer-use, bot-screen}
- `website/docs/getting-started/`: installation, updating; `developer-guide/`: plugins/*, multiplexing-gateway,
  gateway-monitoring, stable-releases, state-db-recovery, middleware; `guides/`: secure-hermes-on-a-work-machine,
  manage-hermes-cloud-with-mcp, delegation-patterns, troubleshooting-agent-quality
- Source: `hermes_cli/config_defaults.py`, `hermes_constants.py`, `hermes_cli/subcommands/*.py`,
  `hermes_cli/kanban_parser.py`, `hermes_cli/config_migrations.py`, `plugin-catalog/`
- Release pages: https://github.com/NousResearch/hermes-agent/releases/tag/<tag> for v2026.7.7 … v0.21.7
- Hermes Cloud: https://portal.nousresearch.com/cloud

## B. Locally-installed Hermes skill on the build machine (high, machine-specific)

- **`hermes-agent` skill v2.3.0** (installed at build time; author "Hermes Agent + Teknium").
  Source for the CLI reference, slash commands, config sections, toolsets, providers, and the
  durable systems (cron/delegation/curator/kanban). Its linked references `native-mcp.md` and
  `webhooks.md` ground `reference/08` and `reference/07`.
  > **Caveat:** this is a snapshot of one machine's installed skill. Treat CLI *subcommands*
  > as "verify with `hermes <command> --help`," because versions move. The hub deliberately
  > phrases uncertain CLI behavior that way.
- Other installed skills referenced for scope honesty: `computer-use`, `retriever` (rtrvr MCP),
  `hermes-agent-skill-authoring`, `hermes-remote-db`, `himalaya`.

## C. Community repositories (MIT-licensed; user-provided examples)

- **`hewi333/Mom-n-Pop-Skills`** (MIT) — https://github.com/hewi333/Mom-n-Pop-Skills
  Owner-gated small-business skill pattern; real SKILL.md files (crm-lite, estimator-engine,
  lead-to-payment, stripe-payments, marketing-campaign-builder, agent-cheatsheet-builder).
  Grounds `case-studies/01` and much of the business-role doctrine.
- **`shannhk/hermes-agent-control-room`** (MIT) — https://github.com/shannhk/hermes-agent-control-room
  Multi-agent Control Room architecture, level model, task bus, naming, control-plane vs
  runtime split. Grounds `reference/10` and `case-studies/02`.

## D. Referenced but NOT accessible (explicitly not used as source material)

- **Tony Simons "Hermes Agent Master Class" series on X** —
  https://x.com/tonysimons_/status/2077923978249724073 (article: `.../i/article/2077912657739612160`)
  The public post card confirms the series exists and named *"Part 9: Browser and Computer
  Use."* The **article body is behind an X login wall** and was not readable at build time.
  This hub does **not** reproduce or paraphrase its contents. It is listed as a recommended
  external read in `case-studies/03-master-class-series-study-guide.md`, with a study guide
  built from the hub's own verified material — not from the article.

## How to re-verify

- CLI: `hermes --help`, `hermes <command> --help`, `hermes doctor`
- Docs: browse https://hermes-agent.nousresearch.com/docs
- Source: read https://github.com/NousResearch/hermes-agent
- Community repos: clone and read the SKILL.md files directly

If any hub claim contradicts a live source, the live source wins.
