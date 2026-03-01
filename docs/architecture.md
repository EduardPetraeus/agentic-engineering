# Architecture

This document explains how the five repositories in the agentic engineering ecosystem connect, which repo owns what, and how a new project repo integrates with the framework.

---

## Ecosystem Overview

```
                    ┌──────────────────────────────┐
                    │    agentic-engineering        │
                    │    (umbrella documentation)   │
                    └──────────┬───────────────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              v                v                v
   ┌──────────────────┐ ┌───────────────┐ ┌──────────────────────┐
   │ ai-governance-   │ │ ai-project-   │ │ ai-engineering-      │
   │ framework        │ │ management    │ │ standards            │
   │                  │ │               │ │                      │
   │ What may agents  │ │ What to do    │ │ How to do it         │
   │ do?              │ │               │ │                      │
   └────────┬─────────┘ └──────┬────────┘ └──────────┬───────────┘
            │                  │                     │
            └──────────────────┼─────────────────────┘
                               │
                               v
                    ┌──────────────────────────────┐
                    │    ai-project-templates       │
                    │    (scaffolder)               │
                    └──────────┬───────────────────┘
                               │
                               v
                    ┌──────────────────────────────┐
                    │    Your Project Repo          │
                    │                              │
                    │  CLAUDE.md ←── governance    │
                    │  tasks/   ←── project-mgmt  │
                    │  standards/ ←── eng-stds    │
                    └──────────────────────────────┘
```

---

## Repo Ownership Map

Each repository has a clear, single responsibility. No overlap.

### agentic-engineering (this repo)

**Owns:** Documentation, philosophy, architecture maps, getting-started guide, public-facing narrative.

**Does not own:** Any executable code, governance rules, task definitions, or engineering standards. This repo only describes and connects — it never defines.

**Depends on:** All four other repos (references them in documentation).

### ai-governance-framework

**Owns:** Constitutional governance for AI agents.

- `CLAUDE.md` template and specification
- Agent definitions (roles, permissions, write access)
- Security protocols and enforcement rules
- Confidence ceiling definitions
- Pre-commit hooks and CI/CD governance checks
- Compliance mappings (SOC 2, ISO 42001)
- Health score calculator
- 7-layer governance model specification

**Does not own:** Task definitions, code standards, or scaffolding logic.

**Depends on:** Nothing. This is the foundation layer.

**Current state:** v0.3.0 shipped. 864 tests, 94% coverage. The most mature repo in the ecosystem.

### ai-project-management

**Owns:** Task lifecycle and work definition for AI agents.

- Task YAML schema and templates
- Backlog/active/done workflow
- Priority and dependency definitions
- Acceptance criteria format
- Sprint/iteration structure (optional)
- Agent command definitions for task operations

**Does not own:** Governance rules or code standards.

**Depends on:** ai-governance-framework (tasks operate within governance boundaries).

### ai-engineering-standards

**Owns:** Code quality and engineering conventions.

- Code style guides (per language/framework)
- Testing requirements and coverage thresholds
- Git workflow (branching strategy, commit conventions, PR process)
- Architecture patterns and anti-patterns
- Review checklists
- Documentation standards

**Does not own:** What to build (tasks) or what the agent may do (governance).

**Depends on:** ai-governance-framework (standards are enforced within governance).

### ai-project-templates

**Owns:** Scaffolding logic that instantiates the other three into a new project.

- Scaffolder CLI (`scaffold.py`)
- Template composition logic
- Stack-specific templates (Python, TypeScript, etc.)
- Default configurations per project type

**Does not own:** The content of governance, tasks, or standards. It copies from the source repos.

**Depends on:** All three framework repos (governance, project management, engineering standards).

---

## Integration Points

### How a Project Repo References the Frameworks

A scaffolded project repo contains local copies of framework files, not live references. This is intentional:

1. **CLAUDE.md** is copied from `ai-governance-framework/templates/CLAUDE.md` and customized
2. **Task templates** are copied from `ai-project-management/templates/` and populated
3. **Engineering standards** are copied from `ai-engineering-standards/templates/` and adjusted

The project repo is self-contained. It does not require the framework repos to be present at runtime. This means:

- Developers can work offline
- The project is not coupled to framework repo availability
- Framework updates are pulled explicitly, not automatically

### How Framework Updates Flow to Projects

When a framework repo releases a new version:

```
ai-governance-framework v0.4.0 released
         │
         v
ai-project-templates updated to use v0.4.0 templates
         │
         v
Existing projects: manually diff and merge new template changes
New projects: automatically get v0.4.0 via scaffolder
```

This is a pull model, not push. Projects opt into updates. No framework change silently changes a project's governance.

### How the Scaffolder Works

```
Input:   project name, stack, governance edition
              │
              v
Step 1:  Create directory structure
Step 2:  Copy CLAUDE.md from governance framework
Step 3:  Copy task templates from project management
Step 4:  Copy engineering standards for selected stack
Step 5:  Inject project-specific values (name, stack, etc.)
Step 6:  Initialize git repo
Step 7:  Create initial commit
              │
              v
Output:  Ready-to-use project repo with governance baked in
```

---

## The Flow: From New Project to Productive Development

Here is the full lifecycle, from the moment you decide to start a new project:

```
1. DECIDE        You decide to build a new project
                          │
2. SCAFFOLD      Run the scaffolder (or copy manually)
                          │
3. CUSTOMIZE     Edit CLAUDE.md for your specific context
                          │
4. DEFINE WORK   Create task YAML files in tasks/backlog/
                          │
5. AGENT READS   AI agent reads CLAUDE.md (governance),
                 tasks/ (what to do), standards/ (how to do it)
                          │
6. AGENT WORKS   Agent picks a task, creates a branch,
                 writes code within governance boundaries
                          │
7. VALIDATE      Pre-commit hooks + CI check governance
                 compliance, code quality, test coverage
                          │
8. REVIEW        Human reviews the output
                 (focused review — governance handled the rest)
                          │
9. MERGE + SHIP  Merge to main, deploy
                          │
10. ITERATE      Move task to done/, pick next task, repeat
```

The key insight: steps 5-7 are where the AI agent operates autonomously. Steps 1-4 are human direction. Steps 8-10 are human oversight. The framework makes the boundary between human and agent responsibility explicit and enforceable.

---

## See Also

- [Architecture Diagram (Mermaid)](../assets/architecture-diagram.md) — Visual diagram of repo relationships
- [Philosophy](./philosophy.md) — Why the architecture is designed this way
- [Getting Started](./getting-started.md) — Hands-on setup guide
