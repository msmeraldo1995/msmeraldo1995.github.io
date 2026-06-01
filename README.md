# portfolio-site

A personal portfolio + architecture site built with **Hugo** and the **Docsy**
theme, built and hosted by **GitHub Pages**.

- Theme is installed as a **git submodule** (no Go required).
- A GitHub Actions workflow builds the site and deploys it on every push to `main`.

---

## 1. Prerequisites (one-time, on Windows)

You need three tools. Install whichever you don't have:

| Tool | Install (PowerShell, using winget) | Verify |
| --- | --- | --- |
| **Git** | `winget install Git.Git` | `git --version` |
| **Hugo (Extended!)** | `winget install Hugo.Hugo.Extended` | `hugo version` — output must contain `extended` |
| **Node.js (≥ 24)** | `winget install OpenJS.NodeJS` | `node --version` — must be **24 or newer** |

> The **extended** Hugo build is mandatory — Docsy compiles SCSS and the
> standard Hugo build can't. If `hugo version` doesn't say `extended`, reinstall
> the extended package. Docsy v0.15.0 also requires Hugo **>= 0.146.0** and
> **Node >= 24** (its PostCSS pipeline uses Node-24 features; older Node fails
> with `node: bad option: --permission`). The CI workflow already pins Node 24.

After installing, **close and reopen your terminal** so PATH updates.

## 2. Create the GitHub repository

Two naming choices — pick one:

- **Project site** (recommended to start): name the repo anything, e.g.
  `portfolio`. Your site will live at `https://USERNAME.github.io/portfolio/`.
- **User site** (cleaner URL): name the repo exactly `USERNAME.github.io`. Your
  site lives at `https://USERNAME.github.io/`.

Create an **empty** repo on GitHub (no README), then come back here.

## 3. Wire up this folder and pull in the Docsy theme

> **Already done in this repo.** The submodule is added and **pinned to
> `v0.15.0`**. If you cloned this repo, just run:
> `git clone --recurse-submodules <repo-url>` (or
> `git submodule update --init` if you forgot `--recurse-submodules`), then skip
> to step 4.

The steps below are how it was set up from scratch — keep them for reference:

```powershell
git init -b main
git remote add origin https://github.com/msmeraldo1995/portfolio.git

# Add Docsy, then PIN to a released version.
# IMPORTANT: do NOT track Docsy's `main` branch — as of 2026 it uses a
# module-only "monorepo" layout (theme files under themes/docsy/theme/) that
# breaks the classic `theme = ["docsy"]` submodule setup. The v0.15.0 tag has
# the classic root layout this project expects.
git submodule add https://github.com/google/docsy.git themes/docsy
git -C themes/docsy checkout v0.15.0
git add themes/docsy          # record the pinned commit
```

## 4. Install dependencies

```powershell
# Docsy's frontend dependencies (Bootstrap, Font Awesome).
# This also runs Docsy's postinstall, which creates an empty `themes/github.com`
# placeholder — that is REQUIRED for Hugo to resolve modules without Go.
# Do not delete it; it's gitignored and regenerated here.
npm run prepare:docsy        # = cd themes/docsy && npm install

# This project's PostCSS tooling (autoprefixer).
npm install
```

## 5. Run it locally

```powershell
npm run serve      # = hugo server --baseURL http://localhost:1313/
```

Open <http://localhost:1313/>. Edits to content and config hot-reload.

> This is a GitHub Pages **user site** (repo `msmeraldo1995.github.io`), served
> at the domain root — `https://msmeraldo1995.github.io/`, no `/portfolio/`
> prefix. `baseURL` reflects that, and `canonifyURLs = true` (in `hugo.toml`)
> ensures links written in Markdown like `/projects/` resolve correctly.

## 6. Fill in your details

Search the repo for `YOUR-USERNAME`, `YOUR-PROFILE`, and `you@example.com` and
replace them. The files that matter:

- `config/_default/hugo.toml` — `baseURL`, site `title`.
- `config/_default/params.toml` — `github_repo`, the LinkedIn/Email/GitHub links.
- `config/_default/menus.toml` — the GitHub link in the navbar.
- `content/en/about/_index.md` — your bio.
- `content/en/projects/` — add your projects (copy `analytics-engine.md`).

**Hero image (optional):** the homepage uses a gradient background out of the
box. To use a photo instead, drop a wide JPG at
`content/en/featured-background.jpg` — Docsy picks it up automatically.

## 7. Push and turn on Pages

```powershell
git add .
git commit -m "Initial portfolio site"
git push -u origin main
```

Then, on GitHub:

1. Go to the repo → **Settings** → **Pages**.
2. Under **Build and deployment** → **Source**, choose **GitHub Actions**.

That's it. The workflow in `.github/workflows/deploy.yml` runs on the push you
just made, builds the site, and deploys it. Watch progress under the repo's
**Actions** tab. The live URL appears in the workflow's `deploy` step and under
Settings → Pages.

> The CI build sets the correct `baseURL` automatically, so you don't need to
> get it perfect in `hugo.toml` — that value only affects local `hugo server`.

## 8. Day-to-day

- **Add a project:** copy `content/en/projects/analytics-engine.md`, rename, edit.
- **Add a deep-dive:** drop a new `.md` in `content/en/docs/` with a `weight:` in
  its frontmatter to order it in the left sidebar.
- **Publish:** `git commit` + `git push`. CI redeploys in ~1–2 minutes.

## Optional: custom domain

1. Add a `static/CNAME` file containing your domain (e.g. `mark.dev`).
2. At your DNS provider, point the domain at GitHub Pages
   ([GitHub's DNS instructions](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)).
3. Set the custom domain under Settings → Pages.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| `hugo server` errors about SCSS / `TOCSS` | You have the non-extended Hugo. Install **Hugo Extended**. |
| Build fails on `PostCSS`/`autoprefixer not found` | Run `npm install` in the project root (step 4). |
| `Error: failed to load modules: module "github.com/FortAwesome/Font-Awesome" not found` | The `themes/github.com` placeholder is missing. Re-run `npm run prepare:docsy`. Never delete that folder manually. |
| `template for shortcode "blocks/cover" not found` | The Docsy submodule drifted onto `main`. Re-pin it: `git -C themes/docsy checkout v0.15.0`. |
| `POSTCSS: ... node: bad option: --permission` | Node is older than 24. Docsy v0.15.0 needs **Node ≥ 24**. Upgrade Node (CI is already pinned to 24 in `deploy.yml`). |
| `failed to load Git data: ...does not have any commits yet` | `enableGitInfo` needs history. Make your first commit (step 7); it then works locally and in CI. |
| 404 on `http://localhost:1313/projects/...` (bare path) | Expected for a project site. Use `npm run serve` (serves at root) **or** browse to `http://localhost:1313/portfolio/...`. See the [appendix](#appendix-project-site-vs-user-site). |
| Theme folder is empty after clone on another machine | `git submodule update --init` |
| CI fails downloading Hugo | Bump `HUGO_VERSION` in `deploy.yml` to a current [release](https://github.com/gohugoio/hugo/releases). |
| Styles look unstyled on the live site | `baseURL` mismatch — but CI sets it for you; make sure Pages **Source** is **GitHub Actions**, not "Deploy from a branch". |

## Why this stack

It mirrors the Docsy-based publishing stack used in production for the
[analytics engine](content/en/projects/analytics-engine.md) case study — so the
site is itself a demonstration of the skill it documents.

## Appendix: user site (root URL)

This site is configured as a GitHub Pages **user site**, served at the domain
root:

| | **User site** (this repo) | Project site (alternative) |
| --- | --- | --- |
| Repo name | `msmeraldo1995.github.io` (exact) | any name, e.g. `portfolio` |
| Live URL | `https://msmeraldo1995.github.io/` | `…github.io/<repo>/` |
| `baseURL` | `https://msmeraldo1995.github.io/` | `…/<repo>/` |
| Limit | **one** per account | many per account |

The repo **must** be named exactly `msmeraldo1995.github.io` for the root URL to
work. The CI workflow computes the correct base URL automatically from the repo
name, and `canonifyURLs = true` keeps in-content links correct either way.
