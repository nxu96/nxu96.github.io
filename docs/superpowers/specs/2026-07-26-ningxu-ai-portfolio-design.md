# ningxu.ai — Personal Academic Portfolio (Design Spec)

**Date:** 2026-07-26
**Status:** Approved for implementation (user will iterate after reviewing the site locally)

## Goal

Replace the current Hugo/PaperMod blog at ningxu.ai with a minimal academic
portfolio site in the style of https://gtangg12.github.io/ (the **al-folio**
Jekyll theme), showcasing Ning Xu's work in spatial intelligence, 3D
reconstruction, and autonomous vehicles.

## Decisions (confirmed with user)

| Topic | Decision |
|---|---|
| Framework | al-folio (Jekyll) — exact theme of the reference site |
| Repo | Rebuild in `nxu96/nxu96.github.io` (same repo as current site); work on branch `al-folio-site`, merge to `main` only after local review |
| Domain | Keep `CNAME` → `ningxu.ai` |
| Blog | None — portfolio only (about page) |
| Resume/CV | Not hosted on the site |
| Contact email | `nxu@umich.edu` |
| GitHub link | `github.com/nxu-robot` |
| LinkedIn | `linkedin.com/in/nxu` |
| Google Scholar | `scholar.google.com/citations?user=6dhIxEgAAAAJ` (strip `authuser` param) |
| Extra sections | None — keep minimal (no news, no timeline) |

## Site structure

Single **about** page (homepage). Nav bar: `about` + dark/light toggle only.

1. **Header**: "Ning Xu" title; profile photo floated right
   (`profile/ningxu_profile_square_newbackground.png` → `assets/img/prof_pic.png`).
2. **Bio** (2 paragraphs, first person — approved draft below).
3. **Selected Projects** section:
   - **Instant NuRec** — Feed-Forward 3D Gaussian Reconstruction for Driving
     Scene Simulation. Links: [Project Page](https://research.nvidia.com/labs/sil/projects/instant-nurec/),
     Paper, Code, 🤗 Model (link targets from the public project page).
     Preview image: teaser frame from the public project page (user may
     supply a replacement).
   - **HD Mapping & 3D SLAM at Nuro** — one consolidated entry linking the
     three public Nuro blog posts:
     - https://www.nuro.ai/blog/unified-perception-model (Online Mapping)
     - https://www.nuro.ai/blog/exploring-hd-mapping-that-scales (Map Fusion / HD mapping)
     - https://www.nuro.ai/blog/the-nuro-autonomy-stack (Autonomy Stack)
4. **Publications** section — driven by `_bibliography/papers.bib`,
   `selected=true` so they render on the homepage; Ning Xu's name bolded;
   arXiv + Bib buttons:
   - Bateman, **Ning Xu**, et al. *Exploring Real World Map Change
     Generalization of Prior-Informed HD Map Prediction Models*, CVPR 2024.
     arXiv: 2406.01961
   - Sui, Chang, **Ning Xu**, et al. *GeoFusion: Geometric consistency
     informed scene estimation in dense clutter*, IEEE RA-L 2020.
     arXiv: 2003.12610
5. **Footer/social icons**: email (`nxu@umich.edu`), GitHub (`nxu-robot`),
   LinkedIn (`nxu`), Google Scholar.

## Approved bio draft

> I am a Senior Deep Learning and Computer Vision Engineer at NVIDIA, working
> on neural reconstruction for autonomous vehicle simulation. Most recently I
> helped bring **Instant NuRec** — a feed-forward 3D Gaussian Splatting model
> — into NVIDIA's AV simulation stack. Previously, I built online HD mapping
> and multi-city-scale 3D SLAM systems at **Nuro**, and centimeter-accurate
> localization at **Motional**. I received my M.S. in Robotics from the
> University of Michigan, advised by Prof. Chad Jenkins, and my B.E. from
> Beihang University.
>
> I am interested in spatial intelligence, 3D reconstruction, and
> geometry-grounded foundation models for robots.

(User will refine wording after seeing the site.)

## Privacy / employer-safety constraints

- **No internal content**: no `@nvidia.com` email, no NVIDIA-internal Slack /
  Google Doc / GitLab links, no resume PDF (current file is the
  NVIDIA-internal version).
- Project descriptions limited to what is already public (the Instant NuRec
  project page, Nuro blog posts, arXiv papers, resume header facts).

## Implementation approach

1. Branch `al-folio-site` in the existing repo.
2. Remove Hugo site files (theme submodule, `hugo.yaml`, `content/`,
   `layouts/`, etc.); keep `CNAME`, `.git`, and this spec.
3. Import the al-folio starter (latest release), trim to a single about page:
   disable blog, news, CV, teaching, repositories, and other demo pages;
   remove demo content.
4. Configure `_config.yml` (name, description, url `https://ningxu.ai`,
   social handles, `selected_papers: true`), add `papers.bib`, profile photo,
   projects section markup on the about page.
5. Replace the GitHub Actions workflow with al-folio's `deploy.yml`
   (build → `gh-pages` branch or Pages artifact, matching al-folio's current
   recommended setup).
6. **Local review gate**: `bundle exec jekyll serve` (Docker fallback) —
   user reviews at `localhost:4000` and requests changes.
7. After approval: merge to `main`, confirm Pages settings + custom domain
   still resolve, verify https://ningxu.ai serves the new site.

## Out of scope

- Blog posts / content migration (old site had only a hello-world post).
- Public resume PDF preparation.
- Any new photography/imagery beyond the provided headshot and public
  project imagery.

## Open items for post-review iteration

- Bio wording tweaks.
- InstantNuRec preview image choice.
- Whether to later add a blog tab, news section, or CV page (all easy
  additions in al-folio).
