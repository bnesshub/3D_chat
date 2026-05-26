# Briefing — Next Agent

> **For whoever picks this up next.** This is a session handoff from the agent that closed Phase 0. Read this first, then `Phase0_Character_Consistency/README.md`, then the style guide. Branch: `claude/cartoon-avatar-consistency-aWmxg`.

---

## What This Project Is

`3D_chat` — building a stylized 3D character ("Liora Voss") for an interactive chat application. The project was scoped into phases:

- **Phase 0:** Lock a consistent 2D reference set (done)
- **Phases 1–5:** Not yet documented in-repo. Original intent referenced 3D generation → rigging → animation → interactivity, but no formal plan files exist for these phases.

---

## Current State

**Branch:** `claude/cartoon-avatar-consistency-aWmxg` (pushed, in sync with origin)
**Phase 0 status:** Closed. 13 reference images locked in `Phase0_Character_Consistency/02_Reference_Images/`.

### Reference set composition

| # | File | Source | Notes |
|---|---|---|---|
| 01 | `01_Front_Neutral.png` | ChatGPT | Head + shoulders, neutral expression |
| 02 | `02_ThreeQuarter_Left.jpg` | Grok (v1 painterly) | No ChatGPT replacement yet |
| 03 | `03_ThreeQuarter_Right.jpg` | Grok (v1 painterly) | No ChatGPT replacement yet |
| 04 | `04_Side_Left.png` | ChatGPT | Head + shoulders, true profile |
| 05 | `05_Side_Right.png` | ChatGPT | Head + shoulders, true profile |
| 06 | `06_Back.jpg` | Grok (v1 painterly) | No ChatGPT replacement yet |
| 07 | `07_Front_Happy.png` | ChatGPT | Expression — laughing |
| 08 | `08_Surprised.png` | ChatGPT | Expression — wide eyes, "o" mouth |
| 09 | `09_Smirk.png` | ChatGPT | Expression — ¾-right, sideways glance |
| 10 | `10_Front_FullBody.png` | ChatGPT | Full body front |
| 11 | `11_FullBody_Side.png` | ChatGPT | Full body left side |
| 12 | `12_FullBody_Back.png` | ChatGPT | Full body back |
| 13 | `13_FullBody_Side_Right.png` | ChatGPT | Full body right side |

The Grok originals of replaced slots (01, 04) are preserved in `02_Reference_Images/v1_painterly_archive/`.

---

## Key Decisions Made This Session (and Why)

1. **Switched primary tool from Grok Imagine → ChatGPT/DALL-E mid-phase.** Grok was text-only and produced wide drift between generations. ChatGPT supports image-to-image anchoring ("keep everything, change only X"), which produced dramatically more consistent character results. Recommend continuing with ChatGPT for any new 2D reference work.

2. **Reference set has two co-existing styles.** Slots 02/03/06 are painterly (warm rim light, visible brushstrokes). Slots 01, 04, 05, 07–13 are ChatGPT renders (slightly cleaner, less painterly, similar but not identical). This is intentional — chasing 100% style cohesion would have cost another session of generation. The 3D pipeline cares more about consistent character identity than consistent render style; both sub-styles preserve the same character.

3. **Style guide describes the OLD painterly look** (`05_Locked_Visual_Description.md`). It accurately describes Grok renders 02/03/06 but does NOT match the dominant new ChatGPT style. **This is the biggest unresolved doc-vs-reality gap.** A planner should decide whether to (a) rewrite the visual description to match the new majority style, (b) keep both descriptions side-by-side, or (c) accept the mismatch since the references themselves are the canonical truth.

4. **Master prompt template is stale.** `03_Prompt_Templates/Master_Prompt.txt` was written for Grok with the old painterly style block. It does not reflect the ChatGPT workflow that actually produced most of the locked set.

5. **Full-body 4-view orthographic turnaround was prioritized** (slots 10/11/12/13). This is the standard input for image-to-3D tools (Meshy / Tripo / Rodin / Hunyuan3D). Slots 10–13 are internally consistent in style, which matters more for mesh reconstruction than matching the close-ups.

---

## Open Questions / Known Gaps

- **No formal plan exists for Phases 1–5.** Only Phase 0 has README/docs.
- **¾ left/right close-ups are still Grok-style** — could be regenerated in ChatGPT for full cohesion. Low priority unless a 3D tool complains.
- **Back close-up (06)** is still Grok-style — same situation.
- **Visual description doc references 5 source images** but we now have 13 — doc is out of date.
- **No backup/cloud mirror confirmed** — README mentions it as a Phase 0 deliverable but it was never checked off.
- **3D tool selection has not been made.** Candidates: Meshy, Tripo, Rodin, Hunyuan3D-2, TripoSR. Each has tradeoffs around mesh quality, rigging compatibility, cost, and stylized-character handling.

---

## What a Planner Should Review

If a planner agent comes in, focus the review on these questions:

1. **Roadmap definition for Phases 1–5.** What are the actual phases? What does each deliver? What's the critical path?
2. **3D tool selection for Phase 1.** Given the locked reference set (especially the 4-view full-body turnaround), which tool is the best fit? Consider: mesh quality on stylized characters, rigging-readiness, export formats needed for the eventual chat app, cost/turnaround.
3. **Rigging path.** Auto-rigger (Mixamo, AccuRig) vs manual? Compatible with the chosen 3D tool's output?
4. **Animation strategy.** Is this a real-time avatar (needs blendshapes, lip-sync, IK) or pre-rendered? Affects mesh topology requirements upstream.
5. **Target runtime.** Web (Three.js / Babylon / R3F)? Unity? Unreal? Native mobile? Determines export targets and polycount budget.
6. **Should the style guide be reconciled with the actual reference set,** or is the as-built reference set authoritative enough on its own?
7. **Is the style-mix between slots 02/03/06 and the rest a real problem,** or acceptable for the pipeline?

---

## What an Executor Should Do (If No Planner Review)

If you skip the planner and just continue execution:

1. **Phase 1 — 3D mesh generation.** Try Meshy or Tripo first using `10_Front_FullBody.png` as primary input plus 11/12/13 as additional views. Both tools accept multi-view input and produce rigged FBX/GLB. Compare outputs. Document the choice.
2. **Reconcile docs after Phase 1 produces real output** — at that point you'll know what 3D-side constraints actually matter.

---

## Session History (Compressed)

- Phase 0 started with 6 Grok-generated references (5 close-ups + 1 full-body with auburn color drift)
- Switched to ChatGPT mid-session for image-to-image anchored generation
- ChatGPT produced: 1 close-up right profile, 3 expressions, full-body 4-view turnaround, then re-shoots of front-neutral / side-L / side-R close-ups
- Encountered phone-screenshot artifacts on one batch — cleaned via PIL (paint-over for tiny corner artifacts, crop for one bottom-strip case)
- Archived replaced Grok originals to `v1_painterly_archive/` rather than deleting

---

## How to Resume

```bash
git checkout claude/cartoon-avatar-consistency-aWmxg
ls Phase0_Character_Consistency/02_Reference_Images/
cat Phase0_Character_Consistency/01_Style_Guide.md
cat Phase0_Character_Consistency/04_Notes.txt
```

The user (billyness483@gmail.com) is the project owner and is hands-on with image generation tooling (uses ChatGPT mobile app for generation). Prefers concise updates and direct verdicts over long deliberations. Asks for explicit approval before destructive actions.
