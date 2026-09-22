# Training modes — one rights layer, many payloads

SHOW licenses **motor skill**, not a single file format. A SkillClip is the ownership object. What it *contains* depends on how the robot learns.

**Why we exist:** [why-show.md](why-show.md) · **Join:** [contributing.md](contributing.md) · **Taxonomy:** [skill-taxonomy-atomic.json](../schemas/skill-taxonomy-atomic.json) · **Machine index:** [training-modes-v1.json](../schemas/training-modes-v1.json)

---

## The opportunity

Cheap open **sim-to-real** platforms (example: [Pollen Robotics Microduck](https://pollen-robotics.com/microduck/) — $399 desk biped, Apache-2.0 RL stack, policies trained in MuJoCo and exported to ONNX) make a new kind of creator: someone who **trains a gait or a get-up**, not someone who films a pour.

Hugging Face already hosts the files. SHOW is not a second Hub. SHOW is the **license + residual** when another duck (or another lab) **runs your behavior**.

Same protocol loop as video-ICL:

```
discover (free) → preview (free) → evaluate (free) → license (USDC) → deploy
```

Different bytes.

---

## Four modes (v1)

| Mode | What you mint | Who produces it | Robot consumes it as |
| --- | --- | --- | --- |
| **`prompt`** | RGB SkillClip (5–20 s atomic, or longer Tier A) | Human with a phone | In-context video prompt (S1-class) |
| **`trajectory`** | Episode pack (RLDS / proprio + images + actions) | Teleop op, or onboard logs | Imitation / offline RL / eval |
| **`policy`** | Weights that **run** on a named body (ONNX, safetensors) | Anyone who trained in sim or fine-tuned | Direct control at 50 Hz |
| **`recipe`** | Sim env + reward + domain randomization that **reproduces** a policy | RL researchers, classrooms | Retrain / fork / improve |

`prompt` is the kitchen wedge (Sprint 1). `policy` + `recipe` + `trajectory` are how we show up for Microduck-class robots **without waiting for a humanoid cell**.

Tier C in the capture spec used to mean “teleop operators, not creators.” That was too narrow. A student who trains a better **fall recovery** on a $399 biped is a creator. The sidecar just uses `training_mode: policy` and an **embodiment**.

---

## Embodiment is first-class

A grasp on human hands is not a grasp on a duck beak. Agents filter `embodiment` the same way they filter `skill_id`.

Registry: [embodiments-v1.json](../schemas/embodiments-v1.json)

| `embodiment` | Body | Default modes |
| --- | --- | --- |
| `human.hands.v1` | Phone / chest-cam human demo | `prompt` |
| `pollen.microduck.v1` | 25 cm biped, 15 DoF, beak, IMUs | `policy`, `recipe`, `trajectory` |
| `pollen.reachy-mini.v1` | Desktop interaction robot | `policy`, `trajectory` |
| `generic.biped.v1` | Portable across small walkers | `policy`, `recipe` |

New bodies get a new id. Do not overload `human.hands.v1` with robot logs.

---

## What we do **not** build

- An RL trainer, MuJoCo fork, or Pollen SDK
- A Hugging Face replacement (dual-publish: Hub for bytes + SHOW for rights)
- Hardware files (Microduck mechanics are **not** open; do not claim they are)
- Bounties or Hoodi deploys for these modes until the live gauge says both sides exist

We **do** make them discoverable and mintable: skill ids, sidecar 1.1, use cases, TrainingRequest fields.

---

## Microduck skill wedge (SUGGESTED)

These ship in the taxonomy so agents can ask for them **now**. No USDC attached.

| skill_id | Move | Typical payload |
| --- | --- | --- |
| `locomote.walk.forward.v1` | Walk | policy / recipe |
| `locomote.sit.down.v1` | Sit | policy |
| `locomote.crouch.v1` | Crouch | policy |
| `recover.fall.get-up.v1` | Get up after a fall | policy / trajectory (failures are gold) |
| `locomote.skate.roller.v1` | Roller-skate | policy / recipe |
| `grasp.beak.pick.v1` | Pick with beak | policy / trajectory / prompt (human demo still valid as ICL) |
| `locomote.kick.object.v1` | Kick | policy |

Preview for a policy SkillClip **should** include a short video of the real robot (or the sim twin) doing the move. Wallet still last.

---

## Dual-publish with Hugging Face

```
Train in mjlab / Microduck RL
  → push policy + recipe to a HF repo     ← reproduce
  → mint SkillClip on SHOW (sidecar 1.1)  ← own + license
```

Sidecar `hub_publish.repo_id` points at the Hub. The mint is a **different take of rights**, not a re-upload of Pollen’s stock policies. Fork, improve, attest **your** training run.

---

## Gauge (no payment)

Same Moltbook tags. Add optional fields:

```
training_mode: policy
embodiment: pollen.microduck.v1
skill_id: recover.fall.get-up.v1
```

Templates: [training-request-post.md](../templates/moltbook/training-request-post.md) · [skill-offer-post.md](../templates/moltbook/skill-offer-post.md)

---

## Agent filter

```bash
# Human video atomics (default)
curl -s "$BASE/schemas/skill-taxonomy-atomic.json" | jq '.defaults'

# Desk biped / sim-to-real skills
curl -s "$BASE/schemas/skill-taxonomy-atomic.json" | jq '.embodiment_skills'
curl -s "$BASE/schemas/training-modes-v1.json"
curl -s "$BASE/schemas/embodiments-v1.json"
```

Shop: `skill_id` + `embodiment` + `training_mode`. Do not license a kitchen pour ONNX onto a duck.
