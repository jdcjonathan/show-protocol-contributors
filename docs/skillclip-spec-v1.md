# SkillClip capture spec v1 (draft)

SkillClips are the unit of value in SHOW. They are **not** social video. They are continuous demonstrations that in-context robot models (e.g. Skild S1-class) can use as prompts.

Creators who cannot hit spec do not mint.

**See also:** [creator-funnel.md](creator-funnel.md) (use cases, camera angles, Tier A/B/C) · [schemas/skill-taxonomy.json](../schemas/skill-taxonomy.json)

## Capture tiers

| Tier | Name | Who films | Minimum input |
| --- | --- | --- | --- |
| **A** | PromptClip | Baristas, housekeeping, cooks | Continuous RGB, 1080p+, one primary camera angle |
| **B** | TrainClip | Distributed creators, campaigns | Tier A + egocentric primary + phases + 5+ variants |
| **C** | Robot-native | Teleop operators | Not creator funnel — poses, force, RLDS |

Most bounties launch at **Tier A**. Tier B pays more for diversity bundles.

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

Pinned alongside video on Tack. Hash committed on-chain before mint.

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

## Proof of capture

1. **C2PA** credentials from SHOW capture app (device, timestamp, app signing key)
2. **ERC-8004** creator identity bound at mint
3. **On-chain hash** of video + sidecar committed before Tack pin completes
4. Scraped or re-uploaded content **fails** attestation

## Mint gate

Mint succeeds only when:

- [ ] Video passes automated spec checks (duration, cuts, hands-in-frame)
- [ ] Sidecar validates against JSON schema
- [ ] C2PA chain verifies
- [ ] Creator has registered ERC-8004 identity
- [ ] Content hash registered on Taiko

## Skill taxonomy (initial wedge)

Aligned with S1 unseen-task demos:

- `pour-over.coffee`
- `pancake.flip`
- `plant.pot`
- `kit.assembly`

Expand only after eval pipeline proves attribution for one family.

## Rejection reasons (non-exhaustive)

- Jump cut detected
- Missing hands at t=0
- Background music or talking head dominant
- Sidecar / video hash mismatch
- No ERC-8004 creator binding
- Consent flags incomplete

## Next artifact

Capture app MVP enforcing this spec + Hoodi mint path.
