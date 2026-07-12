# Games Hub — build log / continuation guide

> Audit trail for agents. This file tracks what is done, what is verified, and what remains.

## Status

- **Phase:** built, time-to-play & audience chips added, verified locally.
- **Repository:** `minerclass/games-hub`
- **Default branch:** `main`
- **Public URL target:** `https://minerclass.github.io/games-hub/`

## What the game is

Static catalog deck indexing all 14 interactive simulations in the visual games ecosystem, including featured experiences and search filters by track (History, Friction, Media, Source).

## Key implementation decisions

- **Files:** `index.html`. No build steps or package managers. Styling uses inline and standard system CSS.
- **Scoring constants:** None (static catalog).

## Verification checklist

- [x] Time-to-play chips (e.g. `⏱️ ~10 min`) added to all 14 featured and standard game cards
- [x] Target audience chips (e.g. `🎓 Teacher PD`, `🏫 Grade 6-12`) added to all 14 cards
- [x] Filtering logic verified: clicking filters (e.g. "Historical Inquiry", "Pedagogical Friction") filters cards correctly without errors
- [x] Passed local verification with zero console warnings


## 2026-07-12 - Answerable integration

- Added Answerable as a third featured visual simulation.
- Expanded the hub description to include technoskepticism.
- Added the public Answerable URL to the README.
- Pending at edit time: live Answerable deployment, responsive hub verification, link verification, and final catalog count update.
