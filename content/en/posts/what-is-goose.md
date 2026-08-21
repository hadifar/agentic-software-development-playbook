---
title: "goose: open-source AI agent harness"
date: 2026-08-03
categories: [Tools]
draft: false
slug: what-is-goose
---

[goose](https://goose-docs.ai/) is an open-source AI agent that wraps an LLM in a loop of
tool calls, so the model can actually do things rather than only describe them. Its own
docs put it this way:

> goose, an open source AI Agent, builds upon the basic interaction framework of Large
> Language Models (LLMs), which primarily functions as a text-based conversational
> interface. It processes text input and generates text output. This "text in, text out"
> approach is enhanced with tool integrations, which allows the AI agent to complete tasks,
> creating goose.

In other words, goose is a harness: the layer that sits between you and the model, carrying
requests out to tools and results back in.

## How it works

1. **Human request.** The loop starts and ends with you — a question, command, or problem
   to solve.
2. **Provider chat.** goose sends your request along with the list of available tools to
   the LLM provider you've connected. The provider processes it and, if needed, emits a
   tool call as part of its response.
3. **Extension call.** The model can *request* a tool call but not execute it — that's
   goose's job. It takes the JSON-formatted tool call, runs it, and collects the results.
4. **Response to model.** goose sends the results back to the model. If more extensions are
   needed, these steps repeat.
5. **Context revision.** goose drops old or irrelevant information so the model stays
   focused on what matters, which also keeps token usage in check.
6. **Model response.** Once the tool calls are done, the model sends its final response back
   to you, and the loop restarts when you reply.

## Interesting features

The context-engineering side of goose is, in my opinion, particularly well designed.

**Split providers for planning and coding.** You can point planning at one provider and
code generation at another via configuration variables:

- `GOOSE_PLANNER_PROVIDER` — which provider to use for planning
- `GOOSE_PLANNER_MODEL` — which model to use for planning

Recent models are converging on both capabilities, so you may not need two, but it's a nice
lever to have.

**Subagents.** You can ask goose to spawn subagents and delegate work — *"create the login
and logout page in parallel with two agents"*. Agent creation can be constrained with
[Recipes](https://goose-docs.ai/docs/guides/recipes/): YAML files specifying the system
prompt, inputs, and a timeout after which an agent that hasn't finished is shut down.

**Scheduled agents.** goose has built-in scheduling — essentially cron for agent runs. It's
a minor feature, but a genuinely useful one that other harnesses like Claude Code and Codex
don't offer out of the box.

**Pre-built extensions.** Extensions are add-ons that connect goose to the applications and
tools already in your workflow — adding features, accessing data, or integrating with other
systems. They're built on the Model Context Protocol (see
[What is MCP?](/posts/what-is-mcp/)), so goose plugs
into a wide ecosystem of existing capabilities.

## References

- [goose documentation](https://goose-docs.ai/)
- [goose Recipes](https://goose-docs.ai/docs/guides/recipes/)
