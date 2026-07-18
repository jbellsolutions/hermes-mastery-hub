# 05 — Memory & the Learning Loop

*Source: installed `hermes-agent` skill v2.3.0, official README. Verify with `hermes memory status` and the [Memory docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory).*

Hermes has three distinct long-term systems. Don't confuse them — they solve different problems.

## 1. Persistent memory (facts that should always be in context)

Durable facts injected into every future turn: user preferences, environment details, tool
quirks, stable conventions. Managed by the `memory` tool and configured under `memory.*`.

```bash
hermes memory setup
hermes memory status
hermes memory off
```

Backends are pluggable: built-in, **Honcho** (dialectic user modeling), **Mem0**, and more.

**What belongs in memory:** "User keeps approvals ON for all money flows." "Business email is
GoDaddy IMAP, not M365." "Stripe amounts are in cents."
**What does NOT:** task progress, completed-work logs, PR numbers — those are recalled via
session search, not memory.

## 2. Session search (recall anything ever said)

FTS5-backed full-text search across all past conversations in `state.db`. Effectively free
(no aux-LLM). Three modes: discovery (by query), scroll (around a message), browse (recent).
This is how the agent answers "where did we leave the cold-email agent?" without you
repeating yourself. In-session it's automatic; the CLI surfaces sessions via
`hermes sessions browse`.

## 3. Skills (procedural memory)

Covered in `04-skills-system.md`. Facts → memory. Procedures → skills.

## Why the learning loop compounds

The official positioning: *"the only agent with a built-in learning loop — it creates skills
from experience, improves them during use, nudges itself to persist knowledge, searches its
own past conversations, and builds a deepening model of who you are across sessions."*

Practical implication for this hub's course: **you do not become a top operator by reading.**
You become one by running real workflows so the agent accumulates memory + skills tuned to
*your* business. The hub is the fast-path scaffold; the compounding comes from repetition.

## The learning log (a hub convention)

Because memory should stay compact, the coach keeps a human-readable **learning log** with
you — a running note of what was built, what worked, what to build next. See
`../agent-feed/HERMES_COACH_PROMPT.md`. Treat it as the changelog of your growing agent OS.
