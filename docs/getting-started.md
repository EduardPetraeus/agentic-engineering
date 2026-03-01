# Getting Started

This guide walks you through setting up the agentic engineering ecosystem from scratch. By the end, you will have a project repo where your AI agent operates within defined governance, follows a task system, and writes code to explicit engineering standards.

**Time required:** 10-15 minutes.

---

## Prerequisites

- Git installed
- Python 3.10+ (for the scaffolder)
- An AI coding agent: Claude Code, Cursor, Windsurf, or any editor that reads `CLAUDE.md`

---

## Step 1: Clone the Ecosystem

Clone all five repositories into a single parent directory:

```bash
mkdir agentic-ecosystem && cd agentic-ecosystem

git clone https://github.com/EduardPetraeus/agentic-engineering.git
git clone https://github.com/EduardPetraeus/ai-governance-framework.git
git clone https://github.com/EduardPetraeus/ai-project-management.git
git clone https://github.com/EduardPetraeus/ai-engineering-standards.git
git clone https://github.com/EduardPetraeus/ai-project-templates.git
```

Your directory structure should now look like this:

```
agentic-ecosystem/
├── agentic-engineering/          # This repo — docs and philosophy
├── ai-governance-framework/      # Constitutional governance
├── ai-project-management/        # Task engine
├── ai-engineering-standards/     # Code conventions and quality
└── ai-project-templates/         # Scaffolder
```

---

## Step 2: Create a New Project

### Option A: Use the Scaffolder (Recommended)

The scaffolder creates a new project repo with governance, task structure, and engineering standards pre-configured:

```bash
cd ai-project-templates

python scaffold.py \
  --name my-saas-app \
  --stack python-data \
  --mode solo \
  --output-dir ../
```

This creates:

```
my-saas-app/
├── CLAUDE.md                        # Agent constitution (governance)
├── AGENTS.md -> CLAUDE.md           # Agent-agnostic alias (symlink)
├── CONTEXT.md                       # Project context for agent sessions
├── ARCHITECTURE.md                  # Architecture decisions and patterns
├── README.md                        # Project README
├── .gitignore                       # Git ignore rules
├── .claude/
│   └── commands/
│       ├── pick-next-task.md        # Agent command: select next task
│       ├── complete-task.md         # Agent command: mark task done
│       └── create-task.md          # Agent command: create new task
├── backlog/
│   └── TASK-001.yaml                # First task template
├── .engineering/
│   ├── ruff.toml                    # Linter configuration
│   └── pyproject.toml               # Project tooling config
├── docs/
│   └── adr/
│       └── ADR-000-template.md      # Architecture Decision Record template
├── src/                             # Your source code
├── tests/                           # Your tests
└── .github/
    └── workflows/
        └── ci.yml                   # CI/CD pipeline
```

### Option B: Manual Setup

If you prefer to set things up by hand, or want to add governance to an existing repo:

```bash
# Create project directory
mkdir my-saas-app && cd my-saas-app
git init

# Copy governance constitution
cp ../ai-governance-framework/templates/CLAUDE.md ./CLAUDE.md

# Create agent-agnostic alias
ln -s CLAUDE.md AGENTS.md

# Create task structure
mkdir -p backlog
mkdir -p .claude/commands

# Copy a task template
cp ../ai-project-management/templates/task-template.yaml ./backlog/TASK-001.yaml

# Copy engineering standards
mkdir -p .engineering
cp ../ai-engineering-standards/templates/ruff.toml ./.engineering/ruff.toml
cp ../ai-engineering-standards/templates/pyproject.toml ./.engineering/pyproject.toml
```

---

## Step 3: Customize Your CLAUDE.md

Open `CLAUDE.md` in your project and customize it for your specific context:

```bash
# Open in your editor
code my-saas-app/CLAUDE.md
```

Key sections to customize:

1. **project_context** — Set your repo name, purpose, stack, and status
2. **conventions** — Adjust naming conventions, language, and file structure rules
3. **security_protocol** — Add any project-specific security constraints
4. **quality_standards** — Set your testing requirements and review thresholds

The template provides sensible defaults. You only need to change what is specific to your project.

---

## Step 4: Create Your First Tasks

Tasks are YAML files — one file per task, stored in `backlog/`:

```yaml
# backlog/TASK-001.yaml
id: "001"
title: "Set up database schema"
priority: high
status: backlog
description: |
  Create the initial database schema for user accounts
  and authentication. Use PostgreSQL with SQLAlchemy ORM.
acceptance_criteria:
  - User table with id, email, password_hash, created_at
  - Migration script that creates tables from scratch
  - Seed script with 3 test users
  - All migrations reversible
estimated_complexity: medium
```

The AI agent reads these task files to understand what to build next. It picks up priority, acceptance criteria, and complexity — no verbal re-explanation needed.

---

## Step 5: Start Working

Open your project in your AI-assisted editor:

```bash
cd my-saas-app

# If using Claude Code:
claude

# If using Cursor or another editor:
# Just open the directory — the editor reads CLAUDE.md automatically
```

The AI agent will:

1. Read `CLAUDE.md` and understand the governance rules
2. Check `backlog/` for available work
3. Follow engineering standards from `.engineering/`
4. Operate within the defined boundaries

---

## What Happens Next

With the ecosystem in place, your development workflow looks like this:

1. **Define work** — Create task YAML files in `backlog/`
2. **Agent picks up work** — It reads the task, creates a branch, and starts coding
3. **Governance enforces boundaries** — The agent stays within its constitutional limits
4. **Standards ensure quality** — Code style, testing, and architecture follow defined rules
5. **You review and ship** — Human review, merge, deploy

The agent handles the implementation. You handle the direction.

---

## Next Steps

- Read the [Philosophy](./philosophy.md) to understand why the system is designed this way
- Explore the [Architecture](./architecture.md) to see how the repos connect
- Check the [Future Roadmap](./future.md) for upcoming features

---

## Troubleshooting

**Agent ignores CLAUDE.md:** Make sure the file is in the repository root, not in a subdirectory. Some editors only read `CLAUDE.md` from the workspace root.

**Scaffolder fails:** Ensure Python 3.10+ is installed and you are running from within the `ai-project-templates` directory.

**Tasks not picked up:** Verify the YAML syntax is valid. Use a YAML linter or `python -c "import yaml; yaml.safe_load(open('backlog/TASK-001.yaml'))"` to test.
