# raul-rojor.github.io

Personal portfolio site — built with Jekyll, hosted on GitHub Pages.

---

## Where things live

```
_config.yml            Name, role, nav links, social links. Start here.
index.html             Landing page. Hero copy is in this file's front matter.
_pages/work.html       /work/ — lists every project automatically.
_pages/about.html      /about/ — experience, education and toolkit live in
                       this file's front matter as editable lists.
_projects/*.md         One file per project. This is where you add work.
_layouts/              Page shells (default, page, project).
_includes/             Header, footer, social icons, project card.
assets/style.scss      The entire stylesheet. Colors are CSS variables at the top.
images/                Photos, screenshots, favicon.
```

## Adding a project

Copy an existing file in `_projects/`, rename it, and edit the front matter.
Nothing else needs touching — the homepage and `/work/` pick it up automatically,
and a case-study page is generated at `/work/<filename>/`.

```yaml
---
order: 3                                       # controls ordering
title: "Churn Predictor"
tagline: "Flags at-risk accounts a month out."
status: Live                                   # the green pill; "" hides it

demo_url: "https://churn.streamlit.app"        # your deployed Streamlit app
repo_url: "https://github.com/raul-rojor/churn"

tech: [Python, XGBoost, pandas, Streamlit]

highlights:                                    # 2–3 lines shown on the card
  - "0.89 AUC, 14 points over the business rule it replaced."
  - "Trained on 400k accounts with heavy class imbalance."

cover: /images/churn.png                       # optional screenshot

facts:                                         # optional bar on the detail page
  - label: Role
    value: Solo build
---

## The problem
...markdown body becomes the case study page...
```

Delete a project by deleting its file.

## Editing your About page

Open `_pages/about.html`. The `experience:`, `education:` and `toolkit:` lists in
the front matter render as the timeline and skill groups — add, reorder or delete
entries there. The intro paragraphs are in the HTML body just below.

Removing the whole `experience:` key hides that section entirely; the same is
true for `education:` and `toolkit:`.

## Changing the look

All colors are CSS custom properties in the `:root` block at the top of
`assets/style.scss`. Swapping `--accent` recolors buttons, links, chips, the
timeline and the glow in one move.

## Running it locally

```sh
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://127.0.0.1:4000>. Pushing to `master` deploys automatically via
GitHub Pages.

---

Originally scaffolded from the [Reverie](https://github.com/amitmerchant1990/reverie)
Jekyll theme (MIT, see `LICENSE`); layouts, styles and structure have since been
rewritten.
