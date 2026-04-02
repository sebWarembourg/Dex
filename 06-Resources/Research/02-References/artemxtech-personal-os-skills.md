# ArtemXTech/personal-os-skills

**Repo:** [github.com/ArtemXTech/personal-os-skills](https://github.com/ArtemXTech/personal-os-skills)
**Author:** Artem Zhutov
**License:** MIT
**Language:** Python (100%)
**Stars:** ~358 | **Forks:** ~59
**Last assessed:** 2026-04-02

---

## What Is It?

A collection of six Claude Code skills designed to bridge productivity tools into an Obsidian vault. The project frames itself as a "Personal OS" -- using Claude Code as the orchestration layer and Obsidian as the knowledge store. It is part of a broader "Claude Code x Obsidian Lab" initiative (6-week cohort with 12 live sessions), which explains its educational/community orientation.

Unlike Dex or PM OS, this is **not a full operating system or workflow framework**. It is a curated set of **integration skills** -- each one connects a specific external tool (Granola, Wispr Flow, NotebookLM, TaskNotes) or Claude Code's own session data into Obsidian. There is no overarching daily planning, career tracking, or strategic pillar system.

---

## Architecture and Structure

```
personal-os-skills/
  .claude-plugin/
    marketplace.json          # Plugin registry for Claude Code marketplace
  docs/
    memory-skills-readme.md   # Architecture doc for recall + sync skills
    memory-skills-setup.md    # Setup guide
    tasknotes/
      README.md, install.png
  skills/
    granola/                  # Meeting notes from Granola AI
    wispr-flow/               # Voice dictation analytics
    tasknotes/                # Obsidian TaskNotes plugin bridge
    notebooklm/               # Google NotebookLM importer
    recall/                   # Session memory retrieval
    sync-claude-sessions/     # Export Claude conversations to Obsidian
  CLAUDE.md                   # Repo-level instructions (skill structure, publishing)
  README.md
  LICENSE
```

Each skill follows a consistent internal structure:

```
skill-name/
  SKILL.md          # Skill definition (YAML frontmatter + instructions)
  scripts/          # Python CLI tools (self-contained, no pip deps for most)
  templates/        # Obsidian markdown templates (optional)
  workflows/        # Step-by-step workflow guides (optional)
  docs/             # Skill-specific documentation (optional)
```

**Key design decision:** Scripts are self-contained with no external pip dependencies (exception: NotebookLM requires `notebooklm-py` and Playwright). This makes installation trivial -- copy files, run commands.

**Distribution model:** Uses Claude Code's plugin marketplace system (`.claude-plugin/marketplace.json`). Users install via `/plugin marketplace add ArtemXTech/personal-os-skills`, which is a cleaner distribution path than manual git cloning.

---

## Skill-by-Skill Breakdown

### 1. Granola (Meeting Sync)

Reads directly from Granola's local cache (`~/Library/Application Support/Granola/cache-v3.json`) -- no API key needed. Three commands: `list`, `get`, `sync`. Syncs meetings to `Meetings/` folder with YAML frontmatter (date, duration, attendees, status). Transcript includes speaker attribution with mic/system audio indicators.

**Quality:** Solid. Clean Python CLI, deduplication on sync, local-only data access. The cache-reading approach is clever -- avoids API rate limits and auth complexity. Mac-only (hardcoded macOS paths).

### 2. Wispr Flow (Voice Dictation)

Reads from Wispr Flow's local SQLite database. Four capabilities: stats, search, export, dashboard (interactive HTML). Supports filtering by app, date range, and keyword.

**Quality:** Good utility skill. Direct SQLite access is practical. Dashboard generation is a nice touch. Again Mac-only.

### 3. TaskNotes (Task Management)

Bridges to Obsidian's TaskNotes plugin via its local HTTP API (port 8080). CRUD operations on tasks with projects, priorities, contexts, due dates.

**Quality:** Thin wrapper. Depends on a specific Obsidian plugin being installed and running its HTTP server. Limited compared to native task management approaches.

### 4. NotebookLM (Knowledge Import)

The most complex skill. Imports NotebookLM notebooks into Obsidian as interconnected pages with wikilinks. Three Python scripts handle source import, passage extraction, and citation resolution. Has a Q&A workflow that resolves numeric citations to deep-linked passages.

**Quality:** Ambitious and well-structured. The citation-to-wikilink resolution is genuinely useful for research workflows. Requires `notebooklm-py` + Playwright + Google auth, which adds friction. Depends on Dataview plugin for dashboards.

### 5. Recall (Session Memory)

Three retrieval modes:
- **Temporal:** Scans Claude Code's native JSONL session logs by date
- **Topic:** BM25 search via QMD across sessions, notes, daily logs
- **Graph:** Generates interactive HTML visualizations (nodes = sessions, edges = shared files)

Every recall ends with a "One Thing" synthesis -- the single most impactful next action.

**Quality:** The strongest skill conceptually. The temporal mode works with zero setup (reads existing Claude session files). Graph visualization reveals cross-session patterns. The "One Thing" framing is smart -- prevents information overload.

### 6. Sync Claude Sessions

Exports Claude Code conversations to Obsidian markdown. Features: auto-sync via hooks, session annotation, resume previous sessions, tagging schema (YAML-defined categories).

**Quality:** Well thought out. Preserves a `## My Notes` section during re-syncs. Tag schema enforces consistency. The resume workflow is practical for multi-day work.

---

## Comparison to Similar Projects

| Dimension | ArtemXTech/personal-os-skills | Dex (carl-vellotti) | phuryn/pm-skills |
|---|---|---|---|
| **Scope** | Integration toolkit (6 skills) | Full PM operating system | PM skill collection |
| **Philosophy** | Bridge external tools to Obsidian | Opinionated end-to-end workflow | Role-specific skill library |
| **Planning system** | None | Full (pillars, quarters, weeks, daily) | Partial (meeting/project focused) |
| **People tracking** | None | Deep (person pages, auto-updates) | Basic |
| **Task management** | Via TaskNotes plugin only | Native (Work MCP, smart inference) | Light |
| **Meeting processing** | Granola sync only | Full pipeline (extract, link, follow-up) | Meeting prep focused |
| **Memory/recall** | Strong (temporal, topic, graph) | QMD semantic search | None |
| **Voice integration** | Wispr Flow skill | None | None |
| **Session persistence** | Claude session export + recall | Session learnings capture | None |
| **Career tracking** | None | Deep (evidence, coaching, resume) | None |
| **Distribution** | Claude Code marketplace plugin | Git clone + onboarding wizard | Git clone |
| **Platform** | macOS only (hardcoded paths) | Cross-platform | Cross-platform |
| **Maturity** | Early (15 commits, v1.0.0) | Mature (v1.11.0, extensive docs) | Moderate |

**Key differences:**

- **ArtemXTech is horizontal, Dex is vertical.** ArtemXTech connects many tools shallowly; Dex goes deep on a PM workflow. They are complementary more than competitive.
- **Memory is the standout.** The recall + sync-claude-sessions combo is the most developed "session memory" system in the Claude Code ecosystem. Dex has QMD search and session learnings, but not the graph visualization or temporal browsing.
- **No workflow opinions.** ArtemXTech provides tools, not workflows. There is no daily plan, weekly review, or strategic planning. Users bring their own structure.
- **macOS-locked.** Hardcoded paths to `~/Library/Application Support/` make this Mac-only without modification.

---

## What's Unique or Interesting

1. **Local-first data access pattern.** Reading directly from app caches and SQLite databases (Granola cache, Wispr Flow DB) instead of using APIs. Zero auth friction, works offline, real-time data. This is an underappreciated pattern.

2. **Session graph visualization.** The interactive HTML graph showing sessions as nodes connected by shared files is novel. It reveals work clusters and cross-session dependencies that are invisible in linear session lists.

3. **"One Thing" recall synthesis.** Instead of dumping everything from a recall, the skill forces a single-action summary. Good UX constraint.

4. **Claude Code marketplace distribution.** Using `.claude-plugin/marketplace.json` for install-via-command (`/plugin marketplace add`) is the cleanest distribution model seen so far. No manual file copying.

5. **NotebookLM citation resolution.** Converting numeric NotebookLM citations into Obsidian wikilinks with passage-level anchors is a creative knowledge graph builder.

6. **Tag schema for sessions.** YAML-defined tag categories (type: research/debugging/planning; project: lab/personal/client) enforce vocabulary consistency across exported sessions.

---

## Weaknesses

- **No cohesive system.** Six disconnected skills, no shared state or cross-skill workflows. A meeting synced from Granola does not automatically create tasks, update person pages, or link to projects.
- **macOS only.** Every script assumes macOS file paths. No Windows/Linux support.
- **Shallow documentation.** SKILL.md files are adequate but terse. No architecture overview connecting the skills into a coherent vision.
- **Early maturity.** 15 commits, v1.0.0. Limited error handling in scripts. No tests.
- **TaskNotes dependency.** Requiring a specific Obsidian plugin with an HTTP server running is fragile compared to direct file manipulation.
- **No person or project tracking.** Meetings sync but people and projects are not extracted or tracked.

---

## Key Takeaways

- **The local cache/DB reading pattern is worth stealing.** Granola's cache JSON and Wispr Flow's SQLite are accessed directly, bypassing API complexity entirely. Dex already does this for Granola but could apply the same pattern to other tools (Slack local cache, browser history DBs, etc.).

- **Session graph visualization fills a real gap.** Dex captures session learnings but has no way to visualize cross-session relationships. An interactive graph showing how sessions connect through shared files, people, and projects would be a powerful addition to `/recall` or `/week-review`.

- **The "One Thing" synthesis constraint is good UX.** When recalling context, forcing a single recommended action prevents cognitive overload. Dex's review skills could adopt this -- end every context dump with "The One Thing to do next."

- **Marketplace-based skill distribution is the future.** Dex distributes via git clone + onboarding wizard. A plugin marketplace model would make individual skills installable and updatable independently, lowering the barrier for community contributions.

- **Voice dictation integration is an untapped input channel.** Wispr Flow captures everything dictated across apps. This is ambient context that most PM systems ignore. Even without Wispr Flow specifically, the concept of mining voice-to-text data for commitments and ideas is worth exploring.
