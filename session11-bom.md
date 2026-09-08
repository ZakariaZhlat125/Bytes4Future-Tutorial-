# Session 11: BOM (Browser Object Model) — Student Active-Learning Lab

Open any website and it asks for your name, saves your theme, counts down, or scrolls back to the top. Behind all of that is the **Browser Object Model (BOM)**: a set of objects that let JavaScript talk to the browser itself.

Do not just read — predict, type, run, and fix.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Do not look at the code yet.
3. **Write your prediction** in a comment or notebook.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before you look at the answer key.**

---

## Part 0: Warm-Up — The Browser is an Object

### The problem

A student wants to show a message, ask a question, and wait a few seconds before doing something. All of this is possible through the browser.

### 🤔 Think

What object in the browser might contain `alert`, `setTimeout`, and `location`? Write your guess.

### 🔮 Predict

What will the last line print?

```javascript
console.log(window.innerWidth);
console.log(window.innerHeight);

let message = "Hello";
console.log(window.message);
```

### ✅ Result

Run the code.

### 🧠 Why?

`window` is the global object in the browser. `window.alert`, `window.setTimeout`, `window.location`, and even global variables live inside it.

### 🧪 Experiment

Change `let message` to a `const`. Does `window.message` still work? Why or why not?

---

## Part 1: Dialogs

### 1.1 alert, confirm, prompt

### The problem

We need simple ways to communicate with the user without building custom UI.

### 🔮 Predict

What will `ok` and `name` contain after this code runs?

```javascript
alert("Welcome!");

const ok = confirm("Do you want to continue?");
const name = prompt("What is your name?", "Guest");

console.log(ok);   // ?
console.log(name); // ?
```

### ✅ Result

Run the code.

### 🧠 Why?

- `alert` shows a message with an OK button.
- `confirm` returns `true` if the user clicks OK, otherwise `false`.
- `prompt` returns the entered string or `null` if the user clicks Cancel.

### 🧪 Experiment

Run `prompt` and click Cancel. What is the value of `name`?

### Challenge 1.1 — Confirm Delete

Use `confirm` to ask if the user wants to delete something. Log `"Deleted"` if OK, `"Cancelled"` otherwise.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const confirmed = confirm("Delete this item?");
console.log(confirmed ? "Deleted" : "Cancelled");
```

</details>


---

## Part 2: Timers

### 2.1 setTimeout and clearTimeout

### The problem

Run code after a delay.

### 🔮 Predict

Will the message below print? If so, when? What does `clearTimeout` do?

```javascript
const timeoutId = setTimeout(() => {
  console.log("Executed after 2 seconds");
}, 2000);

clearTimeout(timeoutId);
```

### ✅ Result

Run it.

### 🧠 Why?

`setTimeout` schedules one execution after the delay. `clearTimeout` cancels it before it runs.

### 🧪 Experiment

Call `clearTimeout` after 3 seconds instead of immediately. Does the message print?

### Challenge 2.1 — Delayed Message

Show an alert with `"Hello"` after 3 seconds, then cancel it before it runs.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const timeoutId = setTimeout(() => alert("Hello"), 3000);
clearTimeout(timeoutId);
```

</details>


---

### 2.2 setInterval and clearInterval

### The problem

Run code repeatedly.

### 🔮 Predict

How many times will this log before it stops? What is the last value of `count`?

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

### ✅ Result

Run it.

### 🧠 Why?

`setInterval` runs the callback again and again. You need `clearInterval` to stop it.

### 🧪 Experiment

Change `1000` to `500`. What changes? How long does the whole example take now?

### Challenge 2.2 — Countdown Timer

Count down from 10 to 0, logging each second. Print `"Time's up!"` at the end and stop the interval.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

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

</details>


---

## Bug Hunt 1

### The mission

Find the bugs in this timer code. Do not run it yet. Read and write what you think is wrong.

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

### 🐛 What I think is wrong

1. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

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

</details>


---

## Part 3: Location and Navigation

### 3.1 Location Object

### The problem

We want to know and control the URL.

### 🔮 Predict

What do you think `window.location.href` will contain for this page?

### ✅ Result

Run this:

```javascript
console.log(window.location.href);
console.log(window.location.protocol);
console.log(window.location.hostname);
console.log(window.location.pathname);
console.log(window.location.search);
console.log(window.location.hash);
```

### 🧠 Why?

The `location` object holds every part of the current address. You can read it and also change it to navigate.

### 🧪 Experiment

Type `window.location.hash = "#test"` in the console. What changes in the address bar?

### Challenge 3.1 — Show URL Info

Display the current `href`, `hostname`, and `pathname` in the console.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(location.href);
console.log(location.hostname);
console.log(location.pathname);
```

</details>


---

### 3.2 Navigation Methods

### The problem

We want to move to a new page, reload, or replace the current history entry.

### 🔮 Predict

What will happen if you uncomment `window.location.href = "https://example.com"`? Which methods keep history?

### ✅ Result

Try these one at a time. (Only run one at a time — they may move you away from this page.)

```javascript
// window.location.href = "https://example.com"; // navigate
// window.location.reload();                       // reload
// window.location.assign("https://example.com");  // navigate with history
// window.location.replace("https://example.com"); // navigate without history
```

### 🧠 Why?

- `href` and `assign` keep a history entry you can go back to.
- `replace` replaces the current entry.
- `reload` refreshes the page.

### 🧪 Experiment

Add `?test=1` to the URL manually. What does `location.search` show?

### Challenge 3.2 — Add Query Parameter

Use `URL` and `history.pushState` to add `?page=2` to the current URL without reloading.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const url = new URL(window.location.href);
url.searchParams.set("page", "2");
window.history.pushState({}, "", url);
```

</details>


---

## Part 4: History

### 4.1 History Navigation

### The problem

Move through the browser's back and forward history.

### 🔮 Predict

What will `window.history.length` show? What does `go(-2)` do?

### ✅ Result

Run this:

```javascript
console.log(window.history.length);

// window.history.back();
// window.history.forward();
// window.history.go(-2);
```

### 🧠 Why?

The `history` object is a stack of pages. You can move back, forward, or by a specific number of steps.

### 🧪 Experiment

Click a link, then try `history.back()`.

---

### 4.2 pushState and popstate

### The problem

We want to add new history entries without leaving the page.

### 🔮 Predict

What will the `popstate` listener log when you click the browser back button?

### ✅ Result

Run this and then use the back button:

```javascript
window.history.pushState({ page: 1 }, "Page 1", "?page=1");

window.addEventListener("popstate", (event) => {
  console.log("State changed:", event.state);
});
```

### 🧠 Why?

`pushState` puts a new entry on the history stack. The `popstate` event fires when the user moves backward or forward through those entries.

### 🧪 Experiment

Push a second state with `page: 2`. What happens when you go back twice?

### Challenge 4.1 — Simulate Navigation

Push a state `{ page: 2 }` with URL `?page=2` and listen for `popstate`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
window.history.pushState({ page: 2 }, "Page 2", "?page=2");

window.addEventListener("popstate", (event) => {
  console.log("State:", event.state);
});
```

</details>


---

## Part 5: Scroll

### 5.1 Scroll Methods

### The problem

Control the page scroll position.

### 🔮 Predict

Where will the page be after each line below? What does `scrollIntoView` need?

### ✅ Result

Run this on a page with some height:

```javascript
window.scrollTo({ top: 500, behavior: "smooth" });
window.scrollBy({ top: 100, behavior: "smooth" });

const section = document.getElementById("section2");
section.scrollIntoView({ behavior: "smooth" });
```

### 🧠 Why?

- `scrollTo` jumps to an exact position.
- `scrollBy` moves relative to the current position.
- `scrollIntoView` scrolls until the element is visible.

### 🧪 Experiment

Try `window.scrollTo({ top: 0, behavior: "smooth" })`. Where do you end up?

### Challenge 5.1 — Scroll to Top

When `#topBtn` is clicked, smoothly scroll to the top of the page.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("topBtn").addEventListener("click", () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
});
```

</details>


---

### 5.2 Scroll Indicator

### The problem

Show how far the user has scrolled.

### 🔮 Predict

What will `bar.style.width` be when the user is at the top of the page? What about near the bottom?

### ✅ Result

Run this on a tall page:

```javascript
const bar = document.getElementById("progressBar");

window.addEventListener("scroll", () => {
  const scrollTop = window.scrollY;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  const percent = (scrollTop / docHeight) * 100;
  bar.style.width = percent + "%";
});
```

### 🧠 Why?

`scrollY` is how far down the user has scrolled. Divide it by the total scrollable height to get a percentage.

### 🧪 Experiment

Round `percent` to the nearest whole number before setting the width.

---

## Part 6: Storage

### 6.1 localStorage

### The problem

Save data that persists after the browser is closed.

### 🔮 Predict

Will the value of `name` still be there after you close and reopen the browser?

### ✅ Result

Run this, then close and reopen the tab:

```javascript
localStorage.setItem("username", "John");
const name = localStorage.getItem("username");
console.log(name);

localStorage.removeItem("username");
// localStorage.clear();
```

### 🧠 Why?

`localStorage` stores strings in the browser. They stay until you delete them.

### 🧪 Experiment

Open the same page in a different tab. Can the new tab read the same `localStorage`?

### Challenge 6.1 — Save a Theme

Save `"dark"` as `theme` in `localStorage`, then read it and log it.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
localStorage.setItem("theme", "dark");
console.log(localStorage.getItem("theme"));
```

</details>


---

### 6.2 Storing Objects

### The problem

`localStorage` only stores strings.

### 🔮 Predict

What will `saved` be if you forget to use `JSON.stringify`?

### ✅ Result

Run this:

```javascript
const user = { name: "John", theme: "dark" };
localStorage.setItem("user", JSON.stringify(user));

const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);
```

### 🧠 Why?

Use `JSON.stringify` to turn objects into strings. Use `JSON.parse` to turn them back.

### 🧪 Experiment

Try storing `user` without `JSON.stringify`. What does `localStorage.getItem("user")` return?

### Challenge 6.2 — Save Settings

Save an object `{ language: "en", notifications: true }` to `localStorage`, then load and log `language`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const settings = { language: "en", notifications: true };
localStorage.setItem("settings", JSON.stringify(settings));

const saved = JSON.parse(localStorage.getItem("settings"));
console.log(saved.language);
```

</details>


---

### 6.3 sessionStorage

### The problem

Store data that should disappear when the tab closes.

### 🔮 Predict

What will happen to `temp` if you close this tab and reopen it?

### ✅ Result

Run this, then close the tab and open a new one:

```javascript
sessionStorage.setItem("temp", "This clears on tab close");
console.log(sessionStorage.getItem("temp"));
```

### 🧠 Why?

`sessionStorage` is like `localStorage`, but it is cleared when the tab is closed.

### 🧪 Experiment

Set `temp` in one tab. Open the same page in a second tab. Does the second tab see the value?

### Challenge 6.3 — local vs session

Explain the difference between `localStorage` and `sessionStorage`.

```text
localStorage:    _______________________________________________________________

sessionStorage:  _______________________________________________________________
```

---

## Bug Hunt 2

### The mission

Find the bugs in this storage code. Do not run it yet. Read and write what you think is wrong.

```javascript
const user = { name: "John", age: 30 };
localStorage.setItem("user", user);

const saved = localStorage.getItem("user");
console.log(saved.name);
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
const user = { name: "John", age: 30 };
localStorage.setItem("user", JSON.stringify(user));

const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);
```

</details>


---

## Part 7: Practical BOM Project — Color App

### The problem

Build a color picker that saves the chosen color.

### 🔮 Predict

What will happen to the background color when you click a colored box? Will it stay after you reload the page?

### ✅ Result

Create an HTML page with a `div id="palette"` and this script:

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

### 🧠 Why?

Each box sets the page background and saves the color to `localStorage`. When the page loads, `loadColor` reads it back.

### 🧪 Experiment

Add more colors to the array. What happens?

### Challenge 7.1 — Theme Switcher

Add a "Random Color" button that picks a random color, applies it, and saves it.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00"];

document.getElementById("randomBtn").addEventListener("click", () => {
  const color = colors[Math.floor(Math.random() * colors.length)];
  document.body.style.backgroundColor = color;
  localStorage.setItem("backgroundColor", color);
});
```

</details>


---

## Group Challenge: BOM Race

If you are working in a group, split into teams of 2 or 3. Set a timer for 10 minutes. The first team with all four working solutions wins.

### Situations

1. Create a countdown that updates `#timer` every second from 10 to 0 and stops at 0.
2. Save and load the value of `#input` using `localStorage`.
3. Add a button that scrolls `#section` into view smoothly.
4. Use `confirm` before removing an item from a list.

Do not look at the answer key until everyone has tried.

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Dialog

Show an `alert` with the current time when `#showTime` is clicked.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
document.getElementById("showTime").addEventListener("click", () => {
  alert(new Date().toLocaleTimeString());
});
```

</details>


### Level 2: Timeout

Log `"Hello after 3 seconds"` 3 seconds after the page loads.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
setTimeout(() => console.log("Hello after 3 seconds"), 3000);
```

</details>


### Level 3: Interval

Count from 1 to 10, one number per second. Stop at 10.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
let count = 1;
const intervalId = setInterval(() => {
  console.log(count);
  if (count === 10) clearInterval(intervalId);
  count++;
}, 1000);
```

</details>


### Level 4: URL

Log the current `hostname` and `pathname`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(location.hostname, location.pathname);
```

</details>


### Level 5: Storage

Save and load a username from `localStorage`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
localStorage.setItem("username", "John");
console.log(localStorage.getItem("username"));
```

</details>


### Level 6: Scroll

Create a fixed button that scrolls the page to the top when clicked.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
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

## Mini Project: Personal Notes App

### Goal

Build a notes app that uses `localStorage` to persist notes across page reloads.

### Requirements

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

### Questions to think about

1. Why do we need `JSON.stringify` and `JSON.parse`?
2. What happens if `localStorage` is empty when the page loads?
3. How can we edit a note instead of deleting it?

### Extension ideas

- Add an edit button for each note.
- Sort notes by title.
- Save notes automatically as the user types.

---

## Review Questions

Answer these before you finish.

1. What is the BOM?
   - [ ] A programming language
   - [ ] Browser Object Model for browser interaction
   - [ ] Document Object Model
   - [ ] A styling framework

2. What does `alert()` do?
   - [ ] Shows a confirmation dialog
   - [ ] Shows an alert message with OK button
   - [ ] Shows an input dialog
   - [ ] Shows a file dialog

3. What does `setTimeout()` do?
   - [ ] Executes code repeatedly
   - [ ] Executes code after a delay
   - [ ] Executes code immediately
   - [ ] Cancels a timeout

4. What does `setInterval()` do?
   - [ ] Executes code once after delay
   - [ ] Executes code repeatedly at intervals
   - [ ] Cancels an interval
   - [ ] Shows an alert

5. What is the difference between `localStorage` and `sessionStorage`?
   - [ ] No difference
   - [ ] `localStorage` persists after tab close, `sessionStorage` does not
   - [ ] `sessionStorage` persists after tab close, `localStorage` does not
   - [ ] Both clear on tab close

6. What does `window.location.href` do?
   - [ ] Returns the current URL
   - [ ] Navigates to a new URL
   - [ ] Reloads the page
   - [ ] Returns the browser history

7. What does `window.history.back()` do?
   - [ ] Goes forward in history
   - [ ] Goes back in history
   - [ ] Reloads the page
   - [ ] Clears history

8. What does `window.scrollTo()` do?
   - [ ] Scrolls element into view
   - [ ] Scrolls window to specific position
   - [ ] Scrolls by offset
   - [ ] Gets scroll position

9. What does `confirm()` return?
   - [ ] Always true
   - [ ] Always false
   - [ ] true if OK, false if Cancel
   - [ ] The entered text

10. What does `clearTimeout()` do?
    - [ ] Starts a timeout
    - [ ] Cancels a scheduled timeout
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

---

<details>
<summary>Answer Key — Try everything first!</summary>

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

### Bug Hunt 1 Fixed Version

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

### Bug Hunt 2 Fixed Version

```javascript
const user = { name: "John", age: 30 };
localStorage.setItem("user", JSON.stringify(user));

const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);
```

### Group Challenge Answer Key

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
