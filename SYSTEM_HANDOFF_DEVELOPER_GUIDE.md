# System Handoff & Developer Guide: Visual Learning Simulations

This document serves as a cross-repository handoff package for an AI coding agent such as Claude Code, OpenAI Codex, or Antigravity to manage, extend, and deploy Micah Miner's visual educational games aligned with the Pedagogical Friction dissertation framework.

## Source of Truth

The `BUILD_LOG.md` inside each game repository is the most current project record for that game. Read the game-level build log before writing code.

For future builds, also read the visual-games roadmap and pitfalls checklist in the local workspace when available. The local workspace path may look like:

`C:\Users\mminer\OneDrive - bpd3.org\Desktop\Research\VISUAL_GAMES_HANDOFF.md`

Local Windows paths are workspace references only. Do not assume they exist in a remote coding environment unless the workspace has been mounted or synced.

Reviewed and corrected 2026-07-07 for Pages status, Load-Bearing content topic, Sam mechanic description, games-hub selector scope, and Trailhead state-reset/cache-busting notes.

---

## 1. Project Map & GitHub Repositories

All completed visual games and ecosystem portal pages are managed across these public GitHub repositories.

### Ecosystem Landing Portal

Repository: `minerclass/games-hub`

Live hub: `https://minerclass.github.io/games-hub/`

Primary file: `index.html`

Status:

- Deployed through GitHub Pages.
- Card layout restructured to place Load-Bearing and Trailhead in a dedicated **Featured visual simulations** section near the top of the games area.
- Duplicate lower catalog cards for Load-Bearing and Trailhead removed.
- Selector scope fixed so filter buttons target only the main catalog list: `.games .game-card`.
- Featured simulations remain visible while the main catalog is filtered.

### Load-Bearing

Repository: `minerclass/load-bearing`

Live game: `https://minerclass.github.io/load-bearing/`

Primary files:

- `index.html`
- `style.css`
- `game.js`
- `BUILD_LOG.md`

Status:

- Live through GitHub Pages from `main`.
- User/manual verification reports: structural run, bypass run, replay, and 390px mobile layout passed.
- No open source-side items recorded in the current handoff.

### Trailhead

Repository: `minerclass/trailhead`

Live game: `https://minerclass.github.io/trailhead/`

Primary files:

- `index.html`
- `style.css`
- `game.js`
- `BUILD_LOG.md`

Status:

- Live through GitHub Pages from `main`.
- Script tag uses `game.js?v=3`; bump the version query after every future `game.js` change to avoid stale GitHub Pages/browser cache behavior.
- Source includes fixes for:
  - Debrief state lookup using string hiker IDs directly.
  - Excluding gray-flag/gondola arrivals from successful summit count.
  - Clearing stale run intervals before a new run.
  - Re-hiding the continue button at the start of a run.
  - Avoiding repeated `init()` calls that would stack duplicate event listeners.
- User/manual verification reports: deterministic repeated runs and 390px mobile layout passed.
- No open source-side items recorded in the current handoff.

### Future Specifications

Reference file in local workspace: `NEW_VISUAL_GAMES_IDEAS.md`

Status:

- Completed specifications exist for:
  - Schema Constructor
  - Dialectical Crucible
  - Algorithmic Stream

---

## 2. Core Architecture & Code Review

### A. Load-Bearing

Load-Bearing illustrates the difference between polished artifacts and durable learning by letting players design a six-slot eighth-grade Reconstruction argument-essay unit.

Content topic:

`Was Reconstruction a success, a failure, or both — for whom?`

Do not confuse this with Trailhead. Trailhead is the Civil War causes game.

Key mechanics:

- **X-ray Toggle:** Holding `X` or using the on-screen X-ray button invokes `drawBridge` in X-ray mode, revealing members, hollows, cracks, and access braces rather than the artifact-view skirt.
- **Sam's Bypass Rule:** If surface polish is greater than zero and no process-evidence cards marked `pe: true` are present, Sam bypasses the bridge. The debrief reports this as a zipline arrival with a gray flag. There is no Sam traversal animation in Load-Bearing.
- **Calibration Bounds:** `PRODUCTIVE_CHALLENGE = [55, 80]` and `BARRIER_ACCESSIBLE_MAX = 20`.
- **Assessment Pattern:** Three loads test recall, application, and transfer. The game is designed to show that polish may carry light recall but not durable transfer.

### B. Trailhead

Trailhead illustrates the English Learner Paradox and exclusionary barriers through four student profiles navigating an eight-segment Civil War argument climb.

Key mechanics:

- **Deterministic Evaluation:** Hikers are evaluated segment-by-segment without random factors.
- **Maya:** Halts on text-heavy segments without digital Waymarks when digital support is available.
- **Jordan:** Halts on writing-heavy segments without the digital STT Bridge when digital support is available.
- **Riley:** Halts at the oral-heavy summit defense without Belay Partner support.
- **Sam:** Takes the AI Gondola when it is available and no checkpoint blocks bypass.
- **Device Ban / Turnstile:** Blocks the gondola, but also disables digital Waymarks and STT Bridge, creating infrastructural barriers for Maya and Jordan.
- **Vector Tracing:** Dynamically draws SVG path coordinates in gold, slate, forest-dim, and ember to trace each hiker's actual path.

---

## 3. Shared Technical House Rules

Any agent extending these repositories must follow these rules.

1. **Self-contained files only.**
   Build using standard HTML5, CSS, and vanilla JavaScript. Use `index.html`, `style.css`, and `game.js` unless a future specification explicitly calls for a single-file static build.

2. **No build system.**
   Do not add npm, Vite, Webpack, Next.js, React, Vue, Svelte, Astro, or other framework dependencies.

3. **No CDN dependencies.**
   Do not request Google Fonts, FontAwesome, TailwindCSS, external icons, external scripts, external images, or other remote assets.

4. **Use system fonts.**
   Approved font stacks:
   - Serif display: `Georgia, serif`
   - Sans UI: `"Segoe UI", sans-serif`
   - Monospace: `Consolas, monospace`

5. **Illustrative warning footer.**
   Any scored or score-like output must carry this disclaimer or a close variant:

   `Note: All scores, stats, and achievements are illustrative models of the pedagogical friction framework, not validated learning assessment instruments.`

6. **Calibration constants.**
   Where a game models challenge/access thresholds, use:

   ```js
   const PRODUCTIVE_CHALLENGE = [55, 80];
   const BARRIER_ACCESSIBLE_MAX = 20;
   ```

7. **No data collection.**
   Maintain a client-only architecture. Do not add trackers, analytics, cookies, databases, remote logging, form collection, participant data handling, or telemetry.

8. **Timed-mode controls.**
   Any game with a timed mechanic must offer an Untimed Mode toggle to meet accessibility expectations. Load-Bearing and Trailhead are not timed games.

9. **Accessibility baseline.**
   Preserve keyboard access, visible focus states, semantic structure, readable contrast, non-color-only feedback, and reduced-motion support where animation is used.

10. **Attribution and navigation.**
    Include a link back to the Games Hub on title/debrief screens where appropriate.

---

## 4. Immediate Next Steps

### Done as of 2026-07-07

- Games Hub is live.
- Load-Bearing is live.
- Trailhead is live.
- Games Hub featured visual simulations grouping is implemented.
- Games Hub filter selector scope is fixed to target only `.games .game-card`.
- Trailhead cache-busting uses `game.js?v=3`.
- Trailhead state-reset defects have been corrected in source.

### Next Build Work

Build new specifications from `NEW_VISUAL_GAMES_IDEAS.md`:

1. Schema Constructor
2. Dialectical Crucible
3. Algorithmic Stream

Recommended repository names:

- `minerclass/schema-constructor`
- `minerclass/dialectical-crucible`
- `minerclass/algorithmic-stream`

### Existing Catalog Visual Upgrades

Refer to the visual-games roadmap and Priority 3 items in the local handoff when available. Candidate upgrades include:

- Add the Hindsight Lens to Out of Time.
- Add student dots leaning, polarizing, or clustering in Common Ground.
- Preserve static-site rules and no-data-collection constraints.

---

## 5. Verification Checklist

Before considering any build complete, verify:

- [ ] No escaped template syntax renders as raw `${...}` in the browser.
- [ ] `node --check game.js` passes for any game using separate JavaScript.
- [ ] Replay fully resets state variables, progress indicators, timers/intervals, SVG/canvas elements, and visible controls.
- [ ] Event listeners are not duplicated across replay or start-over cycles.
- [ ] Keyboard navigation works for buttons, cards, SVG nodes, toggles, lens tools, and replay controls.
- [ ] Focus states are visible.
- [ ] Feedback is not color-only.
- [ ] Reduced-motion preferences disable or simplify active animation.
- [ ] Scored or score-like screens include the illustrative disclaimer.
- [ ] Layout works at approximately 390px width without clipped text or unusable controls.
- [ ] No external calls, CDNs, analytics, telemetry, cookies, or tracking are introduced.
- [ ] Games Hub links point to live GitHub Pages URLs.
- [ ] Related `BUILD_LOG.md` files are updated with every commit.
- [ ] For GitHub Pages games, version-query cache busting is bumped after `game.js` changes when needed.

---

## 6. Do Not Do

Do not:

- Rebuild working games from scratch unless explicitly instructed.
- Replace vanilla static builds with framework projects.
- Add external fonts, icon kits, scripts, APIs, analytics, or telemetry.
- Add participant data, survey data, interview data, transcript material, consent records, or identifiable research information.
- Claim that the games measure actual learning.
- Use validated-assessment language.
- Mix Load-Bearing's Reconstruction essay topic with Trailhead's Civil War causes climb.
- Re-run initialization functions on replay if they attach event listeners.

---

## 7. Completion Standard

A task is complete only when:

1. The requested game or hub update works locally and on GitHub Pages when deployed.
2. The build remains static and self-contained.
3. Accessibility checks have been completed.
4. No external calls or data collection were added.
5. Replay/start-over behavior is deterministic and clean.
6. Cache-busting has been updated when needed.
7. The relevant build log has been updated.
8. The games hub card status and link are accurate.
