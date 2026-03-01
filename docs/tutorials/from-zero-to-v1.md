# From Zero to v1.0

This tutorial walks you through the entire process of setting up the Agentic Engineering ecosystem, scaffolding a project, configuring governance, creating tasks, and shipping your first release. By the end, you will have a fully governed project with CI/CD, a task backlog, and your first merged PR.

**Time required:** 45-60 minutes.

**Prerequisites:**

- Python 3.11+ installed
- Git installed and configured
- GitHub CLI (`gh`) installed and authenticated (`gh auth login`)
- Claude Code, Cursor, or any AI-assisted editor
- A GitHub account

---

## Step 1: Clone the Ecosystem

Clone all five repositories into a single parent directory:

```bash
mkdir ~/agentic-ecosystem && cd ~/agentic-ecosystem

git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

Verify the structure:

```bash
ls ~/agentic-ecosystem
# agentic-engineering  ai-engineering-standards  ai-governance-framework
# ai-project-management  ai-project-templates
```

Each repo has a single responsibility. Read the [Repo Map](../reference/repo-map.md) for details on each one.

---

## Step 2: Scaffold a New Project

The scaffolder generates a complete project with governance, tasks, and engineering standards from a single command.

```bash
cd ~/agentic-ecosystem/ai-project-templates
pip install pyyaml
```

Run the scaffolder:

```bash
python scaffold.py \
  --name my-app \
  --stack python-data \
  --mode solo \
  --output-dir ~/agentic-ecosystem/
```

This creates `~/agentic-ecosystem/my-app/` with the full project structure.

---

## Step 3: Explore the Generated Structure

```bash
cd ~/agentic-ecosystem/my-app
```

The scaffolder created these files:

```
my-app/
├── CLAUDE.md                     # Agent constitution (governance rules)
├── AGENTS.md -> CLAUDE.md        # Agent-agnostic alias (symlink)
├── CONTEXT.md                    # Living project state
├── ARCHITECTURE.md               # System design document
├── README.md                     # Project README
├── .gitignore                    # Git ignore rules
├── .claude/
│   └── commands/
│       ├── pick-next-task.md     # Agent command: select next task
│       ├── complete-task.md      # Agent command: mark task done
│       └── create-task.md        # Agent command: create new task
├── backlog/
│   └── TASK-001.yaml             # Starter task template
├── .engineering/
│   ├── ruff.toml                 # Linter configuration
│   └── pyproject.toml            # Project tooling config
├── docs/
│   └── adr/
│       └── ADR-000-template.md   # Architecture Decision Record template
├── src/
│   ├── bronze/                   # Raw data ingestion
│   ├── silver/                   # Cleaned and normalized
│   └── gold/                     # Aggregated business views
├── tests/                        # Test directory
└── .github/
    └── workflows/
        └── ci.yml                # CI/CD pipeline
```

Key observations:

- `CLAUDE.md` is your governance constitution. The agent reads this first.
- `AGENTS.md` is a symlink to `CLAUDE.md` for tool compatibility.
- `backlog/` contains your task files (not `tasks/`).
- `.engineering/` holds linting and tooling config (not `standards/`).
- `src/` follows the medallion architecture: bronze, silver, gold.

---

## Step 4: Customize CLAUDE.md

Open `CLAUDE.md` and update the `project_context` section for your specific project:

```bash
# Open in your editor
code ~/agentic-ecosystem/my-app/CLAUDE.md
```

Update these fields:

```markdown
## project_context

- **Repo:** my-app
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

---

## Step 5: Create Your First Task

Replace the starter task in `backlog/TASK-001.yaml` with a real task:

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

Create a second task for the silver layer:

```bash
cp backlog/TASK-001.yaml backlog/TASK-002.yaml
```

Edit `TASK-002.yaml`:

```yaml
id: TASK-002
title: "Transform weather data in silver layer"
description: |
  Read raw JSON from bronze output, extract key fields
  (temperature, humidity, wind_speed, timestamp), and write
  clean CSV files to src/silver/output/weather/.

status: ready
priority: medium
agent: code
review: human-review

depends_on: ["TASK-001"]
blocks: []

definition_of_done:
  - "Reads all JSON files from src/bronze/output/weather/"
  - "Extracts temperature, humidity, wind_speed, timestamp"
  - "Writes clean CSV to src/silver/output/weather/"
  - "Handles malformed JSON gracefully (log and skip)"
  - "Unit tests cover happy path and error cases"

tags: [silver, transformation]
estimate: m
```

---

## Step 6: Let the AI Agent Work

Open the project with your AI agent:

```bash
cd ~/agentic-ecosystem/my-app

# Claude Code
claude

# Or open in Cursor/Windsurf — they read CLAUDE.md automatically
```

The agent will:

1. Read `CLAUDE.md` to understand governance boundaries.
2. Read `backlog/` to find available tasks.
3. Pick the highest-priority `ready` task (TASK-001).
4. Create a feature branch and start implementing.
5. Follow conventions from `.engineering/ruff.toml`.
6. Self-validate against the definition of done.

You can also use agent commands directly:

```
/pick-next-task    # Agent selects the next task from backlog
/complete-task     # Agent marks current task as done
/create-task       # Agent creates a new task YAML file
```

---

## Step 7: Validate Against Engineering Standards

Before committing, run the quality checks:

```bash
# Install linter
pip install ruff

# Run linting
ruff check src/ tests/ --config .engineering/ruff.toml

# Run formatting check
ruff format --check src/ tests/ --config .engineering/ruff.toml

# Run tests
pip install pytest
pytest tests/ -v
```

Fix any issues the linter finds. The CI pipeline will run these same checks automatically on every PR.

---

## Step 8: Run Governance Checks

Validate your project against the governance framework:

```bash
# Copy automation scripts temporarily
cp ~/agentic-ecosystem/ai-governance-framework/automation/drift_detector.py .
cp ~/agentic-ecosystem/ai-governance-framework/automation/content_quality_checker.py .

# Run drift detection
python drift_detector.py \
  --template ~/agentic-ecosystem/ai-governance-framework/templates/CLAUDE.md \
  --target CLAUDE.md \
  --format text

# Run content quality check
python content_quality_checker.py . --format json

# Clean up
rm drift_detector.py content_quality_checker.py
```

A freshly scaffolded project should show high alignment. Fix any gaps before proceeding.

---

## Step 9: Set Up CI/CD

The scaffolder already created `.github/workflows/ci.yml`. Verify it exists:

```bash
cat .github/workflows/ci.yml
```

For the full governance gate (drift detection + quality checks + secret scanning), copy the governance workflow:

```bash
cp ~/agentic-ecosystem/ai-governance-framework/templates/ci-cd/github-actions-governance-gate.yml \
   .github/workflows/governance-gate.yml

# Copy automation scripts for CI
mkdir -p automation
cp ~/agentic-ecosystem/ai-governance-framework/automation/drift_detector.py automation/
cp ~/agentic-ecosystem/ai-governance-framework/automation/content_quality_checker.py automation/
```

---

## Step 10: Make Your First PR

Initialize the repo and push:

```bash
cd ~/agentic-ecosystem/my-app

git init
git add -A
git commit -m "feat: initial scaffold from ai-project-templates"

# Create remote repo
gh repo create my-app --public --source . --push
```

Create a feature branch for your first task:

```bash
git checkout -b feature/task-001-bronze-weather

# ... agent implements the task ...

git add -A
git commit -m "feat(bronze): add weather API connector

Implements TASK-001: bronze layer ingestion for OpenWeather API.
- Fetches current weather data with retry logic
- Stores raw JSON with timestamp filenames
- API key via environment variable
- Unit tests for success, failure, and retry paths"

git push -u origin feature/task-001-bronze-weather
```

Open the PR:

```bash
gh pr create \
  --title "feat(bronze): add weather API connector" \
  --body "Implements TASK-001. See backlog/TASK-001.yaml for acceptance criteria."
```

The governance gate runs automatically. Once all checks pass, review and merge.

---

## Step 11: Ship v1.0

After completing your core tasks:

1. **Update CONTEXT.md** with the current project state.
2. **Move completed tasks** from `backlog/` to a `done/` directory (or update their `status: done`).
3. **Tag the release:**

```bash
git checkout main
git pull origin main
git tag -a v1.0.0 -m "v1.0.0: Initial release with bronze layer ingestion"
git push origin v1.0.0
```

4. **Create a GitHub release:**

```bash
gh release create v1.0.0 \
  --title "v1.0.0" \
  --notes "Initial release. Bronze layer weather API connector with governance and CI/CD."
```

---

## What You Built

By completing this tutorial, you have:

- A **governed project** where AI agents operate within constitutional boundaries
- A **YAML task backlog** that agents read to determine what to build
- **Engineering standards** enforced by ruff in CI
- **Governance gates** that validate compliance on every PR
- **Agent commands** for task lifecycle operations
- A **tagged v1.0.0 release** on GitHub

This is the foundation. From here, add more tasks, let the agent build more features, and iterate. The governance framework ensures quality stays high as the codebase grows.

---

## Next Steps

- Add more tasks to `backlog/` for silver and gold layers
- Read [Multi-Agent Patterns](../guides/multi-agent-patterns.md) to run agents in parallel
- Read [Context Architecture](../guides/context-architecture.md) to optimize your context files
- Check the [Future Roadmap](../future.md) for MCP distribution and SDK plans

---

## See Also

- [Getting Started](../getting-started.md) — Condensed setup guide
- [Quickstart Tutorial](../quickstart-tutorial.md) — Shorter hands-on walkthrough
- [Repo Map](../reference/repo-map.md) — All five repos documented
- [Glossary](../reference/glossary.md) — Key terms defined
