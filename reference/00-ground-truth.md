# 00 — Ground Truth & Source Discipline

> **Read this first.** This file is the anti-hallucination contract for the entire hub.
> Hermes feature, CLI, and source-repository claims trace to `../SOURCES.md`. Course structure,
> templates, role cards, personal use cases, assessments, examples, and recommendations are
> original synthesis labeled as guidance rather than source facts.

## The rule

If a fact about Hermes Agent is **not** present in one of the verified sources, this hub
does not assert it. When something is uncertain, it is labeled **UNVERIFIED** and you are
told how to check it yourself (`hermes --help`, `hermes <cmd> --help`, the official docs,
or the source tree at `github.com/NousResearch/hermes-agent`).

This matters because the AI ecosystem is full of confident-sounding fabrication. During this
build, unverified draft material used names such as "OpenAGI", "TEN", and "Nous-PSY", plus
unsupported skill formats (TOML/JSON/Python packages). Those names and formats were **not
verified as Hermes features and were not used as sources for this hub**. Feeding unverified
claims to your agent creates painful failures because it may confidently act on a false premise.

## What IS verified (and used throughout this hub)

| Source | What it grounds |
|---|---|
| Installed `hermes-agent` skill (v2.3.0) on this machine | CLI reference, slash commands, config sections, toolsets, providers, durable systems |
| `github.com/NousResearch/hermes-agent` README | Product positioning, install commands, terminal backends |
| `hermes-agent.nousresearch.com/docs` | Docs structure and canonical links |
| `hewi333/Mom-n-Pop-Skills` (MIT) | Small-business owner-gated skill pattern, real SKILL.md files |
| `shannhk/hermes-agent-control-room` (MIT) | Multi-agent Control Room architecture, task bus, level model |
| Installed skill's linked references (`native-mcp.md`, `webhooks.md`) | MCP client + webhook subscription mechanics |

## The one skill-format fact everything depends on

A Hermes **skill** is a directory containing a `SKILL.md` file. `SKILL.md` is **Markdown with
YAML frontmatter** — not TOML, not JSON, not a Python package. Verified frontmatter shape:

```markdown
---
name: crm-lite
description: "Use when tracking customer interactions... (this is the trigger text)"
version: "1.0.0"
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [crm, sqlite, small-business]
    related_skills: [estimator-engine, stripe-payments]
---

# CRM-Lite — human-readable title

## Overview
...markdown body...
```

Skills may carry supporting files in `references/`, `templates/`, `scripts/`, and `assets/`
subdirectories. That is the entire format. Anyone who tells you a skill is a `.toml` or a
`.py` plugin manifest is describing something other than a Hermes skill.

## The one behavioral fact everything depends on

Hermes is a **self-improving** agent: after solving something hard, it can persist the
approach as a new skill, and it maintains **persistent memory** and **session search**
across conversations. This is why the whole hub is designed to be *fed to your agent* —
you are not just reading docs, you are loading operating doctrine your agent will reuse.

## How to challenge this hub

If any file here contradicts what `hermes --help` or the live docs say, **the live source
wins** and you should patch the hub. Versions move. This hub is a snapshot with citations,
not a replacement for the source of truth.
