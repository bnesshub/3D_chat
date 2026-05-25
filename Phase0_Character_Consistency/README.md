# Phase 0 — Character Consistency Foundation

**Goal:** Create a locked, reusable reference set so every later stage (3D generation, rigging, animation, interactivity) produces a single, coherent character instead of a drifting one.

**Time:** 1–2 days (4–10 hours total)
**Difficulty:** Easy–Medium
**Impact on final quality:** Highest single lever in the pipeline.

---

## Folder Layout

```
Phase0_Character_Consistency/
├── 01_Style_Guide.md           # Single source of truth for the character
├── 02_Reference_Images/        # Locked multi-view PNGs (front, side, back, ¾, expressions)
│   └── README.md
├── 03_Prompt_Templates/
│   └── Master_Prompt.txt       # Copy-paste Grok Imagine template
└── 04_Notes.txt                # Generation log, decisions, drift fixes
```

---

## Workflow

1. **Fill in `01_Style_Guide.md`** — name, palette, proportions, signature features, style rules, expression list. 30–60 min.
2. **Generate multi-view references** using `03_Prompt_Templates/Master_Prompt.txt`. Save outputs into `02_Reference_Images/`. 2–4 hrs.
3. **Log every generation** in `04_Notes.txt` (seed, prompt tweaks, drift fixes).
4. **Quality gate** — open all images side-by-side; confirm same character from every angle. Regenerate any that drift.
5. **Generate expression / pose variations** (optional but recommended for lip-sync + animation). 1–2 hrs.
6. **Lock and back up** — check the "LOCKED" box in `01_Style_Guide.md`, mirror the folder to cloud + local backup.

---

## Quality Gate Checklist

- [ ] Ears, eyes, snout/mouth, tail, and clothing are identical across every view
- [ ] Colors match exactly (verified against hex codes in the style guide)
- [ ] A stranger would recognize this as the same character from any angle
- [ ] No realistic textures or unwanted detail crept in
- [ ] Side and back views are clean enough for 3D tools (Meshy / Tripo / Hunyuan)

Once every box is checked, this folder is the canonical reference for Phases 1–5.

---

## Common Mistakes to Avoid

- Generating views one by one **without** copying the exact prompt → drift.
- Using different lighting or background → breaks 3D generation tools.
- Skipping the side / back views → 3D model will have bad proportions.
- Not doing a side-by-side quality check before moving on to Phase 1.
