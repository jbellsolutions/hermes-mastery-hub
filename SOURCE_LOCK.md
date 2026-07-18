# Source Lock — v1.0.0 research snapshot

This file records the exact source snapshot used to build the first release of the Hermes Mastery Hub.

## Build environment

- **Research/build timestamp (UTC):** `2026-07-18T03:51:10Z`
- **Observed Hermes CLI:** `Hermes Agent v0.18.0 (2026.7.1)`
- **Installed local Hermes checkout:** `7c1a029553d87c43ecff8a3821336bc95872213b`
- **CLI-reported upstream revision:** `d59b79fadd1e9edd7afc5c679cc3b143838e7c01`
- **Official source-lock revision used below:** `d59b79fadd1e9edd7afc5c679cc3b143838e7c01`
- **Python:** `3.11.15`
- **Platform:** macOS 15.6

The installed local checkout and the official locked revision differed. The CLI also reported
that its checkout was behind newer upstream work. Consequently, every version-sensitive
command should be checked with `hermes <command> --help` on the machine where it will run.

## Commit-pinned repositories

| Source | Locked revision | Permanent source link |
|---|---|---|
| Nous Research — Hermes Agent | `d59b79fadd1e9edd7afc5c679cc3b143838e7c01` | https://github.com/NousResearch/hermes-agent/tree/d59b79fadd1e9edd7afc5c679cc3b143838e7c01 |
| Mom-n-Pop Skills | `67f2f66831eae1191e8bfa4feda25a03ec28e994` | https://github.com/hewi333/Mom-n-Pop-Skills/tree/67f2f66831eae1191e8bfa4feda25a03ec28e994 |
| Hermes Agent Control Room | `48a1a5a2c3a64416f51b0199a1acc9aba05e6261` | https://github.com/shannhk/hermes-agent-control-room/tree/48a1a5a2c3a64416f51b0199a1acc9aba05e6261 |

## Live sources intentionally not pinned

These are intentionally linked live because they are dynamic service/documentation surfaces rather than immutable repository snapshots:

- Hermes documentation: https://hermes-agent.nousresearch.com/docs
- Skills Hub: https://hermes-agent.nousresearch.com/docs/skills
- Nous Research Discord invite: https://discord.gg/NousResearch
- GitHub issues: https://github.com/NousResearch/hermes-agent/issues

For reproducible fact-checking, prefer the locked repository revisions above. For current setup instructions and commands, prefer live docs plus the local CLI help.

## Installed local skill snapshots used

The build also consulted installed skills, especially `hermes-agent` v2.3.0 and its linked `native-mcp.md` and `webhooks.md` references. These installed files were not imported into this repository and are not independently commit-pinned here. Claims derived from them are labeled as machine-specific guidance and should be checked against the locked Hermes source or current CLI.

Other installed skills consulted only for scope/behavior guidance included `computer-use`, `retriever`, `hermes-agent-skill-authoring`, `hermes-remote-db`, and `himalaya`.

## Inaccessible source

Tony Simons’ X article body (`https://x.com/i/article/2077912657739612160`) was login-gated. Only the publicly visible post card/title was used. The hub does not claim to summarize the inaccessible article.

## Updating this lock

When refreshing the hub:

1. Record a new UTC timestamp and `hermes --version` output.
2. Resolve each repository’s exact commit SHA.
3. Re-run CLI-help, link, frontmatter, and secret validation.
4. Add a new release note rather than silently rewriting the historical snapshot.
