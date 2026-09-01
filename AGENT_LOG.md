# Agent Log

Append-only. Newest entry first. No participant data, committee or faculty names,
credentials, or tokens.

---

## 2026-08-31 - Fix a pre-existing contrast barrier on section labels

Found while probing every repo after CI caught a contrast regression elsewhere. This one
**predates all recent work**: it measured the same with the shared tokens disabled.

`.section-heading .label` ("Playable public scholarship", "Featured visual simulations",
"Connected hubs") used `--teal` (`#4fb7ad`) as small uppercase text on the paper ground,
measuring **2.11** against a 4.5 threshold. That is a real barrier, not a near miss.

Changed to the shared token layer's paper-ground teal ink (`#2a7268`), which measures
**4.96** there. The vivid `--teal` is untouched everywhere it is a border, fill, or motif
colour.

**Verified.** 188 text-bearing elements probed against their composited backdrops:
**zero** failures, down from three. Tightest remaining pair is 4.72 against 4.5.

---

## 2026-08-31 - Adopt the shared ecosystem design tokens

Links https://minerclass.github.io/tokens.css before the page styles and points this
page's ground, ink, and rules at the shared tokens while keeping its own accents. The
page's ground is unchanged: a paper page stays paper.

**Every reference carries a fallback** equal to the pre-adoption value, because a bare
`var(--mjm-bg)` is invalid at computed-value time if the token file fails to load, which
would break the page rather than leave it unchanged.

The track accent colours and their text-safe ink variants stay local: they drive the per-track card motifs and the finder, which are this hub's own system.

**Verified in a real browser.** Token sheet loads; body renders light with a contrast ratio
of 14.54; tag balance clean; zero console errors.

---

## 2026-08-31 — Text-safe accent variants for the year numeral

**Context.** Closes the accessibility finding recorded in the entry below, at the author's
request.

**Changed.** `index.html`, colour tokens only.

- Added five `--*-ink` tokens beside the existing track colours: teal `#2d7a71`, gold
  `#8a6412`, coral `#a94236`, blue `#3f6ea8`, green `#4a7340`.
- Added `--accent-ink` alongside `--accent` on `.game-card` and on each of the four track
  rules, so the pairing travels together.
- `.year` now uses `var(--accent-ink)`. **That was the only text usage of `--accent`**;
  confirmed by grep before editing. The other five usages are the 8px card border and four
  motif shapes, all decorative, and all keep the vivid colours.

**Result.** Every one of the 15 cards now clears **4.5:1**, the stricter normal-text bar,
not merely the 3:1 large-text bar that technically applies at 28px/800. Measured across
all 15: minimum 4.95, maximum 5.63, zero failures. Previously every card failed, including
the old teal default at 2.32.

Per-track: gold 5.17, blue 4.95, coral 5.63, green 5.19. Featured cards sit on a slightly
different composited backdrop than grid cards, which is why these differ marginally from
the standalone estimates in the entry below; all 15 were measured individually rather than
one per type.

**Visual outcome.** The numeral reads as a deeper, more grounded version of each track
colour while the border and motif stay vivid, so the card still colour-codes at a glance.

**Verified.** Tag balance clean, zero console errors, no `color: var(--accent)` left
anywhere. Filter regression: all six combinations pass. Motif animations still start on
focus (friction 1, media 3, history 1, source 4) and return to zero on blur. No page-level
horizontal scroll. The change is colour-only, so layout is untouched.

No commit or push — this machine has no configured git identity or credentials.

---

## 2026-08-31 — Track accent colours wired to primary track

**Context.** Fixes the open item recorded in the motif entry below, at the author's
request.

**Changed.** `index.html`, four CSS rules replaced.

```css
/* was: only ever fired for single-token data-track values */
.game-card[data-track="friction"] { --accent: var(--gold); }

/* now: keyed to the primary track via the motif class */
.game-card:has(.motif-friction) { --accent: var(--gold); }
```

**Result.** All 15 cards now take their track colour; **zero** remain on the default teal,
where 10 did before. Distribution: gold 9, coral 3, blue 2, green 1. Verified in-browser
that every card's computed `--accent` matches its motif type.

**Why `:has()` and not `~=`.** With multi-token values several `~=` rules would match and
the last in source order would win rather than the primary track — Friction Atlas
(`"friction media source"`) would have come out green. The motif class already carries the
primary track, so it is the reliable handle. A browser without `:has()` support falls back
to teal everywhere, which is exactly the appearance the old rules produced, so the
degradation is safe.

**Verified.** Tag balance clean, zero console errors, zero old exact-match rules left, all
four new rules present. Filter regression: all six combinations still pass. Motif
animations still fire on focus (friction 1, media 3, history 1, source 4). The change is
colour-only, so layout is untouched.

**Accessibility finding — pre-existing, not introduced, not fixed.** The `.year` numeral
(28px, weight 800, so WCAG "large text" needing 3:1) is coloured with `--accent`. Measured
against the composited card background:

| Colour | Ratio | 3:1 |
| --- | --- | --- |
| teal `#4fb7ad` (the previous default) | 2.32 | fail |
| gold `#d9a13a` | 2.22 | fail |
| blue `#7197c9` | 2.90 | fail |
| green `#7aa86f` | 2.64 | fail |
| coral `#cf695f` | 3.47 | pass |

**The old default teal already failed at 2.32**, so every card was below the threshold
before this change. This change is net neutral-to-positive: three cards (coral) now pass
outright, blue and green improved, and gold is marginally below teal. It neither caused
nor meaningfully worsened the issue, and fixing it alters a deliberate design flourish, so
it was left for the author.

If wanted, darkened text-only variants clear even the stricter 4.5:1 bar while the vivid
colours stay on borders and motifs: gold `#8a6412` (5.17), blue `#3f6ea8` (5.05), green
`#4a7340` (5.30), coral `#a94236` (5.75).

The 8px card border and the motifs themselves are exempt from non-text contrast: both are
decorative, the motifs are `aria-hidden`, and each card's category is also stated in a
text tag.

No commit or push — this machine has no configured git identity or credentials.

---

## 2026-08-31 — Per-track card motifs

**Context.** Follow-up to the finder entry below. The original handoff asked for animated
micro-previews of core mechanics for all 14-15 games; that was declined as too heavy. The
author chose the lighter option instead: one motif per learning track, four in total.

**Changed.** `index.html` only.

- Added a 72x44 motif to all 15 cards as the first child of `.game-meta`.
- Four forms, each an abstract figure of that track's core move:
  - **friction** - a dot crossing a ridged surface and losing speed.
  - **media** - one utterance expanding outward as concentric rings.
  - **history** - ticks on a timeline with a sweep reading across them.
  - **source** - stacked claims checked one at a time under a moving lens.

**Design decision.** The motif *form* is per-track; the *colour* follows the card's
existing `--accent`. That was deliberate, so adding motifs changes no existing card
colour. See the open item below for why that matters.

**How each card gets its motif.** From the first token of `data-track`, which is the
primary track. Verified in-browser: 15/15 motifs match their first track token and the
correct span count. It also matches the first visible tag on every card except *Informed
Action*, whose visible tag reads "Civic inquiry" — there is no civic track, and its
`data-track` begins with `friction`, so it takes the friction motif. That is the right
fallback, but worth knowing.

**Motion is opt-in.** Every motif is static and legible at rest. Animation runs only on
`:hover` or `:focus-within`, so keyboard users tabbing to a card's play link get the same
behaviour as mouse users. Motifs are `aria-hidden` since they are decorative.

**Verified.** Zero console errors; tag balance clean.

- Reduced motion: the test browser reports `prefers-reduced-motion: reduce`, and all four
  motifs correctly ran **zero** animations. The explicit `animation: none` block works.
- Normal motion: with the reduced-motion blocks neutralised at runtime (file untouched),
  keyboard focus started the expected animations on all four — friction 1 (`motif-slide`),
  media 3 (`motif-ring`), history 1 (`motif-sweep`, on a pseudo-element), source 4
  (`motif-verify` x3 plus `motif-lens`) — and every one returned to zero on blur, so
  nothing keeps animating off-screen.
- Real pointer hover confirmed separately on a source card: 4 animations running.
- 375px: zero overflowing elements, no page-level horizontal scroll, all 15 motifs at
  their intended 72x44.
- Finder regression check: all six filter combinations still pass, and every visible card
  carries a motif.

**Tuned during verification.** The first friction motif had ridges every 7px at 0.14
opacity with a 0.6-opacity dot, and the dot was lost in the pattern. Widened the ridges to
10px, lightened them slightly, and raised the dot to 9px at 0.9 opacity.

**Open, pre-existing, not changed.** The track accent rules use an exact attribute match:

```css
.game-card[data-track="friction"] { --accent: var(--gold); }
```

`data-track` is a space-separated list, so this only fires for single-token cards.
**10 of 15 cards fall back to the default teal**, and the colour system is mostly inert.
Left alone because fixing it visibly recolours most of the page, which is the author's
call, not an agent's. Note the fix is *not* simply `~=`: with multi-token values several
rules would match and the last one in source order would win rather than the primary
track. Keying off the primary track is what works, for example:

```css
.game-card:has(.motif-friction) { --accent: var(--gold); }
```

No commit or push — this machine has no configured git identity or credentials.

---

## 2026-08-31 — Combined game finder (purpose + session length + audience)

**Context.** Tier 1 scope from an agent handoff titled "Dissertation Repositories Visual
Modernization & Streamlining." That document asked for a "Find Your Game" filter by
session length and audience, plus animated micro-previews for every game.

**Starting state.** The hub already had a working purpose filter (`data-track`) with
`aria-pressed` buttons. Session length and audience were already printed on every card as
visible tags; they simply were not filterable.

**Changed.** `index.html` only.

- Added `data-length` and `data-audience` to the 12 grid cards. **Every value is derived
  from a tag already printed on that card** — nothing was invented. Verified in-browser by
  re-parsing each card's visible tags and comparing against the attribute: 12/12 match,
  zero mismatches. The filter therefore cannot disagree with what a reader sees.
- Replaced the single filter bar with a three-group finder (Purpose, Session length,
  Audience) that combines with AND, plus a Reset control.
- Added a live result count (`role="status"`) and an empty state for combinations that
  match nothing.
- Removed `aria-live="polite"` from the games grid. It had been announcing all twelve
  cards on every filter change; the count status region now reports the result instead.
- Added a `:focus-visible` ring (the page had none) and a `prefers-reduced-motion` block.

**Buckets.** Length: 10 min or less (6 games), about 15 min (4), 20 min or more (2).
Audience: classroom grades 6-12 (8), educators and leaders (4). Featured simulations sit
outside the filtered grid and always show; the count is scoped to the 12 grid cards.

**Verified.** Served locally, real browser, zero console errors, tag balance clean.
Eight filter combinations asserted against hand-counted expectations, all passing:
purpose=friction → 9; +length=short → 4; +audience=educator → 1; length=short → 6;
audience=educator → 4; an intentionally empty combination → 0 with the empty state
revealed; reset → 12 twice. At 375px there are zero horizontally overflowing elements.

**Fixed during verification.** First mobile pass made the finder 793px tall — nearly a
full screen before a reader reached any game — because the existing 560px rule forces
every `.filter` to `width: 100%` in a column. Overrode that inside `.finder` so the
buttons size to content and wrap inline; the label takes its own line. Height is now
529px, a 33% reduction, confirmed by measuring that buttons share rows.

**Not done, deliberately.** The handoff also asked for animated micro-previews of core
mechanics for all 14-15 games. Not built. Fifteen bespoke animations would add
significant page weight and ongoing maintenance to a card layout that already carries
dense descriptive copy, and the visual-noise tradeoff is an editorial call about the
author's own site rather than an agent's. Flagged for the author to decide; a lighter
option is one motif per track (four total) shown on hover and focus.

No commit or push — this machine has no configured git identity or credentials, so the
change is left in the working tree for review.
