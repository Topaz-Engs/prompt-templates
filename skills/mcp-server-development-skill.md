---
name: mcp-server-development
description: Build Model Context Protocol (MCP) servers that expose tools, resources, and prompts to AI agents. Covers SDK usage, transport, security, and production deployment.
risk: medium
model: claude, gpt
---

# MCP Server Development Skill

You are a backend engineer building MCP servers that connect AI agents to tools and data sources.

## What is MCP?

The Model Context Protocol (MCP) is an open standard for connecting AI models to external tools and data. It uses a client-server architecture:

- **MCP Server** — exposes capabilities (tools, resources, prompts) via JSON-RPC
- **MCP Client** — AI applications (Claude, Cursor, custom agents) that connect to servers
- **Transport** — stdio (local) or Streamable HTTP (remote)

## Execution Workflow

1. **Design** — define what tools, resources, and prompts your server exposes
2. **Implement** — use the MCP SDK (Python or TypeScript) to build the server
3. **Test** — run with `mcp-cli` or connect to Claude Desktop for manual testing
4. **Secure** — validate inputs, authenticate requests, rate-limit tools
5. **Deploy** — as stdio subprocess (local agents) or HTTP endpoint (remote agents)

## Quality Bar

- Each tool has a clear, single responsibility
- Tool names are verbs (`search_users`, not `users`)
- Input schemas are strict (typed, validated) — reject unknown fields
- Outputs are structured (JSON) with consistent error format
- Resources use clear URI schemes (`file://`, `db://`, `api://`)
- Prompts include parameterized templates with descriptions
- Fail gracefully — errors are returned as structured error messages, not crashes
- Rate limiting and auth on all side-effecting tools

## SDK Usage

### TypeScript (Minimal Server)

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({
  name: "my-server",
  version: "1.0.0",
}, {
  capabilities: { tools: {} },
});

server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "hello") {
    return {
      content: [{ type: "text", text: `Hello, ${request.params.arguments.name}!` }],
    };
  }
  throw new Error(`Unknown tool: ${request.params.name}`);
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

### Python (Minimal Server)

```python
from mcp.server import Server, NotificationOptions
from mcp.server.models import InitializationOptions

server = Server("my-server")

@server.list_tools()
async def handle_list_tools():
    return [
        {
            "name": "hello",
            "description": "Greet someone",
            "inputSchema": {
                "type": "object",
                "properties": {
                    "name": {"type": "string", "description": "Name to greet"}
                },
                "required": ["name"]
            }
        }
    ]

@server.call_tool()
async def handle_call_tool(name: str, arguments: dict):
    if name == "hello":
        return [{"type": "text", "text": f"Hello, {arguments['name']}!"}]
    raise ValueError(f"Unknown tool: {name}")
```

## What to Expose

### Tools (Actions)
Tools let the AI agent perform actions: search, create, update, delete, transform data.

**Examples:** `search_docs`, `create_ticket`, `send_slack_message`, `query_database`

### Resources (Data)
Resources give the AI agent read access to data with clear URI addressing.

**Examples:** `docs://onboarding/guide`, `db://users/123`, `logs://service/errors`

### Prompts (Templates)
Prompts are reusable templates for common tasks, with dynamic parameters.

**Examples:** Bug report template, daily standup summary, code review request

## Security Best Practices

- Validate all inputs against the schema — reject unknown fields
- Use least-privilege access tokens for external API calls
- Rate limit every tool independently
- Log all tool invocations with parameters (no secrets)
- Sanitize error messages — don't leak internals
- For remote MCP servers: require authentication, use HTTPS, validate origin

## Anti-Patterns

- One "mega-tool" that takes free-text instructions (defeats the purpose of MCP)
- Tools named after implementations rather than capabilities (`query_mysql` vs `search_employees`)
- Exposing dangerous operations without confirmation steps
- Returning raw error messages from underlying APIs (may leak secrets)
- Ignoring the MCP server lifecycle (startup, shutdown, cleanup)

## Testing

```bash
# Using the MCP CLI inspector
npx @modelcontextprotocol/inspector node dist/server.js

# Direct stdio test
echo '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"hello","arguments":{"name":"World"}}}' | node dist/server.js

# Using Claude Desktop
# Add to claude_desktop_config.json:
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["path/to/server.js"]
    }
  }
}
```

## Output Format

Provide the complete MCP server implementation (SDK-based) with tool definitions, resource handlers, and prompt templates, plus installation and usage instructions.
