# Trip planners (monorepo)

Static trip dashboards for **Guatemala** and **Woodstock**, deployed to **GitHub Pages** from `main`.

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

- **Guatemala** (bookmark this):  
  `https://<your-username>.github.io/<repo-name>/guatemala/`
- **Woodstock**:  
  `https://<your-username>.github.io/<repo-name>/woodstock/`

If you used to open the Guatemala app at the **repo root URL**, that URL is now the **hub**. Update bookmarks to `/guatemala/`.

### 3. `GEMINI_API_KEY` secret

**Settings → Secrets and variables → Actions** → ensure **`GEMINI_API_KEY`** exists (same as before). The workflow injects it into `guatemala/index.html` and `woodstock/index.html` at deploy time.

### 4. Optional: rename the GitHub repo

If you want the URL to say `trips` instead of `Guat`, rename under **Settings → General → Repository name**. Pages URL updates automatically.

### 5. Standalone Woodstock folder (optional)

The separate `Woodstock` project folder on disk was **copied** into `woodstock/` here. You can archive or delete that folder locally once you are happy, or keep it and periodically sync—this repo is the source of truth for hosting.

## New trip checklist

1. Copy `guatemala/` or `woodstock/` to `your-trip/`.
2. Change `STORAGE_KEY`, Supabase `upsert` `id`, titles, and `manifest.json`.
3. Add a card to root `index.html`.
4. Extend the “Inject API key” step in `.github/workflows/static.yml` with `your-trip/index.html` if that app uses `__GEMINI_API_KEY__`.
