# Character Style Guide — Liora Voss

> **This document and the accompanying image set are the single source of truth for the entire project. Every prompt and every 3D generation must match these references exactly.**

---

## Name & Role

- **Name:** Liora Voss
- **Role / Personality:** Confident, playful adventurer with a mischievous half-smile and self-assured gaze.
- **Short Description:** Stylized photoreal-anime hybrid adult character; charismatic explorer archetype.

---

## Color Palette

| Role       | Hex Code | Usage                                                       |
|------------|----------|-------------------------------------------------------------|
| Primary    | #F5D0C5  | Skin — warm peach base                                      |
| Secondary  | #3A2F2F  | Hair — rich dark brown with subtle warm highlights          |
| Eyes       | #C8741F  | Amber-brown iris (locked from reference set, was #7A4B2A)   |
| Accent 1   | #C9A227  | Wardrobe accents — warm gold (waistband trim, jewelry)      |
| Accent 2   | #E8E8E8  | Wardrobe base — white tank, light gray boyshorts            |
| Outline    | #1A1A1A  | Linework and deepest shadows — bold near-black              |
| Highlight  | #FFF2E6  | Skin and hair specular highlights — warm soft white         |

---

## Proportions

- **Head-to-body ratio:** 1 : 6.5 (slightly stylized — between realistic 1:7.5 and anime 1:6)
- **Limb proportions:** Long legs, elongated tapered limbs; hourglass torso silhouette
- **Notable size cues:**
  - Eyes ≈ 1.5× realistic scale (large, expressive)
  - Pronounced hourglass — full bust, defined narrow waist, wide hips
  - Toned musculature (not athletic-bulky)

---

## Signature Features

- [ ] **Messy high bun** with soft bangs and loose face-framing strands
- [ ] **Large amber-brown eyes** (#7A4B2A irises) with long lashes and subtle eyeliner
- [ ] **Beauty mark** directly under the left eye
- [ ] **Full lips** with a natural slight upturned smile
- [ ] **Warm glowing peach skin** with realistic sheen and soft subsurface highlights
- [ ] **Hourglass silhouette** — full bust, slim waist, wide hips, long toned legs

---

## Style Rules

**Locked rendering style:** Painterly photoreal-anime hybrid — visible brush texture, cinematic rim lighting from upper-left, realistic skin gradients with stylized anime facial proportions. Match the rendering of `01_Front_Neutral.jpg`, `02_ThreeQuarter_Left.jpg`, `03_ThreeQuarter_Right.jpg`, and `06_Back.jpg`. Do NOT drift toward full photoreal (no real-skin micro-pores) or flat cel-shaded anime.

- **Outlines:** Subtle painterly edges on hair and silhouette; no hard cartoon outlines on skin.
- **Shading:** Soft cinematic gradients with warm rim light; brush-texture rendering on skin and fabric.
- **Texture rules:** Painterly skin (no realistic pore detail). Hair as defined strand clusters with rim-light highlights. Fabric reads as ribbed cotton — solid weave, not lace or sheer.
- **Tone:** Confident, charismatic, mature adult — not coy, not exaggerated.
- **Things the style must NEVER include:**
  - Hard cartoon outlines on skin
  - Flat cel-shaded skin without gradient falloff
  - Full photoreal rendering (real-skin micro-pores, photo-grain)
  - Plastic / doll-like skin material
  - Sheer or transparent fabric, visible nipples, wet-look material
  - Oversized anime eyes (keep realistic eye-to-face ratio)
  - Childlike or ambiguous-age face — must read as adult woman

---

## Wardrobe (Default Reference Outfit)

- **Opaque white ribbed cotton tank top** — scoop neckline, hem at waist, no transparency
- **Light gray opaque cotton boyshorts** with warm gold (#C9A227) stitched trim at the waistband
- No footwear required for reference views
- Wardrobe is the **reference outfit only** — additional outfits get their own subfolder later

---

## Expression Guide

| Expression  | Description                                       | Reference Image |
|-------------|---------------------------------------------------|-----------------|
| Neutral     | Relaxed, confident half-smile, direct gaze        | `02_Reference_Images/01_Front_Neutral.jpg` |
| Happy       | Bright laugh, teeth visible, eyes crinkled        | `02_Reference_Images/07_Front_Happy.png` |
| Surprised   | Wide eyes, small "o" mouth, raised brows          | `02_Reference_Images/08_Surprised.png` |
| Smirk       | One-sided smirk, raised brow, sideways glance     | `02_Reference_Images/09_Smirk.png` |
| Thinking    | One brow raised, lips slightly pursed             | TODO            |
| Talking     | Mouth mid-word, animated brows                    | TODO            |
| Focused     | Eyes narrowed, lips set, brows lowered            | TODO            |

---

## Reference Image Index

| # | File | Status | Notes |
|---|------|--------|-------|
| 1 | `01_Front_Neutral.jpg` | **LOCKED** | Head-to-mid-torso crop; full-body re-shoot pending |
| 2 | `02_ThreeQuarter_Left.jpg` | **LOCKED** | Hip-up framing, confirms hair silhouette and bust profile |
| 3 | `03_ThreeQuarter_Right.jpg` | **LOCKED** | Hip-up framing, mirrors view 2 |
| 4 | `04_Side_Left.jpg` | **LOCKED** | True 90° profile, head-to-mid-torso crop |
| 5 | `05_Side_Right.png` | **LOCKED** | True right profile, head + collarbone (ChatGPT/DALL-E gen 11423) |
| 6 | `06_Back.jpg` | **LOCKED** | Over-shoulder back view; bun + boyshort waistband visible |
| 7 | `07_Front_Happy.png` | **LOCKED** | Front view, big laugh, teeth visible, eyes crinkled (gen 11428) |
| 8 | `08_Surprised.png` | **LOCKED** | Front view, wide eyes, small "o" mouth, raised brows (gen 11427) |
| 9 | `09_Smirk.png` | **LOCKED** | ¾ right favor, sideways glance, one-sided smirk (gen 11426) |
| 10 | `10_Front_FullBody.png` | **LOCKED** | Full-body head-to-toe, neutral stance; wardrobe + hair color on-model (gen 11429) |
| 11 | `11_FullBody_Side.png` | **LOCKED** | Full-body left-side profile (gen 11430) |
| 12 | `12_FullBody_Back.png` | **LOCKED** | Full-body back view (gen 11431) |

> **Full-body set note (10, 11, 12):** rendered in a slightly cleaner / more photoreal style than the painterly close-ups. Internally consistent with each other — together they form a complete 3-view orthographic turnaround (front / side / back) for 3D mesh reconstruction.

---

## Locked Status

- [x] Style Guide complete
- [x] Rendering style locked (painterly photoreal-anime hybrid)
- [x] Eye color locked (#C8741F)
- [x] Wardrobe locked (opaque ribbed cotton + opaque boyshorts)
- [x] All 12 reference images locked (6 close-up angles + 3 expressions + 3 full-body orthographic views)
- [x] Side-right profile generated
- [x] Core expressions generated (happy, surprised, smirk)
- [ ] Additional expressions (thinking, talking, focused) — optional, generate as needed
- [ ] Side-by-side consistency check passed
- [ ] Folder backed up (cloud + local)
- [ ] **LOCKED — do not change**
