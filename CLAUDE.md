# CLAUDE.md

## Content architecture

This site runs the `jekyll-theme-chirpy` gem theme. Everything is a blog post — there are no
separate reference-page tabs.

- Posts live in `_posts/` as `YYYY-MM-DD-title.md`, dated by the post's actual origin.
- Front matter: `title`, `date` (`YYYY-MM-DD HH:MM:SS +0000`), `categories: [Work, <Sub>]`,
  `tags: [tag-one, tag-two]`, `description`. No `layout` needed — `_config.yml` defaults
  posts to `layout: post`.
- `categories` is two levels: the top level is `Work` for anything career/tech-related (a
  future `Life` category exists for personal posts). The second element is the sub-category:
  `Tools`, `Building Blocks`, `My Takes`, `Agentic Software Dev`, or `Resources`. Reuse an
  existing sub-category unless the post genuinely doesn't fit any — check `_posts/*.md` for
  what's already in use before inventing a new one.
- Shows up automatically in the home feed, its category archive (nested under Work on the
  Categories tab), the Archives tab, and each tag on the Tags tab.

Match the shape and style of existing sibling posts when adding one — front matter fields,
heading structure, and a one-sentence `description` are expected on every post.

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

Note: `jekyll serve`'s file watcher does NOT reload `_config.yml` — restart the server after
editing site config (title, tagline, description, etc.), or you'll see stale values mixed
with hot-reloaded content changes.
