# Session 10: DOM Manipulation — Student Active-Learning Lab

Welcome to this session. Today you will make web pages do things. Do not just read — open the browser, try the code, predict what will change, and fix bugs.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Try to picture what the code should do before you run it.
3. **Write your prediction.**
4. **Create the HTML, type the JavaScript, and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before looking at the answer key.**

---

## Part 0: Warm-Up — The Static Page

### The problem

A page has a heading, but the text needs to change when a button is clicked.

```html
<h1 id="title">Hello</h1>
<button>Change</button>
```

### 🤔 Think

How can JavaScript change the heading without reloading the page?

```text
My idea: _______________________________________________________________
```

### 🔮 Predict

```javascript
const title = document.getElementById("title");
const button = document.querySelector("button");

button.addEventListener("click", () => {
  title.textContent = "Hello, World!";
});
```

What will the heading say before the click? What will it say after the click?

### ✅ Result

Create the HTML file, add the script, and click the button.

### 🧠 Discover

The browser turns your HTML into a tree of objects. JavaScript can **select** and **change** those objects.

### 🧪 Experiment

- Change `textContent` to `innerHTML` and try setting it to `"<em>Hello, World!</em>"`. What is the difference?
- What happens if you use `getElementById("title")` but the `id` is `"heading"`?

---

## Part 1: Selecting Elements

### 1.1 `getElementById`

### The problem

Select a specific element.

### 🔮 Predict

```html
<div id="main">Main content</div>
```

```javascript
const main = document.getElementById("main");
console.log(main.textContent);
```

What will `console.log` print?

### ✅ Result

Try it.

### 🧠 Why?

`document.getElementById` returns the single element with that `id`. It is one of the fastest selectors.

### Challenge 1.1 — Get the Element

Get the element with `id="hero"` and print its `textContent`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const hero = document.getElementById("hero");
console.log(hero.textContent);
```

</details>

---

### 1.2 Class and Tag Selectors

### The problem

Select many elements at once.

### 🔮 Predict

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

What are the two `console.log` outputs?

### ✅ Result

Try it.

### 🧠 Why?

- `getElementsByClassName` returns all elements with that class.
- `getElementsByTagName` returns all elements with that tag name.

### Challenge 1.2 — Count Elements

Count how many `.item` elements and `li` elements are on the page.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const items = document.getElementsByClassName("item");
const lis = document.getElementsByTagName("li");
console.log(items.length, lis.length);
```

</details>

---

### 1.3 `querySelector` and `querySelectorAll`

### The problem

Select with CSS-style selectors.

### 🔮 Predict

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

How many `console.log` lines will the `forEach` produce?

### ✅ Result

Try it.

### 🧠 Why?

- `querySelector` returns the **first** match.
- `querySelectorAll` returns a **NodeList** with all matches.

### Challenge 1.3 — Select Items

Use `querySelectorAll` to select all `li` elements inside `#menu`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const menuItems = document.querySelectorAll("#menu li");
```

</details>

---

## Part 2: Reading and Changing Content

### 2.1 `textContent` vs `innerHTML`

### The problem

Change text safely or add real HTML.

### 🔮 Predict

```javascript
const box = document.getElementById("box");

box.textContent = "<strong>Bold?</strong>";
```

Will the text appear bold or as plain text?

### ✅ Result

Try it.

### 🧪 Experiment

Now try this:

```javascript
box.innerHTML = "<strong>Bold!</strong>";
```

What is the difference between `textContent` and `innerHTML`?

### Challenge 2.1 — Safe Update

Change `#message` to show `Welcome, User!` using only `textContent`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("message").textContent = "Welcome, User!";
```

</details>

---

### 2.2 Attributes

### The problem

Read and change element attributes like `href`, `src`, and `alt`.

### 🔮 Predict

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

console.log(pic.hasAttribute("title"));
```

What will the last `console.log` print?

### ✅ Result

Try it.

### 🧠 Why?

You can use `setAttribute`/`getAttribute` or direct properties like `.src` and `.alt`.

### Challenge 2.2 — Change Image Source

Change the `src` and `alt` of `#avatar` to a new image and description.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const avatar = document.getElementById("avatar");
avatar.src = "new.jpg";
avatar.alt = "User avatar";
```

</details>

---

## Part 3: Creating and Inserting Elements

### 3.1 Creating and Appending

### The problem

Add a new item to a list from JavaScript.

### 🔮 Predict

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

How many `li` elements will the list have after this runs?

### ✅ Result

Try it.

### 🧠 Why?

1. `document.createElement("li")` makes a new `<li>`.
2. `li.textContent = "Banana"` adds text.
3. `fruits.appendChild(li)` adds it to the end of the list.

### Challenge 3.1 — Add to List

Create a `li` with text `"Orange"` and add it to `#fruits`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const li = document.createElement("li");
li.textContent = "Orange";
document.getElementById("fruits").appendChild(li);
```

</details>

---

### 3.2 Modern Insertion Methods

### The problem

Put elements at the start, end, or before another element.

### 🔮 Predict

```html
<div id="parent">
  <div id="reference">Reference</div>
</div>
```

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

What is the final order of the `div` elements inside `#parent`?

### ✅ Result

Try it.

### 🧠 Why?

- `prepend` adds at the beginning.
- `append` adds at the end.
- `before` adds right before a reference element.

### Challenge 3.2 — Insert Before

Create a `<p>Warning</p>` and insert it before `#target`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const p = document.createElement("p");
p.textContent = "Warning";
const target = document.getElementById("target");
target.before(p);
```

</details>

---

### 3.3 Removing and Replacing

### The problem

Remove or replace elements.

### 🔮 Predict

```javascript
const old = document.getElementById("old");
old.remove();

const child = document.getElementById("child");
const replacement = document.createElement("div");
replacement.textContent = "New";
child.parentNode.replaceChild(replacement, child);
```

What happens to `#old`? What happens to `#child`?

### ✅ Result

Try it.

### Challenge 3.3 — Clear Container

Remove all children from `#container`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const container = document.getElementById("container");
while (container.firstChild) {
  container.removeChild(container.firstChild);
}
```

</details>

**Hint:** `while (container.firstChild) { container.removeChild(container.firstChild); }`

---

## Part 4: Styling and Classes

### 4.1 `classList`

### The problem

Add, remove, and toggle CSS classes.

### 🔮 Predict

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

What will the `console.log` print? What classes will the `div` have at the end?

### ✅ Result

Try it.

### 🧠 Why?

`classList` is the safest way to work with CSS classes. Use `add`, `remove`, `toggle`, and `contains`.

### Challenge 4.1 — Toggle Class

Toggle the class `visible` on `#box` when `#toggleBtn` is clicked.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("toggleBtn").addEventListener("click", () => {
  document.getElementById("box").classList.toggle("visible");
});
```

</details>

---

### 4.2 Inline Styles

### The problem

Change the style of an element directly.

### 🔮 Predict

```javascript
const box = document.getElementById("box");
box.style.color = "white";
box.style.backgroundColor = "blue";
box.style.padding = "20px";
```

What color will the text be? What CSS property changes the background?

### ✅ Result

Try it.

### 🧠 Why?

CSS properties with a dash become camelCase in JavaScript: `backgroundColor`, `fontSize`.

### Challenge 4.2 — Style Element

Set `#banner` text color to red, background to yellow, and font size to `24px`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const banner = document.getElementById("banner");
banner.style.color = "red";
banner.style.backgroundColor = "yellow";
banner.style.fontSize = "24px";
```

</details>

---

## Part 5: DOM Events

### 5.1 `addEventListener`

### The problem

Run code when a user clicks a button.

### 🔮 Predict

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

How many `console.log` lines will appear after one click?

### ✅ Result

Click the button and check the console.

### 🧠 Why?

`addEventListener` waits for an event on an element and then runs a function. `event.target` is the element that was clicked.

### Challenge 5.1 — Log Click

When `#btn` is clicked, log the button text.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("btn").addEventListener("click", (event) => {
  console.log(event.target.textContent);
});
```

</details>

---

### 5.2 Event Types

### The problem

Run code on different user actions.

### 🔮 Predict

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

When will each event fire?

### ✅ Result

Type in the input, click into it, and click out.

### 🧪 Experiment

Add a `keydown` listener. What does `event.key` show?

### Challenge 5.2 — Input Event

Show the length of `#message` input below it while typing.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const input = document.getElementById("message");
const output = document.getElementById("length");

input.addEventListener("input", () => {
  output.textContent = input.value.length;
});
```

</details>

---

### 5.3 `preventDefault`

### The problem

A form reloads the page on submit. We want to stop that.

### 🔮 Predict

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

Will the page reload when the form is submitted?

### ✅ Result

Try it.

### 🧠 Why?

`preventDefault()` stops the browser's default action. For forms, that means the page does not reload.

### Challenge 5.3 — Prevent Link

Add a click listener to `#link` that prevents navigation and logs `"Link clicked"`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("link").addEventListener("click", (event) => {
  event.preventDefault();
  console.log("Link clicked");
});
```

</details>

---

### 5.4 Event Delegation

### The problem

A list has 100 items. We do not want 100 listeners.

### 🔮 Predict

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

If you click on a `li`, what happens? Where is the listener attached?

### ✅ Result

Try clicking each item.

### 🧠 Why?

The listener is on the parent `<ul>`. When a child is clicked, the event bubbles up. This is **event delegation** — one listener handles many children.

### Challenge 5.4 — Delete with Delegation

When a `.delete` button inside `#list` is clicked, remove its parent `li`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("list").addEventListener("click", (event) => {
  if (event.target.classList.contains("delete")) {
    event.target.closest("li").remove();
  }
});
```

</details>

---

## Part 6: Form Validation

### 6.1 Basic Validation

### The problem

A form should not submit if fields are empty or invalid.

### 🔮 Predict

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

What happens if the email has no `@`? What happens if it does?

### ✅ Result

Try it.

### 🧠 Why?

Validation stops bad data before it is submitted. `return` stops the rest of the function.

### Challenge 6.1 — Validate Name

If `#name` is empty, show `"Name is required"` in `#nameError`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

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

</details>

---

### 6.2 Real-Time Validation

### The problem

Give feedback as the user types.

### 🔮 Predict

```javascript
const emailInput = document.getElementById("email");

emailInput.addEventListener("input", function() {
  const value = this.value;
  const isValid = value.includes("@") && value.includes(".");
  this.style.borderColor = isValid ? "green" : "red";
});
```

What color will the border be while the user types `"hello"`? What color for `"test@example.com"`?

### ✅ Result

Try it.

### Challenge 6.2 — Live Password Check

While typing in `#password`, make the border green if length >= 8, red otherwise.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const password = document.getElementById("password");
password.addEventListener("input", () => {
  password.style.borderColor = password.value.length >= 8 ? "green" : "red";
});
```

</details>

---

## Bug Hunt 1

### The mission

The following code has three deliberate bugs. Do not run it yet. Read it and write what you think is wrong.

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

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________
3. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const greeting = document.getElementById("greeting");
const button = document.getElementById("btn");

button.addEventListener("click", function() {
  greeting.textContent = "Hello";
  greeting.classList.add("active");
  button.textContent = "Done";
});
```

</details>

---

## Part 7: DOM Traversal and Cloning

### 7.1 Traversing

### The problem

Move around the DOM tree without a selector.

### 🔮 Predict

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

What will each `console.log` print?

### ✅ Result

Try it.

### 🧠 Why?

- `parentElement` is the parent element.
- `nextElementSibling` is the next element at the same level.
- `children` is the child elements.

### Challenge 7.1 — Find Sibling

Log the `textContent` of `#start`'s next sibling.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const start = document.getElementById("start");
console.log(start.nextElementSibling.textContent);
```

</details>

---

### 7.2 Cloning

### The problem

Copy an existing element and change it.

### 🔮 Predict

```javascript
const original = document.getElementById("card");
const clone = original.cloneNode(true);
clone.id = "card2";
clone.querySelector("h3").textContent = "Copy";

document.getElementById("container").appendChild(clone);
```

Will the original change? Will the clone have the same content at first?

### ✅ Result

Try it.

### 🧠 Why?

`cloneNode(true)` copies an element and all of its children. `cloneNode(false)` copies only the element itself.

### Challenge 7.2 — Clone Item

Clone `.template` and change the title before appending it.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const template = document.querySelector(".template");
const clone = template.cloneNode(true);
clone.querySelector("h3").textContent = "New title";
document.getElementById("container").appendChild(clone);
```

</details>

---

## Bug Hunt 2

### The mission

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
});
```

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const addBtn = document.getElementById("add");
const taskInput = document.getElementById("task");
const list = document.getElementById("todo");

addBtn.addEventListener("click", () => {
  const li = document.createElement("li");
  li.textContent = taskInput.value;
  list.append(li);
  taskInput.value = "";
});

list.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    event.target.remove();
  }
});
```

</details>

---

## Group Challenge: DOM Builder Race

If you are in a group, split into teams of 2 or 3.

Each team writes the DOM code for one task.

### Tasks

1. When `#btn1` is clicked, change `#output` background to a random color.
2. When `#btn2` is clicked, duplicate `#box` and append it to `#container`.
3. Add a new `li` from `#input` to `#list` when `#btn3` is clicked, and clear the input.
4. Use event delegation on `#list2` so clicking any `li` toggles the class `selected`.

Set a timer for 12 minutes.

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Select and Change

Select `#title` and change its `textContent` to `"DOM Manipulation"`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("title").textContent = "DOM Manipulation";
```

</details>

### Level 2: Add Class

Add the class `highlight` to `#paragraph`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("paragraph").classList.add("highlight");
```

</details>

### Level 3: Create Element

Create a `div` with text `"New box"` and append it to `#container`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const div = document.createElement("div");
div.textContent = "New box";
document.getElementById("container").appendChild(div);
```

</details>

### Level 4: Button Click

When `#changeColor` is clicked, set `#box` background to `blue`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("changeColor").addEventListener("click", () => {
  document.getElementById("box").style.backgroundColor = "blue";
});
```

</details>

### Level 5: Prevent Submit

Prevent `#form` from submitting and log the input value.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("form").addEventListener("submit", (event) => {
  event.preventDefault();
  console.log(document.getElementById("input").value);
});
```

</details>

### Level 6: Event Delegation

Use event delegation on `#todoList` to remove the `li` when a `.delete` button inside it is clicked.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("todoList").addEventListener("click", (event) => {
  if (event.target.classList.contains("delete")) {
    event.target.closest("li").remove();
  }
});
```

</details>

---

## Mini Project: To-Do List App

### Time

25 minutes

### Goal

Build a complete interactive to-do list using DOM selection, creation, events, and form handling.

### Requirements

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

### Questions to think about

1. Why do we use `textContent` instead of `innerHTML` for the task text?
2. How does event delegation help if we had 100 tasks?
3. What does `preventDefault` do and where would we use it?
4. What happens if the user adds an empty task?

### Extension ideas

- Add a "Clear completed" button.
- Save tasks to `localStorage`.
- Add a delete button that uses event delegation instead of its own listener.

---

## Review Questions

Answer these before you finish.

1. What is the DOM?
   - [ ] A programming language
   - [ ] A programming interface for HTML/XML documents
   - [ ] A database
   - [ ] A styling language

2. Which method selects an element by ID?
   - [ ] querySelector
   - [ ] getElementsByClassName
   - [ ] getElementById
   - [ ] getElementsByTagName

3. What does querySelectorAll return?
   - [ ] A single element
   - [ ] A NodeList of all matching elements
   - [ ] An HTMLCollection
   - [ ] An array

4. What does preventDefault() do?
   - [ ] Prevents default browser behavior
   - [ ] Removes event listeners
   - [ ] Stops event propagation
   - [ ] Deletes the element

5. What is event delegation?
   - [ ] Creating multiple event listeners
   - [ ] Using a single listener on a parent to handle child events
   - [ ] Removing event listeners
   - [ ] Preventing events

6. What does classList.add() do?
   - [ ] Removes a class
   - [ ] Adds a class
   - [ ] Checks for a class
   - [ ] Replaces a class

7. What is the difference between append and prepend?
   - [ ] No difference
   - [ ] append adds at beginning, prepend at end
   - [ ] append adds at end, prepend at beginning
   - [ ] Both remove elements

8. What does cloneNode(true) do?
   - [ ] Clones only the element
   - [ ] Clones element and all descendants
   - [ ] Clones nothing
   - [ ] Clones the parent only

9. What is the difference between parentNode and parentElement?
   - [ ] No difference
   - [ ] parentNode can be any node, parentElement is always an element
   - [ ] parentNode is always an element
   - [ ] parentElement includes text nodes

10. What does addEventListener do?
    - [ ] Removes an event listener
    - [ ] Adds an event listener to an element
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

---

<details>
<summary>Answer Key — Try everything first!</summary>

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

### Group Challenge Answer Key

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

</details>
