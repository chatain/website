# Outstanding Work

Status notes for olivierchatain.com. Companion to `../CLAUDE.md` (which documents how the
project is structured and built). The live site is **https://www.olivierchatain.com**,
built from this directory (`github.com/chatain/website`, branch `main`, auto-deployed to
GitHub Pages on push).

_Last updated: 2026-06-17._

---

## Outstanding

### 1. Hugo / theme upgrade — DEFERRED (decided to hold)
The site runs on an old, pinned toolchain and cannot build on modern Hugo without a real migration.

- **Current pinned versions:** Hugo `0.137.1` (`.github/workflows/hugo.yml`) and `0.136.5`
  (`hugoblox.yaml`, `netlify.toml`); theme module `blox-tailwind v0.3.1` (`go.mod`).
- **Why it's stuck:** `blox-tailwind v0.3.1` fails on recent Hugo (`GetTerms` API error). The
  fix is bumping the theme to `v0.10.0`, but that version adds a **hard dependency on the
  TailwindCSS CLI (needs Node.js)** at build time, which the current CI workflow does not install.
- **What a full upgrade requires:**
  - Bump `blox-tailwind` (→ v0.10.0) in `go.mod` via `hugo mod get -u ./...`.
  - Rewrite `.github/workflows/hugo.yml`: newer Hugo version + install Node.js + TailwindCSS CLI.
  - Update `netlify.toml` / `hugoblox.yaml` versions to match.
  - Fix the content/config deprecations in item 2 below.
  - **Visually review** the re-themed site (major 0.3 → 0.10 jump changes rendering) before trusting
    it on the live domain. Note: Node.js is **not** installed on the local machine, so the upgraded
    site can't currently be built/previewed locally.

### 2. Content & config deprecation warnings (surface only on newer Hugo; harmless on 0.137.1)
These are warnings on the upgraded toolchain — clean them up as part of (or before) the upgrade:
- Top-level `doi:` in publications → `hugoblox: { ids: { doi: ... } }` (most `content/publication/*`).
- `url_pdf` / `url_code` / `url_slides` / `url_video` → `links: [{type: ..., url: ...}]`
  (working papers, `event/example`, `capron-2008`).
- `callout` shortcode → standard Markdown alerts (`> [!NOTE]`) in `post/second-brain`, `event/example`.
- Header `blox: navbar` → `block: navbar` in `config/_default/params.yaml`.
- `module.mounts.includeFiles` → `files` in `config/_default/module.yaml`.
- `_build:` → `build:` in `content/authors/_index.md` (only valid on Hugo 0.145+).
- `languageCode` → `locale` in `config/_default/languages.yaml` (only valid on Hugo 0.158+).
- `markup.goldmark.renderHooks.link.enableDefault` deprecation (comes from the theme module;
  resolved by the theme upgrade).

### 3. Hero background image quality
`content/_index.md` now uses `assets/media/DSCF5053.jpeg` (~23 KB) for the full-screen homepage
background — small/soft for that use. Consider replacing with a properly web-optimized export
(~200–400 KB JPEG or WebP at full resolution). The oversized `.tiff` original was removed.

### 4. CV cache-busting (minor)
The CV is `static/uploads/Chatain CV 2026 06 17.pdf`, linked from the homepage Download CV button.
CV updates so far reuse the same filename → unchanged URL → browsers/CDN may serve a stale cached
copy. Optional: use date-versioned filenames on each update so the URL changes.

### 5. Repo hygiene (optional)
- `public/`, `resources/_gen/`, and `hugo_stats.json` (build output/cache) are **tracked in git**.
  The GitHub Pages workflow rebuilds `public/` fresh on every deploy, so committing it is just noise.
  Consider adding a `.gitignore` for these and removing them from version control.
- The `Website/` parent folder holds several dated snapshots (`OC241107`, `OC241113`, `OC250924`,
  `OC251010`). Only `OC251010/website/` is live; the older ones can be archived/removed when convenient.

---

## Recently completed (2026-06-17)
- Fixed broken Download CV link; added current CV (`Chatain CV 2026 06 17.pdf`).
- Replaced the overflowing "OCh" favicon with a clean emerald "OC" (`assets/media/icon.png`).
- Set `baseURL` to the live custom domain `https://www.olivierchatain.com/`.
- Switched homepage hero from a 1.9 MB `.tiff` to a web-friendly `.jpeg`.
- Removed leftover Hugo Blox template content (placeholder awards/skills/hobbies, orphaned `projects.md`).
- Updated Chatain & Plaksenkova (2026) SMJ publication — final citation (47(1), 228–256),
  marked featured so it leads the publication lists.
- Updated the About Me bio (IRSEM associate, co-leads the Business and Geopolitics Lab).
