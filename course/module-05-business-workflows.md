# Module 05 — Business Workflows (your first real loop)

## Outcome
You've run a full, owner-gated business workflow end-to-end: lead → estimate → approve →
payment link → draft email → track.

## Required reading
- ../reference/11-best-practices.md
- ../templates/approval-gate.md, ../templates/workflow-map.md
- ../case-studies/01-mom-n-pop-small-business.md
- ../examples/01-lead-to-payment.md  (the step-by-step you'll follow)

## Exercise
1. Fill `../templates/business-intake.md` with your real business (or a realistic dry run).
2. Install the estimator + CRM + Stripe skills (from Module 02's pack) and wire what you can
   (`STRIPE_SECRET_KEY` in `.env` for **test mode**; email via himalaya/Gmail if available).
3. Follow `../examples/01-lead-to-payment.md` exactly, in test mode.
4. At each 🚦 approval gate, make the agent show you the draft and wait for your "yes".
5. Use Stripe test card `4242 4242 4242 4242` to complete a payment; confirm the agent detects
   `paid` and updates the CRM.

## Artifact
- One lead processed end-to-end with every side effect gated (test mode).

## Verification checklist
- [ ] Estimate produced from a rate card (not invented numbers)
- [ ] Estimate + email required your approval before sending
- [ ] Stripe amount was in **cents** and matched the estimate
- [ ] Payment detected; CRM updated to won/paid

## Coach notes
This is the pattern for *every* role: draft → approve → execute. Amounts in cents ($3,300 =
330000). Never auto-send customer email. Match the charge to the estimator output exactly.

## Next action
Proceed to Module 06 — Business Roles (build the operator).
