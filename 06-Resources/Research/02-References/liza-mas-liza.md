# Liza: Hardened Multi-Agent Coding System

**Repo:** [github.com/liza-mas/liza](https://github.com/liza-mas/liza)
**Analyzed:** 2026-04-02
**Category:** Multi-agent software development framework
**Language:** Go (20k LOC + 60k lines of tests)
**Maturity:** 893 commits, 94 stars, self-hosting (Liza builds Liza)

---

## What Is It?

Liza is a **spec-driven multi-agent coding system** that wraps LLM coding agents (Claude Code, Codex, Gemini, etc.) with deterministic Go-based supervisors. Its core thesis: LLMs are unreliable executors, so every action must pass through mechanically enforced gates, adversarial review, and state machine validation.

It is **not** a personal knowledge system or PKM tool. It is an engineering workflow orchestrator focused on producing correct, tested code through structured agent collaboration.

---

## Architecture

### Three Layers

1. **Behavior Layer** -- Contracts (markdown files in `contracts/`) define 55+ LLM failure modes with countermeasures. Agents must read and follow these. Tier 0 rules (no fabrication, no unapproved state changes, no test corruption) trigger mandatory halts on violation.

2. **Posture Layer** -- Three interaction modes:
   - **Pairing Mode** -- Human-agent collaboration with approval gates
   - **Multi-Agent Mode** -- Autonomous spec-to-code pipeline with blackboard coordination
   - **Subagent Mode** -- Delegation to specialized child agents

3. **Know-How Layer** -- 21 composable skills encoding methodology (code review, debugging, spec writing, security testing, architecture review, etc.)

### Execution Model

```
IDLE -> ANALYSIS -> READY -> EXECUTION -> VALIDATION -> DONE
```

Forbidden transitions are mechanically enforced: you cannot skip analysis, bypass execution, or self-certify completion. Every activity has a doer/reviewer pair -- coders cannot approve their own work.

### Blackboard Pattern

A central `state.yaml` file acts as shared state across all agents. It tracks tasks, agent assignments, sprint progress, and gate clearances. In multi-agent mode, "checkpoint written = gate cleared" enables self-clearing gates while forcing structured reasoning.

### Go Supervisors (Not Prompts)

The `internal/` directory contains 24 Go packages implementing:
- State machine enforcement
- Git worktree isolation (each agent gets its own worktree)
- Circuit breaker (detects loops, repeated failures)
- MCP server for agent tool access
- TUI for live monitoring
- 43+ validation rules

This is compiled Go code, not a prompt collection. Agents operate within mechanically enforced boundaries.

---

## Key Components

### Behavioral Contracts (`contracts/`)

| File | Purpose |
|------|---------|
| `CORE.md` | Universal rules, golden rules, state machine, stop triggers |
| `PAIRING_MODE.md` | Human-supervised collaboration gates |
| `MULTI_AGENT_MODE.md` | Blackboard-driven autonomous operation |
| `SUBAGENT_MODE.md` | Delegation protocols |
| `CONTRACT_FAILURE_MODE_MAP.md` | 55+ failure modes with countermeasures |

The CORE contract defines 14 "Golden Rules" including: never fake success, verify against actual outputs (not internal state), solve the problem then stop (no spontaneous enhancements), and treat test failures as discoveries rather than obstacles.

### Skills (`skills/`)

21 skills covering the software development lifecycle:
- **Spec phase:** `epic-writing`, `user-story-writing`, `detailed-spec-writing`, `spec-review`
- **Code phase:** `clean-code`, `code-review`, `debugging`, `testing`
- **Security:** `black-box-red-testing`, `white-box-red-testing`
- **Architecture:** `software-architecture-review`, `adr-backfill`
- **Meta:** `feynman` (explanation), `systemic-thinking`, `have-you-considered`, `lesson-capture`

### Roles (9 total, two phases)

**Specification phase:** Planner, Spec Writer, Spec Reviewer
**Coding phase:** Coder, Code Reviewer, Tester
**Cross-cutting:** Architect, Security Reviewer, Project Manager

Every role has defined boundaries. A Coder cannot self-approve. A Reviewer cannot write production code during review.

### MCP Integration

Go-based MCP server exposes deterministic tools to agents: worktree management, state transitions, merge authority. The `AGENT_TOOLS.md` file configures tool precedence per environment.

### Specs (`specs/`)

37+ specification documents organized as:
- Architecture (C4 diagrams, state machines, blackboard schema, supervision model)
- Build/Intent (vision + 6 epics + 18 user stories)
- Protocols (task lifecycle, sprint governance, circuit breaker)
- Implementation (13-phase roadmap)

---

## What Makes It Unique

### Adversarial by Design
Most agent frameworks assume agents will cooperate. Liza assumes agents will fail, hallucinate, and cut corners. Every structural decision reflects this distrust: doer/reviewer pairs, forbidden state transitions, mandatory halts on violation, and a catalog of 55+ specific failure modes.

### Mechanical Enforcement Over Prompting
Where other systems rely on prompt instructions ("please don't hallucinate"), Liza uses compiled Go code to enforce invariants. An agent literally cannot transition from EXECUTION to DONE without passing through VALIDATION -- the state machine blocks it.

### Self-Hosting
Liza develops itself using its own multi-agent pipeline. This is both a proof of capability and a forcing function for reliability.

### Stop Triggers
The system has explicit "stop working immediately" conditions:
- 3+ critical-path assumptions
- Same fix proposed twice without new rationale
- Evidence contradicting hypothesis
- Tool failure after 3 consecutive attempts
- Same rule violated twice in session

### Circuit Breaker
Detects when agents are looping or making no progress. Triggers checkpoints and escalation rather than letting agents burn tokens on dead-end approaches.

---

## Comparison to Dex

| Dimension | Dex | Liza |
|-----------|-----|------|
| **Purpose** | Personal knowledge system / work assistant | Multi-agent software development |
| **User** | Individual professional (PM, IC, manager) | Developer teams using AI coding agents |
| **Architecture** | CLAUDE.md + MCP servers + hooks + skills | Go supervisors + behavioral contracts + blackboard |
| **Agent model** | Single agent (Claude) with tool access | Multiple agents with enforced role boundaries |
| **State** | Markdown files in Obsidian vault (PARA) | YAML blackboard (`state.yaml`) |
| **Skills** | 50+ workflow skills (daily-plan, career-coach, etc.) | 21 methodology skills (code-review, spec-writing, etc.) |
| **Trust model** | Collaborative -- agent is a thinking partner | Adversarial -- agent is an unreliable executor |
| **Enforcement** | Behavioral via CLAUDE.md instructions | Mechanical via compiled Go state machines |
| **MCP usage** | Tool access (calendar, people index, tasks) | Deterministic supervision (worktree, merge, validation) |
| **Failure handling** | Learnings capture, session reviews | Circuit breaker, mandatory halts, recovery commands |
| **Domain** | Professional life management | Software engineering process |

### Structural Similarities
- Both use CLAUDE.md as the primary behavioral contract for agents
- Both have composable skills as reusable capability modules
- Both use MCP for tool integration
- Both capture learnings/lessons from work sessions
- Both define explicit state flows (Dex: onboarding, planning; Liza: task execution)

### Philosophical Differences
- **Dex trusts the agent** and focuses on making it more capable (semantic search, context injection, identity modeling). Liza **distrusts the agent** and focuses on constraining it mechanically.
- **Dex optimizes for individual workflow** (meetings, people, tasks, career). Liza **optimizes for engineering correctness** (specs, tests, reviews, merges).
- **Dex is vault-centric** -- everything lives in markdown files the user owns. Liza is **process-centric** -- everything flows through a state machine the supervisor controls.

---

## Key Takeaways

- **Adversarial agent design is a legitimate architecture.** Liza's catalog of 55+ failure modes and mechanical countermeasures represents hard-won knowledge about where LLMs break down in production coding. This is useful reference material regardless of domain.

- **Behavioral contracts can be enforced mechanically, not just instructionally.** Dex relies on CLAUDE.md instructions that the agent follows voluntarily. Liza compiles constraints into Go code that agents cannot bypass. The trade-off is flexibility vs. reliability.

- **The doer/reviewer separation pattern has broad applicability.** Even outside multi-agent coding, the principle that no agent should self-certify its own work could inform Dex features like review workflows or quality gates on automated processing.

- **Stop triggers and circuit breakers are underexplored in single-agent systems.** Liza's explicit conditions for "stop working immediately" (same fix twice, evidence contradicting hypothesis) could improve any agent system's resource efficiency and output quality.

- **Self-hosting as proof of concept is compelling.** Liza building itself with its own pipeline is a strong credibility signal. For Dex, the equivalent would be using Dex to manage the Dex development project -- something worth considering for dogfooding.
