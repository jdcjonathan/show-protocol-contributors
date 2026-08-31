# Creator opportunity feed — always-on jobs

**Version 0.1 · August 2026**

Related: [luel-onboarding.md](competitive/luel-onboarding.md) · [atomic-skills-sprint-1.md](atomic-skills-sprint-1.md) · [mission-and-queue.md](mission-and-queue.md)

---

## Problem (from Luel)

Fresh signup → **Browse Tasks** → **1 available opportunity**, status **Coming soon**, Record disabled.

Contributors complete a 3-step profile and wait for approval, then land on an empty marketplace. Supply dies before the first clip.

**SHOW rule:** the opportunity feed is **never empty**. Creators should always see something they can film **today**, with or without funded escrow.

**Treasure hunt:** each card is a **branch prompt** — try a variant, explore a sparse niche, maybe robots breed your clip later. Not a shift assignment. → [participation-picbreeder.md](participation-picbreeder.md)

---

## Design principles

| Principle | SHOW | Luel (observed) |
| --- | --- | --- |
| **Create before qualify** | Film first; spec gate at upload | Profile + device + approval before tasks |
| **Never zero listings** | Minimum 10 visible cards at all times | 1 “coming soon” card |
| **Honest labels** | LIVE / OPEN / SUGGESTED / PRACTICE | “Coming soon” without alternative |
| **No hardware gate** | Phone 2021+ implied in brief, not blocked | Device inventory filters tasks |
| **Permissionless submit** | OPEN + SUGGESTED accept uploads | Record disabled until campaign opens |

**Engagement without deception:** SUGGESTED and PRACTICE cards must say clearly whether USDC is attached. Never imply payout when escrow is empty.

---

## Opportunity tiers

Every card in the feed has an `availability_tier`:

| Tier | Pays USDC? | Escrow required? | Creator action | Purpose |
| --- | --- | --- | --- | --- |
| **LIVE** | Yes — fixed bounty | Yes (TaskEscrow or manual) | Record → mint → payout | Funded Sprint bounties |
| **OPEN** | Yes — on accept | No — protocol accepts if spec passes | Record → mint → bounty when funded | Always-on atomic slots |
| **SUGGESTED** | Maybe — “up to $X when funded” | No | Record → mint → queue for review | Keep feed full; seed catalog |
| **PRACTICE** | No | No | Record → local save or free mint | Onboard without wallet; learn spec |

**Phase 0 default mix (minimum 10 cards):**

- 3× **LIVE** — Sprint 1 skills with manual USDC payout committed  
- 5× **OPEN** — same skills, permissionless mint, payout within 24h if spec passes  
- 2× **SUGGESTED** — Sprint 2 preview skills (e.g. `filter.place.v1`, `jar.twist.open.v1`)  
- 0× **PRACTICE** until capture UI exists — use OPEN instead  

When LIVE slots fill, downgrade card to OPEN (still filmable) — never remove the card.

---

## Feed UX (v0 — static JSON + landing pages)

No app required for Phase 0. Ship:

1. **`schemas/opportunity-feed-v1.json`** — canonical feed (agents + humans)  
2. **One HTML/Markdown index** — “Browse bounties” linked from README  
3. **Per-skill URLs** — `/bounty/{skill_id}` with example clip + sidecar form  

### Empty-state forbidden

```javascript
// Pseudocode — feed renderer invariant
assert(feed.opportunities.filter(o => o.status !== 'archived').length >= 10)
```

If fewer than 10 LIVE/OPEN items exist, **backfill from SUGGESTED** templates in taxonomy JSON until count ≥ 10.

### Card copy patterns

| Tier | Badge | CTA | Subtext |
| --- | --- | --- | --- |
| LIVE | `Live · $10` | **Record now** | “3 slots left · paid on accept” |
| OPEN | `Open · $10` | **Submit clip** | “No approval needed · paid when accepted” |
| SUGGESTED | `Suggested` | **Film anyway** | “Not funded yet · mint free · bounty when live” |
| PRACTICE | `Practice` | **Try the spec** | “Learn the format · no payment” |

---

## Qualifications: encourage, don’t block

**Do not** hide opportunities behind:

- Device checklist completion  
- Country / language profile  
- Application review  
- “Matching” algorithm with zero results  

**Do** use soft nudges:

- “Best with chest mount” (not required)  
- “Kitchen table props” (any cup OK)  
- Gold example clip — copy the move, not the gear  

Upload gate is **automated spec only**: duration, resolution, hands visible, one skill_id per clip.

---

## Backfill strategy (when supply is thin)

Priority order to generate SUGGESTED cards:

1. **Sprint 1 atomics** not yet in LIVE — rotate OPEN  
2. **Sprint 2 preview** from [gtm-atomic-launch.md](gtm-atomic-launch.md)  
3. **Variant prompts** — same skill_id, different object (“grasp with mug / bottle / box”)  
4. **Environment variants** — “same grasp, different lighting” (Tier B seed)  
5. **Composite teasers** — “film `grasp` now; robots stack into pour-over later”  

Script: `scripts/seed-opportunity-feed.ts` (P3) reads taxonomy + PROGRESS counts → emits feed JSON.

---

## Agent discoverability

Same feed serves robot agents:

- Filter `availability_tier in [LIVE, OPEN]` for paid acquisition  
- Filter `status=accepted` + `skill_id` for catalog depth  
- SUGGESTED clips with mints become discoverable SkillClips even before bounty  

---

## Phase 0 checklist

| # | Item | Status |
| --- | --- | --- |
| F0.1 | `opportunity-feed-v1.json` seeded (≥10 cards) | Done |
| F0.2 | Link feed from README + atomic-skills-sprint-1 | Next |
| F0.3 | 3 LIVE bounties with committed USDC (manual) | Not started |
| F0.4 | Per-skill landing stub or doc anchor | Not started |
| F0.5 | PROGRESS auto-reflects slots remaining | Not started |

---

## Changelog

| Date | Notes |
| --- | --- |
| 2026-08-30 | Created after Luel empty browse-tasks capture; always-on feed rule |
