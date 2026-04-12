# Trip planners (monorepo)

Static trip dashboards for **Guatemala** and a **reusable weekend-trip planner** (`trip/`), deployed to **GitHub Pages** from `main`.

**Live site (repo `ilan-berdy/trips`):** [https://ilan-berdy.github.io/trips/](https://ilan-berdy.github.io/trips/) — hub with links to each trip. The first path segment is the **GitHub repo name** (here `trips`), not necessarily the folder names.

| Path | App |
|------|-----|
| `/` | Hub (links to each trip) |
| `/guatemala/` | Guatemala planner |
| `/trip/` | General trip planner (Catskills / Woodstock 2026 sample data + `TRIP_APP` config) |
| `/woodstock/` | **Redirect** to `/trip/` (keeps old bookmarks working) |
| `/shared/` | Reusable snippets and docs (see `shared/README.md`) |

## What you need to do

### 1. Commit and push

From this repo:

```bash
git add -A
git status   # should show guatemala/, trip/, woodstock/, index.html, shared/, .github/
git commit -m "Your message"
git push origin main
```

### 2. GitHub Pages

- Repo **Settings → Pages**: source **GitHub Actions** (not “Deploy from branch” only).
- After the workflow runs, the site is:

  `https://<your-username>.github.io/<repo-name>/`

  Example with this repo named **`trips`**:  
  `https://ilan-berdy.github.io/trips/`

- **Guatemala** (bookmark this):  
  `https://ilan-berdy.github.io/trips/guatemala/`
- **Trip planner (canonical URL):**  
  `https://ilan-berdy.github.io/trips/trip/`

If you used to open the Guatemala app at the **repo root URL**, that URL is now the **hub**. Update bookmarks to `/guatemala/`.

### 3. `GEMINI_API_KEY` secret

**Settings → Secrets and variables → Actions** → ensure **`GEMINI_API_KEY`** exists. The workflow injects it into `guatemala/index.html` and `trip/index.html` at deploy time.

### 4. Per-trip passwords (simple gate)

Each planner compares the typed password to a value **injected at deploy time** from GitHub Actions secrets (plain text in the built HTML after deploy—fine for casual privacy, not for high-security data).

| Secret | Purpose |
|--------|---------|
| `SITE_PASSWORD_GUATEMALA` | Guatemala planner login |
| `SITE_PASSWORD_TRIP` | Trip planner (`trip/`) login — **preferred** |
| `SITE_PASSWORD_WOODSTOCK` | **Optional legacy fallback** — if `SITE_PASSWORD_TRIP` is unset, the workflow uses this so older setups keep working |

**GitHub:** Settings → Secrets and variables → Actions → New repository secret.

**CLI (optional):** `gh secret set SITE_PASSWORD_TRIP` and paste the value when prompted.

**Local preview:** Placeholders in `index.html` are not valid JSON until CI runs, so the gate **does not block** when you open files from disk.

**Limitations:** Client-side only; anyone with the deployed page can inspect it. Use strong, unique secrets if that matters to you.

### 5. Changing the site URL (`github.io/.../something/`)

The segment right after `github.io/<username>/` is the **repository name**. To get URLs like `.../trips/trip/`, name the repo **`trips`** (this repo is already `ilan-berdy/trips`). Rename anytime under **Settings → General → Repository name**, or locally: `gh repo rename <new-name>`. GitHub usually redirects old repo URLs; update bookmarks to the new Pages URL.

### 6. Standalone `Woodstock` folder on disk vs `trip/` in this repo

**GitHub Pages builds from `trip/` in this repository.** A separate folder on disk (e.g. `…/Projects/Woodstock`) does **not** deploy until you copy changes into **`trip/`** here. This repo is the source of truth for hosting.

### 7. Forking `trip/` for another destination

1. Copy `trip/` to `your-trip/`.
2. Edit **`TRIP_APP`** at the top of `your-trip/index.html` (`storageKey`, `supabaseRowId`, `brandShort`, `tripStartISO`) and replace **`DEFAULT_*`** sample data (itinerary, stays, food, refs).
3. Use unique `STORAGE_KEY`-style values so localStorage does not collide with other trips on the same origin.
4. Add a hub card in root `index.html` and extend `.github/workflows/static.yml`: `sed` for `__GEMINI_API_KEY__`, and a password inject (copy the `trip/index.html` pattern with a new `__TRIP_PW_JSON_*__` placeholder and secret).

## New trip checklist (short)

1. Copy `trip/` to `your-trip/`.
2. Change `TRIP_APP` + `DEFAULT_*` blocks + `manifest.json`.
3. Add a card to root `index.html`.
4. Extend the deploy workflow: Gemini `sed` + JSON password inject + GitHub secret.
