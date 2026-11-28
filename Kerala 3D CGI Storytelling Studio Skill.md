---
name: kerala-3d-cgi-studio
description: Build single-file, mobile-first 3D CGI storytelling web studios using Three.js, Wolf3D rigged GLB avatars from GitHub, Kerala/Malayalam styling, Gemini Orus TTS with gap-ranked waveform sync, and audio-locked MP4/WebM export—using the stacked Apple Liquid Glass card UI.
---

# Kerala 3D CGI Storytelling Studio — Workflow Skill

Use this skill whenever building or updating an interactive 3D cartoon/CGI storytelling web app (9:16 Shorts & 16:9 YouTube) in a **single self-contained `index.html` file**.

---

## 1. Preferred UI Architecture (Stacked Apple Liquid Glass Cards)

**IMPORTANT:** Do **not** hide controls behind a cramped 3-tab dock. Always use the **previous stacked, scrollable Apple Liquid Glass card layout** so playback, voiceover, waveform, model swapping, and export remain immediately visible and accessible:

1. **Topbar (Sticky Glass Pill):** App title/logo on the left, live rig status pills (`♂ Rigged` / `♀ Rigged`) on the right.
2. **Composite Stage Viewport:** Centered `9:16` (mobile Shorts) or `16:9` (YouTube) `<canvas id="mainCanvas">` with a floating frosted-glass beat badge in the top-left corner and burned-in Apple Glass captions drawn directly onto the composite canvas.
3. **Card 1 — Playback & Beat Timeline:**
   - Format segmented toggle (`9:16 Shorts` / `16:9 YouTube`), Restart (`↺`), and primary Play/Pause button.
   - Full-width timeline scrubber with `currentTime / Beat X of 8 / totalDuration`.
   - Horizontally scrollable snap-start beat pill buttons.
4. **Card 2 — Voiceover, Foley & Sync:**
   - Gemini API Key input (persisted in `localStorage`), Mood Directive dropdown, and **Generate Orus VO** button.
   - Custom Audio Upload (`.mp3/.wav/.m4a`), Copy Script button, Browser Speech Preview toggle (`ml-IN` / `en-IN`), and Procedural 3D Comedy Foley SFX toggle.
   - **Interactive Waveform Canvas** showing audio peaks, yellow playhead, and draggable `B2`–`B8` beat cut markers.
   - **Tap-to-Sync** button (also wired to `Spacebar`), **Reset Gap Auto-Sync**, and **Subtitle Offset** slider (`-1.5s` to `+1.5s`).
5. **Card 3 — 3D Rig Swapper & Scene Controls:**
   - Repo auto-load status + file upload / drag-and-drop override slots for `male.glb` and `female.glb`.
   - Camera Zoom slider, Lighting Exposure slider, and Burned-In Captions toggle.
6. **Card 4 — Audio-Locked Video Export:**
   - Resolution selector (`720x1280`, `1080x1920`, `1280x720`, `1920x1080`).
   - **Export MP4 (WebCodecs H.264 + AAC)** and **Export WebM (Audio-Locked MediaRecorder)** buttons.
   - Full-screen frosted **Busy/Progress Overlay** with percentage bar and working **Cancel Export** button.

---

## 2. Core Tech Stack (Single-File Mandate)

- **Single `.html` file:** All CSS, HTML, and ES Module JavaScript in one file.
- **Three.js `0.170.0`** via `<script type="importmap">` (`three`, `three/addons/loaders/GLTFLoader.js`, `DRACOLoader.js`, `SkeletonUtils.js`).
- **MP4 Muxing:** `https://cdn.jsdelivr.net/npm/mp4-muxer@5.1.3/build/mp4-muxer.min.js`.
- **Typography:** Load `Noto Sans Malayalam` (`400, 700, 800`) from Google Fonts for Malayalam stories, and await `document.fonts.ready` to redraw 3D signs and captions cleanly.

---

## 3. GitHub Wolf3D Rig Pipeline (`saheermk/Animation-gen`)

Never block Frame 1 waiting for remote 3D models.

1. **Instant Boot:** Render Frame 1 immediately with stylized capsule/sphere placeholders so the stage is never blank.
2. **Auto-Discovery:** Query the GitHub Trees API (`main` then `master`) for `saheermk/Animation-gen` to locate `male.glb` and `female.glb`.
3. **Multi-Source Fallback + 12s Timeout:** For each discovered path, try in order:
   - Same-origin `./path` and `./models/filename.glb` (when served over `http:`/`https:`)
   - `https://cdn.jsdelivr.net/gh/saheermk/Animation-gen@{branch}/{path}`
   - `https://raw.githubusercontent.com/saheermk/Animation-gen/{branch}/{path}`
4. **Hot-Swap Cloning:** Clone loaded rigs via `SkeletonUtils.clone(gltf.scene)`, clone materials per instance, tint `Wolf3D_Outfit_Top`, `Wolf3D_Outfit_Bottom`, and `Wolf3D_Hair`, toggle `Wolf3D_Glasses`, normalize feet to `y = 0` via `THREE.Box3`, and rebuild all beat dioramas seamlessly.

---

## 4. Kerala & Malayalam Character Styling Rules

Transform the 2 base Wolf3D rigs into a full Kerala cast using bone-mounted accessories:

| Role | Base Rig | Scale | Styling & Bone Attachments |
| :--- | :--- | :--- | :--- |
| **Hero (12% Machan)** | `male.glb` | `1.00` | Cyan top, dark pants, glasses hidden |
| **Classmate / Proxy** | `male.glb` | `0.98` | Purple top, slate pants, glasses hidden |
| **Friend (Chunk)** | `male.glb` | `0.97` | Gold/amber top, navy pants, glasses hidden |
| **HOD / Principal** | `male.glb` | `1.04` | White shirt + **Kasavu Mundu** (white wrap + gold kasavu border) attached to `Hips`, glasses visible |
| **Ammachi (Grandma)** | `female.glb` | `0.91` | Silver hair bun + gold hairpin on `Head`, glasses visible, **Kasavu Saree Skirt** (cream cylinder + maroon hem + gold kasavu stripe) on `Hips`, **Cricket Bat** attached to `RightHand` |

**Strict Visual Rules:**
- **NEVER** put a red/maroon torus ring around a character's chest/neck for a shawl—it looks awkward and clips. Use a clean hip-mounted Kasavu saree skirt instead.
- If mixing Mixamo/scaled rigs (`0.01` bone scale), compensate with `bone.getWorldScale()` (`attachInWorldMeters`) before attaching props.

---

## 5. Natural Wolf3D Skeletal Poses & 63 Facial Morphs

Always store each bone's `restQuat` on load, reset to `restQuat` every frame, and apply clean local Euler offsets via quaternion multiplication.

- **Relaxed Arms Default:** Wolf3D rigs load in an A/T-pose. Bring arms down naturally (`LeftArm` rz `~0.92`, `RightArm` rz `~-0.92`) with slight elbow bend (`ForeArm` rx `~0.15`).
- **Action Presets:**
  - `idle`: Gentle `Spine1` breathing + subtle `Head` drift + relaxed arms + `mouthSmile (0.22)`.
  - `talk`: Natural waist-level two-hand gestures + `jawOpen` / `mouthOpen` lip-sync + periodic `eyeBlinkLeft/Right` every `~3.2s`.
  - `wave`: Left arm relaxed, right arm raised high with forearm waving + `mouthSmile (0.78)`.
  - `bat` (Ammachi Cricket): Rotate character `~55°` (`rotation.y = 0.96`) sideways, crouch knees slightly (`UpLeg`/`Leg`), hold both arms forward/across waist, twist `Spine` through the swing synchronized with the flying cricket ball.
  - `scold`: Left arm down, right arm pointing/jabbing forward + `browDownLeft/Right`, `mouthFrownLeft/Right`, `jawOpen`.
  - `panic`: Both hands raised near head, fast subtle torso shake + `browInnerUp`, `eyeWideLeft/Right`, `jawOpen`.
  - `shiver`: Hunched `Spine`, crouched knees, arms hugging chest tightly, high-frequency tremor (`sin(t * 36)`) + `eyeSquintLeft/Right`, `mouthFrownLeft/Right`.
  - `cheer`: Both arms pumping overhead + `mouthSmile (0.95)`.

---

## 6. Full 3D Sets & Mobile-Safe Signs

Every beat must have a complete 3D environment—never leave characters floating in a dark empty void.

- **Per-Beat Atmosphere:** Update `scene.background` and `scene.fog.color` per beat (bright tropical sky for Kerala campus/parambu, warm classroom wall, dark green flu corridor, bright hospital/home split, clean exam hall, warm spotlight principal office).
- **Kerala Environment Props:** Procedural curved **Coconut Palms** (segmented trunk + 7 fronds + coconuts), tiled-roof college facade, cricket pitch & stumps, classroom desks & blackboard, lockers + spiky 3D viruses + animated 104°F thermometer, home/hospital beds + IV stand + glowing red cross, spinning gold trophy, and falling 3D confetti.
- **Crisp Sign Textures (Zero Black Boxes on Mobile WebGL):**
  - Always use **power-of-two** canvas sizes (`512x128` or `512x256`).
  - Always set `tex.colorSpace = THREE.SRGBColorSpace` and `toneMapped: false` on `MeshBasicMaterial`.

---

## 7. Voiceover (`Orus` Gemini TTS), Foley & Sync Engine

1. **Gemini TTS Engine:**
   - Endpoint: `gemini-2.5-flash-preview-tts:generateContent`
   - Locked voice: `Orus` (`prebuiltVoiceConfig: { voiceName: 'Orus' }`)
   - Include 4 selectable Mood Directives (`comedy`, `viral_pitch`, `deadpan`, `hype`) instructing a `~0.4s` breath pause between paragraphs.
   - Use exponential backoff (`fetchWithBackoff`) for `429`/`503` responses.
   - Decode `audio/L16` PCM16 base64 cleanly (with fallback `decodeAudioData` for encoded audio).
2. **Silence Trim & Gap-Ranked Auto-Sync:**
   - Run `trimAudioSilence(ctx, rawBuffer)` to strip leading/trailing dead air.
   - Run `smartGapRankedBeatSplit(buffer)`: scan 20ms energy windows for pauses `>= 0.18s`, and snap beats `B2`–`B8` to the strongest pause near each beat's character-length proportional target.
3. **Manual Fine-Tuning:**
   - Draggable `B2`–`B8` markers directly on `#waveformCanvas` (pointer/touch supported).
   - **Tap-to-Sync** mode (`click` or `Spacebar`) to lock beat cuts live during playback.
   - Live **Subtitle Offset** slider (`-1.5s` to `+1.5s`).
4. **Procedural 3D Comedy Foley SFX:**
   - WebAudio oscillator pops, drops, and victory fanfare scheduled relative to beat timestamps (`scheduleFoleyOnContext`), mixed offline with the voiceover via `OfflineAudioContext` during export.

---

## 8. Composite Rendering & Audio-Locked Export

- **Composite Canvas:** Render Three.js to an offscreen `webglCanvas`, draw it onto `#mainCanvas`, then draw wrapped Apple Glass captions directly on `#mainCanvas` so preview and export match pixel-for-pixel.
- **Primary Export (MP4):**
  - Mix VO + Foley into a stereo `48kHz` buffer via `OfflineAudioContext`.
  - Encode deterministically (`t = i / 30`) using WebCodecs `VideoEncoder` (`avc1.4d002a` / `avc1.640028` / `avc1.42001f`) + `AudioEncoder` (`mp4a.40.2`) muxed with `Mp4Muxer`.
  - Check `exportCancelled` and queue backpressure (`encodeQueueSize`) every frame.
- **Fallback Export (WebM):**
  - Drive `renderCompositeFrame(ctx.currentTime - audioStartCtxTime)` locked to the live `AudioContext` clock while recording `mainCanvas.captureStream(30)` + `MediaStreamAudioDestinationNode`.

---

## 9. Pre-Ship Verification Checklist

- [ ] Single self-contained `index.html` with stacked Apple Liquid Glass cards (no cramped tabs).
- [ ] Frame 1 renders immediately with placeholders; GitHub GLBs hot-swap cleanly within 12s or fall back gracefully.
- [ ] Ammachi holds the bat naturally in a sideways batting stance with a Kasavu saree skirt (no chest torus).
- [ ] All 3D signs use power-of-two `SRGBColorSpace` textures and render crisply on mobile.
- [ ] `Orus` Gemini TTS, custom audio upload, draggable waveform markers, Tap-to-Sync, and Foley SFX all work.
- [ ] Both MP4 (WebCodecs) and WebM (audio-locked) exports include burned-in captions and mixed audio, and can be cancelled cleanly.