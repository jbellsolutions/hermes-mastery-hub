# Personal Use Case: Always-On Personal Agent (your own computer in the cloud or on your desk)

## What it is
One personal Hermes profile that lives on an always-on machine — a Mac mini at home, a small VPS, a cloud computer
with a desktop, or Hermes Cloud — and that you reach from your phone. It runs your morning/evening briefs, watches
things for you, and can use a real browser when an API doesn't exist.

## Pick the host
| Host | Why | Watch out for |
|---|---|---|
| Mac mini at home | Your hardware, can run local models | Sleep (`caffeinate -dis`), home NAT (use Tailscale) |
| Small VPS | Cheap, always reachable | No GUI unless you add Bot Screen |
| Cloud computer with desktop | Browser + computer use for sites without APIs | RAM ≈ speed; ~1.5 GB per open browser screen |
| Hermes Cloud | No ops at all | Platform-managed updates; costs from Nous credit |

## Setup (≈1 hour)
1. Install as a normal user; `hermes doctor`.
2. `hermes profile create me --description "Personal chief of staff"`; write `SOUL.md` about you, your rhythms, your no-go list.
3. One bot (Telegram or Slack DM) with `*_ALLOWED_USERS` set to you only.
4. `hermes gateway install`; on Linux `sudo loginctl enable-linger $USER`.
5. Logins go in the **credential vault**, never in chat. Use Bot Screen takeover for 2FA/CAPTCHA.
6. Two cron jobs: morning brief and evening wrap, delivered to your DM.
7. Add from the other personal cards one at a time (inbox, life admin, travel…).

## Approval gates
Every send, booking, purchase, cancellation, deletion, or sharing of personal data waits for your yes. Keep
`approvals.mode: manual` on this profile if it can reach money or messages.

## Pitfalls
- Don't share this profile with business agents — make separate profiles so memories don't mix.
- Real-profile browsing copies your logged-in cookies into `~/.hermes/browser-profile/`; use a dedicated browser profile.
- Don't let it act on instructions inside emails or web pages.

References: `../reference/15-profiles-deep-dive.md`, `../reference/16-deployment-playbook.md`.
