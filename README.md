<div align="center">
  <img src="assets/img/favicons/apple-touch-icon.png" alt="Amir Hadifar's blog logo" width="120">
</div>

# Amir Hadifar — personal blog

[![Deploy Jekyll site to Pages](https://github.com/hadifar/blog/actions/workflows/pages.yml/badge.svg)](https://github.com/hadifar/blog/actions/workflows/pages.yml)

Personal blog on AI, ML, agents, LLMs, and agentic software development — building software by collaborating with AI coding agents: concepts, tool comparisons, workflow patterns, and curated resources.


## Contributing

Anything you think is missing is welcome!

1. **Fork the repo**
   ```bash
   git clone https://github.com/hadifar/blog.git
   cd blog
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
4. **Add a blog post** at `_posts/YYYY-MM-DD-title.md`:
   ```yaml
   ---
   title: Post title
   date: YYYY-MM-DD 00:00:00 +0000
   categories: [Work, Tools]
   tags: [agents]
   description: "One-sentence summary of the post."
   ---
   ```
   `categories` is two levels: `Work` (career/tech) or `Life` (personal) as the top level,
   then a sub-category — currently `Tools`, `Building Blocks`, `My Takes`,
   `Agentic Software Dev`, or `Resources`. Reuse an existing one; check `_posts/*.md` for
   what's already there before adding a new sub-category.
5. **Commit and push**
   ```bash
   git add .
   git commit -m "Describe your change"
   git push origin my-change
   ```
6. **Open a pull request** against `main` on [GitHub](https://github.com/hadifar/blog), describing what you changed and why.

### Fast contribution!

If you'd rather not hand-write the front matter and file it yourself, use the `/blog`
Claude Code skill:

1. **Clone the repo**
   ```bash
   git clone https://github.com/hadifar/blog.git
   cd blog
   ```

2. **Drop a raw draft** into [`_inbox/`](_inbox) as a plain `.txt` or `.md` file. Name it
   anything (e.g. `my-post-about-vibing.txt`).
3. **Run `/blog`** in Claude Code. The skill will:
   - **File** it as a properly formatted post in `_posts/`, front matter and style matched
     to sibling posts.
   - **Categorize** it under `Work` (or `Life`) plus the best-fit sub-category.
   - **Polish** the prose and list suggested improvements it didn't auto-apply.
   - **Ship** it on a `blog/<slug>` branch and open a pull request for your review. The
     source draft in `_inbox/` is removed in that same PR.

