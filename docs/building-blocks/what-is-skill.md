---
title: "What is a Skill?"
parent: Building Blocks
nav_order: 3
description: "How skills fit into an LLM's context, what a SKILL.md file looks like, and how to write your own."
layout: minimal
---

# What is a Skill?

A skill is a Markdown file an agent loads on demand to learn how to handle a particular
kind of request. To see why that matters, it helps to look at what actually reaches the
model when you send a message.

## What gets sent to the model

When you send a request to Claude or ChatGPT, your message is not the only thing that
travels to the model. Alongside it go:

- **System prompt** — a general instruction telling the model what to do (see
  [leaked examples](https://github.com/asgeirtj/system_prompts_leaks)).
- **Conversation history** — what you've discussed in the chat so far.
- **Tool schemas** — any tools you've enabled, e.g. deep research or web search.
- **Retrieved items** — results of any search the model runs, locally or online.
- **User input** — your actual question or request.

Some concatenation of the above is passed to the LLM, which then generates a response.

Skills slot into that picture as retrieved items. When you enable a skill — or Claude
decides to load one — it retrieves one or more `SKILL.md` files that specify what to do in
a given situation to better fulfil the request.

## An example: `dataviz`

In Claude, if you ask:

```text
draw me a plot which shows y = x^2
```

it loads (or asks permission to load) a skill called `dataviz` that instructs Claude to
complete the request in a certain way. That skill looks roughly like this — see the
[full version](https://gist.github.com/hadifar/2908f52654139ee6391824715fff1909):

```markdown
# Data Visualization
A chart is read by people and executed by you. This skill turns "make it look good" into a
procedure with checks, so the result is right by construction rather than by taste...

## The procedure — do these in order
Color comes LAST. Most bad charts pick colors first.
Pick the form. What is the data's job — magnitude, identity, polarity, a single headline, ...

## Non-negotiables (true in every design system)
...

## Plugging in a design system
...

## Reference files
| File | What it answers |
|------|-----------------|
| `references/choosing-a-form.md` | Which chart type / is it even a chart? |
| `references/color-formula.md` | The four jobs, the six checks, snap-to-passing |
| `references/marks-and-anatomy.md` | Mark specs, spacers, labels, figures, hero number |
...
```

It's a well-written Markdown file that guides the model on what to do and what not to do.
It also points to reference files the model can pull in for more detail when needed.

Beyond `dataviz`, Claude ships other skills such as `/debug` and `/doctor`, designed to
perform an action; you invoke them with `/` followed by the skill name.

## Writing your own

You can write your own skills too — see the
[Claude Code skills docs](https://code.claude.com/docs/en/skills#getting-started). Ask
Claude to create a skill that helps you polish your email, for instance, and it will write
a file at:

```text
~/.claude/skills/<skill-name>/SKILL.md
```

Note that it's a *folder*, not a lone file. That means you can include additional resources
— code, docs, examples — that help the model further, on top of the single `SKILL.md`.

For guidance on what makes a skill effective, a solid set of practices is collected in the
[agentskills.io best practices](https://agentskills.io/skill-creation/best-practices).

## Where did the idea come from?

The earliest place I saw skills introduced for LLMs was
[Metacognitive Capabilities of LLMs](https://proceedings.neurips.cc/paper_files/paper/2024/file/2318d75a06437eaa257737a5cf3ab83c-Paper-Conference.pdf),
where the authors define various skills for solving math problems — later picked up and
refined by others.

The underlying idea of decomposing a task into simpler sub-tasks isn't new. Industrial
automation has long done the same:

- The highest-level goal (e.g. *make beer*).
- The major production stages (e.g. *mash*, *ferment*).
- The functions within each stage, which is where a skill fits (e.g. *heat liquid*,
  *add ingredients*).

## Limitations

Context limits and forgetting are the usual suspects, but the main gap is **composition**:
having the model look across several skills and combine them to reach a goal more complex
than any single skill describes. Picking and following one skill is largely solved;
stitching several together is not.

## References

- [agentskills.io — skill creation best practices](https://agentskills.io/skill-creation/best-practices)
- [Gist — full `dataviz` skill](https://gist.github.com/hadifar/2908f52654139ee6391824715fff1909)
- [GitHub — anthropics/skills](https://github.com/anthropics/skills)
- [Claude Code — bundled skills](https://code.claude.com/docs/en/skills#bundled-skills)
- [Metacognitive Capabilities of LLMs (NeurIPS 2024)](https://proceedings.neurips.cc/paper_files/paper/2024/file/2318d75a06437eaa257737a5cf3ab83c-Paper-Conference.pdf)
- [GitHub — asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks)
