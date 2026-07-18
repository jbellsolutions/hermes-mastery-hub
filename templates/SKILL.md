---
name: your-skill-name
description: "Use when <the exact trigger situation>. <One sentence on what it does>. This description is how Hermes decides WHEN to load the skill — make the trigger unambiguous."
version: "1.0.0"
author: Your Name
license: MIT
metadata:
  hermes:
    tags: [tag1, tag2, tag3]
    related_skills: [other-skill, another-skill]
---

# Skill Title — human-readable name

<!--
  This is the CANONICAL Hermes skill template. It matches the verified frontmatter used in
  hewi333/Mom-n-Pop-Skills and shannhk/hermes-agent-control-room. A skill is Markdown + YAML
  frontmatter in a SKILL.md file. Do NOT add invented fields (no `triggers:`, no `tools:`, no
  TOML/JSON). Optional supporting files live in references/, templates/, scripts/, assets/.
  Save this at ~/.hermes/skills/<category>/<your-skill-name>/SKILL.md then run /reload-skills.
-->

## Overview
One short paragraph: what this skill does and for whom.

## When to Use
- Bullet the concrete situations that should trigger this skill.
- Mirror these in the `description` above.

## Don't Use For
- The adjacent cases this skill is NOT for (prevents misfires).

## Prerequisites
- Exact env vars needed (e.g. `STRIPE_SECRET_KEY` in `~/.hermes/.env`).
- Accounts / API keys / MCP servers that must be wired first.
- Be honest: if it isn't wired, the skill can't do it.

## The Flow (numbered, with exact commands)
1. Step one — what the agent does. Include the literal command/tool call.
2. Step two — …
3. **Approval gate** — before any real-world side effect (send/spend/publish/delete), show
   the user exactly what will happen and wait for an explicit "yes".
4. Execute only after approval.

## Agent Integration Patterns
- **User says:** "…"  → **Agent does:** …
- **User says:** "…"  → **Agent does:** …

## Common Pitfalls
1. The scar-tissue lessons. (e.g. "Amounts are in cents: $3,300 = 330000.")
2. …

## Verification Checklist
- [ ] Prerequisite env vars set
- [ ] Trigger fires on the intended phrasing
- [ ] Each step produces the expected real output
- [ ] Approval gate blocks side effects until "yes"

## One-Shot Recipes
### "Common request phrasing"
```bash
# copy-paste starting point
```
