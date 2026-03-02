# Tutorial

A progressive, hands-on guide to setting up and using the Agentic Engineering ecosystem. Start with the quickstart and go deeper as needed.

---

## Part 1: Quickstart (10 minutes)

Get a governed project running in under 10 minutes.

### Prerequisites

- Git installed
- Python 3.10+
- An AI coding agent: Claude Code, Cursor, Windsurf, or any editor that reads `CLAUDE.md`

### Step 1: Clone the scaffolder

```bash
git clone https://github.com/EduardPetraeus/ai-project-templates.git
cd ai-project-templates
pip install pyyaml
```

### Step 2: Scaffold a project

```bash
python scaffold.py --name my-project --stack python-data --mode solo --output-dir ../
```

This creates a complete project with governance, task management, and engineering standards:

```
my-project/
├── CLAUDE.md                     # Agent constitution (governance)
├── AGENTS.md -> CLAUDE.md        # Agent-agnostic alias (symlink)
├── CONTEXT.md                    # Living project state
├── ARCHITECTURE.md               # System design document
├── README.md                     # Project README
├── .gitignore
├── backlog/
│   └── TASK-001.yaml             # Starter task template
├── .claude/
│   └── commands/
│       ├── pick-next-task.md
│       ├── complete-task.md
│       └── create-task.md
├── .engineering/
│   ├── ruff.toml
│   └── pyproject.toml
├── docs/
│   └── adr/
│       └── ADR-000-template.md
├── src/
│   ├── bronze/                   # Raw data ingestion
│   ├── silver/                   # Cleaned and normalized
│   └── gold/                     # Aggregated business views
├── tests/
└── .github/
    └── workflows/
        └── ci.yml
```

### Step 3: Start working

```bash
cd ../my-project
claude   # or open in Cursor, Windsurf, etc.
```

The AI agent reads `CLAUDE.md` at session start and operates within its governance boundaries. It checks `backlog/` for tasks and follows conventions from `.engineering/`.

That is it. Your AI agent now operates within a structured framework.

!!! success "Checkpoint: Quickstart complete"
    You have a scaffolded project with governance, tasks, and engineering standards. The agent reads CLAUDE.md automatically and follows the rules.

---

## Part 2: Full Setup (30 minutes)

Customize governance, create real tasks, validate compliance, and set up CI/CD.

### Prerequisites (in addition to Part 1)

- GitHub CLI (`gh`) installed and authenticated
- A GitHub account

### Step 4: Clone the full ecosystem

For governance validation and CI/CD setup, you need all five repos:

```bash
mkdir agentic-ecosystem && cd agentic-ecosystem

git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

### Step 5: Customize CLAUDE.md

Open `CLAUDE.md` in your project and update the `project_context` section:

```markdown
## project_context

- **Repo:** my-project
- **Purpose:** Data pipeline for weather analytics
- **Stack:** Python 3.11, DuckDB, requests
- **Status:** Active development
- **License:** MIT
```

Add project-specific conventions:

```markdown
## conventions

- All API calls go through `src/bronze/`
- Transformations in `src/silver/` must be idempotent
- Gold layer outputs are read-only views
- Environment variables for all API keys (never hardcode)
```

Keep CLAUDE.md under 100 lines. Move detailed architecture to ARCHITECTURE.md and current state to CONTEXT.md.

### Step 6: Create real tasks

Replace the starter task in `backlog/TASK-001.yaml`:

```yaml
id: TASK-001
title: "Set up bronze layer ingestion for weather API"
description: |
  Create a connector in src/bronze/weather.py that fetches
  current weather data from the OpenWeather API. Store raw
  JSON responses in src/bronze/output/weather/.

status: ready
priority: high
agent: code
review: human-review
assignee: null
sprint: null

depends_on: []
blocks: []

definition_of_done:
  - "src/bronze/weather.py fetches data from OpenWeather API"
  - "Raw JSON saved to src/bronze/output/weather/ with timestamp filenames"
  - "Retry logic: 3 attempts with exponential backoff"
  - "API key read from OPENWEATHER_API_KEY environment variable"
  - "Unit tests in tests/test_bronze_weather.py"
  - "All tests pass with ruff check clean"

tags: [bronze, ingestion, api]
estimate: m

created: 2026-03-01
updated: 2026-03-01
notes: "Use requests library. Free tier API key from openweathermap.org."
```

Tasks live in `backlog/`, one YAML file per task. The agent reads these to determine what to build. Priority, acceptance criteria, and dependencies are all machine-readable.

### Step 7: Run governance checks locally

Before pushing, validate your project against the governance framework:

```bash
cd my-project

# Copy automation scripts temporarily
cp ../ai-governance-framework/automation/drift_detector.py .
cp ../ai-governance-framework/automation/content_quality_checker.py .

# Run drift detection
python drift_detector.py \
  --template ../ai-governance-framework/templates/CLAUDE.md \
  --target CLAUDE.md \
  --format text

# Run content quality check
python content_quality_checker.py . --format json

# Clean up
rm drift_detector.py content_quality_checker.py
```

A freshly scaffolded project should show high alignment. Fix any gaps before proceeding.

### Step 8: Set up CI/CD with governance gate

Copy the governance gate workflow into your project:

```bash
mkdir -p .github/workflows

cp ../ai-governance-framework/templates/ci-cd/github-actions-governance-gate.yml \
   .github/workflows/governance-gate.yml

# Copy automation scripts for CI
mkdir -p automation
cp ../ai-governance-framework/automation/drift_detector.py automation/
cp ../ai-governance-framework/automation/content_quality_checker.py automation/
```

The governance gate runs four checks on every PR:

| Check | What it validates |
|---|---|
| **Lint** | `ruff check` and `ruff format --check` enforce code style |
| **Test** | `pytest` runs on the `tests/` directory |
| **Security Scan** | Scans PR diff for secrets (API keys, tokens, passwords) |
| **Governance** | Verifies CLAUDE.md exists, runs drift detection and quality checks |

### Step 9: Initialize and push

```bash
git init
git add -A
git commit -m "feat: initial scaffold from ai-project-templates"

# Create the remote repository
gh repo create my-project --public --source . --push
```

### Step 10: Make your first PR

```bash
git checkout -b feature/task-001-bronze-weather

# ... agent implements the task ...

git add -A
git commit -m "feat(bronze): add weather API connector

Implements TASK-001: bronze layer ingestion for OpenWeather API."

git push -u origin feature/task-001-bronze-weather

gh pr create \
  --title "feat(bronze): add weather API connector" \
  --body "Implements TASK-001. See backlog/TASK-001.yaml for acceptance criteria."
```

The governance gate runs automatically. All checks must pass before merge.

!!! success "Checkpoint: Full setup complete"
    You have a governed project with custom CLAUDE.md, real tasks, governance validation, and CI/CD. Every PR is validated against the governance framework before merge.

---

## Part 3: Advanced Topics

Deep dives into multi-agent patterns, context optimization, and governance tuning.

### Running multiple agents in parallel

The most reliable pattern for cross-repo work is dispatching one agent per repository:

```
Orchestrator (you or a coordinator agent)
    |
    +--- Agent 1 --> ai-governance-framework
    |
    +--- Agent 2 --> ai-project-management
    |
    +--- Agent 3 --> ai-engineering-standards
    |
    +--- Agent 4 --> ai-project-templates
```

Key rules for parallel agents:

1. **Agents must work on independent tasks.** If Agent B needs Agent A's output, run them sequentially.
2. **Pre-read key files before dispatch.** The orchestrator reads scaffold.py, CLAUDE.md templates, and test files, then embeds the relevant details in each agent's prompt.
3. **Include exact file paths, code snippets, and acceptance criteria** in every agent prompt.
4. **Four parallel agents is the sweet spot.** Enough for meaningful speedup, few enough to review all outputs.

For the full guide, see [Multi-Agent Patterns](guides/multi-agent-patterns.md).

### Optimizing context architecture

Context files follow a hierarchy:

```
Global CLAUDE.md (universal rules)
    └── Repo CLAUDE.md (project-specific rules)
        └── Session context (CONTEXT.md, memory files)
```

Key principles:

- **Keep CLAUDE.md under 100 lines.** Long constitutions get skimmed by both humans and agents.
- **Put critical rules first.** Agents process context sequentially — front-loaded context has more influence.
- **Be specific.** "Follow best practices" is meaningless. "Run `ruff check .` before committing" is actionable.
- **Reference, do not duplicate.** If a convention is defined in `.engineering/ruff.toml`, reference it from CLAUDE.md.

For the full guide, see [Context Architecture](guides/context-architecture.md).

### Tuning governance thresholds

The default confidence ceiling is 85%. Adjust it based on your project's risk profile:

- **High-risk projects** (financial, medical, security): Lower to 70-75%. More human review, fewer undetected errors.
- **Low-risk projects** (internal tools, prototypes): Raise to 90%. Faster iteration, acceptable risk.
- **Default (85%)**: Good baseline for production software. Catches overconfident agent output without excessive review overhead.

Trust profiles map action types to approval requirements:

| Action type | Risk | Approval |
|---|---|---|
| Formatting, linting | Low | Auto-approve |
| New feature code | Medium | Agent review + human spot-check |
| Database migrations | High | Human review required |
| Security-related changes | Critical | Human review + security audit |

### Shipping a release

After completing your core tasks:

1. Update `CONTEXT.md` with the current project state.
2. Update completed tasks to `status: done` in their YAML files.
3. Tag and release:

```bash
git checkout main
git pull origin main
git tag -a v1.0.0 -m "v1.0.0: Initial release"
git push origin v1.0.0

gh release create v1.0.0 \
  --title "v1.0.0" \
  --notes "Initial release with governance, CI/CD, and core features."
```

### Scaffolder reference

The scaffolder (`scaffold.py` in [ai-project-templates](https://github.com/EduardPetraeus/ai-project-templates)) supports these arguments:

| Argument | Required | Options |
|---|---|---|
| `--name` | Yes | Project name (directory name) |
| `--stack` | Yes | `python-data`, `python-web`, `docs-only`, `databricks-lakehouse` |
| `--mode` | Yes | `solo`, `team` |
| `--output-dir` | No | Parent directory (default: current) |
| `--dry-run` | No | Show what would be created without writing files |

The three main stacks:

- **python-data** — Medallion architecture with bronze/silver/gold layers, suited for data engineering and analytics.
- **python-web** — Web application structure for APIs and services.
- **docs-only** — Documentation-only repos with no source code directories.

!!! success "Checkpoint: Advanced topics complete"
    You understand multi-agent patterns, context optimization, governance tuning, and the full scaffolder API. You are ready to build production software with AI agents.

---

## What You Have Built

By completing this tutorial, your project has:

1. **Governance** — CLAUDE.md defines what the AI agent may and must not do
2. **Task management** — YAML backlog the agent reads to determine what to build
3. **Engineering standards** — Linter config, conventions, and architecture patterns
4. **Agent commands** — Pre-built commands for task lifecycle operations
5. **CI/CD governance gate** — Automated checks that block non-compliant PRs

---

## Next Steps

- Read the [Philosophy](philosophy.md) to understand the design principles
- Explore the [Architecture](architecture.md) to see how the five repos integrate
- Check [Governance in Action](governance-in-action.md) for concrete before/after scenarios
- See [Success Metrics](success-metrics.md) to measure whether the framework is working
- Read the [Future Roadmap](future.md) for upcoming features
