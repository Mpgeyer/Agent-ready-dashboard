# Task: Build Agent-Ready Dashboard v5

Build a single-file `index.html` — a clean, professional dashboard scoring software platforms on AI agent compatibility.

## Requirements

### Design
- NVIDIA dark theme: background #0a0a0a, text #e0e0e0, accent #76B900
- Single-file HTML with all CSS inline (no external deps)
- Responsive (works on desktop + mobile)
- Professional — this will be shared with NVIDIA employees

### Section 1: Criteria Descriptions (TOP of page)
Brief descriptions of each of the 10 scoring criteria in a clean grid/table at the top, BEFORE the heat map:

| # | Criterion | What It Measures |
|---|-----------|-----------------|
| 1 | API Spec | Machine-readable API definition (OpenAPI, GraphQL, gRPC) |
| 2 | Protocol / MCP | Model Context Protocol or equivalent agent-discovery support |
| 3 | Auth | Agent-suitable auth (API keys, OAuth scopes, service accounts) |
| 4 | Idempotency | Deterministic operations, safe retries, idempotency keys |
| 5 | Errors | Structured error codes, actionable messages, Retry-After |
| 6 | Events | Webhooks, WebSockets, SSE for real-time state changes |
| 7 | Sandbox | Isolated test environments with full API parity |
| 8 | Design | API consistency, REST conventions, pagination patterns |
| 9 | Rate Limits | Transparent limits, X-RateLimit headers, per-endpoint tiers |
| 10 | Docs | Agent-dev docs, code examples, LLM training data presence |

### Section 2: Heat Map Table
Color-coded scoring table. Scores 1-10, color intensity = readiness.
Sticky headers. Sorted by total descending. Omniverse is LAST row (highlighted).

CSS color scale:
- .s9,.s10 → dark green (#1b5e20 bg, #4CAF50 text)
- .s8 → medium green (#2e7d32 bg, #66BB6A text)
- .s7 → yellow-green (#33691e bg, #8BC34A text)
- .s6 → yellow (#4a4a00 bg, #CDDC39 text)
- .s5 → orange-brown (#5d4037 bg, #FFAB91 text)
- .s4 → darker orange (#4e342e bg, #FF8A65 text)
- .s3 → light red (#b71c1c33 bg, #EF5350 text)
- .s2 → red (#c6282833 bg, #F44336 text)

Platform data (DO NOT include Scan2Twin):

| Platform | Spec | MCP | Auth | Idemp | Err | Events | Sand | Design | Rate | Docs | TOTAL |
|----------|------|-----|------|-------|-----|--------|------|--------|------|------|-------|
| GitHub | 10 | 10 | 10 | 8 | 8 | 10 | 9 | 9 | 10 | 10 | 94 |
| Stripe | 10 | 4 | 10 | 10 | 10 | 9 | 10 | 10 | 10 | 10 | 93 |
| Linear | 9 | 9 | 8 | 8 | 8 | 9 | 7 | 10 | 8 | 8 | 84 |
| Slack | 8 | 9 | 8 | 7 | 8 | 10 | 7 | 8 | 8 | 9 | 82 |
| Supabase | 9 | 6 | 8 | 7 | 7 | 9 | 9 | 9 | 7 | 9 | 80 |
| Google Drive | 8 | 8 | 9 | 6 | 7 | 7 | 7 | 6 | 7 | 8 | 73 |
| Sentry | 8 | 7 | 7 | 6 | 8 | 8 | 7 | 7 | 7 | 8 | 73 |
| PostgreSQL/MCP | 7 | 10 | 6 | 9 | 6 | 3 | 9 | 8 | 3 | 7 | 68 |
| Notion | 8 | 9 | 6 | 6 | 6 | 6 | 6 | 7 | 4 | 8 | 66 |
| Puppeteer | 6 | 9 | 4 | 4 | 6 | 3 | 8 | 7 | 4 | 7 | 58 |
| NVIDIA Omniverse | 6 | 2 | 6 | 8 | 5 | 7 | 7 | 6 | 4 | 7 | 58 |

Omniverse row should be highlighted (class="hl") with a green left border.

### Section 3: Platform Deep Dives (2-column grid)
Only include NVIDIA Omniverse deep dive card:
- Score: 58/100
- Strengths: USD deterministic/idempotent (8/10), Kit Python API, Nucleus sandboxed asset mgmt (7/10), Event system via Action Graph + Omnigraph (7/10), Emerging MCP support
- Gaps: MCP nascent (2/10), No OpenAPI/REST spec (6/10), Rate limiting nonexistent (4/10), Error responses lack structured codes (5/10)

### Section 4: Key Insight
"MCP = SEO for AI Agents" — platforms without MCP are invisible to agents. Early adopters capture agent mindshare like early SEO captured search traffic.

### Section 5: Omniverse Roadmap
Path from 58 → 80+:
- Phase 1 (Quick wins, +10): OpenAPI spec, MCP server, structured error codes
- Phase 2 (Foundation, +8): Agent auth scoping, rate limiting, WebSocket events
- Phase 3 (Ecosystem, +6): Agent cookbook, training data presence, CLI tooling

### Section 6: Footer
"Agent-Ready Software Platforms v5 — NVIDIA | April 2026"
Methodology note. Copyright.

### DO NOT INCLUDE
- Scan2Twin (removed entirely)
- BCG critique section
- Market context section (keep it focused)
- Executive summary (the criteria descriptions replace it)

### Title
"🔬 Agent-Ready Software Platforms"
Subtitle: "10-Factor Agent Compatibility Assessment v5 — NVIDIA | April 2026"

Write the complete `index.html` file. Make it polished and professional.
