---
title: "Conductor: IDE to run parallel coding agents"
parent: Tools
nav_order: 3
description: "Conductor — a desktop app for running multiple AI coding agents in parallel"
layout: minimal
---

# Conductor: run parallel coding agents

[Conductor](https://www.conductor.build/) is a desktop app for running several AI coding
agents in parallel on the same project. Instead of driving one agent at a time, you spin up
multiple agents that each work independently and hand you the results to review.

## What it does

Conductor connects to your AI provider — Claude, Codex, and others — and lets you launch
multiple agents to work on your project at once. Each agent runs in its own **git
worktree**, so their changes stay isolated from one another. When an agent finishes, its
worktree makes it easy to review the diff, integrate the work, and push it to git.

Running agents in parallel worktrees is the key idea: several tasks progress at the same
time without stepping on each other's files, and you keep a clean path from an agent's
output to a commit.

For a walkthrough of the tool, see the [overview video](https://www.youtube.com/watch?v=VsWWy2kVpa8).

## Downsides

- **macOS only.** At the time of writing, Conductor is available only for Mac.

## References

- [Conductor](https://www.conductor.build/)
- [Overview video (YouTube)](https://www.youtube.com/watch?v=VsWWy2kVpa8)
