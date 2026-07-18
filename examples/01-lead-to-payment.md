# Example 01 — Lead → Payment (the core owner-gated loop)

*Grounded in `../case-studies/01-mom-n-pop-small-business.md` and the real `lead-to-payment`,
`estimator-engine`, `crm-lite`, `stripe-payments` skills. **Run in Stripe TEST mode.***

## Goal
Take one inbound lead all the way to a paid (test) transaction, with the owner approving the
estimate and the email before anything goes out.

## Prereqs
- Skills installed: `estimator-engine`, `crm-lite`, `stripe-payments`, `lead-to-payment`.
- `STRIPE_SECRET_KEY` (test, `sk_test_…`) in `~/.hermes/.env`.
- Email optional (himalaya/Gmail) — if not wired, save the email as a draft instead of sending.
- `approvals.mode: manual`.

## Workflow map
```
Lead (Telegram paste) → summarize → estimate → CRM log → 🚦approve estimate →
Stripe payment link → draft email → 🚦approve email → send/draft → track → paid → CRM won
```

## Step-by-step (say these to the agent)
1. **Paste the lead:** "New lead: Jane Doe, 2,400 sqft home, [your city], [service], referral."
2. Agent summarizes and asks to draft an estimate → say "yes."
3. Agent prices via `estimator-engine` (from your rate card — not invented) and logs a `draft`
   estimate in `crm-lite`. It shows you line items + total.
4. **🚦 Approval gate:** "Send this estimate as a payment link? (yes/edit/no)" → review, say "yes."
5. Agent creates a Stripe **payment link** (amount in **cents** — $3,300 = 330000) and returns
   the `https://buy.stripe.com/…` URL.
6. Agent drafts the customer email (estimate + link). **🚦 Approval gate:** "Send this email?"
   - Email wired → say "send"; agent sends.
   - Email not wired → say "save as draft"; agent saves to Drafts (or shows you the text to copy).
7. **Pay in test mode:** open the link, use card `4242 4242 4242 4242`, any future expiry/CVC/ZIP.
8. Ask "did it get paid?" → agent checks Stripe status; on `paid` it updates `crm-lite`
   (`estimate=paid`, `customer=won`) and confirms.

## Approval gates (must fire)
- Before sending the estimate/payment link.
- Before sending the customer email.

## Verification checklist
- [ ] Estimate total came from the rate card, not a guess
- [ ] Stripe amount in cents matched the estimate exactly
- [ ] Both sends required your explicit "yes"
- [ ] Test payment succeeded and CRM updated to won/paid

## Common pitfalls
- Amounts in **cents** ($3,300 = 330000). Test card only works in **test mode**.
- Never auto-send the email — draft → approve → send.
- If email is GoDaddy IMAP (himalaya): `message save --folder Drafts` (not `template save`);
  `template reply <msg_id>` for thread replies.
