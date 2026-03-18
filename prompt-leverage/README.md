# Prompt Leverage

Upgrade raw prompts into execution-ready instructions for Claude Code.

## Design Philosophy

**Lens, not procedure.** The skill provides a framework of what matters — Claude decides how to apply it. No prescribed workflows, no forced output formats, no intensity level selection.

**Proportionality-first.** The default is minimal intervention. A one-liner stays concise. A complex architecture task gets full structure. The skill's opening lines enforce this before any framework blocks are considered.

**Selective blocks.** 7 framework blocks exist but most tasks need 2-3. The "When to include" column tells Claude which to skip — previous version applied all 7 by default.

## What was cut (and why)

| Cut | Reason |
|-----|--------|
| 5-step workflow | Claude doesn't need "read prompt, classify, apply, return" spelled out |
| Intensity levels | "Light=simple, Deep=complex" is tautological |
| 4 output modes | Claude picks format from context naturally |
| Python classifier script | Keyword regex — Claude classifies better natively |
| Transformation rules | 7 bullets that all say "preserve intent" |
| Quality bar checklist | Restates proportionality constraint |
| Security policy | Built-in Claude behavior |

## What survived (and why)

| Keep | Reason |
|------|--------|
| Framework blocks table | Structural backbone — defines the upgrade format |
| Task adjustments | Concrete per-domain hints Claude can't infer |
| Proportionality constraint | Counteracts Claude's natural verbosity bias |
| Tool name references | Claude Code differentiator from generic prompting |
