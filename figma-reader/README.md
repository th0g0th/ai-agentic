# Figma Reader

Read Figma files via MCP with token-efficient guided discovery.

## Design Philosophy

**Ask before fetch.** A 10-token question prevents 500+ tokens of unwanted data. The skill guides users through structured discovery before making any MCP call.

**Structured output, not prose.** Compact key-value format reduces output tokens by ~80% compared to natural language descriptions of the same design data.

**Three-layer optimization.** Raw Figma API returns ~960k tokens per component. Through API-level targeting (depth/nodeId), MCP server filtering, and agent-level guided discovery, this reduces to 2k-10k tokens (~95-99% savings).

## Structure

```
figma-reader/
├── SKILL.md                                    # Core instructions (<300 lines)
└── references/
    ├── token-optimization-patterns.md          # 8 patterns with cost comparisons
    └── mcp-server-recommendations.md           # Server comparison + setup guide
```

## Key Features

| Feature | Token Impact |
|---------|-------------|
| Guided discovery (ask before fetch) | Saves 60-80% |
| nodeId + depth targeting | Saves ~80% |
| Structured key-value output | Saves ~80% |
| Batch image downloads | Saves ~75% |
| Cross-session token caching | Variable |

## Installation

Copy the `figma-reader/` folder to `~/.claude/skills/`:

```bash
cp -r figma-reader ~/.claude/skills/
```

Requires a Figma MCP server configured in Claude Code. Recommended: [Official Figma MCP Server](https://developers.figma.com/docs/figma-mcp-server/) (Dev Mode). Alternative: [Figma-Context-MCP](https://github.com/GLips/Figma-Context-MCP) if no Dev Mode access.
