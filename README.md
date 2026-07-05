# Prompt Templates

A curated collection of prompts, skills, and best practices for working effectively with AI coding agents.

## Structure

```
prompt-templates/
├── prompts/                  # Reusable prompt templates for common tasks
│   ├── code-review.md        # Systematic PR review with categorized feedback
│   ├── debugging.md          # Root cause analysis workflow
│   ├── test-generation.md    # Comprehensive test suite generation
│   ├── refactoring.md        # Behavior-preserving code improvement
│   ├── api-design.md         # REST/GraphQL API design with standards
│   ├── architecture-planning.md  # Structured architecture proposals
│   ├── security-audit.md     # OWASP-focused security review
│   ├── documentation.md      # Audience-aware technical documentation
│   ├── database-design.md    # Schema, migration, and query design
│   └── git-workflow.md       # Git operations and history management
├── skills/                   # Agent skill definitions (SKILL.md format)
│   ├── code-review-skill.md        # Code review automation
│   ├── debugging-skill.md          # Systematic debugging
│   ├── test-writing-skill.md       # Test generation
│   ├── refactoring-skill.md        # Code transformation
│   ├── security-review-skill.md    # Security auditing
│   ├── prompt-engineering-skill.md # CRTSE framework mastery
│   └── documentation-skill.md      # Technical writing
├── best-practices/           # Guides for effective AI collaboration
│   ├── working-with-agents.md      # End-to-end agent workflow guide
│   ├── prompt-engineering.md       # CRTSE framework and advanced techniques
│   └── skill-development.md        # Creating and maintaining agent skills
└── README.md
```

## Quick Start

1. **Browse `prompts/`** for copy-paste templates you can adapt to your task
2. **Read `best-practices/working-with-agents.md`** for the overall workflow
3. **Check `best-practices/prompt-engineering.md`** for the CRTSE framework
4. **Configure your project** with an `AGENTS.md` (see the guide for what to include)
5. **Define custom skills** following `best-practices/skill-development.md`

## Key Concepts

### CRTSE Framework
All prompts follow the same structure: **C**ontext, **R**ole, **T**ask, **S**tandards, **E**xamples. This ensures consistent, high-quality output regardless of the specific task.

### Spec-Driven Development
Write a spec before asking an agent to code. The more structured your input, the less time you spend fixing the output.

### Context Management
AI agents have finite context windows. Start fresh sessions for unrelated tasks and summarize progress between sessions.

## Contributing

1. Fork the repository
2. Add or improve prompts, skills, or best practices
3. Follow the existing format and conventions
4. Submit a pull request

## License

MIT
