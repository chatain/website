# Outstanding Work

Status notes for olivierchatain.com. Companion to `../CLAUDE.md` (which documents how the
project is structured and built). The live site is **https://www.olivierchatain.com**,
built from this directory (`github.com/chatain/website`, branch `main`, auto-deployed to
GitHub Pages on push).

_Last updated: 2026-06-18._

---

## Outstanding

### 1. Hero background image — resolution ceiling (mitigated, not fully fixed)
The hero photo `assets/media/DSCF5053.jpeg` is only **1280×504** in *every* copy across the repo,
including the `.tiff` "original" (preserved in `../../images/DSCF5053.tiff`) — so it upscales soft
on full-screen displays and that can't be fixed from existing files. As a mitigation it was
**darkened** (brightness 0.6, baked into the image) on 2026-06-18 to mask the softness and boost
white-text contrast — note the landing page renders via `parse_block_v3`, which ignores the
`design.filters.brightness` config knob, hence baking it in.
**A true sharpness fix still requires a higher-resolution original from the camera** (Fujifilm
DSCF originals are ~6000×4000); if Olivier supplies it, crop/optimize to a ~2560px-wide WebP + JPEG.

### 2. CV cache-busting (resolved — keep doing this)
The CV is `static/uploads/Chatain CV 2026 06 19.pdf`, linked from the homepage Download CV button
(`content/_index.md`). As of 2026-06-19 each CV update uses a **date-versioned filename** so the
URL changes and stale cached copies are never served. On the next update: copy in the new PDF with
that day's date in the name, update the link in `content/_index.md`, and `git rm` the old file.

### 3. Repo hygiene — old snapshots
The `Website/` parent folder holds several dated snapshots (`OC241107`, `OC241113`, `OC250924`,
`OC251010`). Only `OC251010/website/` is live; the older ones can be archived/removed when convenient.
(Build output — `public/`, `resources/_gen/`, `hugo_stats.json`, `node_modules/` — is now
git-ignored and no longer tracked, as of the 2026-06-18 upgrade.)

### 4. Theme override to re-sync on future theme upgrades
`layouts/_partials/views/card.html` is a **vendored copy** of the theme's card view, modified to
render the image section only when a paper has a featured image (suppresses the empty placeholder).
On a future `blox-tailwind` upgrade, re-diff this against the new upstream `card.html` so it doesn't
drift. The other override is `layouts/partials/hooks/head-end/github-button.html`.

### 5. Theme-internal deprecation warnings (informational — not fixable here)
The production build emits 4 deprecation warnings that originate **inside the theme module**
(`blox-tailwind v0.10.0`), not our config/content, so they can only be cleared by a future theme
release: `module.mounts.includeFiles`, `.Site.LanguageCode`, `.Site.Data`, `.Site.AllPages`.
All are harmless and the build succeeds.

---

## Recently completed

### 2026-06-18 — Hugo/theme upgrade + publication abstracts
- **Upgraded the toolchain:** Hugo `0.137.1` → `0.162.1`; theme `blox-tailwind v0.3.1` → `v0.10.0`
  (Tailwind CSS v4); netlify plugin `v1.2.0`, analytics `v0.3.0`.
- Added Node/Tailwind build deps (`package.json`: `@tailwindcss/cli`, `@tailwindcss/typography`
  + `package-lock.json`); CI uses **Node 24** (Hugo's Tailwind transformer needs Node ≥ 23.5 for
  the `--permission` flag — Node 20 fails).
- Updated CI/deploy configs (`hugo.yml` + `actions/setup-node`, `hugoblox.yaml`, `netlify.toml`).
- Fixed config deprecations: `includeFiles→files`, navbar `blox→block`, `languageCode→locale`,
  `_build→build`.
- Migrated content front matter: top-level `doi:` → `hugoblox.ids.doi`; `url_pdf:` → `links:`.
- **Added abstracts to all 14 publications + working papers** (Crossref for the journal articles,
  the working-paper PDF, and the bib for the 2026 SMJ paper). They display on each paper's detail
  page (reached by clicking the title in the publication lists).
- Suppressed the empty featured-image placeholder via the `card.html` override (item 4 above).
- Darkened the homepage hero background (item 1 above) to mask upscaling softness / improve contrast.
- Removed leftover Hugo Blox **demo content** (`content/post/*`, `content/event/*`).
- Added `.gitignore`; stopped tracking build output (`public/`, `resources/_gen`, `hugo_stats.json`,
  `node_modules/`, `.DS_Store`).

### 2026-06-17
- Fixed broken Download CV link; added current CV (`Chatain CV 2026 06 17.pdf`).
- Replaced the overflowing "OCh" favicon with a clean emerald "OC" (`assets/media/icon.png`).
- Set `baseURL` to the live custom domain `https://www.olivierchatain.com/`.
- Switched homepage hero from a 1.9 MB `.tiff` to a web-friendly `.jpeg`.
- Removed leftover Hugo Blox template content (placeholder awards/skills/hobbies, orphaned `projects.md`).
- Updated Chatain & Plaksenkova (2026) SMJ publication — final citation (47(1), 228–256),
  marked featured so it leads the publication lists.
- Updated the About Me bio (IRSEM associate, co-leads the Business and Geopolitics Lab).
