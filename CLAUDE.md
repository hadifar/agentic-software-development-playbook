# CLAUDE.md

## Content architecture

Every page under `docs/` is Markdown with Just the Docs front matter. Navigation is driven
entirely by that front matter, not by config — get it right or the page lands in the wrong
place (or nowhere).

- **Top-level sections** are files directly in `docs/`: `index.md` (Intro), `tools.md`,
  `workflows.md`, `building-blocks.md`, `resources.md`. Each declares a `title` and `nav_order`.
- **Sub-pages** live in a matching subfolder (e.g. `docs/tools/landscape.md`) and set
  `parent:` to the **exact section title** (e.g. `parent: Tools`). The section file and the
  folder name must correspond.
- `nav_order` controls ordering within a section — a new sub-page takes the next unused number
  among its siblings.
- Sub-pages use `layout: minimal`; `docs/index.md` uses `layout: home`. The `docs` scope
  defaults to `layout: default` (see `_config.yml`).

Match the shape and style of existing sibling pages when adding a page — front matter fields,
heading structure, and a one-sentence `description` are expected on every page.
