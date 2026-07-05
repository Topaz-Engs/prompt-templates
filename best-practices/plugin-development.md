# Plugin Development Guide

## What is a Plugin?

A plugin is a distributable package that bundles AI agent customizations into a single installable unit. Think of it as "npm for agent configs." Plugins can contain:

| Component | File Pattern | Purpose |
|-----------|-------------|---------|
| **Skills** | `skills/*/SKILL.md` | Reusable instruction sets |
| **Hooks** | `hooks.json` or `hooks/` | Event handlers for agent lifecycle |
| **MCP Servers** | `.mcp.json` or `mcp.json` | Model Context Protocol integrations |
| **Custom Agents** | `agents/*.agent.md` | Pre-defined agent personas |
| **LSP Servers** | `lsp.json` | Language Server Protocol integrations |
| **Commands** | `commands/*.md` | Custom slash commands |

## Plugin Ecosystem (2026)

Multiple AI coding agents now support plugin systems:

| Platform | Plugin system | Install source |
|----------|--------------|----------------|
| **Claude Code** | `/plugin` command | Anthropic marketplace + community repos |
| **OpenCode** | `plugin` in `opencode.json` | npm packages + git repos |
| **GitHub Copilot** | `.github/plugins/` | GitHub marketplace |
| **Cursor** | Built-in extension system | Cursor marketplace |
| **Codex CLI** | `.agents/` directory | GitHub repos |

## Plugin Structure

```
my-plugin/
├── plugin.json              # Required manifest
├── README.md                # Documentation
├── skills/
│   ├── my-skill/
│   │   └── SKILL.md
│   └── another-skill/
│       └── SKILL.md
├── hooks/
│   ├── pre-tool-use.json
│   └── post-tool-use.json
├── mcp.json                 # MCP server configurations
├── agents/
│   └── my-agent.agent.md
└── commands/
    └── my-command.md
```

### plugin.json (Manifest)

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "Useful plugin for frontend development",
  "author": "your-name",
  "license": "MIT",
  "skills": ["my-skill", "another-skill"],
  "hooks": {
    "preToolUse": "hooks/pre-tool-use.json",
    "postToolUse": "hooks/post-tool-use.json"
  },
  "mcpServers": {
    "my-server": {
      "command": "npx",
      "args": ["-y", "my-server-package"]
    }
  },
  "agents": ["agents/my-agent.agent.md"],
  "commands": ["commands/my-command.md"],
  "minAppVersion": "2.1.0",
  "compatibility": ["claude-code", "opencode", "codex"]
}
```

## Hooks

Hooks allow plugins to intercept and modify agent behavior at specific lifecycle points.

### Pre-Tool-Use Hook

Runs before a tool is called. Can validate, modify, or block the call.

```json
{
  "name": "security-check",
  "description": "Validate tool calls before execution",
  "hookPoint": "preToolUse",
  "condition": {
    "toolName": ["bash", "write", "edit"]
  },
  "action": "validate",
  "rules": [
    {
      "pattern": "rm\\s+-rf",
      "response": "block",
      "message": "rm -rf is not allowed via agent"
    }
  ]
}
```

### Post-Tool-Use Hook

Runs after a tool returns. Can inspect, log, or transform results.

```json
{
  "name": "log-results",
  "description": "Log tool execution results",
  "hookPoint": "postToolUse",
  "action": "log",
  "fields": ["toolName", "duration", "success"]
}
```

## Distributing Plugins

### Method 1: Git Repository

Publish to GitHub and users install via:

```
/plugin marketplace add your-name/your-plugin
```

Or in OpenCode:

```json
{
  "plugin": ["your-plugin@git+https://github.com/your-name/your-plugin.git"]
}
```

### Method 2: npm Package

Package and publish to npm:

```json
// package.json
{
  "name": "your-plugin",
  "version": "1.0.0",
  "description": "Your plugin description",
  "main": "plugin.json",
  "files": ["skills/", "hooks/", "plugin.json", "README.md"]
}
```

Install via:

```json
{
  "plugin": ["your-plugin"]
}
```

### Method 3: Local Directory

For development:

```json
{
  "plugin": ["file:///path/to/your-plugin"]
}
```

## Plugin Design Principles

### 1. Focused Scope

Each plugin should solve one problem well. A "full-stack" plugin is too broad — split into frontend, backend, and DevOps plugins.

### 2. Progressive Enhancement

A plugin should work without configuration. Add optional settings for advanced use cases.

### 3. Good Defaults

Default behavior should be safe and reasonable. Users can override, but they shouldn't have to.

### 4. Clear Documentation

Every plugin needs a README explaining:
- What it does
- How to install
- What skills/agents it provides
- Configuration options
- Compatibility notes

### 5. Versioning

Semantic versioning for plugins:
- **Major** — breaking changes to existing skills or hooks
- **Minor** — new skills, hooks, or features
- **Patch** — fixes, documentation, non-breaking improvements

## Testing Plugins

### Local Development

```bash
# Symlink your plugin into the agent's plugin directory
ln -s /path/to/my-plugin ~/.claude/plugins/my-plugin

# Or test in OpenCode
make dev-link
```

### Validation Checklist

- [ ] `plugin.json` is valid JSON
- [ ] All referenced paths exist
- [ ] Skill descriptions are clear and specific
- [ ] MCP server commands are tested
- [ ] Hooks don't break normal agent behavior
- [ ] Plugin installs cleanly (no errors)
- [ ] Plugin uninstalls cleanly (no orphaned files)
- [ ] Works on clean environment (fresh install test)
- [ ] Documented minimum app version

## Cross-Platform Compatibility

Design plugins to work across multiple AI coding agents:

| Feature | Claude Code | OpenCode | Codex CLI | Copilot |
|---------|-------------|----------|-----------|---------|
| SKILL.md | Native | Native | Native | Supported |
| hooks.json | Supported | Planned | Not yet | Not yet |
| MCP config | Supported | Supported | Supported | Planned |
| Custom agents | Supported | Supported | Not yet | Not yet |

**Rule of thumb:** Skills (SKILL.md) are the most portable format. MCP servers are the second most portable. Hooks are the least portable.

## Popular Open-Source Plugins

### Obra Superpowers
A complete agentic development methodology plugin with composable skills:
- `brainstorming` — extract real specs before coding
- `test-driven-development` — strict Red-Green-Refactor cycle
- `writing-plans` — break features into 2-5 minute tasks
- `subagent-driven-development` — two-stage code review
- `systematic-debugging` — structured debugging workflow
- `using-git-worktrees` — parallel branch management
- `verification-before-completion` — quality gating

Install:
```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

### Vercel React Best Practices
React and Next.js performance rules from Vercel Engineering. Covers:
- Server Components vs Client Components usage
- Data fetching patterns
- Image optimization
- Bundle size management
- Caching strategies

## Plugin Marketplaces

| Marketplace | Plugins | Access Method |
|-------------|---------|---------------|
| **Anthropic Official** | Verified plugins | `/plugin` in Claude Code |
| **SkillsMP** | 2,000,000+ skills | Website + API |
| **Skill Marketplace AI** | 414,000+ skills | Website |
| **ClaudeSkill** | Curated market | `npx @claudeskill/install` |
| **LobeHub** | 100,000+ skills | `npx @lobehub/market-cli` |

## Anti-Patterns

- **Over-bundling** — packaging unrelated features into one plugin
- **Stale hooks** — hooks that trigger on every action with no conditions
- **Missing cleanup** — plugins that leave artifacts when uninstalled
- **Hardcoded paths** — using absolute paths that don't work on other systems
- **No versioning** — breaking changes without version bumps
- **Ignoring errors** — hooks that fail silently and hide agent issues
