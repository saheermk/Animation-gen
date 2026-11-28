---
name: zero-asset-3d-cartoon-studio
description: Build single-file, mobile-first 3D CGI cartoon video studios using Three.js r170 with sculpted procedural 3D humans, zero external .glb/texture files, interactive waveform beat-sync, and 60fps audio-locked MP4/WebM export.
---

# Zero-Asset 3D Cartoon Studio (`zero-asset-3d-cartoon-studio`)

Use this skill whenever the user asks to create an animated 3D cartoon video, story reel, Shorts/YouTube explainer, or 3D studio web app in the **zero-asset 3D cartoon style**.

---

## 1. Core Technical Mandates

- **Single-File Architecture:** Deliver everything (HTML, CSS, and ES Module JavaScript) inside a single self-contained `.html` file.
- **Zero External 3D/Media Assets:** Never rely on external `.glb`, `.gltf`, `.fbx`, `.png`, or `.mp3` URLs. All 3D characters, props, environments, and signs must be generated procedurally in Three.js and offscreen 2D `<canvas>` textures.
- **Libraries:** Load **Three.js r170** via `<script type="importmap">` from `https://cdn.jsdelivr.net/npm/three@0.170.0/build/three.module.js` and **Tailwind CSS** via `https://cdn.tailwindcss.com`.
- **Instant Frame 1 Boot:** Render Beat 1 immediately on page load with a live `"3D Humans Ready"` status badge—no loading screens or required file uploads.
- **Mobile-First Performance:** Cap `renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 1.5))`, keep shadow maps at `1024x1024`, and avoid heavy post-processing so playback and 60fps video export run smoothly on phones.

---

## 2. Visual Style & Procedural 3D Character System

### Sculpted 3D Humans (`createSculptedHuman`)
- **No Blocky Cubes:** Build characters using smooth spheres, capsules, cylinders, and tori with `MeshStandardMaterial` PBR shading.
- **Articulated Rig Hierarchy:** Every character must expose an animatable hierarchy:
  - `root` → `hips` → `torso` → `neck` → `head`
  - `leftArm` / `rightArm` (`shoulder`, `elbow`, `hand`)
  - `leftLeg` / `rightLeg` (`hip`, `knee`)
- **Expressive Sculpted Faces:**
  - 3D ears and rounded nose.
  - Multi-part 3D eyes (white sclera, dark iris/pupil, and tiny white specular glint).
  - Positioned eyebrows and mouths supporting expressions: `'happy'`, `'angry'`, `'shocked'`, `'worried'`, and `'laugh'`.
- **Hair & Wardrobe Variety:**
  - Support distinct 3D hairstyles (`'quiff'`, `'sidepart'`, `'bun'`, `'messy'`, etc.) and outfits (collars, belts with buckles, ties, cultural attire such as draped sarees/shawls, sunglasses, and round gold glasses).

### Canvas-Generated 3D Signs & Props (`makeSignTexture`)
- Generate crisp 2D graphics (signs, badges, calendars, exam papers, notice boards, leave letters) on an offscreen `<canvas>` using `ctx.roundRect()` and bold `Inter` typography, converted via `new THREE.CanvasTexture(canvas)` with `THREE.SRGBColorSpace`.

### Stylized Three-Point Lighting
- `HemisphereLight` for soft sky/ground fill.
- Warm `DirectionalLight` key light with `PCFSoftShadowMap` shadows.
- Cool rim `DirectionalLight` from behind to separate characters from the dark studio background.
- `ACESFilmicToneMapping` (`toneMappingExposure ≈ 1.15`).

---

## 3. Story Structure & Beat Animation Engine

- Break every story into **6–8 distinct narrative beats**, each stored in its own `THREE.Group` so only the active beat is visible at a given timestamp.
- Each beat must include:
  - Dedicated 3D environment props and set dressing on a grounded circular stage floor.
  - Custom camera choreography (smooth push-ins, orbits, or multi-zone pans driven by normalized beat progress `0..1`).
  - Lively procedural animations (arm waves, prop swings, head nods, shivering, floating particles/confetti/sparkles, rising gauges).

---

## 4. Liquid Glass Studio UI & Workflow

Design the interface with a dark radial background and frosted glass cards (`backdrop-filter: blur(20px)`, subtle `1px` white borders, rounded-3xl containers):

1. **Header:** Studio title, green pulsing `"3D Humans Ready"` badge, and **9:16 Shorts / 16:9 YouTube** aspect-ratio switcher (dynamically resizing the renderer between `720×1280` and `1280×720`).
2. **3D Viewport:** Top HUD pills (`Beat X / N` and `00.0s / 36.0s`) and a bottom live caption overlay synced to the timeline.
3. **Playback Controls:** Play/Pause, Reset, Web Speech API `TTS: On/Off` preview toggle, timeline scrubber, and horizontally scrollable beat-jump chips.
4. **Voiceover & Beat Sync Panel:**
   - Custom audio uploader (`MP3`/`WAV`/`M4A`) decoded via Web Audio API.
   - Interactive `<canvas>` waveform visualizer with **draggable `B2`–`Bn` boundary markers** and live playhead.
   - **Gap-Ranked Auto-Sync:** Scans RMS energy across the audio buffer to automatically place beat markers on the quietest speech pauses.
   - **Tap-to-Sync:** Stamps the next beat marker at the current playhead time.
   - **Subtitle Offset Slider:** Shifts caption timing (`-1.5s` to `+1.5s`) independently of scene cuts.
5. **60fps Audio-Locked Video Export:**
   - Captures `glCanvas.captureStream(60)` combined with Web Audio `MediaStreamDestination` audio tracks into `MediaRecorder` (auto-selecting MP4 or WebM).
   - Drives frame stepping from the audio clock (`AudioContext.currentTime`) for zero drift.
   - Includes a throttled progress bar, working **Cancel** button, and non-blocking inline toast notifications (never use `alert()`).