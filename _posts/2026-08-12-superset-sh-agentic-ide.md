---
title: "superset.sh: IDE to run parallel coding agents"
date: 2026-08-12 00:00:00 +0000
categories: [Tools]
tags: [superset, worktrees, ide]
description: "superset.sh, IDE for running multiple coding agents in isolated git worktrees"
---

[superset.sh](https://docs.superset.sh/) is a desktop app for running several AI coding
agents at once, each in its own isolated workspace. It covers the same ground as
[Conductor]({{ '/posts/conductor-ide-for-parallel-agent/' | relative_url }}), but
also runs on Linux (Conductor is Mac-only as of August 2026).

## What it does

Like other agentic IDEs, superset.sh is built around a few features specifically for
modern agentic coding:

- Run multiple agents simultaneously
- Isolate each task in its own git worktree so agents don't interfere with each other
- Monitor all agents from one place and get notified when they need attention
- Switch between LLM providers (Claude, Codex, and others) per task

## Why worktrees, not branches

The core concept behind superset.sh — and tools like it — is the git
[worktree](https://git-scm.com/docs/git-worktree): a separate directory with its own
files and branch, sharing the same repository history and remote as your main checkout.

A plain branch checkout still points at the same files on disk. A worktree gives each
task its own physical copy of those files. That's what lets multiple agents (or humans)
work on the same repo at the same time without stepping on each other — and lets you
throw a workspace away without touching your main branch. Each workspace also gets its
own worktree, so switching between tasks is a click instead of a
stash-checkout-rebuild cycle.

## Install on Ubuntu

1. Download the AppImage from the [latest release](https://github.com/superset-sh/superset/releases/latest/download/Superset-x86_64.AppImage).
2. Run it:

   ```bash
   ./Superset-x86_64.AppImage --no-sandbox
   ```

   `--no-sandbox` skips a permission prompt — only pass it if you trust the app.

3. If that doesn't work, extract and fix sandbox permissions manually:

   ```bash
   ./Superset-x86_64.AppImage --appimage-extract
   sudo chown root:root squashfs-root/chrome-sandbox
   sudo chmod 4755 squashfs-root/chrome-sandbox
   ./squashfs-root/AppRun
   ```

## Daily recipe

The [docs](https://docs.superset.sh/) have a full set of examples and best practices.
For day-to-day bug fixes and features, this pattern works well:

1. **Feature** (⌘N → new branch): prompt an agent with the task and let it run.

   ```
   Build the PDF parsing button on the upload page. Match the existing
   export patterns in this codebase. Stop and ask before adding dependencies.
   ```

2. **Bugfix** (⌘N → new branch): describe the bug and let the agent reproduce and fix it.

   ```
   Users report the auth form clears on validation errors. Reproduce it,
   find the root cause, and fix it. Include the repro steps in your final message.
   ```

3. **Rotate attention**: use the sidebar to jump to whichever workspace needs you next.

## What stands out

- **Notifications.** A small feature, but a useful one — it lets you switch attention to
  a task the moment an agent finishes or needs input, instead of polling each workspace.
- **Side-by-side terminals.** Beyond separate terminal tabs, you can view terminal/agent
  panes side by side per worktree, giving more visibility into what's running.
- **Fast provider switching.** Swap between Claude, Codex, and other LLM providers per task.
- **Skills and hooks.** Combined with custom skills and hooks, superset.sh can enforce
  conventions automatically — e.g., tagging branches as `feat/`, `bug/`, or `doc/`, or
  running pre-commit checks before every push.

## References

- [superset.sh docs](https://docs.superset.sh/)
- [superset.sh automations](https://docs.superset.sh/automations)
- [Latest release (AppImage)](https://github.com/superset-sh/superset/releases/latest/download/Superset-x86_64.AppImage)
- [git-worktree docs](https://git-scm.com/docs/git-worktree)
