# 13 — Community Discovery & Skill Curation

Hermes has a large and fast-moving community ecosystem. Discovery is useful; installing arbitrary agent instructions is a supply-chain decision. Treat every new skill as code review, even when it is “just Markdown.”

## Where to discover skills and implementations

### Official surfaces

- **Skills Hub:** https://hermes-agent.nousresearch.com/docs/skills
- **Hermes repository:** https://github.com/NousResearch/hermes-agent
- **Issues and discussions through GitHub:** https://github.com/NousResearch/hermes-agent/issues
- **Nous Research Discord:** https://discord.gg/NousResearch

The Skills Hub catalog loads dynamically. This research verified that the surface exists but did **not** claim a stable catalog count or successfully crawl a catalog API.

### CLI discovery (verify syntax on your installed version)

The observed Hermes CLI supports:

```bash
hermes skills browse
hermes skills search "crm"
hermes skills inspect <identifier>
hermes skills install <identifier>
hermes skills check
hermes skills update
hermes skills audit
hermes skills tap --help
hermes skills publish --help
```

Run `hermes skills --help` and the specific subcommand’s `--help` before use. Prefer `inspect` before `install`.

### GitHub discovery searches

Useful starting searches:

- https://github.com/search?q=%22SKILL.md%22+%22hermes-agent%22&type=code
- https://github.com/search?q=topic%3Ahermes-agent&type=repositories
- https://github.com/search?q=org%3ANousResearch+SKILL.md&type=code
- https://github.com/NousResearch?tab=repositories

Search by source/domain as well as function. For example, search for a specific CRM, ad platform, or email provider rather than only “business automation.”

## Curation rubric: inspect before trust

### 1. Confirm the format

A normal Hermes skill should have:

- A `SKILL.md` beginning at byte zero with `---`.
- Valid YAML frontmatter with `name` and a trigger-focused `description`.
- A non-empty Markdown body.
- Optional support files only in understandable directories such as `references/`, `templates/`, `scripts/`, or `assets/`.

Unexpected binaries, installers, obfuscated scripts, or encoded payloads require much deeper review.

### 2. Read the trigger and scope

Ask:

- When will this skill load?
- Is the description so broad that it will hijack unrelated tasks?
- Does it clearly state “don’t use for” boundaries?
- Does it distinguish read-only work from external writes?

### 3. Inspect prerequisites and permissions

List every required:

- Environment variable or secret.
- Shell command/package.
- MCP server or remote URL.
- Filesystem path.
- External account and permission scope.

Reject a skill that asks for broader credentials than its job requires.

### 4. Review every executable artifact

Before running anything:

- Read all scripts line by line.
- Check network destinations and downloaded packages.
- Look for destructive shell operations, credential collection, persistence, or broad filesystem traversal.
- Confirm commands use test/sandbox modes where appropriate.
- Do not run `curl | bash` or equivalent installers without independently inspecting the retrieved content.

### 5. Check provenance, maintenance, and license

- Who authored it?
- Is the repository active and reviewable?
- Is there a license permitting your use?
- Are issues and pull requests healthy?
- Does the skill cite official API docs or merely assert behavior?

No license usually means no automatic right to redistribute or adapt it.

### 6. Test in isolation

- Inspect first: `hermes skills inspect <identifier>`.
- Use a separate Hermes profile or temporary `HERMES_HOME` when practical.
- Use dummy data and sandbox/test API keys.
- Keep approvals manual.
- Disable network, terminal, browser, or write tools the skill does not need.
- Verify actual output before trusting a claim.

### 7. Pin known-good skills and maintain them

Once a skill is trusted and important:

- Record its source and revision.
- Pin agent-created skills you do not want archived: `hermes curator pin <name>` (verify with local help).
- Re-review updates before applying them.
- Patch discovered pitfalls into your local copy and retest.
- Keep a backup/export of business-critical profiles and skills.

## Red flags: do not install casually

- Requests raw passwords, card numbers, recovery codes, or 2FA.
- Wants all environment variables rather than a narrow allowlist.
- Includes hidden/encoded payloads or unknown binaries.
- Disables approvals, secret redaction, or security scanning as a prerequisite.
- Uses destructive commands without a backup and explicit gate.
- Sends data to an unrelated third-party endpoint.
- Claims guaranteed revenue, unrestricted platform automation, or compliance without evidence.
- Has no license but invites redistribution.

## Safe adoption sequence

1. Discover.
2. Inspect source and license.
3. Review all scripts and requested credentials.
4. Test in a disposable profile with dummy data.
5. Verify read-only behavior.
6. Test external writes in sandbox with manual approvals.
7. Pin the reviewed revision and document the result.

The best skill collection is not the largest one. It is the smallest reviewed set that reliably changes the agent’s behavior in useful, bounded ways.
