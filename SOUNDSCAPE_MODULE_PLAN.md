# JORD Soundscape — standalone module plan

Planning doc only. Nothing here is built yet. The ambient soundscape stays
inline in `index.html` for now — this just captures the shape of a future
extraction so whoever picks it up doesn't start cold.

## Why

The generative ambient engine (drone voices, plucked notes, percussion, and
whatever chimes/gongs/swell get added next) is a self-contained enough idea —
"a small reactive Web Audio ambience generator" — that it could be useful
outside JORD. Splitting it into its own file/project would let it be
versioned, tested, and reused independently, without dragging the rest of the
app along.

## Current shape (as of rev L)

All inline in `index.html`'s single big IIFE, no separation from the rest of
the app's state:

- **Persisted controls** (each a plain var backed by `store.get`/`store.set`):
  `ambientOn`, `ambVolume`, `ambWarmth`, `ambDensity`, `ambVariety`,
  `ambTimbre`, `ambPercOn`, `ambPercType`, `ambPercGroove`, `ambPercVolume`.
- **Web Audio graph state**: `actx` (the `AudioContext`), `ambVoices` (4
  continuous drone oscillators → `ambFilter` → `ambMaster` → destination),
  `ambNoiseBuf` (one shared noise buffer reused per percussion hit).
- **Lifecycle**: `startAmbient()` / `stopAmbient()` build and tear down the
  graph.
- **Three parallel generative systems**, same shape each: a `scheduleX()` that
  re-arms itself via `setTimeout` with some randomized wait, and a `playX()`
  that does one-shot synthesis for that event.
  - `ambEvolve()` — not one-shot, a periodic (`setInterval`, 4s) reactive
    parameter updater for the drone (filter cutoff, voice gains, detune).
  - `scheduleAmbNote()` / `playAmbNote()` — generative plucked notes on a
    pentatonic scale.
  - `schedulePercHit()` / `playPercHit()` — generative percussion (added
    rev K, widened rev L).
- **Reactivity inputs** — the engine reaches directly into JORD-specific
  globals: `tierOf(calm)`, `isDarkHours()`, `+vibeslider.value`,
  `objectPresent`. This is the main thing coupling it to the rest of the app.
- **UI**: the `#sound` panel, one `.tr-card` per control, wired via direct
  `$("snd-...")` DOM lookups and `store.set` calls in each listener.

## Proposed module boundary

A single factory (name TBD, e.g. `createSoundscape(opts)`), exposing:

- `init()` / `start()` / `stop()` — lifecycle (roughly today's
  `startAmbient`/`stopAmbient`).
- `setParam(name, value)` or individual setters (`setVolume`, `setWarmth`,
  `setPercType`, ...) — today's per-slider inline listeners, generalized.
- A small set of **input hooks** instead of reaching into globals directly:
  `setMoodTier(n)`, `setDarkMode(bool)`, `setVibe(v)`, `setVisitorPresent(bool)`
  (or a single `setContext({tier, dark, vibe, visitorPresent})`). The host app
  calls these when its own state changes; the module never imports JORD
  concepts by name.
- An injected **persistence adapter** — `{get(key, default), set(key, value)}`
  — passed in at construction, so the module can use JORD's `store` or its
  own `localStorage` keys when standalone, without assuming a global `store`
  exists.
- An injected **AudioContext factory** (or just `new (window.AudioContext ||
  window.webkitAudioContext)()` internally, same as today) — fine to keep
  inline unless a host ever needs to share one context across modules.

## Distribution / file shape

- One new file, e.g. `soundscape.js` — still hand-authored vanilla JS, no
  framework, matching this project's "no dependencies, no build step"
  philosophy.
- Two ways it could relate back to `index.html`, not mutually exclusive:
  1. `<script src="soundscape.js">` — clean, but breaks the current
     single-file deploy story.
  2. Keep authoring it as a separate file for editing/diffing sanity, but
     paste its contents inline into `index.html` at "release" time (manual,
     or a tiny future script if that stops being tolerable). This preserves
     "single file in production" while giving the module real file boundaries
     during development.
- If it's ever going to be reused by something that isn't JORD, it eventually
  wants its own repo. Not urgent — a subfolder here is a fine intermediate
  step.

## Open questions for whoever picks this up

- Does "single file" stay a hard rule forever, or is a tiny build step
  (inlining `soundscape.js` into `index.html` before commit) acceptable once
  there's enough here to justify it?
- Should the module stay JORD-flavored (keeps "mood tier"/"vibe" as concepts)
  or go fully generic (a general reactive-ambience engine, with JORD's
  mood/vibe mapping as a thin adapter layer built on top)? Generic is more
  reusable; JORD-flavored is less abstraction to maintain for a project that
  may never actually reuse it elsewhere.
- Worth writing a couple of manual test pages (raw HTML + the module, no
  JORD) once it's extracted, since this whole area is otherwise unverifiable
  by anything except human ears.

## Do not do yet

No extraction, no new files beyond this plan, no changes to how `index.html`
loads. This is here so the *next* extraction attempt (whenever that is) has a
running start, not a blank page.
