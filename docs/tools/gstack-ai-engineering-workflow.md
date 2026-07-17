---
title: "Gstack: AI engineering workflow"
parent: Tools
nav_order: 2
description: "Gstack — a collection of SKILL.md files that give AI agents structured roles across the software sprint."
layout: minimal
---

# Gstack: AI engineering workflow

Notes on Garry Tan's AI coding setup.

## What is gstack

**gstack** is a collection of `SKILL.md` files that give AI agents structured roles for
software development. It is a process, not a collection of tools.

A normal software sprint runs through roughly these stages:

```text
think → plan → design → build → review → test → ship
```

In gstack, there is a `SKILL.md` file (often several) for each of these stages. You invoke
them like slash commands, and each stage feeds context into the next.

Each skill feeds into the next. `/office-hours` writes a design doc that `/plan-ceo-review`
reads. `/plan-eng-review` writes a test plan that `/qa` picks up. `/review` catches bugs
that `/ship` verifies are fixed. Nothing falls through the cracks because every step knows
what came before it.

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

## Starting with /office-hours

`/office-hours` is where you begin. Before you write any code, it acts as a structured
brainstorming partner that helps you **upgrade and revise your idea** — honing it toward
the real problem, testing feasibility, and surfacing what might work and what won't. It
adapts to your context — a startup concept or a side project — and works through several
phases.

## Why it works

The failure mode of AI coding isn't that the model can't write code — it's that it produces
plausible code faster than an unstructured human can review it. gstack front-loads the two
things that keep that speed useful: structured context (think, plan, design) and
verification (review, test, ship). The agent supplies throughput; the roles and gates
supply judgment.

## References

- [YC Library — "Inside Garry Tan's AI Coding Setup"](https://www.ycombinator.com/library/OW-inside-garry-tan-s-ai-coding-setup)
- [GitHub — garrytan/gstack](https://github.com/garrytan/gstack)

