# ppl-study

Single-file PPL ground school study organizer. Live at **https://lloydfaulkner.github.io/ppl-study/**

## Architecture

One file: `index.html` — all CSS and JS inline, no build step, no framework. Google Fonts (Inter) is the only external dependency.

Storage: localStorage only (saved videos, LLM settings, theme preference).

## Deployment

Two branches must stay in sync — always push to both:

```bash
git add index.html
git commit -m "description"
git push origin main
cp index.html /tmp/ppl-index.html
git checkout gh-pages
cp /tmp/ppl-index.html index.html
git add index.html
git commit -m "description"
git push origin gh-pages
git checkout main
```

GitHub Pages serves from `gh-pages` branch at `/`.

## Design

- Sectional chart aesthetic: tan/khaki background `#f0ead8`, blue `#004fa8` as primary accent, magenta `#b8005a` sparingly (logo only + pulse animation)
- Night mode: dark slate `#14161a` base, NOT warm brown
- Inter font, map grid overlay on body
- CSS custom properties in `:root` and `[data-theme="night"]`

## Structure

**Three tabs** (Study Advisor is first/default):

1. **Study Advisor** — 18 topic chips → static chapter recommendations → clickable rows jump to Library card with pulse highlight. AI tips button if API key set.
2. **Library** — 6 FAA docs + 6 books. Filter: All / FAA Official / Books. Card/list view toggle. Cards have "What it is" + "When to read it" only (no chapter breakdown — that lives in Study Advisor).
3. **Videos** — 8 seeded YouTube videos + user-saved. Filter: All / My Saved / Weather / Radio / Landing. "+ Save a Video" modal saves to localStorage.

**Settings modal** (gear icon): LLM provider (Anthropic/OpenAI/custom), model dropdown, API key (localStorage only).

## Key data

- `FAA_DOCS` — 6 resources: PHAK, AFH, AIM, FAR, ACS, Wx Handbook
- `BOOKS` — 6 books: Stick & Rudder, Killing Zone, Rod Machado, Say Again, Fate Is Hunter, Weather Flying
- `VIDEOS_SEED` — 8 seeded videos (IDs verified June 2026)
- `TOPICS` — 18 advisor topics including Aircraft Systems, Night Flying, Checkride Prep, etc.
- `SOURCE_TO_CARD` — maps advisor source labels to library card IDs for jump-link behavior

## AI advisor

Student context: ~4–5 hours flight time, Cessna 172, training at KUZA (Rock Hill, SC).

Supports Anthropic (messages API) and OpenAI-compatible (chat completions API). Key stored in localStorage, never in code.

## URL routing

Hash-based: `#advisor`, `#library`, `#videos`. Default is `#advisor`.
