# SkillClip capture spec v1 (draft)

SkillClips are the unit of **rights** in SHOW. They are not social video. The payload can be a continuous demonstration for in-context models (S1-class), a **robot trajectory**, a **runnable policy**, or a **sim-to-real recipe**.

**Humans and agents mint the same object.** Optional sidecar field `contributor.kind`: `human` | `agent` | `joint`. Optional `donate` object: thanks in USDC, not a license.

Creators (of any kind) who cannot hit the spec for their **training_mode** do not mint.

**Join:** [contributing.md](contributing.md) · **Training modes:** [training-modes.md](training-modes.md) · [training-modes-v1.json](../schemas/training-modes-v1.json) · [embodiments-v1.json](../schemas/embodiments-v1.json)

**See also:** [creator-funnel.md](creator-funnel.md) (use cases, camera angles, Tier A/B/C) · [schemas/skill-taxonomy.json](../schemas/skill-taxonomy.json)

## Capture tiers

| Tier | Name | Who films | Minimum input |
| --- | --- | --- | --- |
| **A** | PromptClip | Baristas, housekeeping, cooks | Continuous RGB, 1080p+, one primary camera angle |
| **B** | TrainClip | Distributed creators, campaigns | Tier A + egocentric primary + phases + 5+ variants |
| **C** | Robot-native | Teleop ops, RL trainers, onboard logs | Trajectories, policies, recipes — not phone-only |

Most bounties launch at **Tier A**. Tier B pays more for diversity bundles. Tier C is how **sim-to-real** skills (e.g. Microduck walk / get-up) mint — see [training-modes.md](training-modes.md).

## Video requirements (Tier A minimum)

| Rule | Requirement |
| --- | --- |
| Duration | 20 seconds – 10 minutes |
| Takes | Single continuous take. No jump cuts. |
| Camera | At least one primary angle (see decision tree in creator-funnel). Dual stream (egocentric + third-person) recommended for long-horizon |
| Frame 0 | Hands and task-relevant objects visible at t=0 |
| Audio | Optional ambient only. No music, no voice-over. |
| Face | Default off. Skill is in the hands. |
| Appendix | Optional 15s failure-and-recovery segment (high value) |

## Sidecar JSON (required)

Pinned alongside the **primary payload** on Tack. Hash committed on-chain before mint.

v1.0 (prompt) uses `video_cid`. v1.1 adds `training_mode` + `embodiment`; policy / recipe / trajectory use `payload.primary_cid`.

Prompt example:

```json
{
  "type": "https://show.protocol/skillclip-v1",
  "skill_id": "pour-over.coffee.v1",
  "objects": ["kettle", "filter", "carafe", "cup"],
  "environment": "commercial_kitchen",
  "location_class": "hotel_breakfast",
  "lighting": "indoor_natural",
  "handedness": "right",
  "embodiment_hint": "bimanual_arm",
  "duration_sec": 142,
  "consent": {
    "commercial_training": true,
    "residual_participation": true,
    "signed_at": "2026-08-30T00:00:00Z"
  },
  "video_cid": "bafy...",
  "sidecar_version": "1.0.0"
}
```

Policy / recipe examples: [sidecar-policy-microduck.example.json](../templates/sidecar-policy-microduck.example.json) · [sidecar-recipe-microduck.example.json](../templates/sidecar-recipe-microduck.example.json)

Optional thanks field: `"donate": { "enabled": true, "currency": "USDC" }` — [donate.md](donate.md). Does not grant commercial rights.

## Proof of capture

1. **C2PA** (prompt) or **training-run attestation** (policy/recipe: seed, commit, sim hash) plus SHOW app or signed sidecar
2. **ERC-8004** creator identity bound at mint
3. **On-chain hash** of primary payload + sidecar committed before Tack pin completes
4. Scraped or re-uploaded third-party weights **fail** unless `hub_publish.creator_certifies_rights` and the bytes are the minter's run

## Mint gate

Mint succeeds only when:

- [ ] Sidecar validates against JSON schema for its `training_mode`
- [ ] Prompt: video passes spec (duration, cuts, hands-in-frame)
- [ ] Policy/recipe/trajectory: `embodiment` set; `payload.primary_cid` + format; preview video recommended
- [ ] Creator has registered ERC-8004 identity
- [ ] Content hash registered on Taiko

## Skill taxonomy (initial wedge)

Human video (Sprint 1): atomic primitives in [skill-taxonomy-atomic.json](../schemas/skill-taxonomy-atomic.json).

Sim-to-real (SUGGESTED): `embodiment_skills` — walk, get-up, skate, beak pick on `pollen.microduck.v1`. No bounties until demand shows up.

## Rejection reasons (non-exhaustive)

- Jump cut detected
- Missing hands at t=0
- Background music or talking head dominant
- Sidecar / video hash mismatch
- No ERC-8004 creator binding
- Consent flags incomplete

## Next artifact

Capture app MVP enforcing this spec + Hoodi mint path.
