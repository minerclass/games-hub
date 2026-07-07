# Code Review & Developer Handoff — Visual Game Builds

This document coordinates code review and developer handoff for Micah Miner's visual learning simulations. It bridges the public games hub with the individual game repositories for Load-Bearing and Trailhead.

## Repository Map

| Project | Repository | Public URL Target | Current Status |
| --- | --- | --- | --- |
| Games Hub | `minerclass/games-hub` | `https://minerclass.github.io/games-hub/` | Hub cards for Load-Bearing and Trailhead are present. |
| Load-Bearing | `minerclass/load-bearing` | `https://minerclass.github.io/load-bearing/` | Built and pushed to `main`; build log updated. |
| Trailhead | `minerclass/trailhead` | `https://minerclass.github.io/trailhead/` | Built and pushed to `main`; build log updated. |

GitHub Pages activation for Load-Bearing and Trailhead must be confirmed manually in repository settings unless a future agent has a tool that can change Pages settings.

## Shared Architecture Rules

All games in this portfolio should remain static and self-contained.

Required structure:

- `index.html`
- `style.css`
- `game.js`
- `BUILD_LOG.md`

Do not add frameworks, package managers, bundlers, compilers, build steps, CDNs, analytics, telemetry, databases, forms, cookies, trackers, or external API calls.

Approved font stacks:

- Serif display: `Georgia, serif`
- Sans UI: `"Segoe UI", sans-serif`
- Monospace: `Consolas, monospace`

The actual CSS may include fallback system fonts, but it must not call Google Fonts, FontAwesome, or any external font provider.

## Shared Simulation Constants

Where a game models challenge/access thresholds, use:

```js
const PRODUCTIVE_CHALLENGE = [55, 80];
const BARRIER_ACCESSIBLE_MAX = 20;
```

## Shared Disclaimer

Any game or hub element that displays scores, stats, achievements, rankings, progress indicators, or performance labels must include this disclaimer or a close variant:

> Note: All scores, stats, and achievements are illustrative models of the pedagogical friction framework, not validated learning assessment instruments.

## Code Review Findings

### Games Hub

The hub is a static single-page HTML document with inline CSS and a small inline filter script. It contains no external stylesheet or script imports. It includes the public-facing illustrative warning in the hero panel. It contains cards for Load-Bearing and Trailhead that point to their GitHub Pages targets.

Review focus for future agents:

- Confirm live Pages links after deployment.
- Keep hub cards accurate when a game moves from planned to deployed.
- Preserve accessible filter buttons with `aria-pressed`.
- Avoid dead links for games that are not yet deployed.

### Load-Bearing

Load-Bearing is a canvas-based simulation of structural learning versus surface polish. It contrasts artifact view and X-ray view, includes process-evidence logic, implements Sam's bypass rule, and uses the required constants.

Review focus for future agents:

- Run `node --check game.js` after code edits.
- Complete one structural run, one bypass run, and one replay cycle.
- Confirm the X-ray button and keyboard `X` behavior still work.
- Confirm the debrief disclaimer remains visible below the stats matrix.
- Confirm reduced-motion users receive static resolution rather than animation.

### Trailhead

Trailhead is an SVG-based simulation of calibrated challenge and exclusionary barriers. It uses deterministic hiker movement, shows path traces, and models the interaction between digital scaffolds, device bans, and AI bypass.

Review focus for future agents:

- Run `node --check game.js` after code edits.
- Complete calibrated support, gondola bypass, and turnstile lockdown scenarios.
- Confirm keyboard selection of SVG trail nodes works.
- Confirm the reflection block gates the framework mapping.
- Confirm the debrief disclaimer remains visible below score-like outputs.

## Definition of Done

Before marking a game or hub update complete, verify:

- The site opens from `index.html` with no build step.
- Browser console shows no errors.
- No external network calls are introduced.
- Keyboard navigation works.
- Focus states are visible.
- Feedback is not color-only.
- Layout works on desktop and mobile widths.
- Any score-like output includes the illustrative disclaimer.
- No data is collected, stored, transmitted, or logged.
- The related `BUILD_LOG.md` is updated.
- The games hub card status and URL are accurate.

## Remaining Manual Deployment Checks

The connector confirms repository access and file contents, but it does not expose a direct GitHub Pages settings action. A human or a future agent with Pages permissions should verify:

1. `minerclass/load-bearing`: Settings → Pages → Deploy from branch → `main`.
2. `minerclass/trailhead`: Settings → Pages → Deploy from branch → `main`.
3. Open the live URLs and complete browser smoke tests.
4. Confirm the games hub links resolve to live pages.

## Do Not Do

Do not add:

- React, Vue, Svelte, Next.js, Vite, Astro, Webpack, npm dependencies, or build tooling.
- Google Fonts, FontAwesome, CDN scripts, external icons, external images, or API calls.
- Analytics, telemetry, cookies, form submission, databases, user accounts, or cloud storage.
- Validated-assessment language.
- Claims that the games measure actual student learning.
- Participant data, survey data, interview data, transcripts, consent records, or identifiable research information.
