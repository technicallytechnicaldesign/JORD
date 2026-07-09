# JORD — Handover Doc

Last updated: 2026-07-09, after a long collaborative session redesigning and heavily
expanding the app. This doc exists so a fresh Claude Code session (after a context
clear) can pick up exactly where things left off without re-deriving everything.

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
  the rare UFO) are siblings of `#orbbody`, not descendants — this separation
  matters: reactive animations (shake/deflate/blush/breathing) should target
  `#orbbody` specifically so they don't drag the whole sky along, which was a
  real bug fixed earlier in this project.
- **Full-screen panel pattern** — `#trends`, `#shelf`, `#sound` all follow the
  same convention: `position:fixed; inset:0`, toggled via an `.open` class, own
  close button. Mirror this for any new full-screen page.
- **Vibe slider** (`#vibeslider`) drives `reactToVibePreview(v)`, which computes
  mood bands and now also drives hot/cold tinting on the slider itself and a
  "blush" overlay (`#orbblush`) on Jord.
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
  `window.print()`. `#tr-keepsake` is a whimsical print-only summary page;
  `#tr-wave` is a second, data-only page (`break-before:page`) showing the
  week's logged vibes as one smooth line, no icons/glyphs.
- **Dark mode** — auto 5pm–5am, with a manual override (auto/always-on/always-off)
  in Settings.
- **`prefers-reduced-motion`** — there's a `reduceMotion` const and a
  `@media (prefers-reduced-motion:reduce)` block that lists every looping
  animation to disable. Any new looping animation needs to be added there.

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
   it unless there's a specific reason to need the cloud path again.

## Known caveats

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
