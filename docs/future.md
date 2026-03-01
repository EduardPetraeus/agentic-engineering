# Future Roadmap

The current ecosystem is five repos with file-based integration. This is the foundation. The vision is significantly larger.

This document outlines the direction — not promises, but the trajectory the ecosystem is built to support.

---

## MCP Server Distribution

**Status:** Research phase

Today, a project repo contains local copies of governance rules, task templates, and engineering standards. This works, but it means every project is a snapshot. Updates require manual diffing.

The Model Context Protocol (MCP) changes this. MCP allows AI agents to fetch context from external servers at runtime. This enables a new distribution model:

```
Your Project Repo                    MCP Servers
┌───────────────────┐         ┌─────────────────────────┐
│ CLAUDE.md          │         │ governance-mcp-server   │
│  → fetch governance│────────>│  Serves latest rules    │
│    via MCP         │         │  Per-project overrides  │
│                    │         └─────────────────────────┘
│ tasks/             │         ┌─────────────────────────┐
│  → local (yours)   │         │ standards-mcp-server    │
│                    │         │  Serves latest standards │
│ standards/         │         │  Per-stack standards    │
│  → fetch standards │────────>│  Version-pinned         │
│    via MCP         │         └─────────────────────────┘
└───────────────────┘
```

Benefits:

- **Always current:** Agents fetch the latest governance rules without manual updates
- **Version pinning:** Projects can pin to a specific framework version
- **Per-project overrides:** MCP server merges base rules with project-specific config
- **Zero local files:** Governance and standards live in the server, not in every repo

This does not eliminate local files. It makes them optional for teams that prefer centralized management.

---

## Governance-as-Code SDK

**Status:** Planned for v0.4.0+

A Python library that wraps the governance framework for programmatic use:

```python
from agentic_governance import GovernanceEngine

engine = GovernanceEngine.from_claude_md("./CLAUDE.md")

# Validate an agent action before execution
result = engine.validate_action(
    agent="code-reviewer",
    action="modify_file",
    target="src/auth/login.py",
    confidence=0.78
)

if result.approved:
    # Proceed with the action
    pass
elif result.requires_review:
    # Flag for human review
    notify_reviewer(result.reason)
else:
    # Block the action
    log_blocked_action(result)

# Calculate health score programmatically
score = engine.health_score()
print(f"Governance health: {score.percentage}%")
print(f"Missing: {score.gaps}")
```

Use cases:

- **Custom CI/CD integration:** Run governance checks in any pipeline, not just the provided templates
- **Agent orchestration:** Multi-agent systems validate actions through a shared governance engine
- **Dashboards:** Build internal tools that display governance health across all repos
- **Testing:** Write tests that verify governance rules behave as expected

The SDK reads the same YAML and Markdown files the framework already uses. No new configuration format. No lock-in.

---

## Community Patterns

**Status:** Infrastructure planned

The governance framework ships with 19 patterns (blast-radius-control, automation-bias-defense, context-boundaries, etc.). These are the patterns we have identified and validated.

The community will discover more. The pattern library should grow organically:

- **GitHub Discussions integration:** Propose new patterns as discussions, promote validated ones to the repo
- **Pattern template:** Standardized format so community patterns are consistent with core patterns
- **Cross-reference automation:** When a new pattern is added, automatically identify which existing patterns it relates to
- **Pattern maturity levels:** Draft, community-reviewed, core-maintained

Patterns are the highest-leverage contribution type. A single well-defined pattern (like "confidence ceiling" or "blast radius control") can prevent an entire class of AI agent failures.

---

## Enterprise Features

### Multi-Team Orchestration

Solo developers govern one agent. Teams govern several. Enterprises govern hundreds of agents across dozens of teams. This requires orchestration:

- **Team-scoped governance:** Base rules plus team-specific overrides
- **Cross-team consistency:** Shared security protocols, divergent code standards
- **Agent fleet management:** Dashboard showing which agents are active, what they are doing, and whether they are compliant
- **Handoff protocols:** When an agent in Team A produces output consumed by Team B, governance spans the boundary

### Cost Governance

AI agents consume API tokens. Without cost governance, a runaway agent can burn through budget in hours.

- **Per-agent budget caps:** Maximum token spend per session, per day, per sprint
- **Cost attribution:** Which agent, which task, which team consumed what
- **Alert thresholds:** Notify when an agent approaches its budget limit
- **ROI tracking:** Tokens spent vs. tasks completed — is the agent cost-effective?

### Compliance Mapping

Enterprises need to map AI agent governance to existing compliance frameworks:

- **SOC 2 Type II:** Map governance controls to SOC 2 trust service criteria
- **ISO 42001:** AI management system standard — map framework layers to ISO requirements
- **EU AI Act:** Risk classification of AI agents, documentation of decision-making, human oversight requirements
- **Internal audit:** Generate evidence packages showing governance compliance

The governance framework already has the structure for this. The enterprise features add the reporting, aggregation, and multi-tenant support that compliance teams need.

---

## Distribution Models

The ecosystem currently requires cloning repos. Future distribution options:

| Model | Description | Timeline |
|---|---|---|
| **Git submodules** | Reference framework repos as submodules in your project | Available now |
| **Package managers** | `pip install agentic-governance` / `npm install @agentic/governance` | Post-SDK |
| **MCP servers** | Agents fetch governance at runtime via MCP protocol | Research phase |
| **GitHub Actions marketplace** | Governance checks as reusable GitHub Actions | Planned |
| **VS Code extension** | Governance health indicator in the editor | Exploratory |

---

## Non-Goals

To keep the scope honest, here is what this ecosystem will not become:

- **An AI agent itself:** This is governance infrastructure, not another coding agent
- **A project management tool:** It defines task format, not a UI for managing tasks
- **A CI/CD platform:** It provides governance checks, not a build system
- **A proprietary product:** The core will always be MIT-licensed and open source

---

## Contributing to the Roadmap

The roadmap is shaped by real usage. If you are using the framework and hit a gap:

1. Open an issue in the relevant repo
2. Describe the use case, not just the feature request
3. If possible, propose a pattern or solution

The best features come from practitioners who discover what the framework cannot handle yet.
