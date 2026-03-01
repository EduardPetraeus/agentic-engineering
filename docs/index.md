# Agentic Engineering

> Your AI agent writes code 10x faster. Without governance, you ship technical debt 10x faster too.

**Agentic Engineering** is an open-source operating system for building software with AI agents. It provides the structure, governance, and workflows that turn AI code generation from a liability into a superpower.

Five repos. Three concerns. One coherent system.

---

## The Problem

AI coding agents are transformative. They generate code at speeds no human can match. But speed without structure creates chaos:

- **Solo developers** start a project with an AI agent, ship fast for a week, then lose track of what the agent changed, why it changed it, and what conventions it silently broke.
- **Teams** adopt AI agents independently. Each developer configures their agent differently. Code style diverges. Architecture decisions contradict each other.
- **Enterprises** face the hardest version: compliance teams cannot audit what an AI agent decided. Security teams cannot enforce boundaries on autonomous code generation.

The root cause is always the same: **AI agents operate without an operating system.** They have no constitution defining what they may do. No task system defining what they should do. No engineering standards defining how they should do it.

Agentic Engineering solves this.

---

## The Solution

The system is split across five repositories, each with a single responsibility:

```
+----------------------------------------------------------+
|           agentic-engineering (this repo)                 |
|    Docs, philosophy, getting started, architecture map   |
+----------------------------------------------------------+
|  governance  |  project-mgmt   |  engineering-standards   |
|  "what may   |  "what to do"   |  "how to do it"         |
|   agents do?"|                 |                         |
+----------------------------------------------------------+
|          ai-project-templates (scaffolder)                |
|    Instantiates all three into a new project repo        |
+----------------------------------------------------------+
```

**Governance** defines constitutional boundaries: what the AI agent is allowed to do, what it must never do, confidence ceilings, security protocols, and session lifecycle rules.

**Project Management** defines the work: YAML-based task definitions, priorities, dependencies, acceptance criteria.

**Engineering Standards** defines the how: code conventions, testing requirements, git workflow, review checklists, architecture patterns.

**Templates** wire it all together: a scaffolder that instantiates governance, project management, and engineering standards into any new project repo with a single command.

---

## Quick Start

### 1. Clone the ecosystem

```bash
git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

### 2. Scaffold a new project

```bash
cd ai-project-templates
python scaffold.py --name my-project --stack python-data --mode solo --output-dir ../
```

### 3. Start working with your AI agent

```bash
cd my-project
# Your CLAUDE.md is already configured.
# Open the project with Claude Code, Cursor, or any AI-assisted editor.
# The agent reads CLAUDE.md automatically and follows the governance rules.
```

That is it. Your AI agent now operates within a structured framework.

For the full walkthrough, see [Getting Started](getting-started.md).

---

## Repositories

| Repo | Purpose | Version | Link |
|---|---|---|---|
| **agentic-engineering** | Umbrella docs, philosophy, architecture | v1.0.0 | [GitHub](https://github.com/EduardPetraeus/agentic-engineering) |
| **ai-governance-framework** | Constitutional governance for AI agents | v0.3.0 | [GitHub](https://github.com/EduardPetraeus/ai-governance-framework) |
| **ai-project-management** | YAML-based task engine for AI workflows | v1.0.0 | [GitHub](https://github.com/EduardPetraeus/ai-project-management) |
| **ai-engineering-standards** | Code conventions, testing, git workflow | v1.0.0 | [GitHub](https://github.com/EduardPetraeus/ai-engineering-standards) |
| **ai-project-templates** | Project scaffolder wiring all three | v1.0.0 | [GitHub](https://github.com/EduardPetraeus/ai-project-templates) |

---

## Design Principles

1. **Separate files over monolithic configuration** — One file per concern. Small files are readable, diffable, and replaceable.
2. **Separation of concerns** — Governance, project management, and engineering standards are distinct responsibilities that evolve at different speeds.
3. **Templates are generic, instances are specific** — The framework repos provide templates with sensible defaults. Your project customizes them.
4. **Machine-readable over human-only** — YAML for structured data. Consistent metadata blocks. If the agent cannot parse it, it cannot follow it.
5. **Solo-first, team-ready** — Everything works for a single developer. The same structure scales to teams.
6. **Ship early, iterate in the open** — Version 0.1 ships incomplete. Early adopters shape the direction.

---

## Who Is This For

- **Solo developers using AI agents** — Ship fast without losing control.
- **Teams adopting AI-assisted development** — Consistent agent behavior across the entire team.
- **Engineering leads and platform teams** — Governance, audit trails, and compliance for AI-generated code.

---

## Documentation

| Document | Description |
|---|---|
| [Getting Started](getting-started.md) | Step-by-step setup guide |
| [Philosophy](philosophy.md) | Why agentic engineering exists |
| [Architecture](architecture.md) | How the five repos connect |
| [Multi-Agent Patterns](guides/multi-agent-patterns.md) | Running multiple AI agents in parallel |
| [Context Architecture](guides/context-architecture.md) | Managing context files for agents |
| [Repo Map](reference/repo-map.md) | All five repos documented |
| [Glossary](reference/glossary.md) | Key terms and definitions |
| [Quickstart Tutorial](quickstart-tutorial.md) | Hands-on: scaffold, configure, ship |
| [From Zero to v1.0](tutorials/from-zero-to-v1.md) | End-to-end tutorial |
| [Future Roadmap](future.md) | Vision for MCP, SDK, and enterprise features |

---

## Contributing

This is an open-source project built in the open. See [Contributing](https://github.com/EduardPetraeus/agentic-engineering/blob/main/CONTRIBUTING.md) for guidelines.

---

## License

MIT License. See [LICENSE](https://github.com/EduardPetraeus/agentic-engineering/blob/main/LICENSE).

---

*Built by [Eduard Petraeus](https://github.com/EduardPetraeus) — shipping governance for the agentic age.*
