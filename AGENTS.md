# PROJECT: VOID-KEEPER - Agent Context

## Overview
**VOID-KEEPER** is an atmospheric, top-down survival incremental game.
**Current Version:** v1.6 (Definitive Edition)
**Platform:** Web (HTML5/JavaScript)
**File Structure:** Single HTML file containing CSS, JS, and 3 Canvas layers.

**Theme:** Sci-Fi Horror / Survival.
**Tone:** Oppressive, lonely, mechanical, high-contrast.

---

## The Rendering Engine (CRITICAL)
The game uses a **3-Layer Canvas System** to achieve volumetric lighting and fog effects. **DO NOT SIMPLIFY THIS.**

1.  **Layer 0 (`#world-canvas`):**
    * **Z-Index:** 0
    * **Content:** The Starfield, Floating Debris (Carbon/Scrap), and Particle Explosions.
    * **Logic:** Always rendered. Debris only draws its "Body" if `revealed == true`. Otherwise, it only draws a faint 2px glint.

2.  **Layer 1 (`#fog-canvas`):**
    * **Z-Index:** 1
    * **Content:** The Darkness (The Vantablack).
    * **Logic:**
        * Fills screen with semi-opaque black (`rgba(15, 20, 25, 0.95)`).
        * Uses `globalCompositeOperation = 'destination-out'` to "cut" the hole for the Light Beam.
        * Uses Radial Gradients for soft edges on the cut.

3.  **Layer 2 (`#light-canvas`):**
    * **Z-Index:** 2
    * **Pointer Events:** `none` (Clicks pass through to World).
    * **Blend Mode:** `mix-blend-mode: screen` (CSS) or `globalCompositeOperation = 'lighter'` (JS).
    * **Content:** The visible Volumetric Light Beam (Gradient Cone) and the Lighthouse Tower icon.
    * **Purpose:** This layer makes the light look "physical" and glowing on top of the fog.

---

## The Core Loop

1.  **Survival (Fuel):**
    * **Resource:** **Carbon**.
    * **Decay:** Constant decay per frame.
    * **Fail State:** If Carbon <= 0, the light extinguishes, and the "SIGNAL LOST" Death Screen appears.

2.  **The Light (Defense):**
    * **Passive:** The beam rotates automatically (`rpm`), pushing back the fog.
    * **Active (Focus Mode):** Holding **Left Click** stops rotation. The beam narrows (width decreases), changes color to **Cyan**, and burn power/radius increases.

3.  **Scavenging (Income):**
    * **Glints:** Debris hidden in fog renders as 1px white flickers.
    * **Reveal:** The Light Beam must touch debris to reveal it.
    * **Harvest:** The player must **CLICK** revealed debris to collect Carbon (Fuel) or Scrap (Money).

4.  **Progression:**
    * Spend **Scrap** to upgrade:
        * **Lens:** Beam Radius.
        * **Gear:** Rotation Speed.
        * **Focus:** Burn Intensity (Fog clear speed).

---

## Technical Details

| Component | Implementation |
| :--- | :--- |
| **Audio** | Web Audio API. Oscillators (Sawtooth/Sine) + BiquadFilterNodes (Lowpass) for atmospheric drone. |
| **Input** | Hybrid Mouse/Touch. `mousedown` triggers Focus Mode. `mousemove` updates Reticle. |
| **Storage** | `localStorage`. Includes a "Factory Reset" hard wipe function. |
| **Loop** | `requestAnimationFrame` with a Delta Time calculation. |
| **Responsiveness** | `user-scalable=no` meta tag to prevent mobile zoom. Resolution scaling settings in-game. |

---

## UI & Branding

* **Colors:**
    * **Terminal:** Amber (`#ffaa00`)
    * **Fuel:** Red (`#ff3333`)
    * **Scrap:** Blue (`#00aaff`)
    * **Focus:** Cyan (`#00ffff`)
    * **Background:** Pitch Black (`#000000`)
* **Fonts:** `Orbitron` (Headers), `Share Tech Mono` (Data/Logs).
* **Style:** Retro-futuristic terminal. Vignette overlays. Scanlines.

---

## Current Features (v1.6)

* ✅ **Volumetric Fog:** Dynamic clearing and creeping darkness.
* ✅ **Manual Database:** In-game guide with "Directives", "Catalogue" (Lore), and "Entities".
* ✅ **Lore System:** Log entries unlock based on survival time (`aliveTime`).
* ✅ **Settings Menu:** Master/SFX Volume, Graphics Quality (High/Low Fog), Resolution Scale.
* ✅ **Feedback:** Particle explosions on collect. Reticle targeting on hold. Screen shake (implied via glitch effects).