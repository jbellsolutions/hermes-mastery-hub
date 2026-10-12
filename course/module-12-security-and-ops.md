# Module 12 — Security & Ops (make it safe to leave running)

## Outcome
A hardened production agent: allowlisted users, deny rules, secrets out of plain files, a login it can use without
seeing the password, backups, monitoring, and a tested update/rollback.

## Required reading
- ../reference/16-deployment-playbook.md (Security, Operations)
- ../reference/19-edge-cases-dos-and-donts.md
- ../reference/18-plugins-mcp-and-extension-map.md (plugin trust)

## Exercise
1. Allowlist: set `*_ALLOWED_USERS` (and Slack `allowed_channels`); check with `/whoami`.
2. Add `approvals.deny` globs for force-push, `curl | sh`, mass deletes. Try one under `--yolo` and watch it fail.
3. Move one API key to 1Password or Bitwarden (`hermes secrets onepassword setup`) and remove it from `.env`.
4. Store one website login in the credential vault and have the agent log in without the password entering chat.
5. Set `checkpoints.enabled: true`, make a file edit, then `/rollback diff 1`.
6. Schedule `hermes backup` nightly; make a failing backup alert you.
7. Turn on gateway health export (OTLP) or, at minimum, a cron `hermes gateway status` check.
8. Install one catalog plugin with `hermes plugins install <name>`; read its capabilities and scan result before enabling.
9. Practice a rollback: restore yesterday's backup on a test box.
10. Work-machine track: write a managed-scope `/etc/hermes/config.yaml` that pins `approvals.mode: manual`.

## Verification checklist
- [ ] A stranger's DM is declined
- [ ] A denied command fails even in YOLO
- [ ] No provider keys in `config.yaml`
- [ ] The vault login worked and the password never appeared in the transcript
- [ ] Backup restore tested once

## Next action
Go back to `daily-track.md` and keep the daily rhythm.
