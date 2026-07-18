# Module 02 — Skills (the learning loop)

## Outcome
You can install a skill, load it, and author your own `SKILL.md` from the template.

## Required reading
- ../reference/04-skills-system.md
- ../templates/SKILL.md

## Exercise
1. See what's installed: `hermes skills list`.
2. Search the hub: `hermes skills search crm` (or any topic).
3. Install a real pack to study the format:
   ```bash
   git clone https://github.com/hewi333/Mom-n-Pop-Skills.git /tmp/mnp
   cp -r /tmp/mnp/skills/* ~/.hermes/skills/productivity/
   ```
   Then `/reload-skills` and read one with `/skill crm-lite`.
4. Author your own: copy `../templates/SKILL.md` to
   `~/.hermes/skills/business/hello-skill/SKILL.md`, set `name: hello-skill`, write a
   "Use when …" description, keep the body tiny.
5. `/reload-skills`, then trigger it by saying the situation in your description.

## Artifact
- One custom `SKILL.md` that loads and triggers.

## Verification checklist
- [ ] `hermes skills list` shows the installed pack
- [ ] `/skill crm-lite` displays the skill body
- [ ] Your `hello-skill` appears after `/reload-skills`
- [ ] Saying the trigger phrase loads your skill

## Coach notes
Skills are **Markdown + YAML frontmatter** — never TOML/JSON/Python. The `description` is the
trigger; make it unambiguous ("Use when …"). Facts go in memory; procedures go in skills.

## Next action
Proceed to Module 03 — Memory.
