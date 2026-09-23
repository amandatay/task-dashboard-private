# Task Dashboard - Project Spec

**Status:** Phase 6 Complete - Ready for Phase 7 (Pomodoro Timer)  
**User:** Amanda  
**Created:** 22 Sep 2026  
**Last Updated:** 23 Sep 2026  
**Current Build:** Phase 1-6 Complete (LIST, KANBAN, CALENDAR, TIMELINE, SUMMARY, SETTINGS, Time Tracking, Autosave)

---

## Project Overview

A local HTML-based task management + meeting planner dashboard designed for **minimal friction** and **mindfulness**. Built for production use in ML/banking environments. Corporate-friendly (no external CDN calls, single HTML file, no package managers).

**Distribution:** Single self-contained HTML file (double-click to run).

**Key Feature:** Hybrid task/meeting management with 6 complementary views optimized for different workflows.

---

## Core Design Principles

1. **Minimal friction for daily task + meeting capture** — Primary UX focus
2. **Mindfulness-first calendar view** — Focus on today + this week, not calendar grids
3. **Multiple views for different workflows** — LIST (overview), KANBAN (focus), CALENDAR (planning), TIMELINE (roadmap)
4. **Pixel aesthetic with sage green (#a6b08a) accents** — Intentionally lo-fi, retro feel
5. **Local-first persistence** — localStorage, no cloud sync, no external APIs
6. **No external dependencies** — Pure vanilla HTML/CSS/JavaScript

---

## Implemented Features (Phases 1-4)

### Phase 1: Core Task Management ✅

#### LIST View - Project-Organized Tasks
- **Project grouping** with collapsible sections (▼/◀ toggle)
- Tasks organized alphabetically by project
- Each project shows task count badge
- Drag-to-reorder tasks within projects
- **Quick Add Form** at top:
  - Title | Project dropdown | Date picker | ADD button
  - Date defaults to TODAY
  - Expandable "More" section for advanced options

#### Task Features
- **Status workflow:** TO DO → IN PROG → PENDING → DONE
- **Priority flag (🚩)** - independent of status
- **Recurrence support** (Daily/Weekly/Monthly):
  - Weekly: multiselect days (Mon-Sun)
  - Monthly: specific date OR ordinal weekday (1st Monday, Last Friday, etc.)
  - Displays as **【┘】** badge with hover tooltip (e.g., "【┘】Weekly (M,W,F)")
- **Expandable notes** - click "notes ▼" for full editor
- **Drag-to-reorder** via handle (⋮⋮)
- **Color-coded by status:**
  - TO DO: Sage gray (#a6b08a)
  - IN PROG: Lavender (#c9b8e4)
  - PENDING: Pink (#e4c5d8)
  - DONE: Muted (#5a6a4a), strikethrough
  - Flagged: Yellow overlay (#e8ddb8)

---

### Phase 2: KANBAN View ✅

#### Kanban Board - Status-Based Organization
- **4-column layout:** TO DO | IN PROG | PENDING | DONE
- **Project selector** at top - switch between projects
- Only shows tasks from selected project
- Task count per column
- **Drag-to-change-status** between columns:
  - Drag card left/right to update status
  - Auto-sets `startedAt` when moving to IN PROG
  - Auto-sets `completedAt` when moving to DONE
  - Real-time count updates

#### Task Modal - Detailed Editing
- Click any task card to open modal
- Edit: title, project, status, date, notes
- Flag/unflag priority
- Delete task
- Changes persist immediately

---

### Phase 3: CALENDAR View (Weekly Planner) ✅

#### Weekly Meeting Planner - Mindfulness First
**NOT a calendar grid** - focuses on today + this week only.

- **7-day view** (today + next 6 days)
- Each day shows:
  - Day header: NAME DATE (TODAY label if today)
  - Meetings sorted by time
  - Quick "+ ADD" button per day
- **Meeting display:**
  - **★ time – title** (★ shows if priority)
  - Duration (min) • Project (if set)
  - Color: Sage green by default, lavender if priority
- **Empty state:** "(No meetings)" text
- **Date picker modal** to add meetings to any date (past or future allowed)

#### Meeting Features
- **All fields optional:** title, date, time, duration, project, notes
- **Priority flag (★)** - highlights meeting as important
- **Past dates allowed** - can plan/log meetings retroactively
- **Modal supports edit/delete** from calendar
- **Color coding:**
  - Regular: Sage green border (#a6b08a)
  - Priority: Lavender border (#c9b8e4)

#### Sidebar Calendar Widget
- Shows **TODAY only** (not full week)
- Lists all meetings for today with times
- Shows **★** for priority meetings
- Click any meeting to jump to CALENDAR tab + open modal

---

### Phase 4: Timeline + Summary + Settings ✅

#### TIMELINE View - Project Pipeline
- **Project selector** - choose project to view
- **Horizontal swimlanes** by stage (customizable)
- Per-stage info: name, progress %, task count, task preview
- **Edit/delete/add stages** via modal buttons

#### SUMMARY View - Metrics + Review
- **Key metrics:** Total tasks, Completion %, In progress, Overdue
- **Status breakdown:** TO DO, IN PROG, PENDING, DONE (counts + %)
- **Project completion bars** - shows % complete per project
- **Task details table:**
  - Searchable/filterable by title, project, status
  - Shows dates, times, full notes
  - Designed for performance reviews

#### SETTINGS View
- **User name** text input
- **Projects** add/remove/edit inline
- **Statuses** add/remove/edit (default: TO DO, IN PROG, PENDING, DONE)
- **Theme toggle** (light/dark mode via CSS variables)
- **Data export/import** - JSON backups

---

### Phase 5: UI Polish & Bug Fixes ✅

#### Improvements
- Fixed date field in quick add (now editable, defaults to today)
- Fixed KANBAN project selector cosmetic feedback (active state updates on click)
- Improved visual feedback with subtle background highlights

---

### Phase 6: Time Tracking + Autosave ✅

#### Time Tracking
- **Automatic calculation** from statusHistory timestamps
- **No manual timer** — tracks duration in each status
- **Aggregated views:**
  - Time by Status (TO DO, IN PROG, PENDING, DONE)
  - Time by Project (total time per project)
- **Format:** Human-readable (0h, 45min, 2h 30min)
- **Display location:** SUMMARY tab only
- **Zero overhead** — uses existing statusHistory data

#### Autosave & Versioning
- **Automatic backups** every 10 minutes (configurable 5–60 min)
- **Version control:** Keep up to 10 versions, auto-prune older ones
- **Auto-restore:** Latest backup restored on page load
- **Manual restore:** Version history in SETTINGS with restore buttons
- **Data persistence:** All backups stored locally in browser localStorage
- **Configurable:** Autosave frequency adjustable in SETTINGS

---

## Data Models

### Task Object
```javascript
{
  id: string (UUID),
  title: string,
  project: string,
  stage: string (optional),
  status: string ("TO DO" | "IN PROG" | "PENDING" | "DONE"),
  dueDate: string (ISO format: YYYY-MM-DD),
  notes: string (unrestricted, optional),
  priority: boolean (high-priority flag),
  createdAt: ISO timestamp,
  startedAt: ISO timestamp (set when status → IN PROG),
  completedAt: ISO timestamp (set when status → DONE),
  statusHistory: Array[{ status, changedAt }],
  recurrence: {
    type: "never" | "daily" | "weekly" | "monthly",
    daysOfWeek: ["mon", "tue", ...] (if weekly),
    monthlyType: "date" | "weekday" (if monthly),
    dateOfMonth: number (if monthly + date),
    ordinal: "1st" | "2nd" | ... (if monthly + weekday),
    weekday: "monday" | "tuesday" | ... (if monthly + weekday)
  },
  order: number (drag-reorder index)
}
```

### Meeting Object
```javascript
{
  id: string (UUID),
  title: string,
  date: string (ISO format: YYYY-MM-DD),
  time: string (HH:MM format),
  duration: number (minutes),
  project: string (optional, null allowed),
  notes: string (optional),
  priority: boolean (★ flag for important meetings),
  recurrence: { type: "never" } (reserved for future)
}
```

### Project Object
```javascript
{
  id: string (UUID),
  name: string,
  stages: Array[
    { id, name, order }
  ]
}
```

### Settings Object
```javascript
{
  userName: string,
  projects: Array[{ id, name }],
  statuses: Array[{ id, name }],
  theme: "light" | "dark"
}
```

---

## Storage & Persistence

- **Storage engine:** localStorage (JSON serialization)
- **Data stored:** tasks, meetings, projects, settings, UI state (expanded projects, theme)
- **Backup strategy:** Export to JSON file (manual, user-initiated)
- **No cloud sync** — intentional, offline-first design
- **No external APIs** — all data local

---

## UI/UX Details

### Color Scheme
**Dark Mode (Default):**
- **Primary accent:** Sage green (#a6b08a)
- **Background:** Dark (#2a2a2a)
- **Cards:** Medium gray (#3d3d3d)
- **Text primary:** Sage green (#a6b08a)
- **Text secondary:** Muted (#7a8a6a)
- **Borders:** Dark gray (#5a6a4a)

**Status Colors (Tasks):**
- **TO DO:** Gray (#6a7a5a)
- **IN PROG:** Lavender (#c9b8e4)
- **PENDING:** Pink (#e4c5d8)
- **DONE:** Muted gray (#5a6a4a), strikethrough
- **FLAGGED:** Yellow (#e8ddb8) overlay

**Meeting Colors:**
- **Default:** Sage green (#a6b08a)
- **Priority:** Lavender (#c9b8e4)

**Light Mode:**
- Inverted backgrounds (light) + dark text
- Sage green accent maintained
- Better for daytime use

### Typography
- **Font:** Monospace (Courier New) for pixel aesthetic
- **Sizes:** 7px-14px (min-max)
- **Weights:** 400 (regular) + 700 (bold) only
- **Case:** ALL CAPS for headers, mixed for content

### Interactions
- **Drag-to-reorder:** Grab cursor, visual feedback
- **Hover states:** Opacity shifts, border highlights
- **Click targets:** Minimum 24px×24px
- **Modals:** Overlay with close button + escape key support
- **Keyboard:** cmd/ctrl+n (new task), cmd/ctrl+d (mark done), cmd/ctrl+w (working on)

---

## Navigation & Sidebar

### Main Navigation (Tabs)
1. **LIST** - Project-grouped task view
2. **KANBAN** - Status-column board by project
3. **TIMELINE** - Project pipeline stages
4. **CALENDAR** - Weekly meeting planner
5. **SUMMARY** - Metrics + review table
6. **SETTINGS** - Configuration

### Sidebar
- **VIEWS section** - 6 tab buttons
- **Task stats** - User name, total/active/done counts
- **Calendar widget** - Today's meetings (★ for priority)

### Header
- View title + emoji (🕐 LIST, 🕐 KANBAN, 📋 TIMELINE, 📅 CALENDAR, 📊 SUMMARY, ⚙ SETTINGS)
- Current date/time (updates every second)
- Theme toggle button (🌙 dark, ☀️ light)

---

## Architecture Notes

### Single-File Design
- **One HTML file** - `dashboard.html` (~56KB)
- **All CSS inline** - No external stylesheets
- **All JS vanilla** - No frameworks, no build step
- **No CDN dependencies** - Corporate proxy friendly
- **Self-contained** - Copy file, double-click, works

### State Management
- **App object** holds all state (tasks, meetings, projects, settings)
- **localStorage** syncs on every save
- **No cloud** - intentional choice
- **Rendering functions** tied to state changes

### Performance
- Tasks: O(n) render (n = number of tasks)
- Meetings: O(n) render (n = number of meetings)
- Storage read: O(1) per view load
- No external requests - instant load

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `cmd/ctrl + n` | Focus new task input |
| `cmd/ctrl + w` | Toggle "working on" first active task |
| `cmd/ctrl + d` | Mark first active task as done |

---

## Not Included (Out of Scope)

- ❌ Pomodoro timer (planned for Phase 7)
- ❌ Audit trail (full change history)
- ❌ Markdown in notes
- ❌ Tags/labels
- ❌ Recurring meetings (planned for future)
- ❌ Task dependencies
- ❌ Comments/collaboration
- ❌ Attachments
- ❌ Cloud sync
- ❌ Mobile app
- ❌ Calendar grid view (weekly list is better for mindfulness)

## Future Enhancements (Backlog)

- **Streams as tags** — Allow tasks to have multiple streams (currently 1 per task)
- **Drag-to-bulk-assign** — Drag multiple selected tasks onto stream card to assign
- **Stream filtering** — Filter tasks by stream across all views
- **Stream colors** — Color-code streams for quick visual identification
- **Quick stream add** — Add new stream from task modal without leaving view

---

## Phase 7: Pomodoro Timer (Upcoming)

### What Phase 7 Covers
1. **Pomodoro timer** - Integrated timer in TASKS view
2. **Timer controls** - Start, pause, reset, skip break
3. **Work/Break cycles** - Configurable duration (default: 25min work, 5min break)
4. **Sound/notification** - Alert when timer completes
5. **Session tracking** - Count completed pomodoros per task
6. **Visual feedback** - Timer display in task card + header

### Success Criteria Phase 7
- ✅ Start/stop/pause timer without losing state
- ✅ Automatic break after completing work session
- ✅ Auto-switch to next task or stay on current
- ✅ Timer persists in browser (survives page refresh)
- ✅ Notification when session complete
- ✅ Accessible from TASKS tab (LIST or KANBAN view)

---

## Build Stack

- **HTML5** (semantic markup)
- **CSS3** (inline + global, CSS variables for theming)
- **JavaScript ES6+** (vanilla, no transpile needed)
- **localStorage API** (data persistence)
- **No external libraries**

---

## Success Criteria (Current)

✅ Minimal friction for task capture (< 10 seconds)  
✅ Minimal friction for meeting capture (< 15 seconds)  
✅ Visual state feedback (color-coded, emoji indicators)  
✅ No external dependencies (completely offline)  
✅ Local persistence (survives browser restart)  
✅ Cross-view consistency (LIST ↔ KANBAN ↔ SUMMARY)  
✅ Keyboard shortcuts for power users  
✅ Pixel aesthetic (retro, intentional)  
✅ Light/dark theme support  
✅ Mindfulness-first design (focus on today + this week)

---

## Implementation Timeline

| Phase | Feature | Status |
|-------|---------|--------|
| 1 | LIST + Task CRUD + Recurrence | ✅ Complete |
| 2 | KANBAN + Drag-to-status | ✅ Complete |
| 3 | CALENDAR (weekly) + Meetings | ✅ Complete |
| 4 | TIMELINE + SUMMARY + SETTINGS | ✅ Complete |
| 5 | UI Polish + Bug Fixes | ✅ Complete |
| 6 | Time Tracking + Autosave/Versioning | ✅ Complete |
| 7 | Pomodoro Timer (integrated) | ⏳ Upcoming |

---

**Phase 6 complete. Ready for Phase 7 (Pomodoro Timer)!**
