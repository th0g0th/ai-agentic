# Token Optimization Patterns for Figma MCP

## Core Principle

**LLM decides WHAT to fetch. MCP handles HOW to fetch it.**

Every Figma MCP call should be targeted and minimal. The AI should never request more data than needed for the user's current question.

## Pattern 1: Targeted Node Fetching

Always prefer nodeId over full file reads.

| Approach | Tokens | When to use |
|----------|--------|-------------|
| `get_figma_data(fileKey)` | 2000-10000+ | Never (unless explicitly asked) |
| `get_figma_data(fileKey, depth=1)` | 500-1500 | Page overview, top-level structure |
| `get_figma_data(fileKey, depth=2)` | 800-3000 | Section overview with children |
| `get_figma_data(fileKey, nodeId)` | 100-500 | Specific component (preferred) |

## Pattern 2: Progressive Disclosure

Fetch in layers — broad first, then drill down on request:

1. **Layer 1**: `depth=1` — get page names and top-level frames
2. **Layer 2**: `nodeId=<frame>` — get specific frame details
3. **Layer 3**: `nodeId=<component>` — get exact component specs

Never jump to Layer 3 without user confirming which component.

## Pattern 3: Structured Output Format

Present results as structured data, not prose.

Bad (500+ tokens):
> The login card is a frame component that measures 400 pixels wide by 320 pixels tall. It uses auto layout with vertical direction and has padding of 24 pixels on all sides. The background color is white (#FFFFFF) with a border radius of 12 pixels...

Good (80 tokens):
```
LoginCard (45:22) | Frame, Auto Layout vertical
400x320 | Padding: 24px | Gap: 16px | BG: #FFF | Radius: 12px
Children: 2x InputField, Button, Link
```

## Pattern 4: Batch Image Downloads

Download multiple images in one call instead of separate calls per image.

Bad: 4 calls x ~50 tokens each = 200+ tokens overhead
```
download_figma_images(fileKey, nodes=[{nodeId: "1:1", ...}])
download_figma_images(fileKey, nodes=[{nodeId: "1:2", ...}])
download_figma_images(fileKey, nodes=[{nodeId: "1:3", ...}])
download_figma_images(fileKey, nodes=[{nodeId: "1:4", ...}])
```

Good: 1 call = ~60 tokens overhead
```
download_figma_images(fileKey, nodes=[
  {nodeId: "1:1", fileName: "icon-home.svg"},
  {nodeId: "1:2", fileName: "icon-settings.svg"},
  {nodeId: "1:3", fileName: "icon-profile.svg"},
  {nodeId: "1:4", fileName: "icon-search.svg"}
], localPath="/project/assets/icons")
```

## Pattern 5: Ask Before Fetching

Before any MCP call, confirm scope with user. A 10-token question saves 500+ tokens of unwanted data.

Questions that save tokens:
- "Which page/frame should I focus on?"
- "Do you need all components or just [specific one]?"
- "Want colors only, or full design tokens (colors + typography + spacing)?"

## Pattern 6: Avoid Re-fetching

Within a conversation, remember previously fetched node data. If user asks about a component already fetched, use the cached data instead of re-calling MCP.

## Pattern 7: Pruned Tree Strategy

Keep responses <500KB by combining depth + nodeId:
- Fetch pruned node tree with `depth=2-3` instead of full tree
- Extract tokens and do analysis locally — no extra API calls
- Fetch images only for final deduplicated list in small batches with exponential backoff
- Result: reduces from 10+ calls and multi-MB traffic to 2-3 calls and <500KB
- Eliminates 429 rate limit errors on complex frames

## Pattern 8: Cross-Session Token Caching

When design tokens (colors, typography, spacing) are extracted:
- Offer to save them to a local file (e.g., `design-tokens.json` or `figma-tokens.md`)
- In future sessions, check for cached tokens before re-fetching from Figma
- Only re-fetch if user says the design has changed

## Summary: Cost Comparison

| Scenario | Unoptimized | Optimized | Savings |
|----------|-------------|-----------|---------|
| Inspect one component | 2000-5000 | 200-500 | ~80% |
| Extract design tokens | 3000-8000 | 400-1000 | ~75% |
| Page overview | 5000-15000 | 800-2000 | ~70% |
| Download 4 icons | 400+ | 100 | ~75% |
| Full design audit | 10000+ | 2000-4000 | ~65% |

## The Token Gap Problem

Raw Figma API returns ~960k tokens for a component that Figma's UI estimates at ~67k (15x gap). See [mcp-server-recommendations.md](mcp-server-recommendations.md) for the three-layer optimization stack that closes this gap to ~95-99% reduction.
