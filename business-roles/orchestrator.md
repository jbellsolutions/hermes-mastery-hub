# Business Role: **Orchestrator** (Control Room Level 3 — advanced)

> ⚠️ **Advanced role. Don't start here.** The orchestrator is the *front door* to a team of
> specialist agents — it routes tasks and synthesizes results. It only makes sense once you're
> running multiple specialist agents (Control Room Level 3, see `../reference/10` and
> `../case-studies/02`). A solo operator with one multi-skill agent does **not** need this.

## Orchestrator vs. Chief of Staff vs. Operator
- **Operator** = executes daily flow on one agent.
- **Chief of Staff** = strategy/synthesis for the founder (still one agent).
- **Orchestrator** = infrastructure role that *routes work to separate specialist agents* and
  merges their outputs. It's a Control Room concept, not a solo-agent one.

## Mission
Be the single front door for a multi-agent team: read the Control Room, write clear task
briefs, route them to the right specialist through the task bus, review results, and
synthesize a final answer — without hoarding every credential.

## Best Uses
- One place to ask when you have several specialist agents (`hermes-seo`, `hermes-dev`, …).
- Writing task briefs and routing via the task bus / kanban.
- Reviewing specialist outputs and synthesizing a unified deliverable.
- Enforcing the `requires_approval_before` contract on delegated tasks.

## Do NOT Use For
- A single-agent setup (use operator/chief-of-staff instead).
- Holding every specialist's credentials (anti-pattern — it must not).
- Bypassing the specialist that owns a domain.

## Required Integrations (honest wiring table)
| Capability | Needs | Hermes-native? | Status |
|---|---|---|---|
| Multiple specialist agents | profiles or containers (Level 2+) | Yes | ☐ |
| Task routing | Control Room task bus **or** native `kanban` | Yes | ☐ |
| Read the Control Room | the docs repo | Yes | ☐ |
| Delegate subtasks (single agent) | `delegate_task` | Yes | ✅ |

## Approval Gates
Every task brief carries `requires_approval_before: [publishing, pushing commits, rotating
credentials, destructive operations]`. The orchestrator enforces it; it does not approve on
your behalf.

## Inputs Needed From User
Which specialist agents exist + what each is allowed to do, the task bus/kanban location, and
your synthesis preferences.

## First Workflow
Route one task through orchestrator → task bus → specialist → back, then synthesize.
Walkthrough: `../examples/07-control-room-task-routing.md`.

## Example Prompts
- "Route this to the right specialist and synthesize the result."
- "Write a task brief for hermes-dev to fix [bug], with the approval gates set."
- "What's in the task bus? Summarize status across specialists."

## KPIs
Routing accuracy, end-to-end task latency, synthesis quality, zero credential sprawl.

## Failure Modes (and guardrail)
- Becomes a mega-agent with all keys → it routes only; specialists own credentials.
- Routes to the wrong specialist → Control Room registry defines allowed work.
- Skips approval → briefs hard-code `requires_approval_before`.

## Note on scope
On a single agent, "orchestration" is just `delegate_task` for parallel subtasks — that's fine
and native. The full orchestrator role is for when you've genuinely grown into a Level 3
Control Room. Grow into it; don't lead with it.

## Convert to a Hermes Skill
`name: orchestrator`, `description: "Use when routing tasks across a team of specialist Hermes
agents and synthesizing results (Control Room Level 3; routes only, never hoards credentials;
enforces approval gates)."` Save to `~/.hermes/skills/business/orchestrator/SKILL.md`,
`/reload-skills`.
