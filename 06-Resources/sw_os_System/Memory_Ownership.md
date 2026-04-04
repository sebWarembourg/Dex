# Memory Ownership Boundaries

## Claude Auto-Memory (native)
**Owns:** Preferences, style, communication patterns, formatting choices
**Examples:** "User prefers bullet points", "Use neutral mermaid theme", "Direct communication style"
**How it works:** Automatically captured by Claude. Persists across all sessions and harnesses.
**sw_os action:** Don't duplicate. Don't capture preferences in learning-heartbeat.

## Agent Memory (frontmatter, `memory: project`)
**Owns:** Per-agent operational state across sessions
**Examples:** "deal-attention flagged Acme Corp 3 times", "cracks-detector: pricing follow-up resolved"
**How it works:** Each agent reads/writes its own memory. Scoped to that agent.
**sw_os action:** Configured in Phase 1, WP-1.1.

## sw_os Session Memory (learning-heartbeat)
**Owns:** Operational decisions, commitments, work patterns, system learnings
**Examples:** "Agreed to deliver DACH deck by Friday", "Meeting-prep skill needs more account context"
**How it works:** Captured at session Stop, stored in System/Session_Learnings/
**sw_os action:** Filter for operational only (WP-2.1).

## sw_os Vault Search (QMD)
**Owns:** Semantic search across all vault content
**sw_os action:** Unchanged.

## sw_os Proactive Intelligence (Phase 4 — planned)
**Owns:** Anticipation, pre-fetching, pattern prediction across agents
**sw_os action:** Future. Enhanced by agent memory providing richer signal.
