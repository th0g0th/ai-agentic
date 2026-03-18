---
name: prompt-leverage
description: Strengthen raw prompts into execution-ready instruction sets for Claude Code. Use when the user wants to improve a prompt, upgrade task instructions, build reusable prompt templates, add clearer tool/verification rules, or wrap a request with better structure before execution. Triggers on "improve this prompt", "make this prompt better", "upgrade prompt", "prompt engineering", "strengthen instructions", "rewrite as agent prompt".
---

# Prompt Leverage

Upgrade raw prompts into structured, execution-ready instructions. Preserve intent, add missing structure, keep proportional to task complexity.

Do not inflate a one-line request into a giant spec. Add blocks only when they materially improve execution. If the prompt is already strong, say so.

## Framework Blocks

Apply selectively based on complexity. Simple tasks need 2-3 blocks; complex tasks may use all 7.

| Block | When to include |
|-------|-----------------|
| Objective | Always — state task + observable success criteria |
| Context | When files, constraints, or unknowns affect correctness |
| Work Style | Non-trivial tasks — set depth, breadth, first-principles |
| Tool Rules | When specific tools or prerequisite checks matter |
| Output Contract | When format, structure, or depth must be explicit |
| Verification | Non-trivial tasks — correctness, edge cases, alternatives |
| Done Criteria | Always — observable stopping condition |

## Task Adjustments

- **Coding**: read before edit, smallest correct change, compile/test after, edge cases
- **Research**: multiple sources, cite findings, disclose uncertainty
- **Writing**: match audience/tone, keep proportional to scope
- **Review**: full context first, severity levels, actionable suggestions
- **Debugging**: reproduce → diagnose → fix → validate no regressions
- **Planning**: research first, phased approach, success criteria per phase

## Rules

- Preserve user's language — upgraded prompt should feel native
- For Claude Code: reference tools by name (Read, Edit, Grep, Glob, Bash, Agent, LSP) and subagent types (researcher, tester, code-reviewer) where they improve execution
- Return the upgraded prompt plus a brief list of what changed and why
