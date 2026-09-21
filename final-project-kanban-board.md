# Final Project: Kanban Board Task Manager

## Overview

Build a **fully functional Kanban Board** web application using **HTML, CSS, and Vanilla JavaScript only**. No external libraries or frameworks are allowed (no React, no jQuery, no Bootstrap, no Chart.js, etc.).

This project tests your ability to build a real-world application from scratch using everything you learned throughout the course.

---

## Rules

| Rule | Details |
|---|---|
| **No Libraries** | Vanilla HTML + CSS + JS only. No npm packages, no CDN links |
| **No Frameworks** | No React, Vue, Angular, jQuery, Bootstrap, Tailwind, etc. |
| **No Copy-Paste** | You must understand and be able to explain every line of your code |
| **Oral Discussion** | You will be asked to explain your code and make live modifications |
| **Individual Work** | Each student submits their own project independently |

> **Warning:** Using AI to generate your code without understanding it will be detected during the oral discussion. You must be able to explain your logic, modify code on the spot, and answer "what if?" questions.

---

## Project Structure

```
kanban-project/
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── app.js              // Entry point - initializes the application
│   ├── board.js             // Board and column management logic
│   ├── task.js              // Task CRUD operations (Create, Read, Update, Delete)
│   ├── dragdrop.js          // Drag and Drop system
│   ├── storage.js           // localStorage read/write operations
│   ├── filters.js           // Search and filter functionality
│   └── utils.js             // Helper/utility functions
└── assets/
    └── icons/               // Any icons or images used
```

> You may adjust the file structure as needed, but your code **must** be organized into multiple files with clear separation of concerns. A single giant file will lose marks.

---

## Required Features

All features below must be implemented and delivered together as a single final submission.

---

### 1. Board Layout

- Create a board with **4 columns**: `To Do`, `In Progress`, `Review`, `Done`
- Each column displays a **header** with the column name
- Each column displays a **task counter** that updates automatically (e.g., "3 tasks")
- A **"+ Add Task"** button at the bottom of each column

---

### 2. Task Management (CRUD)

#### 2.1 Add a Task

Create a form/modal to add a new task with the following fields:

| Field | Type | Required? |
|---|---|---|
| Title | Text (max 100 characters) | Yes |
| Description | Textarea | No |
| Priority | High / Medium / Low | Yes |
| Label | Design / Development / Testing / Documentation | No |
| Assignee | Text (person's name) | No |
| Deadline | Date input | No |
| Estimated Time | Number (hours) | No |

#### 2.2 Task Card Display

Each task card on the board must show:

- Task title
- Priority indicator (colored border or badge): Red = High, Yellow = Medium, Green = Low
- Label badge (if set)
- Assignee name (if set)
- Deadline (if set) - with a **red warning** if the deadline has passed
- Subtask progress bar (if subtasks exist)
- Comment count badge (if comments exist)
- Edit button
- Delete button

#### 2.3 Edit a Task

- Clicking the **edit button** or the **task card** opens a detail modal
- All fields can be modified
- Changes are saved when the user clicks "Save"

#### 2.4 Delete a Task

- Clicking the **delete button** shows a **custom confirmation dialog** (not the browser's default `alert`/`confirm`)
- If confirmed, the task is removed
- An **Undo notification** appears for 10 seconds allowing the user to restore the deleted task

---

### 3. Drag & Drop System

Implement drag and drop using the **HTML5 Drag and Drop API** (no libraries).

#### 3.1 Drag and Drop Between Columns

- Tasks can be **dragged** from one column and **dropped** into another
- The following events must be handled: `dragstart`, `dragover`, `dragenter`, `dragleave`, `drop`, `dragend`

#### 3.2 Visual Feedback During Drag

- The dragged card becomes **semi-transparent** (opacity)
- The target column **highlights** when a card is dragged over it
- A **drop indicator line** appears between cards showing where the task will be placed

#### 3.3 Reorder Within the Same Column

- Tasks can be **reordered** within the same column by dragging up or down
- The new order is saved to `localStorage`

#### 3.4 Column Movement Rules

Implement the following movement restrictions:

```
Allowed:
  To Do → In Progress
  In Progress → Review
  Review → Done
  Any column → previous column (moving back is allowed)

Blocked:
  To Do → Review (skipping a column forward)
  To Do → Done (skipping columns forward)
  In Progress → Done (skipping a column forward)
```

- If a user tries to skip a column, the **drop is rejected** and a **warning notification** appears

#### 3.5 WIP Limit (Work In Progress Limit)

- The `In Progress` column has a **maximum limit of 5 tasks**
- The `Review` column has a **maximum limit of 3 tasks**
- If the limit is reached, the column **visually indicates** it is full (e.g., red border)
- Dropping a task into a full column is **blocked** with a notification

#### 3.6 Auto-Timestamp on Move

- When a task moves to `In Progress`, record `startedAt` timestamp
- When a task moves to `Done`, record `completedAt` timestamp
- Display **time spent** on completed tasks (e.g., "Took 2 days, 5 hours")

---

### 4. Search and Filter

#### 4.1 Live Search

- A search input in the header
- Results update **as the user types** (no search button needed)
- Search matches against task **title** and **description**
- Matching text is **highlighted** in the results
- Non-matching tasks are **hidden**, not removed

#### 4.2 Filter System

Implement the following filters (can be combined together):

| Filter | Options |
|---|---|
| Priority | All / High / Medium / Low |
| Label | All / Design / Development / Testing / Documentation |
| Assignee | All / list of names from existing tasks |
| Status | Active / Overdue / Completed |

- Filters work **together** (e.g., "High priority" + "Design" shows only high-priority design tasks)
- A "Clear Filters" button resets all filters

#### 4.3 Sort Options

Allow sorting tasks within each column by:

- Date created (newest / oldest)
- Priority (high to low / low to high)
- Deadline (soonest first)
- Alphabetical (A-Z / Z-A)

---

### 5. Subtasks (Checklist)

Each task can contain a **checklist of subtasks**:

- Add subtask items inside the task detail modal
- Each subtask has a checkbox (done / not done)
- Delete a subtask
- A **mini progress bar** appears on the task card showing completion percentage
- Example: 2 out of 5 subtasks done = 40% bar

---

### 6. Comments System

Each task has a comments section in its detail modal:

- Add a comment (author name + text + timestamp)
- Edit a comment
- Delete a comment
- Display time in **relative format** ("2 hours ago", "yesterday", "3 days ago")
- A **comment count** badge appears on the task card

---

### 7. Activity Log

Track and display all actions performed on the board:

```
14:30  Moved "Design Header" from Progress → Done
14:15  Added comment on "Design Footer"
13:00  Created task "Test API"
11:45  Changed priority of "Fix Bug" to High
10:30  Deleted task "Test Task"
```

- Displayed in a **side panel** that opens/closes
- Can be filtered by action type (created, moved, edited, deleted)
- Saved in `localStorage`

---

### 8. Statistics Dashboard

A section (modal or dedicated area) that displays:

| Statistic | Details |
|---|---|
| Total tasks | Broken down by column |
| Completion rate | Visual progress bar |
| Overdue tasks | Count of tasks past their deadline |
| Average completion time | From creation to Done |
| Tasks by label | Percentage breakdown |
| Tasks by priority | Percentage breakdown |

- **Charts must be built with CSS only** (using `div` widths, CSS gradients, etc. - no chart libraries)

---

### 9. Dark Mode

- A toggle button (sun/moon icon) in the header
- Smooth transition between light and dark themes using CSS transitions
- Theme preference saved in `localStorage`
- Implemented using **CSS custom properties** (variables):

```css
:root {
    --bg-primary: #ffffff;
    --text-primary: #1a1a2e;
}

[data-theme="dark"] {
    --bg-primary: #1a1a2e;
    --text-primary: #e0e0e0;
}
```

---

### 10. Export / Import

| Feature | Details |
|---|---|
| Export as JSON | Download all board data as a `.json` file |
| Import from JSON | Upload a `.json` file and restore the board |
| Print-friendly view | A clean layout for printing using `@media print` CSS |

---

### 11. Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `N` | Open new task form |
| `F` | Focus search input |
| `D` | Toggle dark mode |
| `Escape` | Close any open modal |
| `?` | Show keyboard shortcuts help |

---

### 12. Responsive Design

| Screen Size | Behavior |
|---|---|
| Desktop (1024px+) | All columns side by side |
| Tablet (768px - 1024px) | 2 columns per row |
| Mobile (< 768px) | 1 column visible with tabs/swipe to switch |

---

### 13. Data Persistence

- **All data** is saved to `localStorage` automatically after every change
- When the page reloads, the board restores its previous state completely
- No data should be lost on refresh
- Every drag-and-drop operation must immediately update `localStorage`

---

## Oral Discussion (Weight: 15%)

During the oral discussion, you will be asked to:

### Explain Your Code

- Walk through your code file by file
- Explain the logic behind your drag-and-drop implementation
- Explain how data flows from the UI to localStorage and back

### Answer Technical Questions

Examples:

- "Why did you use `preventDefault()` in `dragover`?"
- "What is the difference between `dragenter` and `dragover`?"
- "Why does `draggable='true'` need to be set explicitly?"
- "How does your filter system combine multiple filters?"
- "What happens if localStorage is full?"

### Answer "What If?" Questions

Examples:

- "What if I want to add a 5th column called 'Blocked'? What do you change?"
- "What if two browser tabs have the board open? How would you sync them?"
- "What if I want to limit the board to 50 tasks maximum? Where do you add that check?"

### Live Modification

You will be asked to make a small change **on the spot** without any external help. Examples:

- "Add a strikethrough style to tasks in the Done column"
- "Make the task card show a warning icon if the deadline is today"
- "Add a character counter to the task title input"
- "Change the WIP limit from 5 to 3 and show the remaining slots"

---

## Grading Criteria

| Criteria | Weight | Details |
|---|---|---|
| **Task Management** | 20% | CRUD operations, custom confirm dialog, undo notification |
| **Drag & Drop** | 20% | Smooth DnD, visual feedback, column rules, WIP limits, timestamps |
| **Search, Filter, Sort** | 15% | Live search with highlight, combined filters, sort options |
| **Subtasks & Comments** | 10% | Checklist with progress bar, comments with relative time |
| **Activity Log & Stats** | 10% | Side panel log, statistics dashboard with CSS charts |
| **UI/UX & Extras** | 10% | Dark mode, responsive, export/import, keyboard shortcuts, localStorage |
| **Oral Discussion** | 15% | Code explanation, Q&A, live modification |

### Bonus Points

| Bonus | Points |
|---|---|
| Multiple boards support (create, switch, delete boards) | +5 |
| Undo/Redo system (`Ctrl+Z` / `Ctrl+Y`) | +5 |
| Sound effects on drag and drop | +2 |
| Task card color customization | +2 |
| Animated transitions on task movement | +3 |
| Creative extra features | Up to +5 |

---

## Data Structure Reference

Use this as a starting reference for your data model. You may modify it as needed.

```javascript
const task = {
    id: "task_1695312000000",       // Unique ID (use Date.now())
    title: "Design Homepage",
    description: "Create a responsive homepage design",
    priority: "high",               // "high" | "medium" | "low"
    label: "design",                // "design" | "development" | "testing" | "documentation"
    assignee: "Ahmed",
    color: "#ff6b6b",
    deadline: "2026-09-25",
    estimatedHours: 3,
    columnId: "col_todo",
    createdAt: "2026-09-21T10:00:00",
    updatedAt: "2026-09-21T14:30:00",
    startedAt: null,                // Set when moved to In Progress
    completedAt: null,              // Set when moved to Done
    subtasks: [
        { id: "sub_1", text: "Design Header", done: true },
        { id: "sub_2", text: "Design Footer", done: false }
    ],
    comments: [
        {
            id: "com_1",
            author: "Sara",
            text: "Add dark mode support",
            timestamp: "2026-09-21T09:00:00",
            edited: false
        }
    ]
};
```

---

## Submission Requirements

- [ ] All source code files organized in the project structure shown above
- [ ] The application runs by opening `index.html` in a browser (no build step required)
- [ ] No external libraries or CDN links in the code
- [ ] All features listed above are implemented
- [ ] Data persists after page reload (localStorage)
- [ ] Code is clean, commented where necessary, and split into multiple files
- [ ] Ready for oral discussion

---

## Deadline

**Final Submission:** [Date]
**Oral Discussion:** [Date]

---

**Good luck!**
