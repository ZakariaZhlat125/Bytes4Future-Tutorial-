# Session Tasks Solutions: DOM Integration Challenges — Full Answers

> ⚠️ **Instructor Only.** Do not share with students until they have attempted every task.

---

## 📁 Folder Structure

```
session-tasks-solutions/
├── task1.html
├── task2.html
├── task3.html
├── task4.html
└── task5.html
```

Each file is self-contained (HTML + CSS + JS inline).

---

## 🚀 Task 1 Solution: Dynamic Product Catalog

**File:** `task1.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Task 1 — Product Catalog</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 900px; margin: 20px auto; padding: 0 15px; }
    h1 { text-align: center; }
    .cart-bar { background: #222; color: white; padding: 10px 15px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
    #catalog { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; }
    .card { border: 1px solid #ddd; border-radius: 8px; padding: 15px; background: #fff; box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
    .card h3 { margin: 0 0 8px; }
    .price { font-size: 1.2em; font-weight: bold; color: #2a7a2a; }
    .category { color: #666; font-size: 0.9em; }
    .badge { display: inline-block; padding: 3px 8px; border-radius: 12px; font-size: 0.8em; margin: 8px 0; }
    .in-stock { background: #d4edda; color: #155724; }
    .out-stock { background: #f8d7da; color: #721c24; }
    .card button { width: 100%; padding: 8px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
    .card button:hover { background: #0056b3; }
    .card button:disabled { background: #999; cursor: not-allowed; }
  </style>
</head>
<body>
  <h1>🛍️ Product Catalog</h1>

  <div class="cart-bar">
    <span>🛒 Cart</span>
    <span>Items: <strong id="cartCount">0</strong></span>
  </div>

  <div id="catalog"></div>

  <script>
    // --- Data (Session 5: Arrays + Session 9: Objects) ---
    const products = [
      { id: 1, name: "Laptop",      price: 999.99,  category: "Electronics", inStock: true  },
      { id: 2, name: "Mouse",       price: 19.99,   category: "Electronics", inStock: true  },
      { id: 3, name: "Keyboard",     price: 49.99,   category: "Electronics", inStock: false },
      { id: 4, name: "Notebook",     price: 4.99,    category: "Stationery",  inStock: true  },
      { id: 5, name: "Coffee Mug",   price: 12.50,   category: "Kitchen",     inStock: true  }
    ];

    // --- Cart state ---
    const cart = [];

    // --- Render catalog (Session 6: Loops + Session 10: DOM) ---
    const catalogEl = document.getElementById("catalog");
    const cartCountEl = document.getElementById("cartCount");

    products.forEach(product => {
      const card = document.createElement("div");
      card.className = "card";

      // Session 3: Template literals + toFixed
      card.innerHTML = `
        <h3>${product.name}</h3>
        <div class="price">$${product.price.toFixed(2)}</div>
        <div class="category">${product.category}</div>
        <div class="badge ${product.inStock ? "in-stock" : "out-stock"}">
          ${product.inStock ? "In Stock" : "Out of Stock"}
        </div>
      `;

      // Session 10: Create button + event listener
      const btn = document.createElement("button");
      btn.textContent = "Add to Cart";
      btn.disabled = !product.inStock;

      // Session 8: Arrow function
      btn.addEventListener("click", () => {
        cart.push(product);
        cartCountEl.textContent = cart.length;
        console.log("Added:", product.name, "| Cart total items:", cart.length);
      });

      card.appendChild(btn);
      catalogEl.appendChild(card);
    });

    console.log("Catalog rendered with", products.length, "products.");
  </script>
</body>
</html>
```

### Key Concepts Used
- **Session 5** — `products` array
- **Session 9** — product objects with dot notation
- **Session 6** — `forEach` loop to build cards
- **Session 3** — template literals + `toFixed(2)`
- **Session 4** — ternary for stock badge
- **Session 10** — `createElement`, `appendChild`, `addEventListener`

---

## 🚀 Task 2 Solution: Interactive Quiz App

**File:** `task2.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Task 2 — Quiz App</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 30px auto; padding: 0 15px; }
    #quiz { background: #f9f9f9; padding: 20px; border-radius: 8px; }
    #questionText { margin-top: 0; }
    #choices { display: flex; flex-direction: column; gap: 10px; margin: 15px 0; }
    .choice { padding: 12px; border: 2px solid #ddd; border-radius: 6px; background: white; cursor: pointer; text-align: left; font-size: 1em; }
    .choice:hover { background: #f0f0f0; }
    .choice.selected { border-color: #007bff; background: #e7f1ff; }
    #next { padding: 10px 20px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; font-size: 1em; }
    #next:disabled { background: #999; cursor: not-allowed; }
    #result { margin-top: 20px; padding: 20px; border-radius: 8px; text-align: center; font-size: 1.2em; }
    .perfect { background: #d4edda; color: #155724; }
    .good    { background: #fff3cd; color: #856404; }
    .bad     { background: #f8d7da; color: #721c24; }
  </style>
</head>
<body>
  <h1>📝 Quiz App</h1>

  <div id="quiz">
    <h2 id="questionText"></h2>
    <div id="choices"></div>
    <button id="next" disabled>Next</button>
  </div>

  <div id="result"></div>

  <script>
    // --- Data (Session 9: Objects + Session 5: Arrays) ---
    const questions = [
      {
        question: "Which keyword declares a constant?",
        choices: ["let", "const", "var", "static"],
        answer: 1
      },
      {
        question: "What does typeof null return?",
        choices: ["null", "undefined", "object", "number"],
        answer: 2
      },
      {
        question: "Which method adds an element to the end of an array?",
        choices: ["push()", "pop()", "shift()", "unshift()"],
        answer: 0
      },
      {
        question: "Which selector returns ALL matching elements?",
        choices: ["getElementById", "querySelector", "querySelectorAll", "getElementsById"],
        answer: 2
      }
    ];

    // --- State (Session 2: Variables) ---
    let currentIndex = 0;
    let score = 0;
    let selectedAnswer = null;

    // --- DOM refs (Session 10) ---
    const questionTextEl = document.getElementById("questionText");
    const choicesEl = document.getElementById("choices");
    const nextBtn = document.getElementById("next");
    const resultEl = document.getElementById("result");

    // --- Functions (Session 7) ---
    function showQuestion(index) {
      const q = questions[index];
      questionTextEl.textContent = `Q${index + 1}: ${q.question}`;

      choicesEl.innerHTML = "";
      selectedAnswer = null;
      nextBtn.disabled = true;

      // Session 6: Loop + Session 10: DOM
      q.choices.forEach((choice, i) => {
        const btn = document.createElement("button");
        btn.className = "choice";
        btn.textContent = choice;

        // Session 8: Arrow function
        btn.addEventListener("click", () => {
          // Clear previous selection
          document.querySelectorAll(".choice").forEach(c => c.classList.remove("selected"));
          btn.classList.add("selected");
          selectedAnswer = i;
          nextBtn.disabled = false;
        });

        choicesEl.appendChild(btn);
      });
    }

    function checkAnswer() {
      // Session 4: Conditional
      if (selectedAnswer === questions[currentIndex].answer) {
        score++;
      }
      currentIndex++;

      if (currentIndex < questions.length) {
        showQuestion(currentIndex);
      } else {
        showResult();
      }
    }

    function showResult() {
      // Hide quiz, show result
      document.getElementById("quiz").style.display = "none";

      const total = questions.length;
      resultEl.innerHTML = `<strong>You scored ${score}/${total}</strong><br>`;

      // Session 4: if / else if / else
      let message;
      let className;

      if (score === total) {
        message = "Perfect! 🏆";
        className = "perfect";
      } else if (score >= total / 2) {
        message = "Good job! 👍";
        className = "good";
      } else {
        message = "Keep practicing! 📚";
        className = "bad";
      }

      resultEl.className = className;
      resultEl.innerHTML += message;
    }

    // --- Wire up Next button ---
    nextBtn.addEventListener("click", checkAnswer);

    // --- Start ---
    showQuestion(currentIndex);
  </script>
</body>
</html>
```

### Key Concepts Used
- **Session 4** — `if / else if / else` for the final message
- **Session 7** — `showQuestion`, `checkAnswer`, `showResult` functions
- **Session 9** — question objects with nested arrays
- **Session 10** — `getElementById`, `querySelectorAll`, `addEventListener`, dynamic button creation

---

## 🚀 Task 3 Solution: Todo List with Filters

**File:** `task3.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Task 3 — Todo List</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 500px; margin: 30px auto; padding: 0 15px; }
    .input-row { display: flex; gap: 8px; margin-bottom: 15px; }
    #todoInput { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 4px; }
    #addBtn { padding: 10px 20px; background: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer; }
    #addBtn:hover { background: #218838; }
    #filters { display: flex; gap: 8px; margin-bottom: 15px; }
    #filters button { flex: 1; padding: 8px; border: 1px solid #ccc; background: white; border-radius: 4px; cursor: pointer; }
    #filters button.active { background: #007bff; color: white; border-color: #007bff; }
    #todoList { list-style: none; padding: 0; }
    #todoList li { display: flex; align-items: center; gap: 10px; padding: 10px; border-bottom: 1px solid #eee; }
    #todoList li.done span { text-decoration: line-through; color: #999; }
    #todoList li span { flex: 1; }
    .done-btn { padding: 4px 10px; background: #ffc107; border: none; border-radius: 4px; cursor: pointer; }
    .del-btn  { padding: 4px 10px; background: #dc3545; color: white; border: none; border-radius: 4px; cursor: pointer; }
    #count { margin-top: 15px; color: #666; font-size: 0.9em; }
  </style>
</head>
<body>
  <h1>✅ Todo List</h1>

  <div class="input-row">
    <input id="todoInput" placeholder="Add a task..." />
    <button id="addBtn">Add</button>
  </div>

  <div id="filters">
    <button data-filter="all" class="active">All</button>
    <button data-filter="active">Active</button>
    <button data-filter="completed">Completed</button>
  </div>

  <ul id="todoList"></ul>
  <p id="count"></p>

  <script>
    // --- State ---
    let todos = [];
    let currentFilter = "all";
    let nextId = 1;

    // --- DOM refs ---
    const todoInput = document.getElementById("todoInput");
    const addBtn = document.getElementById("addBtn");
    const todoListEl = document.getElementById("todoList");
    const countEl = document.getElementById("count");
    const filterBtns = document.querySelectorAll("#filters button");

    // --- Functions ---

    // Session 8: filter() + Session 6: forEach
    function render() {
      // Filter based on currentFilter
      let visible;
      if (currentFilter === "active") {
        visible = todos.filter(t => !t.done);
      } else if (currentFilter === "completed") {
        visible = todos.filter(t => t.done);
      } else {
        visible = todos;
      }

      todoListEl.innerHTML = "";

      visible.forEach(todo => {
        const li = document.createElement("li");
        if (todo.done) li.className = "done";

        const span = document.createElement("span");
        span.textContent = todo.text;
        li.appendChild(span);

        const doneBtn = document.createElement("button");
        doneBtn.className = "done-btn";
        doneBtn.textContent = todo.done ? "Undo" : "Done";
        doneBtn.addEventListener("click", () => toggleTodo(todo.id));
        li.appendChild(doneBtn);

        const delBtn = document.createElement("button");
        delBtn.className = "del-btn";
        delBtn.textContent = "Delete";
        delBtn.addEventListener("click", () => deleteTodo(todo.id));
        li.appendChild(delBtn);

        todoListEl.appendChild(li);
      });

      // Update count
      const activeCount = todos.filter(t => !t.done).length;
      const completedCount = todos.filter(t => t.done).length;
      countEl.textContent = `${activeCount} active, ${completedCount} completed`;
    }

    function addTodo() {
      const text = todoInput.value.trim();
      if (text === "") return; // ignore empty

      todos.push({
        id: nextId++,
        text: text,
        done: false
      });

      todoInput.value = "";
      render();
    }

    function toggleTodo(id) {
      // Session 8: map to create new array
      todos = todos.map(t =>
        t.id === id ? { ...t, done: !t.done } : t
      );
      render();
    }

    function deleteTodo(id) {
      // Session 8: filter
      todos = todos.filter(t => t.id !== id);
      render();
    }

    // --- Event listeners (Session 8: Arrow functions) ---
    addBtn.addEventListener("click", addTodo);

    todoInput.addEventListener("keydown", (e) => {
      if (e.key === "Enter") addTodo();
    });

    filterBtns.forEach(btn => {
      btn.addEventListener("click", () => {
        filterBtns.forEach(b => b.classList.remove("active"));
        btn.classList.add("active");
        currentFilter = btn.dataset.filter;
        render();
      });
    });

    // --- Seed data ---
    todos = [
      { id: nextId++, text: "Learn JavaScript", done: true },
      { id: nextId++, text: "Build a project",  done: false },
      { id: nextId++, text: "Push to GitHub",  done: false }
    ];

    render();
  </script>
</body>
</html>
```

### Key Concepts Used
- **Session 5** — `todos` array
- **Session 8** — `filter()`, `map()`, arrow functions in event listeners
- **Session 6** — `forEach` to render list
- **Session 10** — `createElement`, `appendChild`, `addEventListener`, `innerHTML`
- **Session 9** — todo objects + spread operator for immutable update

---

## 🚀 Task 4 Solution: Student Grades Dashboard

**File:** `task4.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Task 4 — Grades Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 30px auto; padding: 0 15px; }
    #stats { background: #f0f4ff; padding: 15px; border-radius: 8px; margin-bottom: 20px; }
    #stats div { margin: 4px 0; }
    table { width: 100%; border-collapse: collapse; }
    th, td { padding: 10px; text-align: left; border-bottom: 1px solid #ddd; }
    th { background: #007bff; color: white; }
    .pass { color: #155724; font-weight: bold; }
    .fail { color: #721c24; font-weight: bold; }
  </style>
</head>
<body>
  <h1>📊 Student Grades Dashboard</h1>

  <div id="stats"></div>

  <table id="table">
    <thead>
      <tr><th>Name</th><th>Average</th><th>Status</th></tr>
    </thead>
    <tbody id="tbody"></tbody>
  </table>

  <script>
    // --- Data (Session 9 + Session 5) ---
    const students = [
      { name: "Alice",   grades: [95, 88, 92] },
      { name: "Bob",     grades: [55, 60, 58] },
      { name: "Charlie", grades: [78, 82, 80] },
      { name: "Diana",   grades: [40, 45, 50] },
      { name: "Eve",     grades: [90, 95, 100] }
    ];

    // --- Functions (Session 7) ---

    // Session 8: reduce
    function getAverage(grades) {
      const sum = grades.reduce((acc, g) => acc + g, 0);
      return (sum / grades.length).toFixed(1);
    }

    // Session 4: Ternary
    function getStatus(average) {
      return Number(average) >= 60 ? "Pass" : "Fail";
    }

    // --- Render table (Session 6 + Session 10) ---
    function renderTable() {
      const tbody = document.getElementById("tbody");
      tbody.innerHTML = "";

      students.forEach(student => {
        const avg = getAverage(student.grades);
        const status = getStatus(avg);

        const tr = document.createElement("tr");

        // Session 3: Template literals
        tr.innerHTML = `
          <td>${student.name}</td>
          <td>${avg}</td>
          <td class="${status === "Pass" ? "pass" : "fail"}">${status}</td>
        `;

        tbody.appendChild(tr);
      });
    }

    // --- Render stats (Session 8: reduce) ---
    function renderStats() {
      const statsEl = document.getElementById("stats");

      // Map each student to their numeric average
      const averages = students.map(s => Number(getAverage(s.grades)));

      // Total students
      const totalStudents = students.length;

      // Class average
      const classAverage = (averages.reduce((a, b) => a + b, 0) / totalStudents).toFixed(1);

      // Pass / fail counts
      const passing = averages.filter(a => a >= 60).length;
      const failing = averages.filter(a => a < 60).length;

      // Highest & lowest
      const maxAvg = Math.max(...averages);
      const minAvg = Math.min(...averages);
      const topStudent = students[averages.indexOf(maxAvg)].name;
      const lowStudent = students[averages.indexOf(minAvg)].name;

      statsEl.innerHTML = `
        <div><strong>Total students:</strong> ${totalStudents}</div>
        <div><strong>Class average:</strong> ${classAverage}</div>
        <div><strong>Passing:</strong> ${passing}</div>
        <div><strong>Failing:</strong> ${failing}</div>
        <div><strong>Highest average:</strong> ${topStudent} (${maxAvg})</div>
        <div><strong>Lowest average:</strong> ${lowStudent} (${minAvg})</div>
      `;
    }

    // --- Init ---
    renderTable();
    renderStats();
  </script>
</body>
</html>
```

### Key Concepts Used
- **Session 9** — student objects
- **Session 5** — `grades` arrays inside objects
- **Session 8** — `reduce()` for averages and counts, `map()` + `filter()`
- **Session 4** — ternary for pass/fail
- **Session 10** — `innerHTML`, `getElementById`, dynamic table rows
- **Session 3** — template literals + `toFixed(1)`

---

## 🚀 Task 5 Solution: Registration Form with Live Validation

**File:** `task5.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Task 5 — Registration Form</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 450px; margin: 30px auto; padding: 0 15px; }
    form { display: flex; flex-direction: column; gap: 8px; }
    input { padding: 10px; border: 1px solid #ccc; border-radius: 4px; font-size: 1em; }
    input.valid { border-color: #28a745; }
    input.invalid { border-color: #dc3545; }
    .error { color: #dc3545; font-size: 0.85em; min-height: 1.2em; }
    #submitBtn { margin-top: 10px; padding: 12px; background: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer; font-size: 1em; }
    #submitBtn:disabled { background: #999; cursor: not-allowed; }
    #success { margin-top: 20px; padding: 15px; background: #d4edda; color: #155724; border-radius: 6px; text-align: center; font-size: 1.1em; }
  </style>
</head>
<body>
  <h1>📝 Registration Form</h1>

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

  <script>
    // --- Validity tracker (Session 9: Object) ---
    const valid = {
      username: false,
      email: false,
      password: false,
      confirm: false
    };

    // --- DOM refs (Session 10) ---
    const usernameInput  = document.getElementById("username");
    const emailInput     = document.getElementById("email");
    const passwordInput  = document.getElementById("password");
    const confirmInput   = document.getElementById("confirm");
    const usernameError  = document.getElementById("usernameError");
    const emailError     = document.getElementById("emailError");
    const passwordError  = document.getElementById("passwordError");
    const confirmError   = document.getElementById("confirmError");
    const submitBtn      = document.getElementById("submitBtn");
    const successEl      = document.getElementById("success");
    const form           = document.getElementById("regForm");

    // --- Validation functions (Session 7) ---

    // Session 3: String methods + Session 6: Loop
    function validateUsername(value) {
      if (value.length < 3) return false;
      // Only letters and numbers
      for (let char of value) {
        const code = char.charCodeAt(0);
        const isLetter = (code >= 65 && code <= 90) || (code >= 97 && code <= 122);
        const isNumber = (code >= 48 && code <= 57);
        if (!isLetter && !isNumber) return false;
      }
      return true;
    }

    // Session 3: String methods (indexOf, includes)
    function validateEmail(value) {
      const atIndex = value.indexOf("@");
      const lastDotIndex = value.lastIndexOf(".");
      return atIndex > 0 && lastDotIndex > atIndex + 1 && lastDotIndex < value.length - 1;
    }

    function validatePassword(value) {
      return value.length >= 6;
    }

    function validateConfirm(value) {
      return value === passwordInput.value && value.length >= 6;
    }

    // --- Helper to update field UI ---
    function setFieldState(input, errorEl, isValid, errorMsg) {
      if (isValid) {
        input.classList.add("valid");
        input.classList.remove("invalid");
        errorEl.textContent = "";
      } else {
        input.classList.add("invalid");
        input.classList.remove("valid");
        errorEl.textContent = errorMsg;
      }
    }

    // --- checkAllValid (Session 4 + Session 9) ---
    function checkAllValid() {
      const allValid = Object.values(valid).every(v => v === true);
      submitBtn.disabled = !allValid;
    }

    // --- Live validation listeners (Session 8: Arrow + Session 10: Events) ---
    usernameInput.addEventListener("input", () => {
      const ok = validateUsername(usernameInput.value);
      valid.username = ok;
      setFieldState(usernameInput, usernameError, ok, "Min 3 chars, letters and numbers only.");
      checkAllValid();
    });

    emailInput.addEventListener("input", () => {
      const ok = validateEmail(emailInput.value);
      valid.email = ok;
      setFieldState(emailInput, emailError, ok, "Enter a valid email with @ and .");
      checkAllValid();
    });

    passwordInput.addEventListener("input", () => {
      const ok = validatePassword(passwordInput.value);
      valid.password = ok;
      setFieldState(passwordInput, passwordError, ok, "Password must be at least 6 characters.");
      // Re-validate confirm in case password changed
      const confirmOk = validateConfirm(confirmInput.value);
      valid.confirm = confirmOk && ok;
      setFieldState(confirmInput, confirmError, valid.confirm, "Passwords must match.");
      checkAllValid();
    });

    confirmInput.addEventListener("input", () => {
      const ok = validateConfirm(confirmInput.value);
      valid.confirm = ok;
      setFieldState(confirmInput, confirmError, ok, "Passwords must match.");
      checkAllValid();
    });

    // --- Submit handler (Session 4 + Session 10) ---
    form.addEventListener("submit", (e) => {
      e.preventDefault();
      // Session 3: Template literal
      successEl.innerHTML = `✅ Registration successful! Welcome, <strong>${usernameInput.value}</strong>.`;
      console.log("Registered user:", usernameInput.value, emailInput.value);
    });
  </script>
</body>
</html>
```

### Key Concepts Used
- **Session 3** — string methods (`indexOf`, `lastIndexOf`, `charCodeAt`), template literals
- **Session 4** — conditionals in validation functions, `Object.values().every()`
- **Session 6** — `for...of` loop to check username characters
- **Session 7** — validation functions with `return`
- **Session 8** — arrow functions in event listeners
- **Session 9** — `valid` object tracking each field
- **Session 10** — `addEventListener("input", ...)`, `classList`, `e.preventDefault()`

---

## 📊 Solution Summary Table

| Task | File | Main Techniques |
|------|------|-----------------|
| Task 1 | `task1.html` | `forEach` + `createElement` + template literals + events |
| Task 2 | `task2.html` | Functions + conditionals + dynamic buttons + state |
| Task 3 | `task3.html` | `filter()` + `map()` + immutable updates + filters |
| Task 4 | `task4.html` | `reduce()` + `Math.max/min` + spread operator + table |
| Task 5 | `task5.html` | Live `input` events + validation + `preventDefault` |

---

## 🧠 Instructor Notes

1. **Reveal solutions one at a time** — do not give all 5 at once.
2. **Compare student code with the solution** — discuss different approaches (e.g., `innerHTML` vs `createElement`).
3. **Common mistakes to highlight:**
   - Forgetting `e.preventDefault()` in forms
   - Using `innerHTML` where `textContent` is safer (XSS)
   - Mutating arrays instead of using `filter`/`map` for new arrays
   - Forgetting to re-render after state changes
4. **Bonus challenges** for fast students:
   - Task 1: Add a "Remove from Cart" button
   - Task 2: Add a progress bar showing question X of Y
   - Task 3: Persist todos in `localStorage`
   - Task 4: Add a sort-by-average feature
   - Task 5: Show password strength meter
