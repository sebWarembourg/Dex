# Carl Vellotti - Product OS Template (Blank + Example)

**Repo:** [carls-product-os](https://github.com/carlvellotti/carls-product-os)
**Purpose:** Public template for building a PM Operating System on Claude Code
**Date analyzed:** 2026-04-02
**Related:** [[carl-vellotti-pg-master]] (the podcast demo version)

---

## What Is This?

A **teaching repository** that ships two versions of the same PM Operating System side by side:

- **BLANK-OS/** -- A stripped-down scaffolding with TODO placeholders. Fork this, fill in the blanks, start working.
- **EXAMPLE-OS/** -- A populated version using the same fictional "Alex Chen at GradeFlow" scenario from the podcast demo. Learn by reading, then adapt.

The repo README frames the core problem clearly: Claude Code offers infinite flexibility, but most people don't know *how to structure a workspace*. This template answers that question for product managers.

**136 stars, 40 forks, only 3 commits** -- this is a reference artifact, not an actively developed project.

---

## Repository Structure

```
carls-product-os/
├── README.md           # Setup guide, conceptual distinctions, getting started
├── BLANK-OS/           # The empty template
│   ├── CLAUDE.md       # TODO placeholders for identity, commands, context
│   ├── GOALS.md        # TODO placeholders for role, ownership, goals, stakeholders
│   ├── .claude/
│   │   └── skills/
│   │       └── standup/SKILL.md    # Single starter skill
│   ├── Tasks/
│   │   ├── active.md   # Template with usage instructions
│   │   └── backlog.md  # Template with priority tiers
│   ├── Templates/
│   │   ├── 1on1-notes.md
│   │   ├── prd.md
│   │   └── weekly-update.md
│   └── _Registry/
│       ├── MCPs.md     # MCP documentation template
│       ├── Tags.md     # Taxonomy template (status, priority, type tags)
│       └── Tools.md    # Tool inventory template
│
└── EXAMPLE-OS/         # The populated example
    ├── CLAUDE.md       # Full Alex Chen persona, folder map, commands, context
    ├── GOALS.md        # Detailed role, Q1 goals with metrics, stakeholder table
    ├── .claude/
    │   └── skills/     # 5 skills
    │       ├── standup/
    │       ├── meeting-prep/
    │       ├── weekly-update/
    │       ├── synthesize-research/
    │       └── draft-prd-section/
    ├── Tasks/
    │   ├── active.md
    │   ├── backlog.md
    │   └── archive/
    ├── Projects/       # 3 populated projects
    │   ├── grading-consistency-research/
    │   ├── q1-board-deck/
    │   └── ta-workload-tracker/
    ├── Meetings/
    │   ├── 1on1s/
    │   ├── one-offs/
    │   └── standups/
    ├── Knowledge/
    │   ├── People/
    │   ├── Reference/
    │   └── Research/
    ├── Workflows/      # 3 workflow directories
    │   ├── quarterly-planning/
    │   ├── user-research-synthesis/
    │   └── weekly-stakeholder-update/
    ├── Templates/      # 6 templates (3 more than BLANK-OS)
    │   ├── 1on1-notes.md
    │   ├── interview-notes.md
    │   ├── prd.md
    │   ├── project-brief.md
    │   ├── research-summary.md
    │   └── weekly-update.md
    ├── Tools/
    ├── _Registry/
    └── _temp/
```

---

## Differences vs Demo Version

The demo version (`pg-carl-vellotti-master-cc`) was a fully staged environment for a podcast episode. This template repo strips it down for real-world use.

### What's Removed

| Feature | Demo Version | Template (BLANK-OS) | Template (EXAMPLE-OS) |
|---------|-------------|---------------------|----------------------|
| **Skill auto-activator hook** | Yes (`skill-activator.py`) | No | No |
| **DEMO-GUIDE.md** | Yes (1,500+ lines) | No | No |
| **`/spin-up` skill** | Yes | No | No |
| **`/make-slides` skill** | Yes | No | No |
| **`/web-research` skill** | Yes | No | No |
| **`data/` folder** (survey CSV) | Yes | No | No |
| **Context status line** | Yes (progress bar + skull emoji) | No | No |
| **Sub-agent delegation rules** | Yes (explicit in CLAUDE.md) | No | No |
| **Populated meeting notes** | Yes (full 1:1 histories) | No | Not verified |
| **People profiles** | Yes (David Chen etc.) | No | Directory exists |

### What's Kept (EXAMPLE-OS)

- Same fictional scenario (Alex Chen, GradeFlow, edtech)
- Same GOALS.md structure with Q1 metrics, baselines, targets
- Same 3 projects (grading consistency, Q1 board deck, TA workload)
- Same workflow-as-directory pattern (3 workflows)
- 5 of the original 8 skills (the 5 core PM skills)

### What's Simplified (BLANK-OS)

- **1 skill** instead of 8 (just `/standup` as a starter)
- **3 templates** instead of 6 (1:1, PRD, weekly update -- the essentials)
- **No Projects/, Meetings/, Knowledge/, Workflows/, Tools/ folders** -- users create these as needed
- **No _temp/ folder**
- **CLAUDE.md** reduced to pure scaffolding with TODO comments
- **GOALS.md** is all placeholders -- quarter, year, goals, stakeholders

### What's New in the Template Repo

- **Side-by-side learning structure** (BLANK-OS + EXAMPLE-OS in one repo) -- the demo version was a single working environment
- **README with conceptual distinctions** -- explicitly teaches the difference between Projects vs Workflows, Templates vs Workflows, Knowledge vs Project Research
- **Getting started guidance** -- "Start with CLAUDE.md and GOALS.md (provides 80% of value)"

---

## CLAUDE.md Comparison

### Demo Version (~120 lines)
- Full Alex Chen persona with current context (big bet, key risk, stakeholder tension, deadline)
- Explicit sub-agent delegation rules with context window thresholds
- Status line configuration for context usage visualization
- Working style preferences ("When I ask about a project: check Projects/ first")
- Commands documented as behavior contracts

### EXAMPLE-OS Version (~80 lines, estimated)
- Same persona setup (Alex Chen, Senior PM, GradeFlow)
- Folder structure reference
- Three key commands: `/standup`, `/weekly-update`, `/synthesize-research`
- Current context block (AI Feedback Assistant launch, TA adoption, Prof. Williams tension)
- Work collaboration guidelines
- No sub-agent delegation rules
- No status line configuration
- No context window management instructions

### BLANK-OS Version (~50 lines, estimated)
- All TODO placeholders: Name, Role, Team
- Generic folder map listing all possible directories
- Command table with blank definitions
- "Context Files" section pointing to GOALS.md and Tasks/active.md
- Brief notes on markdown conventions and wikilinks
- No behavioral instructions at all

**Assessment:** The template CLAUDE.md teaches *structure* but not *behavior*. The demo version's sophistication around context management, sub-agent delegation, and working style preferences is lost. Someone forking BLANK-OS would get the skeleton but miss the interaction design patterns that made the demo compelling.

---

## Skills Comparison

| Skill | Demo (8) | Example (5) | Blank (1) | Category |
|-------|----------|-------------|-----------|----------|
| `/standup` | Yes | Yes | Yes | Daily routine |
| `/meeting-prep` | Yes | Yes | No | People context |
| `/weekly-update` | Yes | Yes | No | Stakeholder comms |
| `/synthesize-research` | Yes | Yes | No | Knowledge work |
| `/draft-prd-section` | Yes | Yes | No | Document creation |
| `/spin-up` | Yes | No | No | Meta / context mgmt |
| `/make-slides` | Yes | No | No | Output generation |
| `/web-research` | Yes | No | No | External info gathering |

**What was cut:** The three most technically advanced skills -- the ones requiring MCP integrations (Tavily, Firecrawl) or self-validation (Puppeteer). The template keeps the five pure-markdown skills that work with zero external setup.

**Skill quality:** The EXAMPLE-OS skills are well-written. The `/standup` skill has a 5-step process (activity gathering, task review, calendar check, issue tracking, synthesis). The `/weekly-update` skill emphasizes metrics over activities and enforces a 500-word limit. These are production-usable templates, not toy examples.

---

## Templates and Scaffolding

### BLANK-OS Templates (3)

1. **1on1-notes.md** -- Pre-meeting section (updates, questions, asks), notes, action items split by "Me" and "Them", follow-ups for next time. Clean and immediately usable.

2. **prd.md** -- Full PRD scaffold: problem statement, goals/non-goals, user stories, P0/P1/P2 requirements, design, technical considerations, launch plan with metrics table, open questions. Comprehensive.

3. **weekly-update.md** -- Summary, accomplishments, in progress, blockers/needs, next week, metrics table with trend column. Standard but well-structured.

### EXAMPLE-OS adds 3 more

4. **interview-notes.md** -- For user research sessions
5. **project-brief.md** -- For kicking off new projects
6. **research-summary.md** -- For consolidating research findings

### Task Templates

The `active.md` and `backlog.md` files in BLANK-OS include inline instructions ("Keep this list short, 3-5 items max", "Move items here from backlog.md when starting work"). They teach the workflow, not just the format.

---

## Setup Flow

There is **no automated onboarding**. The approach is entirely "fill in the blanks":

1. Look at EXAMPLE-OS to understand what a populated system looks like
2. Copy BLANK-OS to your workspace
3. Fill in CLAUDE.md (name, role, team) and GOALS.md (ownership, metrics, stakeholders)
4. Run `/standup` to verify it works
5. Add folders and skills incrementally as needed

The README explicitly recommends starting with just CLAUDE.md and GOALS.md, claiming these two files provide "80% of the value." This is a deliberate minimal-viable-setup philosophy -- the opposite of Dex's guided onboarding with validation steps.

---

## Hooks

**No hooks in either version.** The skill auto-activator from the demo (`skill-activator.py`) was removed entirely. The template relies on explicit slash commands only. This is a significant simplification -- the auto-activator was one of the most technically interesting pieces in the demo, but it also had a learning curve to understand and customize.

---

## What Makes This Effective as a Template

### Strengths

1. **Side-by-side blank + example** is excellent pedagogy. You can diff BLANK-OS/CLAUDE.md against EXAMPLE-OS/CLAUDE.md to see exactly what "filled in" looks like.

2. **Progressive disclosure.** BLANK-OS ships with 1 skill. You don't face 8 skills you need to understand before starting. Add skills as you need them.

3. **The README teaches concepts, not just structure.** The Projects vs Workflows vs Templates distinctions help users make organizational decisions, not just copy folders.

4. **Task files teach workflow.** The inline instructions in active.md and backlog.md ("keep 3-5 items", "move from backlog when starting") encode process, not just format.

5. **Production-quality skills in EXAMPLE-OS.** These are not placeholder examples -- they're genuinely useful skill definitions that someone could copy directly and adapt.

### Weaknesses

1. **No behavioral instructions in BLANK-OS CLAUDE.md.** The demo version's sophistication around "how to interact" (working style, context management, delegation rules) is entirely absent. A new user gets folder structure but no interaction patterns.

2. **No registry content.** The _Registry files are all TODO placeholders. A new user wouldn't know what to put in MCPs.md, Tags.md, or Tools.md without studying the EXAMPLE-OS versions.

3. **No automation.** No hooks, no MCP setup, no calendar integration. Everything is manual slash commands. This is intentionally simple but means the template can't demonstrate the more powerful patterns.

4. **The GOALS.md template is strong but the CLAUDE.md template is weak.** GOALS.md has clear sections with examples in comments. CLAUDE.md has generic folder listings but doesn't prompt the user to define working style, interaction preferences, or behavioral rules -- the things that actually make Claude Code useful as a daily partner.

---

## Key Takeaways

1. **The side-by-side blank/example pattern is the best template structure we've seen.** Rather than one repo with demo data you need to delete, or one empty repo with no guidance, shipping both lets users learn by comparison. Dex's onboarding is more sophisticated (MCP-validated, conversational) but the "here's what it looks like when it's working" reference is something to learn from.

2. **"Start with 2 files, add incrementally" is a compelling setup philosophy.** The claim that CLAUDE.md + GOALS.md provide 80% of the value is likely accurate for most users. Dex's full PARA structure with 10+ folders can feel overwhelming on day one. The template's progressive disclosure approach -- start minimal, add as needed -- is worth considering for Dex's onboarding Phase 2.

3. **The template exposes a gap in behavioral scaffolding.** Structure (folders, files, templates) is well-covered. Behavior (how Claude should interact, when to push back, how to manage context) is absent from the blank version. This is where Dex's CLAUDE.md is dramatically stronger -- it defines persona, communication style, and dozens of behavioral rules. The lesson: structure without behavior is a filing system, not an assistant.

4. **Five PM-specific skills is the right starter set for the audience.** Standup, meeting prep, weekly update, research synthesis, and PRD drafting cover the daily loop of a product manager. They're also the five skills that work without any MCP setup. The template correctly strips the three skills that require external tooling (web research, slides, spin-up).

5. **This template validates that non-engineers are a real audience for Claude Code.** 136 stars and 40 forks on a repo with zero application code -- just markdown files organizing a PM's workflow. The demand signal is clear: people want structured workspaces for knowledge work, not just coding projects.
