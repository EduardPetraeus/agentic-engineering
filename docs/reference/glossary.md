# Glossary

Key terms used throughout the Agentic Engineering ecosystem.

---

## A

### Agent Session
A single working session where an AI agent reads context files, performs tasks, and produces output. Sessions are bounded — they have a defined start (read CLAUDE.md), execution phase, and end (summarize changes). Each session is stateless; the agent does not remember previous sessions unless context files provide that memory.

### AGENTS.md
A symlink to `CLAUDE.md` that serves as an agent-agnostic configuration file. Different AI tools look for different config files — `AGENTS.md` ensures compatibility across Claude Code, Cursor, Windsurf, and other tools without maintaining duplicate files.

### Agentic Engineering OS
The complete ecosystem of five repositories that together provide governance, project management, engineering standards, scaffolding, and documentation for AI-assisted software development. The "operating system" metaphor reflects that these repos provide the foundational layer on which AI agents operate.

---

## B

### Builder-Brain
A centralized, private repository containing a developer's identity, preferences, active projects, and accumulated knowledge. Structured as hot memory (always loaded) and cold memory (loaded on demand). The builder-brain pattern allows a single developer to maintain consistent AI agent behavior across all their projects.

### Backlog
The `backlog/` directory in a project repo containing YAML task files that represent work to be done. The AI agent reads these files to determine what to build next. Each task is a separate file with structured fields for priority, acceptance criteria, and status.

---

## C

### Confidence Ceiling
A governance mechanism that caps the confidence level at which an AI agent's output is auto-approved. Set at 85% by default — outputs above this threshold are flagged for human review because overconfident agent output is the most likely to contain undetected errors.

### Constitution / CLAUDE.md
The governance file at the root of every repository that defines what an AI agent may and must not do. Named after Claude Code's convention, it serves as the agent's constitutional law. Contains project context, conventions, session protocols, quality standards, and security rules.

### Context File
Any file that provides information to an AI agent about the project, developer, or current state. Includes CLAUDE.md, CONTEXT.md, ARCHITECTURE.md, memory files, and task files. Effective context architecture is the foundation of reliable agent behavior.

---

## D

### Definition of Done
A list of acceptance criteria in a task YAML file that the AI agent must satisfy before marking the task as complete. Each criterion is explicit and testable. The agent uses these to self-validate its output.

---

## G

### Governance Gate
An automated CI/CD check that validates governance compliance on every pull request. Runs drift detection (compares CLAUDE.md against the template), content quality checks, secret scanning, linting, and tests. All checks must pass before merge.

### Governance Layer
The set of rules, constraints, and enforcement mechanisms that control what an AI agent is allowed to do. Defined in `ai-governance-framework` and instantiated via CLAUDE.md in each project. Includes security protocols, confidence ceilings, and blast radius controls.

---

## H

### Hot Memory
Context files loaded at the start of every session, regardless of the task. Contains identity, preferences, and active project state. Kept short (under 50 lines) to minimize context window usage. Contrast with cold memory, which is loaded on demand.

### Cold Memory
Context files loaded only when a specific topic comes up during a session. Contains deep reference material like framework specifications, protocol details, and historical decisions. The trigger table in builder-brain defines when each cold memory file should be loaded.

---

## M

### Medallion Architecture
A data engineering pattern used in the `python-data` stack template. Organizes data processing into three layers: bronze (raw ingestion), silver (cleaned and normalized), and gold (aggregated business views). The scaffolder generates this directory structure automatically.

### Mode
A scaffolder parameter that determines the project's collaboration model. `solo` mode configures governance for a single developer with a single agent. `team` mode adds multi-agent coordination, role-based permissions, and team review workflows.

---

## S

### Scaffolder
The CLI tool (`scaffold.py`) in `ai-project-templates` that generates a complete project repository from templates. Takes project name, stack, and mode as inputs. Outputs a self-contained repo with governance, task structure, engineering config, and CI/CD.

### Sprint
An optional time-boxed iteration for organizing tasks. Tasks can be assigned to sprints in their YAML files. Sprints have start dates, end dates, and goals. Not required — solo developers often work without sprints.

### Stack
A scaffolder parameter that determines the project's technology template. Options: `python-data` (data engineering with medallion architecture), `python-web` (web APIs and services), `docs-only` (documentation repos with no source code).

---

## T

### Task Engine
The YAML-based task management system defined in `ai-project-management`. One YAML file per task, stored in `backlog/`. Each file contains structured fields (id, title, description, priority, acceptance criteria, status) that the AI agent parses to determine what to build.

### Trust Profile
A governance concept that defines what level of autonomy an AI agent has for different types of actions. Low-risk actions (formatting, linting) may be auto-approved. High-risk actions (database migrations, security changes) require human review. The trust profile maps action types to approval requirements.

---

## See Also

- [Repo Map](./repo-map.md) — All five repos documented
- [Philosophy](../philosophy.md) — Design principles behind these concepts
- [Context Architecture](../guides/context-architecture.md) — How context files work together
