# Philosophy of Agentic Engineering

Agentic engineering is the discipline of building software with AI agents as first-class participants in the development process. Not as autocomplete tools. Not as chat assistants. As agents that read requirements, write code, make architectural decisions, and produce working software.

This changes everything about how we organize software development. This document explains why.

---

## Separation of Concerns for AI Governance

Traditional software development separates concerns at the code level: business logic from data access, presentation from domain. Agentic engineering requires a new separation at the *governance* level.

Three questions must be answered independently:

| Question | Concern | Repo |
|---|---|---|
| What may the agent do? | Governance | ai-governance-framework |
| What should the agent do? | Project management | ai-project-management |
| How should the agent do it? | Engineering standards | ai-engineering-standards |

These are distinct responsibilities. They evolve at different speeds. Governance changes when security requirements change. Project management changes when priorities shift. Engineering standards change when the team agrees on new conventions.

Mixing them in a single file — a monolithic `CLAUDE.md` that tries to cover governance, tasks, and code standards — creates a document that is too long to read, too coupled to change safely, and too ambiguous for an AI agent to parse reliably.

Separation of concerns is not just good architecture. For AI agents, it is a prerequisite for reliable behavior.

---

## Why YAML Over Markdown for Structured Data

Markdown is for humans. YAML is for machines that also need to be readable by humans.

When an AI agent reads a task, it needs to extract structured fields: priority, status, acceptance criteria, dependencies. Markdown can express these things, but inconsistently. One author uses bullet points, another uses headers, a third uses bold labels. The AI agent has to *guess* the structure every time.

YAML eliminates guessing:

```yaml
id: "042"
title: "Implement rate limiting"
priority: high
status: active
depends_on: ["039", "041"]
acceptance_criteria:
  - "100 requests per minute per API key"
  - "429 response with Retry-After header"
  - "Rate limit state stored in Redis"
confidence_ceiling: 0.85
```

Every field is explicit. Every value is parseable. The AI agent does not need to interpret — it reads. This is the difference between documentation and configuration.

The rule: **if data has structure, use YAML. If content is narrative, use Markdown.** Task definitions are structured. Philosophy documents are narrative. Do not mix them.

---

## Why One File Per Task

A single `TODO.md` or `backlog.md` with 50 tasks is convenient for humans scanning a list. It is terrible for AI agents and for version control.

Problems with monolithic task files:

- **Merge conflicts**: Two agents working on different tasks edit the same file
- **Ownership ambiguity**: Which agent is responsible for which section?
- **Status tracking**: Changing the status of one task creates a diff that touches the entire file
- **Lifecycle**: Completing a task means deleting lines from a shared file instead of moving a file to `done/`

One file per task solves all of these:

```
tasks/
├── backlog/
│   ├── 043-add-export-feature.yaml
│   └── 044-fix-pagination-bug.yaml
├── active/
│   └── 042-implement-rate-limiting.yaml
└── done/
    ├── 039-setup-redis.yaml
    └── 041-api-key-auth.yaml
```

Moving a task from backlog to active is a `git mv`. Completing it is another `git mv`. The diff is clean. The history is clear. And two agents can work on two different tasks without touching each other's files.

---

## The 85% Confidence Ceiling

AI agents are confident. They will produce an answer to any question, write code for any requirement, and make decisions about any architectural choice. Their confidence does not correlate with their accuracy.

The confidence ceiling is a governance mechanism: **no AI agent action with a confidence score above 85% should be auto-approved without human review.**

Why 85%?

- Below 60%: The agent should flag the task as uncertain and request guidance. Proceeding would be reckless.
- 60-85%: The agent proceeds but marks the output for review. Standard operating range.
- Above 85%: The agent is likely overconfident. This is where invisible errors happen — code that looks correct, passes basic tests, but contains subtle logic flaws or security gaps.

The 85% ceiling is not about distrusting the AI. It is about recognizing that the highest-confidence outputs are the ones humans are least likely to scrutinize — and therefore the most dangerous when wrong.

Implementation: the agent self-reports a confidence score per task or decision. The governance framework defines thresholds. The CI/CD pipeline enforces them.

---

## The Three-Level Framework

Agentic engineering operates at three levels:

### Level 1: Define What (Constitution)

Before the agent writes a single line of code, define its boundaries:

- What files it may create or modify
- What actions require human approval
- What security constraints it must follow
- What quality thresholds it must meet

This is the `CLAUDE.md` constitution. It is non-negotiable. The agent reads it at the start of every session and operates within its constraints.

### Level 2: AI Validates AI (Automated Enforcement)

Human review does not scale with AI-speed code generation. If the agent produces 50 commits per day, a human cannot meaningfully review all of them.

The solution: automated validation layers.

- Pre-commit hooks that enforce governance rules
- CI pipelines that run quality gates
- Review agents that validate the output of coding agents
- Health score calculators that measure governance compliance

AI validates AI. Humans validate the validation system. This is the leverage point.

### Level 3: Design for Reversibility

Every agent action should be reversible. This is the safety net under the other two levels.

- Feature branches, not direct commits to main
- Database migrations that support rollback
- Infrastructure as code, not manual changes
- Small, atomic commits over large monolithic changes

If governance fails and validation misses something, reversibility means the damage is contained and fixable. This is not optional. It is the assumption that makes the other levels possible.

---

## Agentic Engineering as a Discipline

Software engineering is the discipline of building software that works. Agentic engineering is the discipline of building software that works *when AI agents are part of the process*.

It requires new skills:

- **Constitutional design**: Writing governance rules that are precise enough for machines and flexible enough for real projects
- **Task decomposition for agents**: Breaking work into units that an AI agent can complete autonomously within defined boundaries
- **Validation architecture**: Designing automated systems that catch agent errors before they reach production
- **Confidence calibration**: Understanding when to trust agent output and when to review it manually

These skills do not replace traditional software engineering. They extend it. A developer who masters agentic engineering does not write less code — they ship more working software with higher confidence and lower risk.

This is the direction software development is moving. The question is not whether AI agents will build your software. It is whether they will build it with governance or without it.
