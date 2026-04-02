# claude-code-pm-skills (Mehdi Bargach)

**Repo:** [github.com/Mehdibargach/claude-code-pm-skills](https://github.com/Mehdibargach/claude-code-pm-skills)
**Author:** Mehdi Bargach (Builder PM, ex-Disney, ex-TF1+)
**License:** MIT
**Reviewed:** 2026-04-02

---

## What Is It?

A collection of 20 self-contained Claude Code skills built specifically for Product Managers. Each skill is a single `SKILL.md` file inside its own folder under `~/.claude/skills/`, invoked via `/command`. No API keys, no external dependencies, no MCP servers -- just markdown prompt engineering that runs directly inside Claude Code.

The pitch: "Real PM workflows you can run with a `/command`" rather than generic prompts.

---

## Architecture

Deliberately minimal. The entire system is a flat collection of skill folders:

```
skills/
├── ab-test-design/SKILL.md
├── competitor-scan/SKILL.md
├── decision-doc/SKILL.md
├── feedback-analyzer/SKILL.md
├── launch-checklist/SKILL.md
├── market-sizing/SKILL.md
├── meeting-prep/SKILL.md
├── metrics-analyzer/SKILL.md
├── okr-writer/SKILL.md
├── persona/SKILL.md
├── prd/SKILL.md
├── prioritize/SKILL.md
├── product-teardown/SKILL.md
├── release-notes/SKILL.md
├── retro-facilitator/SKILL.md
├── roadmap/SKILL.md
├── stakeholder-update/SKILL.md
├── technical-translator/SKILL.md
├── user-interview-prep/SKILL.md
└── user-stories/SKILL.md
```

No shared state, no configuration files, no data persistence, no MCP integration. Each skill is fully independent. Installation is just copying folders into `~/.claude/skills/`.

---

## Skill Categories and Coverage

### Discovery and Strategy (6 skills)

| Skill | Purpose |
|---|---|
| `/product-teardown` | Analyze a product's strategy, UX, business model, growth loops |
| `/competitor-scan` | Competitive positioning and whitespace analysis |
| `/user-interview-prep` | Research briefs with Mom Test-compliant questions |
| `/feedback-analyzer` | Classify feedback by theme, sentiment, urgency from CSV or text |
| `/market-sizing` | TAM/SAM/SOM estimation using dual methodologies |
| `/persona` | Behavioral user archetypes for product decisions |

### Build and Ship (6 skills)

| Skill | Purpose |
|---|---|
| `/prd` | Concise 1-2 page product requirements document |
| `/user-stories` | Feature decomposition with acceptance criteria |
| `/technical-translator` | Convert engineer docs to PM language |
| `/release-notes` | Transform git commits into user-facing narratives |
| `/launch-checklist` | Go-to-market task lists with ownership |
| `/roadmap` | Now/Next/Later structured prioritization |

### Measure and Decide (5 skills)

| Skill | Purpose |
|---|---|
| `/ab-test-design` | Full experiment frameworks with guardrails |
| `/metrics-analyzer` | CSV data analysis generating insights |
| `/prioritize` | RICE/ICE scoring with ranked outputs |
| `/decision-doc` | Options analysis with trade-off evaluation |
| `/okr-writer` | Goal articulation with measurable Key Results |

### Communicate (3 skills)

| Skill | Purpose |
|---|---|
| `/stakeholder-update` | Notes converted to polished communications |
| `/meeting-prep` | Single-page meeting briefs |
| `/retro-facilitator` | Retrospective synthesis using multiple frameworks |

---

## Skill Quality Assessment

Examined four skills in detail: `/prd`, `/product-teardown`, `/ab-test-design`, `/feedback-analyzer`.

### Strengths

- **Opinionated and specific.** Skills enforce constraints aggressively. The PRD skill demands numerical metric targets ("improve retention" is explicitly rejected), the A/B test skill requires predicted magnitude in hypotheses, and feedback-analyzer mandates minimum 3 distinct themes. This is good prompt engineering -- it prevents Claude from producing generic output.
- **Structured output formats.** Each skill defines exact sections and formatting. The A/B test skill specifies 8 sequential steps with a pre-launch checklist. The feedback analyzer defines a complete classification taxonomy (Theme, Sentiment, Urgency, Type).
- **Push-back on vague input.** Multiple skills instruct Claude to demand clarification rather than guessing. The PRD skill says "Vague input requires listing prerequisite questions before drafting." This is a valuable pattern.
- **Density discipline.** Hard 1-2 page caps on output across all skills. Forces prioritization over comprehensiveness.
- **Tool awareness.** Skills declare specific allowed tools (Read, Write, Bash, WebFetch, WebSearch, Glob, Grep) matching their needs. The product-teardown uses WebFetch/WebSearch for research; the A/B test skill uses Bash for sample size calculations.

### Weaknesses

- **No persistent state.** Skills generate documents but nothing connects them. A PRD created today has no relationship to the roadmap created yesterday. There is no project concept, no people tracking, no task management.
- **No context accumulation.** Each invocation starts from zero. The meeting-prep skill cannot reference past meetings. The stakeholder-update cannot pull from a history of communications with that stakeholder.
- **Shallow testing.** The test-cases.md contains one example invocation per skill with no expected outputs, no edge cases, no error scenarios. It is a smoke test, not a quality assurance suite.
- **No workflow orchestration.** Skills are described as "chainable" but there is no mechanism for chaining -- the user must manually run them in sequence and copy outputs between them.
- **Generic PM, not personalized.** No user profile, no company context, no role adaptation. Every user gets identical output regardless of their domain, seniority, or working style.

---

## Comparison with Dex

| Dimension | Dex | Bargach PM Skills |
|---|---|---|
| **Scope** | Full knowledge system (people, projects, meetings, tasks, career) | 20 standalone document generators |
| **State** | Persistent vault with PARA structure, person pages, task tracking | Stateless -- each invocation is independent |
| **Context** | Accumulates over time (meeting history, person relationships, project evolution) | Zero memory between invocations |
| **Personalization** | User profile, communication style, pillar-based task inference, identity model | None -- identical output for every user |
| **Integration** | MCP servers, calendar, Granola, semantic search, ScreenPipe | No external integrations |
| **PM Artifacts** | Product briefs, meeting prep, project health (connected to vault context) | PRDs, teardowns, A/B tests, OKRs (standalone) |
| **Onboarding** | Multi-step MCP-validated onboarding flow | Copy folders, start using |
| **Setup complexity** | Significant (MCP servers, config, Python dependencies) | Near zero |
| **Skill depth** | Fewer PM-specific templates, but deeply integrated with vault data | 20 well-crafted templates with strong constraints |

### What Bargach Does That Dex Does Not

- **Product teardown** -- structured competitive product analysis with web research
- **Market sizing** -- TAM/SAM/SOM estimation frameworks
- **A/B test design** -- experiment design with sample size calculations and guardrail metrics
- **Feedback analyzer** -- bulk classification of user feedback from CSV
- **RICE/ICE prioritization** -- formal scoring frameworks for feature prioritization
- **User persona generation** -- behavioral archetype creation
- **Technical translator** -- converting engineering docs to PM language
- **Release notes generation** -- git commits to user-facing narratives
- **Retro facilitator** -- structured retrospective synthesis
- **OKR writer** -- formal OKR articulation with measurable KRs

### What Dex Does That Bargach Does Not

- People tracking with automatic relationship context
- Meeting processing with person page updates and task extraction
- Persistent task management with cross-reference propagation
- Career development tracking with evidence capture
- Semantic search across accumulated knowledge
- Calendar-aware daily and weekly planning
- Quarterly goal setting and tracking
- Identity modeling and communication adaptation
- Project health monitoring across time

---

## Design Philosophy Differences

**Bargach** optimizes for **individual artifact quality**. Each skill is a finely tuned prompt that produces a specific, high-quality document. The philosophy is: make each output excellent in isolation.

**Dex** optimizes for **accumulated context**. Individual outputs may be less templated, but they are connected to everything else -- people, projects, meetings, tasks. The philosophy is: value compounds over time through relationships between information.

These are complementary rather than competing approaches. Bargach's skills would become significantly more powerful with access to vault context (imagine a `/meeting-prep` that knows your full history with each attendee), and Dex could benefit from Bargach's rigorously constrained output templates.

---

## Key Takeaways

- **The prompt engineering is genuinely good.** Aggressive constraint enforcement (page limits, numerical targets, mandatory sections, push-back on vagueness) is the right approach for PM artifacts. Several of these patterns are worth adopting.
- **Statelessness is both the main strength and the main limitation.** Zero setup cost makes adoption trivial, but it caps the ceiling. These skills cannot get smarter over time because they have no memory.
- **There are 8-10 PM skill archetypes Dex lacks.** Product teardowns, market sizing, A/B test design, feedback analysis, RICE prioritization, persona generation, technical translation, release notes, retro facilitation, and OKR writing are all gaps in Dex's current skill set.
- **The constraint-driven prompting pattern is portable.** The technique of defining explicit rejection criteria ("improve retention" is invalid, hypotheses must include magnitude) could strengthen Dex's existing skills like `/product-brief` and `/meeting-prep`.
- **Integration potential is high.** These skills are MIT-licensed, self-contained markdown files. Any of them could be adapted into Dex skills with vault context injected -- the combination of Bargach's output templates and Dex's accumulated knowledge would be stronger than either alone.
