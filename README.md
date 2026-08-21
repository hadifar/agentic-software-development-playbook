<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/images/logo-dark.png">
    <img src="assets/images/logo.png" alt="Amir Hadifar's blog logo" width="120">
  </picture>
</div>

# Amir Hadifar — personal blog

[![Deploy Jekyll site to Pages](https://github.com/hadifar/agentic-software-development-playbook/actions/workflows/pages.yml/badge.svg)](https://github.com/hadifar/agentic-software-development-playbook/actions/workflows/pages.yml)

Personal blog on AI, ML, agents, LLMs, and agentic software development — building software by collaborating with AI coding agents: concepts, tool comparisons, workflow patterns, and curated resources.


## Contributing

Anything you think is missing is welcome!

1. **Fork the repo**
   ```bash
   git clone https://github.com/hadifar/agentic-software-development-playbook.git
   cd agentic-software-development-playbook
   ```
2. **Create a branch** 
   ```bash
   git checkout -b mybranch
   ```
3. **Install dependencies and run the site locally** so you can preview your edits.
   ```bash
   bundle config set path 'vendor/bundle'
   bundle install
   bundle exec jekyll serve
   ```
   Then open http://localhost:4000/
4. **Add a blog post or a reference page.**
   - **Blog post** (a write-up, opinion, or "here's what I tried") — add
     `_posts/YYYY-MM-DD-title.md`:
     ```yaml
     ---
     title: Post title
     date: YYYY-MM-DD 00:00:00 +0000
     categories: [My Takes]
     tags: [agents]
     description: "One-sentence summary of the post."
     ---
     ```
   - **Reference page** (a durable how-to under Building Blocks / Tools / Workflows /
     Resources) — add `pages/<section>/<slug>.md`:
     ```yaml
     ---
     title: Page title
     layout: page
     permalink: /pages/<section>/<slug>/
     description: "One-sentence summary of the page."
     ---
     ```
     Then link to it from that section's tab at `_tabs/<section>.md` — pages aren't
     auto-discovered.
5. **Commit and push**
   ```bash
   git add .
   git commit -m "Describe your change"
   git push origin my-change
   ```
6. **Open a pull request** against `main` on [GitHub](https://github.com/hadifar/agentic-software-development-playbook), describing what you changed and why.

### Fast contribution!

If you'd rather not hand-write the front matter and file it yourself, use the `/blog`
Claude Code skill:

1. **Clone the repo**
   ```bash
   git clone https://github.com/hadifar/agentic-software-development-playbook.git
   cd agentic-software-development-playbook
   ```

2. **Drop a raw draft** into [`_inbox/`](_inbox) as a plain `.txt` or `.md` file. Name it
   anything (e.g. `my-post-about-vibing.txt`).
3. **Run `/blog`** in Claude Code. The skill will:
   - **Classify** the draft as a blog post (`_posts/`) or a reference page
     (`pages/<section>/`).
   - **Write** a properly formatted post — front matter, style matched to sibling posts or
     pages, linked from the section tab if it's a reference page.
   - **Polish** the prose and list suggested improvements it didn't auto-apply.
   - **Ship** it on a `blog/<slug>` branch and open a pull request for your review. The
     source draft in `_inbox/` is removed in that same PR.

