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

## Step 1 — File the post

Create `_posts/YYYY-MM-DD-<slug>.md`, dated today, `<slug>` kebab-case from the title.

```yaml
---
title: "<Human title>"
date: YYYY-MM-DD 00:00:00 +0000
categories: [Work, <Sub-category>]
tags: [<tag-one>, <tag-two>]
description: "<one-sentence summary, under ~120 chars>"
---
```

`categories` is two levels. The top level is `Work` for anything career/tech-related — a
future `Life` category exists for personal posts, use it if the draft is clearly personal
rather than work/tech. The second element is the sub-category; pick the best fit from what's
already in use (check `_posts/*.md` for the current set — as of writing: `Tools`,
`Building Blocks`, `My Takes`, `Agentic Software Dev`, `Resources`):

- **Tools** — reviews/comparisons of a specific product or tool.
- **Building Blocks** — foundational concept explainers (protocols, primitives).
- **My Takes** — first-person opinion, experience, or incident analysis.
- **Agentic Software Dev** — process/workflow/how-to for building software with agents.
- **Resources** — curated links, course notes, talk notes.

If it's genuinely ambiguous or doesn't fit any existing sub-category, use `AskUserQuestion`
rather than guessing or inventing a new one silently.

`layout: post` and `comments`/`toc` come from `_config.yml` defaults; don't set them
per-post.

## Step 2 — Write the body

- Match the voice, formatting, and inline-code conventions of sibling posts in the same
  sub-category (read one first). Skill/command/tool names go in backticks; shell in
  ```bash fences.
- Start with a short intro; end with a `## References` section of URL links.
- If a fact needs a source and the draft doesn't give one, don't invent it — leave a note
  in the suggestions instead of fabricating a citation or link.

## Step 3 — Polish and report suggestions

Fix typos, tighten prose, and correct Markdown/front-matter issues in the file itself.
Then, in your reply to the user, list **suggested improvements** you did *not* auto-apply —
missing sources, thin sections, structural ideas, unclear claims. Keep it short and
actionable.
