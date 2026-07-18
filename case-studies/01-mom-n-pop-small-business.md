# Case Study 01 — Mom-n-Pop: a real service business run over Telegram

*Source: `hewi333/Mom-n-Pop-Skills` (MIT) — README + verified SKILL.md files (crm-lite,
estimator-engine, lead-to-payment, stripe-payments, marketing-campaign-builder,
agent-cheatsheet-builder). Built during the NVIDIA × Stripe × Nous Research hackathon,
June 2026. This is the strongest, most complete case study in the hub — study it closely.*

## The one-line story

A small service business was run **end-to-end through Telegram** by a Hermes agent — real
leads, real estimates, real Stripe charges, real marketing — with the owner approving every
action by tapping "yes." It shipped in ~11 days and won a hackathon prize. The reason it's
worth 10x-ing: it is the cleanest working proof of the **draft → approve → execute** pattern.

## The problem it solved

Small businesses (1–10 people) don't need another dashboard. They need help with the 3+ hours
of daily manual work nobody signed up for: estimating jobs, following up, sending payment
links, tracking who paid, and figuring out whether the last job made money. The owners are
often non-technical and live in a chat app, not a CRM.

> The product insight: **"No new software to learn. No computer required. Just text."**

## The architecture

```
                Telegram (the owner's existing texting app = the entire UI)
                                     │
                          Hermes Agent (gateway)
                                     │
   ┌──────────────┬──────────────┬───────────────┬────────────────┬──────────────┐
 estimator-     crm-lite      stripe-         himalaya         mailchimp /     agent-cheatsheet-
 engine        (SQLite)      payments      (GoDaddy IMAP/SMTP)  campaign-builder   builder
 (pricing)     (contacts,    (payment       (draft customer     (email            (owner
               leads,        links,         emails — DRAFT      campaigns,        onboarding
               estimates)    invoices)      ONLY)               A/B tests)        cheat sheet)
```

- **Front door:** Telegram. The owner pastes a lead or says "check the email."
- **Model:** GLM-5.2 via Together AI (direct, no proxy). *(Provider-agnostic — swap freely.)*
- **Everything gated:** the owner approves before anything is sent or charged.

## The core workflow (lead → cash)

1. **Intake** — lead arrives via Telegram paste, GoDaddy inbox (himalaya), or website form.
2. **Summarize + notify** — agent posts a short lead summary, asks to draft an estimate.
3. **Price it** — `estimator-engine` computes from a config-driven rate card (sqft tiers,
   region, add-ons). *Never invents prices; pulls from the rate card.*
4. **Log to CRM** — `crm-lite` upserts customer + creates a `draft` estimate (SQLite, no SaaS).
5. **🚦 Approval gate** — agent shows the full estimate in Telegram: *"Send this? (yes/edit/no)"*.
6. **Charge** — on yes, `stripe-payments` creates a payment link. **Amounts in cents**
   ($3,300 = 330000). NET-30 jobs use a Stripe invoice instead.
7. **Draft email** — `himalaya` composes a customer email with the estimate + Stripe link.
   **DRAFT ONLY** — agent shows it and asks "send?" before sending.
8. **🚦 Approval gate** — owner says "send" → agent sends (or saves to Drafts folder).
9. **Track to paid** — poll Stripe (or webhook). On `paid`: Telegram "✅ paid", CRM →
   `won`. Follow-up cron nudges if unpaid after 3 days.
10. **Grow** — `marketing-campaign-builder` drafts owner-approved Mailchimp campaigns from the
    CRM list; `financial-analysis` produces plain-English margin insights quarterly.

See `../examples/01-lead-to-payment.md` for the step-by-step walkthrough and
`../examples/05-marketing-campaign-builder.md` for the campaign flow.

## The skills involved (all real, all in the repo)

| Skill | Role in the flow |
|---|---|
| `estimator-engine` | Config-driven pricing (sqft tiers, region, severity add-ons, ±15% range) |
| `crm-lite` | SQLite CRM: customers, leads, activities, estimates, communications, tasks |
| `stripe-payments` | Customers, payment links, checkout sessions, invoices, refunds, webhooks |
| `lead-to-payment` | Master orchestrator tying the above together, owner-in-the-loop |
| `marketing-campaign-builder` | AI-drafted Mailchimp campaigns + subject A/B tests, owner-gated |
| `agent-cheatsheet-builder` | One-page plain-English "fridge sheet" for the non-technical owner |

## Why it was compelling (and won)

- **Chat-native.** It met owners where they already are. Zero learning curve.
- **Trust through approvals.** *"Owners who've never touched AI will use it every day if they
  trust it. They trust it because nothing happens without their OK. This isn't a limitation.
  It's the product."*
- **It earned and spent real money** (Stripe for revenue; Stripe Link for agent spend).
- **Honest onboarding.** The cheat-sheet builder only lists capabilities that are actually
  wired — "coming later" beats a silent failure.

## Pitfalls it documented (learn these for free)

- **Stripe amounts are in cents.** $3,300 = 330000, not 3300. Match the charge to the
  estimator output exactly.
- **Test mode for demos** (`sk_test_`); test card `4242 4242 4242 4242`.
- **Never auto-send customer email.** Draft → show owner → explicit "send" → send.
- **Email was GoDaddy IMAP/SMTP (himalaya), NOT Microsoft 365.** Don't assume M365/Graph.
  Use `himalaya message save --folder Drafts` (NOT `template save` — doesn't exist);
  `himalaya template reply <msg_id>` for thread replies (preserves In-Reply-To).
- **Region/pricing config must match** across estimator, website, and CRM — one canonical source.
- **Approval holds can expire** (~6 min in the Link CLI) — set up first, then trigger spend.
- **One-time virtual cards don't rebill** — fine for a demo, handle recurring properly later.

## How to reproduce it for YOUR business (the 10x path)

The pattern transfers to any service business (HVAC, landscaping, cleaning, consulting,
freight, etc.). Steps:

1. Install the pack: clone `Mom-n-Pop-Skills`, copy `skills/*` into `~/.hermes/skills/`.
2. **Replace the estimator rate card** with your real pricing (the numbers in the skill are
   EXAMPLES). Keep the tier/region/add-on *structure*.
3. Wire your accounts in `~/.hermes/.env`: `STRIPE_SECRET_KEY`, your email (himalaya config
   or Gmail/Resend API), optional Mailchimp/QuickBooks.
4. Connect Telegram (`hermes gateway setup`), keep `approvals.mode: manual`.
5. Run one lead through by hand (Example 01) before automating anything.
6. Add a daily pipeline-report cron (Example 04) and a follow-up cron only after the manual
   flow is proven.
7. Generate the owner cheat sheet so a non-technical operator can run it.

**The generalization for this hub:** every business *role* (operator, sales, cold-email,
marketer…) is this same pattern — encode the expertise as a skill, gate side effects behind
approval, deliver through chat. That's why the role cards in `../business-roles/` all end with
"convert to a skill."
