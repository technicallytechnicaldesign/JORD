# JORD — Handover Doc

## Version A.3, where this stands

JORD is **v A.3**, as of 2026-08-15 (A.2 was 2026-08-14, A.1 2026-08-01).

The single-letter revs (G → Z) were the prototype era. The letter ran out at Z
at a genuinely good stopping point: two modes shipped and in daily use by two
people, the documentation caught up with the code, and a real test harness in
place. So the scheme rolled over rather than continuing into AA. **A.1 is the
first version of JORD as a thing that exists, rather than JORD as an
experiment.**

### The scheme, from here

- **A.1 → A.2 → A.3 …** — one bump per shipped round of work, the same cadence
  the letters had. Bump the footer's `<span class="rev">v A.n</span>` **once per
  round**, not once per commit. A single small copy or number tweak doesn't
  need one; a real feature or a multi-part round does.
- **B.1** — reserved for a change to *what JORD is*: another mode alongside
  Jord and Little Jord, a rewrite, or a change to the shape of stored data that
  an existing install would notice. Don't spend it on a big feature — features
  are minors. This has to survive someone's phone having a garden in it.
- The letters stay as history in the changelog below. **v A.1 follows rev Z.**
- Anything that reads the version out of the file should take the whole span
  contents (`v A.1`), not assume a single trailing letter — the Little Jord doc
  generator does exactly this.

## Next session — nothing queued

Nothing is queued. Open threads, none urgent:

- **The soundscape has still never been heard by anyone building it** — four
  generative layers (drone, plucks, percussion, chimes) plus the swell macro,
  all reasoned from the Web Audio graph, no audio out in any session that wrote
  them. Character, tuning and mix balance want a real listen.
- **The grown-up print path isn't paper-verified.** Little Jord's two sheets
  have now been printed for real and sit comfortably one page each; the
  three-sheet adult path (keepsake + wave, data, garden) never has been.
- **More of her drops and creatures** whenever the mood takes — the pattern is
  settled and written up under "Adding to it" in `04_DOCS/little-jord.html`.
- The **standalone soundscape module** (`SOUNDSCAPE_MODULE_PLAN.md`) is still
  planning-only, and deliberately so.

---

Last updated: 2026-07-11, after making the **"Today" chart rolling (rev P)**:
`chartToday()` (renamed `chartLast24h()`) used to filter by calendar day, so
"now" only sat at the chart's right edge late in the evening. Rewritten to
match `chartLastHour()`'s pattern — filter by the last 24h of timestamps,
"now" always pinned right — across both the on-screen tracker and PDF page 2
(heading now "Last 24 hours · by hour" in both places). Prior: a
**garden-reset + PDF-spacing round (rev O)**:
a new Settings option "Garden resets" (5am default / weekly / at 50 flowers —
`gardenResetMode`, `checkGardenReset()`, replaces the old day-only
`resetGardenIfNewDay()`), a 200-flower hard cap (was 150), and page 1 of the
keepsake PDF loosened up (bigger portrait, generous margins) instead of
fighting for a strict one-sheet fit. A real bug was caught before shipping:
the Settings-select wiring initially referenced `gardenResetMode` ~230 lines
before its `let` declaration actually ran (temporal dead zone — would have
thrown on every single garden-planting action for every user), found via a
headless DOM test that actually exercises the reset flow rather than just a
syntax check. Prior: a **keepsake-PDF round (rev N)**: three
additions to page 1 of the printed keepsake — a **today's-shelf strip** (a
compact deduped-by-kind row of the objects that drifted in since the 5am reset,
mirroring the existing garden strip; `.ksh-*` styling), a **rare-item
reflection** (one quiet gold line from a new `RARE_NOTES` pool, shown only if
`shelf.some(s=>s.rare)`; omitted otherwise so there's no empty gold box), and a
**bigger Jord portrait** (`.ks-art` 118px→166px) that now has simple arms/hands
and on ~1 reprint in 3 holds a random `OBJECTS` item. Node-verified (syntax +
today-shelf filter/dedup simulation) but NOT print-verified. Prior: 2026-07-10,
a **soundscape round (rev M)**: a tidy pass
(deduped the averaged-trend chart overlay into one `avgOverlay()` helper; a
dead-code sweep came up clean), a new **chimes/gongs/bowls layer** (slow,
spacious, inharmonic sine-partial strikes — singing bowl / bell / temple bowl,
own sub-toggle + type + volume), a dedicated **percussion Dynamics** slider
(soft-vs-loud hit range, split out from Groove which is now timing-only), and a
**Swell** macro (the whole mix breathes louder/softer in slow randomized waves
via a downstream `ambSwellGain` node). Prior: an **optional percussion layer** to the
ambient soundscape — a third generative layer (off by default, own sub-toggle)
with three synthesised textures (brushes/taps/drops) and a **Groove** slider that
loosens the timing from metronome-steady to loose/syncopated (footer bumped to
**rev K** — see the newest changelog entry). Prior same day: **ambient sky
flyers** — occasional butterflies (reusing `BUTTERFLY_SVG`) and the odd distant
bird drifting through the upper sky, biased toward the top-left (footer was at
**rev J**). Prior same day: **5 new expressive eye variants**
(look-left/right/up, a lowered-lid "peer", and a cute "uwu") wired into the
mood system (footer bumped to **rev I** — see the changelog).
Prior same day: a **last-hour vibe chart** and an
**averaged trend-line toggle** across all three charts + PDF (footer bumped to
**rev H** — see the newest changelog entry). Prior same day: a **Rare** section to the keepsake shelf
(objects now roll a ~6% "rare" flag on spawn, get a gold glow + sparkles on the
main stage, and collect into their own gold-bordered/twinkling shelf grid — see
the newest changelog entry; footer bumped to **rev G**). Prior same day:
occasional garden visitors (butterfly/ladybug/spider drifting through the
`#garden` panel), a "slow, fady, floaty ghost" motion pass and a keepsake-PDF
mini-garden strip. Prior:
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
a clock time (it has to be kicked off when someone's around to start it). (The
five items the failed routine was supposed to do — bubble width, loading
flourish, bounce, moon phase/drift, water states — were all subsequently
shipped anyway, via the local `Agent` path, across later rounds this same day.)

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
  not from vibe-logging — that was a real bug once, see caveats). Reset
  schedule is user-choosable (Settings > Garden resets, `gardenResetMode`,
  persisted): **5am** (default — `gardenDayKey()` shifts the clock back 5h
  before taking a calendar day, so anything before 5am still counts as
  yesterday's growth), **weekly** (resets every 7 days off a stored
  `gardenResetAt` timestamp — if that timestamp doesn't exist yet, it's seeded
  to *now* rather than triggering an immediate reset, so switching modes never
  silently wipes an existing garden), or **at 50 flowers** (count-based).
  `checkGardenReset()` (was `resetGardenIfNewDay()` — renamed since it's not
  day-only anymore) is checked both on planting and on opening the panel.
  Hard-capped at **200** blooms regardless of mode (was 150), as a backstop.
  Flowers scatter/overlap chaotically (deterministic per-entry hash via
  `gseed()`, not a tidy grid like the shelf), each paired with a small symbol
  from `MISSION_ICON` for whichever activity triggered it.
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
  a `×N` badge). Each spawn also rolls a **`rare`** flag (`Math.random()<.06`,
  ~6% — deliberately between the golden bloom's 3-5% and the "huge" 8%) recorded
  on its shelf entry (`{i,react,t,rare}`; old entries lack the field, `undefined`
  == not rare). A rare visitor gets a pulsing gold glow circle behind it plus a
  scatter of twinkling 4-point `.obj-sparkle` stars — both on `scaleWrap` (NOT
  `lifeWrap`, so the per-instance hue-rotate leaves the gold true), each star
  nesting a positioning `<g>` inside an animating `<g>` to avoid the SVG
  transform-vs-CSS-animation clobber. Both carry a static gold opacity so
  reduce-motion (which freezes `.obj-rare-glow`/`.obj-sparkle`) still shows a
  distinct gold marker.
- **Keepsake shelf sections** — the `#shelf` panel has THREE stacked sections,
  all sharing `shelfTiles(items, opts)` (group-by-kind + `×N` badge): **Today**
  (`sh-today-*`, 5am reset via `gardenDayKey`), **Rare** (`sh-rare-*`, small
  6-col grid fed by `shelf.filter(s=>s.rare)`; `shelfTiles(...,{rare:true})`
  stamps each tile `.sh-rare-item` = static gold border + `shRarePulse` glow, and
  appends a `.sh-sparkle` `✦` corner twinkle), then **All time** (`sh-grid`, the
  full catch-all). All three grids delegate clicks to the shared `shelfItemClick`.
- **Ambient soundscape** — opt-in, Web Audio only (no audio files), reactive to
  live app state (vibe band, calm tier, day/night, whether an object is
  visiting). Settings live on their own `#sound` page (reachable via a header
  icon, not buried in Settings). Includes a "variety" control that scales how
  much the generative engine wanders.
  - **Percussion layer** (rev K) — a THIRD generative system alongside the
    drone voices and `scheduleAmbNote()`/`playAmbNote()` plucking, structured
    the same way: `schedulePercHit()` re-arms itself via `setTimeout`,
    `playPercHit()` does one-shot synthesis. Opt-in and independent of the main
    `ambientOn` toggle via `ambPercOn` (`#snd-perc-toggle`) — it produces sound
    only when BOTH flags are true (`schedulePercHit()` guards on
    `ambientOn && ambPercOn && actx` and self-cancels otherwise), and the
    toggle's change handler calls `schedulePercHit()` directly so it engages/
    disengages while the rest of the soundscape keeps playing (no full restart).
    `startAmbient()` builds a shared 0.5s white-noise buffer (`ambNoiseBuf`,
    reused via a fresh `AudioBufferSourceNode` per hit) and kicks off
    `schedulePercHit()`; `stopAmbient()` clears `ambPercTimer` and nulls the
    buffer. Percussion routes `src → biquad → gain → ambMaster` (straight to
    master, NOT through the drone's dark ~420Hz `ambFilter`, so transients keep
    their bite); on/off + tab-hide are handled for free because everything hangs
    off `ambMaster`/`actx` (master fades to 0 when `ambientOn` off; `actx`
    suspends when hidden and `playPercHit()` early-returns unless
    `state==="running"`). Three types (`ambPercType`, `#snd-perc-type`):
    **brushes** (airy bandpass ~4200Hz Q0.7, 0.14s), **taps** (woody tick,
    bandpass ~1600Hz Q6, 0.06s), **drops** (soft round mallet, lowpass ~300Hz,
    0.22s) — each a noise burst with a 4ms linear attack + exponential decay,
    node disconnected on `onended` (no leaks). The **Groove** slider
    (`ambPercGroove`, `#snd-perc-groove`) is the "how vibes it is" control:
    beat interval `base = 1.8 - density*0.9`s; groove adds `±base*groove*0.6`
    random jitter, and above groove 0.15 rolls a chance to squeeze in an early
    syncopated hit (`wait*=0.45`) or drop a beat (rest), plus scales a
    per-hit velocity humanisation — so groove 0 = metronomic, groove 100 =
    loose/swung/human. Persisted like every other soundscape control. Audio
    only — NOT ear-verified in this env (no audio out); reasoned for graph
    correctness (envelopes reach ~0, sources stopped+disconnected, sane buffer
    length/filter ranges) but the actual *character/mix balance* of the three
    textures wants a real listen.
  - **Volume + amplified vibey-end (rev L)** — two independent volume sliders:
    `ambVolume`/`#snd-volume` scales `ambMaster.gain` directly (the true
    overall level, since drone+notes+percussion all route through it), while
    `ambPercVolume`/`#snd-perc-volume` is an additional per-hit multiplier
    applied only in `playPercHit()`'s `peak` calc (a channel fader on top of
    the master). Both default 100 so existing saved setups don't go quiet.
    Variety's and Groove's high ends got a quadratic boost layered on top of
    their original linear terms (e.g. `vari*(340+vari*560)` for filter
    cutoff wander, was flat `vari*340`) so max settings genuinely roam/swing
    much further while low/mid values are barely changed from before (the
    quadratic term is small until past ~halfway) — max Variety now wanders
    ~±900Hz with a 0.5s glide (was ~±340Hz/2.4s), max Groove hits ~150%
    timing jitter with ~53%/~46% syncopation/drop chances. Groove above 0.6
    also occasionally substitutes a different percussion type for one hit
    (`PERC_TYPES`, up to 20% at groove=1) for real timbral variety, not just
    timing wobble.
  - **Percussion Dynamics + chimes layer + Swell macro (rev M)** — three
    additions this round:
    - **Percussion Dynamics** (`ambPercDynamics`, `#snd-perc-dynamics`,
      default 50): a dedicated per-hit loudness-range control. Groove is now
      timing-only; the velocity swing moved here. `playPercHit()` scales
      `peak *= 1 + (rand*2-1)*(0.12 + dyn*0.88)`, so 0 = fairly even (±12%),
      100 = ±100% (some hits clamp to a silent ghost, some ~double). The
      type-specific base peaks stay the centre of the swing.
    - **Chimes/gongs/bowls** (`ambChimeOn`/`#snd-chime-toggle`,
      `ambChimeType`/`#snd-chime-type`, `ambChimeVolume`/`#snd-chime-volume`;
      `scheduleChimeStrike()` / `playChimeStrike()`, timer `ambChimeTimer`,
      `CHIME_TYPES`): a FOURTH generative layer, same opt-in/independent shape
      as percussion (guards on `ambientOn && ambChimeOn && actx`, toggle
      re-arms live). Deliberately slow — `scheduleChimeStrike` waits
      `(24 - dens*10) * rand(0.5,1.8)` s (~7s..~44s), never a grid. Each strike
      is a few **detuned sine partials at non-integer ratios** (inharmonic like
      real bowls/gongs; the ±6-cent detune gives the shimmer/beating), summed
      into one gain, keyed to the drone's pentatonic root, routed straight to
      `ambMaster`. Three types: **bowl** (oct×2, partials [1,1.48,2.03], slow
      0.25-0.7s swell-in, 6-11s decay), **bell** (oct×4, [1,2.0,2.76,3.01],
      8ms attack, 2.6-4.4s decay — brighter/quicker), **temple** (oct×1,
      [1,1.5,2.01], deep, 8-14s). Envelope swells in then exp-decays to ~0;
      every node disconnected once all partials `onended` (no leaks).
    - **Swell** (`ambSwell`/`#snd-swell`, default **0 = flat**;
      `scheduleSwell()`, node `ambSwellGain`, timer `ambSwellTimer`, state
      `swellUp`): a macro rise/fall on the WHOLE mix. `ambSwellGain` is spliced
      `ambMaster → ambSwellGain → destination` so it multiplies everything
      *downstream* of `ambEvolve()`'s per-4s `ambMaster.gain` writes — the two
      never touch the same AudioParam (this was the key design constraint).
      `scheduleSwell()` re-arms each half-wave, linear-ramping between peak
      **1** (pinned — never louder than Volume, so no clip) and trough
      `1 - sw*0.7` (down to 0.3 at full). One slider scales depth AND pace:
      full-cycle period `(90 - sw*50)` s with a `rand(0.6,1.4)` buffer on each
      half-wave (~24s..~108s span). `ambSwell 0` early-returns (eases to unity,
      stops cycling) so it costs nothing when off. Audio-only, so
      `prefers-reduced-motion` (visual) doesn't apply.
    - All rev-M additions node-verified (syntax + every new store key
      read+written, every new DOM id present in HTML and referenced in JS) but
      **NOT ear-verified** — no audio out in this env.
- **PDF export** — Trends → "Save as PDF" uses the browser's native
  `window.print()`, and is now THREE printed pages, each populated at print
  time via a single chain off `populateKeepsake()` (→ `populateWave()` →
  `populateDataPage()` → `populateGardenPage()`), so the `tr-print` click
  handler still only calls `populateKeepsake()`. All the print containers are
  siblings inside `#trends`'s `.tr-inner`, `display:none` on screen and shown
  only inside the `@media print` block.
  - **Page 1** — `#tr-keepsake` + `#tr-wave` share ONE sheet (no
    `break-before` between them): the whimsical keepsake (header, a bigger Jord
    **portrait** with arms/hands that sometimes holds an object, sweet stat,
    quote, an optional gold **rare-item reflection**, a capped 12-bloom mini
    **garden strip**, a capped 12-kind **today's-shelf strip**, footer) then the
    wave coda. The portrait `art` string and both compact strips are built in
    `populateKeepsake()`: the portrait `<svg viewBox="0 0 150 100">` draws the
    face + arms (`#arm-l`/`#arm-r`-style curved stroke + hash-mark fingers,
    attached at the lower sides of the r=33 face circle) and, on `Math.random()
    <.33`, a `pick(OBJECTS).svg` wrapped in `translate(92,86) scale(0.38)` to sit
    in the right hand; `.ks-art` CSS width is 166px. The **today's-shelf strip**
    (`.ks-shelf`/`.ksh-head`/`.ksh-row`/`.ksh-none`) filters `shelf` to the
    `gardenDayKey` day and dedupes by kind inline (same Map reduction as
    `shelfTiles()`), capped at 12 kinds. The **rare reflection** (`.ks-rare`,
    gold `var(--sun)`) renders only when `shelf.some(s=>s.rare)`. The mini
    garden strip (page 1) is capped at 12 blooms; the whole thing is
    **NOT print-verified** — the one-sheet fit of portrait+two strips+wave is
    the main thing worth a real print check.
    The wave line uses `smoothPathRamp(pts, sAt)`: the last 24h stays genuinely
    jagged (low `sAt`), older-than-24h overshoots past normal Catmull-Rom
    smoothing (`sAt` up to ~1.6) for an exaggeratedly silky look, so "spiky day
    → smooth week" reads as one continuous but clearly-contrasted line.
  - **Page 2 "JORD · the data"** (`#tr-data`, `break-before:page`) — the SAME
    charts as the on-screen Trends view, titled "Last hour · by the minute",
    "Today · by hour" and "Last 14 days · daily average". `populateDataPage()`
    reuses `chartLastHour()`/`chartToday()`/`chartHistory()` verbatim (they
    return complete self-contained `<svg>` strings off current
    `vibes`/`missions`, and honour the `showMissions`/`showAverage` flags at
    print time automatically).
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
- **Little Jord mode** (rev Q) — an opt-in kid mode, `kidMode` (`store` key
  `kidMode`, default false), toggled at the top of Settings (`#kidtoggle`) and
  mirrored as `body.kid` by `applyKidMode()`. Grown-up Jord is unchanged: every
  difference is gated on the flag, most of them through the one-line helper
  `kp(adult, kid)` used at the call site (`pick(kp(THOUGHTS, KID_THOUGHTS))`),
  so the two voices never blur. What it changes:
  - **Nothing is recorded.** `body.kid` hides `.vibes` wholesale (slider, log
    button, and the ▤ trends button, which lives inside it) plus the header's
    `#lastcheck`; `fireReminder()` early-returns. `vibes` data is untouched, not
    cleared — flipping back restores the lot.
  - **The wish** (`.wish`/`#wish`, `#b-wish`, `#wish-text`, pools `KID_WISHES`/
    `KID_WISH_SAYS`) takes the vibe row's place: one intention for the day,
    stored as `{d, w}` under `kidWish` against `gardenDayKey()` (the same 5am
    day boundary as the garden), re-rolled by tapping again, never the same one
    twice running. Each wish plants a bloom via `plantGarden("wish")` —
    deliberately NOT `logMission()`, since missions are telemetry and this
    isn't. `MISSION_ICON.wish = "✿"` exists only to give that bloom its symbol.
  - **Hearts** — `floatHeart(color, pop)` gained a colour and a `pop` variant
    (`.heart-pop`, bigger travel, staggered `animation-delay`); `heartBurst(n)`
    fires a handful in assorted `HEART_HUES`. The old 45s solitary-heart timer
    is now a 20s one: kid mode bursts at 40% a tick, grown-up Jord keeps its
    original effective rate (was .22/45s, now .1/20s, and still gated on
    `tierOf(calm)>=3`). Pats, rewards and wishes all burst. `MOODS.smitten`
    (heart eyes) is a kid-only pool and `pickMood()` ignores the calm tier
    entirely in kid mode — nothing is being scored, so the face doesn't track it.
  - **Rainbows** — `rainbow()` + the `#rainbow` SVG layer (sibling of
    `#orbbody`, like the other sky layers), seven arcs r=128..106 centred on
    (140,154) so it frames Jord without touching him. `.rainbow-arc` fades in,
    holds ~12s, fades out; one at a time (guarded on the layer being empty).
    Kid mode only — it never appears for grown-up Jord.
  - **More surprises** — object spawn interval 50-140s (was 150-420s), the rare
    flag rolls 22% (was 6%), garden visitors arrive 85% of opens (was 38%),
    idle thoughts every 45-110s (was 70-180s).
  - **Small fingers** — action buttons grow to 66px with 23px icons, icon
    buttons to 40px, corners rounder. The ⚙ gear needs a deliberate 900ms
    press-and-hold in kid mode (plain tap gets a line from Jord instead), so a
    child poking every header button doesn't land in Settings and flip the mode
    off. Implemented as pointerdown/up + a `held` flag that swallows the
    trailing click.
  - Kid copy pools all live together above `OBJECTS`: `KID_THOUGHTS`,
    `KID_NIGHT_THOUGHTS`, `KID_WAITING`, `KID_IS`, `KID_WISHES`,
    `KID_WISH_SAYS`, `KID_PRAISE`, `KID_ACCEPT`, `KID_ENJOY`,
    `KID_GROUND_STEPS`. Breathe/ground/sit mechanics are identical in both
    modes; only the wording and the reward lines swap.
- **Little Jord, second pass** (rev R) — the mode got its own row of buttons,
  its own flora, and a sillier garden. Read alongside the rev-Q note above.
  - **Her button row is four wide, not five**: Breathe, Giggle, Love, Magic.
    `body.kid` hides `#b-ground` and `#b-accept` and reveals `#b-giggle` (which
    is `display:none` everywhere else); `.actions` goes to 4 columns.
    `applyActionWords()` + the `ACTION_WORDS` table swap Enjoy→Love (♥) and
    Sit→Magic (✦) by rewriting each button's icon span and trailing text node,
    so the swap reverses cleanly when the mode goes off. Accept's sentiment
    isn't lost: `KID_ACCEPT` now surfaces in `scheduleThoughts()` at ~18% in
    kid mode, so "it's okay" still turns up on its own.
  - **Breathe** drops the two holds in kid mode: `phases` becomes just
    in/out, 4s each, 4 rounds (32s instead of 64s), with a different silly cue
    per round from `KID_BREATH_IN`/`KID_BREATH_OUT` shown in `#ex-body`.
  - **Giggle** (`#b-giggle`) is ground's slot doing the opposite job: six
    `GIGGLE_STEPS`, Jord pulling a face and flailing an arm on each, ending in
    a giggle fit (chaos eyes, 6 hearts, `GIGGLE_LINES`, a bloom).
  - **Love** takes no modal at all — `bigHeart()` grows one large pink heart
    (`.big-heart`, 5.4s) out of `translate(140,150)`, a line from
    `KID_LOVE_LINES` lands a beat later, a small burst follows, one flower is
    planted. Deliberately the least mechanical button in the app.
  - **Magic** is the sit, enchanted: 90s instead of 120s, `KID_SIT_LINES`, a
    `rainbow(true)` to open and close it, and `starDust()` drifting stars every
    19s via `sitStarTimer` (cleared in `endSit` alongside the other two).
    `rainbow()` gained a `force` argument purely so the sit can show one while
    `sitting` is true. `floatHeart`/`heartBurst` gained the same escape hatch
    for deliberate moments inside a `busy` mode.
  - **Poking him tickles**: in kid mode a pat gives a `TICKLE_LINES` giggle
    ~55% of the time plus `kidBounce()` (`.kid-bounce`, a much less composed
    one-shot on `#orbbody`, same discipline as `.wiggling` and never stacked
    with it — the class is cleared on a timer, not `animationend`, because the
    arms' animations bubble). Five fast pats become a giggle fit rather than
    the grown-up dizzy spell.
  - **One garden, two kinds of flower.** `plantGarden()` stamps `kid:1` on
    blooms planted in kid mode (only when true — grown-up entries are byte-for
    -byte what they were) and picks its symbol from `KID_SYM`. `renderGarden()`
    and the printed garden page both branch on `g.kid` to draw `KID_FLORA` /
    `KID_GOLDEN_FLOWER` (hearts, stars, a small rainbow, a swirl, a bunch of
    hearts), skip the hue-rotate so the pink stays pink, and add a faint
    offset `.gd-kid-tw` twinkle. The count line says "N blooms today · M with
    hearts" when both are present. Poking one of hers gives a `KID_GARDEN_LINES`
    line whichever mode you're in.
  - **Sillier garden company**: `GV_KIDS` adds bee (butterfly's flight path,
    faster), snail (the ladybug's, at a sixth the distance over ~40s), pet rock
    (`gvFade` only, 60-100s, never moves, has his own `KID_ROCK_LINES`),
    drifting hearts and twinkling sparkles. Visitors arrive on 85% of opens and
    every 18s at 60% while it's open. `el.dataset.kind` is what `gvPoke()` reads
    to decide between rock lines and `KID_VISITOR_LINES`.
- **Little Jord, third pass** (rev S) — small additions off the back of real
  use, all still gated on `kidMode`:
  - **Her own drops.** Three objects (heart, sparkle, dinosaur) live at the
    **tail** of `OBJECTS` carrying `kid:true`. That position is load-bearing:
    `shelf` entries store an *index* into `OBJECTS`, so new items may only ever
    be appended. `OBJ_ADULT_COUNT`/`KID_OBJECT_COUNT` split the array, and
    `spawnObject()` rolls inside `0..OBJ_ADULT_COUNT` for grown-up Jord (he
    never sees them at all) or, at 40%, inside the kid tail. `shelfTiles()`
    gives them `.sh-kid-item` (soft pink backing, in either mode — it's about
    whose keepsake it is) and skips the hue-rotate so they keep their colour.
    The printed keepsake's held object filters the same way. **To add more:
    append to the end and bump `KID_OBJECT_COUNT`.**
  - **Pink water**: `body.kid #t-water{stroke:var(--bloom)}` — the ellipses and
    the wave line carry no stroke of their own, so they inherit it. Verified
    live: both resolve to `rgb(227,169,189)`.
  - **The big heart** now drifts all the way up and out of the top of the frame
    (`translateY(-224px)` over 6.8s, holding opacity until 88%) instead of
    fading in place. Its removal timeout had to move with it (7200ms) — at the
    old 5800ms it vanished mid-drift.
  - **Breathe ends heart-eyed**: heart eyes, a big heart, and a five-heart
    burst on the completion screen.
  - **Poking him mid-giggle** is now its own interaction: the `giggling` flag
    (set when `#b-giggle` opens the modal, cleared in `closeModal()`, so every
    exit path clears it) is checked in the `#orbwrap` handler *before* the
    usual `if(busy) return`, giving heart eyes, a `kidBounce()`, flailing arms,
    forced hearts and a `TICKLE_LINES` giggle. Repeatable for as long as the
    exercise is open.
  - **The pet rock got a personality**: his SVG now has a `.rock-body` group
    (so he can move without touching the layer's fade), swappable
    `.rock-pupils`/`.rock-hearts` eyes, a slow `rockIdle` shuffle every ~6s and
    a `rockHop` when poked. `gvPoke()` gives him `KID_ROCK_SOUNDS`
    (*thud*, *shwwp*, *doonk*) 55% of the time and heart eyes for 4.5s at 45%.
- **Tuning A.2 against a real child** (v A.3), all three reported from actual
  use the day after A.2 shipped, and all three were real:
  - **"The hearts are all pink."** They weren't, in code: `heartBurst()` has
    drawn from `HEART_HUES` since rev Q. Two things made that a lie on a phone.
    A heart was a bare 1.4px outline with `fill="none"`, at which size a hue
    barely registers, and half of `HEART_HUES` was theme tokens
    (`var(--leaf)`, `var(--water)`) which the magic sit's **pinkout literally
    turns pink**, collapsing the palette exactly when the most hearts were on
    screen. Hearts now carry a `fill-opacity:.3` fill in their own colour, the
    palette is 9 literal colours that no theme can override, and a
    colour-less `floatHeart()` in her mode picks its own hue instead of
    defaulting to bloom. Grown-up Jord's rare drifting heart is still pink.
  - **"The giant heart doesn't work."** It did (it fired on presses 4 and 8 of
    a scripted run), but the odds were built for a scripted run, not a child:
    a 5-second window starting at the third press with a 30% roll, and the
    pauses to watch each heart were spending the window. The window is 8s now,
    the second quick press can already overflow, and **the fourth always
    does**, so determined smashing is guaranteed a payoff. The coin flip also
    leans 62/38 to the giant, since that's the one she's after.
  - **"We waited and waited and no rabbit."** She was one weighted entry among
    thirteen, so the meadow kept offering a snail instead. She now has her own
    timer, independent of the general visitor roll: one rabbit 2.5–7s after the
    garden opens, then another every 26–42s for as long as it stays open, with
    a one-at-a-time guard so a slow hop can't be lapped (an ordinary roll that
    lands on her while she's mid-hop becomes a butterfly). Her weight in
    `GV_KIDS` went 2/13 to 4/15 on top of that.
  - Harness at **73 assertions**. Verified live: a rabbit arrived 9.5s after the
    garden opened, four presses forced an overflow every time across six runs
    (three giants, three rainbows), and a heart came out `#c58fd6` with a fill.
- **More hearts, rabbits and variety** (v A.2): three asks straight from the
  kiddo, all little-jord-only.
  - **The rabbit visits the garden too.** She was only ever a drop and a magic-sit
    surprise; now `"bunny"` is in `GV_KIDS` (weighted 2 of 13) and
    `spawnGardenVisitor()` has a bunny branch: `RABBIT_ART` at 34px inside a
    `viewBox="-16 -28 32 31"`, travelling the ground line (top 70–84%) on the
    usual `gvFlit` path with `--wob:0`, since the vertical comes from
    `.gv-bunny .gv-hop`'s own `gvBunnyHop` loop instead. She's mirrored with
    `scale(dir,1)` so she faces where she's going, and she's excluded from the
    per-instance `hue-rotate` (like hearts and sparkles) so she stays the same
    rabbit you meet in the sky. Poking her speeds the hop to .34s for 1.8s and
    gives a `KID_BUNNY_LINES` line.
  - **The giggle is a variety tree.** `GIGGLE_STEPS` went from six fixed steps to
    six *branches* (wiggle / stretch / face / noise / shake / the hard bit)
    holding 48 `[label, instruction]` options between them, and the runner draws
    one per branch at open time (`GIGGLE_STEPS.map(branch=>pick(branch))`). The
    themes and their order never change, so the routine still feels like the
    same routine; the specifics never repeat. Labels travel with their option
    (a knee wobble is called "wobble", not "wiggle"), so the phase line stays
    honest. `GIGGLE_LINES` gained five endings.
  - **LOVE, pressed too fast, overflows.** `loveTaps` keeps the press stamps of
    the last 5 seconds (a list, so it forgets on its own; a slow, deliberate
    love is always just the one ordinary big heart). From the third quick press
    the chance of an overflow is `min(.8, .3+.2*(n-3))`, and an overflow is a
    coin flip between **`giantHeart()`** (the same heart path at `scale(6.2)`,
    116×95 user units against a 280-wide viewBox, on a 7.4s wobbling
    `giantHeart` animation, plus heart cheeks) and **`rainbowHearts()`** (two
    hearts per `RAINBOW_COLS` entry, staggered 170ms, spread 96/62px, with a
    real rainbow arc behind them). Either way she gets a `LOVE_RUSH_LINES` line
    and the taps reset, so she has to build it up again.
  - `floatHeart()` gained an optional fourth `spreadPx` argument, the only
    change to existing behaviour in this round, and it defaults to what it did
    before.
  - Harness at **70 assertions**, all passing (plus 15 magic-sit and 9 garden-
    bubble). Verified live in the browser pane: 3 giggle runs drew 3 different
    routines, rabbits spawned naturally in the meadow at 34×33px un-clipped and
    hopping, the giant heart fired on presses 4 and 8 of a fast run, and a
    MutationObserver caught all seven rainbow colours arriving as hearts.
- **Garden bubbles stopped being clipped** (rev Z) — reported from real use: poke
  a creature near an edge and its speech bubble was sliced off. Cause: bubbles
  were appended to the layer their subject lived in, and `#gd-visitors` carries
  `overflow:hidden` (rightly — it stops creatures spilling out of the meadow),
  so anything anchored near a boundary lost its bubble. Compounded by the
  placement being percentage-based with a `translate(-50%,-100%)`, which had no
  idea how wide the bubble actually was.
  - Both call sites (a poked bloom, a poked creature) now go through one
    **`gardenWord(text, anchor)`**. It renders into a new unclipped
    **`#gd-words`** layer, measures the bubble after insertion, clamps it on
    both axes to sit fully inside the field, and **flips it below the anchor**
    when there isn't room above. The `.gd-word` centring transform is gone,
    since placement is now in measured pixels.
  - `startGardenVisitors()` clears `#gd-words` on open alongside `#gd-visitors`,
    so a stale bubble can't survive a close/reopen.
  - Covered by `02_WORK/dom-test/garden-bubbles.test.js`, which stubs the
    layout and drives an anchor into all four edges and both top corners.
    Note it drives the **flower** path deliberately: `#gd-field`'s listener is
    delegated so an injected node works, whereas the visitor path binds its
    handler per element at spawn time and a synthetic node never gets one.
  - Same round: the garden's bloom `ry` floor went 8% → 15%. A bloom's artwork
    is drawn *upward* from its anchor, so at 8% the top flower was sliced by the
    panel header.
- **Her own print sheet** (rev Y) — `#tr-kidpage`, built by `populateKidPage()`
  on the same `populateKeepsake()` chain as the others. In kid mode the print
  becomes **two sheets, not four**: her page, then the garden. The print media
  query drops `#tr-keepsake`, `#tr-wave` and `#tr-data` under `body.kid`,
  because all three carry numbers (a sweet stat, the week's wave, the charts).
  Her sheet has: a big heart-eyed Jord with pink cheeks and both arms out, the
  wish she's carrying today, a row of the blooms *she* planted (`garden` filtered
  on `kid`), a row of her own drops that came to visit (`shelf` filtered through
  `OBJECTS[i].kid`), one warm line from `KID_PAGE_LINES`, and the footer
  "made together · nothing here was counted". **No count, no average, no orbit
  day, no chart** — a test strips the markup and fails if any digit other than
  the date survives, and another fails on stat vocabulary (average / logged /
  entries / streak / score).
  - She needs her own way to reach it: the ▤ trends button lives inside the
    vibe row, which kid mode hides, so `#b-kidprint` ("♥ a picture of today")
    sits under the wish button and runs the same populate-then-print.
  - `02_WORK/dom-test/preview.js` renders the sheet from sample data into a
    standalone HTML file — the only way to actually *look* at a print sheet in
    this environment, since there's no print preview and the browser pane can't
    screenshot.
- **The tickle really is 5% now** (rev X) — rev V set the tickle branch to 5%
  but he still wobbled and giggled on nearly every poke, because the bounce had
  three *other* sources. All three fixed:
  1. **The rapid-poke giggle fit** (5 pokes inside 3s) fired constantly, because
     a child pokes fast — that threshold was written for an adult idly patting.
     In kid mode it now rolls its own 5% and otherwise falls through to the
     ordinary spread. Grown-up Jord's dizzy spell is untouched.
  2. The **heart spray** branch (20%) called `kidBounce()` as well as spraying.
  3. The **fall-through** poke called `kidBounce()` for kid mode instead of the
     gentle `.wiggling` both modes used to share.
  `kidBounce()` is now reached only by the 5% tickle and by poking him during
  the Giggle exercise, where it's the whole point. **Lesson: when a rate looks
  wrong, count every call site before adjusting the number** — the branch
  weight was right, the problem was three other callers.
  A test counts bounces per landed poke and fails if it exceeds a quarter, so
  this can't quietly drift back.
- **A pink T-rex and a rabbit** (rev W) — both are drops *and* big surprises,
  which is now the intended shape for anything new: draw it once as a `*_ART`
  const, list it in `OBJECTS` with `kid:true`, and register it in
  `MAGIC_CREATURES`.
  - `TREX_ART` (stroked in `--bloom`, so she is genuinely pink) and
    `RABBIT_ART` sit beside `DINO_ART`. As drops they're appended to the tail
    of `OBJECTS` and **`KID_OBJECT_COUNT` went 3 → 5** — the two must always
    move together, and a test now asserts they match.
  - `MAGIC_CREATURES` replaces the hard-coded dino branch: `{art, scale, at,
    move, in}` per creature, built by one `magicCreature()`. The rabbit gets
    `bunny-in` (arrives from the right) and `bunny-hop` instead of the dino's
    walk and bob. The old `#magicdino` id is now `#magiccreature`.
  - **Surprises come from a shuffled bag** (`nextMagicKind()`), not a fresh
    roll, so you never get the same one twice running. Worth knowing when
    testing: pinning `Math.random` no longer selects the kind, and the pinned
    value must be **below 0.65** or the "does one fire this tick" roll never
    passes at all.
- **The gear's press-and-hold was dead on a phone** (rev V) — reported as "the
  menu button doesn't work anymore", and it was a real bug, not a
  misunderstanding of the guard. The original implementation cancelled the hold
  timer on `pointercancel` and `pointerleave`, and on a touchscreen a long press
  raises one of those routinely (text selection, the callout menu, the browser
  guessing at a scroll). So: a tap was refused by design, and a hold was
  cancelled by the OS — the grown-up had no way in at all. Now the hold is
  cancelled **only** by a genuine early `pointerup`; a `pointercancel` lets the
  timer run through and open Settings, which is the lesser evil on a page that
  barely scrolls. Also: 900ms → 650ms, `contextmenu` suppressed in kid mode,
  `touch-action:manipulation` + `user-select:none` + `-webkit-touch-callout:none`
  on the button so the OS stops competing for the gesture, a `.holding` class so
  the press visibly does something, and a `pointerup` fallback that opens
  Settings if the press lasted long enough but the timer never fired. Three
  assertions cover it, including the interrupted-hold case.
  **If a press-and-hold ever looks broken again, check what's cancelling it
  before touching the timing.**
- **The tickle went to 5%** (rev V) — at 24% of pokes it swamped the other
  reactions and stopped being funny. The roll is now 5% tickle, 20% heart spray,
  20% heart blush, 15% turn-around, 40% falling through to the shared
  repertoire.
- **Varied home reactions + the magic sit's big surprises** (rev U):
  - **Poking him at home** is no longer always the tickle. One giggle every
    time got repetitive fast, so the kid branch of the `#orbwrap` handler is now
    a weighted roll: 24% tickle, 18% heart spray (`heartBurst(5)` + a line),
    14% heart blush, 14% turn-around, and the remaining 30% falls through to
    the shared grown-up repertoire (a drop, a turn, an ordinary thought). The
    unconditional per-pat `heartBurst` is gone — that was what made the spray
    feel constant. Milestone pats and the five-fast-pats giggle fit still take
    precedence. `blushHearts()` drives a new `#blushhearts` SVG group (two
    hearts at the cheek positions, sibling of `#orbblush`, self-fading) —
    needed because the ordinary blush is driven by the vibe slider, which
    doesn't exist in her mode.
  - **The magic sit now has big surprises.** Every 16s of sitting, a 65% roll
    fires one of `MAGIC_KINDS`: **fireworks** (nine bursts of radiating
    coloured spokes over ~10s), **the dinosaur** (`DINO_ART` at scale 2.9,
    ambling in from the left and bobbing), **pinkout** (`body.pinkout` swaps
    the whole palette to pink, with light and dark sets and a 1.8s transition
    on the SVG's stroke/fill so the world changes its mind slowly), and
    **heartfall**. Each clears itself after 15-20s.
  - **Touching sends a surprise away and leaves you sitting.** That's the
    mechanic: wait and another comes, touch and you're back to the quiet.
    `magicActive` is checked in the `#orbwrap` handler *before* the existing
    tap-to-end-sit path, and a stage-level listener catches taps that land
    beside him. A tap with nothing showing still ends the sit, unchanged.
  - **Two SVG gotchas worth keeping**: a CSS `transform` on an SVG element
    *replaces* its `transform` attribute outright, so the dinosaur's position
    lives on an outer `<g>` attribute and the walk-in animation on an inner one
    (get this wrong and he lands at the origin). And `DINO_ART` is now a shared
    const used by both her dinosaur drop and this surprise.
  - Covered by a second test file, `magic-sit.test.js` — it pins `Math.random`
    to force a chosen surprise and waits out the real 16s interval. All three
    kinds run green, including the "touch dismisses but keeps you sitting"
    behaviour.
- **Pink accents throughout her mode** (rev T) — the leaf green is the app's
  entire accent language, so kid mode now overrides every interactive use of it
  with bloom pink: button and icon-button hovers, all focus rings (buttons,
  selects, inputs), `button.primary` text, `#ex-phase` and `#ex-count` on the
  exercise card, checkbox `accent-color`, shelf-tile hover, and a soft pink
  resting shadow under her action buttons and the wish button. Ink, paper and
  borders are shared and untouched.
  - **Gotcha worth remembering**: `.stage`'s arrival animation runs with
    `fill-mode: both`, and an animation's fill beats any static declaration in
    the cascade regardless of specificity. The kid-mode pink ring added in
    rev R as a plain `box-shadow` rule was therefore never visible — it was
    being overwritten by `stageArrive`'s transparent 100% keyframe. Fixed by
    giving kid mode its own `stageArriveKid` keyframes whose 100% IS the
    resting ring. Toggling the mode swaps `animation-name`, which restarts the
    animation, so the pulse replays on switching too. Any future "static style
    on `.stage` doesn't apply" report is almost certainly this.
- **Chart lead-in vectors** (rev Q) — `chartLastHour()`, `chartLast24h()` and
  `chartHistory()` each draw a dotted, half-lit vector toward the newest entry
  that falls *before* their window, with a `"3h ago · 77"` note. The point
  itself is never drawn (it isn't in this window); only the direction, clipped
  at the plot box by `clipToBox()` and capped at 110 units long so a big gap
  doesn't paint a dotted line clean across the chart as if it were a measured
  trend. A completely empty window falls back to a 34-unit stub held at the
  last known level. Helpers: `prevBefore(cutoff)`, `agoText(ms)`,
  `clipToBox()`, `leadInSVG()`. The three "nothing yet" early-returns now also
  require *no* prior reading before they'll show the bare text.
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
- Rare shelf section + special animations (rev G): objects roll a ~6% `rare`
  flag at spawn (`spawnObject()`), recorded on the shelf entry. Live on the main
  stage a rare visitor wears a pulsing gold glow + twinkling 4-point stars (new
  `.obj-rare-glow`/`.obj-sparkle` classes on `scaleWrap`, gold untouched by the
  object's hue-rotate; static gold opacity so reduce-motion keeps the marker). A
  new **Rare** section sits between Today and All time on the `#shelf` panel — a
  small 6-col grid of `shelf.filter(s=>s.rare)` where each tile gets a gold
  border + `✦` corner sparkle via the new `shelfTiles(items,{rare})` option.
  Reduce-motion keeps the gold border/`✦` static (meaningful state) and only
  drops the pulse/twinkle. Structurally sanity-checked in Node (6% rate, filter,
  tile classes, old-entry fallthrough) but NOT browser-verified — the glow/
  sparkle *feel* and the sparkle-star placement relative to varying object scales
  are the things worth a real look.

- Last-hour chart + averaged trend-line toggle (rev H): new `chartLastHour()`
  filters `vibes` to the last 60 min and plots them on a minute-resolution
  x-axis (ticks at `-60/-45/-30/-15/now`, 60-min-ago pinned left at x=20, now
  right at x=300 via `X=m=>20+((60-m)/60)*280`), reusing `midBand()`, the leaf
  stroke, panel/ink dots and mission markers like the other two charts. It's
  first in the on-screen Trends order (most zoomed-in/recent) and a third
  `.dp-chart` on PDF page 2. New "averaged trend line" checkbox (`#avgtoggle`,
  `showAverage` persisted via `store`, **default off**) overlays a
  `smoothPath()` (Catmull-Rom) line at `opacity=".5"` / same leaf stroke on top
  of the raw polyline in ALL THREE vibe charts (`chartLastHour`/`chartToday`/
  `chartHistory`) whenever there are >2 points — guarded the same as the raw
  line. Because the overlay lives inside the shared chart functions, the PDF
  path picks it up for free. Structurally node-sanity-checked (axis mapping,
  overlay emission, empty-state) but NOT browser/print-verified — worth a real
  look at how the half-opacity smoothed line reads over the jagged one and
  whether three charts still paginate cleanly on print page 2.

- Expressive eyes expansion (rev I): 5 new `e-*` variants added as siblings in
  `#face` and registered in `EYES` — `lookleft`/`lookright`/`lookup` (r=8
  sockets like `e-open`, pupils shifted ±5 in cx, or -4.5 in cy for up — a
  harder, more obvious glance than the subtle +4.5 `e-side`), `peer` (a
  lowered-lid suspicious side-glance: a flat-ish upper lid + shallow lower
  curve forming a slot with pupils peeking up under it, slightly asymmetric
  L/R — distinct from `squint`'s symmetric pupil-less almond), and `uwu` (two
  rounder/wider upward arcs w/ `stroke-linecap:round`, cuter than `e-happy`'s
  arcs). Wired into `MOODS`: `uwu`→delighted/content/sleepy, the direction
  eyes + `peer`→curious (with `lookup`→pensive), and `uwu`/`peer` added to the
  poke-acknowledgement pool in `ackJord()`. Node-verified that every `EYES`
  entry ↔ `id="e-*"` element matches both ways and all MOODS eye refs resolve,
  but NOT browser-verified — the pupil-in-socket placement for the direction
  eyes and the peer slot/uwu arc shapes are reasoned from coordinates only, so
  worth a real look that pupils land inside the rims and uwu reads distinctly
  from happy.

- Ambient sky flyers (rev J): occasional gentle life for the empty upper-left
  sky the moon/sun/clouds leave bare — butterflies (art reused **verbatim** from
  `BUTTERFLY_SVG`, outer `<svg>` wrapper stripped at runtime via `SKY_BUTTERFLY`
  so it drops straight into `#jord`'s coordinate space) plus a smaller distant
  `SKY_BIRD` (a two-arc gull with a `.skybird-flap` scaleY flap), weighted
  `["fly","fly","fly","bird"]`. `spawnSkyFlyer()` appends a `<g class="skyfly">`
  to `#visitor` (the same sibling-of-`#orbbody` sky layer the UFO uses, so
  reactive body animations never drag them). Structure: outer `<g>` carries a
  static `transform="translate(startX,startY)"` start position; an inner `move`
  `<g>` runs the garden's **existing** `gvFlit`+`gvFade` keyframes (relative
  translate + fade, driven by per-instance `--dx/--dy/--wob` in px≈user-units,
  same trick the UFO uses) so it composes on top of the start; innermost a
  `<g transform="scale(...)">` sizes the art, and butterfly wings flutter via
  `.skyfly .gv-flap`→`gvFlutter` (reused). Removed on a `setTimeout` matching the
  flight (`dur*1000+400`) with a `skyFlyerCount` decrement — no leaked nodes;
  capped at 2 on screen. Cadence is **steady ambient**, not the rare UFO: a 20s
  `setInterval` with a 45% roll (guarded on `!busy && !sitting &&
  !modal.open && skyFlyerCount<2`) plus one seeded ~4s after load. Top-left
  bias: ~65% of spawns start in the left half (`startX` left `rand(4,112)` vs
  right `rand(150,252)`); `startY -15..48` keeps them in the upper sky clear
  above Jord's head, with a gentle local `dx`/`dy` wander + wobble (edge-nudge
  so they don't drift off-frame) rather than an edge-to-edge crossing — the
  fade in/out means they materialise mid-drift and dissolve. Per-instance
  `objHue` hue-rotate on butterflies for colour variety (birds left plain ink).
  Clickable for a deadpan line reusing `VISITOR_LINES` via `think()`.
  Reduce-motion: **no spawn at all** (pure ornament, same call as the garden
  visitors); `.skyfly`/`.skyfly .gv-flap`/`.skybird-flap` still added to the
  disable list for safety. Node-verified (syntax + wiring/cleanup regex checks)
  but NOT browser-verified — the flight-path feel, whether the top-band `startY`
  never clips the viewBox top (`y=-40`) at a wobble peak, and whether the bird
  reads as a bird are the things worth a real look.

- Optional percussion + Groove slider (rev K): a third generative layer on the
  soundscape, off by default behind its own `#snd-perc-toggle` (`ambPercOn`),
  engaging/disengaging live without restarting the rest of the engine. Three
  synthesised noise-burst textures (brushes/taps/drops via `#snd-perc-type`) and
  a Groove slider (`#snd-perc-groove`, steady↔vibey) that loosens the timing
  grid — see the percussion sub-note under the Ambient soundscape architecture
  section for the full graph/lifecycle/timing-math breakdown. Node-verified
  (syntax + every new store key read+written, every new DOM id present in both
  HTML and JS) but NOT ear-verified — no audio out in this env, so the texture
  character and mix balance want a real listen.

- Volume sliders + amplified vibey-end (rev L): independent Volume and
  Percussion-volume sliders (both default 100), plus a quadratic boost on
  Variety's and Groove's high ends so max settings genuinely roam/swing much
  further while low/mid feel is close to unchanged — see the sub-note under
  the Ambient soundscape architecture section for the exact before/after
  numbers. Groove above 0.6 can also swap in a different percussion texture
  for one hit. Verified with a standalone Node calc of the amplified formulas
  across the slider range, plus a headless DOM check of the new sliders'
  persistence — not ear-verified (no audio out in this env).

- Tidy + chimes layer + percussion Dynamics + Swell macro (rev M): four
  commits. (1) **Tidy** — folded the identical dashed averaged-trend `<path>`
  string, repeated verbatim across `chartLastHour`/`chartToday`/`chartHistory`,
  into one `avgOverlay(points)` helper; a const/let/var and function dead-code
  sweep came up clean (nothing orphaned to remove). (2) **Chimes/gongs/bowls**
  — a fourth generative layer (singing bowl / bell / temple bowl), slow and
  spacious, inharmonic detuned-sine-partial strikes; own sub-toggle/type/volume,
  same lifecycle as percussion. (3) **Percussion Dynamics** slider — soft-vs-loud
  per-hit range, split out of Groove (now timing-only). (4) **Swell** macro — the
  whole mix breathes louder/softer in slow randomized waves via a downstream
  `ambSwellGain` multiplier node, default off (flat). See the rev-M sub-note
  under the Ambient-soundscape architecture section for the full graph/synthesis/
  timing-math breakdown. Node-verified (syntax + store-key read/write + DOM-id
  presence both ways) but NOT ear-verified — no audio out in this env, so the
  chime character/tuning, the dynamics range and the swell depth/period all
  want a real listen.

- Keepsake-PDF page-1 additions (rev N): three commits. (1) **Today's-shelf
  strip** — a compact, deduped-by-kind row of the objects that drifted in since
  the 5am reset (same `gardenDayKey` boundary the on-screen shelf's Today
  section uses), capped at 12 kinds, mirroring the existing garden strip's
  bespoke `.ksh-*` print styling (NOT the screen-only `.sh-grid` classes). (2)
  **Rare-item reflection** — one quiet gold-tinted line from a new `RARE_NOTES`
  pool, shown only if a rare (~6%) item was ever kept (`shelf.some(s=>s.rare)`),
  a light `var(--sun)` nod to the app's rare=gold language; omitted entirely
  otherwise so there's no empty gold box. (3) **Bigger portrait + arms +
  sometimes-held item** — `.ks-art` grown 118px→166px, the portrait given
  simple arms/hands (the main stage's `#arm-l`/`#arm-r` convention: a short
  curved limb stroke + three hash-mark fingers) attached at the lower sides of
  the face circle, and on `Math.random()<.33` a random `pick(OBJECTS).svg`
  wrapped in `translate(92,86) scale(0.38)` held in the right hand (hands rest
  empty the other ~2/3). Node-verified (syntax + a today-shelf filter/dedup
  Node simulation confirming the 5am boundary and per-kind grouping) but NOT
  print-verified — whether page 1 (taller portrait + garden strip + shelf strip
  + optional rare line + wave coda) still fits ONE sheet, and whether the
  arms/held-object read right on paper, want a real Trends→Save-as-PDF check.

- Garden reset modes + 200 cap + PDF page-1 breathing room (rev O): a new
  Settings option lets the garden reset on a 5am boundary (default), weekly,
  or once it hits 50 flowers — see the Garden sub-note under architecture
  notes above for the full mechanics. Hard cap raised 150→200. Page 1 of the
  keepsake PDF given real margins and a bigger 220px portrait instead of
  fighting for a strict one-sheet fit, on request — the wave coda will likely
  land on its own page now, intentionally. A genuine temporal-dead-zone bug
  (Settings-select code referencing a `let` ~230 lines before its declaration
  ran) was caught by a headless DOM test that exercises the actual reset
  flow, not just `node --check` — fixed before it shipped. Verified via Node
  simulation: count-mode reaching 50 and resetting, weekly mode not wiping an
  existing garden on first switch, and the 200 cap actually capping. Not
  visually verified — nobody's seen the new Settings row in a real browser.

- Rolling "Today" chart (rev P): `chartToday()` → `chartLast24h()`, now a
  genuine rolling last-24-hours window (mirrors `chartLastHour()`'s "now
  pinned right" shape) instead of calendar-day-bound — on-screen tracker and
  PDF page 2 both updated, heading now "Last 24 hours · by hour" in both
  places. Verified with seeded data crossing a calendar-day boundary (23h-old
  point included, 25h-old excluded).

- Little Jord mode + chart lead-in vectors (rev Q): an opt-in kid mode (see the
  architecture note above) built because the app's owner's daughter had taken to
  Jord — especially the garden, the visitors and the heart eyes — and wanted a
  version of him with no mood logging, hearts and rainbows as the everyday
  register, a daily wish to carry, and buttons a small hand can hit. Grown-up
  Jord deliberately untouched. Alongside it, all three vibe charts learned to
  draw the dotted direction toward the last reading *before* their window, so a
  quiet stretch stops rendering as an empty box with no history. Verified in a
  real browser this time (a local `python -m http.server` on the repo + the
  in-app browser pane's JS console): kid mode toggling, persistence across
  reload, hidden vibe surface, the wish planting a `✿` bloom against the 5am day
  key, a 5-heart burst in mixed colours, the rainbow layer rendering, the
  press-and-hold gear guard (plain tap refuses, 900ms hold opens), and the
  lead-in geometry on seeded data in all three charts including the empty-window
  stub and the 110-unit length cap. **Not** visually eyeballed — screenshots
  weren't available in this environment, so nobody has actually *looked* at the
  rainbow's arc or the heart burst's spacing yet.

- Little Jord's own row + a shared garden (rev R): her four buttons (breathe,
  giggle, love, magic), her own flora planted into the SAME meadow as the
  grown-up blooms, tickling, and a sillier garden (bee, snail, pet rock,
  drifting hearts and sparkles). See the architecture note above. This round
  was verified properly: a jsdom harness now loads the real `index.html`,
  flips the mode, drives every button and asserts on the DOM and storage —
  34 assertions, all passing, including "kid mode records no mission" and
  "both kinds of bloom survive a mode switch". The harness lives OUTSIDE this
  repo (the single-file no-dependency rule stands) at
  `PROJECTS/JORD/02_WORK/dom-test/` in the workspace, with a README. Still not
  eyeballed: nothing here has been seen on a screen, so colour, spacing and
  the feel of the timings are all unverified.

- Little Jord, third pass (rev S): her own drops (heart, sparkle, dinosaur)
  with pink keepsake tiles, pink water, the big heart drifting out of frame,
  heart eyes at the end of breathe, poking him mid-giggle, and a pet rock with
  noises and heart eyes. The jsdom harness grew to 43 assertions. Two things
  worth knowing from this round: **animations are frozen in the in-app browser
  pane** (it doesn't composite, so `document.hidden` is true and every
  `getAnimations()` entry sits at `currentTime: 0` forever) — you can verify
  computed colours, classes and DOM state there, but never motion; and that's
  how the big heart's removal-vs-duration mismatch was found, by measuring
  rather than assuming.

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
   - **Bump the footer's `<span class="rev">v A.n</span>` (search for it) by one
     minor whenever a round ships a substantial change** — a standing
     convention the user asked for. Use judgment on "substantial": a single
     small copy/number tweak doesn't need it, a real feature or a multi-part
     round does. Never more than once per round, however many commits it takes.
     The full scheme (and when a major is warranted) is at the top of this file
     under **Version A.1**. Bumping the version means updating it in the
     workspace too — `showcase.json`'s Revision metric, `04_DOCS/showcase.html`,
     and `04_DOCS/at-a-glance.html`'s ledger all state it.
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
