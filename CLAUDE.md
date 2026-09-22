# Task Dashboard - Project Spec

**Status:** Design Complete - Ready for Build  
**User:** Amanda  
**Created:** 22 Sep 2026  
**Last Updated:** 22 Sep 2026

---

## Project Overview

A local HTML-based task management and project timeline dashboard designed for minimal friction task creation and tracking. Built for production ML/banking environment. Corporate-friendly (no external CDN calls, single HTML file, no package managers).

**Distribution:** Single self-contained HTML file (double-click to run).

---

## Core Design Principles

1. **Lowest path of resistance to adding/updating tasks** — Primary UX focus
2. **Everything else is secondary** — Projects, templates, summaries are supporting features
3. **Pixel aesthetic with sage green (#a6b08a) accents** — Intentionally lo-fi to avoid corporate security triggers
4. **Local-first persistence** — localStorage or IndexedDB, no cloud sync
5. **No external dependencies** — Pure vanilla HTML/CSS/JavaScript

---

## Feature Set - Confirmed

### 1. TASKS Tab ✅

**Primary focus.** Minimal friction for task management.

#### Quick Add Form (Hero Section)
- Fields: Task title | Project (dropdown) | Date | ADD button
- Date pre-fills with TODAY (DD MMM YYYY format, e.g., "22 Sep 2026")
- No tags field
- All required fields validated before submit

#### Task Card Layout
Grid columns: Drag Handle | Task Title + Project/Stage | Status Dropdown | Date | Notes Link | **Three Action Buttons** | Delete

**Three Action Buttons (separate):**
1. **State button** → "Start working" or "Done" (context-aware)
2. **Flag button (🚩)** → Toggle high-priority flag (separate, always available)
3. **Delete button (×)** → Remove task

**Task Card Interactions:**
- **Drag handle (⋮⋮)** → Reorder tasks within active list
- **Status dropdown** → 4 states: TO DO | IN PROG | PENDING | DONE
- **State button** → See workflow below
- **Flag button (🚩)** → Toggle high-priority flag (independent of state)
- **Notes link** → Expandable long-form notes (unrestricted character count, secondary view)
- **Delete button (×)** → Remove task

#### Task Card Cosmetics (Gray Background + Outline Colors)
- **Background:** Always #3d3d3d (gray)
- **Outline color + font color changes ONLY (no fill):**
  - **Default (TO DO):** Gray outline (#6a7a5a), gray text (#a6b08a)
  - **In Progress (IN PROG):** Lavender outline (#c9b8e4), lavender text (#d9cce9)
  - **Pending:** Pink outline (#e4c5d8), pink text (#e8c5d8)
  - **Flagged (High Priority):** Yellow outline (#e8ddb8), yellow text (#f0e8d0) — overlays on top of current state
  - **Done:** Muted (#5a6a4a), strikethrough text

#### Workflow: The Full Cycle (Critical Flow)
**User journey for activating a task:**

1. **Initial state (TO DO):**
   - Card has default gray outline
   - State button shows: **"Start working"**
   - Status dropdown shows: TO DO
   
2. **User clicks "Start working" button:**
   - Status dropdown auto-changes to: **IN PROG**
   - Card outline changes to: **Lavender (#c9b8e4)**
   - Card text changes to: **Lavender (#d9cce9)**
   - State button text changes to: **"Done"**
   - Flag button remains available (independent)

3. **User clicks "Done" button:**
   - Status dropdown auto-changes to: **DONE**
   - Card moves to DONE section
   - Card becomes muted/strikethrough
   - All colors fade to gray (#5a6a4a)

**Flag behavior (independent):**
- Clicking flag button toggles high-priority state at ANY point in the workflow
- When flagged: Card outline + text become yellow (#e8ddb8 / #f0e8d0)
- Can flag a TO DO, IN PROG, PENDING, or even DONE task
- Flag state persists independently of task status

#### Task List Organization
- **Active Tasks section** → Displays TO DO + IN PROG + PENDING tasks
- **Done section** → Collapsed by default, shows count "▶ DONE (7)"
- Completed tasks are muted, strikethrough text

#### Notes/Remarks (Expandable)
- Expandable per task via "notes ▼" link
- Unrestricted long-form text input
- Collapses to save space
- Secondary visual—not prominent but always accessible

---

### 2. TIMELINE Tab ✅

Project-centric view showing pipeline stages.

#### Project Selector
- Buttons for each project (Adaptive-Seg, Regulatory, Resume Bot, etc.)
- Click to switch projects

#### Pipeline View (Customizable Stages)
Horizontal swimlane of project stages with:
- Stage name (ENGAGEMENT, KICKOFF, DATA EXPLORE, MODELING, DEPLOY, etc.)
- Progress bar (visual completion %)
- Task count (e.g., "2/2 tasks", "3/5 tasks")
- Collapsible task preview (checkmarks for done, bullets for active)
- "⋮ edit" button per stage to rename/customize/remove
- "+ add stage" button to extend pipeline

#### Stage Colors (Status Visual)
- **Completed stage:** Sage green border (#a6b08a)
- **Pending/In-progress:** Muted gray border
- **Active work:** Warm amber accent

#### Task Association
- Each task shows its project → stage inline (e.g., "Adaptive-Seg → Data Exploration")
- Clicking task name jumps to task detail in TASKS tab (if modal/expanded view exists)
- Optional: Stage selection dropdown in task card creation/edit

---

### 3. SUMMARY Tab ✅

Performance review and completion tracking.

#### Key Metrics (4-column grid)
- Total Tasks (count)
- Completion % (calculated)
- In Progress (count)
- Overdue (count)

#### Tasks by Status (4-column breakdown)
- TO DO count + percentage
- IN PROG count + percentage
- PENDING count + percentage
- DONE count + percentage

#### Completion by Project (Progress bars)
- Project name | % complete (N/total) | Progress bar
- One bar per project
- Example: "Adaptive-Seg 60% (6/10)"

#### Completed Tasks with Details (Filterable Task View) ⭐ **NEW**
**Replaces simple "completed tasks list"**

A detailed task view serving as a **filter/reference** for review writing. Shows:

**Column headers:**
- Task Title
- Project
- Status
- Completed/Started Date & Time
- Notes (preview or clickable expand)

**Data rows:**
- One row per task (all statuses: DONE, IN PROG, PENDING)
- Date/time format: "22 Sep 2026 14:32" (when completed, started, or status changed)
- Notes column shows preview or expandable text area (user can view full notes for review context)
- Sortable by date, project, status (optional)
- Searchable by task title (optional)

**Purpose:** User can easily scan completed work with timestamps and notes for performance review context. Notes are fully readable (no character limit shown).

#### Export / Action Buttons
- Export as PDF
- Copy for review (to clipboard)
- Email summary

---

### 4. SETTINGS Tab ✅

User configuration UI (similar to config file but with UI).

#### Editable Sections
1. **User Name** → Text input (pre-filled with current user)
2. **Projects** → List with add/remove per item
3. **Statuses** → List with add/remove per item (default: TO DO, IN PROG, PENDING, DONE)

#### Save Button
- Persists all settings to localStorage

---

## Header & Navigation

### Header Banner (All Views)
- View title (TASK DASHBOARD / PROJECT TIMELINE / PERFORMANCE SUMMARY)
- Current date + time (format: "22 Sep 2026 • 14:32 SGT")
- Emoji anchor (🕐 for tasks, 📋 for timeline, 📊 for summary)

### Left Sidebar
- Navigation tabs: TASKS | TIMELINE | SUMMARY | SETTINGS
- Active tab highlighted (sage green background)
- User stats (name, task counts: total | active | done)

---

## Keyboard Shortcuts (Mac + Windows)

Display at bottom of each tab.

| Shortcut | Action |
|----------|--------|
| `cmd/ctrl + n` | New task |
| `cmd/ctrl + d` | Mark done |
| `cmd/ctrl + w` | Working on (toggle) |
| `cmd/ctrl + /` | Filter tasks |
| `cmd/ctrl + e` | Export |
| `cmd/ctrl + k` | Show all shortcuts |

Label: "cmd = mac • ctrl = windows"

---

## Data Model

### Task Object
```
{
  id: string (uuid),
  title: string,
  project: string,
  stage: string (optional),
  status: "TO DO" | "IN PROG" | "PENDING" | "DONE",
  dueDate: string (DD MMM YYYY),
  notes: string (unrestricted),
  priority: boolean (flagged),
  createdAt: ISO timestamp,
  startedAt: ISO timestamp (nullable, set when status → IN PROG),
  completedAt: ISO timestamp (nullable, set when status → DONE),
  statusHistory: [
    { status: string, changedAt: ISO timestamp }
  ],
  order: number (drag-reorder index)
}
```

**Timestamp Notes:**
- `createdAt`: Automatically set on task creation
- `startedAt`: Set when user clicks "Start working" (status → IN PROG)
- `completedAt`: Set when user clicks "Done" (status → DONE)
- `statusHistory`: Track all status changes for audit/review context

### Project Object
```
{
  id: string,
  name: string,
  stages: [
    { id: string, name: string, order: number }
  ]
}
```

### Settings Object
```
{
  userName: string,
  projects: [{ id, name }],
  statuses: [{ id, name }],
  theme: "light" | "dark"
}
```

---

## Storage & Persistence

- **Primary:** localStorage (JSON serialization)
- **Backup strategy:** Export to JSON (manual save/restore)
- **No cloud sync** — Intentional design choice
- All data stays on local machine

---

## UI/UX Details

### Color Scheme
- **Primary accent:** Sage green (#a6b08a)
- **Background:** Dark (#2a2a2a)
- **Cards/Surfaces:** Medium gray (#3d3d3d)
- **Text:** Light gray (#a6b08a for primary, #7a8a6a for secondary)
- **State colors (outline only):**
  - Lavender (working): #c9b8e4
  - Pink (pending): #e4c5d8
  - Yellow (flagged): #e8ddb8
- **Success/completion:** Green (#7aaa6a)
- **Warning/overdue:** Red-ish (#d4756a)

### Typography
- **Font:** Monospace (Courier New) for pixel aesthetic
- **Headings:** 10px-14px bold, ALL CAPS for section headers
- **Body:** 9px-10px regular
- **Minimal font weights:** Regular (400) + Bold (500) only

### Borders & Spacing
- **Borders:** 1px or 2px solid, matching state color
- **Border-radius:** 0 (hard edges) or minimal (2px)
- **Spacing:** 8px-12px gaps between elements
- **Padding:** 10px-14px inside cards

### Interactions
- Hover states: subtle border/text color shift
- Focus states: visible (outline or highlight)
- Drag-and-drop: cursor: grab, visual feedback on drag
- Buttons: transparent bg, border-based style

#### Light Mode (Optional Toggle)
**Inverted palette while keeping sage green + state colors:**
- **Primary accent:** Sage green (#a6b08a) — kept same
- **Background:** Light (#f8f8f8)
- **Cards/Surfaces:** White (#ffffff)
- **Sidebar:** Very light gray (#f5f5f5)
- **Text:** Dark sage (#5a6a4a for primary, #666 for secondary, #888 for tertiary)
- **Borders:** Light gray (#d0d0d0)
- **State colors (outline only) — lighter shades:**
  - Lavender (working): #c9b8e4 (same, works on light bg)
  - Pink (pending): #e4c5d8 (same, works on light bg)
  - Yellow (flagged): #d4a76a (warmer tone for light bg, lighter than dark mode's #e8ddb8)
  - TO DO (gray): #666
  - IN PROG text: #7a6aa0
  - PENDING text: #8a5a7a
- **Success/completion:** Green (#7aaa6a, same)

**Light Mode Benefits:**
- Better for daytime use + accessibility
- Keeps sage green as recognizable primary accent
- State colors (lavender, pink, yellow) still pop on light backgrounds
- Maintains pixel aesthetic + minimal design
- Toggle via CSS variables (theme switcher optional)

**Implementation:** Use CSS custom properties to swap between dark/light on page load or with user toggle. See INSTRUCTIONS.md CSS Guidelines section.

---

## Not Included (Explicitly Out of Scope)

- ❌ Time tracking / time logged (mentioned but removed for MVP)
- ❌ Audit trail / change history
- ❌ Markdown support in notes
- ❌ Tags
- ❌ Recurring tasks
- ❌ Dependencies between tasks
- ❌ Comments/collaboration
- ❌ Attachments
- ❌ Cloud sync
- ❌ Mobile app (single-file HTML only)

---

## Build Stack

- **HTML5** (single file)
- **CSS** (inline styles + minimal global)
- **Vanilla JavaScript** (no frameworks)
- **localStorage** (data persistence)
- **No external libraries** (corporate-friendly)

---

## File Structure

```
dashboard.html (single file, ~50-80KB)
├── <style> (CSS for pixel aesthetic + animations)
├── <body> (HTML structure)
└── <script> (JS for state, interactions, persistence)
```

---

## Success Criteria

✅ Minimal friction for task creation (< 10 seconds per task)  
✅ Visual state feedback (color/outline changes on interaction)  
✅ No external dependencies (corporate proxy friendly)  
✅ Local persistence (survives browser restart)  
✅ Keyboard shortcuts for power users  
✅ Performance review-ready summary view  
✅ Pixel aesthetic (intentionally retro)  

---

## Next Steps

1. ✅ Design mockups (COMPLETE)
2. ⏳ Build single HTML file with localStorage
3. ⏳ Implement TASKS tab (full CRUD)
4. ⏳ Implement TIMELINE tab (stage management)
5. ⏳ Implement SUMMARY tab (metrics + export)
6. ⏳ Implement SETTINGS tab
7. ⏳ Add keyboard shortcuts
8. ⏳ Testing + polish
9. ⏳ Export feature (PDF/JSON/email)

---

**Ready to build?**