# 11 — Best Practices (the operating doctrine)

*Synthesized from: the Mom-n-Pop and Control Room source repos, the installed `hermes-agent` skill, and the durable-systems design. Every practice below traces to a verified source pattern.*

These are the habits that separate a person who *has* Hermes from a person who *operates* it.

## 1. Draft → approve → execute (the trust pattern)

The single most important pattern in the whole hub, straight from the prize-winning Mom-n-Pop
build: **the agent drafts and proposes; the human approves; then the agent executes.** Nothing
with a real-world side effect (send email, charge a card, publish, deploy, delete) happens
without an explicit yes. Their words: *"This isn't a limitation. It's the product."* Keep
`approvals.mode: manual` (or `smart`) on for any agent touching money or outbound comms.

## 2. Encode expertise as skills, not chat

Anything you'll do more than twice becomes a skill. A skill carries the trigger (`description`),
the steps, the pitfalls, and a verification checklist. This is how the agent gets better at
*your* business over time. Facts go in memory; procedures go in skills.

## 3. Be honest about what's wired vs documented

From the `agent-cheatsheet-builder` skill: only present capabilities that actually work today.
If QuickBooks isn't connected, it's "coming later," not a feature. Overstating capability
destroys trust and produces silent failures. This hub's `SOURCES.md` and role cards apply the
same rule.

## 4. Match numbers to the source of truth

The lead-to-payment pitfall: the Stripe charge must equal the estimator output *exactly*.
Amounts are in cents ($3,300 = 330000). Region classification must match across estimator,
website, and CRM — pick one canonical source and make the others follow it.

## 5. Start manual, automate later

From the Control Room: *"Only add automation after the manual workflow works."* Prove
lead→estimate→approve→pay by hand first. Then add the cron job. Then add webhooks. Then, maybe,
add a second agent.

## 6. One stable name everywhere

Slug discipline (`hermes-cmo` = container = data dir = docs = key prefix = bot name) prevents
the "which thing is this?" chaos as you scale.

## 7. Secrets in `.env`, settings in `config.yaml`, nothing raw in git

`.gitignore` excludes `.env`. The Control Room records secret *names/scopes/locations*, never
values. MCP stdio servers only receive the env vars you explicitly pass.

## 8. Keep skills sharp

If a skill is wrong, stale, or missing a pitfall you just hit — patch it immediately
(`skill_manage(action='patch')` / `hermes` skill tooling). Let the Curator archive idle ones.
Pin the skills you never want auto-touched.

## 9. Verify before you claim "done"

Every source SKILL.md ends with a Verification Checklist for a reason. The deliverable is a
working artifact backed by real output, not a description of one. Run the command; read the
result; then report.

## 10. Design for the phone

The Mom-n-Pop insight: owners will use an agent every day if it lives in the app they already
text in (Telegram) and never demands a computer. Build for chat-first, approval-by-tap.

## 11. Reframe "mastery"

You don't become a top operator by reading this hub. You become one by running real workflows
until the agent's memory and skills are tuned to your business. This hub is the fast-path
scaffold; the compounding is yours to earn. (See the honest framing in the root `README.md`.)
