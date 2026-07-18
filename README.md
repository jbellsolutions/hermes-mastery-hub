# Hermes Mastery Hub

A verified reference library, weekend course, real case studies, and a 13-role business
agent library for **Hermes Agent** by Nous Research — built to be *fed to your own Hermes
agent* so it can coach you from beginner to top-tier operator.

> **This hub is designed to be loaded by an agent, not just read by a human.** Copy it into
> your skills directory and let Hermes walk you through it step by step.

## Quick start

```bash
# 1. put the hub where your agent can load it
cp -R ~/Desktop/hermes-mastery-hub ~/.hermes/skills/hermes-mastery-hub

# 2. start a session with the hub loaded
hermes -s hermes-mastery-hub
```
Then say: **"Be my Hermes coach. Start the weekend bootcamp."**

The agent reads `agent-feed/HERMES_COACH_PROMPT.md`, verifies your setup, and teaches +
builds with you one step at a time — gating every real-world action behind your approval.

## What's inside

| Folder | What it is |
|---|---|
| `reference/` | 13 ground-truth docs (`00-ground-truth` → `12-troubleshooting`) |
| `case-studies/` | Mom-n-Pop small business, multi-agent Control Room, Master Class study guide |
| `business-roles/` | 13 role cards: operator, marketer, cold-email, sales, media-buyer, copywriter, email-reply, customer-success, finance-ops, research-analyst, automation-engineer, founder-chief-of-staff, orchestrator |
| `course/` | 9 modules + a `weekend-roadmap.md` |
| `templates/` | copy-paste SKILL.md, role-card, task-brief, approval-gate, workflow-map, business-intake |
| `examples/` | 7 concrete, approval-gated workflow walkthroughs |
| `agent-feed/` | everything the agent reads to run as your coach |

## Honest expectation setting

You set the goal "top 1% Hermes user by the weekend." The truthful version this hub holds you
to: **in a focused weekend you build the operating system of a top-tier user** — a working
setup, a gateway you talk to from your phone, 3+ approval-gated business workflows, your first
custom skills, and a roadmap. Reading informs you; *running real workflows* is what makes the
agent (and you) elite. The hub is the fast-path scaffold; the compounding is yours to earn.

## The five doctrines (from the source repos + docs)

1. **Draft → approve → execute.** The human gates every real-world side effect.
2. **Encode expertise as skills.** Do it twice → make it a skill.
3. **Honest capability.** Only claim what's actually wired.
4. **Start manual, automate later.**
5. **Verify before "done."**

## Source discipline

Every claim traces to a source in [`SOURCES.md`](SOURCES.md). Provenance is separated into:
official Nous docs/source, a locally-installed Hermes skill on the build machine, community
MIT repos, and sources that were **not** accessible (and therefore not used). See
[`reference/00-ground-truth.md`](reference/00-ground-truth.md) for the anti-hallucination
contract. Attribution for the MIT source repos is in [`NOTICE.md`](NOTICE.md).

## Verify anything

If a command or feature here differs from what `hermes <command> --help` or the
[official docs](https://hermes-agent.nousresearch.com/docs) say, **the live source wins** —
patch the hub. This is a cited snapshot, not a replacement for the source of truth.

## License

MIT — see [`LICENSE`](LICENSE). Incorporates patterns from two MIT-licensed community repos
(see `NOTICE.md`).
