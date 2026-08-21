# CLAUDE.md

## Content architecture

This site runs [Hugo](https://gohugo.io/) with the [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
theme (vendored as a git submodule at `themes/PaperMod`). Everything is a blog post — there are
no separate reference-page tabs.

- Posts live in `content/en/posts/` as `<slug>.md`.
- Front matter: `title`, `date` (`YYYY-MM-DD`), `description`, `categories: [<Category>]`,
  `draft: false`, `slug: <slug>` (must match the filename — it pins the post's URL to
  `/posts/<slug>/` regardless of title changes later). No `tags` field — this site only uses
  categories.
- `categories` is a flat list, one of: `Tools`, `Building Blocks`, `My Takes`,
  `Agentic Software Dev`, `Resources`. Reuse an existing category unless the post genuinely
  doesn't fit any — check `content/posts/*.md` for what's already in use before inventing a
  new one.
- Shows up automatically in the home feed, its category archive page, and the Archives page.

Match the shape and style of existing sibling posts when adding one — front matter fields,
heading structure, and a one-sentence `description` are expected on every post.

## Languages

This is a Hugo multilingual site — English is the default language, content lives under
`content/en/`, served at the site root (`/`). Persian (`fa`) is scaffolded (config, RTL layout,
language switcher in the header) but has no translated content yet — `content/fa/` is empty.

To add a Persian translation of a page, create the matching file under `content/fa/` (e.g.
`content/fa/posts/<slug>.md` for a post already at `content/en/posts/<slug>.md`) with the same
front matter shape, translated `title`/`description`, and the same `slug` — PaperMod will then
link the two as translations of each other and the per-page language switcher lights up.
Categories are per-language; a Persian post needs Persian category names, and there's no
existing convention for those yet, so ask before inventing one.

## Images and media

- **Location:** `static/images/<post-slug>/`, matching the post's slug.
- **Reference:** `![Alt text](/images/<post-slug>/foo.png)`.
- **Sizing or captions:** use HTML — `<img src="/images/<post-slug>/foo.png" alt="…" width="600">`.
- **Format:** SVG for diagrams, PNG/WebP for screenshots.
- **Dark mode:** PaperMod has a light/dark toggle — for diagrams with text, author the SVG with
  `currentColor` or ship a separate `-dark` variant of the image (e.g. `foo.png` / `foo-dark.png`).
- **Alt text** is required on every image.

## Internal links

Link to other posts with a plain absolute path: `[title](/posts/other-post-slug/)`. Don't use
Jekyll-style Liquid tags (`{{ '/posts/x/' | relative_url }}`) — this is a Hugo site now. Persian
posts should link to other Persian posts the same way, once they exist.

## Local build

```bash
git submodule update --init --recursive   # once, pulls in the PaperMod theme
hugo server                                 # live-reloading dev server on :1313
hugo --minify                               # production build into public/
```
