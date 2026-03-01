# Agentic Engineering — An Operating System for AI-Assisted Software Development

> Your AI agent writes code 10x faster. Without governance, you ship technical debt 10x faster too.

**Agentic Engineering** is an open-source operating system for building software with AI agents. It provides the structure, governance, and workflows that turn AI code generation from a liability into a superpower.

Five repos. Three concerns. One coherent system.

---

## The Problem

AI coding agents are transformative. They generate code at speeds no human can match. But speed without structure creates chaos:

- **Solo developers** start a project with an AI agent, ship fast for a week, then lose track of what the agent changed, why it changed it, and what conventions it silently broke. The codebase becomes a black box written by something that does not remember yesterday.

- **Teams** adopt AI agents independently. Each developer configures their agent differently. Code style diverges. Architecture decisions contradict each other. The AI agents are productive — but they are productive in five different directions.

- **Enterprises** face the hardest version: compliance teams cannot audit what an AI agent decided. Security teams cannot enforce boundaries on autonomous code generation. Engineering leads cannot answer "what governance exists over the code our agents produce?"

The root cause is always the same: **AI agents operate without an operating system.** They have no constitution defining what they may do. No task system defining what they should do. No engineering standards defining how they should do it.

Agentic Engineering solves this.

---

## The Solution

The system is split across five repositories, each with a single responsibility:

```
┌──────────────────────────────────────────────────────────┐
│           agentic-engineering (this repo)                 │
│    Docs, philosophy, getting started, architecture map   │
├──────────────┬─────────────────┬─────────────────────────┤
│  governance  │  project-mgmt   │  engineering-standards   │
│  "what may   │  "what to do"   │  "how to do it"         │
│   agents do?"│                 │                         │
├──────────────┴─────────────────┴─────────────────────────┤
│          ai-project-templates (scaffolder)                │
│    Instantiates all three into a new project repo        │
└──────────────────────────────────────────────────────────┘
```

**Governance** defines constitutional boundaries: what the AI agent is allowed to do, what it must never do, confidence ceilings, security protocols, and session lifecycle rules.

**Project Management** defines the work: YAML-based task definitions, priorities, dependencies, acceptance criteria — everything the agent needs to know *what* to build next.

**Engineering Standards** defines the how: code conventions, testing requirements, git workflow, review checklists, architecture patterns — the quality bar the agent must meet.

**Templates** wire it all together: a scaffolder that instantiates governance, project management, and engineering standards into any new project repo with a single command.

**This repo** (agentic-engineering) is the umbrella: documentation, philosophy, architecture maps, and the getting-started guide that ties the ecosystem together.

---

## Quick Start

### 1. Clone the ecosystem

```bash
# Clone all five repos
git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

### 2. Scaffold a new project

```bash
# Option A: Use the template scaffolder (recommended)
cd ai-project-templates
python scaffold.py --name my-project --output ../my-project

# Option B: Manual setup — copy the governance file into any existing repo
cp ai-governance-framework/templates/CLAUDE.md my-project/CLAUDE.md
```

### 3. Start working with your AI agent

```bash
cd my-project

# Your CLAUDE.md is already configured.
# Open the project with Claude Code, Cursor, or any AI-assisted editor.
# The agent reads CLAUDE.md automatically and follows the governance rules.
```

That is it. Your AI agent now operates within a structured framework: it knows what it may do, what it should do, and how it should do it.

For the full walkthrough, see [docs/getting-started.md](./docs/getting-started.md).

---

## Repositories

| Repo | Purpose | Status | Link |
|---|---|---|---|
| **agentic-engineering** | Umbrella docs, philosophy, architecture | Active | [GitHub](https://github.com/EduardPetraeus/agentic-engineering) |
| **ai-governance-framework** | Constitutional governance for AI agents | v0.3.0 shipped | [GitHub](https://github.com/EduardPetraeus/ai-governance-framework) |
| **ai-project-management** | YAML-based task engine for AI workflows | Scaffolding | [GitHub](https://github.com/EduardPetraeus/ai-project-management) |
| **ai-engineering-standards** | Code conventions, testing, git workflow | Scaffolding | [GitHub](https://github.com/EduardPetraeus/ai-engineering-standards) |
| **ai-project-templates** | Project scaffolder wiring all three | Scaffolding | [GitHub](https://github.com/EduardPetraeus/ai-project-templates) |

---

## Design Principles

### 1. Separate files over monolithic configuration

One file per concern. One file per task. One file per agent. Never a 500-line configuration file that does everything and is understood by nothing. Small files are readable, diffable, and replaceable.

### 2. Separation of concerns

Governance, project management, and engineering standards are three distinct responsibilities. They evolve at different speeds, are owned by different roles, and serve different purposes. Mixing them creates coupling that makes everything harder to change.

### 3. Templates are generic, instances are specific

The framework repos provide templates with sensible defaults. Your project repo customizes those templates for its specific context. The templates never assume your stack, your team size, or your domain — they provide structure you fill in.

### 4. Machine-readable over human-only

YAML over free-form markdown for structured data. Consistent metadata blocks. Parseable confidence scores. This is not just documentation for humans — it is configuration for AI agents. If the agent cannot parse it, it cannot follow it.

### 5. Solo-first, team-ready

Everything works for a single developer with a single AI agent. No setup overhead that only pays off at team scale. But the same structure scales to teams without rearchitecting: add role-based governance, multi-agent orchestration, and team review workflows on top of the same foundation.

### 6. Ship early, iterate in the open

Version 0.1 ships. It is incomplete. It is opinionated. It will change. But it exists, it works, and it invites contribution. Perfection is the enemy of ecosystems — early adopters shape the direction.

---

## Who Is This For

**Solo developers using AI agents** — You want to ship fast without losing control. You want your AI agent to follow consistent rules across sessions. You want to pick up a project after two weeks and immediately understand what the agent did and why. This gives you that structure without slowing you down.

**Teams adopting AI-assisted development** — You need every developer's AI agent to follow the same conventions, the same security rules, the same architecture patterns. You need governance that is enforceable, not just documented. This framework makes agent behavior consistent across your entire team.

**Engineering leads and platform teams** — You are responsible for the quality and security of code that AI agents produce. You need audit trails, confidence ceilings, blast radius controls, and compliance mappings. This framework gives you a governance layer that integrates with your existing CI/CD and review processes.

---

## Documentation

| Document | Description |
|---|---|
| [Getting Started](./docs/getting-started.md) | Step-by-step setup guide with commands |
| [Philosophy](./docs/philosophy.md) | Why agentic engineering exists and the principles behind it |
| [Architecture](./docs/architecture.md) | How the five repos connect and integrate |
| [Future Roadmap](./docs/future.md) | Vision for MCP distribution, SDK, and enterprise features |
| [Quickstart Tutorial](./docs/quickstart-tutorial.md) | Hands-on walkthrough: scaffold, configure, and ship |
| [Architecture Diagram](./assets/architecture-diagram.md) | Mermaid diagram of the full ecosystem |

---

## Contributing

This is an open-source project built in the open. Contributions are welcome:

1. Open an issue describing the problem or improvement
2. Fork the relevant repo (governance, project management, standards, or templates)
3. Submit a pull request with a clear description of the change

For governance changes, read [ai-governance-framework's contribution guide](https://github.com/EduardPetraeus/ai-governance-framework) first — governance changes have a higher review bar.

---

## License

MIT License. See [LICENSE](./LICENSE).

---

*Built by [Eduard Petraeus](https://github.com/EduardPetraeus) — shipping governance for the agentic age.*
