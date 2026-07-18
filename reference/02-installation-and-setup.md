# 02 — Installation & Setup

*Source: official README (`NousResearch/hermes-agent`), docs landing page, installed `hermes-agent` skill v2.3.0. Verify anything here with `hermes --help` and `hermes <cmd> --help`.*

## Install (pick one)

**Shell installer (Linux / macOS / WSL2 / Termux)** — sets up uv, Python 3.11, Node.js, ripgrep, ffmpeg, and the launcher:
```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

**Windows (native, PowerShell)** — installs everything incl. a portable Git Bash:
```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

**PyPI:**
```bash
pip install hermes-agent        # or: uv pip install hermes-agent
```

**Desktop app (Windows/macOS):** download the Hermes Desktop installer from the website — it bundles the CLI and the GUI.

After install:
```bash
source ~/.bashrc     # or ~/.zshrc
hermes               # start chatting
```

## The fastest path to a working agent

Per the docs' "FASTEST PATH" callout:
```bash
hermes setup --portal
```
One OAuth via **Nous Portal** covers a model plus the four Tool Gateway tools (web search,
image generation, TTS, browser). This is the lowest-friction way to a fully-capable agent —
no juggling API keys on day one.

If you'd rather bring your own provider, use the wizard/picker instead:
```bash
hermes setup          # full interactive wizard (model|terminal|gateway|tools|agent)
hermes model          # interactive model/provider picker
hermes doctor         # check dependencies and config; add --fix to auto-repair
hermes status --all   # component status
```

## First-run checklist (used in course Module 01)

1. `hermes doctor` — green across the board.
2. `hermes model` — a model is selected and its provider is authenticated.
3. `hermes` — you can hold a conversation and it can call a tool (ask it to run `echo hi`
   in the terminal, or do a web search).
4. `hermes config path` and `hermes config env-path` — know where your settings and secrets live.

## Key paths (memorize these)

```
~/.hermes/config.yaml       Main configuration (settings)
~/.hermes/.env              API keys and secrets
~/.hermes/skills/           Installed skills
~/.hermes/state.db          Canonical session store (SQLite + FTS5)
~/.hermes/logs/             Gateway and error logs
~/.hermes/auth.json         OAuth tokens and credential pools
```
Profiles live under `~/.hermes/profiles/<name>/` with the same layout.

> **Rule:** settings go in `config.yaml`, secrets go in `.env`. Never put API key *values*
> in config.yaml, and never commit `.env` to git.

## Where it can run (terminal backends)

`local` (default), `docker`, `ssh`, `singularity`, `modal`, `daytona`. For an always-on
business agent, the common pattern is a cheap VPS (`ssh`/`local` on the box) or a serverless
backend (`modal`/`daytona`) that hibernates when idle. See `10-multi-agent-control-room.md`
and the real `hermes-remote-db` pattern for a VPS + local Mac split.
