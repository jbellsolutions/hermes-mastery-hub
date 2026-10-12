# Profile distribution template

Copy this folder to start a templated "employee" agent you can install many times (one per client or pod).

1. Copy the folder, rename it, edit `distribution.yaml`, `SOUL.md`, `config.yaml`, add skills.
2. `.gitignore` is already here — keep it before `git init`.
3. Test locally: `hermes profile install ./my-agent --name test-agent`
4. Push to a (private) repo. Install per client: `hermes profile install git@github.com:you/my-agent.git --name acme-sdr --alias`
5. Ship changes: bump `version`, push, then `hermes profile update acme-sdr` (memory, sessions, auth and .env are never touched).

Full guide: `../../reference/15-profiles-deep-dive.md`.
