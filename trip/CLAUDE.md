# `trip/` planner — AI / contributor notes (Woodstock 2026 sample instance)

## Images: real places only (not generic stock)

- **Do not** paste “Woodstock NY” into Unsplash/Pexels/Pixabay and grab the first hit. That is decorative stock, not a picture of *your* restaurant, trail, or listing.
- **Do** use:
  - **Airbnb / VRBO** listing image URLs (paste flow / Jina) — photos of the actual rental.
  - **Official venue sites** — `og:image` / hero assets from the restaurant, inn, or hotel domain (Squarespace, WordPress, Wix, etc.).
  - **Documentary photos of named public places** — e.g. Wikimedia Commons for Overlook Mountain, Kaaterskill Falls, or a specific street address, with a stable `upload.wikimedia.org` URL and a clear caption of what it depicts.
  - **User photos** — URLs they provide.
- The app blocks known stock/placeholder hosts on venue and inspiration cards, strips them on every load, then reapplies bundled **canonical** image URLs from `DEFAULT_REFS` / `DEFAULT_FOOD` when the card **title** matches the shipped defaults (rename the card if you need a different photo).

## Stack

- Single-page app: `index.html` (inline CSS/JS), Supabase for optional cloud sync, Jina Reader for Airbnb enrichment when Microlink is blocked.
