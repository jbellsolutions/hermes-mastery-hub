# Example 04 — Daily Pipeline Report (cron → your phone)

*Grounded in `../reference/09-cron-and-durable-systems.md` and the `crm-lite` weekly-report
pattern. Automate only a workflow you've already run manually.*

## Goal
A cron job that every morning summarizes your pipeline (new leads, estimates out, unpaid,
pipeline value) and delivers it to your gateway (Telegram).

## Prereqs
- `crm-lite` in use with real data.
- A gateway connected (Telegram) — **delivery must target a gateway platform** or it won't reach you.
- The manual version proven first: ask the agent for the summary by hand once and confirm it's right.

## Workflow map
```
cron tick (daily 8am) → query crm-lite → summarize by stage → deliver to Telegram
```

## Step-by-step
1. Prove it manually: "Give me today's pipeline summary: new leads, estimates out, unpaid
   >3 days, pipeline value." Confirm the numbers are right.
2. Create the job (verify flags with `hermes cron --help`):
   ```bash
   hermes cron create "0 8 * * *"
   ```
   In the flow, set the prompt to the same summary request and set delivery to `telegram`
   (your chat id). Or use the `cronjob` tool from within a session.
3. Trigger a test run now: `hermes cron run <id>` and confirm it arrives in Telegram.
4. Leave it running. Adjust the schedule/prompt with `hermes cron edit <id>`.

## Approval gates
- Reporting is read-only — safe. If you later add "and send reminders to unpaid customers,"
  that send must be gated (draft → approve).

## Verification checklist
- [ ] Manual summary was correct before automating
- [ ] `hermes cron list` shows the job
- [ ] A test run delivered the report to your gateway
- [ ] Delivery target is a real platform (not local-only)

## Common pitfalls
- A CLI/TUI-scheduled job with default delivery is **saved but not pushed** to you — set
  `deliver` to a gateway platform.
- Automating an unproven workflow → prove it manually first (the Control Room rule).
