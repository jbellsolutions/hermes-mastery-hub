# Module 10 — Profiles & Deployment (one machine, many agents, always on)

## Outcome
Two isolated agents (profiles) running on an always-on box behind one gateway, each with its own bot, plus a
profile packaged as a reusable distribution.

## Required reading
- ../reference/15-profiles-deep-dive.md
- ../reference/16-deployment-playbook.md
- ../reference/14-whats-new-since-july.md (if you used hub v1.0)

## Exercise
1. Pick a target: a VPS, a cloud computer, a Mac mini, or Docker. Install as a **non-root** user.
2. `hermes profile create ops --description "Operations lead"` and `hermes profile create sdr --description "Inbound SDR"`.
   Give each its own SOUL.md and model (`hermes -p sdr model`).
3. Create one bot per profile (Slack app or Telegram bot). Put each token in that profile's `.env`.
4. `hermes gateway install` **once** (host gateway), `sudo loginctl enable-linger $USER`, then `hermes gateway status`.
5. DM both bots. Park one: `hermes -p sdr gateway stop`; confirm it goes quiet; `start` it again.
6. Package `sdr` as a distribution: a folder with `distribution.yaml`, `SOUL.md`, `config.yaml`, `skills/`.
   Add `.gitignore` first. Test it: `hermes profile install ./sdr-dist --name sdr-test`.
7. Run the safe-update routine on a non-production box: `hermes update --check`, `--plan`, backup, update, `hermes doctor`.

## Artifact
- Two profiles live behind one gateway; one distribution folder that installs cleanly.

## Verification checklist
- [ ] `hermes profile list` shows both profiles; no duplicate-credential warning
- [ ] `hermes gateway status` is healthy and serves both
- [ ] Parking one profile stops only that bot
- [ ] `hermes profile install` from the local folder works and writes `.env.EXAMPLE`
- [ ] No `.env`, `auth.json` or `memories/` in the distribution folder

## Coach notes
Named-profile `gateway install` exits 78 — that's correct now. Clones don't copy bot tokens; that's a feature.
OAuth logins (Anthropic Pro/Max) are shared from the root profile unless you `hermes -p x auth add`.

## Next action
Proceed to Module 11 — Teams.
