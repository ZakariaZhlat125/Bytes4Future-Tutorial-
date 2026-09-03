# Training Project Generation — HTML, CSS & JavaScript (Sessions 1-7)

This curriculum builds **one real application** across seven JavaScript sessions. Students see the same project grow from a static page to a fully interactive task manager.

---

## 1. Project Overview

### Project Name

**FocusTask** — a simple Task Management App.

### Project Idea

Students build a personal task manager where they can add, view, complete, delete, and filter tasks. The app calculates simple statistics and provides a polished, responsive interface.

### What the Final Project Will Do

By the end of Session 7, the app will:

- Display a clean task list with cards.
- Let users add new tasks through a form.
- Mark tasks as completed or pending.
- Delete tasks.
- Filter tasks by status (All / Pending / Completed).
- Show a task counter and completion percentage.
- Validate form inputs.
- Show helpful empty states and feedback messages.

### Why This Project is Suitable for Sessions 1-7

- It has a clear real-world use case that every student understands.
- It starts as plain HTML and CSS, then adds JavaScript step by step.
- Each session adds one layer of behavior that naturally needs the new concept.
- It gives visible results quickly, which keeps students motivated.
- It can be built without frameworks, classes, modules, or APIs.
- The same data structure (an array of task objects) is reused from Session 4 onward.

---

## 2. Technologies

- **HTML5** — semantic structure, forms, buttons, lists.
- **CSS3** — resets, custom properties, flexbox, grid, media queries.
- **Vanilla JavaScript** — variables, operators, conditionals, loops, functions, DOM, events.

No build tools, frameworks, or external libraries are used.

---

## 3. Final Project Features

| Feature | Description |
| ------- | ----------- |
| Add task | User fills a form with title and priority. |
| Task list | Tasks displayed as cards with status and priority. |
| Complete task | Click a button to toggle completion. |
| Delete task | Remove a task from the list. |
| Filter | Show all, only pending, or only completed tasks. |
| Statistics | Display total, pending, completed counts and percentage. |
| Validation | Prevent empty, invalid, or duplicate task titles. |
| Empty state | Friendly message when there are no tasks. |
| Responsive | Works on desktop and mobile screens. |

---

## 4. Project Roadmap

| Session | Feature Added | JavaScript Concept |
| ------- | ------------- | ------------------ |
| 1 | Static HTML/CSS structure; first JS variables and console output | Variables, `console.log`, basic data types, simple calculations |
| 2 | Dynamic stats calculations and summary messages using template literals | `let` / `const`, arithmetic/string operators, template literals |
| 3 | Priority rules, input validation logic, and status messages | `if` / `else`, comparison and logical operators |
| 4 | Task list HTML string built from an array of objects | Arrays, objects, `for` loops, `for...of` |
| 5 | Reusable functions to render tasks and stats to the page | Functions, parameters, `return`, DOM selection and updates |
| 6 | Forms, click/submit events, and user interaction | Event listeners, form handling, input values, validation |
| 7 | Filters, complete application behavior, and final polish | Filter logic, event listeners, complete app flow |

---

## 5. Session 1 — HTML Structure & First JavaScript

### Learning Objectives

1. Create a valid HTML5 page with semantic elements.
2. Link a CSS file and a JavaScript file.
3. Declare variables and use basic data types.
4. Use `console.log` to inspect values.
5. Perform a simple calculation with variables.

### What We Already Have

Nothing. This is the starting point. Students create the project folder and the first three files.

### What We Will Add Today

- `index.html` with a header, a hardcoded sample task card, and a stats section.
- `style.css` with a clean, simple layout.
- `script.js` with variables for the app name, total tasks, and a `console.log` message.

### Problem

"We want to build a task manager, but we do not have a web page yet. Before we make it interactive, we need a solid structure and a place to show information."

### Let Students Guess

- What files does a basic website need?
- Where should the `<script>` tag go, and why?
- If we write `console.log("Hello")`, will students see it on the web page?
- What data type is the app title? What data type is the total number of tasks?

### Explain

Every web project starts with HTML for content, CSS for appearance, and JavaScript for behavior. In this session, JavaScript runs in the background and talks to the developer console. The page content is still static, but we are preparing the variables that will control it later.

### Live Coding

Create the folder structure:

```
focustask/
├── index.html
├── style.css
└── script.js
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FocusTask</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header class="app-header">
    <h1>FocusTask</h1>
    <p>Your simple task manager</p>
  </header>

  <main class="container">
    <section class="stats">
      <div class="stat-card">
        <span class="stat-label">Total</span>
        <span class="stat-value" id="totalTasks">0</span>
      </div>
      <div class="stat-card">
        <span class="stat-label">Pending</span>
        <span class="stat-value" id="pendingTasks">0</span>
      </div>
      <div class="stat-card">
        <span class="stat-label">Completed</span>
        <span class="stat-value" id="completedTasks">0</span>
      </div>
    </section>

    <section class="task-list">
      <h2>My Tasks</h2>
      <div class="task-card">
        <h3>Buy groceries</h3>
        <p>Priority: medium</p>
        <p>Status: pending</p>
      </div>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
```

**style.css**

```css
:root {
  --primary: #4f46e5;
  --bg: #f8fafc;
  --card-bg: #ffffff;
  --text: #1e293b;
  --muted: #64748b;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: var(--bg);
  color: var(--text);
  line-height: 1.5;
}

.app-header {
  background-color: var(--primary);
  color: white;
  padding: 1.5rem;
  text-align: center;
}

.container {
  max-width: 720px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-bottom: 2rem;
}

.stat-card {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  text-align: center;
}

.stat-label {
  display: block;
  color: var(--muted);
  font-size: 0.875rem;
}

.stat-value {
  display: block;
  font-size: 1.5rem;
  font-weight: bold;
}

.task-list h2 {
  margin-bottom: 1rem;
}

.task-card {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
}

.task-card h3 {
  margin-bottom: 0.5rem;
}

.task-card p {
  color: var(--muted);
  font-size: 0.875rem;
}

@media (max-width: 480px) {
  .stats {
    grid-template-columns: 1fr;
  }
}
```

**script.js**

```javascript
// App information
let appName = "FocusTask";
const appVersion = "1.0";

// Hardcoded sample data for now
let totalTasks = 1;
let completedTasks = 0;
let pendingTasks = totalTasks - completedTasks;

console.log("App name:", appName);
console.log("App version:", appVersion);
console.log("Total tasks:", totalTasks);
console.log("Pending tasks:", pendingTasks);
console.log("Completed tasks:", completedTasks);

// Simple calculation
let completionPercentage = (completedTasks / totalTasks) * 100;
console.log("Completion percentage:", completionPercentage);
```

### Student Interaction

- Predict the output of each `console.log` line before refreshing.
- Change `completedTasks` to `1` and recalculate `pendingTasks` and `completionPercentage`.
- Try changing `appVersion` later in the file. What happens? Why?

### Mini Challenge

**Challenge: Introduce Yourself Through the Console**

Create three variables: `firstName`, `lastName`, and `age`. Use `console.log` to print a sentence like:

```
FocusTask was built by [firstName] [lastName], who is [age] years old.
```

**Requirements**

- Use `let` for values that can change.
- Use `const` for values that never change.
- Use one `console.log` with string concatenation (`+`).

**Hints**

- `const PI = 3.14;` is a constant because PI never changes.
- `let` is good for a name you might update.

**Solution (do not show immediately)**

```javascript
const firstName = "Alex";
const lastName = "Rivera";
let age = 28;

console.log("FocusTask was built by " + firstName + " " + lastName + ", who is " + age + " years old.");
```

### Checkpoint

1. What are the three main files in this project and what is each one responsible for?
2. What is the difference between `let` and `const`?
3. Why do we place the `<script>` tag at the end of the `<body>`?
4. What does `console.log` do, and where can we see the result?
5. If `totalTasks` is `5` and `completedTasks` is `2`, what is `pendingTasks`?

### Common Student Mistakes

- Forgetting to close quotes around strings.
- Writing `let totalTasks = 1` and then `let totalTasks = 3` in the same scope.
- Trying to reassign a `const` variable.
- Looking for `console.log` output on the page instead of in the console.
- Using a space in a variable name.

### Questions Students May Ask

- Why is `const` needed if `let` can do everything?
  - `const` protects values that should never change, which prevents accidental bugs.
- What is `console.log` for if users cannot see it?
  - It is a developer tool. We use it to understand what our code is doing.
- Can we write the script tag in the head?
  - Yes, but then we must use `defer` or wait for the page to load. Putting it at the end is simpler for now.

### Instructor Tips

- Keep the CSS simple but professional. Students should see that a clean layout is achievable with little code.
- Emphasize the browser console as the best debugging friend.
- Let students experiment with changing values and refreshing.
- Do not worry that the page is static; explain that every interactive app starts this way.

### What NOT to Explain Yet

- DOM manipulation.
- Event listeners.
- Arrays and objects.
- Functions.

---

## 6. Session 2 — Variables, Operators & Strings

### Learning Objectives

1. Use `let` and `const` correctly.
2. Use arithmetic and assignment operators.
3. Work with strings and template literals.
4. Combine strings and numbers in output.
5. Build readable messages in the console with template literals.

### What We Already Have

A static HTML page with one hardcoded task card and a stats section. JavaScript declares a few variables and logs them to the console.

### What We Will Add Today

- Use variables and operators to calculate pending tasks and completion percentage.
- Use template literals to build a welcome message and a task summary string.
- Compare string concatenation with template literals.
- Keep all output in the console for now.

### Problem

"The stats on the page are hardcoded, but JavaScript can already calculate them. We want to calculate the stats in variables and prepare a friendly summary message that we will later show on the page."

### Let Students Guess

- If `totalTasks` is `8` and `completedTasks` is `2`, how do we calculate pending tasks?
- How can we put a variable inside a sentence that we log to the console?
- What happens when we add a number to a string?

### Explain

Operators let us change and combine values. Template literals (backticks with `${}`) let us embed variables directly inside text, which is cleaner than using many `+` signs.

### Live Coding

**script.js additions / changes**

```javascript
// Updated app information
const appName = "FocusTask";
let appVersion = "1.0";

// Hardcoded data for this session
let totalTasks = 8;
let completedTasks = 3;
let pendingTasks = totalTasks - completedTasks;
let completionPercentage = (completedTasks / totalTasks) * 100;

// Build a summary message using a template literal
let summary = `${appName} v${appVersion} — ${pendingTasks} task(s) left out of ${totalTasks}.`;
console.log(summary);

// Format the percentage to 1 decimal place using string concatenation
let percentageMessage = "Completion: " + completionPercentage.toFixed(1) + "%";
console.log(percentageMessage);

// Convert the percentage message to a template literal
let percentageTemplate = `Completion: ${completionPercentage.toFixed(1)}%`;
console.log(percentageTemplate);

// String method example
console.log("App name in uppercase:", appName.toUpperCase());
```

### Student Interaction

- Predict the value of `pendingTasks` and `completionPercentage` before running.
- Convert the `percentageMessage` from string concatenation to a template literal.
- Add a new variable `appSlogan` and log it to the console.

### Mini Challenge

**Challenge: Build a Status Sentence**

Create a variable `userName` with your name. Create variables `totalTasks`, `completedTasks`, and `pendingTasks`. Use a template literal to build the following sentence and display it in the console:

```
Welcome back, [userName]! You have [pendingTasks] pending tasks and [completedTasks] completed tasks out of [totalTasks] total.
```

**Requirements**

- Use `let` for counters.
- Use `const` for the user name.
- Use a template literal (backticks).

**Hints**

- `pendingTasks` can be calculated with subtraction.
- Remember that `${variableName}` goes inside backticks.

**Solution (do not show immediately)**

```javascript
const userName = "Sara";
let totalTasks = 10;
let completedTasks = 4;
let pendingTasks = totalTasks - completedTasks;

console.log(`Welcome back, ${userName}! You have ${pendingTasks} pending tasks and ${completedTasks} completed tasks out of ${totalTasks} total.`);
```

### Checkpoint

1. What operator calculates the remaining tasks?
2. Why is a template literal better than many `+` signs for long sentences?
3. What does `.toFixed(1)` do to a number?
4. What happens if you try to assign a new value to a `const`?
5. Which is better for the app title: `let` or `const`? Why?

### Common Student Mistakes

- Using `+` between a string and a number and expecting math instead of concatenation.
- Forgetting backticks and writing `${variable}` inside normal quotes.
- Confusing `let` and `const`.
- Calling `.toFixed()` on a string.
- Forgetting to refresh the browser after saving.

### Questions Students May Ask

- Can we use `let` for everything?
  - Yes, but `const` is safer for values that should not change and makes the code easier to read.
- Why does `console.log` show `"Completion: 37.5%"` instead of `37.500%`?
  - `.toFixed(1)` keeps one digit after the decimal point and rounds the number.

### Instructor Tips

- Show both string concatenation and template literals so students see why template literals are cleaner.
- Use `console.log` frequently so students can verify calculations before showing them on the page later.
- Keep all output in the console in this session; students should master variables and strings first.

### What NOT to Explain Yet

- Full DOM manipulation with many elements.
- Event listeners.
- Arrays and objects.
- Conditional logic for validation.

---

## 7. Session 3 — Conditions, Logic & Validation

### Learning Objectives

1. Use `if`, `else if`, and `else` to make decisions.
2. Use comparison operators (`>`, `<`, `===`, `!==`, etc.).
3. Use logical operators (`&&`, `||`, `!`).
4. Validate user input with conditions.
5. Display different messages based on data.

### What We Already Have

A static HTML page with one hardcoded task. JavaScript can calculate stats and prepare summary messages in the console.

### What We Will Add Today

- A priority property for each task (`high`, `medium`, `low`).
- Conditional logic to decide whether a task is urgent.
- Conditional logic to choose the correct CSS class name for a priority level (used later).
- Input validation rules for task title length.

### Problem

"Not all tasks are equal. A task with high priority should be treated differently. Also, if a user tries to add an empty task, we should stop them. We need the app to make decisions in JavaScript."

### Let Students Guess

- How can the program know whether a task is high priority?
- What should happen if a task title is empty?
- How can we check two things at once: priority is high AND task is not completed?

### Explain

Programs make decisions with `if` / `else`. Comparison operators compare values, and logical operators combine multiple comparisons. We can use these tools to classify tasks and validate input.

### Live Coding

**script.js additions / changes**

```javascript
// Task information with priority
const appName = "FocusTask";

let totalTasks = 5;
let completedTasks = 2;
let pendingTasks = totalTasks - completedTasks;

let completionPercentage = (completedTasks / totalTasks) * 100;

// Log stats
console.log(`Total: ${totalTasks}, Pending: ${pendingTasks}, Completed: ${completedTasks}`);

// Example task data
let taskTitle = "Finish homework";
let taskPriority = "high";
let taskCompleted = false;

// Decide urgency
let isUrgent = taskPriority === "high" && taskCompleted === false;

if (isUrgent) {
  console.log("This task is urgent!");
} else if (taskPriority === "medium") {
  console.log("This task is important but not urgent.");
} else if (taskPriority === "low") {
  console.log("This task can wait.");
} else {
  console.log("Unknown priority.");
}

// Validation example
let newTaskTitle = "";
let minLength = 3;
let maxLength = 50;

if (newTaskTitle === "") {
  console.log("Error: task title cannot be empty.");
} else if (newTaskTitle.length < minLength) {
  console.log("Error: task title is too short.");
} else if (newTaskTitle.length > maxLength) {
  console.log("Error: task title is too long.");
} else {
  console.log("Task title is valid.");
}

// Priority class message
let priorityClassName = "";
if (taskPriority === "high") {
  priorityClassName = "priority-high";
} else if (taskPriority === "medium") {
  priorityClassName = "priority-medium";
} else {
  priorityClassName = "priority-low";
}

console.log(`Priority class for "${taskTitle}" is ${priorityClassName}.`);
```

**CSS addition (at the bottom of style.css)**

```css
.priority-high {
  border-left: 5px solid #ef4444;
}

.priority-medium {
  border-left: 5px solid #f59e0b;
}

.priority-low {
  border-left: 5px solid #22c55e;
}
```

These classes will be used in Session 5 when we render tasks to the page.

### Student Interaction

- Change `taskPriority` to `"medium"` and predict the output.
- Change `taskCompleted` to `true` and see if the task is still urgent.
- Add a new priority level called `"critical"` before `"high"` and update `priorityClassName` logic.
- Find the missing validation: what if `newTaskTitle` contains only spaces?

### Mini Challenge

**Challenge: Grade the Task Title**

Write a condition that grades a task title:

- If the title is empty, log `"Missing title"`.
- If the title has fewer than 5 characters, log `"Too short"`.
- If the title has more than 40 characters, log `"Too long"`.
- Otherwise, log `"Good title"`.

Test it with these titles: `""`, `"Buy"`, `"Buy milk and eggs from the supermarket before it closes"`, `"Call mom"`.

**Requirements**

- Use `if`, `else if`, and `else`.
- Use `.length` to measure the string.
- Log a different message for each case.

**Hints**

- Compare the length to numbers.
- Start with the empty string check.

**Solution (do not show immediately)**

```javascript
let title = "Call mom";

if (title === "") {
  console.log("Missing title");
} else if (title.length < 5) {
  console.log("Too short");
} else if (title.length > 40) {
  console.log("Too long");
} else {
  console.log("Good title");
}
```

### Checkpoint

1. What is the difference between `=` and `===`?
2. What does `&&` do in a condition?
3. How do we check the length of a string?
4. What message will the console show when `taskPriority` is `"low"`?
5. Why is it important to validate input before saving it?

### Common Student Mistakes

- Using `=` instead of `===` inside an `if` condition.
- Forgetting that `&&` needs both sides to be true.
- Checking `taskPriority = "high"` and accidentally assigning instead of comparing.
- Writing `if (title.length > 40 && < 5)` instead of two separate comparisons.

### Questions Students May Ask

- What is the difference between `==` and `===`?
  - `===` checks both value and type. `==` can convert types and cause surprises, so we use `===`.
- Can we write `if (title)` instead of `if (title !== "")`?
  - Yes, but an empty string is "falsy." We will learn about truthy and falsy values later. For now, be explicit.

### Instructor Tips

- Emphasize the difference between assignment (`=`) and comparison (`===`).
- Use real examples: empty title, too short, too long.
- Show how `&&` and `||` mirror everyday language.
- Explain that the CSS classes chosen today are preparation for styling in Session 5.

### What NOT to Explain Yet

- Truthy and falsy values.
- Ternary operator (`? :`).
- Switch statements.
- Arrays and loops.

---

## 8. Session 4 — Arrays, Objects & Loops

### Learning Objectives

1. Create arrays and objects.
2. Access object properties and array items.
3. Use `for` loops and `for...of` loops.
4. Build an HTML string for a list of items using a loop.
5. Prepare data that can later be rendered on the page.

### What We Already Have

A static HTML page with one hardcoded task. JavaScript can calculate stats, classify a single task, and validate a title from the console.

### What We Will Add Today

- A `tasks` array that contains task objects.
- A loop that counts how many tasks are completed.
- A loop that builds the HTML string for all tasks.
- Output the HTML string in the console so we can render it in Session 5.

### Problem

"A real task manager has many tasks, not one. We cannot hardcode every task in HTML. We need JavaScript to build the full list automatically as an HTML string."

### Let Students Guess

- How would you store 10 tasks in JavaScript?
- How can you repeat the same action for every task?
- What information should each task remember?

### Explain

An array is a list of values. An object stores related properties together. By combining them, we can store many tasks, each with a title, priority, and status. A loop lets us process every task without writing the same code many times. In this session, we will build the HTML string that we will insert into the page in Session 5.

### Live Coding

**script.js — replace the single-task code with an array of objects**

```javascript
const appName = "FocusTask";

// Array of task objects
let tasks = [
  { title: "Buy groceries", priority: "medium", completed: false },
  { title: "Finish homework", priority: "high", completed: false },
  { title: "Walk the dog", priority: "low", completed: true },
  { title: "Call the bank", priority: "high", completed: false },
  { title: "Read 10 pages", priority: "low", completed: true }
];

// Count stats with a for loop
let totalTasks = tasks.length;
let completedTasks = 0;
let pendingTasks = 0;

for (let i = 0; i < tasks.length; i = i + 1) {
  if (tasks[i].completed === true) {
    completedTasks = completedTasks + 1;
  } else {
    pendingTasks = pendingTasks + 1;
  }
}

let completionPercentage = (completedTasks / totalTasks) * 100;

// Build the task list HTML using for...of
let html = "";

for (let task of tasks) {
  let priorityClass = "";
  if (task.priority === "high") {
    priorityClass = "priority-high";
  } else if (task.priority === "medium") {
    priorityClass = "priority-medium";
  } else {
    priorityClass = "priority-low";
  }

  let statusText = "";
  if (task.completed === true) {
    statusText = "completed";
  } else {
    statusText = "pending";
  }

  html += `
    <div class="task-card ${priorityClass}">
      <h3>${task.title}</h3>
      <p>Priority: ${task.priority}</p>
      <p>Status: ${statusText}</p>
    </div>
  `;
}

console.log(`Built HTML for ${totalTasks} tasks:`);
console.log(html);

console.log(`Completion: ${completionPercentage.toFixed(1)}%`);
```

**index.html — remove the hardcoded task card inside `.task-list`**

Replace:

```html
<section class="task-list">
  <h2>My Tasks</h2>
  <div class="task-card">
    <h3>Buy groceries</h3>
    <p>Priority: medium</p>
    <p>Status: pending</p>
  </div>
</section>
```

with:

```html
<section class="task-list">
  <h2>My Tasks</h2>
</section>
```

### Student Interaction

- Add a new task object to the array and predict how the HTML string will change.
- Change one task's `completed` value and see the stat numbers update.
- Convert the `for` loop that counts stats into a `for...of` loop.
- Predict what the HTML string looks like if `tasks` is empty.

### Mini Challenge

**Challenge: Find High-Priority Pending Tasks**

Loop through the `tasks` array and log the title of every task that is high priority AND not completed.

**Requirements**

- Use a `for` loop or `for...of` loop.
- Check both conditions inside the loop.
- Log only the matching titles.

**Hints**

- `task.priority === "high"` checks the priority.
- `task.completed === false` checks the status.
- Use `&&` to combine them.

**Solution (do not show immediately)**

```javascript
for (let task of tasks) {
  if (task.priority === "high" && task.completed === false) {
    console.log(task.title);
  }
}
```

### Checkpoint

1. How do you access the title of the first task in the array?
2. What is the difference between a `for` loop and a `for...of` loop?
3. What does `tasks.length` represent?
4. Why do we build an HTML string inside a loop instead of writing every card by hand?
5. What happens to `completionPercentage` if `totalTasks` is `0`?

### Common Student Mistakes

- Using `tasks.title` instead of `tasks[i].title` inside a `for` loop.
- Forgetting to update the loop counter (`i = i + 1`) and creating an infinite loop.
- Using `for...of` but trying to get the index instead of the value.
- Building the HTML string but never logging or returning it.

### Questions Students May Ask

- Can an array contain objects?
  - Yes, and this is a very common pattern in JavaScript.
- When should I use `for` versus `for...of`?
  - Use `for` when you need the index or want to count. Use `for...of` when you only need each value.

### Instructor Tips

- Draw the array of objects on the board to help students visualize it.
- Show how one small change in data updates the whole HTML string automatically.
- Warn about infinite loops and make sure students save before testing loops.

### What NOT to Explain Yet

- Array methods such as `.map`, `.filter`, `.forEach`, `.find`.
- Destructuring.
- Object methods.
- Event listeners for user input.

---

## 9. Session 5 — Functions & DOM Updates

### Learning Objectives

1. Declare functions with parameters and return values.
2. Split code into small, reusable functions.
3. Select DOM elements and update them.
4. Build HTML strings inside functions.
5. Call functions in the right order.

### What We Already Have

A page with an empty task list container. JavaScript can build a full HTML string for the task list from an array, but it is not yet inserted into the page. The code is also getting long.

### What We Will Add Today

- Functions for `getPriorityClass`, `getStatusText`, `calculateStats`, `buildTaskHTML`, and `renderTasks`.
- Insert the generated HTML string into the page.
- Update the stat cards on the page.
- A more organized script where each function has one job.

### Problem

"Our code builds the HTML string, but it is long and mixed together. We also need to put the string into the page. We will organize the code into functions and finally render the tasks on the page."

### Let Students Guess

- What could be a function in the current code?
- If a function needs information, how does it receive it?
- How can a function give a result back?
- Which HTML element should hold the task list?

### Explain

A function is a reusable block of code. It can accept inputs (parameters) and can return an output. Functions make code easier to read, test, and change. Now that the HTML string is ready, we can use `document.querySelector` and `innerHTML` to place it on the page.

### Live Coding

**script.js — refactor into functions and render to the page**

```javascript
const appName = "FocusTask";

let tasks = [
  { title: "Buy groceries", priority: "medium", completed: false },
  { title: "Finish homework", priority: "high", completed: false },
  { title: "Walk the dog", priority: "low", completed: true },
  { title: "Call the bank", priority: "high", completed: false },
  { title: "Read 10 pages", priority: "low", completed: true }
];

function getPriorityClass(priority) {
  if (priority === "high") {
    return "priority-high";
  } else if (priority === "medium") {
    return "priority-medium";
  } else {
    return "priority-low";
  }
}

function getStatusText(completed) {
  if (completed === true) {
    return "completed";
  } else {
    return "pending";
  }
}

function calculateStats(tasksList) {
  let total = tasksList.length;
  let completed = 0;

  for (let task of tasksList) {
    if (task.completed === true) {
      completed = completed + 1;
    }
  }

  let pending = total - completed;
  let percentage = 0;
  if (total > 0) {
    percentage = (completed / total) * 100;
  }

  return {
    total: total,
    completed: completed,
    pending: pending,
    percentage: percentage
  };
}

function buildTaskHTML(task) {
  let priorityClass = getPriorityClass(task.priority);
  let statusText = getStatusText(task.completed);

  return `
    <div class="task-card ${priorityClass}">
      <h3>${task.title}</h3>
      <p>Priority: ${task.priority}</p>
      <p>Status: ${statusText}</p>
    </div>
  `;
}

function renderTasks(tasksList) {
  let container = document.querySelector(".task-list");
  let html = "";

  for (let task of tasksList) {
    html += buildTaskHTML(task);
  }

  container.innerHTML = html;
}

function updateStatsDisplay(stats) {
  document.getElementById("totalTasks").textContent = stats.total;
  document.getElementById("pendingTasks").textContent = stats.pending;
  document.getElementById("completedTasks").textContent = stats.completed;
}

// Use the functions
let stats = calculateStats(tasks);
updateStatsDisplay(stats);
renderTasks(tasks);

console.log(`${appName} loaded with ${stats.total} tasks.`);
console.log(`Completion: ${stats.percentage.toFixed(1)}%`);
```

### Student Interaction

- Trace what happens when `buildTaskHTML(tasks[0])` is called.
- Add a parameter `dueDate` to `buildTaskHTML` and display it.
- Predict what `calculateStats([])` returns.
- Rename one function and update all calls.

### Mini Challenge

**Challenge: Create a Function That Counts Tasks by Priority**

Write a function `countByPriority(tasksList, priority)` that returns how many tasks have the given priority.

**Requirements**

- Use a loop.
- Return a number.
- Test it with `"high"`, `"medium"`, and `"low"`.

**Hints**

- Start a counter at `0`.
- Compare each task's `priority` to the parameter.

**Solution (do not show immediately)**

```javascript
function countByPriority(tasksList, priority) {
  let count = 0;
  for (let task of tasksList) {
    if (task.priority === priority) {
      count = count + 1;
    }
  }
  return count;
}

console.log("High priority tasks:", countByPriority(tasks, "high"));
```

### Checkpoint

1. What is the purpose of a function parameter?
2. What does `return` do inside a function?
3. Why did we split `calculateStats` and `updateStatsDisplay` into two functions?
4. How do you call the `renderTasks` function with the `tasks` array?
5. What does `container.innerHTML = html` do?

### Common Student Mistakes

- Forgetting to call a function after declaring it.
- Passing the wrong number of arguments.
- Using `return` inside a loop and ending the function too early.
- Naming a function and a variable the same thing.
- Trying to use a variable defined inside a function outside of it.
- Trying to select a container that does not exist in the HTML.

### Questions Students May Ask

- Do we need to use `return` in every function?
  - No. Use `return` when the function needs to give a value back.
- Can a function call another function?
  - Yes, and this is very common. `buildTaskHTML` calls `getPriorityClass`.

### Instructor Tips

- Explain that each function should do one thing well.
- Encourage descriptive function names.
- Show the call stack mentally: `renderTasks` calls `buildTaskHTML` which calls `getPriorityClass`.
- Explain that `innerHTML` replaces the content of the container with the new HTML string.

### What NOT to Explain Yet

- Arrow functions.
- Function expressions.
- Scope and closures.
- Callbacks.
- Higher-order functions.

---

## 10. Session 6 — Events, Forms & User Interaction

### Learning Objectives

1. Add event listeners for `submit` and `click` events.
2. Read values from form inputs.
3. Prevent default form behavior with `preventDefault`.
4. Validate input before adding it to the app.
5. Update the page after user interaction.

### What We Already Have

A page that renders tasks from an array using organized functions. The data is still hardcoded.

### What We Will Add Today

- A form with fields for title and priority.
- An `addTask` function that creates a new task object from the form.
- Click handlers for complete and delete buttons on each task.
- A `validateTask` function that checks title length, valid priority, and duplicate titles.
- Feedback messages for success and errors.

### Problem

"Right now, the tasks are hardcoded. The user cannot add a new task from the page. We need a form and a way to react when the user clicks the Add button."

### Let Students Guess

- What HTML element lets the user type a task title?
- How do we know when the user has submitted the form?
- What should we do before adding the task to the array?
- How do we stop the page from reloading when the form is submitted?

### Explain

Events are signals that something happened, such as a button click or a form submission. We attach an event listener to an element to run code when the event happens. We can read input values and use them to update our data and the page.

### Live Coding

**index.html — add the form and a message area**

Add this inside `<main class="container">` before the stats section:

```html
<section class="form-section">
  <h2>Add New Task</h2>
  <form id="taskForm">
    <input type="text" id="taskTitle" placeholder="Task title" required maxlength="50">
    <select id="taskPriority">
      <option value="low">Low</option>
      <option value="medium" selected>Medium</option>
      <option value="high">High</option>
    </select>
    <button type="submit">Add Task</button>
  </form>
  <p id="formMessage" class="message"></p>
</section>
```

**style.css additions**

```css
.form-section {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 2rem;
}

.form-section h2 {
  margin-bottom: 1rem;
}

#taskForm {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

#taskForm input,
#taskForm select,
#taskForm button {
  padding: 0.5rem;
  font-size: 1rem;
}

#taskForm input {
  flex: 1 1 60%;
}

#taskForm select {
  flex: 1 1 20%;
}

#taskForm button {
  flex: 1 1 15%;
  background-color: var(--primary);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

#taskForm button:hover {
  background-color: #4338ca;
}

.message {
  margin-top: 0.75rem;
  min-height: 1.25rem;
  font-size: 0.875rem;
}

.message.error {
  color: #dc2626;
}

.message.success {
  color: #16a34a;
}

.task-actions {
  margin-top: 0.75rem;
  display: flex;
  gap: 0.5rem;
}

.task-actions button {
  padding: 0.25rem 0.75rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}

.complete-btn {
  background-color: #22c55e;
  color: white;
}

.delete-btn {
  background-color: #ef4444;
  color: white;
}
```

**script.js — add form handling and task actions**

Replace the `buildTaskHTML` and `renderTasks` functions and add the event listeners at the bottom.

```javascript
function buildTaskHTML(task, index) {
  let priorityClass = getPriorityClass(task.priority);
  let statusText = getStatusText(task.completed);

  return `
    <div class="task-card ${priorityClass}" data-index="${index}">
      <h3>${task.title}</h3>
      <p>Priority: ${task.priority}</p>
      <p>Status: ${statusText}</p>
      <div class="task-actions">
        <button class="complete-btn" data-index="${index}">
          ${task.completed ? "Undo" : "Complete"}
        </button>
        <button class="delete-btn" data-index="${index}">Delete</button>
      </div>
    </div>
  `;
}

function renderTasks(tasksList) {
  let container = document.querySelector(".task-list");
  let html = "";

  if (tasksList.length === 0) {
    html = `<p class="empty-state">No tasks yet. Add your first task above!</p>`;
  } else {
    for (let i = 0; i < tasksList.length; i = i + 1) {
      html += buildTaskHTML(tasksList[i], i);
    }
  }

  container.innerHTML = html;
  attachTaskButtonListeners();
}

function attachTaskButtonListeners() {
  let completeButtons = document.querySelectorAll(".complete-btn");
  let deleteButtons = document.querySelectorAll(".delete-btn");

  for (let button of completeButtons) {
    button.addEventListener("click", handleComplete);
  }

  for (let button of deleteButtons) {
    button.addEventListener("click", handleDelete);
  }
}

function handleComplete(event) {
  let index = Number(event.target.getAttribute("data-index"));
  tasks[index].completed = !tasks[index].completed;
  refreshApp();
}

function handleDelete(event) {
  let index = Number(event.target.getAttribute("data-index"));
  tasks.splice(index, 1);
  refreshApp();
}

function addTask(title, priority) {
  let newTask = {
    title: title,
    priority: priority,
    completed: false
  };

  tasks.push(newTask);
}

function validateTask(title, priority, tasksList) {
  let trimmedTitle = title.trim();

  if (trimmedTitle === "") {
    return "Task title cannot be empty.";
  }

  if (trimmedTitle.length < 3) {
    return "Task title must be at least 3 characters.";
  }

  if (trimmedTitle.length > 50) {
    return "Task title must be 50 characters or less.";
  }

  if (priority !== "low" && priority !== "medium" && priority !== "high") {
    return "Please choose a valid priority.";
  }

  for (let task of tasksList) {
    if (task.title.toLowerCase() === trimmedTitle.toLowerCase()) {
      return "A task with this title already exists.";
    }
  }

  return "";
}

function showMessage(text, type) {
  let messageElement = document.getElementById("formMessage");
  messageElement.textContent = text;
  messageElement.className = "message " + type;
}

function refreshApp() {
  let stats = calculateStats(tasks);
  updateStatsDisplay(stats);
  renderTasks(tasks);
}

// Form submission
document.getElementById("taskForm").addEventListener("submit", function (event) {
  event.preventDefault();

  let titleInput = document.getElementById("taskTitle");
  let priorityInput = document.getElementById("taskPriority");

  let title = titleInput.value.trim();
  let priority = priorityInput.value;

  let error = validateTask(title, priority, tasks);

  if (error !== "") {
    showMessage(error, "error");
    return;
  }

  addTask(title, priority);
  titleInput.value = "";
  priorityInput.value = "medium";
  showMessage("Task added successfully!", "success");
  refreshApp();
});

// Initial render
refreshApp();

console.log(`${appName} loaded with interactive form.`);
```

### Student Interaction

- Add a task and watch the stats update.
- Try to submit an empty title, a title with only spaces, and a title shorter than 3 characters.
- Try to add the same task twice and see the duplicate error.
- Click Complete and Delete on tasks.
- Add a new priority option "critical" and update `getPriorityClass` and `validateTask`.

### Mini Challenge

**Challenge: Add a Clear Form Button**

Add a button next to the Add button that clears the title input and resets the priority to `"medium"`. Attach a click event listener to it.

**Requirements**

- Use `addEventListener("click", ...)`.
- Reset the title input to an empty string.
- Reset the select to `"medium"`.

**Hints**

- Select the button by its `id`.
- Set `.value` on the inputs inside the click handler.

**Solution (do not show immediately)**

```html
<button type="button" id="clearForm">Clear</button>
```

```javascript
document.getElementById("clearForm").addEventListener("click", function () {
  document.getElementById("taskTitle").value = "";
  document.getElementById("taskPriority").value = "medium";
  showMessage("", "");
});
```

### Checkpoint

1. What does `event.preventDefault()` do?
2. How do you read the value of an input field in JavaScript?
3. Why do we call `refreshApp()` after adding a task?
4. What is the purpose of `data-index` on the buttons?
5. What is the difference between `click` and `submit` events?

### Common Student Mistakes

- Forgetting `event.preventDefault()` and reloading the page.
- Reading `.value` from the wrong element.
- Not trimming the title before validating or saving it.
- Checking for duplicates without ignoring uppercase/lowercase differences.
- Not re-rendering the list after data changes.
- Using `event.target` without understanding which element was clicked.
- Attaching listeners before the elements exist on the page.

### Questions Students May Ask

- Why do we need `Number()` around `getAttribute`?
  - HTML attributes are strings. We need a number to use as an array index.
- Why do we check for duplicate titles?
  - So users do not accidentally add the same task twice.
- Why do we trim the title before validating?
  - So a title with only spaces is treated as empty.
- What is the difference between `type="submit"` and `type="button"`?
  - `submit` triggers form submission. `button` does nothing by default and is safer for custom actions.

### Instructor Tips

- Show the browser console to confirm that form submission is captured.
- Highlight the data flow: input → validation → array update → render.
- Test every validation case live: empty, too short, too long, only spaces, duplicate, and invalid priority.
- Remind students to re-attach listeners after re-rendering.

### What NOT to Explain Yet

- Event delegation.
- Event bubbling.
- Arrow functions.
- Forms with many fields and complex validation patterns.

---

## 11. Session 7 — Filters, Complete Application Behavior & Final Polish

### Learning Objectives

1. Add filter buttons that control which tasks are displayed.
2. Use conditions to filter an array.
3. Keep the UI in sync with the selected filter.
4. Add final polish such as empty states and active filter highlighting.
5. Review the complete data flow of the application.

### What We Already Have

An interactive task manager with a form, buttons, validation, and dynamic rendering. Tasks are kept in memory during the session.

### What We Will Add Today

- Filter buttons to show All, Pending, or Completed tasks.
- A function that returns only the tasks matching the selected filter.
- Active state styling for the selected filter button.
- A search mini challenge for stronger students.

### Problem

"The app works, but when there are many tasks it becomes hard to focus. Users should be able to see only pending tasks or only completed tasks. We need a way to filter the list."

### Let Students Guess

- Where should the filter buttons go?
- How can we show only pending tasks without deleting the others?
- How do we know which filter is currently active?

### Explain

Filtering means choosing which items from a list to display. We can create a function that looks at each task and decides whether it matches the filter. The `renderTasks` function already takes a list, so we can give it a filtered list instead of all tasks.

### Live Coding

**index.html — add filter buttons after the form section**

```html
<section class="filter-section">
  <button class="filter-btn active" data-filter="all">All</button>
  <button class="filter-btn" data-filter="pending">Pending</button>
  <button class="filter-btn" data-filter="completed">Completed</button>
</section>
```

**style.css additions**

```css
.filter-section {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.filter-btn {
  padding: 0.5rem 1rem;
  border: 1px solid #e2e8f0;
  background-color: white;
  border-radius: 4px;
  cursor: pointer;
}

.filter-btn.active {
  background-color: var(--primary);
  color: white;
  border-color: var(--primary);
}

.empty-state {
  color: var(--muted);
  text-align: center;
  padding: 2rem;
}
```

**script.js — add filtering**

Add these new functions near the other utility functions.

```javascript
let currentFilter = "all";

function getFilteredTasks(filterType) {
  if (filterType === "completed") {
    let result = [];
    for (let task of tasks) {
      if (task.completed === true) {
        result.push(task);
      }
    }
    return result;
  }

  if (filterType === "pending") {
    let result = [];
    for (let task of tasks) {
      if (task.completed === false) {
        result.push(task);
      }
    }
    return result;
  }

  return tasks;
}

function updateFilterButtons(activeFilter) {
  let buttons = document.querySelectorAll(".filter-btn");
  for (let button of buttons) {
    if (button.getAttribute("data-filter") === activeFilter) {
      button.classList.add("active");
    } else {
      button.classList.remove("active");
    }
  }
}
```

Update `refreshApp` to use filtering.

```javascript
function refreshApp() {
  let stats = calculateStats(tasks);
  updateStatsDisplay(stats);

  let visibleTasks = getFilteredTasks(currentFilter);
  renderTasks(visibleTasks);
  updateFilterButtons(currentFilter);
}
```

Add filter button listeners and render the app.

```javascript
// Filter buttons
let filterButtons = document.querySelectorAll(".filter-btn");
for (let i = 0; i < filterButtons.length; i = i + 1) {
  filterButtons[i].addEventListener("click", function () {
    currentFilter = filterButtons[i].getAttribute("data-filter");
    refreshApp();
  });
}

// Initial render
refreshApp();

console.log(`${appName} loaded with filters.`);
```

### Student Interaction

- Switch between All, Pending, and Completed filters.
- Delete all tasks and see the empty state message.
- Add a task while the Pending filter is active and predict whether it appears.
- Predict what `getFilteredTasks("unknown")` returns.

### Mini Challenge

**Challenge: Add a Search Filter**

Add an input field where the user can type a word. Only tasks whose title contains that word should be shown.

**Requirements**

- Add an `<input type="text" id="searchInput" placeholder="Search tasks...">`.
- Attach an `input` event listener.
- Update the task list as the user types.

**Hints**

- `task.title.includes(searchText)` checks if a title contains the search text.
- Use `.toLowerCase()` on both sides to make the search case-insensitive.

**Solution (do not show immediately)**

```javascript
document.getElementById("searchInput").addEventListener("input", function (event) {
  let searchText = event.target.value.toLowerCase();
  let container = document.querySelector(".task-list");
  let html = "";

  for (let i = 0; i < tasks.length; i = i + 1) {
    if (tasks[i].title.toLowerCase().includes(searchText)) {
      html += buildTaskHTML(tasks[i], i);
    }
  }

  if (html === "") {
    html = `<p class="empty-state">No matching tasks.</p>`;
  }

  container.innerHTML = html;
  attachTaskButtonListeners();
});
```

### Checkpoint

1. What does `getFilteredTasks("completed")` return?
2. Why do we keep the original `tasks` array and only filter what we display?
3. How does the app know which filter button is active?
4. What happens if the user deletes the last pending task while the Pending filter is active?
5. What are two ways we could combine the filter and the search feature?

### Common Student Mistakes

- Filtering the original `tasks` array and losing data.
- Not calling `updateFilterButtons` after the filter changes.
- Returning `tasks` by mistake when the filter should be empty.
- Forgetting to call `refreshApp()` after deleting or adding a task.

### Questions Students May Ask

- Will the filter change the data permanently?
  - No. Filtering only chooses what to display. The `tasks` array stays the same.
- Can we have more than one active filter?
  - Yes, but it needs more logic. For now, we keep one active filter.

### Instructor Tips

- Show how the same `renderTasks` function works with any list: full, filtered, or searched.
- Emphasize that filtering is a display concern, not a data change.
- Encourage students to trace the flow: button click → `currentFilter` changes → `getFilteredTasks` → `renderTasks`.

### What NOT to Explain Yet

- Multiple simultaneous filters.
- Servers and databases.
- APIs.
- `localStorage` or cookies.

---

## 12. Final Instructor Project

### Complete Features

- Add tasks with title and priority.
- Mark tasks as complete or undo.
- Delete tasks.
- Filter tasks by All / Pending / Completed.
- Search tasks by title.
- Validate form input.
- Display real-time statistics.
- Show empty states and feedback messages.
- Responsive layout with clean CSS.

### Folder Structure

```
focustask/
├── index.html
├── style.css
└── script.js
```

### Complete HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FocusTask</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header class="app-header">
    <h1>FocusTask</h1>
    <p>Your simple task manager</p>
  </header>

  <main class="container">
    <section class="form-section">
      <h2>Add New Task</h2>
      <form id="taskForm">
        <input type="text" id="taskTitle" placeholder="Task title" required maxlength="50">
        <select id="taskPriority">
          <option value="low">Low</option>
          <option value="medium" selected>Medium</option>
          <option value="high">High</option>
        </select>
        <button type="submit">Add Task</button>
      </form>
      <p id="formMessage" class="message"></p>
    </section>

    <section class="stats">
      <div class="stat-card">
        <span class="stat-label">Total</span>
        <span class="stat-value" id="totalTasks">0</span>
      </div>
      <div class="stat-card">
        <span class="stat-label">Pending</span>
        <span class="stat-value" id="pendingTasks">0</span>
      </div>
      <div class="stat-card">
        <span class="stat-label">Completed</span>
        <span class="stat-value" id="completedTasks">0</span>
      </div>
    </section>

    <section class="filter-section">
      <button class="filter-btn active" data-filter="all">All</button>
      <button class="filter-btn" data-filter="pending">Pending</button>
      <button class="filter-btn" data-filter="completed">Completed</button>
    </section>

    <section class="task-list">
      <h2>My Tasks</h2>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
```

### Complete CSS

```css
:root {
  --primary: #4f46e5;
  --bg: #f8fafc;
  --card-bg: #ffffff;
  --text: #1e293b;
  --muted: #64748b;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: var(--bg);
  color: var(--text);
  line-height: 1.5;
}

.app-header {
  background-color: var(--primary);
  color: white;
  padding: 1.5rem;
  text-align: center;
}

.container {
  max-width: 720px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.form-section {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 2rem;
}

.form-section h2 {
  margin-bottom: 1rem;
}

#taskForm {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

#taskForm input,
#taskForm select,
#taskForm button {
  padding: 0.5rem;
  font-size: 1rem;
}

#taskForm input {
  flex: 1 1 60%;
}

#taskForm select {
  flex: 1 1 20%;
}

#taskForm button {
  flex: 1 1 15%;
  background-color: var(--primary);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

#taskForm button:hover {
  background-color: #4338ca;
}

.message {
  margin-top: 0.75rem;
  min-height: 1.25rem;
  font-size: 0.875rem;
}

.message.error {
  color: #dc2626;
}

.message.success {
  color: #16a34a;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-bottom: 2rem;
}

.stat-card {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  text-align: center;
}

.stat-label {
  display: block;
  color: var(--muted);
  font-size: 0.875rem;
}

.stat-value {
  display: block;
  font-size: 1.5rem;
  font-weight: bold;
}

.filter-section {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.filter-btn {
  padding: 0.5rem 1rem;
  border: 1px solid #e2e8f0;
  background-color: white;
  border-radius: 4px;
  cursor: pointer;
}

.filter-btn.active {
  background-color: var(--primary);
  color: white;
  border-color: var(--primary);
}

.task-list h2 {
  margin-bottom: 1rem;
}

.task-card {
  background: var(--card-bg);
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
}

.priority-high {
  border-left: 5px solid #ef4444;
}

.priority-medium {
  border-left: 5px solid #f59e0b;
}

.priority-low {
  border-left: 5px solid #22c55e;
}

.task-actions {
  margin-top: 0.75rem;
  display: flex;
  gap: 0.5rem;
}

.task-actions button {
  padding: 0.25rem 0.75rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}

.complete-btn {
  background-color: #22c55e;
  color: white;
}

.delete-btn {
  background-color: #ef4444;
  color: white;
}

.empty-state {
  color: var(--muted);
  text-align: center;
  padding: 2rem;
}

@media (max-width: 480px) {
  .stats {
    grid-template-columns: 1fr;
  }

  #taskForm input,
  #taskForm select,
  #taskForm button {
    flex: 1 1 100%;
  }
}
```

### Complete JavaScript

```javascript
const appName = "FocusTask";

let tasks = [
  { title: "Buy groceries", priority: "medium", completed: false },
  { title: "Finish homework", priority: "high", completed: false },
  { title: "Walk the dog", priority: "low", completed: true }
];

let currentFilter = "all";

function getPriorityClass(priority) {
  if (priority === "high") {
    return "priority-high";
  } else if (priority === "medium") {
    return "priority-medium";
  } else {
    return "priority-low";
  }
}

function getStatusText(completed) {
  if (completed === true) {
    return "completed";
  } else {
    return "pending";
  }
}

function calculateStats(tasksList) {
  let total = tasksList.length;
  let completed = 0;

  for (let task of tasksList) {
    if (task.completed === true) {
      completed = completed + 1;
    }
  }

  let pending = total - completed;
  let percentage = 0;
  if (total > 0) {
    percentage = (completed / total) * 100;
  }

  return {
    total: total,
    completed: completed,
    pending: pending,
    percentage: percentage
  };
}

function buildTaskHTML(task, index) {
  let priorityClass = getPriorityClass(task.priority);
  let statusText = getStatusText(task.completed);

  return `
    <div class="task-card ${priorityClass}" data-index="${index}">
      <h3>${task.title}</h3>
      <p>Priority: ${task.priority}</p>
      <p>Status: ${statusText}</p>
      <div class="task-actions">
        <button class="complete-btn" data-index="${index}">
          ${task.completed ? "Undo" : "Complete"}
        </button>
        <button class="delete-btn" data-index="${index}">Delete</button>
      </div>
    </div>
  `;
}

function renderTasks(tasksList) {
  let container = document.querySelector(".task-list");
  let html = "";

  if (tasksList.length === 0) {
    html = `<p class="empty-state">No tasks found.</p>`;
  } else {
    for (let i = 0; i < tasksList.length; i = i + 1) {
      html += buildTaskHTML(tasksList[i], i);
    }
  }

  container.innerHTML = html;
  attachTaskButtonListeners();
}

function attachTaskButtonListeners() {
  let completeButtons = document.querySelectorAll(".complete-btn");
  let deleteButtons = document.querySelectorAll(".delete-btn");

  for (let button of completeButtons) {
    button.addEventListener("click", handleComplete);
  }

  for (let button of deleteButtons) {
    button.addEventListener("click", handleDelete);
  }
}

function handleComplete(event) {
  let index = Number(event.target.getAttribute("data-index"));
  tasks[index].completed = !tasks[index].completed;
  refreshApp();
}

function handleDelete(event) {
  let index = Number(event.target.getAttribute("data-index"));
  tasks.splice(index, 1);
  refreshApp();
}

function validateTask(title, priority, tasksList) {
  let trimmedTitle = title.trim();

  if (trimmedTitle === "") {
    return "Task title cannot be empty.";
  }

  if (trimmedTitle.length < 3) {
    return "Task title must be at least 3 characters.";
  }

  if (trimmedTitle.length > 50) {
    return "Task title must be 50 characters or less.";
  }

  if (priority !== "low" && priority !== "medium" && priority !== "high") {
    return "Please choose a valid priority.";
  }

  for (let task of tasksList) {
    if (task.title.toLowerCase() === trimmedTitle.toLowerCase()) {
      return "A task with this title already exists.";
    }
  }

  return "";
}

function addTask(title, priority) {
  let newTask = {
    title: title,
    priority: priority,
    completed: false
  };

  tasks.push(newTask);
}

function showMessage(text, type) {
  let messageElement = document.getElementById("formMessage");
  messageElement.textContent = text;
  messageElement.className = "message " + type;
}

function updateStatsDisplay(stats) {
  document.getElementById("totalTasks").textContent = stats.total;
  document.getElementById("pendingTasks").textContent = stats.pending;
  document.getElementById("completedTasks").textContent = stats.completed;
}

function getFilteredTasks(filterType) {
  if (filterType === "completed") {
    let result = [];
    for (let task of tasks) {
      if (task.completed === true) {
        result.push(task);
      }
    }
    return result;
  }

  if (filterType === "pending") {
    let result = [];
    for (let task of tasks) {
      if (task.completed === false) {
        result.push(task);
      }
    }
    return result;
  }

  return tasks;
}

function updateFilterButtons(activeFilter) {
  let buttons = document.querySelectorAll(".filter-btn");
  for (let button of buttons) {
    if (button.getAttribute("data-filter") === activeFilter) {
      button.classList.add("active");
    } else {
      button.classList.remove("active");
    }
  }
}

function refreshApp() {
  let stats = calculateStats(tasks);
  updateStatsDisplay(stats);

  let visibleTasks = getFilteredTasks(currentFilter);
  renderTasks(visibleTasks);
  updateFilterButtons(currentFilter);
}

// Form submission
document.getElementById("taskForm").addEventListener("submit", function (event) {
  event.preventDefault();

  let titleInput = document.getElementById("taskTitle");
  let priorityInput = document.getElementById("taskPriority");

  let title = titleInput.value.trim();
  let priority = priorityInput.value;

  let error = validateTask(title, priority, tasks);

  if (error !== "") {
    showMessage(error, "error");
    return;
  }

  addTask(title, priority);
  titleInput.value = "";
  priorityInput.value = "medium";
  showMessage("Task added successfully!", "success");
  refreshApp();
});

// Filter buttons
let filterButtons = document.querySelectorAll(".filter-btn");
for (let i = 0; i < filterButtons.length; i = i + 1) {
  filterButtons[i].addEventListener("click", function () {
    currentFilter = filterButtons[i].getAttribute("data-filter");
    refreshApp();
  });
}

// Initial render
refreshApp();

console.log(`${appName} loaded with ${tasks.length} tasks.`);
```

### How the Data Works

- Tasks are stored in one array: `tasks`.
- Each task is an object with `title`, `priority`, and `completed`.
- The app works with the data in memory during the session.

### How the UI Works

- HTML provides the structure: header, form, stats, filters, and task list container.
- CSS controls layout, colors, spacing, and responsive behavior.
- JavaScript builds task cards and inserts them into the task list container.

### How JavaScript Controls the UI

- `refreshApp()` is the main coordinator. It calculates stats, filters tasks, renders the list, and updates filter buttons.
- Form submission calls `addTask`, then `refreshApp`.
- Complete and delete buttons call their handlers, which update the data and call `refreshApp`.

### Final User Flow

1. User opens the page and sees the default tasks.
2. User types a title, chooses a priority, and clicks Add Task.
3. Validation runs; if the input is valid, the task appears in the list.
4. Stats update instantly.
5. User clicks Complete or Delete on any task.
6. User switches filters to view All, Pending, or Completed tasks.

---

## 13. Final Student Assignment

### Project Description

Students build **EventHorizon**, a personal Event Management App. Instead of tasks, users manage events they want to attend or organize. The app uses the same concepts as FocusTask but requires students to make more decisions about structure, data, and features.

### Required Features (Level 1)

1. Display a list of events with title, date, category, and status.
2. Add a new event through a form with title, date, category, and location.
3. Validate that the title is not empty and the date is not in the past.
4. Mark an event as "Upcoming", "Today", or "Past" based on the date.
5. Mark an event as attended or cancelled.
6. Delete an event.
7. Filter events by category and by status (All / Upcoming / Past).
8. Show a count of total, upcoming, and attended events.
9. Show empty states and feedback messages.
10. Use semantic HTML and responsive CSS.
11. Build the project with Vanilla JavaScript only.

### Optional Features (Level 2 and 3)

1. Edit an existing event.
2. Sort events by date.
3. Search events by title or location.
4. Show a countdown (e.g., "3 days left") for upcoming events.
5. Export the event list as a simple text summary.
6. Add a dark mode toggle.
7. Group events by month or category.
8. Add a simple RSVP counter for each event.

### Technical Requirements

- Use arrays and objects to store events.
- Use `let` and `const` appropriately.
- Use arithmetic and comparison operators.
- Use `if` / `else` for validation and classification.
- Use `for` loops or `for...of` loops.
- Use functions with parameters and return values.
- Use `document.getElementById` and `document.querySelector`.
- Use event listeners for form submission and button clicks.
- Do not use frameworks, classes, modules, or external APIs.

### UI Requirements

- Header with app name and short description.
- Event form with inputs for title, date, category, and location.
- Filter controls for category and status.
- Statistics cards (total, upcoming, attended).
- Event cards with title, date, category, location, and status.
- Action buttons for each event (attended, cancelled, delete, edit if implemented).
- Empty state message when no events exist.
- Error and success messages after form submission.
- Responsive layout that works on mobile.

### User Stories

- As a user, I want to add an event so I can keep track of my plans.
- As a user, I want to see if an event is upcoming, today, or past so I know what is relevant.
- As a user, I want to mark an event as attended or cancelled so I can record the outcome.
- As a user, I want to delete an event so I can remove plans that are no longer needed.
- As a user, I want to filter events by category so I can focus on one type of activity.
- As a user, I want to see clear messages when I make a mistake in the form.

### Acceptance Criteria

- The form cannot submit an empty title.
- The date input must be valid and not earlier than today for new events.
- Each event card displays the correct status badge.
- Stats update automatically when events are added, changed, or deleted.
- Filters show the correct subset of events.
- The app works without errors on desktop and mobile.

---

## 14. Assignment Requirements

### Project Description

Build **EventHorizon**, an event management web application. Users can add, view, classify, filter, and delete events.

### Required Features

1. Add an event with title, date, category, and location.
2. Display events as cards in a responsive list.
3. Classify each event as Upcoming, Today, or Past.
4. Mark events as attended or cancelled.
5. Delete events.
6. Filter by status (All, Upcoming, Today, Past).
7. Filter by category (Work, Personal, Social, Other).
8. Show statistics: total, upcoming, attended.
9. Validate form input.
10. Show empty states and messages.
11. Use only HTML, CSS, and Vanilla JavaScript.

### Optional Features

1. Edit events.
2. Sort events by date.
3. Search by title or location.
4. Countdown display for upcoming events.
5. Export event list as text.

### Technical Requirements

- Arrays and objects.
- Functions.
- Loops.
- Conditions.
- DOM manipulation.
- Form submission handling.
- Input validation.
- No frameworks.

### UI Requirements

- Clean, responsive layout.
- Form with at least four inputs.
- Filter buttons or dropdowns.
- Statistics section.
- Event cards with status badges.
- Empty and error states.

### User Stories

- As a user, I want to add events easily.
- As a user, I want to see upcoming events first.
- As a user, I want to mark events as attended or cancelled.
- As a user, I want to filter by category.
- As a user, I want clear validation messages when I make a mistake.

### Acceptance Criteria

- Adding an event updates the list and stats.
- Invalid input shows a clear error.
- Event status is calculated from the date.
- Filters display the correct events.
- The app works without errors on desktop and mobile.

---

## 15. Difficulty Levels

### Level 1 — Required

- Basic HTML structure and styling.
- Add and display events.
- Mark events as attended/cancelled.
- Delete events.
- Basic validation.
- Basic stats.

### Level 2 — Intermediate

- Filter events by status and category.
- Classify events as Upcoming / Today / Past.
- Responsive CSS.
- Empty states and messages.
- Clean function organization.

### Level 3 — Challenge

- Edit events.
- Sort events by date.
- Search functionality.
- Countdown display.
- Export summary.
- Extra polish such as themes or animations.

---

## 16. Grading Rubric

| Category | Points | Description |
| -------- | ------ | ----------- |
| HTML structure | 10 | Semantic, valid, accessible structure. |
| CSS / UI | 15 | Clean layout, responsive design, visual hierarchy. |
| JavaScript fundamentals | 25 | Correct use of variables, operators, conditionals, loops, and filter logic. |
| DOM manipulation | 15 | Correct selection, creation, and updating of elements. |
| Events / forms | 15 | Proper event handling, input reading, and validation. |
| Filtering / search | 10 | Correct filter logic and active state handling. |
| Code organization | 5 | Functions are clear and code is readable. |
| User experience | 5 | Empty states, feedback messages, intuitive flow. |
| Extra features | 5 | Optional features or polish beyond requirements. |
| **Total** | **100** | |

---

## 17. Common Mistakes

### Across All Sessions

- Forgetting to refresh the browser after saving a file.
- Mixing up `=` (assignment) and `===` (comparison).
- Forgetting that `console.log` goes to the browser console, not the page.
- Naming variables with spaces or starting with numbers.
- Not closing quotes, parentheses, or braces.

### Session 1-2

- Using `let` for values that never change.
- Trying to reassign a `const`.
- Adding a number to a string and expecting math.
- Forgetting backticks when using template literals.

### Session 3

- Using `=` inside an `if` condition.
- Confusing `&&` and `||`.
- Checking `title.length > 40 && < 5` instead of separate conditions.

### Session 4

- Accessing `tasks.title` instead of `tasks[i].title`.
- Creating infinite loops by forgetting the counter update.
- Not handling an empty array.

### Session 5

- Declaring a function but never calling it.
- Forgetting to `return` a value.
- Returning inside a loop too early.
- Trying to select a container that does not exist in the HTML.

### Session 6

- Forgetting `event.preventDefault()`.
- Reading `.value` from the wrong element.
- Not trimming the title before validating or saving it.
- Checking for duplicates without ignoring uppercase/lowercase differences.
- Not re-rendering the list after data changes.

### Session 7

- Filtering the original `tasks` array and losing data.
- Not updating the active filter button styling.
- Forgetting to call `refreshApp()` after data changes.

---

## 18. Instructor Tips

### General Teaching

- Follow the pattern: Problem → Guess → Explain → Live Code → Test → Break the Code → Debug → Challenge → Review.
- Never explain theory for more than 15 minutes without an activity.
- Use the browser console constantly.
- Let students predict before you run the code.
- Celebrate small wins at every session.

### Per Session

- **Session 1:** Focus on structure. Let students see a real page even though JavaScript only logs to the console.
- **Session 2:** Show the power of template literals. Make students convert concatenation to templates.
- **Session 3:** Use real validation examples. Ask "what could go wrong?"
- **Session 4:** Draw the array of objects. Visualize the loop that builds the HTML string.
- **Session 5:** Refactor live. Show before and after code side by side, then insert the HTML string into the page.
- **Session 6:** Pause after each event handler. Test the form many times.
- **Session 7:** Show how the same `renderTasks` function works with full or filtered lists. Emphasize that filters are display-only.

### What to Emphasize

- One project grows over seven sessions.
- Data flows from variables → arrays → functions → DOM.
- Functions keep code clean.
- Validation protects the app from bad data.
- Filtering does not change the underlying data.

### What NOT to Explain Yet

- Arrow functions.
- Classes and object-oriented patterns.
- Modules and imports.
- `async/await` and APIs.
- Advanced array methods such as `.map`, `.filter`, `.reduce`.
- Frameworks (React, Vue, Angular).
- Complex regular expressions.
- `localStorage`, cookies, or databases.

---

## 19. Summary

This curriculum builds **FocusTask** from a static page into a fully interactive task manager using only concepts from JavaScript Sessions 1-7. Each session adds one layer of behavior, and the final student assignment **EventHorizon** extends the same ideas into a larger, independent project. The focus is on active learning, visible progress, and a polished final product.
