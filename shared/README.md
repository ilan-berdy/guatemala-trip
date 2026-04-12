# Shared layer (`shared/`)

Use this folder for **code and docs you want to reuse** across trip planners.

## Ideas

- **`js/`** — ES modules copied or imported by trip apps (e.g. Supabase helpers, date formatting). Reference from a trip with  
  `<script type="module" src="../shared/js/example.js"></script>`  
  (paths are relative to the trip’s `index.html`.)
- **`css/`** — Design tokens (`tokens.css`) that trips load before their own overrides.
- **`templates/`** — Snippets or a minimal `index.partial.html` to copy when starting a new trip.

## Conventions

1. **Per-trip isolation** — Each trip keeps its own `index.html`, `manifest.json`, and Supabase row id / `localStorage` keys so data never collides.
2. **Copy vs import** — Until everything is modular, it’s fine to **copy** a pattern from `guatemala/` or `woodstock/` into a new trip, then **extract** duplicated pieces into `shared/` when you notice repetition.
3. **No secrets in `shared/`** — API keys stay in GitHub Actions inject (`__GEMINI_API_KEY__` placeholder) or user localStorage.

## Adding a new trip

1. Duplicate `guatemala/` or `woodstock/` to `your-trip/`.
2. Rename titles, `STORAGE_KEY`, Supabase `id` in `saveState`, and `manifest.json` (`name`, `start_url`: use `"./"`).
3. Link the trip from the root `index.html` hub page.
