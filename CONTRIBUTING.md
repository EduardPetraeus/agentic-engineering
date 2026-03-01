# Contributing to Agentic Engineering

Thank you for your interest in contributing to the Agentic Engineering ecosystem. This document explains how to contribute effectively.

---

## What Can You Contribute?

This is a **documentation-only** repository. Contributions include:

- **Fix broken links** — Cross-references between docs, external URLs, repo links
- **Improve clarity** — Rewrite confusing sections, add examples, fix typos
- **Add content** — New guides, tutorials, reference pages, or glossary entries
- **Report issues** — File bugs for incorrect information, missing docs, or broken examples

For code contributions (governance framework, task engine, engineering standards, scaffolder), contribute to the relevant repo:

| Contribution type | Repo |
|---|---|
| Governance rules, patterns, compliance | [ai-governance-framework](https://github.com/EduardPetraeus/ai-governance-framework) |
| Task schema, commands, workflow | [ai-project-management](https://github.com/EduardPetraeus/ai-project-management) |
| Code conventions, linting, testing | [ai-engineering-standards](https://github.com/EduardPetraeus/ai-engineering-standards) |
| Scaffolder, templates, stacks | [ai-project-templates](https://github.com/EduardPetraeus/ai-project-templates) |

---

## How to Contribute

### 1. Open an Issue First

Before submitting a pull request, open an issue describing:

- What you want to change
- Why the change is needed
- Which files are affected

This ensures we agree on the direction before you invest time writing.

### 2. Fork and Branch

```bash
# Fork the repo on GitHub, then:
git clone https://github.com/YOUR-USERNAME/agentic-engineering.git
cd agentic-engineering
git checkout -b fix/your-change-description
```

### 3. Make Your Changes

- All content must be in **English**
- File names use **kebab-case**: `my-new-guide.md`
- Cross-references use **relative links**: `[file](./path/file.md)`
- Diagrams use **Mermaid syntax** in fenced code blocks
- No placeholder text, no TODOs, no empty sections

### 4. Verify Locally

If you have MkDocs Material installed:

```bash
pip install mkdocs-material
mkdocs build --strict
mkdocs serve
```

The `--strict` flag ensures all internal links resolve. Fix any warnings before submitting.

### 5. Submit a Pull Request

```bash
git add -A
git commit -m "docs: description of your change"
git push -u origin fix/your-change-description
```

Open a PR against `main` with:

- A clear title describing the change
- A body explaining what and why
- Reference to the related issue (e.g., "Fixes #12")

---

## Style Guide

### Writing style

- Write in present tense, active voice
- Be specific and actionable — no vague advice
- Use code blocks for commands, file paths, and config examples
- Keep paragraphs short (3-5 sentences maximum)

### File structure

- Every document starts with a level-1 heading (`#`)
- Use `---` horizontal rules to separate major sections
- End every document with a "See Also" section linking to related docs

### Tables

Use tables for structured comparisons. Keep columns aligned:

```markdown
| Column A | Column B | Column C |
|---|---|---|
| Value 1 | Value 2 | Value 3 |
```

---

## Code of Conduct

All contributors must follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

---

## Questions?

Open an issue with the label `question` and we will respond.
