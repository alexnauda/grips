# GRIPS Methodology

**Guided Requirements Implementation Planning System**

A methodology for spec-driven development with AI agents that maintains living, synchronized documentation across all project layers.

## What is GRIPS?

GRIPS is a systematic approach to building software with AI agents that keeps your requirements, design, implementation plans, and code in perfect sync. Think of it like the CASE tools of the 1990s, but adapted for modern AI-powered development workflows.

**Key Features:**
- **Living Documentation**: All specs stay current—no stale docs or drift
- **Bidirectional Sync**: Changes flow up and down through requirements → design → implementation → code
- **Structured Q&A**: Major decisions documented through agent-led Q&A sessions
- **Phase-Based Development**: Clear progression through prototype → MVP → expansions
- **Agent-Guided**: AI agents draft, humans review and approve
- **Critic Markup Review**: Humans provide feedback using standardized markup that agents understand

## When to Use GRIPS

**GRIPS is designed for:**
- New projects starting from scratch (greenfield development)
- Teams using AI agents (like Claude Code) for development
- Projects requiring clear specification-to-implementation traceability
- Long-lived projects where documentation drift is a concern

**Current limitations:**
- Not yet designed for adding to existing codebases (deferred to future versions)

## Prerequisites

**Critic Markup Support**: GRIPS uses [Critic Markup](http://criticmarkup.com/) for human review and feedback on agent-drafted documents. You'll need a text editor or tool that supports Critic Markup syntax to provide feedback on requirements, design documents, and implementation plans.

Popular options:
- Obsidian (macOS) with Sidebar Highlights community plugin (and we recommend the setting to default to inline comments)
- VS Code with Critic Markup extensions
- Sublime Text with CriticMarkup package
- MultiMarkdown Composer (macOS)
- Any editor with Critic Markup preview support

## Quick Start

### 1. Install the Methodology

Download the latest methodology from the repository:

```bash
mkdir -p ~/.grips
curl -o ~/.grips/methodology.md https://raw.githubusercontent.com/alexnauda/grips/main/methodology.md
```

### 2. Reference it in Your Project

Create `methodology.md` in your project root:

```markdown
# GRIPS Methodology

This project uses the GRIPS methodology.

For more information, visit: https://github.com/alexnauda/grips

@~/.grips/methodology.md
```

Add `@methodology.md` to your project's `CLAUDE.md` or agent configuration file.

### 3. Start Working

Tell your AI agent to set up GRIPS and begin your first requirements Q&A session. The agent will create all necessary directory structures, templates, and files according to the methodology.

## Project Structure

Every GRIPS project follows this structure:

```
project-root/
├── methodology.md              # Points to GRIPS version
├── PROJECT-STATUS.md           # Current phase, stage, active work
├── specs/
│   ├── requirements/
│   │   ├── requirements.md     # Main requirements document
│   │   ├── qa/                 # Q&A sessions
│   │   └── adr/                # Architecture Decision Records
│   ├── design/
│   │   ├── design.md           # Main design document
│   │   ├── qa/
│   │   └── adr/
│   └── implementation-plan/
│       ├── implementation-plan.md
│       ├── qa/
│       └── adr/
└── src/                        # Your actual code
```

## Documentation

- **[methodology.md](methodology.md)** - Complete GRIPS methodology (current: v0.0.2-dev)
- **[releases/](releases/)** - Stable released versions

## Version Pinning

Projects pin to a specific GRIPS methodology version and stay on that version indefinitely. This ensures your development process remains stable and predictable. Methodology upgrade paths will be defined in future versions.

## Current Status

**Latest Stable Release**: v0.0.1
**Development Version**: v0.0.2-dev

This project is in active development. The methodology is being refined through real-world use and structured Q&A sessions.

## License

MIT License - Copyright (c) 2026 Alex Nauda

See [LICENSE](LICENSE) for details.

## Contributing

This methodology is being developed in the open. The best way to contribute is to:
1. Try GRIPS on your own projects
2. Document what works and what doesn't
3. Share your experiences and suggestions

## Learn More

For detailed information on:
- Layer propagation and synchronization
- Q&A session format and process
- Implementation plan structure
- Phase management and versioning

Read the complete [methodology.md](methodology.md) document.
