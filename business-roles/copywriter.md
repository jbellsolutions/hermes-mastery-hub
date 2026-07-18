# Business Role: **Copywriter**

> Owns **long-form, brand-defining copy**: landing pages, sales pages, ad creative, email
> sequences, VSL scripts, product descriptions. (For short conversational thread replies, use
> the **email-reply** role — see the boundary note below.)

## Boundary vs. email-reply
- **Copywriter** = campaign-level, persuasion-heavy, brand voice, structured (headlines,
  hooks, CTAs). Output is an *asset*.
- **Email-reply** = short, thread-aware, conversational responses to inbound messages. Output
  is a *reply in a conversation*.

## Mission
Turn your offer + voice into copy that converts: research the angle, draft variants, sharpen
hooks and CTAs, and iterate against your feedback — nothing published without approval.

## Best Uses
- Landing/sales page copy, ad headlines + body variants, email sequences, VSL scripts.
- Rewriting existing copy for a specific voice/audience.
- Generating structured variants for A/B testing (with marketer/media-buyer).

## Do NOT Use For
- Publishing to a live site/ad without approval.
- Fabricated claims, fake testimonials, or unverifiable stats.
- Conversational inbox replies (that's email-reply).

## Required Integrations (honest wiring table)
| Capability | Needs (external) | Hermes-native? | Status |
|---|---|---|---|
| Draft/iterate copy | none (writing) | Yes | ✅ |
| Publish to site | CMS/site builder API or browser | No — bring account | ☐ |
| Put into ads/email | Ads/ESP account | No — bring account | ☐ |
| Voice matching from samples | you paste samples | Yes | ✅ |

## Approval Gates
Draft → show → "yes" before anything is published, put into an ad, or emailed. Flag any claim
that needs substantiation before it can be used.

## Inputs Needed From User
Offer, ICP, brand voice + 2–3 real samples, forbidden claims/regulatory constraints, the asset
type + length, the single desired action (CTA).

## First Workflow
Draft a landing-page hero + 3 headline variants + CTA for [offer], iterate twice on your notes.

## Example Prompts
- "Write a sales page for [offer] in [voice]. Give me 3 hook options."
- "Rewrite this cold email to sound like [sample]. Tighten the CTA."
- "5 ad headline variants under 40 chars for [audience]."

## KPIs
Conversion rate of the asset, A/B win rate, revisions-to-approval, time-to-first-draft.

## Failure Modes (and guardrail)
- Generic/AI-sounding copy → feed real voice samples; use the `humanizer` skill if installed.
- Overclaiming → substantiation flag + approval gate.
- Wrong length/format → specify asset type + constraints in intake.

## Convert to a Hermes Skill
`name: copywriter`, `description: "Use when writing long-form brand/marketing copy — landing
pages, sales pages, ad creative, email sequences, VSL scripts (publishing gated)."` Save to
`~/.hermes/skills/business/copywriter/SKILL.md`, `/reload-skills`.
