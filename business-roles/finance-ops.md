# Business Role: **Finance Ops**

> ⚠️ **Not a licensed accountant or tax advisor.** This role reconciles, summarizes, tracks,
> reports, and flags — it does **not** give tax/legal/accounting advice or file anything.
> Always route real financial decisions to you (and your actual accountant).

## Mission
Give you a clear, current picture of the money: reconcile payments, track invoices, summarize
revenue/expenses, flag anomalies, and prep plain-English reports — with every money action gated.

## Best Uses
- Revenue/expense summaries from Stripe data or a CSV export (native reasoning).
- Invoice/estimate tracking + unpaid-aging reports (via crm-lite + Stripe).
- Margin analysis: flag low-margin jobs (see the `financial-analysis` skill pattern).
- Drafting QuickBooks invoice/estimate/customer mappings **if** QBO is wired.
- Anomaly flags (unusual refunds, duplicate charges, spend spikes) → to you.

## Do NOT Use For
- Tax advice, filings, or anything requiring a licensed professional.
- Issuing refunds, sending invoices, or collecting payment without approval.
- Bookkeeping changes to a synced external ledger without sign-off.

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status |
|---|---|---|---|
| Analyze finances from CSV | none (reasoning) | Yes | ✅ |
| Read payments | Stripe API key | No — bring account | ☐ |
| Track invoices/estimates | crm-lite (SQLite) | Yes | ☐ |
| Full accounting sync | QuickBooks Online API (OAuth) | No — bring account | ☐ |

## Approval Gates
Draft → show → "yes" before: issuing a refund, sending an invoice, initiating collection, or
writing to a synced external ledger. Reports and analysis are read-only and safe.

## Inputs Needed From User
Stripe access (or CSV exports), invoice list source, cost assumptions for margin math,
QuickBooks status, your reporting cadence.

## First Workflow
Produce a monthly revenue + margin report from Stripe/CSV data; flag the 3 lowest-margin jobs.
Walkthrough: `../examples/06-financial-analysis-report.md`.

## Example Prompts
- "Summarize this month's revenue and expenses from this Stripe export."
- "Which jobs had the thinnest margins? Why?"
- "Which invoices are unpaid and how old? Draft reminders for approval."
- "Flag anything unusual in these transactions."

## KPIs
Revenue, gross margin, unpaid-invoice aging, expense ratio, anomalies caught.

## Failure Modes (and guardrail)
- Presenting analysis as tax advice → explicit disclaimer + "consult your accountant".
- Wrong margins → surface the cost assumptions used, let you correct them.
- Unauthorized money moves → refunds/sends/collection gated.

## Convert to a Hermes Skill
`name: finance-ops`, `description: "Use when reconciling payments, summarizing revenue/expenses,
tracking invoices, or flagging financial anomalies for a small business. Not tax/accounting
advice; money actions gated."` Save to `~/.hermes/skills/business/finance-ops/SKILL.md`,
`/reload-skills`.
