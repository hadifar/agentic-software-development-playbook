---
title: Workflows
nav_order: 3
description: "Common patterns and best practices for working with coding agents."
---

# Workflows & patterns

## The basic loop

1. **Describe intent** — explain the task, not the implementation. Give context: constraints, style, related files.
2. **Let the agent plan** — for anything non-trivial, ask for a plan before code changes. Review and adjust the plan.
3. **Generate** — the agent writes/edits code, possibly running tests or commands along the way.
4. **Review the diff** — read every change before accepting. Treat agent output like a PR from a fast, occasionally-wrong teammate.
5. **Iterate** — correct misunderstandings conversationally rather than doing the fix yourself; this improves the agent's context for next time.

## Guardrails worth having

- **Tests** — a test suite lets the agent verify its own work and catch regressions.
- **Linting/type-checking** — fast, automatic feedback the agent can self-correct against.
- **Small, reviewable diffs** — prefer several focused changes over one giant one.
- **Sandboxing** — run agents with restricted permissions/dangerous-command confirmation, especially for file deletion, git force-push, or shell access.
- **Version control discipline** — commit before large agentic edits so you can always roll back.

## Patterns worth naming

- **Plan → Act → Review** — explicit planning step before code generation (many tools now support a "plan mode").
- **Spec-first** — write a short spec/requirements doc, then have the agent implement against it.
- **Subagents/parallel agents** — delegate independent subtasks (e.g., research vs. implementation) to separate agent runs to keep context focused.
- **Human-in-the-loop checkpoints** — require explicit approval before risky actions (pushes, deletes, deployments).

## Anti-patterns to avoid

- Accepting large diffs without reading them.
- Letting an agent run destructive commands unsupervised.
- Treating agent output as authoritative without tests or review.
- Over-specifying implementation details instead of intent (defeats the point of delegation).
