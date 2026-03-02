# STATUS — Workstream 5 (V2 Docs Overhaul)

**Branch:** feature/v2
**Date:** 2026-03-02
**Status:** Complete

---

## Tasks Completed

### Task 1: Deduplicate README.md and docs/index.md
- **README.md** is now the GitHub landing page: badges, project overview, quick start, repo table, design principles, links to docs site.
- **docs/index.md** is now the docs site welcome page: "What You Will Learn", navigation guide, site map, ecosystem overview.
- Content overlap: <20%. README focuses on "what is this and how to install", index.md focuses on "what will you learn and where to start".

### Task 2: Consolidate tutorials
- Merged 3 files into 1 progressive tutorial (`docs/tutorial.md`):
  - **Part 1: Quickstart (10 min)** — Clone scaffolder, scaffold project, start working.
  - **Part 2: Full Setup (30 min)** — Customize CLAUDE.md, create tasks, governance checks, CI/CD, first PR.
  - **Part 3: Advanced Topics** — Multi-agent patterns, context optimization, governance tuning, scaffolder reference.
- Removed: `docs/getting-started.md`, `docs/quickstart-tutorial.md`, `docs/tutorials/from-zero-to-v1.md`.
- Updated mkdocs.yml navigation and all cross-references.

### Task 3: Verify scaffold.py references
- Read actual `scaffold.py` in ai-project-templates repo to verify API.
- Fixed README.md: old command was `--output ../my-project` (wrong), now uses correct `--stack`, `--mode`, `--output-dir` flags.
- Added `databricks-lakehouse` as valid stack option in tutorial scaffolder reference.
- Added `--dry-run` flag documentation.
- All scaffold.py references across docs now match the actual CLI API.

### Task 4: Add governance-in-action.md
- Created `docs/governance-in-action.md` with 5 before/after scenarios:
  1. Secret Committer — Agent hardcodes API key vs. governance blocks it.
  2. Convention Breaker — Three agents, three naming styles vs. governance enforces consistency.
  3. Overconfident Refactorer — Agent refactors 200 lines unrequested vs. blast radius controls.
  4. Context Amnesiac — Lost context after break vs. session protocol with CONTEXT.md.
  5. Unreviewed Merge — 12 commits merged blind vs. confidence ceiling catches issues.
- Added to mkdocs.yml under Concepts.

### Task 5: Success metrics guidance
- Created `docs/success-metrics.md` with:
  - 5 core metrics (violations caught, test pass rate, context recovery time, blast radius, time to ship).
  - 3 derived metrics (governance health score, agent autonomy ratio, review efficiency).
  - How to start measuring (3 practical steps).
  - Diagnostic table (symptom -> cause -> fix).
- Added to mkdocs.yml under Reference.

### Task 6: Improve glossary
- Added rationale paragraphs to all key entries in `docs/reference/glossary.md`:
  - Agent Session: why stateless by design
  - AGENTS.md: why symlink over copy
  - Agentic Engineering OS: why five repos instead of one
  - Builder-Brain: why centralized
  - Backlog: why directory over single file
  - Confidence Ceiling: why 85% specifically (not 80 or 90)
  - Constitution: why the name
  - Context File: why files over prompts
  - Definition of Done: why explicit criteria matter for AI
  - Governance Gate: why automated enforcement
  - Hot Memory: why 50-line limit
  - Cold Memory: why not load everything
  - Medallion Architecture: why three layers
  - Mode: why two modes
  - Scaffolder: why scaffolder over docs
  - Task Engine: why YAML over markdown
  - Trust Profile: why variable trust
- Added `databricks-lakehouse` to Stack entry.

## Additional Fixes
- Fixed `tasks/` -> `backlog/` and `standards/` -> `.engineering/` references in architecture.md and architecture-diagram.md.
- Removed broken link to architecture-diagram.md from architecture.md (file is outside docs/).
- MkDocs builds without warnings in `--strict` mode.

## Files Modified
- `README.md` — Rewritten as GitHub landing page
- `docs/index.md` — Rewritten as docs site welcome
- `docs/tutorial.md` — New consolidated tutorial (created)
- `docs/governance-in-action.md` — New governance scenarios (created)
- `docs/success-metrics.md` — New metrics guidance (created)
- `docs/reference/glossary.md` — Rationale paragraphs added
- `docs/architecture.md` — Fixed directory references, removed broken link, updated cross-refs
- `docs/reference/repo-map.md` — Updated file references
- `docs/guides/multi-agent-patterns.md` — Updated cross-reference
- `assets/architecture-diagram.md` — Fixed directory names
- `mkdocs.yml` — New navigation structure

## Files Removed
- `docs/getting-started.md`
- `docs/quickstart-tutorial.md`
- `docs/tutorials/from-zero-to-v1.md`
