# CLAUDE.md

## Content architecture

This site runs the `jekyll-theme-chirpy` gem theme. Two content types, two different rules —
get the type right or the page lands in the wrong place (or nowhere).

**Blog posts** — narrative write-ups, opinions, incident reviews (the "My Takes" and similar
one-off pieces):
- Live in `_posts/` as `YYYY-MM-DD-title.md`, dated by the post's actual origin.
- Front matter: `title`, `date` (`YYYY-MM-DD HH:MM:SS +0000`), `categories: [Category]`,
  `tags: [tag-one, tag-two]`, `description`. No `layout` needed — `_config.yml` defaults
  posts to `layout: post`.
- Shows up automatically in the home feed, its category archive, and each tag archive.

**Reference pages** — durable how-tos and comparisons grouped under a section (Building
Blocks, Tools, Workflows, Resources):
- Each section is one file in `_tabs/` (e.g. `_tabs/tools.md`) — this is what puts it in the
  sidebar. Set `title`, `icon` (a Font Awesome class), `order`, `description`. Body of the
  tab is a short intro plus a bullet list linking to that section's detail pages.
- Detail pages live under `pages/<section>/<slug>.md` (e.g. `pages/tools/landscape.md`), with
  `layout: page` and an explicit `permalink: /pages/<section>/<slug>/`.
- Adding a detail page means editing its section's `_tabs/<section>.md` to link to it — tabs
  don't auto-discover children the way `_posts/` does.

Match the shape and style of existing sibling pages when adding one — front matter fields,
heading structure, and a one-sentence `description` are expected on every page.

## Images and media

`baseurl` is empty (the site is served from `hadifar.net`, not a project sub-path), but still
route paths through `relative_url` — it's a no-op today and keeps pages portable if that
changes.

- **Location:** `assets/images/<page-slug>/`, matching the page's filename.
- **Reference:** `![Alt text]({{ '/assets/images/<page-slug>/foo.png' | relative_url }})`.
- **Sizing or captions:** use HTML — `<img src="{{ … | relative_url }}" alt="…" width="600">`.
- **Format:** SVG for diagrams, PNG/WebP for screenshots.
- **Dark mode:** Chirpy has a light/dark toggle — for diagrams with text, author the SVG with
  `currentColor` or ship a `-dark` variant (as `logo.png` / `logo-dark.png` do).
- **Alt text** is required on every image.
- **Post images:** Chirpy also supports a per-post `image:` front-matter field (social
  preview / header image) — see the theme docs before adding one.

## Local build

```bash
bundle config set path 'vendor/bundle'   # once, keeps gems out of the system path
bundle install
bundle exec jekyll build                 # or `jekyll serve` while iterating
```
