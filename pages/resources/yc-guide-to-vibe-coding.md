---
title: YC guide to vibe coding
layout: page
permalink: /pages/resources/yc-guide-to-vibe-coding/
description: "YC guide on practical vibe-coding technique."
---

These are the notes from talks at *Y Combinator*.

## General rule of thumb

The best technique is to do what a professional software engineer already does. **Small code**, **modularity**, and
**abstraction** are your friends — they help both you and the LLM reason about the project.

> Note: this advice will likely shift over the next few months. As models
> get more capable, some of these guardrails will loosen.

## Plan before you build

Don't ask the model to one-shot the whole project. Instead, create a planning file (a
`CLAUDE.md` or a plain markdown file) that lays out step by step what you want to build.
Iterate on it over time and revise it whenever your understanding changes. You can even
use a Planner agent to help revise it.

Some sections that are useful to keep in that file:

```markdown
# Preliminaries
- Stack: pydantic, FastAPI, pydantic-settings, ...

# Priorities
## 1. General architecture for this project
## 2. Use MongoDB as the database
## 3. ...

# Do not implement
- (things to explicitly keep out of scope)

# Ideas for later
- (parking lot for scope you're deferring)
```

Then ask the LLM to implement one section at a time. **Ask small.** Narrow requests are
easier to review, easier to test, and easier to roll back when they go wrong.

## Version control

Use git extensively — for essentially every feature:

- Create a branch and try out your idea.
- If it works, merge it.
- If it doesn't, go back to the branch and try again with a different prompt.
- Make sure what you push is clean and ready.

## Testing

Write **high-level tests** rather than tests for tiny individual functions. High-level
tests capture the behavior you actually care about, so that future changes — whether made
by you or the model — don't quietly destroy your logic.

## Fixing bugs

Often the fastest fix is the simplest: copy and paste the output of the error, the
JavaScript console, or the IDE logs straight into the model. That context alone is
frequently enough for it to find and fix the problem.

## Documentation

Give the model good docs to work from:

- Download documentation locally and point the LLM at it.
- Reference it via a link.
- Or expose it through an MCP server so the model can pull it in on demand.

## Complex functionality

For a hard or unfamiliar feature, build it in a **separate project**, isolated from your
main codebase (sometimes this is even someone else's project on GitHub). Then ask the LLM
to implement the feature in your project while respecting your existing code. This keeps
experimental complexity away from your working code until it's ready.

## Refactor frequently

Ask your LLM to refactor regularly:

- Remove dead code, repetitive code, and junk.
- Keep large files smaller and more modular.

Frequent, small refactors keep the codebase in a shape that both you and the model can
keep working in effectively.

## References

- [YouTube - Tom Blomfield, "How to vibe code"](https://www.youtube.com/watch?v=BJjsfNO5JTo)
- [Youtube - Y Combinator, "Vibe Coding Is The Future"](https://www.youtube.com/watch?v=IACHfKmZMr8)
