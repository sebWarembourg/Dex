# Upstream Dex Analysis

**Repo:** [davekilleen/Dex](https://github.com/davekilleen/Dex)
**Analyzed:** 2026-04-02
**Our local HEAD:** `b7de6a9` — "fix(granola): extract notes from meetings recorded on other machines" (2026-02-19)

---

## Repository Snapshot

| Metric | Value |
|--------|-------|
| Stars | 282 |
| Forks | 89 |
| Open issues | 9 |
| Default branch | main |
| License | **PolyForm Noncommercial 1.0.0** |
| Created | 2026-01-25 |
| Last push | 2026-03-23 |
| Formal GitHub releases | None (changelog-only versioning) |

**Description:** "Your AI Chief of Staff — a personal operating system starter kit that adapts to your role. No coding required."

**Topics:** ai-assistant, anthropic, claude-code, cursor, mcp, personal-knowledge-management, pkm, productivity

---

## Development Velocity

### Commit Cadence

- **Jan 25 - Feb 19 (~25 days):** Rapid initial build. v1.0 through v1.15. Multiple commits per day during this burst. The project went from 0 to a full-featured system in under a month.
- **Feb 19 - Mar 1 (~10 days):** Relative quiet. Consolidation period.
- **Mar 1 - Mar 23 (~22 days):** Second wave of feature work. v1.16 through v1.19. Roughly 15 commits in this window, averaging one every 1.5 days.
- **Mar 23 - Apr 2 (10 days):** No new commits. Current lull.

**Pattern:** Burst development with quiet intervals. Single-developer project (davekilleen) with one external contributor (ajbermudezh22, single fix). Not a steady cadence — more "inspiration-driven."

### Version Velocity

19 versions in ~2 months (Jan 25 - Mar 23). That's roughly one release every 3 days. However, many of these are minor (changelog rewrites, doc tweaks). Meaningful feature releases are closer to one every 5-7 days.

---

## Key Recent Changes (v1.16 - v1.19)

### v1.19.0 (2026-03-23) — Semantic Search Expansion
- QMD semantic search expanded from 6 to 14 collections (covers full vault)
- PRDs, implementation plans, session learnings, and resource docs now searchable by meaning

### v1.18.x (2026-03-05 to 2026-03-21) — Infrastructure Hardening
- **v1.18.3:** Python venv isolation (fixes PEP 668 errors), Atlassian MCP pointed to official remote server
- **v1.18.2:** Cleaned up dead references to `granola-auth.cjs` and sync-v2 script
- **v1.18.1:** Meeting sync switched from Granola MCP server to direct API. Eliminates silent failures, mobile recordings work automatically

### v1.18.0 (2026-03-02) — Skill Model Routing
- Skills now carry `model_hint` and `model_routing` metadata
- Update conflict resolver preserves custom skill instructions

### v1.17.0 (2026-03-01) — Mobile Granola Sync
- Mobile recordings sync every 30 minutes via Granola's official MCP server
- One-time browser sign-in, automatic fallback to local data

### v1.16.0 (2026-03-01) — Scrapling Web Scraper
- Scrapling replaces previous web scraping approach
- Handles Cloudflare-protected sites, runs locally, no API keys

---

## What We're Missing (Commits After Our HEAD)

Our local repo is at `b7de6a9` (Feb 19, 2026). The upstream `main` branch has advanced significantly. Based on the commit log and changelog, here is what exists upstream that we do not have:

### Features We're Behind On

1. **Scrapling web scraper** (v1.16) — local, no-API-key web scraping
2. **Mobile Granola sync** (v1.17) — 30-min auto-sync of mobile recordings
3. **Skill model routing metadata** (v1.18.0) — skills specify preferred models
4. **Direct Granola API** (v1.18.1) — replaces MCP server approach for meetings
5. **Semantic search across full vault** (v1.19) — 14 collections vs 6
6. **Ritual Intelligence v1** — new feature (commit `db9382a`, Mar 10)
7. **tau-mirror web UI** — integration extension (commit `d273ce9`, Mar 11)
8. **Industry truths skill** — time-horizoned strategic assumptions (commit `5bd1764`, Mar 21)
9. **Automatic meeting processing on by default** for new users (commit `96dc0ce`, Mar 12)
10. **CI hardening** — imported from remote lanes (commit `fd8c024`, Mar 10)
11. **Commercial model added to CLAUDE.md** (commit `f21e3b8`, Mar 13)
12. **Analytics switched to opt-out default** for new installs (commit `26b376c`, Mar 10)

### Bug Fixes We're Behind On

1. **Python venv isolation** (v1.18.3) — fixes PEP 668 system package errors
2. **Atlassian MCP config** pointed to official remote server
3. **Granola sync repair** for transcript-only meetings and new cache format
4. **Removed dead references** to `granola-auth.cjs`
5. **Install script fixes** — no longer references non-existent files

### Upstream Commits After Our HEAD (Chronological)

| SHA | Date | Message |
|-----|------|---------|
| `5be68a6` | Mar 6 | feat: Phase 3 — JSDoc, sed portability fixes, environment validation |
| `26b376c` | Mar 10 | Switch analytics to opt-out default for new installs |
| `fd8c024` | Mar 10 | feat: import CI hardening from remote (lane a/b/c) |
| `48b30fb` | Mar 10 | feat: add missing vault template stub READMEs |
| `db9382a` | Mar 10 | feat: ship Ritual Intelligence v1 |
| `d273ce9` | Mar 11 | feat: add tau-mirror web UI integration extension |
| `8fdb433` | Mar 12 | fix: remove references to deleted granola-auth.cjs and sync-v2 script |
| `0a6ae19` | Mar 12 | fix: repair Granola sync for transcript-only meetings and new cache format |
| `96dc0ce` | Mar 12 | feat: automatic meeting processing on by default for new users |
| `f21e3b8` | Mar 13 | docs: add commercial model to CLAUDE.md |
| `5bd1764` | Mar 21 | feat: add industry-truths skill for time-horizoned strategic assumptions |
| `3a26847` | Mar 21 | docs: add Strategic Context (Industry Truths) section to CLAUDE.md |
| `8845a2c` | Mar 21 | fix: use venv for Python deps, fix Atlassian MCP config |
| `72f08bc` | Mar 22 | Semantic search expanded to cover entire vault (14 collections) |
| `a3422e3` | Mar 23 | Clean up legacy path references and add QMD to MCP example config |

**Total: 15 commits ahead of our local copy.**

---

## Architecture Direction

### Where the Project Is Heading

1. **Local-first AI stack:** QMD for semantic search, Scrapling for web scraping, ScreenPipe for ambient capture. All run locally. The pattern is clear — minimize cloud dependencies and API keys.

2. **Model flexibility:** Skill-level model routing (`model_hint`, `model_routing`). Budget models for simple tasks, capable models for complex ones. Supports OpenRouter (Kimi K2.5, DeepSeek) and offline local models.

3. **Integration breadth:** 8+ integrations (Slack, Google Workspace, Teams, Todoist, Things 3, Trello, Zoom, Jira/Confluence). The architecture is moving toward "hub" status — Dex as the central nervous system.

4. **Autonomous behaviors:** Background meeting sync, automatic person page updates, self-healing People Directory, update notifications. The system is becoming more agent-like with less manual triggering.

5. **Commercial model emerging:** A "commercial model" was added to CLAUDE.md (Mar 13) and analytics switched to opt-out by default (Mar 10). The project appears to be exploring monetization.

6. **Ritual Intelligence:** New concept (Mar 10) — likely pattern recognition around user habits and routines. Aligns with the "identity model" and "pattern detection" features from v1.14.

---

## License Status

**Confirmed: PolyForm Noncommercial License 1.0.0**

This has NOT changed back to MIT. The license was switched from MIT to PolyForm Noncommercial around Feb 19, 2026 (commit `0203983` in our local history). It remains PolyForm Noncommercial upstream.

### Key Implications

- **No commercial use permitted.** Personal, research, hobby, and educational/charitable use only.
- **No sublicensing.** Cannot transfer licenses.
- **Distribution allowed** but must pass along license terms.
- **Patent defense clause** — claiming patent infringement terminates your license.
- **32-day cure period** for violations.

This is a significant restriction. Any fork or derivative used commercially would violate the license. The combination of the commercial model appearing in CLAUDE.md and the noncommercial license suggests the author is reserving commercial rights while keeping the project source-available for personal use.

---

## Risk Assessment

| Risk | Level | Notes |
|------|-------|-------|
| Development stalling | Medium | 10-day gap since last commit, but project has had similar pauses before |
| License incompatibility | High | PolyForm Noncommercial blocks any commercial derivative work |
| Breaking changes on update | Medium | 15 commits ahead; Granola sync and analytics defaults changed |
| Single maintainer | High | Essentially one developer (davekilleen), one external fix ever |
| Scope creep | Medium | Moving fast in many directions (scraping, rituals, web UI, commercial model) |

---

## Recommendations

1. **Do not blindly pull upstream.** 15 commits include structural changes (analytics opt-out default, Granola API switch, venv isolation). Review each before merging.
2. **Cherry-pick bug fixes first.** The Python venv fix (v1.18.3) and Granola sync repairs are high-value, low-risk.
3. **Evaluate the license carefully** before any commercial or organizational use.
4. **Monitor the commercial model direction.** The addition of a commercial model to CLAUDE.md may signal upcoming license changes or paid tiers.
5. **Semantic search expansion** (v1.19) is worth adopting if QMD is already set up locally.
