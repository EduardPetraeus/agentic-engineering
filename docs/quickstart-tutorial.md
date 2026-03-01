# Quickstart Tutorial

A hands-on walkthrough that takes you from zero to a fully governed, scaffolded project with CI/CD governance gates. By the end, you will have a working project repo with governance, a task backlog, and automated quality checks on every PR.

**Time required:** 20-30 minutes.

**Prerequisites:**
- Git installed
- Python 3.10+
- GitHub CLI (`gh`) installed and authenticated
- A GitHub account with push access to a remote repository

---

## Part 1: Clone All Five Repos

The Agentic Engineering OS is split across five repositories. Clone them all into a single parent directory:

```bash
mkdir agentic-ecosystem && cd agentic-ecosystem

git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

After cloning, your directory structure should look like this:

```
agentic-ecosystem/
├── agentic-engineering/          # Umbrella docs, philosophy, architecture
├── ai-governance-framework/      # Constitutional governance for AI agents
├── ai-project-management/        # YAML-based task engine
├── ai-engineering-standards/     # Code conventions, testing, git workflow
└── ai-project-templates/         # Project scaffolder
```

Each repo has a single responsibility. Together they form the complete operating system for AI-assisted development. See [Architecture](./architecture.md) for how they connect.

---

## Part 2: Run the Scaffolder

The scaffolder generates a complete project with governance, task management, and engineering standards baked in from a single command.

### Install the dependency

```bash
cd ai-project-templates
pip install pyyaml
```

### Run scaffold.py

```bash
python scaffold.py --name my-data-project --stack python-data --mode solo --output-dir ../
```

**Arguments explained:**

| Argument | Required | Description |
|---|---|---|
| `--name` | Yes | Project name (used as the directory name) |
| `--stack` | Yes | Stack type: `python-data`, `python-web`, or `docs-only` |
| `--mode` | Yes | Working mode: `solo` or `team` |
| `--output-dir` | No | Parent directory for the project (default: current directory) |

The three stacks determine which directory structure and conventions are generated:
- **python-data** — Medallion architecture with bronze/silver/gold layers, suited for data engineering and analytics
- **python-web** — Web application structure for APIs and services
- **docs-only** — Documentation-only repos with no source code directories

### Expected output

After running the command, the scaffolder creates this structure:

```
my-data-project/
├── CLAUDE.md                        # Agent constitution (governance)
├── CONTEXT.md                       # Project context for agent sessions
├── ARCHITECTURE.md                  # Architecture decisions and patterns
├── README.md                        # Project README
├── .gitignore                       # Git ignore rules
├── requirements.txt                 # Python dependencies (empty, ready to fill)
├── backlog/
│   └── TASK-001.yaml                # First task template
├── .claude/
│   └── commands/
│       ├── pick-next-task.md        # Agent command: select next task
│       ├── complete-task.md         # Agent command: mark task done
│       └── create-task.md           # Agent command: create new task
├── .engineering/
│   ├── ruff.toml                    # Linter configuration
│   └── pyproject.toml               # Project tooling config
├── docs/
│   └── adr/
│       └── ADR-000-template.md      # Architecture Decision Record template
├── src/
│   ├── bronze/                      # Raw data ingestion layer
│   ├── silver/                      # Cleaned and normalized data
│   └── gold/                        # Aggregated views for consumption
└── tests/                           # Test directory
```

The project is self-contained. It does not require the framework repos at runtime. Your AI agent reads `CLAUDE.md` at session start and operates within its governance boundaries.

> **Note:** The `python-web` and `docs-only` stacks produce different directory structures tailored to their use cases, but the governance files (CLAUDE.md, CONTEXT.md, backlog/, .claude/commands/) are always present.

---

## Part 3: Set Up Your First Backlog Task

The scaffolder creates a starter task at `backlog/TASK-001.yaml`. Replace it with a real task for your project. Here is an example:

```yaml
id: TASK-001
title: "Set up bronze layer ingestion for weather API"
description: |
  Create the initial bronze layer connector that pulls raw data
  from the OpenWeather API and stores it as JSON files in src/bronze/.
  Include error handling, retry logic, and logging.

status: ready
priority: high
agent: code
review: human-review
assignee: null
sprint: null

depends_on: []
blocks: []

definition_of_done:
  - "Connector fetches data from OpenWeather API successfully"
  - "Raw JSON responses saved to src/bronze/weather/"
  - "Retry logic handles transient failures (3 retries with backoff)"
  - "Logging captures request/response metadata"
  - "Unit tests cover success, failure, and retry paths"

tags: [bronze, ingestion, api]
estimate: m

actual_tokens: null
actual_duration_minutes: null
completed: null

created: 2026-03-01
updated: 2026-03-01
notes: "Use requests library. API key via environment variable."
```

### Task fields

| Field | Purpose |
|---|---|
| `id` | Unique identifier (TASK-NNN format) |
| `title` | Short description of the task |
| `description` | Detailed context the agent needs to understand the work |
| `status` | Lifecycle state: `ready`, `active`, `done`, `blocked` |
| `priority` | `high`, `medium`, or `low` |
| `agent` | Which agent type handles this: `code`, `docs`, `test`, `review` |
| `review` | Review requirement: `human-review`, `auto-merge`, `pair-review` |
| `definition_of_done` | Acceptance criteria the agent must satisfy |
| `estimate` | Size: `xs`, `s`, `m`, `l`, `xl` |

### Backlog structure

Tasks live in the `backlog/` directory, one YAML file per task:

```
backlog/
├── TASK-001.yaml
├── TASK-002.yaml
└── TASK-003.yaml
```

The agent reads these files to determine what to build. Priority, acceptance criteria, and dependencies are all machine-readable — no verbal re-explanation needed between sessions.

For the full task schema and lifecycle specification, see the [ai-project-management](https://github.com/EduardPetraeus/ai-project-management) repository.

---

## Part 4: Run Governance Checks Locally

Before pushing your first PR, verify that your project passes governance checks. The ai-governance-framework provides two automation tools for this.

### Copy the automation scripts

```bash
cd my-data-project

cp ../ai-governance-framework/automation/drift_detector.py .
cp ../ai-governance-framework/automation/content_quality_checker.py .
```

### Run drift detection

The drift detector compares your project's CLAUDE.md against the governance framework template to find missing required sections or significant content deviations:

```bash
python drift_detector.py \
  --template ../ai-governance-framework/templates/CLAUDE.md \
  --target CLAUDE.md \
  --format text
```

Expected output shows alignment status, any missing sections, and actionable recommendations. A freshly scaffolded project should show high alignment since the template was just copied.

### Run content quality check

The content quality checker scans your repository for governance files and evaluates whether they meet minimum quality standards — line counts, required sections, structural expectations:

```bash
python content_quality_checker.py . --format json
```

This outputs a JSON report with pass/fail status for each governance file found. Fix any issues before proceeding.

### Clean up (optional)

If you prefer not to keep the automation scripts in your project, you can remove them after verifying. The CI workflow (Part 5) runs these checks automatically on every PR.

```bash
rm drift_detector.py content_quality_checker.py
```

---

## Part 5: Create a PR with Governance Gate

Now wire up automated governance checks so every PR to main is validated before merge.

### Copy the governance gate workflow

```bash
mkdir -p .github/workflows

cp ../ai-governance-framework/templates/ci-cd/github-actions-governance-gate.yml \
   .github/workflows/governance-gate.yml
```

You also need the automation scripts available in CI. Copy them into an `automation/` directory:

```bash
mkdir -p automation

cp ../ai-governance-framework/automation/drift_detector.py automation/
cp ../ai-governance-framework/automation/content_quality_checker.py automation/
```

### Initialize and push

```bash
git init
git add -A
git commit -m "feat: initial scaffold from ai-project-templates"

# Create the remote repository (if it does not exist yet)
gh repo create my-data-project --public --source . --push

# Or if the remote already exists:
git remote add origin https://github.com/YOUR-USERNAME/my-data-project.git
git push -u origin main
```

### Create a feature branch and open a PR

```bash
git checkout -b feature/first-task
# ... make your changes ...
git add -A
git commit -m "feat(bronze): add weather API connector"
git push -u origin feature/first-task

gh pr create \
  --title "feat(bronze): add weather API connector" \
  --body "Implements TASK-001: bronze layer ingestion for the weather API."
```

### What the governance gate checks

The governance gate workflow runs four jobs on every PR:

| Job | What it does |
|---|---|
| **Lint** | Runs `ruff check` and `ruff format --check` to enforce code style |
| **Test** | Runs `pytest` on the `tests/` directory |
| **Security Scan** | Scans the PR diff for secrets (API keys, tokens, passwords) and verifies `.gitignore` includes `.env` |
| **Governance Check** | Verifies CLAUDE.md exists, runs content quality checks, runs drift detection against the template, and scans CLAUDE.md for secrets |

All four jobs must pass before the PR can be merged. This is the automated enforcement layer — AI validates AI, humans validate the system.

To require these checks for merge, enable branch protection on `main` in your GitHub repository settings (Settings > Branches > Add rule > Require status checks to pass).

---

## What You Have Now

After completing this tutorial, your project has:

1. **Governance** — CLAUDE.md defines what the AI agent may and must not do
2. **Task management** — YAML backlog the agent reads to determine what to build
3. **Engineering standards** — Linter config, conventions, and architecture patterns
4. **Agent commands** — Pre-built commands for task lifecycle operations
5. **CI/CD governance gate** — Automated checks that block non-compliant PRs

Open your project with Claude Code or any AI-assisted editor. The agent reads CLAUDE.md automatically and operates within the defined boundaries.

## Next Steps

- Read the [Philosophy](./philosophy.md) to understand the design principles
- Explore the [Architecture](./architecture.md) to see how the five repos integrate
- Review the [Getting Started](./getting-started.md) guide for manual setup and customization details
- Check the [Future Roadmap](./future.md) for upcoming features like MCP distribution and SDK
