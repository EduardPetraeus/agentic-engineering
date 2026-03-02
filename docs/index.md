# Welcome to Agentic Engineering

This is the documentation site for the Agentic Engineering ecosystem — an open-source operating system for building software with AI agents.

---

## What You Will Learn

This documentation covers everything you need to govern, manage, and ship software with AI agents:

- **How to set up** a governed project from scratch using the scaffolder
- **How governance works** — constitutional boundaries, confidence ceilings, and enforcement gates
- **How to structure context** so your AI agent behaves consistently across sessions
- **How to run multiple agents** in parallel without conflicts
- **How to measure** whether the framework is actually working

---

## Where to Start

### New here?

Start with the [Tutorial](tutorial.md). It is a progressive guide with three levels:

1. **10-minute quickstart** — Clone, scaffold, run. See governance in action.
2. **30-minute full setup** — Customize governance, create tasks, set up CI/CD.
3. **Advanced topics** — Multi-agent patterns, context optimization, governance tuning.

### Want to understand the philosophy?

Read [Philosophy](philosophy.md) to understand why the system separates governance, project management, and engineering standards — and why that separation matters for AI agents specifically.

### Looking for architecture details?

The [Architecture](architecture.md) page explains how the five repos connect, what each one owns, and how a scaffolded project integrates with the framework.

---

## Site Map

| Section | What It Covers |
|---|---|
| [Tutorial](tutorial.md) | Hands-on guide from zero to production |
| [Philosophy](philosophy.md) | Design principles and rationale |
| [Architecture](architecture.md) | Repo relationships and integration |
| [Governance in Action](governance-in-action.md) | Before/after scenarios showing governance value |
| [Multi-Agent Patterns](guides/multi-agent-patterns.md) | Running multiple AI agents in parallel |
| [Context Architecture](guides/context-architecture.md) | Structuring context files for agent effectiveness |
| [Success Metrics](success-metrics.md) | How to measure if the framework is working |
| [Repo Map](reference/repo-map.md) | All five repos documented |
| [Glossary](reference/glossary.md) | Key terms with rationale |
| [Future Roadmap](future.md) | Vision for MCP, SDK, and enterprise features |

---

## The Ecosystem at a Glance

```
┌──────────────────────────────────────────────────────────┐
│           agentic-engineering (this site)                 │
│    Documentation, philosophy, architecture               │
├──────────────┬─────────────────┬─────────────────────────┤
│  governance  │  project-mgmt   │  engineering-standards   │
│  "what may   │  "what to do"   │  "how to do it"         │
│   agents do?"│                 │                         │
├──────────────┴─────────────────┴─────────────────────────┤
│          ai-project-templates (scaffolder)                │
│    Instantiates all three into a new project repo        │
└──────────────────────────────────────────────────────────┘
```

Each repo has a single responsibility. Together they form the complete operating system for AI-assisted development.
