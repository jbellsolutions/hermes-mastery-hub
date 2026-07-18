# Known Limitations & Validation Status

This hub is an educational and operating reference, not a formal workflow engine or official Nous Research product.

## What static validation covers

The release process checks:

- Root `SKILL.md` frontmatter and description limits.
- Existence of local Markdown links.
- Documented CLI syntax against the installed Hermes CLI help where practical.
- Git whitespace errors.
- Accidental credential patterns in repository files.
- Skill discovery/inspection using a temporary Hermes home when supported.

## What cannot be validated without your environment

The following require your accounts, credentials, platforms, data, and explicit approval:

- Telegram/Slack/Discord gateway setup and delivery.
- Stripe, Mailchimp, email, QuickBooks, ad-platform, calendar, and CRM write operations.
- Browser/MCP services that require authentication or paid credits.
- Whether a particular model follows the coaching contract perfectly.
- The business validity of your rate cards, claims, compliance rules, or financial assumptions.

## Coaching behavior is guidance

A skill supplies context and operating instructions. It is not deterministic control logic. Adherence depends on the active model, Hermes version, available tools, and user confirmation. If the coach drifts, say:

> Follow the Hermes Mastery Hub coach contract. Teach one module at a time, verify real output, and stop before every external side effect for approval.

## Version drift

Hermes changes quickly. Commands and configuration may differ in later releases. Run `hermes <command> --help` and consult the live official docs when a command differs. Live documentation is intentionally unpinned.

## Source limitations

- This is a **curated deep-research hub, not an exhaustive survey** of every forum thread,
  Skills Hub entry, repository, video, or community project. Reference 13 provides repeatable
  discovery and review methods for extending it safely.
- The Tony Simons X article body was login-gated during research. The hub confirms only the public card/title and does not claim to summarize the inaccessible article.
- The Mom-n-Pop and Control Room repositories are community examples, not official Nous documentation.
- Course structure, role cards, templates, personal use cases, assessments, and recommendations are original synthesis—not claims copied from source repositories.

## Professional and platform boundaries

- Nothing here is legal, tax, accounting, investment, medical, or compliance advice.
- Cold-email, advertising, data collection, and automation must follow applicable laws, platform terms, and user-approved limits.
- Never provide secrets, card data, passwords, recovery codes, or 2FA codes to the agent.
- Use test/sandbox modes before production money or customer communication.

## Safe adoption path

1. Inspect the hub and its source lock.
2. Load it in a non-production session.
3. Run Modules 01–03 with harmless data.
4. Test approval behavior in a sandbox.
5. Wire one integration at a time.
6. Keep every write action gated until it has earned trust.
