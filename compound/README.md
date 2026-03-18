# Compound

Extract reusable learnings from completed work into searchable solution docs. Inspired by [Compound Engineering](https://every.to/guides/compound-engineering).

## Design Philosophy

**Each work cycle should teach the system.** Traditional workflows end at commit. Compound adds a knowledge extraction step so the same class of problem is never debugged from scratch twice.

**Structured for machines, not humans.** Solution docs use YAML frontmatter (category, tags, severity, date) so agents can search and filter them during planning and debugging. Journals are for humans; compound is for the system.

**Three critical questions drive extraction:**
1. What was the hardest decision made here?
2. What alternatives were rejected, and why?
3. What are we least confident about?

## How it fits

```
Plan → Implement → Test → Review → Commit → Compound → Journal
                                                ↓
                                     docs/solutions/
                                     ├── bug-fix-*.md
                                     ├── performance-*.md
                                     └── architecture-*.md
```

Runs standalone via `/compound`, or integrate into your existing workflow's finalize step.

## Output format

Each solution doc follows:

```yaml
---
category: bug-fix|performance|security|architecture|integration|workflow|data|testing|devops
tags: [specific, searchable, terms]
severity: p1|p2|p3
discovered: YYYY-MM-DD
context: "one-line trigger"
---
```

Followed by: Problem, Root Cause, Solution, Prevention, Related sections. Kept under 50 lines.

## What it doesn't do

- Fabricate learnings from nothing
- Store trivial/obvious patterns
- Replace journals (different purpose, different audience)
- Implement fixes (extraction only)
