# JORD — Handover Doc

Last updated: 2026-07-10, after adding occasional garden visitors (butterfly/
ladybug/spider drifting through the `#garden` panel — see the newest changelog
entry). Prior same day: a "slow, fady, floaty ghost" motion pass and a
keepsake-PDF mini-garden strip. Prior:
2026-07-09 (night), after several more rounds on top of the redesign
below: cheek blush, fading vibe-word tint, a slimmer/more-blended thought bubble,
a one-page infographic-style PDF with a spiky→smooth wave, header/viewBox layout
changes, a new Garden page, and a service-worker caching fix. This doc exists so a
fresh Claude Code session (after a context clear) can pick up exactly where things
left off without re-deriving everything.

**Cloud routine confirmed unreliable a second time:** the one-time
`trig_01Sj3XQ6zPdgMKrJTRc4AS2s` routine (thought bubble width, loading flourish,
Jord bounce, moon phase/drift, water states) fired on schedule
(`last_fired_at: 2026-07-09T23:00:16Z`, `ended_reason: run_once_fired`) but
produced zero commits — same silent-failure shape as the first attempt noted
below, now confirmed twice. `persist_session:false` on the trigger means there's
no transcript to inspect after the fact either. **Do not rely on this path for
unattended work again** — if the user wants something done at a specific future
clock time with nobody present to babysit it, say so plainly and treat the local
`Agent` path as the only trustworthy option, even though it can't itself wait for
a clock time (it has to be kicked off when someone's around to start it). The
five items above still need doing as of this writing.

## What this is

JORD is a single-file, single-page PWA — a whimsical "grounding companion." A
hand-drawn-style SVG creature (Jord) reacts to your mood, offers short grounding
exercises, tracks a self-reported "vibe" over time, and generally tries to be a
calm, funny, slightly weird presence rather than a clinical wellness app.

- Repo: https://github.com/technicallytechnicaldesign/JORD (branch `main`)
- Local working copy: `C:\WINDOWS\system32\jord`
- Live file: `index.html` — currently ~141KB, single file, no build step, no
  dependencies. Everything (HTML, inline `<style>`, two inline `<script>` blocks)
  lives in this one file on purpose. Keep it that way unless the user explicitly
  says otherwise.
- Other files: `manifest.json`, `sw.js` (service worker), `icon-*.png` /
  `apple-touch-icon.png`. These rarely need touching.

## Architecture notes (read before editing)

- **Two inline `<script>` blocks.** The large one is a single IIFE holding all
  app logic. The small one just registers the service worker. When checking
  syntax, both need `node --check`.
- **`store`** — a tiny `get`/`set` helper wrapping `localStorage` with an
  in-memory fallback if storage isn't available. Nearly all persisted state
  goes through it.
- **Jord's body** is the SVG group `#orbbody` (arms, main circle, `#surface`
  texture lines, `#backface`, `#faceflip`/`#face` with many `<g id="e-NAME">` /
  `<path id="m-NAME">` eye/mouth variants toggled via `display:none`, registered
  in the `EYES`/`MOUTHS` arrays, set via `setEyes()`/`setMouth()`). Sky/background
  elements (`#night`, `#clouds`, `#t-water`, `#t-zen`, `#meteor`, `#visitor` for
  the rare UFO, `#weather` for the ambient rain/sun/overcast layer) are siblings
  of `#orbbody`, not descendants — this separation
  matters: reactive animations (shake/deflate/blush/breathing) should target
  `#orbbody` specifically so they don't drag the whole sky along, which was a
  real bug fixed earlier in this project.
- **Full-screen panel pattern** — `#trends`, `#shelf`, `#sound` all follow the
  same convention: `position:fixed; inset:0`, toggled via an `.open` class, own
  close button. Mirror this for any new full-screen page.
- **Vibe slider** (`#vibeslider`) drives `reactToVibePreview(v)`, which computes
  mood bands and drives hot/cold tinting on the slider track and two small cheek
  ellipses (`#orbblush`, NOT a whole-body wash) on Jord. The vibe *word*
  (`#vibeword`) separately tints on genuine slider interaction via `tintVibeWord()`
  and fades back to its neutral color a couple seconds after you let go
  (`vibeWordFadeTimer`) — this is deliberately decoupled from the slider's own
  persistent tint.
- **Garden** (`#garden` panel, `❀` header icon) — the anti-shelf: one flower
  plants per *activity* (`plantGarden(type)`, called from `logMission(type)`,
  not from vibe-logging — that was a real bug once, see caveats), and the whole
  garden clears out fresh at 5am (`gardenDayKey()` shifts the clock back 5h
  before taking a calendar day, so anything before 5am still counts as
  yesterday's growth — checked both on planting and on opening the panel via
  `resetGardenIfNewDay()`). Flowers scatter/overlap chaotically (deterministic
  per-entry hash via `gseed()`, not a tidy grid like the shelf), each paired
  with a small symbol from `MISSION_ICON` for whichever activity triggered it.
- **Garden visitors** (`#gd-visitors`, a sibling of `#gd-field`, NOT inside it)
  — occasional ambient creatures (`spawnGardenVisitor()`, `GV_KINDS` weighted
  toward butterflies, plus ladybug/spider; SVGs `BUTTERFLY_SVG`/`LADYBUG_SVG`/
  `SPIDER_SVG`) that drift through the meadow while the panel's open. They live
  in their own overlay layer precisely because `renderGarden()` rewrites
  `#gd-field.innerHTML` wholesale — putting creatures there would leak/wipe
  them. Each runs a finite CSS animation (`gvFlit`+`gvFade` for butterfly/
  ladybug, `gvDangle`+`gvSway` for the spider) and self-removes on the first
  `animationend` (the flutter/sway loops never fire one). `startGardenVisitors()`
  (called from the `b-garden` open handler) clears the layer, rolls ~38% for an
  arrival shortly after open, then runs a 26s interval that self-terminates once
  `#garden` is no longer `.open` (so none of the three close paths need to touch
  it). Clickable for a deadpan `VISITOR_LINES` one-liner via the reused `.gd-word`
  bubble. Under `reduceMotion` they don't spawn at all (pure ornament); the
  `.gv`/`.gv-flap`/`.gv-swing` classes are also in the reduce-motion disable list.
- **Weather** (`#weather`, sibling of `#orbbody` like the other sky layers) —
  purely decorative, no geolocation/network: `WEATHER_STATES` (clear/sun/rain/
  overcast, weighted toward clear) picked via `weatherRoll(bucket)` where
  `bucket = Math.floor(Date.now()/(3*3600*1000))`, hashed through `gseed()` so
  the pick is stable within a 3h window and re-derives identically on reload
  with nothing persisted. `render()` re-checks the bucket periodically. `sun`
  is suppressed at night (falls back to `clear`) so the moon/stars own the
  dark; rain/overcast show day or night.
- **`think()`/`say()`** are called from dozens of places throughout the file.
  They've been unified to render through one bubble (`#thought`, now centered
  above Jord's head, not the old top-left corner placement) with a random idle
  waiting-phrase pool (`WAITING`) instead of a fixed "…". Don't break existing
  call sites when touching this again.
- **Objects/visitors system** (`spawnObject()`, `OBJECTS` array, `#objects`
  SVG layer) — temporary visiting items with a deliberate nested group structure
  (`drop` outer / `scaleWrap` middle / `lifeWrap` inner) so CSS animations never
  clobber the base scale. Objects are clickable (spin/launch/bob), get subtle
  per-instance `hue-rotate` color variation, and every spawn is passively logged
  to the **keepsake shelf** (`shelf` array, `#shelf` panel, deduped by type with
  a `×N` badge).
- **Ambient soundscape** — opt-in, Web Audio only (no audio files), reactive to
  live app state (vibe band, calm tier, day/night, whether an object is
  visiting). Settings live on their own `#sound` page (reachable via a header
  icon, not buried in Settings). Includes a "variety" control that scales how
  much the generative engine wanders.
- **PDF export** — Trends → "Save as PDF" uses the browser's native
  `window.print()`, and is now THREE printed pages, each populated at print
  time via a single chain off `populateKeepsake()` (→ `populateWave()` →
  `populateDataPage()` → `populateGardenPage()`), so the `tr-print` click
  handler still only calls `populateKeepsake()`. All the print containers are
  siblings inside `#trends`'s `.tr-inner`, `display:none` on screen and shown
  only inside the `@media print` block.
  - **Page 1** — `#tr-keepsake` + `#tr-wave` share ONE sheet (no
    `break-before` between them): the whimsical keepsake (header, face, sweet
    stat, quote, capped 12-bloom mini garden strip, footer) then the wave coda.
    The wave line uses `smoothPathRamp(pts, sAt)`: the last 24h stays genuinely
    jagged (low `sAt`), older-than-24h overshoots past normal Catmull-Rom
    smoothing (`sAt` up to ~1.6) for an exaggeratedly silky look, so "spiky day
    → smooth week" reads as one continuous but clearly-contrasted line.
  - **Page 2 "JORD · the data"** (`#tr-data`, `break-before:page`) — the SAME
    two charts as the on-screen Trends view, titled "Today · by hour" and
    "Last 14 days · daily average". `populateDataPage()` reuses
    `chartToday()`/`chartHistory()` verbatim (they already return complete
    self-contained `<svg>` strings off current `vibes`/`missions`).
  - **Page 3 "JORD · the garden, right now"** (`#tr-gardenpage`,
    `break-before:page`) — a full static scatter of the WHOLE live `garden`
    (not the capped page-1 strip), rebuilt with renderGarden()'s exact `gseed`
    formula so it's recognisably the same meadow, plus a fixed handful of
    static visitors (`populateGardenPage()` hard-codes 4 — two butterflies, a
    ladybug, a dangling spider — at deterministic left/top % so reprints match,
    reusing `BUTTERFLY_SVG`/`LADYBUG_SVG`/`SPIDER_SVG`). Field is
    `.gp-field` (position:relative, fixed 420px height, max-width 460px) so the
    absolute % positions land sensibly on paper. Empty garden shows a light
    in-voice line AND still keeps the visitors for company. Nothing animates —
    it's paper. NOT print-preview-verified (no print access in this env) —
    worth a real 3-page print/pagination check.
- **Dark mode** — auto 5pm–5am, with a manual override (auto/always-on/always-off)
  in Settings.
- **`prefers-reduced-motion`** — there's a `reduceMotion` const and a
  `@media (prefers-reduced-motion:reduce)` block that lists every looping
  animation to disable. Any new looping animation needs to be added there.
- **Nap state** (`napping` flag, alongside `busy`/`sitting`) — after a long
  untouched idle stretch (`NAP_IDLE_MS`, currently 7 min) a coarse 45s checker
  *sometimes* (`NAP_CHANCE` 0.4, rolled each tick — organic, not a rigid timer)
  nods Jord off: eyes close, mouth flat, and `#jordbob` swaps `.bob`→`.napping`
  so he droops into a settled sag (a one-shot CSS `transition`, NOT a loop; the
  droop transform is applied inline by JS after freezing his live float position
  so the swap eases down instead of snapping). A `zzz` from the `SNORES` pool
  loops in the shared `#thought` bubble via `think()`. All other idle systems
  (`applyMood`, arm-wave/blink intervals, `scheduleThoughts`, `turnAround`,
  `goIdle`) are gated on `!napping` so nothing fights it visually. Waking is
  instant: a single delegated `document` `pointerdown` listener (plus
  `vibeslider` `input`) stamps `lastInteraction` and, if napping, calls
  `wakeNap()` — this runs on `pointerdown`, *before* the corresponding `click`,
  so the pat/tap handlers see `napping` already false and behave normally. Under
  reduce-motion only the droop *motion* is stripped (`#jordbob.napping{transition:none}`
  → instant sag); the eyes/zzz/wake-on-touch state still happens, since it's
  meaningful feedback, not ornament. Never put a competing animation/inline
  transform on `#orbbody` for this — `#jordbob` is the safe layer, same as the bob.

## Feature changelog (chronological, oldest first)

Early session (layout + core interaction rework):
- Compact one-page layout; settings folded into a gear-icon menu
- Exercise buttons (Breathe/Ground/Accept/Enjoy) became in-place "modes" living
  inside Jord's own stage card instead of a full-screen popup — Jord stays
  visible at all times
- Fixed a real bug: missing trailing commas in string arrays were silently
  killing the entire inline script
- Vibe slider redesigned around a wide "sweet spot" (middle, not the top) with
  matching highlighted band on the trend charts; slider reactions (shaky/still/
  warm) tied to Jord's body specifically, made transient (clear after ~2s) and
  "floaty" rather than jarring
- Dark mode (auto + manual override), mission tracker with chart callouts,
  drifting clouds/extra stars, monochrome chart icons matching the action
  button glyphs

Object/visitor expansion:
- ~30 total visiting objects (line-art, varied size incl. occasional
  "comedically large"), floaty/shimmer ambient life, occasional small "mini-say"
  text/emoji, a very rare UFO fly-by easter egg, click interactions (spin/
  launch/bob), per-instance color variation

Latest big round:
- Long-press-to-breathe on Jord (hold gesture, body swells/shrinks, eyes close,
  murmurs a randomized "ahhhhhhhh"/"oooooooooh"/etc., doesn't conflict with the
  normal tap-to-pat gesture)
- Passive keepsake shelf (auto-logs every object seen, its own page, dedup +
  seen-count badge, click for a little flourish/reflection)
- Generative ambient soundscape with its own settings page and a variety
  control
- Whimsical, private (not social) two-page PDF export via print
- Hot/cold tinting on the slider + a subtle blush on Jord at the extremes
- Unified speech/thought bubble, centered above Jord, varied idle phrases

Latest rounds (same day, later):
- Cheek-only blush (was tinting Jord's whole body), vibe-word tint-then-fade
- Slimmer, more translucent/blended thought bubble (color-mix + backdrop-filter)
- One-page infographic PDF with the spiky-day/smooth-week wave blend
- Rev tag moved from the `<h1>` into the footer; viewBox reworked so there's
  real sky above Jord and he sits low, tight against a cropped water tier
  (`viewBox="0 -40 280 304"`, `#t-water` raised to `cy=248`)
- Garden page added, then its trigger corrected from vibe-logging to actually
  doing an activity (`logMission()`)
- `sw.js` fixed from stale-while-revalidate to network-first for `index.html`,
  since that staleness was making shipped/working features look broken
- Added a `controllerchange` listener that reloads once when a new SW takes
  over, so an already-open tab actually adopts fresh code instead of sitting
  a version behind until manually refreshed
- Idle bounce floor raised (-5px → -9px, ceiling -12px → -22px — the first
  pass read as too subtle) and rescoped to a new `#jordbob` wrapper around
  just `#orbbody`, since it used to animate the whole outer `<svg>` and drag
  the sky/moon/water/stars along with Jord
- Garden reworked again: one bloom per *activity* (not per day), whole thing
  resets at 5am (`gardenDayKey()`) — see architecture notes above
- Visiting objects (`spawnObject()`) were spawning at y=264-272 while the
  frame's bottom edge is y=264 (a leftover from the viewBox crop — the water
  tier moved but objects weren't adjusted to match) and getting clipped;
  shifted to y=240-248, the same 24-unit offset the water moved
- Accept/Enjoy line pools doubled (10 → 20 each)
- Ambient weather layer added (clear/sun/rain/overcast, random per 3h window)
  — see architecture notes above
- Nap state: after ~7 min untouched Jord sometimes (40% per 45s check) falls
  asleep — eyes close, droops into a sag on `#jordbob`, soft `zzz` in the
  bubble; wakes instantly on any interaction (delegated pointerdown listener).
  Reduce-motion keeps the state, drops only the droop motion — see arch notes
- "Floaty ghost" motion pass: idle bob tempo slowed hard (was 4.5-7s typical /
  32% chance 9-16s; now 7-13s typical / 25% chance 16-24s), given a floatier
  `cubic-bezier(.37,0,.63,1)` ease and even 25/50/75 keyframe spacing; tilt/sway
  softened (.6-1.6→.4-1.1deg, 3-9→2-6px) so it drifts not swings. A synced
  `jordFade` opacity pulse (1→.9 at the top of the lift, same `--bob-dur`) on
  `#jordbob` gives the ethereal fade — never on `#orbbody`. Buzzy made rare: the
  vibe-slider shaky trigger floor went 84→94 (supernova only) behind a 35% roll
  made once on band-entry (tracked via `vibeWasInBuzz`/`buzzArmed`, so no
  mid-drag flicker). The `applyMood` head-tilt wiggle dropped 30%→12% and slowed
  .9s→1.7s at gentler ±1.3deg. The fady/soften changes are covered by the
  existing `.bob`/`.wiggling` reduced-motion entries (no new entry needed).
- Keepsake PDF mini-garden: `populateKeepsake()` now appends a compact,
  single-row strip of TODAY's live `garden` (capped at 12 blooms) above the
  footer, reusing `FLORA`/`GOLDEN_FLOWER`/`gseed()`/`objHue()` at a small fixed
  height; empty garden shows a light in-voice one-liner. A little `ks-notes`/
  `ks-foot` spacing was trimmed to protect the one-page keepsake+wave budget
  (not print-preview-verified — worth a real print check).

- Garden visitors: occasional line-art creatures (butterfly weighted highest,
  the odd ladybug or dangling spider) drift through the `#garden` panel while
  it's open. Live in a new `#gd-visitors` overlay sibling of `#gd-field` (kept
  out of the flowers' rewritten innerHTML on purpose), each a finite CSS
  animation that self-removes on animationend. ~38% arrival shortly after open +
  a 30% roll every 26s on a self-terminating interval; clickable for a deadpan
  line. No spawns under reduce-motion — see architecture notes above.
- Thought bubble anchor switched from a fixed `top:9px` to `top:28%` (percentage
  of `.stage`, tracking the SVG's own proportions) — several rounds of moving
  Jord's head lower in the frame had left a fixed-px bubble drifting further
  from him each time. All its transition timings slowed drastically per
  request: entrance .5s→1.6s, the natural drift-away exit .85s→3s (bounce
  entrance .6s→1.3s) — `TB_LEAVE_MS` updated to match the 3s exit, same class
  of handoff bug fixed for the nap earlier (don't let JS swap content before
  a slowed CSS transition has actually finished).
- Keepsake shelf: added a "today" section (5am reset, same boundary as the
  garden) above the existing all-time listing — `shelfTiles()` factors out
  the shared group-by-kind-with-badge rendering so both sections reuse it
  rather than duplicating the logic.
- PDF export grown from one page to three (see the PDF architecture note
  above for the full breakdown): page 1 unchanged (keepsake + wave), new
  page 2 "JORD · the data" reprints the two live tracker charts via
  `populateDataPage()`, new page 3 "JORD · the garden, right now" statically
  renders the whole live garden (renderGarden's exact `gseed` scatter) with
  4 fixed-position static visitors via `populateGardenPage()`. Each new page
  gets its own sheet via `break-before:page`; both chain off
  `populateKeepsake()`. Not print-preview-verified.

Run `git log --oneline` for the exact commit-by-commit list — commit messages
are descriptive and were kept small/independent deliberately.

## The working pattern that's proven reliable

For anything beyond a small tweak, the effective loop has been:

1. **Ground yourself in the actual current code first** — grep/read the
   relevant functions/CSS before writing a brief. This file has been rewritten
   enough times that assumptions from memory are often stale.
2. **Delegate to a background `Agent` call** (`subagent_type: general-purpose`,
   `model: opus`) with a long, specific, code-grounded prompt: exact function
   names, exact current behavior, exact task list, and explicit process
   requirements (see below). Run it in the background — no need to block on it.
3. **Process requirements to always include in the brief:**
   - Standing permission to commit + push directly to `origin/main`, no PR, no
     asking — already established, git identity already configured locally.
   - Verify JS syntax after every edit to the big `<script>` block, before
     every commit: extract `<script>([\s\S]*?)<\/script>` blocks via a small
     node script, write each to a temp file (scratch dir, not the repo), run
     `node --check` on it.
   - **Work in small, independently-committable chunks and push after each
     one** — this needed to be repeated explicitly more than once; left
     unstated, agents tend to batch everything into one big commit at the end.
   - Respect `prefers-reduced-motion`, extend the existing CSS block.
   - Match the existing deadpan-whimsical voice — skim `THOUGHTS`/`LINES`/
     `OBJECTS[].react`/`VIBE_NOTES`/`SHELF_MEMORY`/`WAITING` for tone.
   - Single self-contained file, no new dependencies, no build step.
   - Never leave a broken/non-parsing script committed.
   - **Bump the footer's `<span class="rev">rev X</span>` (search for it) to
     the next letter whenever a round ships a substantial change** — the
     user asked for this as a standing convention (currently at `rev F`).
     Use judgment on "substantial" — a single small copy/number tweak
     doesn't need it, a real feature or a multi-part round does. Don't bump
     it more than once per round even if the round has several commits.
   - End with a clear summary: what shipped, commit hashes, anything skipped
     and why.
4. **When the agent finishes, verify locally before reporting to the user:**
   `git pull`, re-run the `node --check` extraction/verification yourself,
   `Grep` for the new identifiers to confirm they're actually wired (not just
   present), open `index.html` in a browser for a look. Agents in this
   environment have no browser access, so anything visual/audio is "reasoned,
   not pixel-verified" on their end — say so plainly when reporting back, and
   name the specific things worth the user's own eyes/ears.
5. A cloud-scheduled routine (`RemoteTrigger`/`/schedule` skill) was tried once
   for a one-time delayed run and silently produced nothing (fired, marked
   complete, zero commits, no visible error via the API). The local background
   `Agent` approach has been reliable every time it's been used instead — prefer
   it unless there's a specific reason to need the cloud path again. It was
   tried a second time on 2026-07-09 specifically because the user was going
   to sleep and asked for work to happen at a specific future clock time
   (something a local `Agent` call can't do — it runs to completion now, it
   doesn't sleep-then-run). **Confirmed failed again** — it fired on schedule
   but landed zero commits, with no session transcript retained to diagnose
   why. Two-for-two failures now. Treat this path as effectively non-functional
   for this project until something changes; the honest answer for "do X while
   I'm asleep" is that it currently can't be done unattended — say so instead
   of quietly re-trying the same mechanism.

## Known caveats

- **`sw.js` caches `index.html` network-first now** (as of the "Fix garden
  appearing broken" commit) — it used to be stale-while-revalidate with a
  never-bumped `CACHE_NAME`, so a user could sit a full round of changes behind
  for an entire session and a brand-new feature would look completely broken.
  Static assets (icons/manifest) are still cache-first for offline support. If
  something *shipped and verified in this repo* still looks broken to the
  user, a stale PWA install is worth ruling out before assuming the code is
  wrong — ask them to hard-refresh / reopen the PWA.
- No browser automation is available in this environment — all verification of
  animation feel, audio character, print/PDF layout, and gesture timing is
  static/reasoned. Always flag this and suggest specific things for the user
  to actually try.
- `.claude/settings.local.json` (gitignored) accumulates an approved-command
  allowlist locally — this is separate from and unaffected by conversation
  context clears.

## Backlog — discussed, deliberately not built yet

From an earlier brainstorming pass, still open for a future planning
conversation:

- **Real local weather integration** (rain streaks, sun glow) via geolocation +
  a weather API — breaks the "single file, no external dependencies" rule and
  needs a permission-prompt design decision. Needs the user's buy-in first.
  Note: a **self-contained, decorative** version of this is now built (the
  ambient `#weather` layer — see below). Only the *real-location* variant
  (geolocation + external API) remains deferred; the whimsical randomized one is
  shipped.
- **Seasonal / date-aware object & flora sets** (snowflakes weighted in winter,
  a birthday-specific object, etc.) — needs input on where "the date that
  matters" comes from and how much date-personalization is wanted.

Everything else from that original brainstorm (keepsake shelf, ambient
soundscape, whimsical PDF, long-press breathing) has since been built.

## Permissions / continuity notes

- Standing permission to commit and push directly to `origin/main` in this
  repo, without asking, is already established (confirmed explicitly by the
  user) and also recorded in Claude's persistent memory (not just this repo).
  This survives a context clear.
- Local git identity in this repo: `technicallytechnicaldesign` /
  `technicallytechnicaldesign@users.noreply.github.com`.
