# Task Dashboard

A local HTML-based task management and project timeline dashboard designed for minimal friction task creation and tracking. Built for production ML/banking environments with a pixel aesthetic and no external dependencies.

**[📥 Download & Run](dashboard.html)** — Just double-click to open in your browser. All data stored locally in your browser.

## Features

### 🕐 TASKS Tab
- **Quick Add Form** — Minimal friction: title + project + date + ADD
- **Task Cards** — Drag to reorder, status dropdown, three action buttons
- **State Workflow** — TO DO → Start working → IN PROG → Done → DONE
- **Flag Priority** — Toggle high-priority flag (yellow) independent of status
- **Expandable Notes** — Unrestricted long-form text per task
- **Done Section** — Collapsed by default, shows completed task count

### 📋 TIMELINE Tab
- **Project Selector** — Switch between projects (PROJECT_1, PROJECT_2, PROJECT_3)
- **Pipeline View** — Horizontal swimlanes with stages (customize in Settings)
- **Stage Progress** — Per-stage completion %, task count, task preview
- **Stage Management** — Edit/delete stages, add new stages

### 📊 SUMMARY Tab
- **Key Metrics** — Total tasks, completion %, in progress, overdue
- **Status Breakdown** — Task counts & percentages by status (TO DO, IN PROG, PENDING, DONE)
- **Project Completion** — Progress bars showing % complete per project
- **Task Details Table** — Filterable/searchable task reference for performance reviews
  - Search by task title
  - Filter by project & status
  - Shows full notes (no character limit)
  - Timestamps: created, started, completed
  - Sticky header for easy scrolling

### ⚙ SETTINGS Tab
- **User Name** — Customize who is using the dashboard
- **Projects** — Add/remove projects
- **Statuses** — Add/remove custom statuses (defaults: TO DO, IN PROG, PENDING, DONE)

## Color Scheme

All colors follow a **dark pixel aesthetic** with sage green accents:

| State | Outline | Text | Use Case |
|-------|---------|------|----------|
| TO DO | #6a7a5a | #a6b08a | Default task state |
| IN PROG | #c9b8e4 | #d9cce9 | Active work (lavender) |
| PENDING | #e4c5d8 | #e8c5d8 | Blocked/waiting (pink) |
| DONE | #5a6a4a | #5a6a4a | Completed (muted gray) |
| FLAGGED | #e8ddb8 | #f0e8d0 | High priority (yellow) — overlays any state |

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `cmd/ctrl + n` | Focus new task input |
| `cmd/ctrl + w` | Toggle "working on" for first active task |
| `cmd/ctrl + d` | Mark first active task as done |

## Data Persistence

All data is stored in **localStorage** — your browser's local storage. No cloud sync, no external servers.

- **Survives**: Browser refresh, system restart
- **Does not survive**: Browser data clear, switching browsers/devices
- **Backup**: Manually export tasks to JSON file if needed

## Data Model

### Task Object
```javascript
{
  id: string,                   // UUID
  title: string,                // Task name
  project: string,              // Project name
  stage: string,                // Project stage (optional)
  status: string,               // TO DO, IN PROG, PENDING, DONE
  dueDate: string,              // DD MMM YYYY format
  notes: string,                // Unrestricted long-form text
  priority: boolean,            // Flagged as high priority
  createdAt: ISO timestamp,     // Auto-set on creation
  startedAt: ISO timestamp,     // Set when status → IN PROG
  completedAt: ISO timestamp,   // Set when status → DONE
  statusHistory: Array,         // All status changes with timestamps
  order: number                 // For drag-reorder
}
```

### Project Object
```javascript
{
  id: string,      // UUID
  name: string,    // Project name
  stages: Array    // Pipeline stages
}
```

### Settings Object
```javascript
{
  userName: string,
  projects: Array,   // { id, name }
  statuses: Array    // { id, name }
}
```

## Tech Stack

- **HTML5** — Single file, semantic markup
- **CSS** — Inline styles + minimal global (no external stylesheets)
- **Vanilla JavaScript** — No frameworks, no libraries
- **localStorage** — Data persistence (JSON serialization)
- **Font** — Monospace (Courier New) for pixel aesthetic

## Browser Support

Works in all modern browsers:
- ✅ Chrome/Chromium
- ✅ Firefox
- ✅ Safari
- ✅ Edge

Requires:
- JavaScript enabled
- localStorage enabled
- ~56KB of disk space for the HTML file

## Usage

1. **Download** → Save `dashboard.html` anywhere
2. **Open** → Double-click the file (or drag into browser)
3. **Start tracking** → Add a project, then add tasks
4. **Review** → Check the Summary tab for performance review context

## Default Projects

Pre-loaded with:
- **PROJECT_1**
- **PROJECT_2**
- **PROJECT_3**

Customize project names anytime in the Settings tab.

## Size & Performance

- **File size**: 56 KB (single HTML file)
- **No external requests** — Fully offline capable
- **Fast load time** — Instant on local machine
- **Lightweight** — Works on older machines/browsers

## Future Enhancements (Out of Scope)

- Cloud sync
- Mobile app
- Time tracking
- Recurring tasks
- Dependencies between tasks
- Comments/collaboration
- Attachments
- PDF export

## Questions?

Refer to `CLAUDE.md` for full project specification and design details.

---

**Built with ❤️ for minimal friction task management**
