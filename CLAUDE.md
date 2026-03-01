# CLAUDE.md — Agentic Engineering

This file governs all AI agent sessions in this repository.

## project_context

- **Repo:** agentic-engineering
- **Purpose:** Umbrella documentation for the agentic engineering ecosystem
- **License:** MIT
- **Status:** Active development
- **Type:** Docs-only repo — no source code, no tests, no CI/CD
- **Content language:** English only

## related_repos

| Repo | Role | Path |
|---|---|---|
| ai-governance-framework | What may agents do? | ~/Github repos/ai-governance-framework |
| ai-project-management | What to do? | ~/Github repos/ai-project-management |
| ai-engineering-standards | How to do it? | ~/Github repos/ai-engineering-standards |
| ai-project-templates | Scaffolder | ~/Github repos/ai-project-templates |

## conventions

- File names: `kebab-case.md`
- Directory names: `kebab-case/`
- All content in English — no exceptions
- Cross-references use relative links: `[file](./path/file.md)`
- Diagrams use Mermaid syntax in fenced code blocks
- No placeholder text, no TODOs, no empty sections

## mandatory_session_protocol

### Start

1. Read README.md — understand current content and structure
2. Confirm scope with user: which doc, what outcome
3. Do not create or modify files until scope is confirmed

### During

- One document at a time
- After each file: verify cross-references resolve
- Keep README.md as the primary landing page — it must always be self-contained

### End

- Summarize: files created/modified, cross-references updated
- Verify all internal links resolve to existing files

## quality_standards

- Every file has real, usable content — no stubs
- README.md must work as a standalone introduction even without the docs/ folder
- Diagrams must render correctly in GitHub markdown
- Could a developer read this repo and understand the full ecosystem? If not, the content is incomplete.
