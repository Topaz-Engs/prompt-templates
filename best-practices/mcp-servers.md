# MCP Servers Guide

## What is MCP?

The Model Context Protocol (MCP) is an open standard for connecting AI agents to external tools and data sources. It was created by Anthropic in 2024 and is now governed by the Linux Foundation's Agentic AI Foundation.

MCP uses a client-server architecture:
- **MCP Servers** expose capabilities (tools, resources, prompts) to AI agents
- **MCP Clients** (Claude Desktop, Cursor, custom agents) connect to servers
- **Transport** is stdio (local subprocess) or Streamable HTTP (remote)

Think of MCP as **USB-C for AI** — one universal connector instead of custom integrations for every AI + tool combination.

## Why MCP Matters

Before MCP, connecting AI to tools required custom code for each pairing:
- 5 AI apps × 10 tools = 50 custom integrations
- Adding one new tool = 5 more integrations

With MCP:
- 5 AI apps + 10 tools = 15 protocol connections
- Adding one new tool = 1 server implementation

The ecosystem has grown to **9,400+ public MCP servers** with **97 million+ monthly SDK downloads** as of mid-2026.

## Top MCP Servers (2026)

### Official Reference Servers (modelcontextprotocol/servers)

| Server | Purpose | Stars |
|--------|---------|-------|
| **Filesystem** | Secure file operations with configurable access controls | 84k+ |
| **GitHub** | Repository management, PRs, issues, file operations | 84k+ |
| **Fetch** | Web content fetching and conversion for LLM usage | 84k+ |
| **Memory** | Knowledge graph-based persistent memory for agents | 84k+ |
| **PostgreSQL** | Read-only database access with schema inspection | 84k+ |
| **SQLite** | Database interaction and business intelligence | 84k+ |
| **Puppeteer** | Browser automation and web scraping | 84k+ |
| **Brave Search** | Web and local search via Brave Search API | 84k+ |
| **Git** | Read, search, and manipulate Git repositories | 84k+ |
| **Slack** | Channel management and messaging | 84k+ |
| **Google Drive** | File access and search for Google Drive | 84k+ |
| **Google Maps** | Location services, directions, place details | 84k+ |
| **Redis** | Interact with Redis key-value stores | 84k+ |
| **Sentry** | Retrieve and analyze issues from Sentry.io | 84k+ |
| **Time** | Time and timezone conversion | 84k+ |

### Popular Third-Party Servers

| Server | Purpose | Stars |
|--------|---------|-------|
| **markitdown** (Microsoft) | Convert PDF, Office, HTML to Markdown | 119k+ |
| **netdata** | Real-time infrastructure monitoring | 78k+ |
| **Playwright** | Browser automation (Puppeteer alternative) | 45k+ |
| **Cloudflare** | Manage Cloudflare resources | 30k+ |
| **Docker** | Container management | 28k+ |
| **Kubernetes** | K8s cluster management | 25k+ |
| **Stripe** | Payment processing integration | 24k+ |
| **Jira** | Issue and project management | 22k+ |
| **Notion** | Wiki and document access | 22k+ |
| **Supabase** | Database and auth management | 18k+ |
| **Linear** | Issue tracking | 16k+ |
| **Pinterest** | Image generation and search | 15k+ |
| **YouTube** | Video transcription and search | 14k+ |
| **Cloudinary** | Media management | 12k+ |
| **Airtable** | Spreadsheet-database access | 10k+ |

### Data & Storage

- **PostgreSQL** — read-only DB access with schema inspection
- **SQLite** — DB interaction and BI capabilities
- **Redis** — key-value store interaction
- **Elasticsearch** — full-text search and analytics
- **DuckDB** — embedded analytical SQL
- **Chroma** — vector database for embeddings

### Developer Tools

- **GitHub** — full API integration (issues, PRs, repos, files)
- **GitLab** — project management and CI
- **Docker** — container lifecycle management
- **Kubernetes** — cluster operations
- **Sentry** — error tracking and monitoring
- **Datadog** — observability and metrics

### Communication

- **Slack** — messaging and channel management
- **Discord** — community platform integration
- **Jira** — issue tracking
- **Linear** — project management
- **Notion** — documentation and wikis
- **Confluence** — enterprise documentation

### AI & Media

- **markitdown** — document to Markdown conversion
- **Puppeteer** / **Playwright** — browser automation
- **Brave Search** — web search
- **EverArt** — AI image generation
- **Stability AI** — image generation and editing

## How to Install MCP Servers

### For Claude Desktop

Edit `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/allowed/dir"
      ]
    },
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_TOKEN": "ghp_..."
      }
    }
  }
}
```

### For OpenCode

Add to `opencode.json`:

```json
{
  "mcp": {
    "servers": {
      "filesystem": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
      }
    }
  }
}
```

### For Cursor

Configure in Cursor Settings → MCP Servers → Add Server.

### For VS Code

Use the MCP extension or configure in VS Code settings.

## Choosing MCP Servers

### Selection Criteria

1. **Maintenance** — recently updated, active issue resolution
2. **Documentation** — clear setup instructions, examples, input schemas
3. **Security** — proper input validation, least-privilege design
4. **Performance** — low overhead, handles concurrent requests
5. **Compatibility** — works with your AI agent/client of choice

### Rule of thumb

- Install servers **per project** (in `.mcp.json`) for team sharing
- Install servers **globally** only for personal utilities you use across all projects
- Use **stdio transport** for local, private data (filesystem, local DB)
- Use **HTTP transport** for shared services (Slack, Jira, GitHub)

## MCP vs Skills vs Plugins

| Concept | Purpose | Format |
|---------|---------|--------|
| **MCP Server** | Connects AI agents to external tools/APIs | JSON-RPC service |
| **Skill** | Teaches an agent how to perform a task | `SKILL.md` file |
| **Plugin** | Packages skills + hooks + MCP configs into one installable bundle | Directory with `plugin.json` |

They work together: a plugin can bundle skills that teach the agent how to use an MCP server effectively.

## Resources

- [MCP Specification](https://spec.modelcontextprotocol.io)
- [MCP GitHub](https://github.com/modelcontextprotocol)
- [MCP Servers Registry](https://mcpservers.org)
- [Awesome MCP Tools](https://awesome-mcp.tools)
- [OpenCode MCP Docs](https://opencode.ai/docs/mcp)
