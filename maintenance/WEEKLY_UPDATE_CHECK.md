# Weekly Update Check — keep the hub true

Run weekly (a scheduled task does this). Goal: find what changed in Hermes since the last check, decide what in the
hub is now wrong, and patch it — never silently.

## 1. What's the latest release?

Tags changed scheme in October 2026: old `v2026.M.D`, new `vX.Y.Z`. Check **both**.

```bash
git ls-remote --tags https://github.com/NousResearch/hermes-agent.git \
  | grep -v '\^{}' | grep -v '+canary' | sed 's#.*refs/tags/##' | sort -V | tail -15
# daily canary builds are tagged vX.Y.Z+canary.<timestamp> — ignore them unless you test canary
# note: sort -V puts v0.x.y BEFORE v2026.x — the newest release is the highest v0.x.y, not the last line
```
Then read each new release page: `https://github.com/NousResearch/hermes-agent/releases/tag/<tag>`
(and `/releases/latest`). Note headline features, breaking changes, migrations, security fixes, new models.

## 2. What changed in the docs?

```bash
git clone --depth 1 https://github.com/NousResearch/hermes-agent.git hermes-src
git -C hermes-src fetch --depth 1 origin tag <last-checked-tag>
git -C hermes-src diff --stat <last-checked-tag> HEAD -- website/docs | tail -40
git -C hermes-src diff <last-checked-tag> HEAD -- \
  website/docs/user-guide/profiles.md website/docs/user-guide/multi-profile-gateways.md \
  website/docs/user-guide/features/kanban.md website/docs/user-guide/bot-mode.md \
  website/docs/user-guide/features/plugins.md website/docs/user-guide/security.md \
  website/docs/getting-started/updating.md hermes_cli/config_defaults.py
```

## 3. Re-check the load-bearing facts

| Fact in the hub | Where to confirm |
|---|---|
| `approvals.mode` default + unattended modes | `hermes_cli/config_defaults.py` (`approvals`) |
| Delegation defaults | `config_defaults.py` (`delegation`) |
| `agent.max_turns` default | `config_defaults.py` (`agent`) |
| Profile name regex | `hermes_constants.py` `PROFILE_ID_RE` |
| Profile subcommands | `hermes_cli/subcommands/profile.py` |
| Multiplexing default | `config_defaults.py` `multiplex_profiles` |
| Update channels | `hermes_cli/subcommands/update.py` |
| Curator timings | `config_defaults.py` (`curator`) |
| Plugin catalog size | `ls plugin-catalog/*.yaml \| wc -l` |
| Claude models in catalog | `hermes_cli/models_catalog_static.py` |

## 4. Patch, don't rewrite

- Add a row to `reference/14-whats-new-since-july.md` (or start a new "what's new" file per quarter).
- Fix any wrong-now fact where it lives; add a line to `CHANGELOG.md`.
- Append a new snapshot to `SOURCE_LOCK.md` (timestamp, version, commit SHA). Never overwrite old snapshots.
- If nothing changed, log "no change" with the date — that's a useful result too.

## 5. Report

A short update with: new releases (version, date, 3 headlines), anything that breaks existing agents, new features
worth adopting (ranked), hub files changed, and one recommended action for this week.
