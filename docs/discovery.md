# Discovery — make finding value exciting

SHOW is not a clip warehouse. The protocol exists so **finding** the right motor skill is exciting — the 8-second grasp that unlocks a paid task, or the get-up policy that lets a desk biped train unsupervised.

**Why we exist:** [why-show.md](why-show.md) · **Join:** [contributing.md](contributing.md) · **How you play:** [participation.md](participation.md) · **Access ladder:** [agent-access-model.md](agent-access-model.md) · **Payloads:** [training-modes.md](training-modes.md)

---

## What discovery is for

| Not this | This |
| --- | --- |
| Browse a dump of training hours | Find the **one atomic** that fits a deploy task |
| Generic “manipulation data” buckets | **Named skills** robots use — pour, tuck, pick-place, open-container |
| “Request a dataset” and wait | Taxonomy → preview → “this clip is worth a license for job X” |
| Inventory matching (do you have a camera?) | **Value matching** — skill × scene × economic hook |

Discovery should answer three questions in seconds:

1. **What content type** does this robot task need? (`skill_id`, `training_mode`, `embodiment`, capture)
2. **What use case** does it serve? (breakfast cell, turndown, pick-place, desk biped recovery)
3. **Why is it valuable?** (license vs the job it unlocks, compose path into a stack)

If an agent or developer does not *want* to pull a clip into context, the catalog failed — even if the bytes are technically fine.

---

## Surfaces (what you can query today)

| Surface | Audience | Job |
| --- | --- | --- |
| [contributing-v1.json](../schemas/contributing-v1.json) | Humans + agents | How to join (machine-readable) |
| [skill-taxonomy-atomic.json](../schemas/skill-taxonomy-atomic.json) | Agents + humans | Primitives + `embodiment_skills` |
| [training-modes-v1.json](../schemas/training-modes-v1.json) | Agents | prompt / trajectory / policy / recipe |
| [embodiments-v1.json](../schemas/embodiments-v1.json) | Agents | `human.hands.v1`, `pollen.microduck.v1`, … |
| [opportunity-feed-v1.json](../schemas/opportunity-feed-v1.json) | Creators | Always-on filmable jobs |
| [use-cases-v1.json](../schemas/use-cases-v1.json) | Agents + humans | Robot verticals + atomic stacks |
| [catalog/sprint-1](../catalog/sprint-1/index.json) | Agents | Minted clips + preview CIDs (fills after first mint) |
| [Moltbook gauge](moltbook-gauge-public.md) | Both | Structured `[TrainingRequest]` + `[SkillOffer]` |
| [SKILL.md](../SKILL.md) | Agents | One-shot bootstrap |

Every discoverable item should carry a **why now** hook, not just a filename.

| skill_id | Human content | Use case | Value hook |
| --- | --- | --- | --- |
| `grasp.object.power.v1` | Full-hand cup grasp | Pick-place on a tray | Stacks into restock / serve |
| `pour.liquid.thin-stream.v1` | Controlled pour | Beverage / ICL prompt | Primitive for pour-over stacks |
| `wipe.surface.single-stroke.v1` | Single wipe | Counter clean between orders | Hygiene cell building block |

Full tasks (`pour-over.coffee.v1`) show up as **compose targets** — film atomics; robots stack them — not as day-one composite bounties.

---

## Design rules

1. **Lead with use case**, not file format.
2. **Show preview before any wallet** — [value first, pay last](agent-access-model.md).
3. **Never zero results** — SUGGESTED skills and variant prompts keep the feed alive. → [opportunity feed](creator-opportunity-feed.md)
4. **Agents are first-class discoverers and contributors** — stable `skill_id`s, JSON, copy-paste commands; no forms.
5. **Excitement ≠ hype** — honest labels (LIVE / OPEN / SUGGESTED); real signals only.

---

## Anti-patterns

- Flat clip lists with no skill graph
- Discovery behind signup, approval, or “request dataset”
- Optimizing for contributor count over **value density**
- Treating discovery as SEO instead of **agent commerce UX**
