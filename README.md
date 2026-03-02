# Agentic Engineering

[![Docs](https://img.shields.io/badge/docs-live-blue)](https://eduardpetraeus.github.io/agentic-engineering/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/EduardPetraeus/agentic-engineering)](https://github.com/EduardPetraeus/agentic-engineering/stargazers)

> Your AI agent writes code 10x faster. Without governance, you ship technical debt 10x faster too.

**Agentic Engineering** is an open-source operating system for building software with AI agents. It provides governance, project management, and engineering standards that turn AI code generation from a liability into a superpower.

---

## Why This Exists

AI coding agents generate code at speeds no human can match. But speed without structure creates chaos: broken conventions, missing tests, security gaps, and codebases that no one — human or AI — can maintain.

Agentic Engineering solves this with three separated concerns:

| Concern | Question | Repo |
|---|---|---|
| **Governance** | What may the agent do? | [ai-governance-framework](https://github.com/EduardPetraeus/ai-governance-framework) |
| **Project Management** | What should the agent do? | [ai-project-management](https://github.com/EduardPetraeus/ai-project-management) |
| **Engineering Standards** | How should the agent do it? | [ai-engineering-standards](https://github.com/EduardPetraeus/ai-engineering-standards) |

A fourth repo, [ai-project-templates](https://github.com/EduardPetraeus/ai-project-templates), scaffolds all three into any new project with a single command. This repo is the umbrella: documentation, architecture maps, and the philosophy that ties the ecosystem together.

---

## Quick Start

```bash
# 1. Clone the scaffolder
git clone https://github.com/EduardPetraeus/ai-project-templates.git
cd ai-project-templates
pip install pyyaml

# 2. Scaffold a new project
python scaffold.py --name my-project --stack python-data --mode solo --output-dir ../

# 3. Start working with your AI agent
cd ../my-project
claude   # or open in Cursor, Windsurf, etc.
```

Your AI agent reads `CLAUDE.md` automatically and operates within the defined governance boundaries.

For the full walkthrough, see the [documentation site](https://eduardpetraeus.github.io/agentic-engineering/).

---

## Repositories

| Repo | Purpose | Version |
|---|---|---|
| [agentic-engineering](https://github.com/EduardPetraeus/agentic-engineering) | Umbrella docs, philosophy, architecture | v1.0.0 |
| [ai-governance-framework](https://github.com/EduardPetraeus/ai-governance-framework) | Constitutional governance for AI agents | v0.3.0 |
| [ai-project-management](https://github.com/EduardPetraeus/ai-project-management) | YAML-based task engine for AI workflows | v1.0.0 |
| [ai-engineering-standards](https://github.com/EduardPetraeus/ai-engineering-standards) | Code conventions, testing, git workflow | v1.0.0 |
| [ai-project-templates](https://github.com/EduardPetraeus/ai-project-templates) | Project scaffolder wiring all three | v1.0.0 |

---

## Design Principles

1. **Separate files over monolithic configuration** — One file per concern. Small files are readable, diffable, and replaceable.
2. **Separation of concerns** — Governance, project management, and engineering standards evolve at different speeds and serve different purposes.
3. **Templates are generic, instances are specific** — The framework provides defaults. Your project customizes them.
4. **Machine-readable over human-only** — YAML for structured data. If the agent cannot parse it, it cannot follow it.
5. **Solo-first, team-ready** — Everything works for a single developer. The same structure scales to teams.

---

## Documentation

The full documentation site is available at **[eduardpetraeus.github.io/agentic-engineering](https://eduardpetraeus.github.io/agentic-engineering/)**.

Key pages:

- [Tutorial](https://eduardpetraeus.github.io/agentic-engineering/tutorial/) — Progressive guide from quickstart to advanced topics
- [Philosophy](https://eduardpetraeus.github.io/agentic-engineering/philosophy/) — Why the system is designed this way
- [Architecture](https://eduardpetraeus.github.io/agentic-engineering/architecture/) — How the five repos connect
- [Governance in Action](https://eduardpetraeus.github.io/agentic-engineering/governance-in-action/) — Concrete examples of governance preventing real mistakes

---

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

For governance changes, read [ai-governance-framework's contribution guide](https://github.com/EduardPetraeus/ai-governance-framework) first — governance changes have a higher review bar.

---

## License

MIT License. See [LICENSE](./LICENSE).

---

*Built by [Eduard Petraeus](https://github.com/EduardPetraeus) — shipping governance for the agentic age.*
