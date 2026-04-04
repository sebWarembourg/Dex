# Ritual Command Bar

**Status:** ✅ Implemented (Pending Testing)

## What It Does

Contextual command bar below the chat input that surfaces daily/weekly/quarterly rituals and quick actions.

## UI States

### Collapsed (Default - 1 line)

```
🔴 Daily Review (yesterday)  🟡 Daily Plan  📋 Quick actions  [↓ Expand]
```

**Visual indicators:**
- 🔴 Overdue (yesterday's review not done)
- 🟡 Pending (should do today)
- ✅ Completed (already done today)

### Expanded (Interactive Overlay)

Press `Ctrl+.` or run `/rituals` to open full SelectList:

```
┌─ Your Rituals & Actions ────────────────┐
│ 🔴 Daily Review (yesterday)             │
│ 🟡 Daily Plan                            │
│ ─── Done Today ───                       │
│ ✅ Weekly Plan                           │
│ ─── Quick Actions ───                    │
│ 📋 Triage Inbox                          │
│ 📅 Meeting Prep                          │
│ 🎯 Project Health                        │
└──────────────────────────────────────────┘
```

## Smart Timing

**Daily rituals:**
- Daily Plan: Shows until done (checks for file)
- Daily Review: Shows from evening until done
  - If yesterday's review missing → shows as 🔴 overdue

**Weekly rituals:**
- Weekly Plan: Shows Monday AM or Sunday PM
- Weekly Review: Shows Friday afternoon/evening

**Quarterly rituals:**
- Quarterly Plan: Shows first week of new quarter OR last week of previous quarter
- Checks for actual goals in Quarter_Goals.md

## File Detection

Ritual status is determined by checking file existence:

| Ritual | File Path | Completion Check |
|--------|-----------|------------------|
| Daily Plan | `00-Inbox/Daily_Prep/Daily_Prep_YYYY-MM-DD.md` | File exists for today |
| Daily Review | `00-Inbox/Daily_Reviews/Daily_Review_YYYY-MM-DD.md` | File exists for yesterday |
| Weekly Plan | `02-Week_Priorities/Week_Priorities.md` | Contains current week section |
| Weekly Review | Today's daily review | Contains "Week Review" section |
| Quarterly Plan | `01-Quarter_Goals/Quarter_Goals.md` | Contains current quarter section |

## Quick Actions (Always Available)

- 📋 Triage Inbox → `/triage`
- 📅 Meeting Prep → `/meeting-prep`
- 🎯 Project Health → `/project-health`

## Commands

- `/rituals` - Open expanded view
- `Ctrl+.` - Keyboard shortcut to expand
- `/refresh-rituals` - Manually refresh bar after completing rituals

## Auto-Refresh

The bar automatically refreshes:
- On session start
- After each agent turn (in case rituals completed during conversation)

## Integration with Existing Dashboard

The ritual command bar sits **below** the existing dashboard widget:

```
┌────────────────────────────────────────┐
│ Conversation Area                      │
├────────────────────────────────────────┤
│ Type your message...              Send │ ← Chat input
├────────────────────────────────────────┤
│ ┌───────────────────────────────────┐  │
│ │ Week Priorities    Top Tasks      │  │ ← Dashboard (existing)
│ │ ───────────────    ────────       │  │
│ │ □ Priority 1       🔥 Task 1      │  │
│ │ □ Priority 2       ⚡ Task 2      │  │
│ │ □ Priority 3       ○ Task 3       │  │
│ │                                   │  │
│ │ 📅 Focus Time: 4h 30m             │  │
│ └───────────────────────────────────┘  │
├────────────────────────────────────────┤
│ 🔴 Review  🟡 Plan  [↓ More]          │ ← Ritual bar (NEW)
└────────────────────────────────────────┘
```

## Implementation Details

**File:** `pi-extensions/sw_os/ritual-command-bar.ts`

**Key functions:**
- `getRitualStates()` - Detects ritual completion state via file checks
- `renderCollapsedWidget()` - Renders 1-line collapsed view
- `showExpandedRituals()` - Opens interactive SelectList overlay
- `registerRitualCommandBar()` - Wires up event handlers and commands

**Registered in:** `pi-extensions/sw_os/index.ts` (line ~1498)

## Testing Checklist

- [ ] Collapsed bar appears below dashboard on session start
- [ ] Daily plan shows 🟡 pending until done, then ✅ completed
- [ ] Daily review shows 🔴 overdue if yesterday's missing
- [ ] `Ctrl+.` opens expanded overlay
- [ ] `/rituals` command works
- [ ] Selecting a ritual executes the command
- [ ] Bar auto-refreshes after completing a ritual
- [ ] Weekly/quarterly rituals show at correct times
- [ ] Quick actions always visible and executable
- [ ] Esc cancels expanded view
- [ ] Type-to-filter works in expanded view

## Future Enhancements

1. **Custom Commands** - Allow users to add own commands to quick actions
2. **Drag-to-Reorder** - Reorder quick actions via UI
3. **Smart Learning** - Track when user actually does rituals, adjust timing
4. **Progress Indicators** - Show partial completion (e.g., "3/5 priorities done")
5. **Notifications** - Optional gentle nudges at ideal times

## Comparison to Original Design

**Matches original vision:**
- ✅ Collapsed by default (1 line)
- ✅ Color-coded status (🔴 overdue, 🟡 pending, ✅ done)
- ✅ Temporal awareness (weekly rituals only on Monday/Friday)
- ✅ One-click command execution
- ✅ Minimal when done, prominent when needed

**Improvements over original:**
- Interactive overlay instead of inline expansion (cleaner UX)
- Keyboard shortcut for power users (Ctrl+.)
- Type-to-filter in expanded view (fast navigation)
- Auto-refresh after agent turns (no manual refresh needed)
- Smart overdue detection (yesterday's review highlighted)
