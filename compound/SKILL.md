---
name: compound
description: "Extract learnings from completed work into searchable solution docs. Run after features, fixes, or reviews to compound knowledge. Use when session ends, after PR merge, post-incident, or when a reusable pattern emerges."
argument-hint: "[topic or recent work context]"
metadata:
  author: th0g0th
  version: "1.0.0"
---

# Compound - Knowledge Extraction

Extract reusable learnings from completed work cycles into searchable solution documents. Each unit of work should make subsequent work easier.

**Principles:** YAGNI, KISS, DRY | Token efficiency | Concise outputs

## Usage

```
/compound                          # Auto-detect from recent git changes
/compound "auth middleware rewrite" # Specific topic
/compound path/to/plan.md          # Extract from completed plan
```

## Process

### Step 1: Context Gathering

Analyze recent work to understand what happened:

**If argument is a plan path:**
- Read the plan and all phase files
- Check git log for commits related to the plan

**If argument is a topic:**
- Search git log for related recent commits
- Read changed files and their diffs

**If no argument:**
- Run `git log --oneline -20` to see recent work
- Run `git diff HEAD~5..HEAD --stat` for change summary
- Identify the most significant recent work

### Step 2: Knowledge Extraction

Ask these three critical questions (answer them yourself by analyzing the code):

1. **"What was the hardest decision made here?"** — Identify judgment calls, tricky parts, non-obvious choices
2. **"What alternatives were rejected, and why?"** — Show considered options and reasoning (check git history, plan docs, comments)
3. **"What are we least confident about?"** — Flag areas that might fail, need revisiting, or lack test coverage

### Step 3: Pattern Classification

Categorize each learning into one of:

| Category | Description |
|----------|-------------|
| `bug-fix` | Root cause analysis, fix pattern, prevention strategy |
| `performance` | Bottleneck found, optimization applied, metrics |
| `security` | Vulnerability class, mitigation, detection method |
| `architecture` | Design decision, trade-offs, when to apply |
| `integration` | API quirk, library gotcha, compatibility note |
| `workflow` | Process improvement, tooling insight, automation |
| `data` | Schema decision, migration lesson, query pattern |
| `testing` | Test strategy, coverage gap found, flaky test fix |
| `devops` | Deploy issue, CI/CD lesson, infra insight |

### Step 4: Write Solution Document

Save to `<project-dir>/docs/solutions/` directory. One file per distinct learning.

**Naming:** `{category}-{short-descriptive-slug}.md`

**Format:**

```markdown
---
category: <category>
tags: [tag1, tag2, tag3]
severity: p1|p2|p3
discovered: YYYY-MM-DD
context: "<one-line what triggered this>"
---

# <Descriptive Title>

## Problem
What went wrong or what challenge was faced. 1-3 sentences.

## Root Cause
Why it happened. Be specific.

## Solution
What was done. Include code snippets if relevant (keep minimal).

## Prevention
How to avoid this in the future. Concrete, actionable.

## Related
- Links to related files, commits, or other solution docs
```

### Step 5: Update Project Knowledge

Check if learnings warrant updates to:
- `CLAUDE.md` (project-level) — Add new patterns/preferences discovered
- Code standards docs — If a coding convention was established
- Existing solution docs — If this extends/supersedes a prior learning

Only update if the learning is **reusable across future work**, not one-off.

### Step 6: Verify Compounding

Confirm the system would catch this issue next time:
- Is there a test that prevents recurrence?
- Is there a lint rule or type check?
- Is the pattern documented where an agent would find it?
- If none: flag as "not yet compounded" in the solution doc

## Output Summary

After writing solution docs, print a concise summary:

```
Compounded [N] learnings from [context]:
- [category] [filename]: [one-line description]
- [category] [filename]: [one-line description]
Docs updated: [list or "none"]
Not yet compounded: [list of items without automated prevention]
```

## When to Run

- After completing a feature or fix
- After a bug fix or incident resolution
- After a code review reveals patterns
- After merging a significant PR
- End of a work session with notable learnings
- When you notice a reusable pattern emerging

## Critical Constraints

- DO NOT fabricate learnings — only extract from actual work done
- Keep solution docs under 50 lines — concise > comprehensive
- One learning per file — makes search and linking easy
- Skip trivial/obvious patterns — only compound what's genuinely useful
- If no meaningful learnings found, say so and skip
