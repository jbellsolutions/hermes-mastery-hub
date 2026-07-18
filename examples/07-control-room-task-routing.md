# Example 07 — Control Room Task Routing (orchestrator → specialist)

*Grounded in `../reference/10-multi-agent-control-room.md`, `../business-roles/orchestrator.md`,
and the Control Room task-bus templates. **Advanced — only relevant at Level 3.** On a single
agent, the equivalent is `delegate_task`.*

## Goal
Route one task from an orchestrator front door to the right specialist, enforce approval gates,
collect the result, and synthesize a final answer.

## Prereqs
- At least two specialist agents exist. Containers may use slugs such as `hermes-dev` and
  `hermes-cmo`; Hermes profiles use alphanumeric names such as `dev` and `cmo`.
- A task bus (Control Room `inbox/working/outbox/archive`) OR native Hermes `kanban`.
- A Control Room registry listing each agent's allowed/forbidden work.

## Workflow map
```
You → orchestrator → write task brief (with approval gates) → route via task bus/kanban →
specialist works → returns findings/artifacts/risks → 🚦approve any publish/deploy/destructive →
orchestrator synthesizes → you
```

## Step-by-step
1. "Route this to the right specialist and synthesize: 'fix the checkout bug on the site.'"
2. Orchestrator reads the registry, picks `hermes-dev`, and writes a task brief using
   `../templates/task-brief.md` with:
   ```yaml
   requires_approval_before: [publishing, pushing commits, deploying, destructive operations]
   ```
3. Brief lands in the task bus (`kanban create` or `inbox/`). The specialist claims it.
4. Specialist does the work in its own isolated runtime and returns: findings, files changed
   (paths), risks/assumptions, recommended next action — **without** pushing/deploying.
5. **🚦 Gate:** you approve the commit/deploy. Only then does the specialist push.
6. Orchestrator reviews the result and synthesizes a final summary back to you.

## Approval gates (must fire)
- Before any publish, commit push, deploy, or destructive operation (encoded in the brief).

## Verification checklist
- [ ] Orchestrator routed to the correct specialist per the registry
- [ ] Task brief included `requires_approval_before`
- [ ] Specialist returned findings/artifacts/risks and did NOT auto-publish
- [ ] You approved before any push/deploy
- [ ] Orchestrator synthesized a clear final answer

## Common pitfalls
- Orchestrator hoarding all credentials → anti-pattern; specialists own their tools.
- Two gateways on one data dir → never. One container/profile per long-running agent.
- Starting here → don't. Prove single-agent + skills first (Level 1–2).
