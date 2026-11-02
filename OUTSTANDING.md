# Outstanding Work

Status notes for olivierchatain.com. Companion to `../CLAUDE.md` (which documents how the
project is structured and built). The live site is **https://olivierchatain.com** (no www; `www.` redirects there, HTTPS enforced),
built from this directory (`github.com/chatain/website`, branch `main`, auto-deployed to
GitHub Pages on push).

_Last updated: 2026-11-02._

---

## Outstanding

### 0. Theme is stranded at an abandoned module path; Hugo pinned at 0.162.1 until migrated
Upstream renamed `HugoBlox/hugo-blox-builder` → **`HugoBlox/kit`** (Jan 2026) and restructured the
modules: `modules/blox-tailwind` → `modules/blox`, `modules/blox-plugin-netlify` →
`modules/integrations/netlify` (untagged pseudo-versions; latest May 2026). Our `go.mod` paths will
never update again. The current theme **does not build on Hugo 0.168** (`includeFiles` removed;
`tailwindcss` not whitelisted in `security.exec.allow`), and upstream's own academic-cv starter
still pins Hugo 0.162.0 — so **stay on Hugo 0.162.1** (CI + local `~/.local/bin/hugo-0.162.1`) until
a migration to `HugoBlox/kit`. When migrating / bumping Hugo past 0.163, also:
- replace `imaging.quality: 80` in `config/_default/hugo.yaml` with `imaging.jpeg.quality` /
  `imaging.webp.quality` (the new keys don't exist in 0.162.1, so changing early would silently drop
  quality to the default);
- re-diff the `card.html` override (item 1).

### 1. Theme override to re-sync on future theme upgrades
`layouts/_partials/views/card.html` is a **vendored copy** of the theme's card view, modified to
render the image section only when a paper has a featured image (suppresses the empty placeholder).
On a future `blox-tailwind` upgrade, re-diff this against the new upstream `card.html` so it doesn't
drift. The other override is `layouts/partials/hooks/head-end/github-button.html`.

### 2. Theme-internal deprecation warnings (informational — not fixable here)
The production build emits 4 deprecation warnings that originate **inside the theme module**
(`blox-tailwind v0.10.0`), not our config/content, so they can only be cleared by a future theme
release: `module.mounts.includeFiles`, `.Site.LanguageCode`, `.Site.Data`, `.Site.AllPages`.
All are harmless and the build succeeds.

---

## Standing practices

- **Paper PDFs go in `static/uploads/`** (never `public/` — it is build output, wiped and regenerated
  on every build and git-ignored). Link from front matter as `links: [{type: pdf, url: uploads/<file>.pdf}]`.
- **CV updates** use a **date-versioned filename** so the URL changes and stale cached copies are
  never served. To update: drop the new PDF in `static/uploads/` named with that day's date
  (`Chatain CV YYYY MM DD.pdf`), point the Download CV link in `content/_index.md` at it, and
  `git rm` the old file. Current: `static/uploads/Chatain CV 2026 06 30.pdf`.
- **Hero background** is `assets/media/hero-clouds.jpg`. To re-crop, work from the archived full-res
  source `../../images/hero-clouds-source-pexels-17785314.jpg` (4000×3000); export a ~2560px-wide
  JPEG and bake in any darkening (the landing page's `parse_block_v3` ignores the
  `design.filters.brightness` config knob, so brightness must live in the file).

---

## Recently completed

### 2026-11-02 — Restored 4 missing paper PDFs (all were 404 on the live site)
- The working-paper PDFs (Cappelli–Chatain biform, Chatain–Plaksenkova ecosystem, Chatain–Kaul
  political externalities) and Capron–Chatain 2008 AMR had only ever lived in `public/uploads/`
  (per a wrong template comment), so they vanished once deploys built from a clean checkout.
  Recovered from `_archive/` (OC250924 snapshot) into `static/uploads/`; fixed the comment.

### 2026-11-02 — Canonical domain + HTTPS
- Turned on **Enforce HTTPS** in the Pages settings (cert covers apex + www). Before this,
  `http://` served the site unencrypted and CI baked `http://olivierchatain.com/` into canonical
  tags and the sitemap.
- Standardised on **`olivierchatain.com` (no www)** — matching the existing Pages custom-domain
  setting and what search engines had indexed. Updated `CNAME` and `baseURL` to match.

### 2026-11-02 — Version audit
- Bumped GitHub Actions off the deprecated Node 20 runtime: `checkout` v4→v7, `configure-pages`
  v5→v6, `setup-node` v4→v7, `upload-pages-artifact` v3→v5 (now skips dotfiles — `public/` has none
  that matter), `deploy-pages` v4→v5.
- `@tailwindcss/cli` 4.3.1 → 4.3.3 (output verified identical apart from trivial CSS normalisation).
- Installed pinned Hugo 0.162.1 locally at `~/.local/bin/hugo-0.162.1` (Homebrew `hugo` is 0.168 and
  can't build the theme). See Outstanding item 0.

### 2026-07-01
- **Replaced the hero background** with a high-res, rights-free storm-cloud image
  (`assets/media/hero-clouds.jpg`, 2560×1024, cropped 2.5:1 from a 4000×3000 Pexels photo — free
  for commercial use, no attribution). Fixes the prior 1280×504 upscaling softness. Darkened
  (brightness 0.7, contrast 1.05, baked in) so white name/title text stays readable (top-third mean
  brightness ≈ 36/255). Removed the unused old `DSCF5053.jpeg`; full-res source archived in `images/`.
- **Updated the CV** to `Chatain CV 2026 06 30.pdf` (date-versioned, per standing practice above).
- **Updated the About Me bio** (`content/authors/admin/_index.md`) — dropped the
  "co-leads the Business and Geopolitics Lab" sentence.
- **Archived old snapshots** — compressed `OC241107`, `OC241113`, `OC250924` into
  `../../_archive/old-snapshots-2026-07-01.tar.gz` (92M, integrity-verified) and removed the
  originals. Only `OC251010/website/` remains live in the `Website/` parent folder.

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
- Suppressed the empty featured-image placeholder via the `card.html` override.
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
