# Context Architecture

AI agents are only as effective as the context they receive. This guide covers how to structure, organize, and maintain the context files that govern agent behavior across sessions, repos, and projects.

---

## Core Context Files

Every project in the Agentic Engineering ecosystem uses a set of standard context files. Each file has a specific role and scope.

### CLAUDE.md — The Constitution

`CLAUDE.md` is the constitutional governance file at the root of every repository. It is the first file an AI agent reads at the start of every session.

**Mandatory sections:**

| Section | Purpose |
|---|---|
| `project_context` | Repo name, purpose, stack, status, license |
| `conventions` | Naming, file structure, language, style rules |
| `mandatory_session_protocol` | What the agent must do at session start, during work, and at session end |
| `quality_standards` | Testing, coverage, review requirements |
| `security_protocol` | What the agent must never do (secrets, destructive ops, production access) |

**Key principles:**

- Keep it under 100 lines. Long constitutions get skimmed by both humans and agents.
- Be specific. "Follow best practices" is meaningless. "Use ruff with the config in `.engineering/ruff.toml`" is actionable.
- Update it when conventions change. A stale CLAUDE.md is worse than no CLAUDE.md — the agent follows outdated rules with full confidence.

### AGENTS.md — Agent-Agnostic Alias

`AGENTS.md` is a symlink to `CLAUDE.md`. It exists because different AI tools look for different configuration files:

```bash
ln -s CLAUDE.md AGENTS.md
```

Claude Code reads `CLAUDE.md`. Other tools may look for `AGENTS.md`, `.cursorrules`, or similar files. By making `AGENTS.md` a symlink, you maintain a single source of truth while supporting multiple tools.

!!! tip
    The scaffolder creates this symlink automatically. If you set up a project manually, create it yourself.

### CONTEXT.md — Living Project State

`CONTEXT.md` captures the current state of the project: what was done last session, what is in progress, what decisions were made, and what the agent should focus on next.

Unlike `CLAUDE.md` (which changes rarely), `CONTEXT.md` is updated every session:

```markdown
# Context

## Current State
- Bronze layer ingestion complete (TASK-001 done)
- Silver layer transformation in progress (TASK-002 active)
- CI pipeline passing on main

## Recent Decisions
- Chose DuckDB over SQLite for local analytics (see ADR-002)
- Rate limiting set to 100 req/min per API key

## Next Session Focus
- Complete TASK-002: silver layer joins
- Add integration tests for bronze-to-silver pipeline
```

The agent reads this at session start to understand where the project stands without reading the entire git history.

### ARCHITECTURE.md — System Design

`ARCHITECTURE.md` documents the high-level architecture: component relationships, data flow, key design decisions, and boundaries. This file helps the agent understand the system it is modifying, not just the task it is completing.

---

## Memory Files: Cross-Session Persistence

For information that persists across sessions but is not project-specific, use memory files stored in `.claude/memory/`:

```
.claude/
└── memory/
    ├── profile.md       # Developer identity, goals, preferences
    ├── stack.md         # Tech stack, architecture principles
    └── products.md      # Active products and pipeline
```

Memory files are loaded on demand. They give the agent context about the developer and their broader goals without cluttering the project-level CLAUDE.md.

---

## The Builder-Brain Pattern

For developers working across multiple projects, a centralized context repository provides identity and preferences that apply everywhere.

### Structure

```
builder-brain/
├── hot/
│   └── always-loaded.md      # Identity, preferences, communication style
├── cold/
│   ├── agentic-framework.md  # Framework deep reference
│   ├── context-architecture.md
│   └── update-protocol.md
├── INDEX.md                   # Active projects overview
└── builder-brain-master.md    # Full reference document
```

### Hot Memory vs. Cold Memory

**Hot memory** is loaded at the start of every session. It contains identity, communication style, active project list, and current priorities. Keep it short — under 50 lines.

**Cold memory** is loaded on demand when a specific topic comes up. Framework specifications, protocol details, and deep reference material live here. The agent loads cold memory only when needed, keeping the default context window small.

### Trigger table

Define when to load cold memory:

| Trigger | Load |
|---|---|
| Working on framework code | `cold/agentic-framework.md` |
| Setting up context files | `cold/context-architecture.md` |
| Updating protocols | `cold/update-protocol.md` |

This pattern scales to any number of cold memory files without bloating the default session context.

---

## Global vs. Repo-Level CLAUDE.md

When using Claude Code, there are two levels of CLAUDE.md:

1. **Global CLAUDE.md** (`~/.claude/CLAUDE.md` or project root) — Rules that apply to every repo: language preferences, output format, identity.
2. **Repo CLAUDE.md** — Rules specific to this repository: conventions, session protocol, quality standards.

The hierarchy is:

```
Global CLAUDE.md (universal rules)
    └── Repo CLAUDE.md (project-specific rules)
        └── Session context (CONTEXT.md, memory files)
```

If there is a conflict, the repo-level CLAUDE.md takes precedence. The global level sets defaults; the repo level overrides them.

---

## Structuring Context for Maximum Effectiveness

### The pyramid principle

Put the most important information first. Agents process context sequentially — front-loaded context has more influence on behavior.

```
CLAUDE.md line 1-10:   Critical rules (security, never-do-this)
CLAUDE.md line 10-30:  Project context (stack, purpose, status)
CLAUDE.md line 30-60:  Conventions (naming, structure, style)
CLAUDE.md line 60-100: Session protocol (start, during, end)
```

### Specificity over generality

Every rule should be actionable by the agent:

| Bad | Good |
|---|---|
| "Write clean code" | "Run `ruff check .` before committing" |
| "Follow best practices" | "Use type hints on all public functions" |
| "Be careful with secrets" | "Never commit files matching `*.env`, `*credentials*`, `*secret*`" |

### Reference, do not duplicate

If a convention is defined in `.engineering/ruff.toml`, reference it from CLAUDE.md — do not copy the entire config into CLAUDE.md. Duplication creates drift.

```markdown
## code_style
- Linting: ruff with config in `.engineering/ruff.toml`
- Formatting: ruff format with same config
```

---

## Anti-Patterns

### Duplicate context

The same rule defined in CLAUDE.md, CONTEXT.md, and a memory file. When one is updated and the others are not, the agent receives contradictory instructions.

**Fix:** Define each rule in exactly one place. Reference it from other files.

### Stale context

CONTEXT.md says "TASK-002 is in progress" but it was completed three sessions ago. The agent wastes time on already-done work or makes decisions based on outdated state.

**Fix:** Update CONTEXT.md at the end of every session. Make it part of the session protocol.

### Over-long files

A 500-line CLAUDE.md that covers governance, task details, architecture, coding standards, and project history. The agent cannot distinguish critical rules from background information.

**Fix:** Keep CLAUDE.md under 100 lines. Move detailed content to dedicated files (CONTEXT.md, ARCHITECTURE.md, `.engineering/`).

### Missing context hierarchy

All context in a single flat file with no structure. The agent treats every line as equally important.

**Fix:** Use sections, headers, and the pyramid principle. Critical rules first, details later.

---

## See Also

- [Multi-Agent Patterns](./multi-agent-patterns.md) — How agents use context in parallel execution
- [Philosophy](../philosophy.md) — Why context architecture matters
- [Glossary](../reference/glossary.md) — Key terms defined
