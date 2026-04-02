# Everything Claude Code

**Source:** [github.com/affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
**Reviewed:** 2026-04-02
**Stars:** 133k+ | **Forks:** 19.2k+ | **Contributors:** 30+

---

## What It Is

A production-ready performance optimization system for AI coding agents. Despite the name suggesting a curated list, it is actually a **comprehensive plugin framework** — a full collection of agents, skills, hooks, rules, and configurations designed to supercharge Claude Code (and compatible with Cursor, OpenCode, Codex, and other AI harnesses).

Won an Anthropic hackathon award. Evolved through intensive daily use building real products.

---

## Architecture Overview

The system is organized into 9 interconnected layers:

| Layer | Count | Purpose |
|-------|-------|---------|
| **Agents** | 36 | Specialized subagents for planning, review, testing, etc. |
| **Skills** | 151+ | Workflow definitions across domains |
| **Commands** | 68 | Slash-command entry points (legacy shims) |
| **Rules** | ~12 languages | Hierarchical coding standards (common + language-specific) |
| **Hooks** | ~15 | Event-driven automations (pre/post tool use, lifecycle) |
| **Scripts** | Various | Cross-platform Node.js utilities |
| **Contexts** | 3 | Dynamic system prompts (dev, research, review modes) |
| **Examples** | 8+ | Real-world CLAUDE.md configs for different stacks |
| **MCP Configs** | 4+ | GitHub, Supabase, Vercel, Railway integrations |

---

## Agent Categories

36 specialized subagents organized by function:

- **Code Review** (12 agents) — Language-specific reviewers for TypeScript, Python, Go, Java, Kotlin, Rust, C++, Flutter, plus database, healthcare, and security reviewers
- **Build/Error Resolution** (7 agents) — Language-specific build error resolvers including PyTorch
- **Planning & Architecture** (3) — Architect, Planner, Chief of Staff
- **Optimization** (3) — Performance Optimizer, Harness Optimizer, Refactor Cleaner
- **Documentation** (2) — Doc Updater, Docs Lookup
- **Testing** (2) — TDD Guide, E2E Runner
- **Open Source** (3) — Forker, Packager, Sanitizer (for open-sourcing internal code)
- **ML/AI** (3) — GAN Planner, Generator, Evaluator
- **Utility** (1) — Loop Operator

---

## Skill Domains (151+ Skills)

Skills cover an unusually broad range:

### Software Engineering
- Language-specific patterns: Python, Go, Rust, Kotlin, Java, C++, Perl, PHP
- Framework guidance: Django, Laravel, Spring Boot, Next.js, Nuxt 4, Flutter, SwiftUI
- Architecture: hexagonal, clean architecture, microservices
- Testing: TDD workflows, E2E, regression testing
- Database: Postgres patterns, ClickHouse, migrations
- Deployment: Docker, CI/CD pipelines

### AI & Agents
- Agentic engineering patterns
- Agent evaluation and autonomous harnesses
- Continuous agent loops
- SWE agent patterns
- Enterprise agent operations

### Specialized Verticals
- **Healthcare** — EMR/CDSS patterns, PHI compliance
- **Supply Chain** — Logistics, inventory, carrier management
- **Finance** — Billing, payments, customs compliance
- **Security** — Scanning, review procedures

### Content & Operations
- Article writing, market research, investor materials
- Video creation (Remotion, Manim, VideoDb)
- Brand voice and design systems
- Slide decks

---

## Rules System

Two-tier hierarchical approach:

1. **Common rules** (language-agnostic) — coding style, git workflow, testing, performance, design patterns, security
2. **Language extensions** (12 languages) — TypeScript, Python, Go, Swift, PHP, Rust, C++, C#, Java, Kotlin, Perl

Language-specific rules take precedence over common rules when conflicts arise (CSS-specificity model). Each language directory mirrors the common structure (coding-style.md, testing.md, patterns.md, hooks.md, security.md) with cross-references back.

**Key distinction:** Rules define standards ("80% test coverage"); skills provide implementation details for specific tasks.

---

## Hooks System

Event-driven automations at three lifecycle points:

### PreToolUse (before Claude acts)
- Dev server blocker (prevents `npm run dev` outside tmux)
- Git push review reminder
- Pre-commit quality checks
- Strategic compaction suggestions (~every 50 tool calls)
- Security monitoring (opt-in)

### PostToolUse (after Claude acts)
- PR URL logging
- Build output analysis
- Quality gates after file edits
- Auto-formatting (Prettier)
- TypeScript type checking
- Console.log warnings

### Lifecycle
- Session start/end markers
- Context compaction state saving
- Pattern extraction for continuous learning
- Cost tracking and desktop notifications

Hooks are configurable via environment variables (`ECC_HOOK_PROFILE`, `ECC_DISABLED_HOOKS`).

---

## Example Configurations

Real-world CLAUDE.md templates for different stacks:

- **SaaS** — Next.js + Supabase + Stripe
- **Go Microservices**
- **Django REST APIs**
- **Laravel APIs**
- **Rust APIs**
- **GAN/ML Harness**

Each provides a working starting point for that tech stack's conventions, testing requirements, and deployment patterns.

---

## Contexts (Mode Switching)

Three dynamic system prompt modes:

- **dev.md** — Development mode (building features)
- **research.md** — Research/exploration mode (investigating, prototyping)
- **review.md** — Code review mode (quality-focused analysis)

These switch Claude's behavior profile depending on the current task type.

---

## Guides & Documentation

Three core guides (paths returned 404 on direct fetch, but referenced in README):

1. **Shorthand Guide** — Setup and foundational philosophy
2. **Longform Guide** — Token optimization, memory persistence, evals, parallelization
3. **Security Guide** — Attack vectors, AgentShield, CVE coverage

Supporting docs: CONTRIBUTING.md, SECURITY.md, TROUBLESHOOTING.md, plus translations in 7 languages (Chinese, Portuguese, Japanese, Korean, Turkish, and more).

---

## Notable Patterns & Best Practices

### Token Optimization
The system emphasizes token efficiency as a first-class concern — compaction hooks suggest context reduction every ~50 tool calls, and the longform guide covers specific optimization techniques.

### Memory Persistence
Hooks manage session lifecycle to preserve context across sessions. Pattern extraction during sessions feeds a continuous learning loop.

### Multi-Agent Orchestration
Commands like `/multi-plan` and `/multi-execute` coordinate multiple specialized agents on complex tasks. The planner-executor pattern separates planning from implementation.

### Security-First Design
Dedicated security reviewer agent, pre-commit quality checks, optional security monitoring hook, and a full security guide covering attack vectors specific to AI coding agents.

### Continuous Learning
The system extracts patterns and "instincts" from usage — skills for continuous learning and instinct extraction suggest the framework improves its own recommendations over time.

---

## Distribution

- Cross-platform installers (Bash, PowerShell)
- npm package
- GitHub App marketplace (150+ installations)
- Plugin marketplace integration

---

## Key Takeaways

- **This is the most comprehensive Claude Code optimization framework publicly available.** At 133k+ stars and 151+ skills, it represents a mature, battle-tested system — not a toy or proof-of-concept. The breadth (36 agents, 12 language rulesets, hooks, contexts) is substantial.

- **The layered rules system is a smart pattern worth adopting.** Common rules provide a universal baseline; language-specific extensions override where needed. This avoids the "one giant prompt" problem and keeps context relevant to the current task.

- **Hooks for session lifecycle management solve a real problem.** Strategic compaction suggestions, pattern extraction, and cost tracking address the context window limitations that plague long coding sessions. The ~50 tool call compaction trigger is a practical heuristic.

- **The agent specialization model shows how to scale AI coding beyond single-agent patterns.** Rather than one agent doing everything, dedicated reviewers (per language), build resolvers, and planning agents each carry focused context. This mirrors how human engineering teams specialize.

- **Vertical domain skills (healthcare, supply chain, finance) signal where AI coding assistants are heading.** Most frameworks stop at generic software engineering. The inclusion of PHI compliance patterns, logistics workflows, and financial operations suggests high-value enterprise use cases that go beyond "write me a function."
