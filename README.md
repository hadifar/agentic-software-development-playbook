<div align="center">
  <img src="static/favicons/apple-touch-icon.png" alt="Amir Hadifar's blog logo" width="120">
</div>

# Amir Hadifar — personal blog

Personal blog on AI, ML, agents, LLMs, and agentic software development — building software by collaborating with AI coding agents: concepts, tool comparisons, workflow patterns, and curated resources.

Built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Local development

```bash
git clone --recurse-submodules https://github.com/hadifar/blog.git
cd blog
hugo server
```

Then open http://localhost:1313/

If you already cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

## Adding a post

Create a new file at `content/posts/my-post-slug.md`:

```yaml
---
title: Post title
date: YYYY-MM-DD
description: "One-sentence summary of the post."
categories: [Tools]
tags: [agents]
draft: false
slug: my-post-slug
---
```

- `categories` is one of: `Tools`, `Building Blocks`, `My Takes`, `Agentic Software Dev`, `Resources`.
  Reuse an existing category unless the post genuinely doesn't fit any.
- `slug` should match the filename and controls the post's URL: `/posts/<slug>/`.
- Images live at `static/images/<post-slug>/` and are referenced as `/images/<post-slug>/foo.png`.

## Build

```bash
hugo --minify
```

Output goes to `public/`.
