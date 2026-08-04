# When should I run? — v2

A single-file web app that picks the best time of day to run, based on the
weather forecast, daylight, and the user's daily schedule. Published on GitHub
Pages; must stay mobile friendly.

## Hard constraints

- **Single, self-contained `index.html`.** No build step, no frameworks, no
  external JS/CSS. Fonts (Hanken Grotesk, IBM Plex Mono, Newsreader — latin
  subsets) are embedded as base64 `@font-face` data URIs in the first
  `<style>` block. The only network calls allowed are to
  `api.open-meteo.com` (forecast) and `geocoding-api.open-meteo.com`
  (city search). One deliberate exception: `apple-touch-icon.png`
  (180×180, sage `#8fa9ab` with a serif R) ships next to index.html
  because iOS home-screen icons can't reliably use data URIs.
- Schedule times are fractional minutes — every feasibility comparison
  must use the `EPS` tolerance or exact-fit schedules get falsely flagged.
- Option cards are click-to-select; the click delegation must keep
  ignoring clicks on form controls nested inside a card (packing input,
  suggestions dropdown), or those controls become unusable.
- **Privacy.** No analytics, no cookies, nothing sent anywhere. User
  *preferences* persist in `localStorage` under key `wsir.prefs.v1`
  (this browser only). Daily inputs (date, distance, run type, preferred
  time, busy slots, packing lists) are deliberately never stored.
- **Defaults must stay close to the owner's setup** (Oslo, work 08:00–16:00
  with arrival by 08:00, 25 min commute, done by 19:00, easy/long 6:20/km and
  tempo/intervals/hills 6:00/km, cooldown 15 min for intense sessions only).
  New features should be additive and default-off or default-equivalent.

## File layout (all inside index.html)

1. `<style>` #1 — embedded fonts (huge base64 block; don't hand-edit).
   Regenerate only if fonts change.
2. `<style>` #2 — app CSS. Design tokens are CSS variables in `:root`.
   Mobile styles live in one `@media (max-width:680px)` block at the end.
3. `<script>` — vanilla JS, sections marked with `/* ---------- */` banners:
   - defaults & constants (`DEFAULT_PREFS`, `OPTION_META`, block colors,
     packing-list defaults & suggestions, quotes, WMO weather-code map)
   - prefs load/save (deep-merged over defaults so schema changes are safe)
   - per-visit `state`
   - sun-time math (generic lat/lon/timezone, ported from v1)
   - weather fetch + window aggregation (`windowWeather`)
   - scoring (`tempScore`, `scores` — weights normalized from
     `prefs.weights`; `bestStart` scans 30-min slots)
   - option building (`buildBlocks`, `assemble`, `buildOptions`)
   - rendering — plain template strings, full re-render via
     `render()`; plan view + settings view
   - events — delegation only: `data-action` (click), `data-change`
     (change), handlers in the `actions` / `changes` maps

## Domain model

- **Options** (weekday): morning before work, run to work, lunch (11:30),
  run from work (starts at work end, no travel), after work (travel home
  first). Weekend: morning/midday/afternoon bands with best-start search.
- **Planning blocks** per option (coffee, warmup, cooldown, shower, travel)
  come from `prefs.blocks`; durations from `prefs.times`. Travel placement
  is positional: pre-run for after-work, post-run for morning/run-from-work.
- Morning + run-to-work are scheduled *backwards* from `arriveWork`.
- **Feasibility** checks (in order): starts previous day, before
  `earliestStart`, in the past (when the date is today), misses work
  arrival, misses `doneBy`, overlaps a busy slot. Ineligible ≠ infeasible:
  ineligible cards (lunch over the limit) show only the reason.
- **Preferred time** (optional) creates a pinned `custom` card using the
  blocks of the slot the time falls into; it is excluded from the
  recommendation contest but is the default selection.
- **Selected summary**: the top panel shows whichever eligible card the
  user clicked (`state.selectedId`), falling back to preferred-time card,
  then the recommendation. The recommended card keeps its ✦ tag regardless.
- **Gym day**: every feasible option below `gymFloor` feels-like → show the
  treadmill panel.
- Packing lists: run-to-work and run-from-work cards each have their own
  list (`state.checklists.commute` / `.fromwork`) plus a shared suggestions
  dropdown that auto-adds on select (skips case-insensitive duplicates).

## Testing & deploy

- Test locally: `python3 -m http.server` in this directory (any static
  server works; opening the file directly also works).
- Deploy: push to `main` on GitHub, repo Settings → Pages → deploy from
  branch `main` / root. No build, no `.nojekyll` needed.
- After changes, sanity-check: mobile width (~375px), a weekend date, a
  weekday date, today's date (past options must be infeasible), settings
  round-trip (change → back to plan → reload page), and a non-Oslo location.
