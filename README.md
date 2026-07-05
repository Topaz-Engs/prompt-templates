# Prompt Templates

A curated collection of prompts, skills, best practices, MCP server references, and plugin development guides for working effectively with AI coding agents.

Built from 2026 industry research — including the CRTSE framework, the SKILL.md open standard, the Model Context Protocol (MCP), and the broader AI agent plugin ecosystem.

## Structure

```
prompt-templates/
├── prompts/                         # Reusable prompt templates
│   ├── code-review.md               # Systematic PR review with categorized feedback
│   ├── debugging.md                 # Root cause analysis workflow
│   ├── test-generation.md           # Comprehensive test suite generation
│   ├── refactoring.md               # Behavior-preserving code improvement
│   ├── api-design.md                # REST/GraphQL API design with standards
│   ├── architecture-planning.md     # Structured architecture proposals
│   ├── security-audit.md            # OWASP-focused security review
│   ├── documentation.md             # Audience-aware technical documentation
│   ├── database-design.md           # Schema, migration, and query design
│   ├── git-workflow.md              # Git operations and history management
│   ├── ci-cd-pipeline.md            # CI/CD pipeline design
│   ├── code-migration.md            # Safe incremental code migrations
│   ├── performance-optimization.md  # Profile-driven performance tuning
│   ├── code-explanation.md          # Clear code walkthroughs for any audience
│   ├── pr-commit-messages.md        # Conventional commits and PR descriptions
│   ├── code-conversion.md           # Idiomatic cross-language translation
│   └── dependency-audit.md          # Security and health checks
├── skills/                          # Agent skill definitions (SKILL.md)
│   ├── code-review-skill.md              # Code review automation
│   ├── debugging-skill.md                # Systematic debugging
│   ├── test-writing-skill.md             # Test generation
│   ├── refactoring-skill.md              # Code transformation
│   ├── security-review-skill.md          # Security auditing
│   ├── prompt-engineering-skill.md       # CRTSE framework mastery
│   ├── documentation-skill.md            # Technical writing
│   ├── ci-cd-pipeline-skill.md           # Pipeline design and debugging
│   ├── code-migration-skill.md           # Safe migration execution
│   ├── performance-optimization-skill.md # Systematic profiling and tuning
│   ├── mcp-server-development-skill.md   # MCP server implementation
│   └── prompt-injection-defense-skill.md # LLM security
├── best-practices/                  # Guides for effective AI collaboration
│   ├── working-with-agents.md       # End-to-end agent workflow guide
│   ├── prompt-engineering.md        # CRTSE framework and advanced techniques
│   ├── prompt-security.md           # Prompt injection attacks & defenses
│   ├── skill-development.md         # Creating and maintaining agent skills
│   ├── mcp-servers.md               # MCP ecosystem and server selection
│   └── plugin-development.md        # Building and distributing plugins
├── examples/                         # Concrete prompt examples
│   ├── good-prompts.md              # 7 filled-in CRTSE examples for real scenarios
│   └── bad-prompts.md               # 10 bad prompts with before/after fixes
├── SCHEMA.md                         # Formal schema for prompts, skills, plugins, hooks
└── README.md
```

## Quick Start

1. **Browse `prompts/`** for copy-paste templates adapted to your specific task
2. **Read `best-practices/working-with-agents.md`** for the overall workflow methodology
3. **Configure your project** with an `AGENTS.md` or `CLAUDE.md` (see the guide)
4. **Install MCP servers** from `best-practices/mcp-servers.md` to connect agents to your tools
5. **Define custom skills** following `best-practices/skill-development.md`
6. **Package as plugins** following `best-practices/plugin-development.md` for team distribution

## Key Concepts

### CRTSE Framework
All prompts follow **C**ontext, **R**ole, **T**ask, **S**tandards, **E**xamples. This structure ensures consistently high-quality output.

### SKILL.md Open Standard
Skills are portable Markdown instruction files with YAML frontmatter. Write once, use across Claude Code, OpenCode, Codex CLI, Cursor, Copilot, and Gemini CLI.

### Model Context Protocol (MCP)
MCP is the industry standard (Linux Foundation governed) for connecting AI agents to external tools. See `best-practices/mcp-servers.md` for the top 30+ servers and installation guides.

### Plugin Ecosystem
Plugins bundle skills, hooks, MCP configs, and agents into installable packages. See `best-practices/plugin-development.md` for the development workflow.

## Sources

This repository is informed by research into:
- Top GitHub prompt libraries (Awesome ChatGPT Prompts, DAIR.AI, Fabric, Anthropic's tutorials)
- Skill marketplaces (SkillsMP with 2M+ skills, ClaudeSkill, Skill Marketplace AI, LobeHub)
- Claude Code and OpenCode plugin ecosystems
- The MCP server ecosystem (9,400+ servers, modelcontextprotocol/servers at 84k stars)
- Obra Superpowers, Vercel React Best Practices, and other notable open-source plugins

## License

MIT
