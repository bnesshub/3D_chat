# Phase 1 — Base Mesh Generation

Workspace for turning the 4-view turnaround into a 3D base mesh.

See `PHASE_PLAN.md` at the repo root for the full plan and rationale.

## Folder layout

- `candidates/` — every Meshy (or Tripo) generation, keep them all
  - Suggested naming: `cand_<NN>_<tool>_<short-note>.{fbx,glb,zip}` (e.g. `cand_01_meshy_4view-default.fbx`)
- `winner/` — the chosen mesh, exported as both FBX and GLB
  - `liora_base.fbx`
  - `liora_base.glb`
- `notes/` — generation log (`generation_notes.md`) and any per-candidate observations

## Inputs (from Phase 0)

Upload these 4 to Meshy as the multi-view input:

| Slot | File                                                       | Role          |
| ---- | ---------------------------------------------------------- | ------------- |
| 10   | `Phase0_Character_Consistency/02_Reference_Images/10_Front_FullBody.png`     | Primary front |
| 11   | `Phase0_Character_Consistency/02_Reference_Images/11_FullBody_Side.png`      | Left side     |
| 12   | `Phase0_Character_Consistency/02_Reference_Images/12_FullBody_Back.png`      | Back          |
| 13   | `Phase0_Character_Consistency/02_Reference_Images/13_FullBody_Side_Right.png`| Right side    |

## Verification checklist (run in Blender before promoting to `winner/`)

Full step-by-step procedure (Blender install through silhouette render) is in [`BLENDER_VERIFICATION.md`](./BLENDER_VERIFICATION.md). The list below is the summary.

- [ ] Single connected manifold — no holes, no floating chunks
- [ ] Symmetric within ~2% along YZ plane
- [ ] Silhouette test: render front at 1024px, place next to slot 10 — reads as "same character"
- [ ] Hands have 5 separable fingers (Meshy occasionally mittens them)
- [ ] Tri count between 30k and 80k
- [ ] A-pose (not T-pose) — easier to retopo and rig from A-pose

## Decision gate

User explicitly approves the winning mesh before moving to Phase 2 (retopology). Fixing mesh issues post-retopo costs ~10× more.

## If Phase 0.5 trial fails

If Meshy free-tier outputs don't justify the $20/mo Pro subscription, try Tripo free tier as a fallback before paying anywhere. Document the comparison in `notes/generation_notes.md`.
