---
name: blog
description: Turn a raw draft in _inbox/ into a polished, correctly-filed Jekyll blog post and open a PR. Use when the user runs /blog, drops a draft to publish, or asks to write/file/publish a blog post in this docs site.
---

# /blog — draft to published post

Turn raw text into a polished blog post and open it as a pull request. The user drops a
draft into `_inbox/` and runs `/blog`; you do the rest.

## Input

- **No argument:** process every unprocessed draft in `_inbox/` (skip `README.md`).
- **A path or filename:** process just that draft.
- **Inline text:** if the user pastes text directly instead of using a file, treat that
  text as the draft.

If `_inbox/` has no drafts and no text was given, tell the user to drop a `.txt`/`.md`
file in `_inbox/` (or paste text) and stop.

## Step 1 — Decide: blog post or reference page?

Most drafts are **blog posts** — first-person write-ups, opinions, incident reviews,
"here's what I tried." Those go in `_posts/`.

A draft is a **reference page** instead only when it's a durable how-to or comparison meant
to live under one of the sidebar sections (Building Blocks, Tools, Workflows, Resources) —
the kind of thing that gets linked from, not read chronologically. If genuinely ambiguous,
use `AskUserQuestion` rather than guessing.

## Step 2a — File a blog post

Create `_posts/YYYY-MM-DD-<slug>.md`, dated today, `<slug>` kebab-case from the title.

```yaml
---
title: "<Human title>"
date: YYYY-MM-DD 00:00:00 +0000
categories: [<Category>]
tags: [<tag-one>, <tag-two>]
description: "<one-sentence summary, under ~120 chars>"
---
```

Pick `categories` from what already appears across `_posts/` (e.g. `My Takes`, `Workflows`)
— reuse an existing one unless the draft clearly needs a new one. `layout: post` and
`comments`/`toc` come from `_config.yml` defaults; don't set them per-post.

## Step 2b — File a reference page instead

Only when Step 1 called for it:

1. Create `pages/<section>/<slug>.md` (section is one of `building-blocks`, `tools`,
   `workflows`, `resources`):

   ```yaml
   ---
   title: "<Human title>"
   layout: page
   permalink: /pages/<section>/<slug>/
   description: "<one-sentence summary, under ~120 chars>"
   ---
   ```

2. Add a link to it from that section's tab index at `_tabs/<section>.md` — tabs don't
   auto-discover pages, so a page not linked there is orphaned.

## Step 3 — Write the body

- Match the voice, formatting, and inline-code conventions of sibling posts/pages in the
  same location (read one first). Skill/command/tool names go in backticks; shell in
  ```bash fences.
- Start with a short intro; end with a `## References` section of URL links.
- If a fact needs a source and the draft doesn't give one, don't invent it — leave a note
  in the suggestions instead of fabricating a citation or link.

## Step 4 — Polish and report suggestions

Fix typos, tighten prose, and correct Markdown/front-matter issues in the file itself.
Then, in your reply to the user, list **suggested improvements** you did *not* auto-apply —
missing sources, thin sections, structural ideas, unclear claims. Keep it short and
actionable.
