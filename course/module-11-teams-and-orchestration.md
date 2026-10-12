# Module 11 — Teams: Orchestrator + Employees

## Outcome
A working "pod": one orchestrator profile that assigns kanban cards to two employee profiles, an independent QA check,
and reports that wake the orchestrator — all visible in one team chat.

## Required reading
- ../reference/17-teams-and-orchestration.md
- ../business-roles/orchestrator.md
- ../templates/task-brief.md

## Exercise
1. Create `boss`, `content`, `qa` profiles (reuse `ops`/`sdr` from Module 10 if you like). Write each a one-paragraph
   `hermes profile describe <name> --text "..."` — the orchestrator routes by these.
2. Enable kanban for the orchestrator only: `hermes -p boss tools enable kanban --platform slack` (or telegram).
3. `hermes kanban init`. Ask `boss`: "Create a card for content to draft 3 LinkedIn posts about X, then a card for qa
   to review them." Watch with `hermes kanban watch`.
4. Subscribe `boss` to completions with `notify+wake` delivery so it takes a real turn when cards finish.
5. Put all bots in one group chat with mention-only settings and the loop guard (see reference 17 §4).
6. Add a routine: a cron job named `[bot:content] daily report` delivering to `bot-chat:boss`.
7. Delegation contrast: in one chat, have `boss` fan out 3 research subagents with `delegate_task`. Note that these
   vanish on restart and kanban cards don't.
8. (Cross-machine) If you have a second box: `hermes peer add` and send one `peer dm`.

## Artifact
- A board with at least one completed card and one card that went through review.
- A team group chat where bots only speak when mentioned.

## Verification checklist
- [ ] Each worker run ended with complete / request_review / block (no protocol violations in `hermes kanban diagnostics`)
- [ ] `boss` was woken by a completion and summarized it without you asking
- [ ] QA caught at least one issue, and the fix was written back into a skill
- [ ] Spend caps set: `max_in_progress_per_profile`, a worker model, `agent.max_turns` or `run_budget_seconds`

## Coach notes
Stamp every shared decision into each card body — workers can't see sibling cards. Only one gateway may dispatch a
board. Kanban is single-host; between machines use peers or A2A.

## Next action
Proceed to Module 12 — Security & Ops.
