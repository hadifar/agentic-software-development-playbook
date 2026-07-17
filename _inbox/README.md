# _inbox — blog draft inbox

Drop a raw draft here as a plain `.txt` or `.md` file, then run `/blog` in Claude Code.

The agent will:

1. **Classify** the draft into `docs/tools/`, `docs/workflows/`, or `docs/resources/`.
2. **Write** a properly formatted Jekyll post (front matter, `nav_order`, style-matched).
3. **Polish** the prose and list suggested improvements.
4. **Ship** it on a branch and open a pull request for your review.

## Notes

- One draft = one file. Name it anything (e.g. `my-post-about-cursor.txt`).
- A leading `# Title` line is optional — the agent will infer a title if absent.
- This folder starts with an underscore, so Jekyll ignores it; drafts never publish.
- After a draft is turned into a PR, its source file in `_inbox/` is deleted in that same PR.
