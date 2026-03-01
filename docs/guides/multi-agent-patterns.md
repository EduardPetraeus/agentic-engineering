# Multi-Agent Patterns

Running multiple AI agents in parallel is one of the highest-leverage capabilities in agentic engineering. This guide covers the patterns, practices, and pitfalls of parallel agent execution.

---

## Why Parallel Agents

A single AI agent working sequentially is fast. Four agents working in parallel on independent tasks is transformative. The benefits are:

- **Speed** — Four repos updated simultaneously instead of sequentially.
- **Independence** — Each agent operates in its own scope with no interference.
- **Scope isolation** — An agent working on governance cannot accidentally break engineering standards.
- **Fault tolerance** — If one agent fails, the other three continue unaffected.

The key constraint: agents must work on **independent** tasks. Parallel execution with dependencies between agents creates race conditions and coordination overhead that eliminates the speed advantage.

---

## Core Pattern: One Agent Per Repo

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

Each agent receives a self-contained prompt with everything it needs. No agent depends on the output of another. All four run simultaneously and report back when done.

### Implementation

```bash
# In Claude Code, use run_in_background for each agent
# Each agent gets its own working directory and detailed prompt
```

The orchestrator reads context files first, then dispatches agents with detailed, self-contained prompts. This is the critical step — agents do not share context, so each prompt must include all necessary information.

---

## Agent Prompt Engineering

The quality of parallel agent output depends almost entirely on prompt quality. Vague prompts produce vague results. Detailed prompts produce precise, correct output.

### What to include in every agent prompt

1. **Exact file paths** — Not "update the config file" but "edit `/path/to/pyproject.toml` line 42."
2. **Code snippets** — Include the current code the agent will modify. Do not assume the agent will read it correctly on its own.
3. **Acceptance criteria** — Explicit, testable conditions for what "done" means.
4. **API signatures** — If the agent must match an interface, include the exact function signatures.
5. **Context from other repos** — If the change must align with another repo, embed the relevant context in the prompt.

### Example: Good vs. bad prompt

**Bad prompt:**
> Update the scaffolder to support the new governance format.

**Good prompt:**
> In `/path/to/ai-project-templates/scaffold.py`, update the `copy_governance_files()` function (line 87-112) to copy `AGENTS.md` as a symlink to `CLAUDE.md`. The symlink command is `os.symlink("CLAUDE.md", os.path.join(output_dir, "AGENTS.md"))`. Add this after the existing `CLAUDE.md` copy on line 95. Acceptance criteria: (1) `AGENTS.md` exists as a symlink in scaffolded output, (2) symlink resolves to `CLAUDE.md`, (3) existing tests still pass.

The second prompt eliminates ambiguity. The agent knows exactly what to change, where to change it, and how to verify the result.

---

## Pre-Read Key Files Before Dispatch

The orchestrator must read key files before launching agents. This enables embedding correct details in prompts instead of letting agents guess.

**Pre-read checklist:**

- `scaffold.py` — Current function signatures and line numbers
- `CLAUDE.md` templates — Current structure and required sections
- Existing tests — What tests exist and what they validate
- Configuration files — `pyproject.toml`, `ruff.toml`, CI workflows

Read these files in the orchestrator, extract the relevant sections, and embed them directly in the agent prompts.

---

## Agent Isolation Strategies

### Git worktrees

For agents working on the same repo (different branches), use git worktrees:

```bash
git worktree add ../agent-1-workspace feature/agent-1-task
git worktree add ../agent-2-workspace feature/agent-2-task
```

Each agent operates in its own working directory with its own branch. No merge conflicts during execution. Merge happens after all agents complete.

### Separate repos

For cross-repo work, each agent works in a different repository. This is the simplest isolation model — no worktrees needed, no branch conflicts possible.

### Container isolation

For high-security or enterprise use cases, run each agent in its own container with limited filesystem access. The agent can only see and modify its assigned repository.

---

## Status Reporting

Agents should report their completion status. Two common patterns:

### Completion summary

The agent returns a structured summary when it finishes:

```yaml
agent: governance-updater
status: completed
files_modified:
  - templates/CLAUDE.md
  - tests/test_claude_template.py
changes_summary: "Added AGENTS.md symlink requirement to governance template"
```

### STATUS.md file

The agent writes a `STATUS.md` file in its working directory:

```markdown
# Agent Status
- Task: Update governance template
- Status: Complete
- Files modified: 2
- Tests: All passing
```

!!! note
    STATUS.md is useful for long-running agents or when the orchestrator needs to check progress. For agents that run to completion quickly, the completion summary pattern is simpler and avoids cleanup.

---

## Error Handling

When running agents in parallel, failures must be contained:

1. **Log failures, do not block other agents.** If Agent 2 fails, Agents 1, 3, and 4 should continue.
2. **Collect all results before deciding next steps.** Wait for all agents to finish, then review which succeeded and which failed.
3. **Retry failed agents independently.** Re-dispatch only the failed agent with the same prompt, or a corrected prompt if the failure was due to a prompt issue.
4. **Never cascade failures.** If agents are truly independent, one failure should have zero impact on others.

---

## When to Use Parallel vs. Sequential

| Scenario | Approach | Why |
|---|---|---|
| Cross-repo updates with no dependencies | **Parallel** | Maximum speed, no coordination needed |
| Feature that spans multiple repos with shared interfaces | **Sequential** | Agent 2 needs Agent 1's output |
| Documentation updates across repos | **Parallel** | Docs are independent |
| Refactoring with shared test fixtures | **Sequential** | Changes must be compatible |
| Initial scaffold + customize + CI setup | **Sequential** | Each step depends on the previous |

The decision rule: **if Agent B needs the output of Agent A, run them sequentially. Otherwise, run them in parallel.**

---

## Real Example: Four Agents, Four Repos

Scenario: Ship v1.0 across all four framework repos simultaneously.

**Orchestrator pre-reads:**

- `scaffold.py` current API
- `CLAUDE.md` template current structure
- Test files in each repo
- CI workflow files

**Dispatch:**

| Agent | Repo | Task | Key prompt details |
|---|---|---|---|
| 1 | ai-governance-framework | Add AGENTS.md symlink support | Exact function to modify, test to add |
| 2 | ai-project-management | Update task schema with new fields | Current schema, new field definitions |
| 3 | ai-engineering-standards | Add ruff configuration defaults | Current config, target ruff version |
| 4 | ai-project-templates | Update scaffolder for v1.0 output | scaffold.py line numbers, new tree structure |

All four agents run simultaneously. Each gets a prompt with 50-100 lines of specific context. All complete within minutes.

---

## Lessons Learned

1. **Detailed prompts beat vague prompts** — Every minute spent crafting a precise prompt saves five minutes of debugging agent output.
2. **Agents do not share context** — Each agent is a fresh session. Include everything it needs.
3. **Pre-read before dispatch** — The orchestrator is the single source of truth. Read first, then dispatch.
4. **STATUS.md is often unnecessary** — Agents that run to completion return results directly. STATUS.md adds cleanup overhead without value for short-lived agents.
5. **Four parallel agents is a sweet spot** — Enough for meaningful speedup, few enough to review all outputs manually.

---

## See Also

- [Context Architecture](./context-architecture.md) — How to structure the context files agents read
- [Architecture](../architecture.md) — How the five repos connect
- [From Zero to v1.0](../tutorials/from-zero-to-v1.md) — End-to-end tutorial
