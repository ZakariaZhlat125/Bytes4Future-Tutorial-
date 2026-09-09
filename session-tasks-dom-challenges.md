# Session Tasks: DOM Integration Challenges — Sessions 1–10 Review

## 🎯 Session Overview

**Total Duration:** 2.5 hours (5 tasks × 30 minutes)
**Level:** Intermediate (requires Sessions 1–10)
**Goal:** Combine everything you learned — variables, data types, operators, conditionals, arrays, loops, functions, scope, arrow functions, HOFs, objects, and DOM manipulation — into real, interactive web apps.

---

## 📋 What You Need Before Starting

You must know how to (from previous sessions):

- Declare variables (`let`, `const`) and use data types (Session 2)
- Use operators, template literals, and string methods (Session 3)
- Write `if/else`, `switch`, and use `===` comparisons (Session 4)
- Create and manipulate arrays with `push`, `map`, `filter`, `reduce` (Sessions 5 & 8)
- Write `for`, `for...of`, `forEach` loops (Session 6)
- Declare and call functions with parameters and `return` (Session 7)
- Use arrow functions and higher-order functions (Session 8)
- Create objects, use dot/bracket notation, and object methods (Session 9)
- Select elements, change content, create elements, and handle events with the DOM (Session 10)

---

## 📝 Session Rules

1. **No solutions are provided.** You must build each task yourself.
2. **Each task = 30 minutes.** Manage your time strictly.
3. **Every task must use the DOM.** Output goes to the page, not just the console.
4. **Reuse previous knowledge.** Each task combines multiple sessions.
5. **Test in the browser.** Open DevTools (F12) and check the Console for errors.
6. **One HTML file per task.** Create `task1.html`, `task2.html`, etc.

---

## 🗂️ Folder Structure

```
session-tasks/
├── task1.html
├── task2.html
├── task3.html
├── task4.html
└── task5.html
```

Each HTML file contains its own `<style>` and `<script>` tags (inline) so it is self-contained.

---

## 🚀 Task 1: Dynamic Product Catalog (30 minutes)

**Combines:** Session 5 (Arrays), Session 9 (Objects), Session 6 (Loops), Session 10 (DOM)

### 🎯 Problem

You run an online store. You have a list of products stored as an array of objects. You must display them on the page as cards, and let the user add a product to a cart by clicking a button.

### 📋 Requirements

1. Create an array called `products` containing at least **5 product objects**. Each object must have:
   - `id` (number)
   - `name` (string)
   - `price` (number)
   - `category` (string)
   - `inStock` (boolean)

2. Create a container `<div id="catalog"></div>` in the HTML.

3. Use a **loop** (`for...of` or `forEach`) to generate a product card for each product and append it to `#catalog`. Each card must show:
   - The product name
   - The price formatted like `$999.99` (use `toFixed(2)`)
   - The category
   - A stock badge: `"In Stock"` (green) if `inStock` is `true`, otherwise `"Out of Stock"` (red)
   - An `"Add to Cart"` button

4. Maintain a `cart` array. When the user clicks `"Add to Cart"`:
   - Push the product object into the `cart` array
   - Update a `<span id="cartCount">` to show the number of items in the cart

5. Use **template literals** to build each card's HTML.

### ✅ Acceptance Criteria

- [ ] 5 product cards appear on page load
- [ ] Prices show 2 decimals
- [ ] Stock badge color is correct
- [ ] Clicking "Add to Cart" increases the cart count
- [ ] No errors in the console

### 💡 Hints

- Use `document.createElement` + `appendChild`, OR build a template string and set `innerHTML`.
- Use `addEventListener("click", ...)` on each button.
- The cart count is just `cart.length`.

---

## 🚀 Task 2: Interactive Quiz App (30 minutes)

**Combines:** Session 4 (Conditionals), Session 9 (Objects), Session 7 (Functions), Session 10 (DOM)

### 🎯 Problem

Build a multiple-choice quiz. The user answers questions one by one. At the end, show the final score and a message based on the score.

### 📋 Requirements

1. Create an array called `questions` with at least **4 question objects**. Each object must have:
   - `question` (string)
   - `choices` (array of 4 strings)
   - `answer` (number — the index of the correct choice, 0–3)

2. Create this HTML structure:
   ```html
   <div id="quiz">
     <h2 id="questionText"></h2>
     <div id="choices"></div>
     <button id="next">Next</button>
   </div>
   <div id="result"></div>
   ```

3. Write a function `showQuestion(index)` that:
   - Displays the current question text in `#questionText`
   - Creates 4 buttons in `#choices`, one for each choice
   - Stores the user's selected answer in a variable

4. Write a function `checkAnswer()` that:
   - Compares the selected answer with `questions[currentIndex].answer`
   - Increments `score` if correct
   - Moves to the next question, OR shows the result if it was the last question

5. When the quiz ends, display in `#result`:
   - The final score out of the total (e.g., `"You scored 3/4"`)
   - A message using **conditionals**:
     - `score === total` → `"Perfect! 🏆"`
     - `score >= total / 2` → `"Good job! 👍"`
     - otherwise → `"Keep practicing! 📚"`

6. Use `let` for the current question index and `let` for the score. Use `const` for the questions array.

### ✅ Acceptance Criteria

- [ ] First question appears on page load
- [ ] Clicking a choice highlights or stores the answer
- [ ] "Next" button moves to the next question
- [ ] Final score and message appear after the last question
- [ ] The message changes based on the score

### 💡 Hints

- Keep `currentIndex` and `score` as variables outside the functions (global scope).
- Disable the "Next" button until the user picks an answer.
- Use `if / else if / else` for the final message.

---

## 🚀 Task 3: Todo List with Filters (30 minutes)

**Combines:** Session 5 (Arrays), Session 8 (HOFs: filter/map), Session 6 (Loops), Session 10 (DOM)

### 🎯 Problem

Build a todo list app. The user can add tasks, mark them as done, delete them, and filter them by status (all / active / completed).

### 📋 Requirements

1. Create an array called `todos`. Each todo is an object with:
   - `id` (number — use a counter or `Date.now()`)
   - `text` (string)
   - `done` (boolean, starts `false`)

2. Create this HTML structure:
   ```html
   <input id="todoInput" placeholder="Add a task..." />
   <button id="addBtn">Add</button>
   <div id="filters">
     <button data-filter="all">All</button>
     <button data-filter="active">Active</button>
     <button data-filter="completed">Completed</button>
   </div>
   <ul id="todoList"></ul>
   <p id="count"></p>
   ```

3. Write a function `render()` that:
   - Uses `filter()` to select todos based on the current filter (`"all"`, `"active"`, `"completed"`)
   - Uses a loop to create an `<li>` for each visible todo
   - Each `<li>` shows the todo text, a `"Done"` toggle button, and a `"Delete"` button
   - Done todos should have a CSS class that strikes through the text (`text-decoration: line-through`)
   - Updates `#count` to show: `"X active, Y completed"`

4. Write these functions:
   - `addTodo()` — reads `#todoInput` value, creates a new todo object, pushes it, calls `render()`
   - `toggleTodo(id)` — flips the `done` property of the matching todo, calls `render()`
   - `deleteTodo(id)` — removes the matching todo using `filter()`, calls `render()`

5. Wire up the filter buttons: clicking one sets the current filter and calls `render()`.

6. Use `addEventListener` for all buttons. Use **arrow functions** for the callbacks.

### ✅ Acceptance Criteria

- [ ] Typing a task and clicking "Add" adds it to the list
- [ ] Clicking "Done" toggles the strike-through
- [ ] Clicking "Delete" removes the task
- [ ] Filter buttons show only the matching todos
- [ ] The count updates correctly
- [ ] Empty input does not add an empty todo

### 💡 Hints

- Use `todos.filter(t => t.id !== id)` to delete.
- Use `todos.map(t => t.id === id ? { ...t, done: !t.done } : t)` to toggle, OR mutate directly.
- Store the current filter in a variable like `let currentFilter = "all";`.

---

## 🚀 Task 4: Student Grades Dashboard (30 minutes)

**Combines:** Session 9 (Objects), Session 5 (Arrays), Session 8 (reduce), Session 4 (Conditionals), Session 10 (DOM)

### 🎯 Problem

You are a teacher. You have an array of students with their grades. Build a dashboard that displays all students, their average grade, their pass/fail status, and class statistics.

### 📋 Requirements

1. Create an array called `students` with at least **5 student objects**. Each object must have:
   - `name` (string)
   - `grades` (array of numbers, e.g., `[85, 90, 78]`)

2. Create this HTML structure:
   ```html
   <div id="stats"></div>
   <table id="table">
     <thead>
       <tr><th>Name</th><th>Average</th><th>Status</th></tr>
     </thead>
     <tbody id="tbody"></tbody>
   </table>
   ```

3. Write a function `getAverage(grades)` that uses `reduce()` to calculate the average of a grades array. Return the result rounded to 1 decimal (use `toFixed(1)`).

4. Write a function `getStatus(average)` that returns:
   - `"Pass"` if average >= 60
   - `"Fail"` if average < 60
   - Use a **ternary operator**.

5. Write a function `renderTable()` that:
   - Loops through `students`
   - For each student, creates a `<tr>` with the name, average, and status
   - Status cells should be colored: green for "Pass", red for "Fail" (use `style.color`)
   - Appends each row to `#tbody`

6. Write a function `renderStats()` that uses `reduce()` to calculate and display in `#stats`:
   - Number of students
   - Class average (average of all students' averages)
   - Number of passing students
   - Number of failing students
   - Highest average student name
   - Lowest average student name

7. Call both functions on page load.

### ✅ Acceptance Criteria

- [ ] Table shows all 5 students with name, average, and status
- [ ] Pass/Fail colors are correct
- [ ] Stats section shows all 6 statistics
- [ ] Averages have 1 decimal
- [ ] `reduce()` is used for averages and counts

### 💡 Hints

- `grades.reduce((sum, g) => sum + g, 0) / grades.length` gives the average.
- For the class average, you can map each student to their average, then reduce.
- Use `Math.max` / `Math.min` with the spread operator, or reduce manually.

---

## 🚀 Task 5: Registration Form with Live Validation (30 minutes)

**Combines:** Session 4 (Conditionals), Session 3 (Operators & Strings), Session 7 (Functions), Session 9 (Objects), Session 10 (DOM)

### 🎯 Problem

Build a registration form that validates user input **live** (as the user types) and shows error messages under each field. Only enable the submit button when all fields are valid.

### 📋 Requirements

1. Create this HTML structure:
   ```html
   <form id="regForm">
     <input id="username" placeholder="Username (min 3 chars)" />
     <span id="usernameError" class="error"></span>

     <input id="email" placeholder="Email" />
     <span id="emailError" class="error"></span>

     <input id="password" type="password" placeholder="Password (min 6 chars)" />
     <span id="passwordError" class="error"></span>

     <input id="confirm" type="password" placeholder="Confirm password" />
     <span id="confirmError" class="error"></span>

     <button id="submitBtn" disabled>Register</button>
   </form>
   <div id="success"></div>
   ```

2. Create an object called `valid` that tracks each field's validity:
   ```javascript
   const valid = { username: false, email: false, password: false, confirm: false };
   ```

3. Write these **validation functions** (each returns `true` or `false`):
   - `validateUsername(value)` — must be at least 3 characters, only letters and numbers (use a string method or a loop to check each character)
   - `validateEmail(value)` — must contain `@` and `.` and the `@` must come before the last `.`
   - `validatePassword(value)` — must be at least 6 characters
   - `validateConfirm(value)` — must match the password field value

4. Add `input` event listeners to each field. On every keystroke:
   - Call the matching validation function
   - If invalid, show an error message in the matching `<span>` and set its `valid` entry to `false`
   - If valid, clear the error message and set its `valid` entry to `true`
   - Call `checkAllValid()`

5. Write `checkAllValid()` that:
   - Checks if all 4 entries in `valid` are `true`
   - Enables the submit button only if all are valid (set `button.disabled = !allValid`)

6. On form submit (`submit` event):
   - Prevent the default behavior (`e.preventDefault()`)
   - Show a success message in `#success`: `"Registration successful! Welcome, <username>."`
   - Use a template literal

7. Style the `.error` class in red and add a `.valid` class in green for valid fields (optional bonus).

### ✅ Acceptance Criteria

- [ ] Typing in each field shows/clears errors live
- [ ] Submit button is disabled until all fields are valid
- [ ] Username rejects spaces and special characters
- [ ] Email must contain `@` and `.`
- [ ] Password must be 6+ characters
- [ ] Confirm must match password
- [ ] Submitting shows a success message with the username
- [ ] Form does not actually reload the page

### 💡 Hints

- Use `addEventListener("input", ...)` for live validation.
- Use `e.preventDefault()` in the submit handler.
- For the username check, you can loop through characters and use `charCodeAt` or a regex like `/^[a-zA-Z0-9]+$/`.
- `Object.values(valid).every(v => v === true)` checks if all are valid.

---

## 📊 Scoring Rubric

| Task | Points | Focus |
|------|--------|-------|
| Task 1 | 20 | Arrays + Objects + DOM creation |
| Task 2 | 20 | Conditionals + Functions + Events |
| Task 3 | 25 | HOFs (filter/map) + DOM updates |
| Task 4 | 20 | reduce + Objects + Statistics |
| Task 5 | 25 | Live validation + Conditionals + Events |
| **Total** | **110** | |

### Badge System

- 🥇 **DOM Master** — Complete all 5 tasks
- 🥈 **Logic Pro** — Complete Tasks 2, 4, and 5
- 🥉 **Array Wizard** — Complete Tasks 1 and 3
- 🐛 **Bug Hunter** — Help 3 classmates fix their bugs

---

## ⏱️ Time Management

| Time | Task |
|------|------|
| 0:00 – 0:30 | Task 1: Product Catalog |
| 0:30 – 1:00 | Task 2: Quiz App |
| 1:00 – 1:30 | Task 3: Todo List |
| 1:30 – 2:00 | Task 4: Grades Dashboard |
| 2:00 – 2:30 | Task 5: Registration Form |

> **Tip:** If you finish a task early, start the next one. If you are stuck for more than 10 minutes, ask a classmate or the instructor.

---

## 🔑 Knowledge Map — Which Sessions Each Task Uses

```
Task 1 (Product Catalog)   → Sessions 5, 6, 9, 10
Task 2 (Quiz App)          → Sessions 4, 7, 9, 10
Task 3 (Todo List)         → Sessions 5, 6, 8, 10
Task 4 (Grades Dashboard)  → Sessions 4, 5, 8, 9, 10
Task 5 (Registration Form) → Sessions 3, 4, 7, 9, 10
```

Every task uses **DOM manipulation (Session 10)** as the foundation, and layers on the logic and data skills from Sessions 1–9.

---

## 🧠 Final Reminder

> This session has **no solutions**. The goal is for you to think, write code, test in the browser, and debug using the console. Mistakes are part of learning. Open DevTools, read the errors, and fix them. You have all the knowledge from Sessions 1–10 — now combine it.

Good luck. 🚀
