# Personal Use Case: Personal Finance Review

> Organizational support only—not financial, investment, tax, or legal advice.

## Mission
Organize transaction data, summarize spending and cash flow, track bills, and surface questions for the user or a qualified professional.

## Best uses
- Categorize a CSV export and summarize monthly spending.
- Track recurring bills and renewal dates.
- Compare actual spending with a user-defined budget.
- Flag duplicates, unusual changes, or missing records for review.

## Honest wiring
Read-only analysis can use user-provided CSVs. Bank/account connections require an external provider and should use least-privilege read access. The agent must never request or type bank passwords, card numbers, or 2FA codes.

## Approval gates
Explicit approval before cancelling subscriptions, moving money, paying bills, contacting institutions, or changing financial accounts. Prefer the user completes these actions directly.

## Example prompts
- “Summarize this CSV by category and show the assumptions.”
- “Which recurring charges changed this month?”
- “Create a questions list for my accountant; do not give tax advice.”

## First workflow
Analyze one exported month, reconcile totals against the source, and create three user-verified categories to improve next month’s report.

## Guardrails
- State assumptions and reconcile totals.
- Do not recommend specific investments or tax positions as professional advice.
- Keep sensitive exports local and out of git.

## Skill trigger
`Use when organizing personal financial records, reviewing spending, tracking bills, or preparing questions for a qualified professional.`
