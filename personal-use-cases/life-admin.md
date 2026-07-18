# Personal Use Case: Life Admin

## Mission
Reduce recurring administrative friction: reminders, appointments, document checklists, household renewals, and forms.

## Best uses
- Maintain recurring reminders for renewals, maintenance, and appointments.
- Prepare document checklists for applications or appointments.
- Draft form answers from information the user explicitly provides.
- Research service providers and present options.

## Honest wiring
Calendar/reminder/notes actions require the relevant wired account or installed skill. Websites may require browser automation; logged-in or payment steps must be handed back to the user.

## Approval gates
Ask before submitting forms, cancelling services, booking appointments, contacting providers, deleting records, or sharing personal information.

## Example prompts
- “Build a checklist for renewing my passport; cite the official source.”
- “Create reminders for home and vehicle maintenance, but show me the schedule first.”
- “Research three local providers and compare cost, reviews, and availability.”

## First workflow
Choose one recurring administrative burden. Map it using `../templates/workflow-map.md`, run it manually once, then schedule only the reminder portion.

## Guardrails
- Use official sources for legal/government requirements.
- Do not submit on the user’s behalf without explicit approval.
- Never type SSNs, passwords, card details, or 2FA codes.

## Skill trigger
`Use when organizing recurring personal administration, reminders, appointments, forms, renewals, or document checklists.`
