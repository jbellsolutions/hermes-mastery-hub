# 04 — The Skills System (the learning loop)

*Source: installed `hermes-agent` skill v2.3.0, real SKILL.md files from `hewi333/Mom-n-Pop-Skills` and `shannhk/hermes-agent-control-room`. Verify with `hermes skills --help`.*

Skills are Hermes's procedural memory. They are how the agent **learns** and how *you*
teach it your business. This is the most important chapter for the business use cases.

## What a skill is (the only correct answer)

A skill is a directory with a `SKILL.md` file. `SKILL.md` is **Markdown with YAML
frontmatter**. Optional supporting files live in `references/`, `templates/`, `scripts/`,
`assets/` subdirectories.

```markdown
---
name: crm-lite
description: "Use when tracking customer interactions... — this text is the TRIGGER."
version: "1.0.0"
author: You
license: MIT
metadata:
  hermes:
    tags: [crm, sqlite, small-business]
    related_skills: [estimator-engine, stripe-payments]
---

# CRM-Lite — human title

## Overview / When to Use / Don't Use For / Steps / Pitfalls / Verification Checklist
```

The `description` is the most important line — Hermes reads it to decide *when* to load the
skill. Write it as "Use when …" so the trigger is unambiguous.

## Managing skills (CLI)

```bash
hermes skills list            # what's installed
hermes skills search QUERY    # search the hub
hermes skills install ID      # ID = hub identifier OR a direct https://…/SKILL.md URL
hermes skills inspect ID      # preview before installing
hermes skills browse          # browse all available
hermes skills update          # update outdated
hermes skills publish PATH    # publish your own to the registry
hermes skills tap add REPO    # add a GitHub repo as a skill source
```

In-session: `/skill <name>` loads one; `/skills` searches/installs; `/reload-skills`
re-scans `~/.hermes/skills/` after you add files.

**New since v1.0:** `/learn <dir|url|notes>` writes a skill from material; chain up to 5 skills (`/a /b do X`);
`hermes bundles` (one command loads several skills); `requires_apps:` frontmatter; project-local skills that load only
after `hermes skills trust`; `skills.external_dirs` / `skills.create_dir` for a **shared team skill folder**; install-time
security scans. Full map: `18-plugins-mcp-and-extension-map.md`.

## Installing a whole pack (how the case-study repos are used)

```bash
# Mom-n-Pop small-business pack (MIT)
git clone https://github.com/hewi333/Mom-n-Pop-Skills.git
cp -r Mom-n-Pop-Skills/skills/* ~/.hermes/skills/productivity/
# then add credentials to ~/.hermes/.env per each skill's Prerequisites
```

## The self-improving loop (what makes Hermes special)

1. You (or the agent) hit a non-trivial task.
2. The agent solves it — sometimes after correction.
3. The agent (or you) persists the approach as a skill.
4. Next time, that skill loads automatically and the agent is instantly better.

The **Curator** maintains agent-created skills in the background: tracks usage, marks idle
ones stale after **14 days**, archives (never deletes) after **30 days**, and logs every change to
`.curator_ledger.jsonl` (`hermes curator rollback <id>` undoes one). It only touches skills with
`created_by: agent` provenance (background review) — your hand-authored, `/learn`-made and hub-installed
skills are off-limits unless you `hermes curator adopt` them. CLI: `hermes curator <status|run|pin|unpin|archive|restore>`. Pin the skills you
never want auto-touched: `hermes curator pin <name>`.

## Anatomy of a great skill (learned from the verified examples)

Every strong SKILL.md in the source repos shares this shape:

- **Overview** — one paragraph on what it does.
- **When to Use / Don't Use For** — sharpens the trigger.
- **Prerequisites** — exact env vars and accounts.
- **The Flow / Steps** — numbered, with exact commands.
- **Agent Integration Patterns** — "User says X → Agent does Y".
- **Common Pitfalls** — the scars. (e.g. *"Stripe amounts are in cents: $3,300 = 330000"*.)
- **Verification Checklist** — checkboxes proving it works.
- **One-Shot Recipes** — copy-paste starting points.

Use `../templates/SKILL.md` in this hub as the canonical starting point. See
`software-development/hermes-agent-skill-authoring` (installed skill) for the in-repo validator
and writing-quality principles.

## Turning a business role into a skill

This is the payoff. Each file in `business-roles/` ends with "convert this into a Hermes
skill" instructions: take the role's mission + allowed/forbidden work + approval gates +
example prompts, drop them into `../templates/SKILL.md`, save under
`~/.hermes/skills/business/<role>/SKILL.md`, and `/reload-skills`. Now the agent *is* that role.
