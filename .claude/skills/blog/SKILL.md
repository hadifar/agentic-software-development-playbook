---
name: blog
description: Turn a raw draft in _inbox/ into a polished, correctly-filed Jekyll blog post and open a PR. Use when the user runs /blog, drops a draft to publish, or asks to write/file/publish a blog post in this docs site.
---

# /blog — draft to published post

Turn raw text into a polished blog post, filed in the right docs section, and opened as a
pull request. The user drops a draft into `_inbox/` and runs `/blog`; you do the rest.

## Input

- **No argument:** process every unprocessed draft in `_inbox/` (skip `README.md`).
- **A path or filename:** process just that draft.
- **Inline text:** if the user pastes text directly instead of using a file, treat that
  text as the draft.

If `_inbox/` has no drafts and no text was given, tell the user to drop a `.txt`/`.md`
file in `_inbox/` (or paste text) and stop.

## Step 1 — Classify the draft

Pick exactly one destination based on what the draft is *about*:

| Section | Put here when the draft is… | parent |
| --- | --- | --- |
| `docs/tools/` | about a specific tool, product, or framework (e.g. Cursor, Aider, gstack) | `Tools` |
| `docs/workflows/` | a process, pattern, technique, or how-to for working with agents | `Workflows` |
| `docs/resources/` | curated links, talk notes, or reference material | `Resources` |

If it's genuinely ambiguous, use `AskUserQuestion` to let the user pick — don't guess
silently. State your reasoning either way.

## Step 2 — Write the post

Create `docs/<section>/<slug>.md`, where `<slug>` is kebab-case from the title.

Front matter must match the existing posts in that folder exactly in shape:

```yaml
---
title: "<Human title>"
parent: <Tools | Workflows | Resources>
nav_order: <next available integer in that folder>
description: "<one-sentence summary, under ~120 chars>"
layout: minimal
---
```

- Compute `nav_order` by reading the existing files in the target folder and taking
  `max(nav_order) + 1`. Never collide with an existing value.
- The body starts with a single `# <Title>` H1, then an intro line, then `##` sections.
- Match the voice, formatting, and inline-code conventions of the sibling posts (read one
  first). Skill/command/tool names go in backticks; shell in ```bash fences.
- If a fact needs a source and the draft doesn't give one, don't invent it — leave a note
  in the suggestions instead of fabricating a citation or link.
- Always include a short intro at the beginning and a References section at the end.
  References are URL links.

## Step 3 — Polish and report suggestions

Fix typos, tighten prose, and correct Markdown/front-matter issues in the file itself.
Then, in your reply to the user, list **suggested improvements** you did *not* auto-apply —
missing sources, thin sections, structural ideas, unclear claims. Keep it short and
actionable.

## Step 4 — Ship as a PR

1. Create a branch: `blog/<slug>`.
2. `git rm` the source draft from `_inbox/` (skip this if the input was inline text).
3. Stage the new post + the draft removal;`
4. Push and open a PR with `gh pr create`. The PR body should summarize what the post is,
   which section it went to and why, and the suggestions from Step 3.

Do **not** push to `main` or merge — the PR is the review gate. GitHub Pages rebuilds on
merge, so nothing goes live until the user merges.


