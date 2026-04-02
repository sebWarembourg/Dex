# Carl Vellotti - PM Operating System (Product Growth Podcast Ep. 3 Demo)

**Repo:** [pg-carl-vellotti-master-cc](https://github.com/carlvellotti/pg-carl-vellotti-master-cc)
**Purpose:** Live demo environment for Product Growth podcast Episode 3, "From Watching to Collaborating"
**Date analyzed:** 2026-04-02

---

## What Is This?

A fully functional **PM Operating System** built on Claude Code for a fictional senior PM (Alex Chen at GradeFlow, an edtech startup). It demonstrates how Claude Code can serve as a daily thinking partner for product managers -- not for writing code, but for organizing work, preparing for meetings, synthesizing research, and maintaining institutional knowledge.

The repo is both a demo and a teachable template. Carl Vellotti also maintains a [blank template version](https://github.com/carlvellotti/carls-product-os) for others to fork.

**Context:** This is Episode 3 in a series. The progression shows increasing sophistication -- from watching Claude Code work, to actively collaborating with it as a PM workflow tool.

---

## Architecture and Structure

### Folder Layout

```
pg-carl-vellotti-master-cc/
├── CLAUDE.md              # Entry point -- persona, folder map, commands, working style
├── GOALS.md               # Identity, ownership areas, Q1 goals with metrics
├── DEMO-GUIDE.md          # Exact prompts and scripts for the podcast demo
├── .claude/
│   ├── settings.json      # Empty {} -- minimal config
│   ├── hooks/
│   │   └── skill-activator.py   # Auto-trigger hook (59ms keyword matcher)
│   └── skills/            # 8 slash commands
│       ├── standup/
│       ├── meeting-prep/
│       ├── weekly-update/
│       ├── synthesize-research/
│       ├── draft-prd-section/
│       ├── spin-up/
│       ├── make-slides/
│       └── web-research/
├── Tasks/
│   ├── active.md          # Current sprint
│   ├── backlog.md         # Future work
│   └── archive/           # Completed by month
├── Projects/              # 3 active projects with briefs, research, outputs
│   ├── grading-consistency-research/
│   ├── q1-board-deck/
│   └── ta-workload-tracker/
├── Meetings/              # 1:1 notes, standups, meeting records
├── Knowledge/
│   ├── People/            # Person profiles (communication style, preferences)
│   ├── Reference/         # Company context
│   └── Research/          # Domain research
├── Workflows/             # Multi-step repeatable processes
│   ├── weekly-stakeholder-update/
│   │   ├── CLAUDE.md
│   │   ├── draft-template.md
│   │   ├── gather-metrics.md
│   │   ├── stakeholder-preferences.md
│   │   ├── workflow-spec.md
│   │   └── past-updates/
│   ├── quarterly-planning/
│   └── user-research-synthesis/
├── Templates/             # 6 document templates (PRD, brief, 1:1, interview, etc.)
├── Tools/                 # Python scripts (metrics-pull, meeting-prep)
├── _Registry/             # System docs: MCPs.md, Tags.md, Tools.md
├── _temp/
└── data/                  # 212-response survey CSV for Jupyter demo
```

### Design Philosophy

The system is organized around **a PM's daily workflow**, not around Claude Code's capabilities. The folder structure mirrors how a PM actually thinks: tasks, projects, meetings, people, knowledge. Claude Code is the interface layer, not the organizing principle.

---

## CLAUDE.md Analysis

The CLAUDE.md is approximately 120 lines and does several things well:

### 1. Persona-First Design
Opens with "Who I Am" -- name, role, company, team size, reporting line. This immediately grounds Claude in a specific professional context. Every response is filtered through "I'm a Senior PM at an edtech startup."

### 2. Explicit Folder Map
Maps every folder with its purpose. Claude never has to guess where things live.

### 3. Commands as Behavior Contracts
Each slash command (`/standup`, `/weekly-update`, `/synthesize-research`) is described with what Claude "will" do -- not vague suggestions but specific multi-step behaviors.

### 4. Working Style Preferences
"When I ask about a project: check Projects/ first." "When I'm stuck: ask me clarifying questions." These are interaction protocols, not just instructions.

### 5. Current Context Block
A "Current Context" section with the big bet, key risk, stakeholder tension, and upcoming deadline. This is ephemeral context that changes frequently -- smart to separate it from structural instructions.

### 6. Sub-Agent Delegation Rules
Explicit rules for when to spawn sub-agents vs. stay in the main conversation. The threshold is clear: "If a task will read more than ~3 files or produce output the user doesn't need to see verbatim, delegate it."

---

## Skills Analysis

### Skill Tiers (from the demo narrative)

The demo presents skills in three tiers of increasing capability:

| Tier | Example | Pattern |
|------|---------|---------|
| Prompt-only | `/standup`, `/meeting-prep`, frontend-design | Pure markdown instructions, no tools |
| Prompt + Tools | `/web-research` | Instructions + MCP tools (Tavily, Firecrawl) |
| Prompt + Tools + Self-Validation | `/make-slides` | Instructions + tools + Puppeteer screenshots for self-checking |

This tiering is a useful mental model. Most people stop at tier 1.

### Notable Skills

**`/standup`** -- Pulls from git history, active tasks, GOALS.md, calendar, and Linear. Outputs a structured morning briefing with schedule, in-progress work, blockers, and recommended focus areas. The key insight: it synthesizes across 4-5 data sources into one view.

**`/meeting-prep [person]`** -- Reads the person's profile from `Knowledge/People/`, pulls past meeting notes, gathers current business context, and generates a tailored prep document. Adapts format to individual communication preferences (e.g., BLUF for David Chen).

**`/spin-up`** -- A meta-skill that adds context management instructions to other projects' CLAUDE.md files. It teaches the sub-agent delegation pattern. This is a skill that improves how you use Claude Code itself.

**`/weekly-update`** -- Has its own Workflow folder with a spec, draft template, stakeholder preferences, metrics gathering guide, and archive of past updates. The skill reads past updates to maintain consistent voice. This is the most "production-grade" skill in the repo.

**`/web-research`** -- Integrates Tavily (search API) and Firecrawl (web scraper). Includes a tool selection matrix: quick facts -> tavily_search, broad research -> tavily_research, specific URLs -> firecrawl scrape. The skill is essentially a decision tree for web information gathering.

---

## Hook: Skill Auto-Activator

The `skill-activator.py` hook is the most technically interesting piece:

- **Trigger:** Runs on every `UserPromptSubmit` event
- **Speed:** ~59ms (no AI call, pure keyword matching)
- **Mechanism:** Parses YAML frontmatter from all SKILL.md files, extracts trigger phrases from descriptions, then checks if the user's prompt contains those keywords
- **Output:** When matched, injects `additionalContext` that forces Claude to evaluate each matched skill as YES/NO before proceeding
- **Cap:** Maximum 5 skill matches per prompt

This solves a real problem: users forget slash commands exist. The hook makes skills fire naturally from conversational prompts like "make me a slide" instead of requiring `/make-slides`.

The two-phase matching is smart: first checks if the skill name appears in the prompt (handling hyphenation), then falls back to trigger phrase extraction from the skill description.

---

## People Profiles Pattern

The `Knowledge/People/` directory contains structured profiles that go beyond basic contact info:

**David Chen example includes:**
- Role, reporting line, tenure, career history
- Communication preferences (written > verbal, BLUF format, Slack > email)
- Working style (decides quickly, uncertainty = soft no, pushback = worth exploring)
- Meeting preferences (mornings = focus time, Tue/Thu afternoons for sync)
- What he cares about (specs, customer impact, team health)
- How to work with him (bring agenda, lead with blockers, present options with recommendation)

This is genuinely useful context that transforms generic meeting prep into person-specific preparation. The `/meeting-prep` skill directly reads these profiles.

---

## Workflow Architecture

The `Workflows/` folder is an underrated pattern. Each workflow is a self-contained directory with:

- **CLAUDE.md** -- Workflow-specific instructions (scoped, not global)
- **Spec** -- What the workflow does and when to use it
- **Templates** -- Output formats
- **Supporting docs** -- Preferences, metrics guides, etc.
- **Archive** -- Past outputs for tone/style matching

This creates a "workflow as a package" pattern. The weekly stakeholder update has 5 supporting files plus an archive of past updates. When the skill runs, it has everything it needs in one directory -- it does not depend on scattered context across the repo.

---

## _Registry Pattern

The `_Registry/` folder serves as a system-level documentation layer:

- **MCPs.md** -- Documents active MCP servers (Google Calendar, Granola) and planned integrations (Amplitude, Linear). Includes conditional logic guidance for skills to check MCP availability.
- **Tools.md** -- Catalogs Python tools with usage patterns, development standards, and a roadmap. Defines the `python Tools/tool-name/run.py [args]` convention.
- **Tags.md** -- Likely a taxonomy for organizing content.

This is a self-documenting system. Claude can read the registry to understand what tools and integrations are available before attempting to use them.

---

## What's Unique vs. Standard Claude Code Setup

### 1. Non-Engineering Focus
Most Claude Code repos are about writing software. This one uses Claude Code exclusively as a PM productivity tool -- no application code exists. The Python is only for tools and data analysis.

### 2. Fictional but Realistic Scenario
The GradeFlow/Alex Chen scenario is fictional but deeply fleshed out -- real metrics, real stakeholder dynamics, real product challenges. This makes it both a demo and a learning environment. You can run `/standup` and get a meaningful output because the data is there.

### 3. Context Management as a First-Class Concern
The status line setup (a 10-character progress bar showing context usage, color-coded with skull emoji at 95%+) and the explicit sub-agent delegation rules show unusual sophistication about context window management. The `/spin-up` meta-skill that propagates these patterns to other projects is particularly thoughtful.

### 4. Workflow-as-Directory Pattern
Most Claude Code setups have skills as isolated SKILL.md files. Here, workflows get full directories with specs, templates, preferences, and archives. The weekly-update workflow has 5 supporting files. This creates much richer skill execution.

### 5. Person-Aware Meeting Prep
The combination of structured people profiles + meeting history + active context, routed through a `/meeting-prep` skill, is a complete system for a real PM workflow. The profiles include communication preferences that change the output format.

### 6. Demo-as-Documentation
The DEMO-GUIDE.md is a 1,500+ line script with exact prompts, expected behaviors, and narrative bridges. It doubles as the most thorough documentation of what the system can do and why each feature matters.

---

## Comparison with Dex

| Aspect | Carl's PM OS | Dex |
|--------|-------------|-----|
| **Target user** | Product managers | Knowledge workers (general) |
| **Persona** | Fictional PM (Alex Chen) | Real user (configured during onboarding) |
| **Folder system** | Custom PM-oriented | PARA method |
| **Skills** | 8, PM-focused | 50+, broad coverage |
| **People pages** | `Knowledge/People/` with communication prefs | `05-Areas/People/` with Internal/External routing |
| **Task system** | Simple active/backlog/archive | Full task IDs, cross-linked, MCP-managed |
| **Hooks** | 1 (skill auto-activator) | Multiple (context injectors, etc.) |
| **MCP servers** | Documented but minimal (Calendar, Granola) | Extensive (Work MCP, QMD, Update Checker, etc.) |
| **Context mgmt** | Explicit status line + delegation rules | Implicit via MCP architecture |
| **Workflows** | Directory-based with archives | Skill-based |
| **Self-improvement** | Not present | Backlog, learnings, level-up system |
| **Maturity** | Demo/template | Production system |

---

## Key Takeaways

1. **Workflow-as-directory is worth adopting.** Giving complex skills their own folder with specs, templates, stakeholder preferences, and past output archives creates dramatically richer execution context. The weekly-update workflow with 5 supporting files plus an archive of past updates for tone matching is the best example. This pattern could improve Dex skills like `/weekly-update` and `/process-meetings`.

2. **The skill auto-activator hook is a clever UX pattern.** A 59ms Python script that keyword-matches user prompts against installed skills and injects activation context -- no AI call, no latency, no manual slash commands needed. This bridges the gap between "skills exist" and "skills get used." Dex could benefit from a similar approach to surface relevant skills without requiring the user to remember command names.

3. **Person profiles with communication preferences change meeting prep quality.** Including fields like "prefers BLUF format," "responds to Slack within 2 hours," and "uncertainty means soft no" transforms generic prep into person-specific coaching. Dex person pages could be enriched with a structured communication style section.

4. **Context window awareness should be visible, not hidden.** The status line showing a color-coded progress bar (green -> yellow -> orange -> blinking red skull) and the explicit "smart zone model" (0-40% sharp, 40-60% degrading, 80%+ hallucinations) make context management intuitive. This is a teaching tool as much as a utility.

5. **A GOALS.md file with metrics, baselines, and targets gives Claude real grounding.** Rather than vague "work on product stuff" instructions, the file specifies "reduce grading inconsistency from 0.8 to 0.48 std dev" -- this makes every skill output more specific and actionable. Dex's quarter goals could adopt this baseline/target/status pattern for richer planning outputs.
