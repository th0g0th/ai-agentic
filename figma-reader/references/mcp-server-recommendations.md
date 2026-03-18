# MCP Server Recommendations

The MCP server determines how much raw data reaches the agent. Choose based on setup and access.

## Server Comparison

| Server | Token Efficiency | Setup | Notes |
|--------|-----------------|-------|-------|
| **Official Figma MCP** (Dev Mode) | Best — first-party optimized | Built-in (Figma desktop) | Actively maintained, structured tools, component→code mapping |
| **Figma-Context-MCP** (GLips) | Good — community filter | Self-hosted | Alternative for users without Dev Mode |
| **Raw Figma API via MCP** | Poor — full JSON | Any generic MCP | All optimization burden on agent layer |

## Official Figma MCP Server (Recommended)

Docs: https://developers.figma.com/docs/figma-mcp-server/

- First-party, bundled with Figma desktop — actively maintained by Figma
- Already optimizes responses for LLM consumption (structured tools)
- Variables/styles extraction, component→code mapping built-in
- No extra infrastructure — enable in Figma desktop → Preferences → MCP Server
- Evolves with Figma's API; third-party servers may lag behind

Setup: enable in Figma desktop → Preferences → MCP Server.

## Figma-Context-MCP (Alternative)

GitHub: https://github.com/GLips/Figma-Context-MCP

- Community server — use if you don't have Dev Mode access
- Simplifies and translates API responses before they reach the LLM
- Requires self-hosting and separate API key management
- May not stay maintained as official solution matures

Setup — add to Claude Code MCP config:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-context-mcp"],
      "env": {
        "FIGMA_API_KEY": "<your-figma-api-key>"
      }
    }
  }
}
```

## Three-Layer Optimization Stack

```
Layer 1: API          → depth + nodeId params         (70-90% reduction)
Layer 2: MCP Server   → response simplification       (50-70% reduction)
Layer 3: Agent        → guided discovery + output      (60-80% reduction)
───────────────────────────────────────────────────────────────────────
Combined: ~960k tokens → 2k-10k per component (~95-99% reduction)
```

Layer 1 and 3 are controlled by this skill. Layer 2 depends on MCP server choice.
