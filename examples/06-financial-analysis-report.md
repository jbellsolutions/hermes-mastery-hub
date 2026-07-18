# Example 06 — Financial Analysis Report (read-only, no advice)

*Grounded in `../business-roles/finance-ops.md` and the `financial-analysis` skill pattern.
This is analysis, **not** tax or accounting advice.*

## Goal
Produce a plain-English monthly report — revenue, expenses, margins, unpaid aging, anomalies —
from Stripe data or a CSV export, and flag the lowest-margin jobs.

## Prereqs
- Data source: `STRIPE_SECRET_KEY` (read) in `.env`, OR a CSV/QuickBooks export you provide.
- `crm-lite` for invoice/estimate status (optional but useful).
- Your cost assumptions (labor, materials) for margin math.

## Workflow map
```
Pull payments (Stripe/CSV) → categorize → compute revenue/expenses/margins →
join CRM invoice status → summarize plain-English → flag low-margin + anomalies
```

## Step-by-step
1. "Summarize this month's revenue and expenses from [this Stripe export / CSV]."
2. Agent categorizes transactions and computes totals + gross margin. It **states the cost
   assumptions it used** so you can correct them.
3. "Which 3 jobs had the thinnest margins, and why?" Agent explains the drivers.
4. "Which invoices are unpaid and how old?" Agent produces an aging list from CRM/Stripe.
5. "Flag anything unusual." Agent surfaces duplicate charges, big refunds, spend spikes.
6. (Optional) "Draft payment reminders for the unpaid invoices." → **🚦 gate** before any send.

## Approval gates
- Analysis/reporting is **read-only and safe** — no gate needed.
- Any refund, invoice send, or collection message → draft → approve → execute.

## Verification checklist
- [ ] Totals reconcile against the source data
- [ ] Cost assumptions for margins were shown and confirmed by you
- [ ] Unpaid aging list is accurate
- [ ] No money action taken without approval
- [ ] Report explicitly notes it is not tax/accounting advice

## Common pitfalls
- Presenting analysis as tax advice → always caveat "consult your accountant."
- Wrong margins from bad assumptions → surface and confirm inputs.
- Amounts from Stripe are in cents — divide by 100 for display.
