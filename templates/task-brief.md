# Task Brief Template

> Derived from the Control Room `task-template.md`. Use when an orchestrator (or you) hands a
> discrete task to a specialist agent/role. The `requires_approval_before` list is the safety contract.

```yaml
---
task_id: TASK-YYYY-MM-DD-001
created_at: YYYY-MM-DDTHH:MM:SSZ
created_by: you            # or hermes-orchestrator
assigned_to: <role-or-agent-slug>
priority: normal           # low | normal | high | urgent
status: ready              # ready | working | blocked | done
requires_approval_before:
  - sending outbound messages
  - spending money / launching ads
  - publishing or deploying
  - destructive operations
---
```

## Task
Describe the task in one or two sentences. Be specific about the desired outcome.

## Context
Relevant background, file paths, accounts, prior decisions. Assume the specialist knows
nothing about your other conversations.

## Constraints
- Do not expose secrets.
- Do not perform any `requires_approval_before` action without explicit approval.
- Stay within this role's scope (see its role card).

## Expected Output
- Findings
- Files changed / artifacts created (with paths or URLs)
- Risks and assumptions
- Recommended next action
