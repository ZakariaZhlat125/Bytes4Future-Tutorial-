# Session 11: BOM (Browser Object Model) — Active Learning Redesign

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

- "What does `window` represent?"
- "What will `setTimeout` do after the delay?"
- "How is `setInterval` different from `setTimeout`?"
- "What is the difference between `localStorage` and `sessionStorage`?"
- "What happens if we use `alert` too often?"

---

## Part 0: Warm-Up — The Browser is an Object (5 minutes)

### Problem

A student wants to show a message, ask a question, and wait a few seconds before doing something. All of this is possible through the browser.

### Guess

Ask: "What objects can we use to talk to the browser itself?"

### Explain

The `window` object is the global object in the browser. It contains methods for dialogs, timers, navigation, storage, and more.

### Live Code

```javascript
console.log(window.innerWidth);
console.log(window.innerHeight);

// Global variables are properties of window
let message = "Hello";
console.log(window.message);
```

### Review

`window` is the root object. `window.alert`, `window.setTimeout`, etc. are all built into it.

---

## Part 1: Dialogs

### 1.1 alert, confirm, prompt

#### Problem

We need simple ways to communicate with the user without building custom UI.

#### Live Code

```javascript
// alert
alert("Welcome!");

// confirm
const ok = confirm("Do you want to continue?");
console.log(ok); // true or false

// prompt
const name = prompt("What is your name?", "Guest");
console.log(name); // string or null
```

#### Challenge 1.1 — Confirm Delete (individual, 3 minutes)

- **Requirement:** Use `confirm` to ask if the user wants to delete something. Log `"Deleted"` if OK, `"Cancelled"` otherwise.
- **Time limit:** 3 minutes

---

## Part 2: Timers

### 2.1 setTimeout and clearTimeout

#### Problem

Run code after a delay.

#### Live Code

```javascript
const timeoutId = setTimeout(() => {
  console.log("Executed after 2 seconds");
}, 2000);

// Cancel before it runs
clearTimeout(timeoutId);
```

#### Challenge 2.1 — Delayed Message (individual, 3 minutes)

- **Requirement:** Show an alert with `"Hello"` after 3 seconds, then cancel it before it runs.
- **Time limit:** 3 minutes

### 2.2 setInterval and clearInterval

#### Problem

Run code repeatedly.

#### Live Code

```javascript
let count = 0;

const intervalId = setInterval(() => {
  count++;
  console.log("Count:", count);

  if (count >= 5) {
    clearInterval(intervalId);
  }
}, 1000);
```

#### Challenge 2.2 — Countdown Timer (individual, 5 minutes)

- **Requirement:** Count down from 10 to 0, logging each second. Print `"Time's up!"` at the end and stop the interval.
- **Time limit:** 5 minutes

---

## Bug Hunt 1

### Problem

Find the bugs in this timer code.

```javascript
let counter = 0;
const interval = setInterval(() => {
  counter++;
  console.log(counter);
}, 1000);

setTimeout(() => {
  clearInterval(intervalId);
}, 5000);
```

### Issues

1. The interval ID is stored in `interval`, but `clearInterval` uses `intervalId` which is not defined.

### Fixed Version

```javascript
let counter = 0;
const intervalId = setInterval(() => {
  counter++;
  console.log(counter);
}, 1000);

setTimeout(() => {
  clearInterval(intervalId);
}, 5000);
```

### Points

1 point for finding the bug.

---

## Part 3: Location and Navigation

### 3.1 Location Object

#### Problem

We want to know and control the URL.

#### Live Code

```javascript
console.log(window.location.href);
console.log(window.location.protocol);
console.log(window.location.hostname);
console.log(window.location.pathname);
console.log(window.location.search);
console.log(window.location.hash);
```

#### Challenge 3.1 — Show URL Info (individual, 3 minutes)

- **Requirement:** Display the current `href`, `hostname`, and `pathname` in the console.
- **Time limit:** 3 minutes

### 3.2 Navigation Methods

#### Live Code

```javascript
// window.location.href = "https://example.com"; // navigate
// window.location.reload();                       // reload
// window.location.assign("https://example.com");  // navigate with history
// window.location.replace("https://example.com"); // navigate without history
```

#### Challenge 3.2 — Add Query Parameter (individual, 4 minutes)

- **Requirement:** Use `URL` and `history.pushState` to add `?page=2` to the current URL without reloading.
- **Time limit:** 4 minutes

---

## Part 4: History

### 4.1 History Navigation

#### Problem

Move through the browser's back and forward history.

#### Live Code

```javascript
console.log(window.history.length);

// window.history.back();
// window.history.forward();
// window.history.go(-2);
```

### 4.2 pushState and popstate

#### Live Code

```javascript
window.history.pushState({ page: 1 }, "Page 1", "?page=1");

window.addEventListener("popstate", (event) => {
  console.log("State changed:", event.state);
});
```

#### Challenge 4.1 — Simulate Navigation (individual, 4 minutes)

- **Requirement:** Push a state `{ page: 2 }` with URL `?page=2` and listen for `popstate`.
- **Time limit:** 4 minutes

---

## Part 5: Scroll

### 5.1 Scroll Methods

#### Problem

Control the page scroll position.

#### Live Code

```javascript
window.scrollTo({ top: 500, behavior: "smooth" });
window.scrollBy({ top: 100, behavior: "smooth" });

const section = document.getElementById("section2");
section.scrollIntoView({ behavior: "smooth" });
```

#### Challenge 5.1 — Scroll to Top (individual, 4 minutes)

- **Requirement:** When `#topBtn` is clicked, smoothly scroll to the top of the page.
- **Time limit:** 4 minutes

### 5.2 Scroll Indicator

#### Live Code

```javascript
const bar = document.getElementById("progressBar");

window.addEventListener("scroll", () => {
  const scrollTop = window.scrollY;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  const percent = (scrollTop / docHeight) * 100;
  bar.style.width = percent + "%";
});
```

---

## Part 6: Storage

### 6.1 localStorage

#### Problem

Save data that persists after the browser is closed.

#### Live Code

```javascript
localStorage.setItem("username", "John");
const name = localStorage.getItem("username");
console.log(name);

localStorage.removeItem("username");
// localStorage.clear();
```

#### Challenge 6.1 — Save a Theme (individual, 4 minutes)

- **Requirement:** Save `"dark"` as `theme` in `localStorage`, then read it and log it.
- **Time limit:** 4 minutes

### 6.2 Storing Objects

#### Problem

`localStorage` only stores strings.

#### Live Code

```javascript
const user = { name: "John", theme: "dark" };
localStorage.setItem("user", JSON.stringify(user));

const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);
```

#### Challenge 6.2 — Save Settings (individual, 4 minutes)

- **Requirement:** Save an object `{ language: "en", notifications: true }` to `localStorage`, then load and log `language`.
- **Time limit:** 4 minutes

### 6.3 sessionStorage

#### Live Code

```javascript
sessionStorage.setItem("temp", "This clears on tab close");
console.log(sessionStorage.getItem("temp"));
```

#### Challenge 6.3 — local vs session (individual, 3 minutes)

- **Requirement:** Explain the difference between `localStorage` and `sessionStorage`.
- **Time limit:** 3 minutes

---

## Bug Hunt 2

### Problem

Find the bugs in this storage code.

```javascript
const user = { name: "John", age: 30 };
localStorage.setItem("user", user);

const saved = localStorage.getItem("user");
console.log(saved.name);
```

### Issues

1. `localStorage.setItem("user", user)` stores `[object Object]` because the object is converted to a string.
2. `saved.name` will not work because `saved` is a string, not an object.

### Fixed Version

```javascript
const user = { name: "John", age: 30 };
localStorage.setItem("user", JSON.stringify(user));

const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);
```

### Points

1 point per found issue.

---

## Part 7: Practical BOM Project — Color App

### Live Code

```javascript
const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00"];

function createPalette() {
  const palette = document.getElementById("palette");

  colors.forEach(color => {
    const box = document.createElement("div");
    box.style.width = "40px";
    box.style.height = "40px";
    box.style.backgroundColor = color;
    box.style.display = "inline-block";
    box.style.margin = "5px";
    box.style.cursor = "pointer";

    box.addEventListener("click", () => {
      document.body.style.backgroundColor = color;
      localStorage.setItem("backgroundColor", color);
    });

    palette.appendChild(box);
  });
}

function loadColor() {
  const saved = localStorage.getItem("backgroundColor");
  if (saved) {
    document.body.style.backgroundColor = saved;
  }
}

createPalette();
loadColor();
```

#### Challenge 7.1 — Theme Switcher (individual, 5 minutes)

- **Requirement:** Add a "Random Color" button that picks a random color, applies it, and saves it.
- **Time limit:** 5 minutes

---

## Group Challenge: BOM Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team writes the code for one task.
- **Scoring:** 2 points per working solution. The first team to finish all four gets 2 bonus points.

### Tasks

1. Create a countdown that updates `#timer` every second from 10 to 0 and stops at 0.
2. Save and load the value of `#input` using `localStorage`.
3. Add a button that scrolls `#section` into view smoothly.
4. Use `confirm` before removing an item from a list.

### Instructor Answer Key

```javascript
// 1
let count = 10;
const timer = document.getElementById("timer");
const intervalId = setInterval(() => {
  timer.textContent = count;
  count--;
  if (count < 0) {
    clearInterval(intervalId);
    timer.textContent = "Time's up!";
  }
}, 1000);

// 2
const input = document.getElementById("input");
input.value = localStorage.getItem("savedInput") || "";
input.addEventListener("input", () => {
  localStorage.setItem("savedInput", input.value);
});

// 3
document.getElementById("scrollBtn").addEventListener("click", () => {
  document.getElementById("section").scrollIntoView({ behavior: "smooth" });
});

// 4
document.getElementById("list").addEventListener("click", (event) => {
  if (event.target.classList.contains("delete")) {
    const confirmed = confirm("Delete this item?");
    if (confirmed) {
      event.target.closest("li").remove();
    }
  }
});
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Dialog (3 minutes)

- **Requirement:** Show an `alert` with the current time when `#showTime` is clicked.

### Level 2: Timeout (3 minutes)

- **Requirement:** Log `"Hello after 3 seconds"` 3 seconds after the page loads.

### Level 3: Interval (4 minutes)

- **Requirement:** Count from 1 to 10, one number per second. Stop at 10.

### Level 4: URL (4 minutes)

- **Requirement:** Log the current `hostname` and `pathname`.

### Level 5: Storage (4 minutes)

- **Requirement:** Save and load a username from `localStorage`.

### Level 6: Scroll (5 minutes)

- **Requirement:** Create a fixed button that scrolls the page to the top when clicked.

---

## Mini Project: Personal Notes App

### Time

25 minutes

### Goal

Build a notes app that uses `localStorage` to persist notes across page reloads.

### Requirements for the Students

1. Create an HTML page with:
   - Input for note title
   - Textarea for note body
   - "Add Note" button
   - `div` to display notes
   - "Search" input

2. Store notes in `localStorage` as an array of objects.

3. On page load, load and display notes.

4. Each note should have a delete button.

5. Search should filter notes by title.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notes App</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .note { background: #f9f9f9; padding: 10px; margin: 10px 0; border-radius: 4px; }
    .note button { float: right; }
    input, textarea, button { padding: 8px; margin: 5px; }
  </style>
</head>
<body>
  <h1>Notes App</h1>

  <input type="text" id="title" placeholder="Title" />
  <textarea id="body" placeholder="Note body"></textarea>
  <button id="addBtn">Add Note</button>

  <input type="text" id="search" placeholder="Search notes" />
  <div id="notes"></div>

  <script>
    const STORAGE_KEY = "notes";

    function getNotes() {
      const saved = localStorage.getItem(STORAGE_KEY);
      return saved ? JSON.parse(saved) : [];
    }

    function saveNotes(notes) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
    }

    function renderNotes(filter = "") {
      const notes = getNotes().filter(note =>
        note.title.toLowerCase().includes(filter.toLowerCase())
      );

      const container = document.getElementById("notes");
      container.innerHTML = "";

      notes.forEach((note, index) => {
        const div = document.createElement("div");
        div.className = "note";

        const title = document.createElement("strong");
        title.textContent = note.title;

        const body = document.createElement("p");
        body.textContent = note.body;

        const deleteBtn = document.createElement("button");
        deleteBtn.textContent = "Delete";
        deleteBtn.addEventListener("click", () => {
          const allNotes = getNotes();
          allNotes.splice(index, 1);
          saveNotes(allNotes);
          renderNotes(document.getElementById("search").value);
        });

        div.append(title, body, deleteBtn);
        container.appendChild(div);
      });
    }

    document.getElementById("addBtn").addEventListener("click", () => {
      const title = document.getElementById("title").value.trim();
      const body = document.getElementById("body").value.trim();
      if (!title) return;

      const notes = getNotes();
      notes.push({ title, body });
      saveNotes(notes);

      document.getElementById("title").value = "";
      document.getElementById("body").value = "";
      renderNotes();
    });

    document.getElementById("search").addEventListener("input", (event) => {
      renderNotes(event.target.value);
    });

    renderNotes();
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why do we need `JSON.stringify` and `JSON.parse`?"
- "What happens if `localStorage` is empty when the page loads?"
- "How can we edit a note instead of deleting it?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
const confirmed = confirm("Delete this item?");
console.log(confirmed ? "Deleted" : "Cancelled");
```

### Challenge 2.1

```javascript
const timeoutId = setTimeout(() => alert("Hello"), 3000);
clearTimeout(timeoutId);
```

### Challenge 2.2

```javascript
let count = 10;
const intervalId = setInterval(() => {
  console.log(count);
  count--;
  if (count < 0) {
    clearInterval(intervalId);
    console.log("Time's up!");
  }
}, 1000);
```

### Challenge 3.1

```javascript
console.log(location.href);
console.log(location.hostname);
console.log(location.pathname);
```

### Challenge 3.2

```javascript
const url = new URL(window.location.href);
url.searchParams.set("page", "2");
window.history.pushState({}, "", url);
```

### Challenge 4.1

```javascript
window.history.pushState({ page: 2 }, "Page 2", "?page=2");

window.addEventListener("popstate", (event) => {
  console.log("State:", event.state);
});
```

### Challenge 5.1

```javascript
document.getElementById("topBtn").addEventListener("click", () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
});
```

### Challenge 6.1

```javascript
localStorage.setItem("theme", "dark");
console.log(localStorage.getItem("theme"));
```

### Challenge 6.2

```javascript
const settings = { language: "en", notifications: true };
localStorage.setItem("settings", JSON.stringify(settings));

const saved = JSON.parse(localStorage.getItem("settings"));
console.log(saved.language);
```

### Challenge 6.3

`localStorage` persists after the tab/browser is closed. `sessionStorage` is cleared when the tab is closed.

### Challenge 7.1

```javascript
const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00"];

document.getElementById("randomBtn").addEventListener("click", () => {
  const color = colors[Math.floor(Math.random() * colors.length)];
  document.body.style.backgroundColor = color;
  localStorage.setItem("backgroundColor", color);
});
```

### Individual Challenges Solutions

```javascript
// Level 1
document.getElementById("showTime").addEventListener("click", () => {
  alert(new Date().toLocaleTimeString());
});

// Level 2
setTimeout(() => console.log("Hello after 3 seconds"), 3000);

// Level 3
let count = 1;
const intervalId = setInterval(() => {
  console.log(count);
  if (count === 10) clearInterval(intervalId);
  count++;
}, 1000);

// Level 4
console.log(location.hostname, location.pathname);

// Level 5
localStorage.setItem("username", "John");
console.log(localStorage.getItem("username"));

// Level 6
const btn = document.createElement("button");
btn.textContent = "Top";
btn.style.position = "fixed";
btn.style.bottom = "20px";
btn.style.right = "20px";
btn.addEventListener("click", () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
});
document.body.appendChild(btn);
```

</details>

---

## Review Questions

1. What is the BOM?
   - [ ] A programming language
   - [x] Browser Object Model for browser interaction
   - [ ] Document Object Model
   - [ ] A styling framework

2. What does alert() do?
   - [ ] Shows a confirmation dialog
   - [x] Shows an alert message with OK button
   - [ ] Shows an input dialog
   - [ ] Shows a file dialog

3. What does setTimeout() do?
   - [ ] Executes code repeatedly
   - [x] Executes code after a delay
   - [ ] Executes code immediately
   - [ ] Cancels a timeout

4. What does setInterval() do?
   - [ ] Executes code once after delay
   - [x] Executes code repeatedly at intervals
   - [ ] Cancels an interval
   - [ ] Shows an alert

5. What is the difference between localStorage and sessionStorage?
   - [ ] No difference
   - [x] localStorage persists after tab close, sessionStorage doesn't
   - [ ] sessionStorage persists after tab close, localStorage doesn't
   - [ ] Both clear on tab close

6. What does window.location.href do?
   - [ ] Returns the current URL
   - [x] Navigates to a new URL
   - [ ] Reloads the page
   - [ ] Returns the browser history

7. What does window.history.back() do?
   - [ ] Goes forward in history
   - [x] Goes back in history
   - [ ] Reloads the page
   - [ ] Clears history

8. What does window.scrollTo() do?
   - [ ] Scrolls element into view
   - [x] Scrolls window to specific position
   - [ ] Scrolls by offset
   - [ ] Gets scroll position

9. What does confirm() return?
   - [ ] Always true
   - [ ] Always false
   - [x] true if OK, false if Cancel
   - [ ] The entered text

10. What does clearTimeout() do?
    - [ ] Starts a timeout
    - [x] Cancels a scheduled timeout
    - [ ] Starts an interval
    - [ ] Cancels an interval

---

## Additional Resources

- [MDN: Window Object](https://developer.mozilla.org/en-US/docs/Web/API/Window)
- [MDN: localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN: sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- [MDN: setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout)
- [MDN: setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval)
- [MDN: Location](https://developer.mozilla.org/en-US/docs/Web/API/Location)
- [MDN: History](https://developer.mozilla.org/en-US/docs/Web/API/History)
