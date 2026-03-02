# Success Metrics

How do you know the framework is working? Not by whether you installed it, but by whether it changes outcomes. This page defines concrete metrics for measuring governance effectiveness.

---

## Core Metrics

### 1. Governance Violations Caught

**What it measures:** The number of issues caught by governance gates before they reach main.

**How to track:** Count the number of PR checks that fail due to governance violations (secret detection, drift detection, quality checks, linting).

**Healthy range:**

- First month: 5-15 violations caught per week. This is good — the system is catching real issues.
- After 3 months: 1-5 violations per week. The agent has learned the rules. Violations become edge cases.
- Zero violations consistently: Either governance is working perfectly, or the checks are not strict enough. Audit the rules.

**Warning sign:** Zero violations from day one. This usually means governance checks are too loose, not that the agent is perfect.

### 2. Test Pass Rate on First PR Submission

**What it measures:** The percentage of PRs where all CI checks pass on the first push.

**How to track:** `(PRs passing all checks on first push) / (total PRs) * 100`

**Healthy range:**

- Without governance: 40-60% (agents produce code that looks right but fails linting, missing tests, or style violations).
- With governance (first month): 70-80%.
- With governance (mature): 85-95%.

**Why it matters:** A high first-pass rate means the agent is writing code that meets standards consistently. It means less time in review-fix-resubmit cycles.

### 3. Context Recovery Time

**What it measures:** How long it takes to resume productive work after a break (hours, days, or weeks away from the project).

**How to track:** Time from opening a new agent session to the first meaningful commit.

**Healthy range:**

- Without governance: 20-45 minutes of re-explaining context each session.
- With governance (CONTEXT.md + session protocol): 2-5 minutes. The agent reads context files and picks up where it left off.

**Why it matters:** Over a project's lifetime, context recovery time compounds. At 30 minutes per session, 5 sessions per week, that is 2.5 hours/week wasted on re-explanation.

### 4. Blast Radius per PR

**What it measures:** The average number of files changed per PR.

**How to track:** `git diff --stat` on merged PRs. Count files changed.

**Healthy range:**

- Without governance: Highly variable. Some PRs touch 1 file, others touch 30.
- With governance: 3-8 files per PR. Focused, reviewable changes.

**Warning sign:** PRs consistently touching 15+ files. The agent is likely bundling unrelated changes or performing unrequested refactoring.

### 5. Time to Ship a Task

**What it measures:** The elapsed time from task status `ready` to `done`.

**How to track:** Compare `created` and `completed` timestamps in task YAML files.

**Healthy range:** Depends heavily on task complexity, but track the trend. If medium-complexity tasks took 4 hours in month 1 and take 2 hours in month 3, the framework (better context, clearer tasks, consistent conventions) is reducing friction.

---

## Derived Metrics

### Governance Health Score

The ai-governance-framework includes a health score calculator that evaluates your project's governance compliance:

```bash
# Run from your project directory
python ../ai-governance-framework/automation/content_quality_checker.py . --format json
```

The score measures:

- Presence of required governance files (CLAUDE.md, CONTEXT.md, etc.)
- Required sections within each file
- Minimum content quality (line counts, structural expectations)

**Target:** 80%+ on initial setup. 90%+ after customization.

### Agent Autonomy Ratio

**What it measures:** The percentage of agent actions that are auto-approved vs. requiring human review.

**How to calculate:** `(auto-approved actions) / (total actions) * 100`

**Healthy range:** 60-80%. Below 60% means too much human intervention — the governance is too strict or the trust profiles need tuning. Above 90% means too little oversight — raise the review requirements.

### Review Efficiency

**What it measures:** How much of the human reviewer's time is spent on substance vs. style.

**Without governance:** Reviewers catch formatting issues, naming violations, missing tests, and security gaps alongside actual logic review. Review time: 30-60 minutes per PR.

**With governance:** Automated checks handle formatting, naming, testing, and security scanning. Reviewers focus only on logic, architecture, and business correctness. Review time: 5-15 minutes per PR.

---

## How to Start Measuring

You do not need a dashboard on day one. Start with these three steps:

### Step 1: Track CI check results

Look at your GitHub Actions history. Count how many PR checks fail on first push vs. pass on first push. This gives you test pass rate and a proxy for governance violations caught.

### Step 2: Track context recovery time

At the start of each session, note the time. Note when you make the first meaningful commit. The difference is your context recovery time.

### Step 3: Track blast radius

After each merged PR, run `git diff --stat HEAD~1` and note the file count. Over time, this shows whether changes are getting more focused.

---

## When Metrics Indicate Problems

| Symptom | Likely cause | Fix |
|---|---|---|
| Zero governance violations | Checks too loose | Audit governance rules, enable stricter scanning |
| First-pass rate below 60% | CLAUDE.md conventions unclear | Make rules more specific and actionable |
| Context recovery above 15 min | Stale or missing CONTEXT.md | Add session-end protocol requiring CONTEXT.md update |
| Blast radius above 15 files/PR | Missing scope controls | Add blast radius limits to CLAUDE.md quality standards |
| Agent autonomy below 50% | Governance too strict | Raise confidence ceiling, expand auto-approve scope |
| Agent autonomy above 95% | Governance too loose | Lower confidence ceiling, require review for more action types |

---

## See Also

- [Governance in Action](governance-in-action.md) — Concrete scenarios showing governance value
- [Philosophy](philosophy.md) — Why these metrics matter
- [Tutorial](tutorial.md) — Set up governance and start measuring
