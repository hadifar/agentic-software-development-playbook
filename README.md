<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/images/logo-dark.png">
    <img src="assets/images/logo.png" alt="Vibe Coding & Agentic Coding logo" width="120">
  </picture>
</div>

# Vibe Coding & Agentic Coding

[![Deploy Jekyll site to Pages](https://github.com/hadifar/vibe-coding-best-practices/actions/workflows/pages.yml/badge.svg)](https://github.com/hadifar/vibe-coding-best-practices/actions/workflows/pages.yml)

A practical guide to building software by collaborating with AI coding agents — concepts, tool comparisons, workflow patterns, and curated resources.


## Contributing

Anything you think is missing is welcome!

1. **Fork the repo**
   ```bash
   git clone https://github.com/hadifar/vibe-coding-best-practices.git
   cd vibe-coding-best-practices
   ```
2. **Create a branch** 
   ```bash
   git checkout -b mybranch
   ```
3. **Install dependencies and run the site locally** so you can preview your edits.
   ```bash
   bundle install --path vendor/bundle
   bundle exec jekyll serve
   ```
   Then open http://localhost:4000/vibe-coding-best-practices/
4. **Edit or add a page** under [`docs/`](docs). Pages are Markdown files with Just the Docs front matter, for example:
   ```yaml
   ---
   title: Page title
   parent: Tools        
   nav_order: 1
   description: "One-sentence summary of the page."
   layout: minimal
   ---
   ```
   - Top-level sections (`Intro`, `Tools`, `Workflows`, `Resources`) live directly in `docs/`.
   - Sub-pages live in a matching subfolder (e.g. `docs/tools/landscape.md`) and set `parent:` to the section title.
5. **Commit and push**
   ```bash
   git add .
   git commit -m "Describe your change"
   git push origin my-change
   ```
6. **Open a pull request** against `main` on [GitHub](https://github.com/hadifar/vibe-coding-best-practices), describing what you changed and why.

### Fast contribution!

If you'd rather not hand-write the front matter and file it yourself, use the `/blog`
Claude Code skill:

1. **Clone the repo** ```bash
   git clone https://github.com/hadifar/vibe-coding-best-practices.git
   cd vibe-coding-best-practices
   ```

2. **Drop a raw draft** into [`_inbox/`](_inbox) as a plain `.txt` or `.md` file. Name it
   anything (e.g. `my-post-about-vibing.txt`); 
3. **Run `/blog`** in Claude Code. The skill will:
   - **Classify** the draft into `docs/tools/`, `docs/workflows/`, or `docs/resources/`.
   - **Write** a properly formatted post — front matter, next `nav_order`, style matched to
     the sibling pages.
   - **Polish** the prose and list suggested improvements it didn't auto-apply.
   - **Ship** it on a `blog/<slug>` branch and open a pull request for your review. The
     source draft in `_inbox/` is removed in that same PR.

