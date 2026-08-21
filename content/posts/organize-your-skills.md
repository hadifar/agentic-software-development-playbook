---
title: Organize your skills
date: 2026-08-20
description: "How to package skills into plugins, load them automatically, and share versioned releases with a marketplace."
categories: [Agentic Software Dev]
tags: [claude-code, plugins, skills]
draft: false
slug: organize-your-skills
---

A single `SKILL.md` file is enough for personal use, but it doesn't scale once you want to
version it, bundle it with agents and hooks, or share it with teammates. This post covers how
to package skills into a plugin and distribute that plugin through a marketplace.

You probably don't need plugins for a personal project or a quick one-off customization. You
do need them once you want to share with teammates, distribute to a community, cut versioned
releases, or reuse the same setup across projects.

## Definitions

**Skill** — an instruction (or set of instructions) that your harness (Claude Code, Codex,
etc.) can load during a session. Example: `/design` in Claude Code, which can be loaded
manually or automatically when you ask it to draw a chart.

**Plugin** — a package that contains skills plus other pieces: agents, tools, commands, hooks,
docs. Example: the
[`claude-security`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-security)
plugin. Running it can spawn a team of agents (explorer, patch-generator, scanner), each with
its own responsibility and capability. It also ships scripts (e.g. `scripts/write_scan_meta.py`,
which writes `scan-meta.json` for a run) and hooks (e.g. `hooks/banner_notice.py`, which shows a
banner at the start of a session). In the simple case, a plugin can just be a bundle of skills.

**Marketplace** — a collection of plugins hosted remotely (e.g. GitHub) or locally. See
[hadifar/myplugins](https://github.com/hadifar/myplugins) for an example.

## Build a plugin

1. Create the plugin directory:

   ```bash
   mkdir -p plugins/daily-plugin/.claude-plugin
   ```

2. Add a plugin manifest at `plugins/daily-plugin/.claude-plugin/plugin.json`:

   ```json
   {
     "name": "daily-plugin",
     "description": "Adds useful skills for my daily tasks",
     "version": "1.0.0",
     "author": {
       "name": "Amir Hadifar"
     }
   }
   ```

3. Add a skill to the plugin (here, one called `grill-me`):

   ```bash
   mkdir -p plugins/daily-plugin/skills/grill-me/
   ```

   Then create `SKILL.md` inside that folder:

   ```markdown
   ---
   name: grill-me
   description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
   ---
   Interview the user relentlessly until you reach a shared understanding. Map this as a
   design tree: every decision branches into the decisions that hang off it.
   ...
   ```

## Load it in Claude Code

For a one-time load, pass `--plugin-dir`:

```bash
claude --plugin-dir ./plugins/daily-plugin
```

To avoid passing `--plugin-dir` on every launch, drop the plugin into your skills directory
(`~/.claude/skills/`). Any folder under a skills directory that contains a
`.claude-plugin/plugin.json` manifest is loaded automatically as `<name>@skills-dir` on the
next session — no marketplace, no install step. This works, but the marketplace approach below
is the better way to distribute and version a plugin.

## Share it and version it with a marketplace

1. Create the marketplace directories (called `myplugins` here):

   ```bash
   mkdir -p myplugins/plugins/
   mkdir -p myplugins/.claude-plugin
   ```

2. Add a marketplace manifest at `myplugins/.claude-plugin/marketplace.json`:

   ```json
   {
     "name": "myplugins",
     "owner": {
       "name": "Amir Hadifar"
     },
     "plugins": [
       {
         "name": "daily-plugin",
         "source": "./plugins/daily-plugin",
         "description": "Adds useful skills for my daily tasks"
       }
     ]
   }
   ```

3. Push the `myplugins` folder to git. Anyone can then install it:

   ```text
   # in a new Claude Code session
   /plugin marketplace add https://github.com/hadifar/myplugins
   /plugin install daily-plugin@myplugins
   ```

4. To ship an update, push to git, then have teammates go to `/marketplace` → select
   `myplugins` → **Update marketplace**.

## References

- [Claude Code — Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [GitHub — anthropics/claude-plugins-official, claude-security plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-security)
- [GitHub — hadifar/myplugins](https://github.com/hadifar/myplugins)
