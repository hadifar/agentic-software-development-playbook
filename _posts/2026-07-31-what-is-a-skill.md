---
title: "What is a Skill?"
date: 2026-07-31 00:00:00 +0000
categories: [Building Blocks]
tags: [skills, claude-code, agents]
description: "How skills fit into an LLM's context, what a SKILL.md file looks like, and how to write your own."
---

A skill is a Markdown file an agent loads on demand to learn how to handle a particular
kind of request. It's useful when you have a repetitive task and don't want to re-prompt
your agent each time.

Think of it as the utility function of prompting: instead of duplicating the same
instructions in every conversation, you write them once and reuse them. Skills are more
general than that, of course — their behaviour adapts to the request in a way a single
utility function doesn't.

Before jumping into skills, let's review what the agent actually sees when you enter a
prompt.

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

Skills slot into that picture as **retrieved items**. When you enable a skill — or Claude
decides to load one — it loads one or more `SKILL.md` files that specify what to do in a
given situation to better fulfil the request.

The loading happens in two stages. Initially the agent sees only the **name** and
**description** of each available skill — nothing else. When the description tells it that a
skill is needed to fulfil your request, it triggers that skill and loads the entire file
into its context. From then on, the content of `SKILL.md` travels alongside everything else
listed above each time you submit a prompt.

## An example: `dataviz`

In Claude, if you ask:

```text
draw me a plot which shows y = x^2
```

Initially, Claude only sees the name and description:

```text
/dataviz  Use this skill whenever you are about to create ANY chart, graph, plot,
          dashboard, or data visualization, in ANY output medium — an HTML or React
          artifact, inline SVG, plotting code in any library (matplotlib, plotly, d3,
          Recharts, …)…
```

That's enough to decide, so it loads (or asks permission to load) the `dataviz` skill,
which instructs Claude to complete the request in a certain way. The actual content looks
roughly like this — see the
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

It's a well-written text that guides the model on what to do and what not to do.
It also points to reference files the model can pull in for more detail when needed.

Beyond `dataviz`, Claude ships other skills such as `/debug` and `/doctor`, designed to
perform an action, which you can invoke directly with `/` followed by the skill name. As
above, Claude initially sees only their name and description, and loads the actual content
when one is triggered.

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

```text
skill-name/
├── SKILL.md              # Required: metadata + core instructions (<500 lines)
├── scripts/              # Executable code (Python/Bash) designed as tiny CLIs
├── references/           # Supplementary context (schemas, cheatsheets)
└── assets/               # Templates or static files used in output
```

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
- [GitHub — mgechev/skills-best-practices](https://github.com/mgechev/skills-best-practices)
