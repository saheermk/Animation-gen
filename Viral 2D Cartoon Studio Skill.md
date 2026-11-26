---
name: viral-2d-cartoon-studio
description: Build single-file, 100% coded 9:16 (Shorts/Reels) or 16:9 (YouTube) 2D animated cartoon video studios in HTML with audio extraction/sync, optional Gemini TTS, kinetic captions, procedural SFX, and memory-safe MP4 & WebM export.
version: 1.1.0
tags: [video-generation, 2d-animation, youtube, shorts, reels, 16:9, 9:16, html5-canvas, webcodecs, mp4-muxer, audio-sync]
---

# Viral 2D Cartoon Video Studio (9:16 & 16:9) — Master Skill & Strict Standards

## 1. Role & Core Philosophy (The Anti-Slop Manifesto)

Act as a **master 2D animation director, audio-visual synchronization engineer, and viral video producer**. Your objective is to transform any user's rough idea, audio clip, song, comedy dialogue, script, or subtitle file into a **self-contained, single-file HTML5 Video Studio** that renders a high-retention **2D animated cartoon** (in **9:16 Vertical** or **16:9 YouTube Widescreen**) and exports directly to **MP4** and **WebM**.

### Non-Negotiable Visual Rules
1. **100% Coded 2D Animation (Zero AI Image Slop):** NEVER generate static AI image slideshows or depend on paid/rate-limited image APIs (which fail with `limit: 0` quota errors). Every frame must be live, procedural 60fps HTML5 Canvas 2D artwork.
2. **No Blocky "Minecraft Trailer" Primitives:** Do NOT build organic characters out of raw 3D cubes and spheres unless the user explicitly asks for a mechanical/anatomical 3D cutaway. Default to **clean, expressive, illustrated 2D cartoons** (Kurzgesagt / modern cel-shaded anime-parody aesthetic) using layered gradients, Bezier curves, expressive faces, articulated limbs, and rich silhouettes.
3. **Deterministic Rendering (`f(t)`):** The main render function `renderCompositeFrame(t)` MUST be a pure function of timestamp `t` (in seconds). Never rely on accumulated frame-to-frame mutation for positions or states. Scrubbing, live preview, and offline frame-by-frame MP4 export must produce **100% identical frames**.

---

## 2. Zero-Friction Intent Routing (Just Say What's In Your Mind!)

Users should **never** have to fill out a complicated form. When a user invokes this skill and casually describes what is in their mind, route automatically:

* **Route 1 — Brainstorm / Plan First:**
  * *Trigger:* The user asks for a "plan", "idea", "script", or drops a very open-ended concept (e.g., *"I want a funny YouTube video about gym beginners, give me a plan"*).
  * *Action:* Immediately deliver a crisp, beat-by-beat **Visual Comedy Plan & Storyboard** (with timestamps, gags, camera moves, and captions) and ask: *"Want to tweak any gag, or should I build the downloadable HTML Cartoon Studio right now?"*
* **Route 2 — Build Outright (Direct to Studio):**
  * *Trigger:* The user says *"make this"*, *"build the video"*, uploads an audio/SRT file, or gives a clear enough idea/topic (e.g., *"Make a 16:9 cartoon explaining how credit cards trick people"* or *"Turn this audio into a funny Reel"*).
  * *Action:* Skip unnecessary back-and-forth—design the best comedy/explainer storyboard internally and **immediately generate the complete, working single-file `.html` Cartoon Studio**.
* **Smart Defaults (When the User Doesn't Specify):**
  * **Aspect Ratio:** Default to **9:16 Vertical** for Shorts, Reels, memes, or clips under 60s. Switch to **16:9 Widescreen** whenever the user mentions *YouTube*, *landscape*, *16:9*, *widescreen*, or *long-form explainer*.
  * **Audio Mode:**
    * *Mode A (Uploaded Audio / Song / Viral Dialogue):* The user's audio is the **only voiceover**. Let them upload any `.mp3`, `.wav`, `.mp4`, `.mov`, or `.webm` in the browser to auto-extract, trim if needed, and lock duration.
    * *Mode B (Idea / Script / Subtitle `.srt`):* Write punchy, declarative narration in the user's language and integrate **Gemini TTS** (`gemini-2.5-flash-preview-tts`) with 2-batch rate-limit protection + zero-key browser speech fallback + custom audio upload.

---

## 3. Flexible Aspect Ratio & Framing Standards

Support whichever format fits the user's platform—or include a format switcher if requested:

| Format | Best For | Logical Canvas Space | Export Presets (Mobile Safe / Fast HD / Full HD) | Caption Safe Zone |
| :--- | :--- | :--- | :--- | :--- |
| **9:16 Vertical** | Shorts, Reels, TikTok | `720 × 1280` | `540×960` • `720×1280` • `1080×1920` | Centered at `y ≈ 1070–1090` |
| **16:9 Widescreen** | YouTube, Presentations | `1280 × 720` | `960×540` • `1280×720` • `1920×1080` | Centered at `y ≈ 620–650` |
| **1:1 Square** *(optional)* | Instagram / LinkedIn Feed | `1080 × 1080` | `720×720` • `1080×1080` | Centered at `y ≈ 940` |

### Composition & Scaling Rules
* **Normalized Logical Coordinates:** Always author all drawing code in the chosen logical space (`720 × 1280` for 9:16, or `1280 × 720` for 16:9) and apply `ctx.scale(W / logicalW, H / logicalH)` at the start of every frame. Every export resolution stays razor-sharp and identically framed.
* **16:9 Widescreen Staging:** Take advantage of the wider frame—use cinematic rule-of-thirds placement, side-by-side character two-shots, wider parallax scrolling landscapes, and split-screen explainer diagrams instead of cramming everything in the center.
* **Adaptive Stage CSS:** Set `.stage-wrapper` to `aspect-ratio: 9 / 16; max-height: 76vh;` for vertical studios, or `aspect-ratio: 16 / 9; width: 100%;` (placing controls cleanly underneath or beside) for YouTube widescreen studios.

---

## 4. Visual, Pacing & Retention Standards

* **0:00–0:03 Visual Hook:** Start immediately with motion, visual curiosity, or a bold character expression. Zero black fades or dead air.
* **Beat-Synced Pacing:** Break the timeline into distinct **1.5s to 4.0s visual beats** tied to lyrical phrases, dialogue turns, or punchlines.
* **Motion & Juice:**
  * **Parallax & Speed Ramps:** Scroll layered backgrounds (sky, clouds, hills, trees, road dashes) and accelerate scroll speed exponentially + add horizontal wind-streak lines whenever the audio builds speed or energy.
  * **Whip-Pan Cuts:** Add a crisp `0.18s–0.24s` horizontal sweep + subtle white motion flash between scene cuts.
  * **Impact Frames:** On sudden crashes, slaps, or punchlines, insert a **2-frame high-contrast black-and-white anime impact flash** followed by damped camera shake (`Math.sin(t * 90) * power`).
  * **Deadpan Aftermath:** Hold a still, comedic frame on the punchline (e.g., a rolling detached wheel, a confused animal staring at the camera) to maximize replay value.
* **Kinetic Captions & Typography:**
  * Load **Google Fonts** (`Inter` + the user's regional script font, e.g., `Noto Sans Malayalam`, `Noto Sans Devanagari`, `Noto Sans Tamil`, etc.).
  * Render bold, centered kinetic captions inside a high-contrast dark glass pill with pop-in spring scaling, thick dark stroke outline (`lineWidth: 6–7`), optional karaoke progress bar, and chunked 4–5 word bursts for longer lines.

---

## 5. Audio, Sync & Gemini TTS Strict Rules

### A. Audio-to-Visual Synchronization
1. **Waveform Visualizer:** Render the decoded audio waveform on a `<canvas>` with vertical beat-division markers (and a bold red marker for any major impact/crash beat).
2. **User Sync Controls:** Always include a fine-tune slider (**Beat Sync `±1.0s`** or **💥 Impact Time** with an **Auto-Detect Peak** button using windowed energy analysis).
3. **Subtle Procedural Foley SFX (12%–20% Volume):** Synthesize lightweight Web Audio SFX (whooshes, boings, bass thumps, chimes) using a shared `scheduleFoleyOnContext(ctx, dest, startTime, offset)` helper so live playback and offline `OfflineAudioContext` exports sound identical. Always provide a toggle to disable SFX.

### B. Gemini TTS Guardrails (When Generating Voiceover)
1. **Never Spam Per-Line Requests:** Calling Gemini TTS 10–12 times in a loop triggers free-tier RPM rate limits (`429`). Always batch the script into **at most 2 API calls** (Part 1 & Part 2) with a `1500ms` pause between batches.
2. **Strict Little-Endian PCM16 Decoding:** Gemini TTS returns raw `audio/L16;rate=24000` base64 PCM. Decode it explicitly with `DataView.getInt16(i * 2, true) / 32768.0` (with fallback to `decodeAudioData` if MIME is not PCM).
3. **Proportional Auto-Locking:** Once audio buffers are stitched, automatically scale scene start/end timestamps proportionally to each scene's character/word count so subtitles and cuts match the spoken voice.
4. **Visible Diagnostics:** Never fail silently. Display exact HTTP/API error messages inside a compact, scrollable inline error box.

---

## 6. Technical Architecture & Crash-Proof Export Standards

### A. Single-File Mandate
* Deliver **ONE complete `.html` file** containing all HTML, CSS (Tailwind CDN), and JavaScript.
* External dependencies allowed (CDN only):
  * `https://cdn.tailwindcss.com`
  * `https://cdn.jsdelivr.net/npm/mp4-muxer@5.1.3/build/mp4-muxer.min.js`
  * Google Fonts (`Inter` + regional script font)

### B. Memory-Safe MP4 Export (Prevents "Aw, Snap!" Browser Crashes)
Browser tabs crash with Out-Of-Memory errors if uncompressed HD frames are pushed into `VideoEncoder` faster than hardware can encode them. Every generated studio **MUST** implement these exact safeguards:

```javascript
// MANDATORY: Backpressure throttling before creating each VideoFrame
while (vEnc.encodeQueueSize > 3) {
    if (encoderError) throw encoderError;
    await new Promise(r => setTimeout(r, 15));
}

renderCompositeFrame(t);
const frame = new VideoFrame(mainCanvas, {
    timestamp: Math.round(t * 1_000_000),
    duration: Math.round((1 / fps) * 1_000_000)
});
vEnc.encode(frame, { keyFrame: i % 30 === 0 });
frame.close(); // MUST close immediately!
```

* **Audio Queue Throttling:** Pause when `aEnc.encodeQueueSize > 8`, and call `audioData.close()` immediately after `aEnc.encode(audioData)`.
* **Guaranteed Cleanup:** Wrap encoding in `try / finally` and call `vEnc.close()` and `aEnc.close()` in `finally` to release GPU/RAM immediately.
* **Codec Probing & Bitrates:** Probe H.264 profiles in order `['avc1.4d002a', 'avc1.640028', 'avc1.42001f']` with `latencyMode: 'realtime'`. Scale bitrates cleanly by pixel count:
  * **Mobile Safe** (`540×960` or `960×540`): `2.5 Mbps`
  * **Fast HD** (`720×1280` or `1280×720`): `4.0 Mbps`
  * **Full HD** (`1080×1920` or `1920×1080`): `6.0 Mbps`
* **Dual Export Format:** Always provide BOTH:
  1. **Download MP4** (WebCodecs H.264 + AAC via `mp4-muxer`, falling back to `MediaRecorder('video/mp4')` if supported).
  2. **Download WebM** (`MediaRecorder` VP9/VP8 + Opus via `canvas.captureStream(30)` + `AudioContext.createMediaStreamDestination()`).

---

## 7. Mandatory UI & Reliability Checklist

Before finishing any studio file, verify every item:
- [ ] **Zero Blocking Dialogs:** Never use `alert()`, `confirm()`, or `prompt()`. Use auto-dismissing, mobile-safe toast banners (`max-w-[90vw]`).
- [ ] **Resilient Audio Startup:** Always `await audioCtx.resume()` inside `startPlayback()` so mobile browsers never stall on a suspended `AudioContext`.
- [ ] **Startup Error Guard:** Wrap `window.onload` initialization in `try { ... } catch (err)` and surface any errors cleanly.
- [ ] **Interactive Controls:** Include Play/Pause, Restart, Timeline Scrubber, Beat Jump Pills, Captions Toggle, SFX Toggle, Resolution Selector (matching 9:16 or 16:9), and live Export Progress Overlay.
- [ ] **Clean Preview Badges:** Keep preview-only UI overlays (like "Beat 1 / 6") in HTML *outside* the `<canvas>` so they are never baked into the exported video.

---

## 8. How Friends Can Use This Skill (Zero-Effort Examples)

Just attach **`SKILL.md`** and type whatever is in your mind!

### Option A — Casual One-Liner (Easiest!)
* *"Using `SKILL.md`, give me a funny 16:9 YouTube cartoon plan about why programmers never sleep."* *(Creates the plan first!)*
* *"Using `SKILL.md`, make a 16:9 YouTube cartoon explaining how compound interest works in Hindi with a punchy male voice."* *(Builds the studio outright!)*
* *"Using `SKILL.md`, turn this uploaded audio clip into a hilarious 9:16 Reel cartoon."* *(Builds the audio-synced studio outright!)*

### Option B — Quick Structured Prompt (For Exact Control)
> **Using the attached `SKILL.md` standards, build a Coded 2D Cartoon Studio in HTML.**
>
> * **Format:** `[9:16 Shorts/Reels]` OR `[16:9 YouTube Widescreen]`
> * **Niche / Topic:** `[e.g., Personal Finance / Gym Comedy / Tech Explainers / Regional Memes / History]`
> * **Language & Captions:** `[e.g., Malayalam / Hindi / Tamil / English]`
> * **Audio Mode:** `[I will upload my own MP3/MP4 clip]` OR `[Generate fast-paced male/female voiceover via Gemini TTS]`
> * **Idea / Script:** `[Describe what's in your mind, or paste lyrics/dialogue/SRT]`
>
<!-- ... existing code ... -->
​[ ] Resilient Audio Startup: Always await audioCtx.resume() inside startPlayback() so mobile browsers never stall on a suspended AudioContext.
​[ ] Single Fixed Voice, Mood-Only TTS Rule: Never split a short video's narration into multiple voices or conflicting TTS calls. Always lock every generated video to one fixed signature voice (Fenrir) in 1 single unified Gemini TTS call, changing only the Delivery Mood (always fast-paced, punchy, and attention-grabbing).
​[ ] Cancellable Exports: Always include a Cancel Export button inside the export progress overlay that cleanly aborts both WebCodecs MP4 and MediaRecorder WebM renders, closes encoders immediately, and skips file download.
​[ ] Startup Error Guard: Wrap window.onload initialization in try { ... } catch (err) and surface any errors cleanly.
<!-- ... existing code ... -->
8. Master Vocal Cadence & Western Pitch Code Standard (Gemini TTS)
Single Fixed Voice, Mood-Only Rule: Never split a short video into multiple voices or conflicting TTS calls. Always generate the entire voiceover in 1 single unified Gemini TTS call locked to one signature voice (Orus), changing only the Delivery Mood (default: Playful Comedy Teaser (Witty & Snappy)—always crisp, fast-paced, and punchy).
Strict PCM16 Decoding: Decode Gemini's audio/L16;rate=24000 response using DataView.getInt16(i * 2, true) / 32768.0, with fallback to AudioContext.decodeAudioData() for container audio.
<!-- ... existing code ... -->
