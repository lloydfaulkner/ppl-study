# ppl-study

Single-file PPL ground school study organizer. Live at **https://lloydfaulkner.github.io/ppl-study/**

## Architecture

One file: `index.html` — all CSS and JS inline, no build step, no framework. Google Fonts (Inter) is the only external dependency.

Storage: localStorage only (saved videos, saved tools, LLM settings, theme preference).

## Deployment

GitHub Actions deploys automatically on push to `main`. The workflow (`.github/workflows/pages.yml`) uploads the repo root and deploys to GitHub Pages. Do not manually sync a `gh-pages` branch — just push to `main`.

## Design

- Sectional chart aesthetic: tan/khaki background `#f0ead8`, blue `#004fa8` as primary accent, magenta `#b8005a` sparingly (logo only + pulse animation)
- Night mode: dark slate `#14161a` base, NOT warm brown
- Inter font, map grid overlay on body
- CSS custom properties in `:root` and `[data-theme="night"]`

## Structure

**Four tabs** (Study Advisor is first/default):

1. **Study Advisor** — 15 topic chips → static chapter recommendations → clickable rows open the source directly (FAA docs open the specific chapter PDF or HTML in a new tab ↗; books jump to the Library card with pulse highlight →). AI tips button if API key set.
2. **Library** — 6 FAA docs + 6 books. Filter: All / FAA Official / Books. Card/list view toggle. Cards have "What it is" + "When to read it" only (no chapter breakdown — that lives in Study Advisor).
3. **Videos** — 8 seeded YouTube videos + user-saved. Filter: All / My Saved / Weather / Radio / Landing. "+ Save a Video" modal saves to localStorage.
4. **Tools** — 9 seeded study tools + user-saved. Filter: All / My Saved / Apps / Web Tools / Simulators / Posters. Card/list view toggle. "+ Add a Tool" modal saves to localStorage. Card titles are tappable links when a URL is present. Tools can have an optional `tip` field that renders italic/muted below the description.

**Settings modal** (gear icon): LLM provider (Anthropic/OpenAI/custom), model dropdown, API key (localStorage only).

**Mobile nav**: On phones (≤600px) the top nav hides and a fixed bottom tab bar appears with inline SVG icon + label for each tab: graduation cap (Advisor), open book (Library), screen+play (Videos), wrench (Tools). Icons use `currentColor` so they inherit active/inactive states. Desktop and iPad use the top nav unchanged.

## Key data

- `FAA_DOCS` — 6 resources: PHAK, AFH, AIM, FAR, ACS, Wx Handbook
- `BOOKS` — 6 books: Stick & Rudder, Killing Zone, Rod Machado, Say Again, Fate Is Hunter, Weather Flying
- `VIDEOS_SEED` — 8 seeded videos (IDs verified June 2026)
- `TOOLS_SEED` — 9 seeded tools: Sporty's, Chairfly Dash, Chairfly Posters, King Schools, Gleim, ForeFlight, MSFS, Infinite Flight, SkyVector
- `TOOL_TYPE_LABEL` — maps type keys (`app`, `web`, `sim`, `poster`) to display labels
- `TOPICS` — 15 advisor topics including Aircraft Systems, Night Flying, Checkride Prep, etc. Each rec has an optional `url` field; FAA docs link to specific chapter PDFs or AIM HTML pages, FAR links to eCFR sections, books have no `url` and fall back to library card jump.
- `PHAK` / `AFH` / `ACS_PDF` / `WX_PDF` — URL prefix/path constants used in TOPICS to build chapter PDF links
- `SOURCE_TO_CARD` — maps advisor source labels to library card IDs; used as fallback when a rec has no `url` (books only)

## AI advisor

Student context: ~4–5 hours flight time, Cessna 172, training at KUZA (Rock Hill, SC).

Supports Anthropic (messages API) and OpenAI-compatible (chat completions API). Key stored in localStorage, never in code.

## URL routing

Hash-based: `#advisor`, `#library`, `#videos`, `#tools`. Default is `#advisor`.
