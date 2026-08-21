---
title: "Gstack: AI engineering workflow"
date: 2026-07-31
description: "Gstack — a collection of SKILL.md files that give AI agents structured roles across the software sprint."
categories: [Agentic Software Dev]
draft: false
slug: gstack-ai-engineering-workflow
---

This post explains the AI coding workflow of Y Combinator's CEO — how he uses [GStack](https://github.com/garrytan/gstack.git) for ideation, building, and deployment.

## What is gstack

**gstack** is a collection of `SKILL.md` files that give your AI agents personas for different stages of the
software/product development life cycle.

A normal software sprint runs through roughly these stages:

```text
think → plan → design → build → review → test → ship
```

In gstack, there is a `SKILL.md` file (often several) for each of these stages. You invoke
them to guide your agents toward the goal (generating code, a specification, ideation, etc.).

## Some of the skills

Below is a sample of the available skills. Explore
[the gstack repo](https://github.com/garrytan/gstack) to find the full set.

| Skill | What it does |
| --- | --- |
| `/office-hours` | Starting point. Reframes your product idea before you write code. |
| `/plan-ceo-review` | CEO-level review: find the 10-star product in the request. |
| `/plan-eng-review` | Lock architecture, data flow, edge cases, and tests. |
| `/plan-design-review` | Rate each design dimension 0–10, explain what a 10 looks like. |
| `/plan-devex-review` | DX-mode review: TTHW, magical moments, friction points, persona traces. |
| `/plan-tune` | Self-tune `AskUserQuestion` sensitivity per question. |
| `/autoplan` | One command runs CEO → design → eng → DX review. |
| `/design-consultation` | Build a complete design system from scratch. |
| `/spec` | Turn vague intent into a precise, executable spec in five phases. Files a GitHub issue, optionally spawns a Claude Code agent in a fresh worktree, and lets `/ship` close the source issue on merge. |

## Setup

### Installation

Clone gstack into your Claude skills directory and run the setup script:

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack \
  && cd ~/.claude/skills/gstack \
  && ./setup
```

Then add a `gstack` section to your `CLAUDE.md` that:

- tells the agent to use the `/browse` skill for all web browsing (never the
  `mcp__claude-in-chrome__*` tools), and
- lists the available skills so the agent knows what it can call: `/office-hours`,
  `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review`, `/design-consultation`,
  `/design-shotgun`, `/design-html`, `/review`, `/ship`, `/land-and-deploy`, `/canary`,
  `/benchmark`, `/browse`, `/connect-chrome`, `/qa`, `/qa-only`, `/design-review`,
  `/setup-browser-cookies`, `/setup-deploy`, `/setup-gbrain`, `/retro`, `/investigate`,
  `/document-release`, `/document-generate`, `/codex`, `/cso`, `/autoplan`,
  `/plan-devex-review`, `/devex-review`, `/careful`, `/freeze`, `/guard`, `/unfreeze`,
  `/gstack-upgrade`, `/learn`.

Optionally, add gstack to the current project too, so teammates get it.

### Quick start

1. Install gstack (30 seconds — see above).
2. Run `/office-hours` — describe what you're building.
3. Run `/plan-ceo-review` on any feature idea.
4. Run `/review` on any branch with changes.
5. Run `/qa` on your staging URL.
6. Stop there. You'll know if this is for you.

`/office-hours` is where you begin. Before you write any code, it acts as a structured
brainstorming partner that helps you **upgrade and revise your idea** — honing it toward
the real problem, testing feasibility, and surfacing what might work and what won't. It
adapts to your context — a startup concept or a side project — and works through several
phases. For example, `/office-hours` asks you several questions that depend on your product:
who said this software/product/idea is interesting? Is it a commercial, learning, or
entrepreneurship project? It even searches the web to find relevant competitors and identify your differentiator. Based on your answers, it then suggests a couple of points worth
noting and considering. By answering those questions, your agent can construct better context
for later stages in the pipeline and development.

## Alternatives

[Superpowers](https://github.com/obra/superpowers/) is basically the same idea; we review it in another post.

## References

- [YC Library — "Inside Garry Tan's AI Coding Setup"](https://www.ycombinator.com/library/OW-inside-garry-tan-s-ai-coding-setup)
- [GitHub — garrytan/gstack](https://github.com/garrytan/gstack)
