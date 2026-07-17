---
title: "Gstack: An AI Engineering Workflow"
parent: Resources
nav_order: 2
description: "Gstack — a collection of SKILL.md files that give AI agents structured roles across the software sprint."
layout: minimal
---

# Gstack: An AI Engineering Workflow

Notes on Garry Tan's AI coding setup.

- [YC Library — "Inside Garry Tan's AI Coding Setup"](https://www.ycombinator.com/library/OW-inside-garry-tan-s-ai-coding-setup)
- [GitHub — garrytan/gstack](https://github.com/garrytan/gstack)

## What is gstack

**gstack** is a collection of `SKILL.md` files that give AI agents structured roles for
software development. Instead of treating the agent as a blank-slate copilot, gstack gives
it **specialized roles and decision gates** that mirror how a real engineering team
operates — product strategy, architecture review, design quality, security, QA, and
deployment.

A normal software sprint runs through roughly these stages:

```text
think → plan → design → build → review → test → ship
```

In gstack, there is a `SKILL.md` file (often several) for each of these stages. You invoke
them like slash commands, and each stage feeds context into the next.

## Some of the skills

Below is a sample of the available skills. Explore
[the gstack repo](https://github.com/garrytan/gstack) to find the full set.

| Skill | What it does |
| --- | --- |
| `/office-hours` | Start here. Reframes your product idea before you write code. |
| `/plan-ceo-review` | CEO-level review: find the 10-star product in the request. |
| `/plan-eng-review` | Lock architecture, data flow, edge cases, and tests. |
| `/plan-design-review` | Rate each design dimension 0–10, explain what a 10 looks like. |
| `/plan-devex-review` | DX-mode review: TTHW, magical moments, friction points, persona traces. |
| `/plan-tune` | Self-tune `AskUserQuestion` sensitivity per question. |
| `/autoplan` | One command runs CEO → design → eng → DX review. |
| `/design-consultation` | Build a complete design system from scratch. |
| `/spec` | Turn vague intent into a precise, executable spec in five phases. Files a GitHub issue, optionally spawns a Claude Code agent in a fresh worktree, and lets `/ship` close the source issue on merge. |

## Starting with /office-hours

`/office-hours` is where you begin. Before you write any code, it acts as a structured
brainstorming partner that helps you **upgrade and revise your idea** — honing it toward
the real problem, testing feasibility, and surfacing what might work and what won't.

It adapts to your context — a startup concept or a side project — and works through several
phases:

1. **Gather context.** Reads existing docs, maps the codebase, and clarifies what you want
   to explore.
2. **Interrogate the idea.** For startups it applies *six forcing questions* — probing real
   demand, the status quo, the target user, the minimal viable product, surprises you've
   observed, and future fit. "Specificity is the only currency": vague answers get pushed
   toward concrete evidence. For side projects, it takes a more collaborative approach,
   hunting for the most exciting version of the idea and the fastest path to something
   shareable.
3. **Challenge assumptions** before proposing any solution.
4. **Generate approaches** — typically two or three distinct paths: one minimal, one
   architecturally ideal, one creatively lateral. Optionally it dispatches the problem to
   an independent AI perspective for a fresh second opinion.
5. **Synthesize and document.**

The deliverable is a **design doc**, not code — capturing the problem, the premises, the
chosen approach, and one concrete next assignment. Crucially, `/office-hours` never starts
implementation; it produces the design doc that downstream skills like `/plan-eng-review`
or `/plan-ceo-review` pick up from.

## Why it works

The failure mode of AI coding isn't that the model can't write code — it's that it produces
plausible code faster than an unstructured human can review it. gstack front-loads the two
things that keep that speed useful: **structured context** (think, plan, design) and
**verification** (review, test, ship). The agent supplies throughput; the roles and gates
supply judgment.
