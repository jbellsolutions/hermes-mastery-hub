# Module 03 — Memory & Session Search

## Outcome
You understand the three long-term systems and can make the agent remember a durable fact.

## Required reading
- ../reference/05-memory-and-learning-loop.md

## Exercise
1. Check memory status: `hermes memory status`.
2. Tell the agent a durable fact about how you work, e.g. *"Remember: always keep approvals
   ON for anything that spends money or sends email."* Confirm it saved a compact fact.
3. Start a new session (`/new`) and ask what it remembers about your approval preference —
   verify it persisted.
4. Test recall: ask *"What did we do in the last session?"* — the agent uses session search
   (FTS5) to find it, no repetition needed from you.

## Artifact
- One durable preference in memory that survives a new session.

## Verification checklist
- [ ] `hermes memory status` shows memory enabled
- [ ] The agent saved your approval preference as a compact fact (not a paragraph)
- [ ] After `/new`, the agent still knows the preference
- [ ] The agent can recall prior-session context via session search

## Coach notes
Three systems, three jobs: **memory** = durable facts always in context; **session search** =
recall anything ever said; **skills** = procedures. Keep memory compact — don't dump task logs
into it (that's what session search is for).

## Next action
Proceed to Module 04 — Gateway.
