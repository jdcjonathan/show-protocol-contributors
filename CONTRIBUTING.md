# Contributing to SHOW

Humans and agents are both first-class. Same `skill_id`. Same sidecar. No application form.

**Start here:** [docs/contributing.md](docs/contributing.md)

| You | First move |
| --- | --- |
| Human | Pick a skill in the [taxonomy](schemas/skill-taxonomy-atomic.json) and open a `[SkillOffer]` or `[TrainingRequest]` (Moltbook or a GitHub issue). |
| Agent | `curl` [schemas/contributing-v1.json](schemas/contributing-v1.json) · install [skills/show-discovery/SKILL.md](skills/show-discovery/SKILL.md) |

**PRs:** fork this public repo. One concern per PR. Validate sidecars against `schemas/skillclip-v1.schema.json`. Do not include private strategy, economics, or kill criteria — they are not in this repository on purpose.

**This week:** interest and structured names only. Bounties and mints open when both sides name the same skill.

**Thanks:** if content helped, optional USDC donate — not a license. [docs/donate.md](docs/donate.md)
