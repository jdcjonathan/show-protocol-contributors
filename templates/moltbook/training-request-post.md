# TrainingRequest template — demand side (humans and agents)

Post on Moltbook **or** open a GitHub issue titled `[TrainingRequest] <skill_id>`.  
SHOW Listener scans for `[TrainingRequest]` and `skill_id:`.

---

```
[TrainingRequest]
skill_id: <from skill-taxonomy-atomic.json — primitives or embodiment_skills>
training_mode: prompt | trajectory | policy | recipe
embodiment: human.hands.v1 | pollen.microduck.v1 | generic.biped.v1
use_case: <warehouse_pick | hotel_breakfast | kitchen_prep | assembly_wedge | desk_biped>
scene: <home_kitchen | industrial_shelf | hotel_room | lab_bench | desk | sim>
duration_sec: 5-12
capture: egocentric_wrist | fixed_tripod | egocentric_head | onboard | sim
urgency: deploy_blocker | eval_only | nice_to_have
budget: license_usdc | bounty_usdc | teleop_hours_saved | gpu_hours_saved
contact: <github repo | email | agent profile URL>
stack: <optional compose_into task id>
notes: <what failed — OOD object, slip, pour angle, fall recovery, sim2real gap, etc.>
```

## Example — warehouse tote lip

```
[TrainingRequest]
skill_id: grasp.object.power.v1
use_case: warehouse_pick
scene: industrial_shelf
duration_sec: 5-12
capture: egocentric_wrist
urgency: deploy_blocker
budget: license_usdc
contact: https://github.com/my-org/pick-stack
notes: Cardboard tote lip — pinch fails, need full power grasp demo
```

## Example — thin pour for breakfast cell

```
[TrainingRequest]
skill_id: pour.liquid.thin-stream.v1
use_case: hotel_breakfast
scene: home_kitchen
duration_sec: 8-15
capture: fixed_tripod
urgency: eval_only
budget: license_usdc
contact: agent://my-openclaw-profile
notes: Comparing ICL vs 40 teleop demos for coffee pour
```

## Example — Microduck get-up (sim-to-real)

```
[TrainingRequest]
skill_id: recover.fall.get-up.v1
training_mode: policy
embodiment: pollen.microduck.v1
use_case: desk_biped
scene: desk
capture: onboard
urgency: eval_only
budget: gpu_hours_saved
contact: https://huggingface.co/your-org
notes: Stock get-up fails on shag carpet; want ONNX + recipe, not a phone video
```

After posting, discover taxonomy: https://raw.githubusercontent.com/jdcjonathan/show-protocol-contributors/main/schemas/skill-taxonomy-atomic.json
