# Hermes Mastery Hub

A verified reference library, weekend course, real case studies, and a 13-role business
agent library for **Hermes Agent** by Nous Research — built to be *fed to your own Hermes
agent* so it can coach you from beginner to top-tier operator.

> **This hub is designed to be loaded by an agent, not just read by a human.** Copy it into
> your skills directory and let Hermes walk you through it step by step.

## Quick start

```bash
# 1. clone the public hub into your skill directory
mkdir -p ~/.hermes/skills
git clone https://github.com/jbellsolutions/hermes-mastery-hub.git \
  ~/.hermes/skills/hermes-mastery-hub

# 2. start a session with the hub loaded
hermes --skills hermes-mastery-hub
```
Then say: **"Be my Hermes coach. Start the weekend bootcamp."**

If you already have the repository locally, copy that directory into
`~/.hermes/skills/hermes-mastery-hub` instead.

The skill instructs the agent to read `agent-feed/HERMES_COACH_PROMPT.md`, verify setup, and
teach one step at a time while gating external side effects. Actual adherence depends on the
active model, installed Hermes version, available tools, and your approval discipline.

## What's inside

| Folder | What it is |
|---|---|
| `reference/` | 14 grounded reference docs (`00-ground-truth` → `13-community-discovery-and-curation`) |
| `case-studies/` | Mom-n-Pop small business, multi-agent Control Room, Master Class study guide |
| `business-roles/` | 13 role cards: operator, marketer, cold-email, sales, media-buyer, copywriter, email-reply, customer-success, finance-ops, research-analyst, automation-engineer, founder-chief-of-staff, orchestrator |
| `personal-use-cases/` | 7 personal patterns: chief of staff, life admin, inbox, learning, travel, home/family, finance review |
| `course/` | 9 modules + a `weekend-roadmap.md` |
| `templates/` | copy-paste SKILL.md, role-card, task-brief, approval-gate, workflow-map, business-intake |
| `examples/` | 8 hub-designed, approval-gated workflow walkthroughs |
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

Hermes product and source-repository claims are cited in [`SOURCES.md`](SOURCES.md).
Course structure, templates, role cards, personal use cases, and recommendations are original
synthesis based on those sources. Provenance is separated into official Nous docs/source, a
locally installed Hermes skill, community MIT repos, and inaccessible sources not relied upon. See
[`reference/00-ground-truth.md`](reference/00-ground-truth.md) for the anti-hallucination
contract, [`SOURCE_LOCK.md`](SOURCE_LOCK.md) for the reproducible snapshot, and
[`NOTICE.md`](NOTICE.md) for attribution.

## Examples: what is externally verified vs. designed here

- **Verified external implementations:** Mom-n-Pop Skills and Hermes Agent Control Room.
- **Hub-designed walkthroughs:** the eight files under `examples/`, built from verified
  patterns but not presented as independently shipped community projects.

Community discovery links and a curation rubric are in
[`reference/13-community-discovery-and-curation.md`](reference/13-community-discovery-and-curation.md).
Known limitations and validation boundaries are in [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md).

## Verify anything

If a command or feature here differs from what `hermes <command> --help` or the
[official docs](https://hermes-agent.nousresearch.com/docs) say, **the live source wins** —
patch the hub. This is a cited snapshot, not a replacement for the source of truth.

## License

MIT — see [`LICENSE`](LICENSE). Incorporates patterns from two MIT-licensed community repos
(see `NOTICE.md`).
