# PM Skills Marketplace (phuryn/pm-skills)

**Source:** [github.com/phuryn/pm-skills](https://github.com/phuryn/pm-skills)
**Author:** Pawel Huryn ([Product Compass](https://www.productcompass.pm))
**License:** MIT
**Reviewed:** 2026-04-02

---

## What It Is

PM Skills Marketplace is an open-source collection of **65 AI-powered product management skills and 36 chained workflows**, packaged as 8 plugins for Claude Code's skill/plugin system. It encodes proven PM frameworks from practitioners like Teresa Torres (*Continuous Discovery Habits*), Marty Cagan, Dan Olsen (*The Lean Product Playbook*), and Alberto Savoia into structured, step-by-step AI workflows.

The core value proposition: instead of getting generic AI output when you ask about product decisions, you get guided walkthroughs of established methodologies with proper structure, checkpoints, and deliverables.

## Problem It Solves

Product managers frequently use AI assistants for strategy, discovery, and execution tasks but get shallow, generic responses. PM Skills solves this by:

- **Encoding domain expertise** into reusable prompt templates (skills)
- **Chaining skills into workflows** (commands) that mirror real PM processes
- **Embedding methodology** so the AI follows frameworks like Opportunity Solution Trees, RICE prioritization, or pre-mortem analysis rather than ad-libbing

The target user is a PM who wants AI to function as a structured thinking partner, not a freeform chatbot.

---

## Architecture and Structure

### Three-Tier Design

| Layer | Purpose | Naming Convention | Invocation |
|-------|---------|-------------------|------------|
| **Skills** | Atomic units of domain knowledge. Each skill is a single framework or methodology. | Noun-based (e.g., `opportunity-solution-tree`, `lean-canvas`) | Loaded contextually or referenced by commands |
| **Commands** | Multi-step workflows that chain skills together. | Verb-based (e.g., `/discover`, `/write-prd`, `/plan-launch`) | User-invoked with `/plugin:command` |
| **Plugins** | Thematic packages grouping related skills and commands. | `pm-[domain]` (e.g., `pm-execution`) | Installed as units |

### Plugin Inventory

| Plugin | Skills | Commands | Coverage |
|--------|--------|----------|----------|
| `pm-product-discovery` | 13 | 5 | Ideation, assumption testing, experiments, interviews, feature prioritization |
| `pm-product-strategy` | 12 | 5 | Vision, lean canvas, SWOT, Porter's Five Forces, PESTLE, pricing, Ansoff Matrix |
| `pm-execution` | 15 | 10 | PRDs, OKRs, roadmaps, sprints, user stories, pre-mortems, stakeholder maps |
| `pm-market-research` | 7 | 3 | Personas, segmentation, sentiment analysis, competitive analysis |
| `pm-data-analytics` | 3 | 3 | SQL generation, A/B test analysis, cohort analysis |
| `pm-go-to-market` | 6 | 3 | GTM strategy, growth loops, beachhead segments, battlecards |
| `pm-marketing-growth` | 5 | 2 | North Star metrics, positioning, product naming, value props |
| `pm-toolkit` | 4 | 5 | Resume review, NDA drafting, privacy policies, proofreading |

### File Structure Pattern

```
pm-[domain]/
  .claude-plugin/
    plugin.json          # Package metadata (name, version, description, author)
  commands/
    verb-action.md       # Workflow definitions (chained skill execution)
  skills/
    noun-concept/
      SKILL.md           # Self-contained skill with domain context + instructions
  README.md              # Plugin documentation
```

The root repo also has:
- `.claude-plugin/marketplace.json` -- registers all 8 plugins for Claude Code's marketplace
- `validate_plugins.py` -- structural validation script

---

## Key Patterns

### Skill Anatomy

Each `SKILL.md` follows a consistent structure:

1. **Title and purpose** -- what the framework is and when to use it
2. **Domain Context** -- deep background on the methodology, citing source material and key principles. This is the "expertise injection" that elevates output quality.
3. **Instructions** -- step-by-step process for the AI to follow, with `$ARGUMENTS` as the user's input
4. **Output format** -- explicit template for the deliverable (markdown tables, structured documents)
5. **Further Reading** -- links to source articles (mostly from Product Compass)

Example: The Opportunity Solution Tree skill includes Teresa Torres's 4-level hierarchy, Dan Olsen's Opportunity Score formula, and principles like "compare and contrast" and "discovery is not linear" -- giving the AI genuine framework knowledge rather than surface-level understanding.

### Command Chaining

Commands orchestrate multiple skills into end-to-end workflows with user checkpoints. The `/discover` command, for example:

1. Determines context (existing vs. new product)
2. Runs brainstorm-ideas skill (divergent phase)
3. Checkpoint: user selects 3-5 ideas
4. Runs identify-assumptions skill (critical thinking phase)
5. Runs prioritize-assumptions skill (focus phase)
6. Checkpoint: user confirms priorities
7. Runs brainstorm-experiments skill (validation phase)
8. Compiles a discovery plan document
9. Offers follow-up commands (`/write-prd`, `/interview`, `/setup-metrics`)

This is the most valuable pattern: breaking a 15-30 minute strategic exercise into a guided sequence with human decision points.

### Existing vs. New Product Variants

Discovery skills come in paired variants (e.g., `brainstorm-ideas-existing` and `brainstorm-ideas-new`). This is a smart design choice -- the framing, risk categories, and validation methods differ substantially between continuous discovery on a live product and greenfield exploration.

### Risk Taxonomy (Pre-Mortem)

The pre-mortem skill introduces a memorable three-category risk classification:
- **Tigers** -- real risks backed by evidence
- **Paper Tigers** -- concerns that sound scary but are overblown
- **Elephants** -- unspoken worries the team avoids discussing

Each Tiger is further classified by urgency (launch-blocking, fast-follow, track).

---

## What Makes It Interesting

1. **Methodology depth over prompt cleverness.** The skills don't rely on prompt engineering tricks. They inject genuine domain expertise -- formulas, frameworks, decision criteria, and principles from named practitioners. The AI becomes a better PM advisor because it has better PM knowledge loaded into context.

2. **Human-in-the-loop checkpoints.** Commands don't auto-run to completion. They pause at decision points ("Which ideas should we carry forward?"), making them collaborative rather than generative.

3. **Composable architecture.** Skills are atomic and commands chain them. This means the same `prioritize-assumptions` skill can be used independently or as part of the `/discover` workflow. Good separation of concerns.

4. **Cross-tool compatibility.** While designed for Claude Code, the skills are plain markdown files. They work with Gemini CLI, Cursor, OpenCode, and other tools that support similar patterns.

5. **Explicit output templates.** Every skill specifies exactly what the deliverable looks like -- markdown tables, document structures, naming conventions. This eliminates the "AI gave me a wall of text" problem.

6. **The plugin as a distribution unit.** Using Claude Code's marketplace system for discovery and installation is a smart distribution choice -- PMs can install just the plugins they need.

---

## Comparison with Dex's Approach

| Dimension | PM Skills | Dex |
|-----------|-----------|-----|
| **Scope** | PM-specific frameworks and methodologies | Full professional knowledge system (meetings, people, projects, tasks, career) |
| **Architecture** | Stateless skills/commands (no persistent data) | Stateful vault with PARA structure, MCP integrations, persistent context |
| **Skill design** | Deep methodology injection + output templates | Workflow orchestration with vault reads/writes and cross-system updates |
| **User model** | Any PM, any context | Single user with accumulated context, preferences, identity model |
| **Data layer** | None -- generates documents per session | Rich: person pages, meeting history, task backlog, career evidence, semantic search |
| **Chaining** | Commands chain skills via markdown instructions | Skills chain via MCP tools, hooks, and vault state |
| **Personalization** | Generic (no user context) | Deep (pillars, communication style, career stage, identity model) |
| **Distribution** | GitHub marketplace plugins | Single integrated system |
| **Learning** | None -- each session starts fresh | Session learnings, usage tracking, identity snapshots, improvement backlog |

### Where PM Skills Excels vs. Dex

- **Framework depth.** PM Skills encodes 65 specific PM methodologies. Dex's `/product-brief` skill covers PRD creation, but PM Skills has a dozen strategy frameworks (Porter's Five Forces, PESTLE, Ansoff Matrix, lean canvas, etc.) that Dex doesn't attempt.
- **Focused simplicity.** No setup, no onboarding, no persistent state to manage. Install and use.
- **Methodology citations.** Skills explicitly reference source practitioners and link to further reading, building trust and enabling the user to go deeper.

### Where Dex Excels vs. PM Skills

- **Accumulated context.** Dex knows who you met with last week, what tasks are pending, and which projects are active. PM Skills starts from zero every time.
- **Cross-domain integration.** Dex connects meetings to people to projects to tasks to career evidence. PM Skills generates standalone documents.
- **Personalization.** Dex adapts communication style, infers task pillars, and tracks career development. PM Skills is one-size-fits-all.
- **Persistent state.** Tasks get tracked, meeting notes get filed, person pages get updated. PM Skills produces output but doesn't manage it.

---

## Key Takeaways

- **Methodology-as-code is a powerful pattern.** Encoding named frameworks (Teresa Torres, Dan Olsen, etc.) into structured prompts with domain context sections produces dramatically better AI output than generic instructions. Dex could adopt this pattern for its strategic skills (e.g., enriching `/product-brief` with the PRD template methodology, or adding strategy frameworks as new skills).

- **Paired variants for context matter.** The existing/new product split in discovery skills is a design insight worth borrowing -- the same framework applied to a live product vs. a greenfield idea needs fundamentally different framing, risk categories, and validation approaches.

- **Checkpoints make workflows collaborative, not generative.** The best commands pause at decision points rather than running to completion. This pattern of "diverge, checkpoint, converge" could improve Dex's longer workflows like `/quarter-plan` or `/week-review`.

- **Atomic skills + composed commands is a clean separation.** PM Skills cleanly separates "what the AI knows" (skills) from "what the AI does" (commands). Dex's skills sometimes blend both. There may be value in extracting reusable knowledge modules that multiple Dex skills can reference.

- **Stateless tools complement stateful systems.** PM Skills and Dex are not competitors -- they occupy different niches. A PM could use PM Skills for ad-hoc strategic analysis while using Dex for ongoing work management. The interesting question is whether Dex should absorb some PM Skills patterns (methodology depth) or integrate with them (install PM Skills plugins alongside Dex).
