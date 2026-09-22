# Task Dashboard - Claude Code Build Instructions

**Project:** Local HTML Task Management Dashboard  
**User:** Amanda  
**Delivery:** Single self-contained HTML file  
**Status:** Ready to build  

---

## Overview

Build a **single HTML file** containing a task management and project timeline dashboard. No external dependencies, no CDN calls, corporate-friendly. All data stored in `localStorage`. Pixel aesthetic with sage green accents.

**Reference Documents:**
- `CLAUDE.md` — Full project spec (features, data model, design)
- Mockups — Design system and UX flows (provided during design phase)

---

## Tech Stack

- **HTML5** — Single file, semantic markup
- **CSS** — Inline styles (no external stylesheets) + minimal global styles
- **Vanilla JavaScript** — No frameworks, no libraries
- **localStorage** — Data persistence (JSON serialization)
- **Font:** Monospace (Courier New) for pixel aesthetic

---

## File Structure

**Output: `dashboard.html` (single file, ~60-100KB)**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Task Dashboard</title>
  <style>
    /* Global CSS variables and styles */
  </style>
</head>
<body>
  <!-- HTML Structure (4 main views: TASKS, TIMELINE, SUMMARY, SETTINGS) -->
  <script>
    /* All JavaScript (state, storage, interactions, event listeners) */
  </script>
</body>
</html>
```

---

## Data Model

### 1. Task Object
```javascript
{
  id: string (uuid),                    // Unique identifier
  title: string,                        // Task name
  project: string,                      // Project name
  stage: string,                        // Project stage (optional)
  status: "TO DO" | "IN PROG" | "PENDING" | "DONE",
  dueDate: string,                      // DD MMM YYYY format
  notes: string,                        // Unrestricted long-form text
  priority: boolean,                    // Flagged as high priority
  createdAt: ISO timestamp,             // Auto-set on creation
  startedAt: ISO timestamp (nullable),  // Set when status → IN PROG
  completedAt: ISO timestamp (nullable),// Set when status → DONE
  statusHistory: [
    { status: string, changedAt: ISO timestamp }
  ],
  order: number                         // For drag-reorder
}
```

### 2. Project Object
```javascript
{
  id: string (uuid),
  name: string,
  stages: [
    { id: string, name: string, order: number }
  ]
}
```

### 3. Settings Object
```javascript
{
  userName: string,
  projects: [{ id: string, name: string }],
  statuses: [
    { id: string, name: string }  // Default: TO DO, IN PROG, PENDING, DONE
  ]
}
```

### 4. Storage Keys (localStorage)
```
tasks              → JSON array of task objects
projects           → JSON array of project objects
settings           → JSON object with user config
```

---

## Core Features (Implementation Order)

### Phase 1: Data & Storage Foundation
1. ✅ Initialize localStorage with defaults (if empty)
2. ✅ Implement Task CRUD (Create, Read, Update, Delete)
3. ✅ Implement Project CRUD
4. ✅ Implement Settings CRUD
5. ✅ Auto-save on every change (debounce if needed)

### Phase 2: UI Structure & Navigation
1. ✅ Build HTML layout: Sidebar + Main area
2. ✅ Sidebar: Navigation tabs (TASKS, TIMELINE, SUMMARY, SETTINGS)
3. ✅ Header banner with date/time (auto-update clock)
4. ✅ Tab switching (show/hide views)
5. ✅ View state management (remember last active tab)

### Phase 3: TASKS Tab
1. ✅ Quick Add Form
   - Fields: title | project dropdown | date (pre-filled today) | ADD button
   - Validate before submit (title + project required)
   - Clear form after success
   - Auto-focus title input after add

2. ✅ Task List Display
   - Grid layout: drag handle | title+project/stage | status dropdown | date | notes link | 3 buttons | delete
   - Render all active tasks (TO DO, IN PROG, PENDING)
   - Collapsed DONE section with count

3. ✅ Task Interactions
   - **Drag to reorder:** Implement drag-and-drop (update `order` field, persist)
   - **Status dropdown:** Change status directly, update task
   - **"Start working" button:**
     - Click → status changes to "IN PROG" + set `startedAt`
     - Button text changes to "Done"
     - Card outline/text color changes to lavender (#c9b8e4)
   - **"Done" button:**
     - Click → status changes to "DONE" + set `completedAt`
     - Task moves to DONE section
     - Card becomes muted/strikethrough
   - **Flag button (🚩):**
     - Independent of status
     - Toggle `priority` boolean
     - Card outline/text changes to yellow (#e8ddb8) when flagged
     - Can flag at any status
   - **Notes link (expandable):**
     - Click "notes ▼" → expand/collapse notes section below card
     - Textarea for editing notes (unrestricted)
     - Save on blur or Enter key
   - **Delete button (×):** Remove task

4. ✅ Task Card Cosmetics (Outline + Text Colors Only)
   - **Always gray background (#3d3d3d)**
   - **Outline & text color by state:**
     - TO DO: Gray (#6a7a5a outline, #a6b08a text)
     - IN PROG (Lavender): #c9b8e4 outline, #d9cce9 text
     - PENDING: #e4c5d8 outline, #e8c5d8 text
     - DONE: Muted (#5a6a4a), strikethrough text
     - FLAGGED: Yellow (#e8ddb8 outline, #f0e8d0 text) — overlays on state color
   - **Dropdown & button colors match card state**
   - **All borders match state outline color**

5. ✅ Keyboard Shortcuts (TASKS tab)
   - `cmd/ctrl+n` → Focus quick add form
   - `cmd/ctrl+w` → Toggle "working on" for first active task
   - `cmd/ctrl+d` → Mark first active task as done
   - Display at bottom

### Phase 4: TIMELINE Tab
1. ✅ Project Selector
   - Button grid of all projects
   - Click to switch (highlight active project)

2. ✅ Pipeline View
   - Horizontal swimlane of stages
   - Per stage: name | progress bar | task count (X/Y) | task preview | edit button | delete button
   - "+ add stage" button at end to insert new stage
   - Edit stage: allow rename, delete
   - Add stage: show input for new stage name

3. ✅ Stage Display
   - Progress bar shows completion % (tasks with status DONE)
   - Task preview: collapsed list of tasks in that stage
   - Task count format: "3/5 tasks" (completed/total)
   - Color: Sage green border if 100% complete, gray otherwise

4. ✅ Keyboard Shortcuts (TIMELINE tab)
   - Same as TASKS tab

### Phase 5: SUMMARY Tab
1. ✅ Key Metrics (4-column grid)
   - Total Tasks (count all tasks)
   - Completion % (DONE count / total count * 100)
   - In Progress (IN PROG count)
   - Overdue (tasks with dueDate < today AND status != DONE)

2. ✅ Tasks by Status (4-column breakdown)
   - TO DO count + %
   - IN PROG count + %
   - PENDING count + %
   - DONE count + %

3. ✅ Completion by Project (Progress bars)
   - Per project: name | % complete (N/total) | progress bar
   - Example: "Adaptive-Seg 60% (6/10)"

4. ✅ Task Details Table (Filterable View) ⭐ **CRITICAL**
   - **Columns:** Task | Project | Status | Date/Time | Notes
   - **Data rows:**
     - One row per task (all statuses)
     - Date/Time format: "22 Sep 2026 14:32" (startedAt for IN PROG, completedAt for DONE, created for others)
     - Notes column: full text, word-wrapping, scrollable height-limited container
     - Strikethrough + muted text for DONE tasks
   - **Filters:**
     - Search box: filter task titles (real-time)
     - Project dropdown: filter by project
     - Status dropdown: filter by status
   - **Sticky header:** Table header stays visible while scrolling
   - **Purpose:** Copy-paste into performance reviews

5. ✅ Export Buttons
   - "Export as PDF" (stretch goal, may skip for MVP)
   - "Copy for review" (copy visible table rows to clipboard)
   - "Email summary" (stretch goal, may skip for MVP)

### Phase 6: SETTINGS Tab
1. ✅ User Name
   - Text input, pre-filled from settings
   - Save on blur or button click

2. ✅ Projects List
   - Display all projects
   - Per project: name | × delete button
   - "+ add project" input at bottom
   - Save new project

3. ✅ Statuses List
   - Display all statuses (default: TO DO, IN PROG, PENDING, DONE)
   - Per status: name | × delete button (except defaults)
   - "+ add status" input at bottom
   - Save new status

4. ✅ SAVE button
   - Persist all settings changes to localStorage

---

## Color Palette

### CSS Variables (define in `<style>`)
```css
:root {
  --bg-dark: #2a2a2a;
  --bg-card: #3d3d3d;
  --bg-input: #2a2a2a;
  
  --text-primary: #a6b08a;     /* Sage green - primary accent */
  --text-secondary: #7a8a6a;   /* Muted sage */
  --text-muted: #5a6a4a;       /* Very muted */
  
  --border-default: #6a7a5a;
  --border-secondary: #5a6a4a;
  
  /* State colors (outline + text) */
  --color-todo: #a6b08a;       /* Gray */
  --color-inprog: #c9b8e4;     /* Lavender */
  --color-pending: #e4c5d8;    /* Pink */
  --color-flagged: #e8ddb8;    /* Yellow */
  --color-done: #5a6a4a;       /* Muted gray */
  
  /* Text colors for states */
  --text-inprog: #d9cce9;
  --text-pending: #e8c5d8;
  --text-flagged: #f0e8d0;
}
```

---

## CSS Guidelines

1. **Inline styles** where possible (for individual elements)
2. **Global `<style>` block** for:
   - CSS variables
   - Reset/base styles (body, inputs, tables)
   - Utility classes (`.active`, `.muted`, `.strikethrough`)
   - Responsive grid layout
   - Hover/focus states

3. **No external stylesheets or CDN imports**

4. **Font:** `font-family: 'Courier New', monospace` (system font, always available)

5. **Spacing:** Use `rem` for vertical rhythm (1rem ≈ 16px)
   - Gaps: 8px-12px
   - Padding: 10px-14px

6. **Borders:** 1px or 2px solid, matching state color
   - Default: `1px solid var(--border-secondary)`
   - Active/focused: `2px solid var(--color-inprog)` etc.

7. **No shadows, gradients, or blur effects** (pixel aesthetic)

---

## JavaScript Architecture

### Global State Object
```javascript
const app = {
  currentTab: 'tasks',        // Active tab
  tasks: [],                  // All tasks
  projects: [],               // All projects
  settings: {},               // User settings
  draggedTaskId: null,        // For drag-reorder
};
```

### Core Functions

#### Storage
```javascript
function saveToStorage()      // Persist app state to localStorage
function loadFromStorage()    // Load app state from localStorage
function initDefaults()       // Initialize with default projects/statuses if needed
```

#### Task Management
```javascript
function createTask(title, project, dueDate)    // Add new task
function updateTask(taskId, updates)            // Update task fields
function deleteTask(taskId)                     // Remove task
function setTaskStatus(taskId, status)          // Change status (with timestamp)
function toggleTaskPriority(taskId)             // Toggle flag
function updateTaskNotes(taskId, notes)         // Save notes
function reorderTasks(fromIndex, toIndex)       // Drag-reorder
```

#### Project Management
```javascript
function createProject(name)                    // Add new project
function addProjectStage(projectId, stageName)  // Add stage
function updateProjectStage(projectId, stageId, stageName)
function deleteProjectStage(projectId, stageId)
function deleteProject(projectId)               // Remove project
```

#### Settings Management
```javascript
function updateUserName(name)
function addProject(name)
function removeProject(projectId)
function addStatus(name)
function removeStatus(statusId)
```

#### UI Rendering
```javascript
function renderTasks()                          // Render TASKS tab
function renderTimeline()                       // Render TIMELINE tab
function renderSummary()                        // Render SUMMARY tab
function renderSettings()                       // Render SETTINGS tab
function switchTab(tabName)                     // Switch between views
function updateClock()                          // Update header date/time (every second)
```

#### Utilities
```javascript
function generateUUID()                         // Create unique IDs
function formatDate(iso)                        // Format ISO → "DD MMM YYYY"
function formatDateTime(iso)                    // Format ISO → "22 Sep 2026 14:32"
function calculateCompletion()                  // % of tasks done
function countTasksByStatus()                   // Return {TO_DO, IN_PROG, PENDING, DONE}
function getTasksByProject(projectName)         // Filter tasks
```

### Event Listeners

**Quick Add Form:**
- `click` on ADD button → validate + create task
- `keypress` on date input → today's date by default

**Task Cards:**
- `click` on drag handle → start drag (update order on drop)
- `change` on status dropdown → update task status
- `click` on "Start working" → status → IN PROG (auto-set startedAt)
- `click` on "Done" → status → DONE (auto-set completedAt, move to DONE section)
- `click` on flag button → toggle priority
- `click` on notes link → expand/collapse notes
- `click` on delete button → confirm + delete task

**Project Selector (Timeline):**
- `click` on project button → switch project

**Timeline Stages:**
- `click` on "+ add stage" → show input for new stage name
- `click` on "⋮ edit" → allow rename/delete stage

**Settings:**
- `input` on user name → update (auto-save)
- `click` on add project → create new
- `click` on delete (×) on project → remove
- `click` on add status → create new
- `click` on SAVE button → persist all changes

**Tab Navigation:**
- `click` on sidebar tabs → switch view

**Keyboard Shortcuts:**
- Bind `cmd/ctrl+n`, `cmd/ctrl+w`, `cmd/ctrl+d`, etc. to respective actions

---

## Drag & Drop Implementation

**For task reordering in TASKS tab:**

1. Use native HTML5 drag-and-drop or simple mouse events
2. Track `draggedTaskId` during drag
3. Update `order` field for affected tasks
4. Re-render task list
5. Save to localStorage

**Simple approach (no library needed):**
- On `mousedown` on drag handle: record start position + taskId
- On `mousemove`: show visual feedback (opacity, highlight)
- On `mouseup`: calculate new position, update order, persist

---

## Keyboard Shortcuts Implementation

```javascript
document.addEventListener('keydown', (e) => {
  // Detect cmd/ctrl based on OS
  const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
  const isCtrlCmd = isMac ? e.metaKey : e.ctrlKey;
  
  if (isCtrlCmd) {
    if (e.key === 'n') {
      e.preventDefault();
      // Focus quick add form
    }
    if (e.key === 'd') {
      e.preventDefault();
      // Mark first active task as done
    }
    if (e.key === 'w') {
      e.preventDefault();
      // Toggle working on first active task
    }
    // ... etc
  }
});
```

Display shortcuts at bottom of each tab:
```
cmd/ctrl+n → new task
cmd/ctrl+d → mark done
cmd/ctrl+w → working on
cmd/ctrl+/ → filter
cmd/ctrl+e → export
cmd/ctrl+k → all shortcuts
```

---

## Important Implementation Notes

### 1. Date Handling
- **Input format:** HTML `<input type="date">` returns ISO (YYYY-MM-DD)
- **Display format:** "DD MMM YYYY" (e.g., "22 Sep 2026")
- **Display datetime:** "22 Sep 2026 14:32" (for summary)
- **Store as:** ISO timestamps (for startedAt, completedAt, etc.)
- **Pre-fill add form:** `today = new Date().toISOString().split('T')[0]`

### 2. Task Status Flow (CRITICAL)
```
1. Default: TO DO
   ↓
2. User clicks "Start working"
   → Status auto-changes to IN PROG
   → Set startedAt timestamp
   → Button text changes to "Done"
   → Card colors change to lavender
   
3. User clicks "Done"
   → Status auto-changes to DONE
   → Set completedAt timestamp
   → Task moves to DONE section
   → Card becomes muted/strikethrough
   
4. Flag independent: Can flag at any status
   → Priority = true
   → Card colors change to yellow
   → Overlays on current status color
```

### 3. Task Card Outline/Text Color Rules
- **ALWAYS keep background #3d3d3d**
- **Only change outline + text colors**
- **Outline color:**
  - TO DO: #6a7a5a
  - IN PROG: #c9b8e4 (lavender)
  - PENDING: #e4c5d8 (pink)
  - DONE: #5a6a4a (muted)
  - FLAGGED: #e8ddb8 (yellow) — overlays
- **Text color matches outline color** (slightly lighter for readability)
- **Dropdown & buttons inherit state colors**

### 4. Dropdown Styling
- `<select>` tags inherit state colors from parent card
- Background: #2a2a2a (dark)
- Border & text: match card state color
- No custom styling beyond border/text color

### 5. Notes Expansion (Expandable Per Task)
- Clicking "notes ▼" shows/hides a textarea below the task card
- Textarea style matches card state color
- Save on blur or Enter key
- Unrestricted character count—no validation

### 6. DONE Section (Collapsed by Default)
- Collapsed header: "▶ DONE (7)" (clickable toggle)
- Show/hide all completed tasks
- Completed tasks show: strikethrough title + muted colors

### 7. Summary Table (Scrollable, Sticky Header)
- Columns: Task | Project | Status | Date/Time | Notes
- Sticky header (use `position: sticky; top: 0;` on `<thead>`)
- Notes column: show full text, word-wrapping, scrollable (max-height)
- Filters (search + project + status) update table in real-time
- Color-code status: Lavender for IN PROG, Pink for PENDING, Green strikethrough for DONE

### 8. localStorage Serialization
- Save entire app state as JSON:
  ```javascript
  localStorage.setItem('tasks', JSON.stringify(app.tasks));
  localStorage.setItem('projects', JSON.stringify(app.projects));
  localStorage.setItem('settings', JSON.stringify(app.settings));
  ```
- Load on page load:
  ```javascript
  app.tasks = JSON.parse(localStorage.getItem('tasks')) || [];
  // etc
  ```
- Debounce saves to avoid excessive writes (e.g., 500ms after last change)

### 9. Auto-Update Clock (Header)
- Update date/time in header every second:
  ```javascript
  setInterval(() => {
    document.querySelector('.header-time').textContent = new Date().toLocaleString('en-SG', {...});
  }, 1000);
  ```

### 10. Default Projects & Statuses
**If settings are empty on first load, initialize:**
```javascript
projects: [
  { id: uuid(), name: 'Adaptive-Seg', stages: [] },
  { id: uuid(), name: 'Regulatory', stages: [] },
  { id: uuid(), name: 'Resume Bot', stages: [] }
]

statuses: [
  { id: '1', name: 'TO DO' },
  { id: '2', name: 'IN PROG' },
  { id: '3', name: 'PENDING' },
  { id: '4', name: 'DONE' }
]
```

---

## Testing Checklist

- [ ] Create task (quick add form)
- [ ] Edit task (click title to edit inline)
- [ ] Delete task
- [ ] Drag task to reorder
- [ ] Click "Start working" → status changes to IN PROG, button shows "Done"
- [ ] Click "Done" → status changes to DONE, task moves to DONE section
- [ ] Toggle flag button → outline changes to yellow
- [ ] Expand/collapse notes
- [ ] Change status via dropdown
- [ ] Switch projects on timeline
- [ ] Add/remove project stage
- [ ] Add/remove project
- [ ] Add/remove status
- [ ] Update user name
- [ ] Refresh page → all data persists (localStorage)
- [ ] Summary metrics calculate correctly
- [ ] Filter tasks in summary table (search, project, status)
- [ ] Keyboard shortcuts work (cmd/ctrl+n, etc.)
- [ ] Clock updates every second
- [ ] All colors match spec (outline/text colors only)

---

## Stretch Goals (Post-MVP)

- Export as PDF (requires library, probably skip)
- Email summary (requires backend, probably skip)
- Undo/redo (complex, skip)
- Recurring tasks (out of scope)
- Time tracking (out of scope)
- Collaboration (out of scope)

---

## Deployment

**Output:** Single `dashboard.html` file  
**Size target:** < 150KB (compressed)  
**Distribution:** User double-clicks to open in browser  
**No server required**

---

## Questions or Blockers?

If any requirement is unclear, refer to `CLAUDE.md` or the mockup designs. Core principle: **lowest friction for task creation**—everything else is secondary.

**Build from Phase 1 → Phase 6, testing as you go. Good luck!** 🚀