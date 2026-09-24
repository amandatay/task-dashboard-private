# Task Dashboard

A minimal, offline-first task management + meeting planner for mindful planning. Single HTML file, no dependencies, works anywhere.

**[📥 Download & Run](dashboard.html)** — Just double-click to open in your browser. All data stays local.

---

## Features

### 🕐 LIST View - Project-Organized Tasks
- Tasks grouped by project (collapsible sections)
- Quick add form: title | project | date | ADD
- Drag-to-reorder within projects
- **Recurrence support:** Daily, Weekly (multiselect), Monthly (date or ordinal)
- Recurrence badges show pattern at a glance (【┘】)
- Expandable notes per task
- Status workflow: TO DO → IN PROG → PENDING → DONE
- Priority flag (🚩) independent of status

### 🕐 KANBAN View - Status-Based Focus
- 4-column board: TO DO | IN PROG | PENDING | DONE
- **Project selector** - view one project at a time
- Drag cards between columns to change status
- **Nested subtasks** - subtasks indented beneath parent tasks (non-draggable)
- Parent tasks draggable between columns, subtasks updated via modal
- Task modal for detailed editing (parent or subtask)
- Auto-timestamps for started/completed

### 📅 CALENDAR View - Weekly Planning
- **Today + next 6 days** (not a grid calendar)
- See all meetings for each day sorted by time
- Add meetings on any date (past or future allowed)
- Priority flag (★) for important meetings
- **Sidebar widget** shows today's meetings only
- Color-coded: Sage green (normal), Lavender (priority)

### 📋 TIMELINE View - Project Pipeline
- Customizable project stages
- Progress bars per stage
- Task counts and previews
- Edit/delete/add stages inline

### 📊 SUMMARY View - Review & Metrics
- Key metrics (total, completion %, in progress, overdue)
- Status breakdown with percentages
- Project completion bars
- Task details table (searchable, filterable)
- Designed for performance reviews

### ⚙ SETTINGS - Configuration
- User name
- Projects (add/remove/edit)
- Statuses (add/remove/edit custom statuses)
- Light/dark theme toggle
- Export/import data as JSON

---

## Quick Start

1. **Download** `dashboard.html`
2. **Double-click** to open in browser
3. **Add a project** in SETTINGS (or use defaults)
4. **Create your first task** in LIST view
5. **Switch views** to find your workflow

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `cmd/ctrl + n` | Focus new task input |
| `cmd/ctrl + w` | Toggle "working on" (mark IN PROG) |
| `cmd/ctrl + d` | Mark first active task as done |

---

## Design Philosophy

- **Minimal friction** — Create tasks in < 10 seconds
- **Mindfulness first** — Focus on today + this week, not overwhelm
- **Offline** — No cloud, no tracking, just data
- **Local storage** — Everything persists in your browser
- **Pixel aesthetic** — Intentionally retro, corporate-friendly
- **No dependencies** — Pure HTML/CSS/JavaScript

---

## Color System

**Tasks:**
- **TO DO** - Sage gray
- **IN PROG** - Lavender (working)
- **PENDING** - Pink (blocked/waiting)
- **DONE** - Muted gray with strikethrough
- **FLAGGED** - Yellow overlay (priority)

**Meetings:**
- **Default** - Sage green
- **Priority** - Lavender (★)

**Theme:** Dark mode (default) + Light mode toggle

---

## Data Persistence

- **Storage:** Browser localStorage (no server needed)
- **Backup:** Export to JSON (manual, user-initiated)
- **Privacy:** 100% local, nothing leaves your machine
- **Migration:** Export data, update HTML, import data

---

## Recurrence Patterns

### Daily
Repeats every day

### Weekly
- Multiselect days (Mon-Sun)
- Shows badge: 【┘】Weekly (M,W,F)

### Monthly
**Option A: Specific date**
- 1st, 5th, 10th, 15th, 20th, 25th, 28th, or last day
- Shows badge: 【┘】Monthly (15)

**Option B: Ordinal weekday**
- 1st Monday, 2nd Tuesday, 3rd Wednesday... Last Friday, etc.
- Shows badge: 【┘】Monthly (Last Fri)

---

## Subtasks

- **Create subtasks** in LIST view using the "+ subtask" button on parent tasks
- **Subtasks are full tasks** - same properties as parent tasks (status, date, notes, priority, recurrence)
- **Parent-child relationship** - subtasks linked via parentTaskId field
- **View in LIST** - subtasks shown indented beneath parent tasks (collapsible)
- **View in KANBAN** - subtasks nested beneath parents in status columns
- **Manage in modal** - click any parent task to see/add/edit subtasks via checkboxes
- **Independent status** - subtasks have separate statuses from parent (can be DONE while parent is IN PROG)
- **Orphan subtasks** - subtasks without parents display as regular tasks

---

### ⏱ Time Tracking (Phase 6)
- Automatic time calculation from task status history
- No manual timer — just track when tasks change status
- Time aggregated by status (TO DO, IN PROG, PENDING, DONE)
- Time aggregated by project
- Visible only in SUMMARY tab (HH:MM format)

### 💾 Autosave & Versioning (Phase 6)
- **Autosave:** Automatic backups every 10 minutes (configurable 5–60 min)
- **Version control:** Keep up to 10 backup versions
- **Auto-restore:** Latest backup restored on page load
- **Manual restore:** Select any version from history in SETTINGS
- **No data loss:** All backups stored locally in browser

### ⏲️ Pomodoro Timer (Phase 7)
- **Work/break cycles** - Configurable durations (default 25min work, 5min break)
- **Session tracking** - Completed pomodoros counted per task (🍅 display)
- **Auto-transition** - Work → break → work cycle with notifications
- **Skip Break button** - Jump directly to next work session
- **Sound alerts** - Beep notification when session completes
- **Visual feedback** - Pomodoro count shown on all task cards
- **TODAY tab** - Focus view for tagged tasks with timer integration
- **Configurable** - Work/break durations editable in SETTINGS

---

## File Size & Performance

- **Single file:** `dashboard.html` (~194 KB)
- **Load time:** Instant (no external requests)
- **Browser support:** Chrome, Firefox, Safari, Edge
- **Rendering:** O(n) where n = number of tasks/meetings
- **Storage:** Up to ~10MB in localStorage (browser limit)

---

## Use Cases

### Daily Planning
1. Open LIST view
2. See today's tasks grouped by project
3. Drag to reorder priority
4. Check CALENDAR for meetings

### Weekly Review
1. Open SUMMARY for key metrics
2. Check TIMELINE for project roadmints
3. Review completed tasks with notes in table
4. Prepare for performance review

### Project Management
1. Switch KANBAN project selector
2. View 4-column status board
3. Drag cards to update status
4. Use TIMELINE for milestone tracking

### Meeting Scheduler
1. Click CALENDAR tab
2. Click "+ ADD" on any day
3. Set title, time, duration, project
4. Mark priority (★) if important
5. See today's meetings in sidebar widget

---

## Settings & Customization

### Projects
- Pre-loaded with PROJECT_1, PROJECT_2, PROJECT_3
- Add/remove/rename anytime in SETTINGS
- Used to organize tasks and meetings

### Statuses
- Defaults: TO DO, IN PROG, PENDING, DONE
- Customize in SETTINGS (add/remove statuses)
- Tasks respect your custom statuses

### Theme
- Dark mode (default, recommended for long use)
- Light mode (better for daytime/accessibility)
- Toggle via button in header

### Backup & Versioning
- Autosave frequency (5–60 minutes, default 10)
- Version history with manual restore buttons
- Auto-restore latest backup on page load
- Up to 10 backup versions kept locally

---

## Troubleshooting

**Tasks disappeared after changing status?**
- This was a bug in early versions. Update to latest `dashboard.html`

**Meetings not showing in sidebar?**
- Sidebar only shows TODAY's meetings by design
- Check CALENDAR tab for full week view

**How do I back up my data?**
- SETTINGS → Export Data → save JSON file
- Keep backup before updating dashboard.html

**Can I use this on my phone?**
- Yes, but UI designed for desktop
- Mobile version possible in future

---

## Tech Stack

- **HTML5** - Semantic markup
- **CSS3** - Inline styles + CSS variables (theming)
- **Vanilla JavaScript** - No frameworks, no build step
- **localStorage** - Data persistence
- **Zero dependencies** - Corporate proxy friendly

---

## License & Privacy

- Single file, no tracking, no external calls
- All data stored locally in your browser
- Safe to use in corporate/regulated environments
- No logs, no analytics, no telemetry

---

## What's Not Included

- ❌ Markdown in notes
- ❌ Tags/labels (planned enhancement: streams as tags)
- ❌ Recurring meetings (coming soon)
- ❌ Task dependencies
- ❌ Collaboration/comments
- ❌ Cloud sync
- ❌ Mobile app (responsive design planned)

---

## Feedback & Suggestions

Found a bug? Want a feature?

- Check `CLAUDE.md` for full technical spec
- Review code in `dashboard.html` (all-in-one file)
- Submit issues via GitHub if applicable

---

**Built for mindful planning. No fluff, just tasks and meetings.**

**Made with ❤️ for offline-first productivity.**
