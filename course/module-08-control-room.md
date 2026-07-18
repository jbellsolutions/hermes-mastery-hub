# Module 08 — Multi-Agent Control Room (ADVANCED — optional)

> ⚠️ **Do NOT start here, and don't feel behind if you skip it.** Most solo operators run
> their entire business on ONE agent with multiple skills. Reach for a Control Room only when
> one agent genuinely isn't enough (multiple distinct roles needing isolated credentials,
> schedules, and memory; or you want parallelism). Level 1 first, always.

## Outcome
You understand the four-level model and can articulate when (and whether) to scale beyond one agent.

## Required reading
- ../reference/10-multi-agent-control-room.md
- ../case-studies/02-agent-control-room.md
- ../business-roles/orchestrator.md

## Exercise (mostly conceptual — build only if you've outgrown one agent)
1. Read the four levels. Honestly place yourself: almost certainly **Level 1–2**.
2. If you have 3+ distinct roles with separate credentials/schedules, sketch a Control Room:
   list agents (slugs), their allowed/forbidden work, and where secrets live (names only).
3. If you want to try it: use profiles as lightweight specialists first —
   `hermes profile create cmo --clone --description "CMO specialist"` — before containers.
   Profile names are lowercase alphanumeric; `hermes-cmo` remains the human-facing Control
   Room slug if you later promote it to a container.
4. If you reach Level 3, run one task through the orchestrator loop:
   `../examples/07-control-room-task-routing.md`.

## Artifact
- Either: a written "I'm Level N and here's why" note (most people), OR a Control Room sketch
  with agent slugs + allowed work + secret names (no values).

## Verification checklist
- [ ] You can name your current level and justify it
- [ ] If scaling: each agent has a slug, allowed/forbidden work, and a secret map (names only)
- [ ] No raw secrets anywhere; no two gateways on one data dir

## Coach notes
The anti-pattern is one mega-agent hoarding every credential. The Control Room *governs*;
specialists *own* their tools. Automate/scale only after the manual workflow works.

## Next action
Proceed to Module 09 — Capstone.
