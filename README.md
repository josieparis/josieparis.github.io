# josieparis.github.io

Personal academic site for Josie Paris — a static [Jekyll](https://jekyllrb.com) site,
rebuilt from the old Wix site so the content lives in plain text files you own.

## What's where

```
_config.yml              site title, email, social links, baseurl
_data/
  nav.yml                the top navigation
  publications.yml       every paper — the single source of truth
  cv.yml                 employment, education, secondments, grants, awards
  more_projects.yml      projects listed on /projects/ without their own page
_projects/               one Markdown file per project write-up
  king-penguins.md
  guppies.md
  podarcis-lizards.md
index.html               home page
projects.html            /projects/
publications.html        /publications/
cv.html                  /cv/
404.html
_layouts/                page shells (default, page, project)
_includes/               head, header, footer, single publication entry
assets/
  css/main.css           all styling, one file, no build step
  js/nav.js              mobile menu toggle only
  img/                   every image, downloaded from the old site
```

## Everyday edits

**Add a paper.** Open `_data/publications.yml` and copy an existing entry. Fields:

```yaml
- year: 2026
  authors: "Paris JR, Someone Else A"
  title: "Title of the paper"
  venue: "Journal Name, 12(3): 45-67"
  doi: "https://doi.org/10.xxxx/yyyy"
  preprint: "https://doi.org/10.1101/..."   # optional
  note: "* Joint first co-authors"          # optional
  projects: [guppies]                       # optional, see below
```

`Paris JR` is bolded automatically wherever it appears in an author list.
Entries under `published` are grouped by `year`, newest first — keep the list in that order.
The `pipeline` and `in_review` lists have no `year`.

**Show a paper on a project page.** Add `projects: [guppies]` to it. The slugs are
`king-penguins`, `guppies`, `podarcis-lizards` — they match the filenames in `_projects/`.
Nothing is duplicated; project pages filter the master list.

**Add a project.** Drop a new `.md` file into `_projects/`. Front matter:

```yaml
---
title: Short name            # used on the /projects/ card and browser tab
headline: Longer project title   # the <h1> on the page itself
species: Genus species
slug: my-project             # must match the filename
order: 4                     # position on the projects page
image: my-photo.jpg          # a file in assets/img/
context: One line on funding and collaborators.
funders:
  - name: Funder
    logo: logo-funder.png
    url: https://example.org   # optional
---
```

**Change a CV entry.** All of `_data/cv.yml`. The "Academic history" prose is the
only CV content written directly in `cv.html`.

**Change the look.** `assets/css/main.css`. The colour and font variables are all at
the top under `:root`, with a dark-mode block right below it. The old Wix site used
Syne for headings and Questrial for body text; Syne is kept, body text moved to Inter
because Questrial only ships one weight and this site has a lot of dense text. To go
back, change `--font-body` and the Google Fonts link in `_includes/head.html`.

## Preview locally

Jekyll needs Ruby 3.1+. macOS ships Ruby 2.6, which is too old — its `ffi` and `json`
gems refuse to install. A conda env named `jekyll` is already set up on this machine
with Ruby 4.0, Jekyll 4.4, and the two plugins:

```bash
conda activate jekyll && jekyll serve --livereload
```

Then open <http://localhost:4000>. Pages rebuild as you save; `_config.yml` changes
need a restart.

If you ever need to rebuild that env from scratch:

```bash
conda create -y -n jekyll -c conda-forge 'ruby>=3.1' clang_osx-64 clangxx_osx-64 && conda run -n jekyll gem install jekyll jekyll-seo-tag jekyll-sitemap
```

The compilers are needed because conda's Ruby builds native gems with its own toolchain.
`brew install ruby` works too if you'd rather not use conda.

## Publishing on GitHub Pages

1. Create a repo. For a URL like `https://<username>.github.io`, name the repo
   exactly `<username>.github.io` and leave `baseurl: ""` in `_config.yml`.
   For `https://<username>.github.io/website`, name it `website` and set
   `baseurl: "/website"`.
2. Push this folder to it.
3. Repo → **Settings** → **Pages** → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. Set `url:` in `_config.yml` to the address Pages gives you.

GitHub builds the site itself — no Actions workflow needed. Only `jekyll-seo-tag`
and `jekyll-sitemap` are used, both of which GitHub Pages supports natively.

### Custom domain

Add a file named `CNAME` at the repo root containing just your domain
(e.g. `josieparis.com`), then point the domain's DNS at GitHub Pages.
