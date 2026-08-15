# fjzs.github.io

Personal website for Francisco Zenteno Smith. Live at https://fjzs.github.io/.

## Deployment

This site is deployed via **GitHub Pages**, configured to serve directly from the `master` branch:

- **Source:** `master` branch, root (`/`) — see repo Settings → Pages.
- **Trigger:** every push to `master` triggers an automatic rebuild and deploy. No CI workflow, no build step — GitHub Pages serves the static files as-is.
- **Live in:** typically 10–60 seconds after a push.
- **`.nojekyll`:** this file tells GitHub Pages to skip its default Jekyll processing, since the site is plain static HTML/CSS/JS, not a Jekyll project.

So: edit `index.html` / `sefop.html`, commit, push to `master` — the live site updates on its own. No manual deploy step needed.

### Changing the Pages source branch

If the Pages source branch is ever changed (Settings → Pages → Source), GitHub does **not** automatically rebuild against the new branch's latest commit — the next push will deploy correctly, but to force an immediate rebuild after a source change, trigger one manually:

```
gh api -X POST repos/fjzs/fjzs.github.io/pages/builds
```

## Custom domain

Not yet configured. Plan is to point `www.fjzs.com` at this repo via a `CNAME` file — see `HANDOFF.md` for status.

## Project status / roadmap

See `HANDOFF.md` for the full plan, locked design decisions, and open items. Short version: `index.html` and `sefop.html` are static prototypes; the plan is to port them to an Astro project while keeping the same GitHub Pages deployment model.
