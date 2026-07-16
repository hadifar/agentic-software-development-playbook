# Vibe Coding & Agentic Coding

Source for the GitHub Pages site at https://hadifar.github.io/vibe-coding-best-practices/

Built with [Jekyll](https://jekyllrb.com/) and the [Just the Docs](https://just-the-docs.com/) theme.

## Local development

```bash
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000/vibe-coding-best-practices/

## Structure

- `index.md` — Intro
- `tools.md` — Tools landscape
- `workflows.md` — Workflows & patterns
- `resources.md` — Curated resources
- `others.md` — Glossary, case studies, misc notes

## Deployment

Pushes to `main` trigger `.github/workflows/pages.yml`, which builds the Jekyll site and deploys it via GitHub Pages (Actions-based deployment). In the repo's Settings → Pages, set **Source** to "GitHub Actions".
