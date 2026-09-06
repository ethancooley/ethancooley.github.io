# ethancooley.github.io

My personal portfolio — [ethancooley.github.io](https://ethancooley.github.io)

Not a static resume page. It's built around a concept I call **"The Long Route"** — a career told the way an endurance event is: checkpoints along a route, a live pulse under the surface, and a finish line. Most of the content is hand-authored, but several sections pull real, live data rather than just describing things statically.

## What's actually in here

- **`index.html`** — the main site: hero, about, experience, skills, education, projects, a live ML demo, hobbies, live Strava data, and contact.
- **`photography.html`** — a separate gallery page for personal photography, with a lightbox viewer.
- **`/photos`** — image files for the photography gallery (not committed at full camera resolution — see below).

Everything is plain HTML/CSS/vanilla JS. No build step, no framework, no bundler. It's intentionally simple to deploy (GitHub Pages serves it as-is) and simple to edit (open the file, find the section, change the text).

## Live features (the parts that aren't just static HTML)

A few sections pull real data instead of hardcoded content:

| Feature | What it does | Backed by |
|---|---|---|
| **Now Playing** widget (floating, bottom-left) | Shows my current Spotify track live, with a real-time progress bar | [`portfolio-api`](https://github.com/ethancooley/portfolio-api) — a standalone Vercel serverless project |
| **Recent Activity** section | Last 5 Strava activities, this year's aggregate training totals, and a real GPS route map of my latest outdoor activity (canvas-rendered from an actual polyline) | `Trace/backend` — the same Vercel project that powers [Trace](https://github.com/ethancooley/Trace) |
| **Live Demo** section | An actual sentence-embedding model (`Xenova/all-MiniLM-L6-v2`) downloaded and run **entirely in the visitor's browser** via [transformers.js](https://github.com/xenova/transformers.js) — no backend involved | Client-side only, loaded from a CDN |
| **SoundCloud** panel (inside Beyond Work → Guitar) | Embedded player for my SoundCloud profile | SoundCloud's public oEmbed widget |

None of these are required for the rest of the site to work — if any of them fail (API down, CDN blocked, token expired), they degrade quietly to an idle/empty state rather than breaking the page.

## Extra stuff worth knowing about

- **Command palette** — press `⌘K` / `Ctrl+K` anywhere to jump to any section or external link via fuzzy search.
- **Konami code** — try `↑ ↑ ↓ ↓ ← → ← → B A` on your keyboard.
- Both of the above are quietly hinted at in the browser console if you open dev tools.

## Architecture notes

This repo is intentionally "dumb" — it's just the frontend. The two live integrations depend on separate, small serverless backends that are **not** part of this repo:

- **Spotify** → [`portfolio-api`](https://github.com/ethancooley/portfolio-api) (its own repo, its own Vercel project). Deliberately kept separate from Trace so that repo stays a clean showcase of the Strava integration work, rather than a grab-bag of unrelated site plumbing.
- **Strava** → `Trace/backend`, which already existed to power the [Trace](https://github.com/ethancooley/Trace) dashboard project. The portfolio's Recent Activity section piggybacks on that existing OAuth setup via two endpoints: `/api/recent_activities` and `/api/yearly_stats`.

If either backend's refresh token ever needs regenerating (Spotify or Strava tokens can be revoked), see the README in the respective backend repo for the one-time re-authorization steps.

## Local development

There's no build step — just open `index.html` directly in a browser, or serve the folder locally if you want relative paths and fetches to behave exactly like production:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

Note: the live widgets (Now Playing, Recent Activity, Live Demo) all fetch from real external endpoints, so they'll behave the same locally as in production — there's no separate "dev mode" or mock data.

## Deployment

Hosted on GitHub Pages directly from this repo. Pushing to `main` is enough — there's no build/CI step, since there's nothing to build.

## Adding new photos

Drop resized, compressed images (aim for ~1600–2000px on the long edge, ~75–85% JPEG quality — [Squoosh](https://squoosh.app) works well) into `/photos`, named `photo-01.jpg` through `photo-NN.jpg`. Then add a matching `<figure>` block in `photography.html` with a title and location/year caption. Missing files fall back to a small placeholder automatically, so the gallery never shows a broken image icon.
