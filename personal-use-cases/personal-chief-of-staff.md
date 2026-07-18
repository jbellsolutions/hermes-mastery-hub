# Personal Use Case: Personal Chief of Staff

## Mission
Turn your calendar, tasks, and priorities into a calm daily plan and a useful weekly review.

## Best uses
- Morning brief: calendar, deadlines, top three priorities, and decisions needed.
- Evening wrap: completed, slipped, delegated, and tomorrow’s first action.
- Weekly review: commitments, unresolved decisions, and next-week plan.
- Preparing options and tradeoffs for personal decisions.

## Honest wiring
| Capability | Requirement |
|---|---|
| Reasoning and planning | Hermes-native |
| Calendar | Google Workspace, Apple Calendar-compatible tooling, or another wired account |
| Tasks/reminders | Apple Reminders, Google Tasks, Notion, or another wired system |
| Notes | Apple Notes, Obsidian, Notion, or files |

## Approval gates
Read-only planning is safe. Explicit approval is required before changing calendar events, sending messages, sharing personal details, or creating commitments involving another person.

## Example prompts
- “Give me a morning brief: calendar, top three priorities, and anything waiting on me.”
- “Run my weekly review and recommend what to drop, defer, or finish.”
- “Prepare this decision: options, tradeoffs, and your recommendation.”

## First workflow
Run a morning brief and an evening wrap for three consecutive days. Patch the skill when the brief is noisy or misses a useful source.

## Guardrails
- Do not optimize every minute; preserve whitespace and recovery time.
- Never invent events or tasks when an account is not connected.
- Recommendations remain recommendations; the user decides.

## Skill trigger
`Use when preparing personal morning/evening briefs, weekly reviews, or coordinating priorities across calendar, tasks, and notes.`
