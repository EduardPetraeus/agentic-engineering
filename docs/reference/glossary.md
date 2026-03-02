# Glossary

Key terms used throughout the Agentic Engineering ecosystem. Each entry includes not just a definition but the reasoning behind it — why this concept exists, why it is designed this way, and what problem it solves.

---

## A

### Agent Session

A single working session where an AI agent reads context files, performs tasks, and produces output. Sessions are bounded — they have a defined start (read CLAUDE.md), execution phase, and end (summarize changes). Each session is stateless; the agent does not remember previous sessions unless context files provide that memory.

**Why sessions are stateless by design:** Stateless sessions force all important context into files rather than relying on agent memory. This makes agent behavior reproducible — any agent, at any time, reading the same files, produces consistent behavior. Stateful sessions sound appealing but create a fragile dependency on conversation history that breaks when you switch agents, restart sessions, or onboard a new team member.

### AGENTS.md

A symlink to `CLAUDE.md` that serves as an agent-agnostic configuration file. Different AI tools look for different config files — `AGENTS.md` ensures compatibility across Claude Code, Cursor, Windsurf, and other tools without maintaining duplicate files.

**Why a symlink instead of a copy:** A copy creates drift. When you update `CLAUDE.md`, you must remember to update `AGENTS.md` too. A symlink guarantees both files are always identical with zero maintenance overhead. The tradeoff is that symlinks do not work on every platform (some Windows configurations), but this is acceptable for the majority of development environments.

### Agentic Engineering OS

The complete ecosystem of five repositories that together provide governance, project management, engineering standards, scaffolding, and documentation for AI-assisted software development. The "operating system" metaphor reflects that these repos provide the foundational layer on which AI agents operate — just as an OS provides the layer on which applications run.

**Why five repos instead of one:** Each repo has a different rate of change, a different audience, and a different concern. Governance changes when security requirements change. Standards change when the team agrees on new conventions. Bundling them together would mean a single massive repo where every change touches unrelated concerns. The separation also allows teams to adopt parts of the ecosystem incrementally — you can use governance without task management, or standards without the scaffolder.

---

## B

### Builder-Brain

A centralized, private repository containing a developer's identity, preferences, active projects, and accumulated knowledge. Structured as hot memory (always loaded) and cold memory (loaded on demand). The builder-brain pattern allows a single developer to maintain consistent AI agent behavior across all their projects.

**Why centralized rather than per-project:** A developer's identity and preferences (language, communication style, naming conventions) do not change between projects. Duplicating them in every repo creates maintenance burden and drift. A single builder-brain repo acts as the single source of truth for developer-level context, while each project's CLAUDE.md handles project-level context.

### Backlog

The `backlog/` directory in a project repo containing YAML task files that represent work to be done. The AI agent reads these files to determine what to build next. Each task is a separate file with structured fields for priority, acceptance criteria, and status.

**Why a directory instead of a single file:** A single `backlog.md` with 50 tasks causes merge conflicts when multiple agents or developers work simultaneously. One file per task means each task can be created, modified, and completed independently — `git mv` from `backlog/` to `done/` is a clean, atomic operation.

---

## C

### Confidence Ceiling

A governance mechanism that caps the confidence level at which an AI agent's output is auto-approved. Set at 85% by default — outputs above this threshold are flagged for human review because overconfident agent output is the most likely to contain undetected errors.

**Why 85%, not 80% or 90%:** The 85% threshold is calibrated based on observed AI agent behavior patterns. Below 60%, agents typically flag uncertainty themselves — the risk is low because the agent is already asking for help. Between 60-85%, agents proceed but with visible caution — this is the productive operating range. Above 85%, agents often exhibit false confidence: the output looks correct, passes basic tests, and reads well — but contains subtle errors (logic flaws, security gaps, edge cases) that are invisible precisely because the agent is so confident. Setting the ceiling at 80% would catch too many legitimate high-quality outputs, creating unnecessary review overhead. Setting it at 90% would let through the most dangerous zone of false confidence. 85% balances safety with productivity. Teams should adjust this threshold based on their risk tolerance — lower for financial or medical software, higher for internal tools and prototypes.

### Constitution / CLAUDE.md

The governance file at the root of every repository that defines what an AI agent may and must not do. Named after Claude Code's convention, it serves as the agent's constitutional law. Contains project context, conventions, session protocols, quality standards, and security rules.

**Why the name "constitution":** A constitution is a foundational document that constrains behavior. It is not a suggestion — it is law. The analogy is intentional: just as a government constitution limits what branches of government may do, CLAUDE.md limits what an AI agent may do. The agent operates with freedom within its constitutional boundaries, but the boundaries themselves are non-negotiable.

### Context File

Any file that provides information to an AI agent about the project, developer, or current state. Includes CLAUDE.md, CONTEXT.md, ARCHITECTURE.md, memory files, and task files. Effective context architecture is the foundation of reliable agent behavior.

**Why files instead of prompts:** Prompts are ephemeral — they exist for one session and disappear. Files persist across sessions, are version-controlled, and are reviewable. When context lives in files, you can diff what changed, review what the agent was told, and ensure consistency across sessions and team members.

---

## D

### Definition of Done

A list of acceptance criteria in a task YAML file that the AI agent must satisfy before marking the task as complete. Each criterion is explicit and testable. The agent uses these to self-validate its output.

**Why explicit criteria matter for AI agents specifically:** Human developers intuit "done" from experience. AI agents do not. Without explicit criteria, an agent will complete the happy path and declare victory — missing error handling, edge cases, tests, and documentation. A definition of done forces completeness by listing every requirement the agent must satisfy. The more specific the criteria, the more reliable the agent's output.

---

## G

### Governance Gate

An automated CI/CD check that validates governance compliance on every pull request. Runs drift detection (compares CLAUDE.md against the template), content quality checks, secret scanning, linting, and tests. All checks must pass before merge.

**Why automated enforcement instead of trusting the agent:** Agents follow CLAUDE.md in good faith, but they are not infallible. They sometimes miss rules, misinterpret constraints, or prioritize task completion over compliance. The governance gate is the safety net — even if the agent skips a rule, the CI pipeline catches it. This is the "AI validates AI" principle: the coding agent produces output, and the governance automation validates it.

### Governance Layer

The set of rules, constraints, and enforcement mechanisms that control what an AI agent is allowed to do. Defined in `ai-governance-framework` and instantiated via CLAUDE.md in each project. Includes security protocols, confidence ceilings, and blast radius controls.

---

## H

### Hot Memory

Context files loaded at the start of every session, regardless of the task. Contains identity, preferences, and active project state. Kept short (under 50 lines) to minimize context window usage. Contrast with cold memory, which is loaded on demand.

**Why 50 lines maximum:** Every token in hot memory competes with task-specific context for the agent's attention. A 200-line identity file means 200 lines less room for code context, task details, and file contents. The 50-line limit forces prioritization — only the most important preferences and rules make the cut. Everything else moves to cold memory and is loaded only when relevant.

### Cold Memory

Context files loaded only when a specific topic comes up during a session. Contains deep reference material like framework specifications, protocol details, and historical decisions. The trigger table in builder-brain defines when each cold memory file should be loaded.

**Why not load everything at session start:** Context windows are finite. Loading all reference material at session start wastes tokens on content that is irrelevant 90% of the time. Cold memory follows a just-in-time principle: load deep context only when the conversation enters that domain. This keeps the default context lean and leaves maximum room for the actual work.

---

## M

### Medallion Architecture

A data engineering pattern used in the `python-data` stack template. Organizes data processing into three layers: bronze (raw ingestion), silver (cleaned and normalized), and gold (aggregated business views). The scaffolder generates this directory structure automatically.

**Why three layers:** Each layer has a different purpose and a different data quality guarantee. Bronze stores raw data exactly as received — no transformations, no assumptions. Silver applies cleaning, normalization, and validation — the data is trustworthy but not yet shaped for business use. Gold aggregates and joins for specific business questions. This separation means a bug in the gold layer never corrupts raw data, and a schema change in bronze never breaks business reports.

### Mode

A scaffolder parameter that determines the project's collaboration model. `solo` mode configures governance for a single developer with a single agent. `team` mode adds multi-agent coordination, role-based permissions, and team review workflows.

**Why two modes instead of one flexible configuration:** Solo and team projects have fundamentally different governance needs. Solo projects need minimal ceremony — fast iteration, light review. Team projects need coordination — consistent conventions, role-based permissions, mandatory reviews. A single "flexible" configuration would either be too complex for solo use or too simple for team use. Two explicit modes set the right defaults for each scenario.

---

## S

### Scaffolder

The CLI tool (`scaffold.py`) in `ai-project-templates` that generates a complete project repository from templates. Takes project name, stack, and mode as inputs. Outputs a self-contained repo with governance, task structure, engineering config, and CI/CD.

**Why a scaffolder instead of just documentation:** Documentation tells you what to set up. A scaffolder does it for you. The difference is 10 minutes of reading and copying vs. 30 seconds of running a command. More importantly, a scaffolder eliminates human error — every generated project has the correct structure, the correct files, and the correct defaults. No missed steps, no typos in file names, no forgotten symlinks.

### Sprint

An optional time-boxed iteration for organizing tasks. Tasks can be assigned to sprints in their YAML files. Sprints have start dates, end dates, and goals. Not required — solo developers often work without sprints.

### Stack

A scaffolder parameter that determines the project's technology template. Options: `python-data` (data engineering with medallion architecture), `python-web` (web APIs and services), `docs-only` (documentation repos with no source code), `databricks-lakehouse` (Databricks-based data platform).

---

## T

### Task Engine

The YAML-based task management system defined in `ai-project-management`. One YAML file per task, stored in `backlog/`. Each file contains structured fields (id, title, description, priority, acceptance criteria, status) that the AI agent parses to determine what to build.

**Why YAML over markdown for tasks:** Markdown is ambiguous for machines. One author formats priority as bold text, another as a heading, a third as a bullet point. The agent must guess the structure every time. YAML eliminates guessing: `priority: high` is unambiguous and parseable. Every field has a known name, a known type, and a known location. The agent reads structure, not prose.

### Trust Profile

A governance concept that defines what level of autonomy an AI agent has for different types of actions. Low-risk actions (formatting, linting) may be auto-approved. High-risk actions (database migrations, security changes) require human review. The trust profile maps action types to approval requirements.

**Why variable trust instead of blanket approval or blanket review:** Blanket approval is dangerous — the agent ships security vulnerabilities without oversight. Blanket review is wasteful — a human reviewing formatting changes adds no value. Variable trust matches the level of oversight to the level of risk. The agent moves fast on low-risk actions and slows down for high-risk ones. This is how experienced engineering teams already work with human developers — junior developers get more review, senior developers get more autonomy, and everyone gets security review.

---

## See Also

- [Repo Map](./repo-map.md) — All five repos documented
- [Philosophy](../philosophy.md) — Design principles behind these concepts
- [Context Architecture](../guides/context-architecture.md) — How context files work together
