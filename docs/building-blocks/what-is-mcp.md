---
title: "What is MCP (Model Context Protocol)?"
parent: Building Blocks
nav_order: 1
description: "A short explainer of tool-calling and how MCP standardizes it for connecting LLMs to external tools and data."
layout: minimal
---

# What is MCP (Model Context Protocol)?

This section briefly explains what MCP is and why it's useful. Before describing MCP, it
helps to understand **tool-calling** first — MCP is built on top of it.

## Tool-calling

Tool-calling is a capability that lets an AI model (like an LLM) interact with the outside
world by invoking external functions or APIs.

Here's a simple example of tool-calling in Python with two tools, `bash_tool` and
`web_search`:

```python
# mock tools
def bash_tool(command: str) -> str:
    """Run a shell command and return its output."""
    return "index.html  main.py  styles.css  README.md" if command == "ls" else "Done"


def web_search(query: str) -> str:
    """Search the web and return the results."""
    return f"Search results for '{query}': Found documentation."


tools = {"bash_tool": bash_tool, "web_search": web_search}

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
    tool_output = tools[tool_name](**args)
    print(f"Tool Output: {tool_output}\n")
```

That's tool-calling in a nutshell: the model picks a tool (e.g. `web_search` or
`bash_tool`) and supplies the right arguments (e.g. `query: "who won the 2022 World
Cup"`), the tool or API is executed — locally or remotely — and the model reads back the
result.

## What is MCP?

MCP is a **standardized wrapper around tool-calling** that makes it easier to connect
external APIs, data, and documentation without writing custom integration code for each
one.

Without MCP, every LLM provider or AI developer needs their own custom tool definitions
for each external API or database. Suppose Claude, OpenAI, and Qwen all want to use the `web_search` tool
above (in the real world, say they all want to use the Google Search API). One might
implement it as `web_search(query: str)`, another as
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

The system prompt describes the flow:

1. The model first checks which MCP servers are available and relevant.
2. If it finds a relevant one, it inspects the tools it contains via their descriptions.
3. Finally it makes a tool request through the relevant tool.

More specifically, the exchange between the user's session, the server, and the LLM goes
as follows:

1. The client (your Claude session) calls `tools/list` on the MCP server ("what can you do?").
2. The server returns JSON describing each tool (name, summary, JSON schema).
3. The host (Claude Code, Codex) injects that JSON into the model's context.
4. A user prompt triggers the model, which emits a structured tool call.
5. The MCP server executes it and the conversation resumes.

Under the hood there are additional tricks. For example, Claude Code and Codex load only
the MCP servers a user has already activated (Claude Code stores these in `.claude.json`)
rather than blindly loading everything.

## Writing your own MCP server

### The easy way

Ask Claude to do it for you. First install the `mcp-server-dev` plugin from Claude Code:

```text
/plugin marketplace add anthropics/claude-plugins-official
/plugin install mcp-server-dev
```

With its skills installed, ask Claude to build the server for you — for example:
`help me build an MCP server with two tools, web_search and bash_tool`.

### Doing it yourself

Here are the same two tools from the tool-calling example above, this time exposed through
an MCP server using the Python SDK's `FastMCP` helper:

```python
# my_mcp_server.py

from mcp.server.fastmcp import FastMCP

# Initialize FastMCP server
mcp = FastMCP("mymcp")


@mcp.tool()
def bash_tool(command: str) -> str:
    """Run a shell command and return its output."""
    ...

@mcp.tool()
def web_search(query: str) -> str:
    """Search the web and return the results."""
    ...

def main():
    mcp.run(transport="http", host="0.0.0.0", port=3000)

if __name__ == "__main__":
    main()

# run via: python my_mcp_server.py
```

The `@mcp.tool()` decorator is what turns a plain Python function into a tool the server
advertises — its signature and docstring become the description an MCP client discovers, so
any client can then call both tools the same way.

You can then inspect the tools with the MCP inspector:

```bash
npx @modelcontextprotocol/inspector
# → select "Streamable HTTP", paste http://localhost:3000/mcp as the URL, click Connect
```

The example above is deliberately minimal. A real server needs decisions this snippet skips,
and [Anthropic's official example](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/mcp-server-dev/skills/build-mcp-server/SKILL.md)
frames them as five questions:

- **What does it connect to?** Decides the deployment shape — remote HTTP as in the example
  above, a bundled MCPB, or local stdio.
- **Who will use it?** A single team, broad public distribution, and Claude desktop users
  wanting UI features each point somewhere different.
- **How many distinct actions does it expose?** A handful is fine as one tool per action; a
  large surface is better served by a search-plus-execute pair, so the model isn't handed
  dozens of near-identical tools.
- **Does a tool need mid-call user input or rich display?** Plain tools cover most cases;
  otherwise you reach for elicitation or MCP app widgets.
- **What auth does the upstream service use?** An API key is trivial; OAuth, CIMD, or DCR
  flows are where most of the real complexity lands.

## Other advantages of MCP

Beyond solving the integration problem, MCP also addresses:

- **Dynamic Client Registration (DCR).** MCP supports DCR, letting clients register
  automatically with OAuth servers. This removes the need for manual client setup or
  hard-coded credentials, streamlining deployment.
- **Secure authorization and token management.** Clients securely obtain OAuth tokens
  scoped precisely to a user's permissions, so they access only the resources the user has
  explicitly permitted — improving security and compliance, especially in multi-user and
  cloud environments.

## Current limitations

MCP is still young, and a few rough edges show up quickly in practice:

- **Tool definitions eat tokens.** Every connected server's tools are injected into the
  model's context, so a handful of servers can consume a large slice of the prompt before
  the user has typed anything — and you pay for it on every turn. Progressive discovery and
  tool RAG (retrieving only the tools relevant to the current request) reduce the cost, but
  don't eliminate it.
- **Tool composition is an open problem.** Picking a single tool is mostly solved; chaining
  several together to reach a desired result is not. Models still struggle to plan which
  tools to combine, in what order, and how to thread one tool's output into the next.
- **Tool descriptions matter more than you'd expect.** You can't simply hand over an
  existing OpenAPI/Swagger spec and call it an MCP server. A spec written for developers —
  who bring the surrounding docs, conventions, and intent — reads very differently to a
  model, which only sees the names, descriptions, and schemas in front of it. Designing an
  MCP server means deliberately writing tool signatures and descriptions for that reader.

## References

- [Model Context Protocol — Getting started](https://modelcontextprotocol.io/docs/getting-started/intro)
- [Anthropic — build-mcp-server skill](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/mcp-server-dev/skills/build-mcp-server/SKILL.md)
- [Stytch — An introduction to the Model Context Protocol](https://stytch.com/blog/model-context-protocol-introduction/)
- [GitHub — asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks)
- [GitHub MCP server](https://github.com/github/github-mcp-server#tools)
- [Notion MCP supported tools](https://developers.notion.com/guides/mcp/mcp-supported-tools#mcp-tools)
