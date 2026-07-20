---
title: "What is MCP (Model Context Protocol)?"
parent: Workflows
nav_order: 2
description: "A short explainer of tool-calling and how MCP standardizes it for connecting LLMs to external tools and data."
layout: minimal
---

# What is MCP (Model Context Protocol)?

This section briefly explains what MCP is and why it's useful. Before describing MCP, it
helps to understand **tool-calling** first — MCP is built on top of it.

## Tool-calling

Tool-calling is a capability that lets an AI model (like an LLM) interact with the outside
world by invoking external functions, APIs, or data.

Here's a simple example of tool-calling in Python with two tools, `bash_tool` and
`web_search`:

```python
# mock tools
tools = {
    "bash_tool": lambda args: "index.html  main.py  styles.css  README.md" if args["command"] == "ls" else "Done",
    "web_search": lambda args: f"Search results for '{args['query']}': Found documentation."
}

# Simulates the AI picking a tool based on keywords
def mock_llm(query):
    if "file" in query or "list" in query:
        return {"tool": "bash_tool", "arguments": {"command": "ls"}}
    return {"tool": "web_search", "arguments": {"query": query}}


while True:
    user_query = input("User: > ")
    if user_query.lower() in ["exit", "quit"]:
        print("Goodbye!")
        break

    # Step 1: Get tool choice from LLM
    decision = mock_llm(user_query)
    tool_name, args = decision["tool"], decision["arguments"]
    print(f"AI wants to call: {tool_name}({args})")

    # Step 2 & 3: execute the tool and print the output
    tool_output = tools[tool_name](args)
    print(f"Tool Output: {tool_output}\n")
```

That's tool-calling in a nutshell: the model picks a tool, supplies arguments, and reads
back the result. So what does MCP add?

## What is MCP?

MCP is a **standardized wrapper around tool-calling** that makes it easier to connect
external APIs, data, and documentation without writing custom integration code for each
one.

Without MCP, every LLM provider needs its own custom tool definitions for each external
API or database. Suppose Claude, OpenAI, and Qwen all want to use the `web_search` tool
above. One might implement it as `web_search(query: str)`, another as
`web_search(query: str, number_of_return_pages: int)` — so each model sees a different
signature and observes different results. That fragmentation introduces a lot of
integration headaches.

MCP standardizes this. Instead of a separate integration per provider, you expose **one
official tool behind an MCP server** that every LLM provider can call the same way.

Many organizations already publish MCP servers, for example:

- [GitHub MCP server — tools](https://github.com/github/github-mcp-server#tools)
- [Notion MCP — supported tools](https://developers.notion.com/guides/mcp/mcp-supported-tools#mcp-tools)

## How do LLMs use MCP?

A good way to understand how an LLM uses MCP is to look at a
[leaked system prompt](https://github.com/asgeirtj/system_prompts_leaks/blob/main/Anthropic/claude-fable-5.md).
It states:

```text
Claude can connect to external apps and services on behalf of the person through MCP
Apps... Claude should check its tool list rather than assume. MCP App tools are identified
by descriptions that begin with the tag [third_party_mcp_app].
```

The system prompt describes the flow clearly: the model first checks which MCP servers are
available and relevant; if it finds a relevant one, it inspects the tools it contains via
their descriptions; and finally it makes a tool request through the relevant tool.

Under the hood there are additional tricks. For example, Claude Code and Codex load only
the MCP servers a user has already activated (Claude Code stores these in `.claude.json`)
rather than blindly loading everything.

## Other advantages of MCP

Beyond solving the integration problem, MCP also addresses:

- **Dynamic Client Registration (DCR).** MCP supports DCR, letting clients register
  automatically with OAuth servers. This removes the need for manual client setup or
  hard-coded credentials, streamlining deployment.
- **Secure authorization and token management.** Clients securely obtain OAuth tokens
  scoped precisely to a user's permissions, so they access only the resources the user has
  explicitly permitted — improving security and compliance, especially in multi-user and
  cloud environments.

## References

- [Stytch — An introduction to the Model Context Protocol](https://stytch.com/blog/model-context-protocol-introduction/)
- [GitHub — asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks)
- [GitHub MCP server](https://github.com/github/github-mcp-server#tools)
- [Notion MCP supported tools](https://developers.notion.com/guides/mcp/mcp-supported-tools#mcp-tools)
