# Phase 1+ Implementation Plan — Liora Voss 3D Chat Avatar

## Context

Phase 0 closed last session with 13 locked 2D reference images for the character "Liora Voss" (on branch `claude/cartoon-avatar-consistency-aWmxg`). The project is **3D_chat** — building a stylized 3D avatar that speaks back to the user via LLM, with lip-sync and full-body animation.

No formal plan existed for Phases 1–5 — the Phase 0 README mentioned them but never defined them. This plan defines that work now that the reference set is stable.

**Locked decisions (this session):**

- **Runtime:** Unity (stronger stylized character + lip-sync ecosystem than Unreal)
- **Behavior:** Voice chat with LLM, avatar lip-syncs to TTS output
- **Animation scope:** Full body + face (talking, gesturing, walking, blendshapes)
- **Style fidelity:** Match the painterly look as closely as possible (NPR/toon shading) — the hardest constraint
- **Budget:** Mixed — pay for tools that meaningfully unlock the project, free everywhere else

**Intended outcome:** A demo-able Unity prototype where you press a button, speak, and Liora responds in voice with lip-synced mouth animation in her painterly style. Target effort: 4–7 weekends.

---

## Phase Overview

| #   | Phase                  | Goal                                                                   | Effort      | Risk    |
| --- | ---------------------- | ---------------------------------------------------------------------- | ----------- | ------- |
| 0.5 | Setup                  | Meshy trial, folder scaffold, validate tool fit before paying          | 1 evening   | Low     |
| 1   | Mesh generation        | Base 3D mesh from 4-view turnaround → FBX/GLB                          | 1–2 days    | Low     |
| 2   | Style preservation     | Retopology + painterly textures + NPR shader                           | 1–2 weeks   | **HIGH**|
| 3   | Rig + blendshapes      | Humanoid rig + ~23 facial blendshapes (visemes + emotions)             | 2–4 days    | Medium  |
| 4   | Unity scene + animation| Idle, talking, gesture animation states under stylized lighting        | 3–5 days    | Low     |
| 5   | Voice loop             | STT → LLM → TTS → lip-sync wired end-to-end                            | 2–3 days    | Low     |
| 6   | Polish                 | Idle behaviors, emotion-tagged talking, demo build                     | 2–4 days    | Low     |

---

## Phase 0.5 — Setup (do this first, ~1 evening)

Before any subscriptions, validate Meshy actually works for THIS character.

1. Create branch `phase1-mesh-generation` off the current Phase 0 branch
2. Create `Phase1_Mesh/` folder structure with subdirs: `candidates/`, `winner/`, `notes/`
3. Sign up for Meshy free tier (no payment yet)
4. Upload slot 10 as primary input, 11/12/13 as additional views, generate 1–2 candidates
5. **Decision gate:** Are the outputs good enough to justify the $20/mo Pro subscription? If yes → upgrade and go to Phase 1. If no → try Tripo free tier as fallback before paying.

This step exists because tool quality on stylized characters varies a lot, and the $20 commitment should follow proof, not precede it.

---

## Phase 1 — Base Mesh Generation

**Tool:** Meshy AI Pro (~$20/mo) — best stylized-character training data, native 4-view input, Mixamo-compatible auto-rigging.

**Inputs:** Slots 10 (front), 11 (left-side), 12 (back), 13 (right-side) from `Phase0_Character_Consistency/02_Reference_Images/`. All four full-body shots are in the same cleaner style, internally consistent — ideal for multi-view 3D solvers.

**Deliverables:**

- `Phase1_Mesh/winner/liora_base.fbx` — A-pose, watertight, 30–80k tris
- `Phase1_Mesh/winner/liora_base.glb`
- `Phase1_Mesh/notes/generation_notes.md` — prompt, view weights, iteration count
- 3–5 candidate generations preserved in `candidates/`

**Verification (open in Blender):**

- Single connected manifold (no holes, no floating chunks)
- Symmetric within ~2% along YZ plane
- Silhouette test: render front view at 1024px, compare to slot 10 — "same character" passes
- Hands have 5 separable fingers (Meshy occasionally mittens them)

**Decision gate:** User explicitly approves mesh before Phase 2. Fixing mesh issues post-retopo costs ~10× more.

---

## Phase 2 — Style Preservation (THE HARD PHASE)

The whole project hinges on this. Image-to-3D tools produce PBR meshes by default — making one look painterly in real-time Unity is the open problem. Plan everything around protecting this step.

Three sub-steps, in order:

### 2a. Retopology — Blender + Quad Remesher add-on ($60 one-time)

Meshy output is generation-quality, not animation-quality. Need clean edge loops around eyes/mouth/joints for blendshapes and deformation. Target: 15–25k tris, quad-dominant, proper face loops.

### 2b. Painterly texture via camera projection bake

This is the trick that captures the brush-stroke feel without hand-painting from scratch:

1. UV-unwrap the retopo'd mesh
2. Set up Blender cameras matching the 4 reference angles
3. Project slots 10/11/12/13 onto the mesh as textures
4. Bake the projection into a 4K diffuse texture — this transfers the painterly feel directly
5. Generate a low-strength normal map (soft normals, not crisp PBR detail)
6. Hand-fix UV seams in Krita/Photoshop (~2–4 hours)

**Fallback** if projection seams are unfixable: Stable Diffusion + ControlNet (Depth) to generate stylized texture maps from the bake. Free, slower iteration.

### 2c. Unity NPR shader — Lux URP Essentials ($30, Unity Asset Store)

URP-native, rim lighting (matches the warm rim signature from references), supports stylized specular. Buy this — do **NOT** write from scratch.

**Why not other shaders:** UTS2 is anime-cel-shaded (wrong style direction). Toon Shader Mobile is free but thinner feature set. Lux is production-quality, well-supported, the right fit.

Tune in-engine with reference images side-by-side until silhouette and shading match.

**Verification:** Render the Unity-shaded model in T-pose from the 4 reference angles at 1024px. Place next to slots 10/11/12/13. Honest viewer says "same character, same style" without qualifiers.

**Decision gate (THE BIG ONE):** Is the NPR shader hitting the painterly look, or are we forcing it? If after 2 weeks of iteration the result still reads as "generic PBR with toon shading on top" — invoke stop-condition (see bottom).

---

## Phase 3 — Rigging + Blendshapes

**Body rig:** Mixamo auto-rig (free) — uploads to mixamo.com, downloads with Unity-Humanoid-compatible skeleton. **Fallback** if Mixamo struggles with the painterly textures: AccuRig (free, Reallusion).

**Facial blendshapes** (the enabler for lip-sync):

- **15 visemes** (Oculus standard) — covers all phonemes for clean lip-sync, future-proofs vs uLipSync's 5-shape minimum
- **8 emotion shapes** — happy, sad, surprised, angry, blink-L, blink-R, brow-up, brow-down

Slots 07 (happy), 08 (surprised), 09 (smirk) are direct visual reference for emotion shapes. Sculpt each shape in Blender by duplicating the head mesh and reshaping (~30 min per shape, ~12 hours total).

**Verification:** Imports into Unity as Humanoid rig with no errors, Mixamo "Idle" animation plays cleanly, all blendshapes drive correctly via Animator, no texture stretching at extreme joint angles.

**Decision gate:** If shoulder/elbow deformation looks broken, bounce back to Phase 2a (retopology). Budget for this.

---

## Phase 4 — Unity Scene + Animation System

- Unity 2022 LTS, URP project (matches Lux shader)
- Mecanim Animator: Idle, Talking, Gesturing, Walking states
- Mixamo animations (free): "Idle (Breathing)", "Talking 1/2/3", "Wave", "Walking"
- 3-point lighting matching the warm rim signature from references — NPR shaders are sensitive to scene lighting, may need a camera-following rim light rig

**Verification:** 60fps on mid-tier GPU, animations blend cleanly, painterly look survives in motion (not just stills).

**Decision gate:** Does the lighting in-scene preserve the painterly look?

---

## Phase 5 — Voice Loop (STT → LLM → TTS → Lip-sync)

| Component | Tool                                       | Cost                       |
| --------- | ------------------------------------------ | -------------------------- |
| STT       | OpenAI Whisper API (or local whisper.cpp)  | $0.006/min or free         |
| LLM       | Claude Haiku (Anthropic API)               | ~$0.10–0.30 per long convo |
| TTS       | ElevenLabs                                 | $5–22/mo                   |
| Lip-sync  | uLipSync (free, MIT, Unity package)        | Free                       |

**LLM choice:** Default to Claude Haiku — fast, cheap, you already have the Anthropic SDK loaded in your workflow. If you'd rather use GPT-4o-mini you'd save ~$0.10/convo but lose vendor consistency.

**TTS choice:** ElevenLabs — voice quality is the difference between "tech demo" and "feels alive." Design "Liora" voice once and lock it.

**Architecture:**

```
[Mic input] → Whisper (STT) → text
           → Claude Haiku → response text
           → ElevenLabs (TTS) → audio + phoneme timing
           → uLipSync drives viseme blendshapes
           → Unity AudioSource plays audio
```

**Verification:** End-to-end loop, user speaks, Liora responds in voice within 3–4 seconds, mouth moves in sync (visemes match phonemes ±100ms).

**Decision gate:** If latency feels too slow, swap to local whisper.cpp (saves ~1s) or use ElevenLabs streaming TTS.

---

## Phase 6 — Polish + Demo Build

- Subtle idle behaviors: eye saccades, blinks, weight shifts (these make avatars feel alive)
- Emotion-tagged talking: ask the LLM to return emotion tags in JSON, drive emotion blendshapes during response audio
- Build standalone Windows/Mac executable

**Verification:** Hand the demo to someone unfamiliar with the project. After a 2-minute voice conversation they say "that felt like talking to a character, not a tech demo."

---

## Tool Summary

| Phase | Paid                                                | Free                                       |
| ----- | --------------------------------------------------- | ------------------------------------------ |
| 0.5   | —                                                   | Meshy free trial                           |
| 1     | Meshy Pro (~$20/mo)                                 | Blender                                    |
| 2     | Quad Remesher ($60), Lux URP Essentials ($30)       | Blender, Krita, SD+ControlNet (fallback)   |
| 3     | —                                                   | Mixamo, AccuRig (fallback), Blender        |
| 4     | —                                                   | Unity 2022 LTS, Mixamo animations          |
| 5     | ElevenLabs ($5–22/mo), Whisper API, Claude API      | uLipSync                                   |
| 6     | —                                                   | Unity build pipeline                       |

**Total upfront paid:** $20/mo (Meshy) + $90 one-time (Quad Remesher + Lux) + variable API costs.

---

## Risks and Off-Ramps

| Risk                                          | Likelihood | Mitigation                                                                                                  |
| --------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------- |
| Phase 2 NPR can't match references            | High       | See off-ramps below                                                                                         |
| Meshy mittens the hands                       | Medium     | Retry with hand-emphasized prompt or sculpt fingers in Blender                                              |
| Retopology destroys facial detail             | Medium     | Keep higher poly count near face                                                                            |
| Mixamo doesn't fit painterly proportions      | Low-Med    | Switch to AccuRig or manual Rigify                                                                          |
| uLipSync visemes look wrong on stylized mouth | Medium     | Reduce blendshape intensity to ~60% — literal phoneme matching reads weirder than implied motion on stylized characters |
| Shading "swims" under animation               | Medium     | Bake more shading into diffuse, reduce shader contribution                                                  |

### Off-ramps if Phase 2 fails

**Off-ramp A — Hybrid PBR + painterly post-FX.** Keep Meshy's PBR materials, add Unity post-processing (color grading, soft bloom, Painterly Toolkit asset ~$40). Quality drops from "painterly illustration" to "stylized 3D with painterly post." Most viewers won't notice. Ships faster.

**Off-ramp B — Clean cel-shading.** Switch to Arcane/Genshin-style hard cel-shaded toon. Hides retopo issues, doesn't try to look painted, bigger asset ecosystem. ~3–5 days texture rework. Style shifts but stays high-quality.

**Off-ramp C — VRoid Studio rebuild.** Abandon image-to-3D. Hand-craft a VRoid character that resembles Liora using its anime-stylized base. Loses reference fidelity but gains a fully-rigged-and-blendshaped VRM character optimized for exactly this use case. ~1 weekend rebuild. Style shifts to anime/VTuber.

**Not recommended:** Switching engines (loses entire Unity stylized ecosystem). Switching to web/Three.js (rebuilds shader and animation system from scratch).

---

## Critical Files / Inputs

- `Phase0_Character_Consistency/02_Reference_Images/10_Front_FullBody.png` — Phase 1 primary input
- `Phase0_Character_Consistency/02_Reference_Images/11_FullBody_Side.png` — Phase 1 additional view
- `Phase0_Character_Consistency/02_Reference_Images/12_FullBody_Back.png` — Phase 1 additional view
- `Phase0_Character_Consistency/02_Reference_Images/13_FullBody_Side_Right.png` — Phase 1 additional view
- `Phase0_Character_Consistency/02_Reference_Images/07_Front_Happy.png` — Phase 3 emotion blendshape target
- `Phase0_Character_Consistency/02_Reference_Images/08_Surprised.png` — Phase 3 emotion blendshape target
- `Phase0_Character_Consistency/02_Reference_Images/09_Smirk.png` — Phase 3 emotion blendshape target
- `BRIEFING_NEXT_AGENT.md` — full Phase 0 handoff context

**Documentation drift to defer (not blocking):** the Phase 0 style guide and master prompt template describe the older painterly Grok style; they're out of sync with the current ChatGPT-rendered majority of the reference set. Fix in a later cleanup pass when style guide actually matters again (probably Phase 2 when shader-tuning).

---

## Decision Points Requiring User Judgment

1. **End of Phase 0.5:** Is Meshy worth $20/mo for this character?
2. **End of Phase 1:** Is the mesh actually Liora?
3. **End of Phase 2a:** Are the face loops clean enough for blendshapes?
4. **End of Phase 2c (BIG ONE):** Does the Unity render match the painterly references?
5. **End of Phase 3:** Do blendshape expressions read as Liora's expressions?
6. **End of Phase 4:** Does she look painterly in motion, not just in stills?
7. **End of Phase 5:** Does the voice fit the character?
8. **End of Phase 6:** Would you ship this to a friend as a demo?

---

## What Happens Tomorrow

1. Branch off: `git checkout -b phase1-mesh-generation`
2. Create `Phase1_Mesh/` folder structure
3. Sign up for Meshy free tier
4. Upload slots 10/11/12/13, generate 1–2 candidates
5. Evaluate against the Phase 1 verification checklist
6. If passing: subscribe to Meshy Pro, run Phase 1 properly
7. If failing: try Tripo free tier before paying anywhere
