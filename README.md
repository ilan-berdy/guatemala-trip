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

### 4. Per-trip passwords (client-side gate)

Each planner asks for a **separate** password. The **SHA-256 hash** of each password is stored in GitHub Actions secrets (not the plain password). The workflow substitutes the placeholder `__SITE_PASS_HASH__` in each `index.html` on deploy.

Add two repository secrets:

| Secret | Used for |
|--------|-----------|
| `PASSWORD_HASH_GUATEMALA` | 64-character hex string = SHA-256 of the Guatemala password |
| `PASSWORD_HASH_WOODSTOCK` | 64-character hex string = SHA-256 of the Woodstock password |

Generate a hash (pick a strong password, then):

```bash
node -e "console.log(require('crypto').createHash('sha256').update('YOUR_PASSWORD_HERE', 'utf8').digest('hex'))"
```

Use **different** passwords for the two trips if you want them independent.

**Local preview:** Opening `index.html` from disk without CI leaves `__SITE_PASS_HASH__` in place; the gate **turns itself off** so you can develop without a password. After push, the deployed site uses the hashes from secrets.

**Limitations:** This is **obscurity, not bank-grade security**—anyone can read the page source and brute-force weak passwords against the hash. It keeps casual visitors out. For sensitive data, do not rely on this alone.

### 5. Optional: rename the GitHub repo

If you want the URL to say `trips` instead of `Guat`, rename under **Settings → General → Repository name**. Pages URL updates automatically.

### 6. Standalone Woodstock folder (optional)

The separate `Woodstock` project folder on disk was **copied** into `woodstock/` here. You can archive or delete that folder locally once you are happy, or keep it and periodically sync—this repo is the source of truth for hosting.

## New trip checklist

1. Copy `guatemala/` or `woodstock/` to `your-trip/`.
2. Change `STORAGE_KEY`, Supabase `upsert` `id`, titles, and `manifest.json`.
3. Add a card to root `index.html`.
4. Extend the deploy step in `.github/workflows/static.yml`: `sed` for `__GEMINI_API_KEY__` and, if you use the same gate pattern, `__SITE_PASS_HASH__` with a new secret (and a unique `COOKIE_NAME` in that trip’s HTML).
