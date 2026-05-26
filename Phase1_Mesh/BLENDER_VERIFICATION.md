# Phase 1 Verification — Loading the Meshy FBX in Blender (Linux, 4.2 LTS)

## Context

You just got a Meshy mesh that looks solid in their viewer. Before committing to Phase 2 (retopology + painterly shading — the hard, expensive phase), you need to verify the mesh actually holds up under Blender scrutiny. Meshy's marketing renderer flatters everything; wireframe + flat shading does not. This guide walks through installing Blender 4.2 LTS on Linux from scratch, loading the FBX, and running the four Phase 1 verification checks from your plan: wireframe/topology inspection, hand mittening check, symmetry drift, and silhouette test against `10_Front_FullBody.png`.

Outcome at the end: a saved `Phase1_Mesh/winner/liora_verify.blend` file, a rendered `silhouette_front.png` for comparison against slot 10, and a clear pass/fail decision on whether to subscribe to Meshy Pro and proceed to Phase 2 or regenerate.

UI references throughout: **Topbar** = the top menu row across the whole window (File / Edit / Render / Window / Help on the left, workspace tabs in the center). **3D Viewport** = the large central panel. **Outliner** = top-right panel (scene tree). **Properties editor** = bottom-right panel; its tabs are a vertical icon strip on its LEFT edge.

## 0. Install Blender 4.2 LTS on Linux

Three install paths in order of preference:

**A. Official tarball (recommended — works on any distro, no sandbox permission issues)**

```bash
mkdir -p ~/blender && cd ~/blender
wget https://download.blender.org/release/Blender4.2/blender-4.2.3-linux-x64.tar.xz
tar -xf blender-4.2.3-linux-x64.tar.xz
cd blender-4.2.3-linux-x64
./blender
```

To launch from anywhere afterward:

```bash
echo 'alias blender="$HOME/blender/blender-4.2.3-linux-x64/blender"' >> ~/.bashrc
source ~/.bashrc
```

**B. Flatpak** (sandboxed; ~/Downloads access works fine):

```bash
flatpak install flathub org.blender.Blender
flatpak run org.blender.Blender
```

**C. Distro package** — only if you know your distro ships 4.2+. Ubuntu/Debian apt repos typically lag 1–2 minor versions; not recommended.

Verify: launch Blender. Splash screen lower-right should show "4.2.x LTS". If you see anything older, you grabbed the wrong tarball — go back to step A with the correct URL.

## 1. First launch — workspace setup

On the splash screen:

1. Click **"New File" > "General"** to dismiss the splash.
2. Default scene loads: a cube, a camera (small triangular pyramid), and a light (small dot with rays).
3. Across the top of the window you have menus (File / Edit / Render / Window / Help) and workspace tabs (Layout / Modeling / Sculpting / UV Editing / Texture Paint / Shading / Animation / Rendering / Compositing / Geometry Nodes / Scripting). **Stay in the Layout tab for this entire guide.**

### Save preferences once (so you don't redo this every session)

**Edit > Preferences** opens a window:

- **Input** tab (left sidebar): turn ON "Emulate Numpad" ONLY if your keyboard has no numeric keypad. With Emulate Numpad on, the top-row 1/3/7 keys become viewport view shortcuts; with it off, only the dedicated numpad keys work.
- Bottom-left hamburger button > **"Save Preferences"**.
- Close the Preferences window.

### Clear the default scene

Hover cursor over the 3D Viewport (this matters — Blender's hotkeys are context-sensitive to where the mouse is).

- Press `A` to select all (the cube, camera, and light all highlight orange).
- Press `X` > confirm "Delete".

You now have an empty scene with just the world floor grid.

## 2. Import the Meshy FBX

1. Topbar: **File > Import > FBX (.fbx)** — opens the file dialog.
2. Navigate to wherever you saved Meshy's export. If you downloaded it via browser, it's likely at `~/Downloads/liora_base.fbx`. Move it to `Phase1_Mesh/winner/liora_base.fbx` first if you haven't; clean structure matters for Phase 2 onward.
3. On the **RIGHT side of the file dialog** is the FBX import options panel (collapsible sections):
   - **Include** section:
     - Custom Normals: **ON** (preserves Meshy's smoothing — critical for stylized characters)
     - Subdivision Data: OFF
     - Custom Properties: OFF
   - **Transform** section:
     - Scale: leave at **1.00**; we'll fix post-import if the mesh comes in wrong-sized
     - Apply Scalings: **FBX All** (default)
     - Forward: **-Y Forward** (default)
     - Up: **Z Up** (default)
     - Manual Orientation: OFF
   - **Armatures** section: defaults are fine; Meshy meshes at this stage usually have no rig.
4. Click the **"Import FBX"** button (bottom-right of the dialog).

### Common immediate problems after import

| Symptom | Fix |
|---|---|
| Empty viewport, nothing visible | Hover viewport, press `Home` (frames all objects). |
| Tiny mesh near origin | Select mesh, `S`, type `100`, `Enter`. Then `Ctrl+A` > **Scale** to bake. |
| Gigantic mesh dwarfing the grid | Select mesh, `S`, type `0.01`, `Enter`. Then `Ctrl+A` > **Scale** to bake. |
| Mesh lying on its back | Select, `R`, `X`, `90`, `Enter`. Then `Ctrl+A` > **Rotation** to bake. |
| Multiple objects appear in Outliner (head, hair, clothes separate) | Normal for Meshy. Click the topmost collection in the Outliner, then `A` over viewport to select the whole group. |

After import settles: hover viewport, press `Home`, then `Numpad 1` for orthographic front view. You should see Liora facing the camera in A or T pose, roughly human-scale (~1.8m, two-thirds the grid spacing).

**Save now**: `Ctrl+S` > save as `Phase1_Mesh/winner/liora_verify.blend`. Re-save (`Ctrl+S` alone) every few steps below.

## 3. Verification Check #1 — Wireframe + flat shading inspection

Goal: spot floating geometry, n-gons, internal mesh chaos, abnormal tri counts.

### Configure the viewport

1. **Shading mode**: top-right corner of the 3D viewport, find four small spheres in a row. Left-to-right they are: Wireframe / Solid / Material Preview / Rendered. Click the **Solid** (second) sphere.
2. **Flatten the lighting**: directly to the right of those four spheres is a small downward-arrow disclosure button. Click it. A dropdown panel appears. Under "Lighting", switch from "Studio" to **"Flat"**. Close the dropdown by clicking elsewhere. The mesh now reads as a flat-tone shape — pure geometry, no lighting cheats.
3. **Show statistics**: still in the top-right of the viewport, find the **Viewport Overlays** button (icon: two overlapping circles, just left of the shading spheres). Click it > a dropdown opens > top-right corner of that dropdown has a "Statistics" checkbox > turn it **ON**. A text block appears in the viewport corner showing Verts / Edges / Faces / Tris / Objects.
4. **Show wireframe on solid**: in the same Viewport Overlays dropdown, scroll to the "Geometry" section > drag the **"Wireframe"** slider to **1.0**. All polygon edges now overlay the solid surface.

### What to inspect

Orbit the mesh:
- Middle-mouse-drag = rotate
- Scroll wheel = zoom
- `Shift+middle-mouse-drag` = pan
- `Numpad 5` = toggle perspective/orthographic
- `Numpad 1/3/7` = front/right/top orthographic views

**Pass criteria:**
- **Tris in viewport overlay**: between 30,000 and 80,000.
- Wireframe density is roughly even — mostly quads, no extreme concentrations of micro-faces.
- One continuous shape — no chunks floating in space disconnected from the body.
- Orbiting to see inside (peek under chin, into mouth) reveals no large internal volumes hiding inside the body.

**Fail signs (log each in `Phase1_Mesh/notes/generation_notes.md`):**
- Under 20k tris → undersampled, will lack detail at close camera
- Over 150k tris → overdense, will fight retopology
- Disconnected micro-polys floating off the silhouette
- A second hidden body-shaped mesh inside the main body
- Long needle-like triangles radiating from a single vertex (zero-area faces — they'll explode in Phase 3 animation)

### Connected-components test (catches floating chunks)

1. Press `Tab` → enter Edit Mode. Vertices appear as black dots.
2. Press `Alt+A` → deselect all.
3. Hover any vertex on the main body, press `L` → selects all geometry connected to that vertex via shared edges.
4. **Pass**: selection covers the entire mesh.
5. **Fail-ish**: only part is selected. Press `Ctrl+I` to invert — the now-selected verts are floating orphans. Inspect: eyeballs and teeth being separate objects is fine and expected; everything else is garbage to log.
6. `Tab` → back to Object Mode.

## 4. Verification Check #2 — Hand mittening inspection

Mittened (webbed) fingers are Meshy's #1 silent failure mode. Marketing renderers hide it; close inspection reveals it.

1. `Numpad 1` for front view.
2. Click the body mesh in viewport to select it.
3. Scroll-wheel zoom aggressively into one hand until it fills roughly half the viewport. If you lose the hand off-screen, `Numpad .` (period) reframes to selection.
4. Press `Tab` to enter Edit Mode. Vertices and edges now visible.
5. Orbit around the hand: palm side, back side, between-finger gaps.

**Pass criteria:**
- Each finger has its own cylindrical loop of edges around it.
- Visible edge gaps between adjacent fingers (you can see the topology that defines the finger-as-distinct-shape).
- The thumb has its own clearly separated geometry from the index finger; visible cleft between them.

**Fail criteria:**
- Four fingers share continuous geometry like a mitten or a glove.
- Fingers exist visually but no edge loops separate them (Phase 3 rigging will be unable to bend them individually).
- Thumb fused to palm with no separating edge.

Repeat for the other hand. `Tab` back to Object Mode.

If both hands fail: this is a hard Phase 1 fail. Decide between regenerating in Meshy with a hand-focused prompt vs. accepting it and budgeting 4–8 hours in Phase 2 to manually sculpt fingers.

## 5. Verification Check #3 — Symmetry drift

Image-to-3D solvers commonly drift 2–5° asymmetric, mostly on the head (one eye higher than the other, ears at different Y).

Non-destructive approach (doesn't modify the mesh):

1. Object Mode. Click the main body mesh in the Outliner to select it.
2. `Shift+D` to duplicate. **Immediately** right-click to drop the duplicate exactly on top of the original (right-click cancels the move-after-duplicate, leaving the copy at the original position).
3. The duplicate is now selected (in the Outliner it's named e.g. `liora_base.001`).
4. In the Properties editor (bottom-right panel), find the vertical icon strip on its left edge. Click the **Object Properties** tab (orange square icon, around the 4th from the top of the strip — hover any icon to see its name as a tooltip).
5. The Object Properties panel opens. Scroll to the **"Transform"** section near the top. Find "Scale" with three values (X, Y, Z). Change **Scale X** from `1.000` to **`-1.000`**. The duplicate is now mirrored across the YZ plane.
6. Still in Object Properties, scroll down to the **"Viewport Display"** section. Find "Display As" dropdown — change it from "Textured" to **"Wire"**. The duplicate now renders as a wireframe overlay only.

The mirrored wireframe sits on top of the original solid. Orbit around. Anywhere the wireframe noticeably diverges from the solid surface is asymmetry.

**Pass:** drift under ~2% on all axes. Eyes at the same height, ears aligned, shoulders level, hips level.

**Fail:** visible drift — one eye clearly higher, one shoulder forward, ears at noticeably different Y positions.

When done with this check, delete the duplicate so it doesn't pollute the rest of the workflow: click it in the Outliner > `X` in viewport > confirm.

## 6. Verification Check #4 — Silhouette test against slot 10

Final and most important check: does this still LOOK like Liora?

### Render the silhouette

1. `Numpad 1` → front orthographic view.
2. `Numpad 5` → confirm orthographic projection (essential — kills perspective distortion so the comparison against the flat reference image is honest). If the top-right viewport status reads "User Perspective" instead of "Front Orthographic", press `Numpad 1` again.
3. With the body mesh selected, `Numpad .` (period) to frame selection so the model fills the viewport.
4. Confirm viewport shading is Solid + Flat lighting (same setup from Check #1).

### Set render resolution to 1024×1024

1. Properties editor right panel > click the **Output Properties** tab (icon: a printer/printout, about 3rd from top of the icon strip).
2. In the **"Format"** section at the top:
   - Resolution X: **1024**
   - Resolution Y: **1024**
   - "%" stays at **100**

### Capture the silhouette

In the 3D Viewport's top menu bar (NOT the Topbar — the menu bar at the top of the viewport panel itself, with "View / Select / Add / Object" etc.):

- **View > Viewport Render Image**

A new image-viewer window opens with the captured frame. (This is faster than F12 because it uses the viewport state directly, with no full render pass.)

In that render window:

- Topbar > **Image > Save As**
- Navigate to `Phase1_Mesh/notes/` and save as `silhouette_front.png`.

### Compare to slot 10

Open `Phase0_Character_Consistency/02_Reference_Images/10_Front_FullBody.png` and your fresh `silhouette_front.png` side by side. Any image viewer works; GIMP, Krita, or even the system file manager preview.

**The blur test (the actual signal):**

In GIMP or Krita, open both images. Apply **Filters > Blur > Gaussian Blur** with radius `20px` to each. View the two blurred blobs side by side.

- **Pass**: the blob shapes match — same proportions, head-to-shoulder ratio, hip width, leg length. → It's Liora.
- **Fail**: the blobs are clearly different shapes (head too small, waist too high, legs disproportioned). → Meshy gave you its default mannequin in Liora's clothing. Regenerate with stronger weight on slot 10.

## 7. Save your work

`Ctrl+S` to save the working file. It should already be saved as `Phase1_Mesh/winner/liora_verify.blend` from step 2. This preserves viewport state, the imported FBX, the resolution settings — a useful starting point for Phase 2.

## 8. Decision tree

| Result | Action |
|---|---|
| All four checks pass | Proceed to Phase 2 (Quad Remesher + projection bake). Subscribe to Meshy Pro. |
| 1 check fails | Re-generate in Meshy with a prompt addressing that specific failure ("perfectly symmetric face," "five distinct fingers per hand"). Compare new vs old candidate; keep the better one. |
| 2+ checks fail | The base mesh isn't strong enough. Try Tripo free tier as fallback **before** subscribing to Meshy Pro. |
| Mesh feels generic — "not Liora" | Reference images may have been under-weighted. In Meshy, retry with slot 10 set as the dominant view (heavy weight) and 11/12/13 as supporting (lighter weight). |
| Tri count fails but other checks pass | Phase 2 retopology will fix this; not a Phase 1 blocker. |

## Critical files and paths

- **Input**: `Phase1_Mesh/winner/liora_base.fbx` (your Meshy export)
- **Reference for silhouette test**: `Phase0_Character_Consistency/02_Reference_Images/10_Front_FullBody.png`
- **Output**: `Phase1_Mesh/winner/liora_verify.blend` (saved Blender session)
- **Output**: `Phase1_Mesh/notes/silhouette_front.png` (rendered front view)
- **Output**: `Phase1_Mesh/notes/generation_notes.md` (free-form notes: tri count, observed defects, decision rationale)

## Verification of the verification

If you finish and aren't sure whether the mesh passed, you should have these four specific data points to point at:

1. **Tri count** (from viewport overlay): a number between 30,000 and 80,000.
2. **Symmetry drift** (from wireframe overlay): visually under ~2%.
3. **Silhouette blob match** (from blurred comparison): the two blurs read as the same shape.
4. **Hands** (from Edit Mode close-up): five geometrically distinct fingers per hand, with edge gaps between them.

If you can answer "yes" to all four with concrete observations rather than vibes, the mesh passes and Meshy Pro is worth the $20/mo. If even one is "I'm not sure," log it in `generation_notes.md` and decide whether to regenerate before committing budget to Phase 2.
