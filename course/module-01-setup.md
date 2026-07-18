# Module 01 — Setup

## Outcome
A working Hermes agent you can talk to, with a healthy config and a selected, authenticated model.

## Required reading
- ../reference/02-installation-and-setup.md
- ../reference/12-troubleshooting.md

## Exercise
1. Install (if not already): `curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash` (or the PowerShell one-liner on Windows).
2. Fastest path to a capable agent: `hermes setup --portal` (one OAuth = model + web/image/TTS/browser). Or bring your own: `hermes model`.
3. Run `hermes doctor` and resolve anything not green.
4. Start a chat: `hermes`. Ask it to run `echo hello` in the terminal and to do one web search.
5. Load this hub: `cp -R ~/Desktop/hermes-mastery-hub ~/.hermes/skills/hermes-mastery-hub` then `hermes -s hermes-mastery-hub`.
6. Say: **"Be my Hermes coach. Start the weekend bootcamp."**

## Artifact
- A running agent that can execute a terminal command and a web search.
- The hub loaded as a skill.

## Verification checklist
- [ ] `hermes doctor` is green
- [ ] A model is selected and its provider is authenticated (`hermes status`)
- [ ] The agent successfully ran `echo hello` and returned real output
- [ ] `hermes config path` and `hermes config env-path` known to you

## Coach notes
If the model errors, check `.env` for the right key and try `hermes auth`. If a tool is
missing, `hermes tools` then `/reset`. If anything is weird, `hermes --ignore-rules` to test
whether it's your setup or Hermes itself.

## Next action
Proceed to Module 02 — Skills.
