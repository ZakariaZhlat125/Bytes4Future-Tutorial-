# Session 10: DOM Manipulation — Active Learning Redesign

## Session Plan for the Instructor

- **Total time:** approximately 90 to 120 minutes
- **Pacing rule:** never explain theory continuously for more than 15–20 minutes. After every concept, students must predict, write, or fix code.
- **Pedagogical pattern for every topic:**
  1. **Problem** — a realistic mini-situation
  2. **Guess** — ask: "What do you expect to happen?"
  3. **Explain** — the shortest rule that fixes the problem
  4. **Code** — live code written in front of the students, step by step
  5. **Challenge** — students solve a small task on their own or in groups
  6. **Review** — discuss the answer and the most common mistake

### Competition and Points

- 1 point per correct prediction in the "Guess" phase.
- 1–3 points per completed challenge, depending on difficulty.
- A "Bug Hunter" badge for each student who finds and fixes an intentional error.
- Keep a simple tally on a shared board or in the chat.

### Instructor Questions to Ask During the Session

- "Which selector is fastest here?"
- "Should we use `textContent` or `innerHTML`?"
- "What does `event.target` point to?"
- "How can we avoid adding 100 event listeners?"
- "What is the default behavior we want to prevent?"

---

## Part 0: Warm-Up — The Static Page (5 minutes)

### Problem

A page has a heading, but the text needs to change when a button is clicked.

```html
<h1 id="title">Hello</h1>
<button>Change</button>
```

### Guess

Ask: "How can JavaScript change the heading without reloading the page?"

### Explain

The browser turns HTML into a tree of objects. JavaScript can select and change those objects.

### Live Code

```javascript
const title = document.getElementById("title");
const button = document.querySelector("button");

button.addEventListener("click", () => {
  title.textContent = "Hello, World!";
});
```

### Review

We selected an element and changed its `textContent` when an event happened.

---

## Part 1: Selecting Elements

### 1.1 getElementById

#### Problem

Select a specific element.

#### Live Code

```html
<div id="main">Main content</div>
```

```javascript
const main = document.getElementById("main");
console.log(main.textContent);
```

#### Challenge 1.1 — Get the Element (individual, 2 minutes)

- **Requirement:** Get the element with `id="hero"` and print its `textContent`.
- **Time limit:** 2 minutes

### 1.2 Class and Tag Selectors

#### Live Code

```html
<p class="intro">Intro 1</p>
<p class="intro">Intro 2</p>
<div>Div 1</div>
```

```javascript
const intros = document.getElementsByClassName("intro");
console.log(intros.length);

const divs = document.getElementsByTagName("div");
console.log(divs.length);
```

#### Challenge 1.2 — Count Elements (individual, 3 minutes)

- **Requirement:** Count how many `.item` elements and `li` elements are on the page.
- **Time limit:** 3 minutes

### 1.3 querySelector and querySelectorAll

#### Live Code

```html
<ul id="list">
  <li class="active">One</li>
  <li>Two</li>
</ul>
```

```javascript
const first = document.querySelector("#list li");
const all = document.querySelectorAll("#list li");

console.log(first.textContent);
all.forEach(item => console.log(item.textContent));
```

#### Challenge 1.3 — Select Items (individual, 3 minutes)

- **Requirement:** Use `querySelectorAll` to select all `li` elements inside `#menu`.
- **Time limit:** 3 minutes

---

## Part 2: Reading and Changing Content

### 2.1 textContent vs innerHTML

#### Problem

Change text safely vs adding HTML.

#### Live Code

```javascript
const box = document.getElementById("box");

box.textContent = "<strong>Bold?</strong>"; // Shows as plain text
box.innerHTML = "<strong>Bold!</strong>";   // Renders HTML
```

#### Challenge 2.1 — Safe Update (individual, 3 minutes)

- **Requirement:** Change `#message` to show "Welcome, User!" using only `textContent`.
- **Time limit:** 3 minutes

### 2.2 Attributes

#### Live Code

```html
<a id="link" href="https://old.com">Link</a>
<img id="pic" src="old.jpg" alt="Old" />
```

```javascript
const link = document.getElementById("link");
link.setAttribute("href", "https://new.com");
console.log(link.getAttribute("href"));

const pic = document.getElementById("pic");
pic.src = "new.jpg";
pic.alt = "New";

console.log(pic.hasAttribute("title")); // false
```

#### Challenge 2.2 — Change Image Source (individual, 3 minutes)

- **Requirement:** Change the `src` and `alt` of `#avatar` to a new image and description.
- **Time limit:** 3 minutes

---

## Part 3: Creating and Inserting Elements

### 3.1 Creating and Appending

#### Problem

Add a new item to a list from JavaScript.

#### Live Code

```html
<ul id="fruits">
  <li>Apple</li>
</ul>
```

```javascript
const fruits = document.getElementById("fruits");

const li = document.createElement("li");
li.textContent = "Banana";
fruits.appendChild(li);
```

#### Challenge 3.1 — Add to List (individual, 4 minutes)

- **Requirement:** Create a `li` with text "Orange" and add it to `#fruits`.
- **Time limit:** 4 minutes

### 3.2 Modern Insertion Methods

#### Live Code

```javascript
const parent = document.getElementById("parent");
const first = document.createElement("div");
const last = document.createElement("div");
const before = document.createElement("div");

first.textContent = "First";
last.textContent = "Last";
before.textContent = "Before reference";

parent.prepend(first);
parent.append(last);

const reference = document.getElementById("reference");
reference.before(before);
```

#### Challenge 3.2 — Insert Before (individual, 4 minutes)

- **Requirement:** Create a `<p>Warning</p>` and insert it before `#target`.
- **Time limit:** 4 minutes

### 3.3 Removing and Replacing

#### Live Code

```javascript
const old = document.getElementById("old");
old.remove();

const child = document.getElementById("child");
const replacement = document.createElement("div");
replacement.textContent = "New";
child.parentNode.replaceChild(replacement, child);
```

#### Challenge 3.3 — Clear Container (individual, 3 minutes)

- **Requirement:** Remove all children from `#container`.
- **Time limit:** 3 minutes
- **Hint:** `while (container.firstChild) { container.removeChild(container.firstChild); }`

---

## Part 4: Styling and Classes

### 4.1 classList

#### Live Code

```html
<div id="box" class="card">Card</div>
```

```javascript
const box = document.getElementById("box");

box.classList.add("active");
box.classList.remove("card");
box.classList.toggle("hidden");
console.log(box.classList.contains("active"));
```

#### Challenge 4.1 — Toggle Class (individual, 3 minutes)

- **Requirement:** Toggle the class `visible` on `#box` when `#toggleBtn` is clicked.
- **Time limit:** 3 minutes

### 4.2 Inline Styles

#### Live Code

```javascript
const box = document.getElementById("box");
box.style.color = "white";
box.style.backgroundColor = "blue";
box.style.padding = "20px";
```

#### Challenge 4.2 — Style Element (individual, 3 minutes)

- **Requirement:** Set `#banner` text color to red, background to yellow, and font size to `24px`.
- **Time limit:** 3 minutes

---

## Part 5: DOM Events

### 5.1 addEventListener

#### Problem

Run code when a user clicks a button.

#### Live Code

```html
<button id="btn">Click me</button>
```

```javascript
const btn = document.getElementById("btn");

btn.addEventListener("click", function(event) {
  console.log("Clicked");
  console.log(event.target);
});
```

#### Challenge 5.1 — Log Click (individual, 3 minutes)

- **Requirement:** When `#btn` is clicked, log the button text.
- **Time limit:** 3 minutes

### 5.2 Event Types

#### Live Code

```javascript
const input = document.getElementById("name");

input.addEventListener("input", () => {
  console.log(input.value);
});

input.addEventListener("focus", () => {
  input.style.backgroundColor = "lightblue";
});

input.addEventListener("blur", () => {
  input.style.backgroundColor = "white";
});
```

#### Challenge 5.2 — Input Event (individual, 3 minutes)

- **Requirement:** Show the length of `#message` input below it while typing.
- **Time limit:** 3 minutes

### 5.3 preventDefault

#### Problem

A form reloads the page on submit. We want to stop that.

#### Live Code

```html
<form id="myForm">
  <input type="text" id="name" />
  <button type="submit">Submit</button>
</form>
```

```javascript
const form = document.getElementById("myForm");

form.addEventListener("submit", function(event) {
  event.preventDefault();
  const name = document.getElementById("name").value;
  console.log(name);
});
```

#### Challenge 5.3 — Prevent Link (individual, 3 minutes)

- **Requirement:** Add a click listener to `#link` that prevents navigation and logs "Link clicked".
- **Time limit:** 3 minutes

### 5.4 Event Delegation

#### Problem

A list has 100 items. We do not want 100 listeners.

#### Live Code

```html
<ul id="todo">
  <li>Task 1</li>
  <li>Task 2</li>
</ul>
```

```javascript
const todo = document.getElementById("todo");

todo.addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    event.target.classList.toggle("done");
  }
});
```

#### Challenge 5.4 — Delete with Delegation (individual, 5 minutes)

- **Requirement:** When a `.delete` button inside `#list` is clicked, remove its parent `li`.
- **Time limit:** 5 minutes

---

## Part 6: Form Validation

### 6.1 Basic Validation

#### Problem

A form should not submit if fields are empty or invalid.

#### Live Code

```javascript
const form = document.getElementById("signup");

form.addEventListener("submit", function(event) {
  event.preventDefault();

  const email = document.getElementById("email").value;
  const error = document.getElementById("emailError");

  if (!email.includes("@")) {
    error.textContent = "Please enter a valid email";
    return;
  }

  error.textContent = "";
  console.log("Valid:", email);
});
```

#### Challenge 6.1 — Validate Name (individual, 4 minutes)

- **Requirement:** If `#name` is empty, show "Name is required" in `#nameError`.
- **Time limit:** 4 minutes

### 6.2 Real-Time Validation

#### Live Code

```javascript
const emailInput = document.getElementById("email");

emailInput.addEventListener("input", function() {
  const value = this.value;
  const isValid = value.includes("@") && value.includes(".");
  this.style.borderColor = isValid ? "green" : "red";
});
```

#### Challenge 6.2 — Live Password Check (individual, 4 minutes)

- **Requirement:** While typing in `#password`, make the border green if length >= 8, red otherwise.
- **Time limit:** 4 minutes

---

## Bug Hunt 1

### Problem

The following code has three deliberate bugs.

```html
<div id="greeting">Hi</div>
<button id="btn">Click</button>
```

```javascript
const greeting = document.getElementById("greeting");
const button = document.getElementById("btn");

button.addEventListener("onclick", function() {
  greeting.innerHTML = "Hello";
  greeting.class = "active";
  button.textcontent = "Done";
});
```

### Issues

1. The event name is `"click"`, not `"onclick"`.
2. To set a class, use `classList.add` or `className`. `greeting.class` is not the right property.
3. The property is `textContent`, not `textcontent`.

### Fixed Version

```javascript
button.addEventListener("click", function() {
  greeting.textContent = "Hello";
  greeting.classList.add("active");
  button.textContent = "Done";
});
```

### Points

1 point per found bug.

---

## Part 7: DOM Traversal and Cloning

### 7.1 Traversing

#### Live Code

```html
<ul id="menu">
  <li id="first">Home</li>
  <li>About</li>
</ul>
```

```javascript
const first = document.getElementById("first");

console.log(first.parentElement.id);
console.log(first.nextElementSibling.textContent);
console.log(first.children.length);
```

#### Challenge 7.1 — Find Sibling (individual, 3 minutes)

- **Requirement:** Log the `textContent` of `#start`'s next sibling.
- **Time limit:** 3 minutes

### 7.2 Cloning

#### Live Code

```javascript
const original = document.getElementById("card");
const clone = original.cloneNode(true);
clone.id = "card2";
clone.querySelector("h3").textContent = "Copy";

document.getElementById("container").appendChild(clone);
```

#### Challenge 7.2 — Clone Item (individual, 4 minutes)

- **Requirement:** Clone `.template` and change the title before appending it.
- **Time limit:** 4 minutes

---

## Bug Hunt 2

### Problem

Find the bugs in this form and list code.

```html
<ul id="todo"></ul>
<input type="text" id="task" />
<button id="add">Add</button>
```

```javascript
const addBtn = document.getElementById("add");
const taskInput = document.getElementById("task");
const list = document.getElementById("todo");

addBtn.addEventListener("click", () => {
  const li = document.createElement("li");
  li.innerHTML = taskInput.value;
  list.append(li);
  taskInput.value = "";
});

list.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    event.target.remove();
  }
});
```

### Issues

1. `li.innerHTML = taskInput.value` is unsafe if users type HTML. Use `textContent` for plain text.
2. `list` click listener removes the `li` itself, not a delete button. This is a design issue, but the main bug is that it is too easy to delete accidentally. For this exercise, the intended bug is using `innerHTML` with user input.

### Fixed Version

```javascript
addBtn.addEventListener("click", () => {
  const li = document.createElement("li");
  li.textContent = taskInput.value;
  list.append(li);
  taskInput.value = "";
});
```

### Points

1 point per found issue.

---

## Group Challenge: DOM Builder Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team writes the DOM code for one of the tasks.
- **Scoring:** 2 points per working solution. The first team to finish all four gets 2 bonus points.

### Tasks

1. When `#btn1` is clicked, change `#output` background to a random color.
2. When `#btn2` is clicked, duplicate `#box` and append it to `#container`.
3. Add a new `li` from `#input` to `#list` when `#btn3` is clicked, and clear the input.
4. Use event delegation on `#list2` so clicking any `li` toggles the class `selected`.

### Instructor Answer Key

```javascript
// 1
document.getElementById("btn1").addEventListener("click", () => {
  const random = `hsl(${Math.random() * 360}, 70%, 80%)`;
  document.getElementById("output").style.backgroundColor = random;
});

// 2
document.getElementById("btn2").addEventListener("click", () => {
  const box = document.getElementById("box");
  const clone = box.cloneNode(true);
  document.getElementById("container").appendChild(clone);
});

// 3
document.getElementById("btn3").addEventListener("click", () => {
  const input = document.getElementById("input");
  const li = document.createElement("li");
  li.textContent = input.value;
  document.getElementById("list").appendChild(li);
  input.value = "";
});

// 4
document.getElementById("list2").addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    event.target.classList.toggle("selected");
  }
});
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Select and Change (3 minutes)

- **Requirement:** Select `#title` and change its `textContent` to "DOM Manipulation".

### Level 2: Add Class (3 minutes)

- **Requirement:** Add the class `highlight` to `#paragraph`.

### Level 3: Create Element (4 minutes)

- **Requirement:** Create a `div` with text "New box" and append it to `#container`.

### Level 4: Button Click (4 minutes)

- **Requirement:** When `#changeColor` is clicked, set `#box` background to `blue`.

### Level 5: Prevent Submit (4 minutes)

- **Requirement:** Prevent `#form` from submitting and log the input value.

### Level 6: Event Delegation (5 minutes)

- **Requirement:** Use event delegation on `#todoList` to remove the `li` when a `.delete` button inside it is clicked.

---

## Mini Project: To-Do List App

### Time

25 minutes

### Goal

Build a complete interactive to-do list using DOM selection, creation, events, and form handling.

### Requirements for the Students

1. Create an HTML page with:
   - Input field for new tasks
   - Add button
   - `ul` list for tasks
   - Filter buttons: All, Active, Completed
   - Counter showing active tasks

2. When the add button is clicked:
   - Create a new `li`
   - Add a checkbox and a delete button
   - Append to the list

3. When a checkbox is clicked:
   - Toggle the `completed` class on the `li`

4. When a delete button is clicked:
   - Remove the `li`

5. Filter buttons show only matching items.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do List</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .todo { display: flex; align-items: center; gap: 10px; padding: 8px; border-bottom: 1px solid #eee; }
    .completed { text-decoration: line-through; color: gray; }
    .filters button { margin: 5px; }
    .output { margin-top: 10px; }
  </style>
</head>
<body>
  <h1>To-Do List</h1>

  <div>
    <input type="text" id="taskInput" placeholder="New task" />
    <button id="addBtn">Add</button>
  </div>

  <div class="filters">
    <button data-filter="all">All</button>
    <button data-filter="active">Active</button>
    <button data-filter="completed">Completed</button>
  </div>

  <ul id="todoList"></ul>
  <p id="counter">0 active tasks</p>

  <script>
    const taskInput = document.getElementById("taskInput");
    const addBtn = document.getElementById("addBtn");
    const todoList = document.getElementById("todoList");
    const counter = document.getElementById("counter");
    const filterBtns = document.querySelectorAll(".filters button");

    function updateCounter() {
      const active = todoList.querySelectorAll(".todo:not(.completed)").length;
      counter.textContent = `${active} active task${active === 1 ? "" : "s"}`;
    }

    function addTask(text) {
      const li = document.createElement("li");
      li.className = "todo";

      const checkbox = document.createElement("input");
      checkbox.type = "checkbox";
      checkbox.addEventListener("change", () => {
        li.classList.toggle("completed");
        updateCounter();
      });

      const span = document.createElement("span");
      span.textContent = text;

      const deleteBtn = document.createElement("button");
      deleteBtn.textContent = "Delete";
      deleteBtn.addEventListener("click", () => {
        li.remove();
        updateCounter();
      });

      li.append(checkbox, span, deleteBtn);
      todoList.appendChild(li);
      updateCounter();
    }

    addBtn.addEventListener("click", () => {
      if (taskInput.value.trim() === "") return;
      addTask(taskInput.value.trim());
      taskInput.value = "";
    });

    filterBtns.forEach(btn => {
      btn.addEventListener("click", () => {
        const filter = btn.dataset.filter;
        const items = todoList.querySelectorAll(".todo");

        items.forEach(item => {
          const isCompleted = item.classList.contains("completed");
          if (filter === "all") {
            item.style.display = "flex";
          } else if (filter === "active") {
            item.style.display = isCompleted ? "none" : "flex";
          } else if (filter === "completed") {
            item.style.display = isCompleted ? "flex" : "none";
          }
        });
      });
    });
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why do we use `textContent` instead of `innerHTML` for the task text?"
- "How does event delegation help if we had 100 tasks?"
- "What does `preventDefault` do and where would we use it?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
const hero = document.getElementById("hero");
console.log(hero.textContent);
```

### Challenge 1.2

```javascript
const items = document.getElementsByClassName("item");
const lis = document.getElementsByTagName("li");
console.log(items.length, lis.length);
```

### Challenge 1.3

```javascript
const menuItems = document.querySelectorAll("#menu li");
```

### Challenge 2.1

```javascript
document.getElementById("message").textContent = "Welcome, User!";
```

### Challenge 2.2

```javascript
const avatar = document.getElementById("avatar");
avatar.src = "new.jpg";
avatar.alt = "User avatar";
```

### Challenge 3.1

```javascript
const li = document.createElement("li");
li.textContent = "Orange";
document.getElementById("fruits").appendChild(li);
```

### Challenge 3.2

```javascript
const p = document.createElement("p");
p.textContent = "Warning";
const target = document.getElementById("target");
target.before(p);
```

### Challenge 3.3

```javascript
const container = document.getElementById("container");
while (container.firstChild) {
  container.removeChild(container.firstChild);
}
```

### Challenge 4.1

```javascript
document.getElementById("toggleBtn").addEventListener("click", () => {
  document.getElementById("box").classList.toggle("visible");
});
```

### Challenge 4.2

```javascript
const banner = document.getElementById("banner");
banner.style.color = "red";
banner.style.backgroundColor = "yellow";
banner.style.fontSize = "24px";
```

### Challenge 5.1

```javascript
document.getElementById("btn").addEventListener("click", (event) => {
  console.log(event.target.textContent);
});
```

### Challenge 5.2

```javascript
const input = document.getElementById("message");
const output = document.getElementById("length");

input.addEventListener("input", () => {
  output.textContent = input.value.length;
});
```

### Challenge 5.3

```javascript
document.getElementById("link").addEventListener("click", (event) => {
  event.preventDefault();
  console.log("Link clicked");
});
```

### Challenge 5.4

```javascript
document.getElementById("list").addEventListener("click", (event) => {
  if (event.target.classList.contains("delete")) {
    event.target.closest("li").remove();
  }
});
```

### Challenge 6.1

```javascript
const form = document.getElementById("signup");
const nameInput = document.getElementById("name");
const error = document.getElementById("nameError");

form.addEventListener("submit", (event) => {
  event.preventDefault();
  if (nameInput.value.trim() === "") {
    error.textContent = "Name is required";
  } else {
    error.textContent = "";
  }
});
```

### Challenge 6.2

```javascript
const password = document.getElementById("password");
password.addEventListener("input", () => {
  password.style.borderColor = password.value.length >= 8 ? "green" : "red";
});
```

### Challenge 7.1

```javascript
const start = document.getElementById("start");
console.log(start.nextElementSibling.textContent);
```

### Challenge 7.2

```javascript
const template = document.querySelector(".template");
const clone = template.cloneNode(true);
clone.querySelector("h3").textContent = "New title";
document.getElementById("container").appendChild(clone);
```

### Individual Challenges Solutions

```javascript
// Level 1
document.getElementById("title").textContent = "DOM Manipulation";

// Level 2
document.getElementById("paragraph").classList.add("highlight");

// Level 3
const div = document.createElement("div");
div.textContent = "New box";
document.getElementById("container").appendChild(div);

// Level 4
document.getElementById("changeColor").addEventListener("click", () => {
  document.getElementById("box").style.backgroundColor = "blue";
});

// Level 5
document.getElementById("form").addEventListener("submit", (event) => {
  event.preventDefault();
  console.log(document.getElementById("input").value);
});

// Level 6
document.getElementById("todoList").addEventListener("click", (event) => {
  if (event.target.classList.contains("delete")) {
    event.target.closest("li").remove();
  }
});
```

</details>

---

## Review Questions

1. What is the DOM?
   - [ ] A programming language
   - [x] A programming interface for HTML/XML documents
   - [ ] A database
   - [ ] A styling language

2. Which method selects an element by ID?
   - [ ] querySelector
   - [ ] getElementsByClassName
   - [x] getElementById
   - [ ] getElementsByTagName

3. What does querySelectorAll return?
   - [ ] A single element
   - [x] A NodeList of all matching elements
   - [ ] An HTMLCollection
   - [ ] An array

4. What does preventDefault() do?
   - [x] Prevents default browser behavior
   - [ ] Removes event listeners
   - [ ] Stops event propagation
   - [ ] Deletes the element

5. What is event delegation?
   - [ ] Creating multiple event listeners
   - [x] Using a single listener on a parent to handle child events
   - [ ] Removing event listeners
   - [ ] Preventing events

6. What does classList.add() do?
   - [ ] Removes a class
   - [x] Adds a class
   - [ ] Checks for a class
   - [ ] Replaces a class

7. What is the difference between append and prepend?
   - [ ] No difference
   - [ ] append adds at beginning, prepend at end
   - [x] append adds at end, prepend at beginning
   - [ ] Both remove elements

8. What does cloneNode(true) do?
   - [ ] Clones only the element
   - [x] Clones element and all descendants
   - [ ] Clones nothing
   - [ ] Clones the parent only

9. What is the difference between parentNode and parentElement?
   - [ ] No difference
   - [x] parentNode can be any node, parentElement is always an element
   - [ ] parentNode is always an element
   - [ ] parentElement includes text nodes

10. What does addEventListener do?
    - [ ] Removes an event listener
    - [x] Adds an event listener to an element
    - [ ] Creates an element
    - [ ] Deletes an element

---

## Additional Resources

- [MDN: DOM Introduction](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [MDN: Locating DOM Elements](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements)
- [MDN: Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events)
- [MDN: classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
- [JavaScript.info: DOM](https://javascript.info/dom-navigation)
- [JavaScript.info: Events](https://javascript.info/introduction-browser-events)
