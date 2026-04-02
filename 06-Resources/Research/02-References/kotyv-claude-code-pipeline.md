# KotyV/claude-code-pipeline

**Repository:** [github.com/KotyV/claude-code-pipeline](https://github.com/KotyV/claude-code-pipeline)
**License:** MIT
**Reviewed:** 2026-04-02

---

## What Is It?

A structured development pipeline for Claude Code consisting of **10 slash commands** and **7 reference documents**. It enforces a waterfall-style quality process for shipping production code through AI-assisted development.

The project was created by a non-developer who needed to ship production-grade software. It was battle-tested on a FastAPI + React codebase that reportedly scored an "A" rating in comparative audits against senior engineer output.

**Category:** Development workflow automation / Claude Code skill library

---

## Architecture Overview

### Pipeline Flow

```
/feature-discovery → /specs → /architecture → /implement → /qa-tests → /security-audit → /cleanup-push
```

Each phase is a standalone slash command (markdown file in `.claude/commands/`) that can be invoked independently or orchestrated through the master `/new-feature` command. A parallel `/bug-fix` command provides a TDD-specific workflow.

### File Organization

```
.claude/commands/
├── skills/                    # 10 slash commands
│   ├── new-feature.md         # Master orchestrator
│   ├── feature-discovery.md   # Requirements analysis
│   ├── specs.md               # Story decomposition
│   ├── architecture.md        # Technical design + ADRs
│   ├── implement.md           # Per-story code generation
│   ├── qa-tests.md            # Test pyramid construction
│   ├── security-audit.md      # OWASP-based scanning
│   ├── security-review.md     # PR-focused security check
│   ├── bug-fix.md             # TDD bug resolution
│   └── cleanup-push.md        # Finalization + deployment
└── references/                # 7 shared reference docs
    ├── quality-gates.md       # 11 automated checks
    ├── clean-code-guidelines.md
    ├── security-rules.md
    ├── domain-examples.md
    ├── preflight-checklist.md
    ├── review-checklist.md
    └── recovery-protocol.md
```

### Design Principles

**Single Source of Truth.** Information lives in one place. Skills reference shared documents in `references/` rather than duplicating rules. This keeps individual skills concise (~150 lines each).

**Enforcement, Not Suggestion.** Skills define what Claude *cannot* do, not what it *should* do:
- No step-chaining without user validation
- No commits without passing all quality gates
- No bug fixes before a failing test exists
- No TODO/FIXME markers in any commit

**Checkpoint Architecture.** Every major phase requires Claude to present deliverables and explicitly ask for user approval before proceeding. This prevents the common failure mode of AI agents diverging silently from intent.

---

## How It Orchestrates Claude Code Tasks

### The Master Orchestrator (`/new-feature`)

The `/new-feature` command chains all phases sequentially with mandatory human checkpoints between each. It enforces:

- **One feature per session** to prevent scope creep
- **Atomic commits** (one per user story, never bundled)
- **Mandatory refactor pass** before QA (dead code removal, SRP enforcement, 300-line file limit)
- **No placeholders** — every deliverable must be production-ready

### Phase-by-Phase Orchestration

| Phase | Input | Output | Checkpoint |
|-------|-------|--------|------------|
| Discovery | Raw feature request | Structured requirements doc | "Here is what I understood..." |
| Specs | Requirements | Epics, user stories (Gherkin AC), API contracts, data models | Story count + priority sign-off |
| Architecture | Specs | File inventory, design patterns, ADRs | Blueprint approval |
| Implement | Architecture | Working code with per-story commits | Quality gate pass per story |
| QA | Implementation | Test pyramid (70/20/10 unit/integration/E2E) | Coverage thresholds |
| Security | Code | OWASP assessment (minimum score 80) | Audit report approval |
| Cleanup/Push | Everything | PR with semantic commit, updated docs | Final review |

### Quality Gates (11 Automated Checks)

The pipeline enforces 11 quality gates before any commit is allowed. Ten are blocking; one is advisory.

| Gate | What It Checks | Blocking? |
|------|---------------|-----------|
| 1. Lint + Format | Zero linting errors | Yes |
| 2. Type Check | No new type errors beyond baseline | Yes |
| 3. Tests | Full test suite passes | Yes |
| 4. File Size | Source files under 300 lines | Yes |
| 5. Function Size | Functions under 20 lines logic; classes under 7 public methods | Yes |
| 6. No Placeholders | No TODO, FIXME, NotImplementedError | Yes |
| 7. No Magic Strings | Status strings should be enums | No (warning) |
| 8. Basic Security | No hardcoded credentials or debug statements | Yes |
| 9. Advanced Secrets | Regex scan for API keys and private keys | Yes |
| 10. Sensitive Files | .env files not tracked by git | Yes |
| 11. Dangerous Patterns | No eval(), exec(), SQL injection, unsafe YAML | Yes |

If any gate fails, the fix-and-reverify loop runs. The rule: "Do not fix one gate and break another."

### TDD Bug Fix Workflow (`/bug-fix`)

A strict RED-GREEN-REFACTOR cycle:

1. **Diagnose** — Articulate root cause in one sentence
2. **RED** — Write a failing test that reproduces the bug; commit it
3. **GREEN** — Fix only the code needed to pass the test; commit it
4. **Refactor** — Optional cleanup
5. **Verify** — Full suite + quality gates

Minimum two commits required (test, then fix). The test is never modified to pass — "the test is right, the code is wrong."

### Recovery Protocol

Handles interrupted work through:

- **State detection** via `pipeline-state.json` + git history + workspace status
- **Phase-specific resumption** — different strategies depending on where work stopped
- **Structured retry logic** — 3 attempts per story with escalating approaches (initial, alternative, simplified), then escalation to human
- **Risk mitigation** — git stash before attempting uncertain fixes

---

## Strengths

- **Comprehensive coverage.** The pipeline addresses the full development lifecycle from requirements through deployment. Most Claude Code skill sets focus on implementation alone.
- **Human-in-the-loop by design.** Mandatory checkpoints at every phase prevent the common problem of AI agents running off in the wrong direction.
- **Reference document pattern.** Shared documents avoid duplication and keep skills maintainable. Updating a rule in `quality-gates.md` automatically applies everywhere.
- **Stack-agnostic design.** While defaults target FastAPI + React, every tool reference is marked for substitution (pytest to jest, ruff to eslint, etc.).
- **Recovery handling.** Most AI development pipelines have no concept of resuming interrupted work. The recovery protocol with state files and retry logic is a genuine innovation.

## Limitations

- **Waterfall assumptions.** The strict sequential flow (discovery before specs before architecture before implementation) may feel heavy for small changes or rapid iteration.
- **No incremental adoption.** The system is designed as an all-or-nothing pipeline. There is no clear path to using just the quality gates or just the TDD workflow independently.
- **Manual verification gates.** Gates 5 (function size) and 6 (no placeholders) require manual file-by-file inspection rather than automated tooling.
- **Single-feature scope.** The "one feature per session" constraint could be limiting for experienced developers working on related changes.
- **No CI/CD integration.** The pipeline runs entirely inside Claude Code sessions. There is no GitHub Actions, pre-commit hooks, or external automation layer.

---

## Key Takeaways

- **Constraint-based prompting works.** Telling Claude what it *cannot* do (no commits without tests, no chaining without approval) is more effective than telling it what it *should* do. The "enforcement, not suggestion" pattern is the core insight of this project.

- **Checkpoint architecture prevents AI drift.** Mandatory stop-and-validate gates between phases solve the biggest practical problem with agentic coding: the agent confidently building the wrong thing. Each checkpoint is a cheap course correction.

- **Shared reference documents are the right abstraction.** Instead of embedding rules in every command, factoring them into standalone reference files that skills load at runtime keeps the system maintainable and consistent. This is the "Single Source of Truth" pattern applied to prompt engineering.

- **Recovery and state management matter.** Most Claude Code skill sets are stateless and assume clean sessions. The recovery protocol with `pipeline-state.json`, git-history-based detection, and structured retry logic makes this pipeline usable in real-world conditions where work gets interrupted.

- **A non-developer shipped production code with this.** The pipeline's reported results (97% test coverage, zero broad exceptions, zero hardcoded secrets) suggest that sufficiently structured AI workflows can bridge significant skill gaps — the constraints do the heavy lifting that expertise normally provides.
