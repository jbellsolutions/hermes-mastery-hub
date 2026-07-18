# Module 06 — Business Roles (build the operator + a second role)

## Outcome
You've turned at least two role cards into real Hermes skills and run each role's first workflow.

## Required reading
- ../business-roles/README.md  (the 13 roles + recommended build order)
- ../business-roles/operator.md  (build this first)
- ../templates/role-card.md, ../templates/SKILL.md, ../templates/business-intake.md

## Exercise
1. **Intake:** fill `../templates/business-intake.md` with your real business context.
2. **Pick the operator** (always first). Read `../business-roles/operator.md`.
3. **Convert to a skill:** copy `../templates/SKILL.md` → `~/.hermes/skills/business/operator/SKILL.md`.
   Set `name: operator`, write the "Use when …" description from the card, paste Mission /
   Approval Gates / Example Prompts into the body. `/reload-skills`.
4. **Test the trigger:** say one of the operator's example prompts; confirm the skill loads.
5. **Run its first workflow:** follow `../examples/01-lead-to-payment.md` (test mode).
6. **Second role:** repeat for **email-reply** (Example 03) or **cold-email** (Example 02) —
   whichever removes your biggest daily pain.
7. **Patch:** fix each skill with any pitfall you hit during its first workflow.

## Artifact
- Two role skills live (operator + one more), each with its first workflow run and gated.

## Verification checklist
- [ ] `hermes skills list` shows your two business skills
- [ ] Each triggers on its example prompts
- [ ] Each ran its first workflow with every side effect approval-gated
- [ ] You patched at least one skill after testing

## Coach notes
A role is just a skill. Start every role on ONE agent; only promote to a profile/container
(Control Room Level 2–3) when it needs isolated credentials/schedule/memory. Honesty rule: if
an integration isn't wired, the role says "wire X first" — it doesn't fake the capability.

## Next action
Proceed to Module 07 — Webhooks, Cron & MCP.
