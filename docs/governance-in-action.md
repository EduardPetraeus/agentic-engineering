# Governance in Action

Abstract frameworks are hard to evaluate. This page shows concrete, real-world scenarios where governance prevents actual mistakes that AI agents make when operating without boundaries.

---

## Scenario 1: The Secret Committer

An AI agent is implementing a weather API connector. It needs an API key to test.

### Without governance

The agent hardcodes the API key directly in the source file to make the tests pass:

```python
# src/bronze/weather.py
import requests

API_KEY = "sk-abc123def456ghi789"  # OpenWeather API key

def fetch_weather(city: str) -> dict:
    url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}"
    return requests.get(url).json()
```

The agent commits this file. The secret is now in git history. Even if someone catches it later and removes the line, the key remains in the commit log permanently. If the repo is public, the key is exposed.

### With governance

CLAUDE.md contains a security protocol:

```markdown
## security_protocol

- Never commit secrets, API keys, tokens, or credentials to source code
- All secrets must be loaded from environment variables
- .gitignore must include .env, *credentials*, *secret*
```

The governance gate CI workflow also runs a secret scanner on every PR diff. The agent, following CLAUDE.md, writes:

```python
# src/bronze/weather.py
import os
import requests

def fetch_weather(city: str) -> dict:
    api_key = os.environ["OPENWEATHER_API_KEY"]
    url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}"
    return requests.get(url).json()
```

**What governance prevented:** A leaked API key in git history. The constitutional rule blocked it at the source. The CI secret scanner acts as a safety net if the agent somehow misses the rule.

---

## Scenario 2: The Convention Breaker

A team of three developers each uses an AI agent. They are building a data pipeline with bronze, silver, and gold layers.

### Without governance

Developer A's agent names files with `camelCase`: `fetchWeather.py`, `transformData.py`.
Developer B's agent uses `PascalCase`: `FetchWeather.py`, `TransformData.py`.
Developer C's agent uses `snake_case`: `fetch_weather.py`, `transform_data.py`.

All three are valid Python. All three pass tests. But the codebase looks like three different projects stitched together. New developers (human or AI) waste time figuring out which convention to follow. Imports break when one agent refactors a file using its preferred naming.

### With governance

CLAUDE.md defines conventions:

```markdown
## conventions

- File names: kebab-case (e.g., fetch-weather.py)
- Functions and variables: snake_case
- Classes: PascalCase
- Modules: snake_case
```

Every agent reads the same CLAUDE.md and follows the same rules. The CI pipeline runs `ruff check` with the team's configuration, catching any deviations before merge. The codebase is consistent regardless of which developer or agent wrote the code.

**What governance prevented:** A fragmented codebase with inconsistent naming that compounds over time. Three weeks of divergence can take days to reconcile.

---

## Scenario 3: The Overconfident Refactorer

An AI agent is asked to add a small feature: rate limiting on an API endpoint. The feature itself is straightforward.

### Without governance

The agent notices that the authentication module "could be improved." Without being asked, it refactors `src/auth/login.py`, `src/auth/tokens.py`, and `src/auth/middleware.py` — touching 200 lines of working, tested code. The refactoring is technically correct but introduces a subtle timing issue in the token refresh flow that only manifests under load.

The PR diff is 400 lines. The human reviewer sees the rate limiting change (30 lines) buried in a massive refactoring diff. They approve it because the tests pass. The timing issue reaches production.

### With governance

CLAUDE.md defines blast radius controls:

```markdown
## quality_standards

- Only modify files directly related to the current task
- If a refactoring opportunity is discovered, create a new task in backlog/ — do not refactor in the current PR
- Maximum blast radius: changes should not touch more than 5 files per task
```

The agent adds rate limiting (the assigned task) and creates `backlog/TASK-047.yaml` for the auth refactoring it noticed. The PR is 30 lines, focused, and easy to review. The refactoring gets its own task, its own branch, and its own review.

**What governance prevented:** An unrequested, large-scope refactoring bundled into a small feature PR. The blast radius control keeps changes focused and reviewable.

---

## Scenario 4: The Context Amnesiac

A developer works on a project for a week, then takes two weeks off. They return and start a new AI agent session.

### Without governance

The agent has no memory of previous sessions. It does not know what was built, what decisions were made, or what the current state of the project is. The developer spends 30 minutes re-explaining context that was already established. The agent proposes changes that contradict decisions made two weeks ago.

### With governance

The session protocol in CLAUDE.md requires the agent to read context files at the start of every session:

```markdown
## mandatory_session_protocol

### Start
1. Read CLAUDE.md — understand governance rules
2. Read CONTEXT.md — understand current project state
3. Read backlog/ — understand available work
4. Read ARCHITECTURE.md — understand system design
```

CONTEXT.md was updated at the end of the last session:

```markdown
## Current State
- Bronze layer complete (TASK-001, TASK-002 done)
- Silver layer joins in progress (TASK-003 active)
- Decision: DuckDB over SQLite for local analytics (ADR-002)

## Next Session Focus
- Complete TASK-003: silver layer joins
- Add integration tests for bronze-to-silver pipeline
```

The agent picks up exactly where the last session left off. No re-explanation needed. No contradicting previous decisions.

**What governance prevented:** 30+ minutes of lost context recovery per session. Over a project's lifetime, this compounds into hours of wasted time and inconsistent decisions.

---

## Scenario 5: The Unreviewed Merge

An agent produces 12 commits in a day. The developer, trusting the agent, merges them all without careful review.

### Without governance

Three of the 12 commits contain subtle issues: a SQL injection vulnerability in a user input handler, a missing index on a frequently queried column, and a logging statement that prints full request bodies (including sensitive data). None of these break tests. All of them create production risks.

### With governance

The confidence ceiling mechanism flags high-confidence agent output for human review:

```markdown
## quality_standards

- Confidence ceiling: 85%
- Outputs above 85% confidence are flagged for mandatory human review
- Security-related changes always require human review regardless of confidence
```

The governance gate workflow enforces review requirements:

- The SQL injection risk is caught by the security scan job
- The missing index is flagged because database changes require human review
- The logging issue is caught because the agent self-reports 90% confidence on that change, triggering mandatory review

**What governance prevented:** Three production-risk issues shipped in a single day. The governance layers — confidence ceiling, security scan, and mandatory review for high-risk changes — caught what blind trust would have missed.

---

## The Pattern

Every scenario above follows the same structure:

1. **Without governance**, the agent does something reasonable-looking that creates a hidden problem.
2. **With governance**, a constitutional rule, automated check, or review requirement catches the issue before it reaches production.

Governance does not slow the agent down. It redirects the agent's speed toward safe outcomes. The agent still writes code at 10x speed — it just writes code that does not create 10x technical debt.

---

## See Also

- [Philosophy](philosophy.md) — The design principles behind governance
- [Tutorial](tutorial.md) — Set up governance in your own project
- [Success Metrics](success-metrics.md) — How to measure governance effectiveness
- [Glossary](reference/glossary.md) — Key terms like confidence ceiling, blast radius, trust profile
