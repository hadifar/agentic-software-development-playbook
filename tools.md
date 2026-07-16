---
title: Tools
nav_order: 2
description: "A comparison of the current landscape of agentic coding tools."
---

# Tools landscape

A quick comparison of tools people use for vibe-coding and agentic coding. Fill in your own experience/notes as you try each one.

| Tool | Type | Runs where | Notes |
|---|---|---|---|
| **Claude Code** | CLI / IDE agent | Terminal, VS Code, JetBrains | Deep agentic loop, tool use, subagents, hooks, MCP support |
| **Cursor** | IDE (fork of VS Code) | Desktop | Agent mode, inline edits, codebase-aware chat |
| **GitHub Copilot / Copilot Workspace** | IDE extension | VS Code, JetBrains, GitHub.com | Autocomplete + chat + task-level "workspace" planning |
| **Aider** | CLI | Terminal | Open-source, git-native, works with many model backends |
| **Windsurf** | IDE | Desktop | "Cascade" agent flow, multi-file edits |
| **Devin** | Autonomous agent | Cloud sandbox | Longer-running autonomous tasks, less interactive |

## What to evaluate when choosing a tool

- **Context handling** — how well does it understand your whole codebase vs. just open files?
- **Tool use / agentic loop** — can it run tests, execute shell commands, browse docs on its own?
- **Editability of trust** — how easy is it to review, diff, and roll back changes?
- **Extensibility** — hooks, MCP servers, custom subagents/skills.
- **Cost model** — subscription vs. usage-based API billing.

_Add your own hands-on notes here as you try each tool._
