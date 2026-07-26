# ningxu.ai al-folio Portfolio Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the Hugo blog in `/Users/ningxu/workspace/nlog` (repo `nxu96/nxu96.github.io`, domain ningxu.ai) with a minimal al-folio v1.0 academic portfolio, reviewed locally before deploying.

**Architecture:** al-folio v1.0 (Jekyll; layouts live in the `al_folio_core` gem) with a single about page: bio → Selected Projects → selected publications (from `_bibliography/papers.bib`) → social icons. Deployed by GitHub Actions building `_site` and publishing via the Pages artifact flow (the repo's Pages source is already "GitHub Actions" — do not switch it to a `gh-pages` branch).

**Tech Stack:** Jekyll + al-folio v1.0, Homebrew Ruby (system Ruby 2.6 is too old), Node (for jekyll-terser/purgecss), ImageMagick (responsive images), GitHub Pages with custom domain `ningxu.ai`.

**Spec:** `docs/superpowers/specs/2026-07-26-ningxu-ai-portfolio-design.md`

**Pre-verified facts (do not re-derive):**
- al-folio v1.0 tarball: `https://github.com/alshedivat/al-folio/archive/refs/tags/v1.0.tar.gz` (already extracted at `/tmp/alfolio/al-folio-1.0`; re-download if missing).
- InstantNuRec links: project page `https://research.nvidia.com/labs/sil/projects/instant-nurec/`, paper `https://arxiv.org/pdf/2607.14203`, code `https://github.com/nvidia/instant-nurec`, model `https://huggingface.co/nvidia/instant-nurec`, teaser image `https://research.nvidia.com/labs/sil/projects/instant-nurec/assets/teaser-poster.webp`.
- Paper metadata verified via arXiv API (authors listed verbatim in Task 6).
- Profile photo source: `/Users/ningxu/workspace/profile/ningxu_profile_square_newbackground.png` (2675×2675).
- Local tools: Homebrew 5.1.2 present; `ruby` is system 2.6.10; `node`, `magick`/`convert`, `gh` NOT installed; `sips` available.
- GitHub account was renamed `nxu96` → `nxu-robot`; the repo is now `nxu-robot/nxu96.github.io` (verified via API redirect). ningxu.ai is live and fronted by Cloudflare (proxied DNS) → GitHub Pages origin; the custom domain survived the rename. The repo is now a *project* site (name no longer matches the handle), which is fine with a custom domain — `baseurl` stays blank.

**Hard constraints (from spec):** No NVIDIA-internal links/email anywhere on the site. Public email is `nxu@umich.edu`. GitHub is `nxu-robot`. Do not push to `main` or deploy until the user approves the local preview (Task 9 gate).

---

### Task 1: Branch and remove the Hugo site

**Files:**
- Delete: `hugo.yaml`, `content/`, `layouts/`, `archetypes/`, `i18n/`, `data/`, `static/`, `assets/`, `themes/PaperMod` (submodule), `.gitmodules`, `.github/workflows/hugo.yml`, `.hugo_build.lock`, `public/`
- Keep: `CNAME`, `docs/`, `.claude/`, `.gitignore` (replaced in Task 2)

- [ ] **Step 1: Update the remote to the renamed account, then create the working branch**

```bash
cd /Users/ningxu/workspace/nlog
git remote set-url origin git@github.com:nxu-robot/nxu96.github.io.git
git ls-remote origin HEAD   # verify SSH access to the renamed repo
git checkout -b al-folio-site
```

Expected: `git ls-remote` prints a commit hash (confirms push access path works post-rename).

- [ ] **Step 2: Remove Hugo files from git**

```bash
cd /Users/ningxu/workspace/nlog
git rm -r --cached themes/PaperMod
git rm .gitmodules
rm -rf themes
git rm -r hugo.yaml content layouts archetypes i18n data static assets .github/workflows/hugo.yml
rm -rf public .hugo_build.lock resources
```

Note: some of these dirs may be empty/untracked; if `git rm` errors with "did not match any files" for a path, `rm -rf` that path instead and continue.

- [ ] **Step 3: Verify only the keepers remain**

Run: `ls -A /Users/ningxu/workspace/nlog`
Expected: `.claude .git .gitignore CNAME docs` (plus `.DS_Store`)

- [ ] **Step 4: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add -A
git commit -m "Remove Hugo site in preparation for al-folio"
```

---

### Task 2: Import al-folio v1.0

**Files:**
- Create: `_bibliography/`, `_data/`, `_pages/`, `assets/`, `_config.yml`, `Gemfile`, `Gemfile.lock`, `package.json`, `package-lock.json`, `purgecss.config.js`, `robots.txt`, `requirements.txt`, `.gitignore`, `.github/workflows/deploy.yml`
- NOT imported: `_books/`, `_news/`, `_posts/`, `_projects/`, `_teachings/`, `test/`, `docs/`, `lighthouse_results/`, `readme_preview/`, `bin/`, Docker files, `README.md`, `CLAUDE.md`, `AGENTS.md`, all other workflows

- [ ] **Step 1: Ensure the template is extracted**

```bash
[ -d /tmp/alfolio/al-folio-1.0 ] || (mkdir -p /tmp/alfolio && cd /tmp/alfolio && curl -sL https://github.com/alshedivat/al-folio/archive/refs/tags/v1.0.tar.gz | tar xz)
ls /tmp/alfolio/al-folio-1.0/_config.yml
```

Expected: path prints without error.

- [ ] **Step 2: Copy the needed subset into the repo**

```bash
cd /tmp/alfolio/al-folio-1.0
SRC=$(pwd); DST=/Users/ningxu/workspace/nlog
cp -R "$SRC/_bibliography" "$SRC/_data" "$SRC/_pages" "$SRC/assets" "$DST/"
cp "$SRC/_config.yml" "$SRC/Gemfile" "$SRC/Gemfile.lock" "$SRC/package.json" "$SRC/package-lock.json" "$SRC/purgecss.config.js" "$SRC/robots.txt" "$SRC/requirements.txt" "$DST/"
cp "$SRC/.gitignore" "$DST/.gitignore"
mkdir -p "$DST/.github/workflows"
cp "$SRC/.github/workflows/deploy.yml" "$DST/.github/workflows/deploy.yml"
```

- [ ] **Step 3: Verify import**

Run: `cd /Users/ningxu/workspace/nlog && ls _pages/about.md _bibliography/papers.bib _data/socials.yml assets/img .github/workflows/deploy.yml && grep -c 'al_folio_core' _config.yml`
Expected: all paths listed; grep prints `1` (or more).

- [ ] **Step 4: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add -A
git commit -m "Import al-folio v1.0 template (trimmed subset)"
```

---

### Task 3: Trim to a single about page

**Files:**
- Delete: `_pages/about_einstein.md`, `_pages/blog.md`, `_pages/books.md`, `_pages/cv.md`, `_pages/dropdown.md`, `_pages/news.md`, `_pages/plugins.md`, `_pages/profiles.md`, `_pages/projects.md`, `_pages/repositories.md`, `_pages/teaching.md`
- Modify: `_pages/publications.md` (keep the page — the homepage "selected publications" block links to it — but hide from nav)

- [ ] **Step 1: Delete demo pages**

```bash
cd /Users/ningxu/workspace/nlog
git rm _pages/about_einstein.md _pages/blog.md _pages/books.md _pages/cv.md _pages/dropdown.md _pages/news.md _pages/plugins.md _pages/profiles.md _pages/projects.md _pages/repositories.md _pages/teaching.md
```

- [ ] **Step 2: Hide publications page from nav**

In `_pages/publications.md`, change the front matter line `nav: true` to `nav: false`.

- [ ] **Step 3: Verify remaining pages**

Run: `ls /Users/ningxu/workspace/nlog/_pages/`
Expected: `404.md about.md publications.md` only.

- [ ] **Step 4: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add -A
git commit -m "Trim al-folio to about + publications pages"
```

---

### Task 4: Configure site identity (_config.yml, socials.yml)

**Files:**
- Modify: `_config.yml`, `_data/socials.yml`

- [ ] **Step 1: Edit `_config.yml` identity fields**

Make these exact replacements (top-of-file "Site settings" section):

| From | To |
|---|---|
| `first_name: You` | `first_name: Ning` |
| `middle_name: R.` | `middle_name:` |
| `last_name: Name` | `last_name: Xu` |
| `description: > # the ">" symbol means to ignore newlines until "footer_text:"`<br>`  A simple, whitespace theme for academics. Based on [*folio](https://github.com/bogoli/-folio) design.` | `description: > # the ">" symbol means to ignore newlines until "footer_text:"`<br>`  Ning Xu — Senior Deep Learning and Computer Vision Engineer at NVIDIA. Spatial intelligence, 3D reconstruction, and autonomous vehicles.` |
| `keywords: jekyll, jekyll-theme, academic-website, portfolio-website # add your own keywords or leave empty` | `keywords: Ning Xu, computer vision, 3D reconstruction, gaussian splatting, SLAM, autonomous vehicles # add your own keywords or leave empty` |
| `url: https://alshedivat.github.io # the base hostname & protocol for your site` | `url: https://ningxu.ai # the base hostname & protocol for your site` |
| `baseurl: /al-folio # the subpath of your site, e.g. /blog/. Leave blank for root` | `baseurl: # the subpath of your site, e.g. /blog/. Leave blank for root` |
| `footer_text: >` block's `Photos from <a href="https://unsplash.com" target="_blank">Unsplash</a>.` line | delete that one line (keep the Jekyll/al-folio credit lines) |

Also remove the `contact_note:` value (make it `contact_note:` with nothing after, deleting the following indented line `You can even add a little note about which of these is the best way to reach you.`).

- [ ] **Step 2: Edit `_config.yml` jekyll-scholar author bolding**

Around line 343:

```yaml
scholar:
  last_name: [Einstein]
  first_name: [Albert, A.]
```

becomes

```yaml
scholar:
  last_name: [Xu]
  first_name: [Ning, N.]
```

- [ ] **Step 3: Replace `_data/socials.yml` entirely**

```yaml
# this file contains the social media links and usernames of the author
# the socials will be displayed in the order they are defined here
# for more information, please refer to the documentation of jekyll-socials plugin: https://github.com/george-gca/jekyll-socials
email: nxu@umich.edu # your email address
github_username: nxu-robot # your GitHub user name
linkedin_username: nxu # your LinkedIn user name
scholar_userid: 6dhIxEgAAAAJ # your Google Scholar ID
```

(No `cv_pdf`, no `inspirehep_id`, no `rss_icon`, no `custom_social`. If the build later warns that `github_username`/`linkedin_username` are not recognized keys, run `grep -rn 'linkedin\|github' vendor/bundle/ruby/*/gems/jekyll-socials*/` to find the exact key names the plugin expects and rename accordingly — but these are the documented jekyll-socials keys.)

- [ ] **Step 4: Verify no Einstein/demo identity remains in config**

Run: `cd /Users/ningxu/workspace/nlog && grep -n 'Einstein\|You R\|al-folio # the subpath\|alshedivat.github.io' _config.yml _data/socials.yml`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add _config.yml _data/socials.yml
git commit -m "Configure site identity, domain, socials, scholar name"
```

---

### Task 5: Profile photo, teaser image, about page content

**Files:**
- Create: `assets/img/prof_pic.png` (replacing demo prof pics), `assets/img/instant-nurec-teaser.webp`
- Modify: `_pages/about.md` (full replacement below)
- Delete: `assets/img/prof_pic.jpg`

- [ ] **Step 1: Install profile photo (resized to 800px) and teaser image**

```bash
cd /Users/ningxu/workspace/nlog
sips -Z 800 /Users/ningxu/workspace/profile/ningxu_profile_square_newbackground.png --out assets/img/prof_pic.png
rm -f assets/img/prof_pic.jpg
curl -sL https://research.nvidia.com/labs/sil/projects/instant-nurec/assets/teaser-poster.webp -o assets/img/instant-nurec-teaser.webp
file assets/img/prof_pic.png assets/img/instant-nurec-teaser.webp
```

Expected: `prof_pic.png: PNG image data, 800 x 800`; teaser reported as `Web/P image` (or RIFF WebP). If the teaser download is empty or HTML, stop and flag — do not ship a broken image.

- [ ] **Step 2: Replace `_pages/about.md` entirely with:**

```markdown
---
layout: about
title: about
permalink: /
subtitle: Senior Deep Learning &amp; Computer Vision Engineer at <a href="https://www.nvidia.com/">NVIDIA</a>. Santa Clara, CA.

profile:
  align: right
  image: prof_pic.png
  image_circular: false # crops the image to make it circular

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false

latest_posts:
  enabled: false
---

I am a Senior Deep Learning and Computer Vision Engineer at NVIDIA, working on neural reconstruction for autonomous vehicle simulation. Most recently I helped bring [Instant NuRec](https://research.nvidia.com/labs/sil/projects/instant-nurec/) — a feed-forward 3D Gaussian Splatting model — into NVIDIA's AV simulation stack. Previously, I built online HD mapping and multi-city-scale 3D SLAM systems at [Nuro](https://www.nuro.ai/), and centimeter-accurate localization at [Motional](https://motional.com/). I received my M.S. in Robotics from the University of Michigan, advised by Prof. Chad Jenkins, and my B.E. from Beihang University.

I am interested in spatial intelligence, 3D reconstruction, and geometry-grounded foundation models for robots.

## selected projects

<div class="row">
  <div class="col-sm-4 mt-3">
    <a href="https://research.nvidia.com/labs/sil/projects/instant-nurec/">
      <img src="/assets/img/instant-nurec-teaser.webp" class="img-fluid rounded z-depth-1" alt="Instant NuRec teaser" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>Instant NuRec: Feed-Forward 3D Gaussian Reconstruction for Driving Scene Simulation</b><br />
    Turns a short multi-camera driving log into a fully simulatable, layered 3DGS world (static, dynamic, sky) in a single forward pass — roughly 1.5 seconds per scene. I work on its integration into NVIDIA's AV simulation pipeline and the LiDAR-free NuRec release.<br />
    <a href="https://research.nvidia.com/labs/sil/projects/instant-nurec/">Project Page</a> ·
    <a href="https://arxiv.org/pdf/2607.14203">Paper</a> ·
    <a href="https://github.com/nvidia/instant-nurec">Code</a> ·
    <a href="https://huggingface.co/nvidia/instant-nurec">🤗 Model</a>
  </div>
</div>

<div class="row">
  <div class="col-sm-12 mt-4">
    <b>HD Mapping &amp; 3D SLAM at Nuro</b><br />
    Online DETR-based HD map construction in a unified camera–LiDAR BEV framework, fusion of prior map data with live sensor streams for robustness to real-world map changes, and a multi-city-scale 3D mapping and SLAM pipeline.<br />
    <a href="https://www.nuro.ai/blog/unified-perception-model">Unified Perception</a> ·
    <a href="https://www.nuro.ai/blog/exploring-hd-mapping-that-scales">HD Mapping that Scales</a> ·
    <a href="https://www.nuro.ai/blog/the-nuro-autonomy-stack">Nuro Autonomy Stack</a>
  </div>
</div>
```

- [ ] **Step 3: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add assets/img/prof_pic.png assets/img/instant-nurec-teaser.webp _pages/about.md
git rm --cached assets/img/prof_pic.jpg 2>/dev/null; true
git add -A
git commit -m "Add profile photo, bio, and selected projects section"
```

---

### Task 6: Publications (papers.bib)

**Files:**
- Modify: `_bibliography/papers.bib` (full replacement)

- [ ] **Step 1: Replace `_bibliography/papers.bib` entirely with:**

```bibtex
---
---

@inproceedings{bateman2024exploring,
  abbr        = {CVPR},
  bibtex_show = {true},
  selected    = {true},
  title       = {Exploring Real World Map Change Generalization of Prior-Informed HD Map Prediction Models},
  author      = {Bateman, Samuel M. and Xu, Ning and Zhao, H. Charles and Ben Shalom, Yael and Gong, Vince and Long, Greg and Maddern, Will},
  booktitle   = {IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Workshop on Autonomous Driving},
  year        = {2024},
  arxiv       = {2406.01961},
}

@article{sui2020geofusion,
  abbr        = {RA-L},
  bibtex_show = {true},
  selected    = {true},
  title       = {GeoFusion: Geometric Consistency informed Scene Estimation in Dense Clutter},
  author      = {Sui, Zhiqiang and Chang, Haonan and Xu, Ning and Jenkins, Odest Chadwicke},
  journal     = {IEEE Robotics and Automation Letters},
  volume      = {5},
  number      = {4},
  pages       = {5913--5920},
  year        = {2020},
  arxiv       = {2003.12610},
}
```

(Author lists verified against arXiv API on 2026-07-26. The leading `---` lines are required Jekyll front matter — keep them.)

- [ ] **Step 2: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add _bibliography/papers.bib
git commit -m "Add publications: CVPR 2024 WAD map-change paper, RA-L 2020 GeoFusion"
```

---

### Task 7: Deploy workflow (Pages artifact flow)

**Files:**
- Modify: `.github/workflows/deploy.yml`

The imported al-folio `deploy.yml` ends by pushing `_site` to a `gh-pages` branch with `JamesIves/github-pages-deploy-action@v4`. This repo's Pages source is already **GitHub Actions** (the Hugo workflow used `actions/deploy-pages`), so keep that model instead of switching repo settings.

- [ ] **Step 1: Change permissions block**

In `.github/workflows/deploy.yml`, replace

```yaml
permissions:
  contents: write
```

with

```yaml
permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false
```

- [ ] **Step 2: Replace the final deploy step and add a deploy job**

Replace

```yaml
      - name: Deploy 🚀
        if: github.event_name != 'pull_request'
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: _site
```

with

```yaml
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: _site

  deploy:
    if: github.event_name != 'pull_request'
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Note: the existing build job in al-folio's `deploy.yml` is named `deploy` — rename that job key to `build` (i.e. under `jobs:` change `deploy:` to `build:`) so `needs: build` resolves.

- [ ] **Step 3: Also remove the giscus config-update step** (comments are unused):

Delete this step from the build job:

```yaml
      - name: Update _config.yml ⚙️
        uses: fjogeleit/yaml-update-action@main
        with:
          commitChange: false
          valueFile: "_config.yml"
          propertyPath: "giscus.repo"
          value: ${{ github.repository }}
```

- [ ] **Step 4: Validate YAML parses**

Run: `cd /Users/ningxu/workspace/nlog && python3 -c "import yaml; yaml.safe_load(open('.github/workflows/deploy.yml')); print('OK')"`
Expected: `OK`

- [ ] **Step 5: Commit**

```bash
cd /Users/ningxu/workspace/nlog
git add .github/workflows/deploy.yml
git commit -m "Adapt deploy workflow to GitHub Pages artifact flow"
```

---

### Task 8: Local build environment and preview

**Files:**
- Create: `vendor/` + `node_modules/` (gitignored), running server

- [ ] **Step 1: Install toolchain via Homebrew**

```bash
brew install ruby node imagemagick
```

Expected: installs succeed (several minutes). Homebrew Ruby lands at `/opt/homebrew/opt/ruby/bin/ruby` (3.x).

- [ ] **Step 2: Install gems and node deps into the project**

```bash
cd /Users/ningxu/workspace/nlog
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
gem install bundler --no-document
bundle config set --local path vendor/bundle
bundle install
npm ci
```

Expected: `bundle install` completes (jekyll-scholar and friends compile; takes a few minutes). `vendor` and `node_modules` are already in the imported `.gitignore`.

- [ ] **Step 3: Build once and verify content**

```bash
cd /Users/ningxu/workspace/nlog
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
bundle exec jekyll build
grep -o 'Ning' _site/index.html | head -1
grep -c 'instant-nurec\|nuro.ai' _site/index.html
grep -c 'Map Change Generalization\|GeoFusion' _site/index.html
```

Expected: build succeeds; `Ning` found; both grep counts ≥ 1 (projects links and both selected papers render on the homepage).
If the build fails on the `imagemagick`/webp step, confirm `magick -version` works; if it fails on terser, confirm `npm ci` ran. Debug per superpowers:systematic-debugging — do not disable features to route around a failure without understanding it.

- [ ] **Step 4: Serve for user review (background)**

```bash
cd /Users/ningxu/workspace/nlog
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
bundle exec jekyll serve --port 4000
```

Run in background. Expected: `Server address: http://127.0.0.1:4000`

- [ ] **Step 5: Self-check the rendered homepage before handing to user**

Fetch `http://127.0.0.1:4000/` and verify: title "Ning Xu"; nav has only `about`; profile photo renders; bio paragraphs; "selected projects" section with the two entries; selected publications list with CVPR 2024 + RA-L 2020 (Ning Xu bolded); social icons for email/GitHub/LinkedIn/Scholar; footer has no Unsplash credit; **zero** occurrences of `nvidia.slack.com`, `gitlab-master`, `ningxu@nvidia.com`, `docs.google.com` in `_site/` (`grep -rn 'nvidia.slack\|gitlab-master\|ningxu@nvidia\|docs.google' _site/` → no output).

---

### Task 9: USER REVIEW GATE (do not pass without approval)

- [ ] **Step 1:** Tell the user the site is up at `http://127.0.0.1:4000` and ask them to review (bio wording, photo crop, project descriptions, publication rendering, dark/light mode).
- [ ] **Step 2:** Apply requested changes, rebuild/reload, repeat until approved. Commit each round of edits with descriptive messages.
- [ ] **Step 3:** Only proceed to Task 10 after explicit user approval to deploy.

---

### Task 10: Deploy to ningxu.ai

**Files:**
- Merge: `al-folio-site` → `main`, push

- [ ] **Step 0: Install and authenticate the GitHub CLI**

```bash
brew install gh
gh auth status || echo "NEEDS AUTH"
```

If it prints `NEEDS AUTH`, ask the user to run `! gh auth login` in the session (interactive) before continuing.

- [ ] **Step 0.5: Rename the repo to match the new handle (user-approved)**

```bash
gh repo rename nxu-robot.github.io -R nxu-robot/nxu96.github.io --yes
cd /Users/ningxu/workspace/nlog
git remote set-url origin git@github.com:nxu-robot/nxu-robot.github.io.git
git ls-remote origin HEAD
```

Expected: rename succeeds; `ls-remote` prints a hash. This makes it a *user* Pages site again (nxu-robot.github.io will redirect to ningxu.ai). All subsequent `gh api` calls use `repos/nxu-robot/nxu-robot.github.io`.

- [ ] **Step 1: Confirm Pages configuration is the artifact flow**

```bash
gh api repos/nxu-robot/nxu96.github.io/pages --jq '{build_type: .build_type, cname: .cname, https: .https_enforced}'
```

Expected: `build_type` is `workflow`, `cname` is `ningxu.ai`. If `build_type` is `legacy`, run `gh api -X PUT repos/nxu-robot/nxu96.github.io/pages -f build_type=workflow`.
(The repo was renamed from `nxu96/...` — always use the `nxu-robot/nxu96.github.io` path.)

- [ ] **Step 2: Merge and push**

```bash
cd /Users/ningxu/workspace/nlog
git checkout main
git merge --no-ff al-folio-site -m "Launch al-folio portfolio site"
git push origin main
```

- [ ] **Step 3: Watch the Actions run**

```bash
cd /Users/ningxu/workspace/nlog
gh run watch --exit-status
```

Expected: `Deploy site` workflow succeeds (build + deploy jobs green). If it fails, read the failing step's log via `gh run view --log-failed` and fix before retrying.

- [ ] **Step 4: Verify production**

```bash
curl -sL https://ningxu.ai | grep -o '<title>[^<]*</title>'
curl -sL https://ningxu.ai | grep -c 'instant-nurec'
```

Expected: title contains `Ning Xu`; grep count ≥ 1. Also confirm https redirect works: `curl -sI http://ningxu.ai | head -5` shows a 301 to https.

Note: ningxu.ai is proxied through Cloudflare. If the deploy succeeded but ningxu.ai still shows the old Hugo site (`<title>N'log</title>`), it's Cloudflare's edge cache — ask the user to purge cache in the Cloudflare dashboard (or wait for TTL expiry) rather than re-deploying.

- [ ] **Step 5: Report to user with the live URL.**

---

## Notes for the executor

- **Never** commit or reference: `ningxu@nvidia.com`, internal Slack/GitLab/Google Doc URLs, or the resume PDF. The internal resume stays in `/Users/ningxu/workspace/profile/` untouched.
- The al-folio layouts/includes live in the `al_folio_core` gem, not in the repo — customization happens only through `_config.yml`, `_data/`, `_pages/`, `_bibliography/`, and `assets/`.
- If the homepage's selected-papers block renders without a heading or looks off, inspect `_site/index.html` first; the fix belongs in `_pages/about.md` content, not in gem files.
- Old Hugo site remains recoverable via git history (`main` before merge, tag if desired).
