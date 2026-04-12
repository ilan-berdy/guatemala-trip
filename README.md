# Trip planners (monorepo)

Static trip dashboards for **Guatemala** and **Woodstock**, deployed to **GitHub Pages** from `main`.

**Live site (repo `ilan-berdy/trips`):** [https://ilan-berdy.github.io/trips/](https://ilan-berdy.github.io/trips/) — hub with links to each trip. The first path segment is the **GitHub repo name** (here `trips`), not the trip folder names.

| Path | App |
|------|-----|
| `/` | Hub (links to each trip) |
| `/guatemala/` | Guatemala planner |
| `/woodstock/` | Woodstock planner |
| `/shared/` | Reusable snippets and docs (see `shared/README.md`) |

## What you need to do

### 1. Commit and push

From this repo:

```bash
git add -A
git status   # should show guatemala/, woodstock/, index.html, shared/, .github/
git commit -m "Restructure as trips monorepo with hub and Woodstock"
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
- **Woodstock**:  
  `https://ilan-berdy.github.io/trips/woodstock/`

If you used to open the Guatemala app at the **repo root URL**, that URL is now the **hub**. Update bookmarks to `/guatemala/`.

### 3. `GEMINI_API_KEY` secret

**Settings → Secrets and variables → Actions** → ensure **`GEMINI_API_KEY`** exists (same as before). The workflow injects it into `guatemala/index.html` and `woodstock/index.html` at deploy time.

### 4. Per-trip passwords (simple gate)

Each planner compares the typed password to a value **injected at deploy time** from GitHub Actions secrets (plain text in the built HTML after deploy—fine for casual privacy, not for high-security data).

Add two repository secrets:

| Secret | Purpose |
|--------|---------|
| `SITE_PASSWORD_GUATEMALA` | Guatemala planner login |
| `SITE_PASSWORD_WOODSTOCK` | Woodstock planner login |

**GitHub:** Settings → Secrets and variables → Actions → New repository secret.

**CLI (optional):** `gh secret set SITE_PASSWORD_GUATEMALA` and paste the value when prompted (same for Woodstock).

**Local preview:** Placeholders in `index.html` are not valid JSON until CI runs, so the gate **does not block** when you open files from disk.

**Limitations:** Client-side only; anyone with the deployed page can inspect it. Use strong, unique secrets if that matters to you.

### 5. Changing the site URL (`github.io/.../something/`)

The segment right after `github.io/<username>/` is the **repository name**. To get URLs like `.../trips/woodstock/`, name the repo **`trips`** (this repo is already `ilan-berdy/trips`). Rename anytime under **Settings → General → Repository name**, or locally: `gh repo rename <new-name>`. GitHub usually redirects old repo URLs; update bookmarks to the new Pages URL.

### 6. Standalone Woodstock folder (optional)

The separate `Woodstock` project folder on disk was **copied** into `woodstock/` here. You can archive or delete that folder locally once you are happy, or keep it and periodically sync—this repo is the source of truth for hosting.

## New trip checklist

1. Copy `guatemala/` or `woodstock/` to `your-trip/`.
2. Change `STORAGE_KEY`, Supabase `upsert` `id`, titles, and `manifest.json`.
3. Add a card to root `index.html`.
4. Extend the deploy step in `.github/workflows/static.yml`: `sed` for `__GEMINI_API_KEY__`, and add a `json.dumps` inject for a new `__TRIP_PW_JSON_*__` placeholder plus a `SITE_PASSWORD_*` secret (and a unique `COOKIE_NAME` in that trip’s HTML).
