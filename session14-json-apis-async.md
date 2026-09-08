# Session 14: JSON, APIs & Asynchronous JavaScript — Student Active-Learning Lab

Your app needs to talk to a server. The data is not here yet — it is *on its way*. Today you will learn how to send and receive data as **JSON**, and how JavaScript keeps working while it **waits** for answers using **promises**, **`fetch`**, and **`async`/`await`**. Do not just read — predict, type, run, and fix.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Do not look at the code yet.
3. **Write your prediction** in a comment or notebook.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before you look at the answer key.**

---

## Part 0: Warm-Up — The Waiting Problem

### The problem

A program fetches data from a server. The code should keep working while the data is loading.

### 🔮 Predict

Look at this code and write the order in which the three messages will print:

```javascript
console.log("Start");
setTimeout(() => console.log("Async"), 1000);
console.log("End");
```

```text
1st: __________   2nd: __________   3rd: __________
```

### ✅ Result

Run the code.

### 🧠 Why?

`setTimeout` does **not** block. Network requests take time, and JavaScript can continue doing other things while it waits. Output: `Start`, `End`, `Async`.

### 🧪 Experiment

Change `1000` to `0`. Does `"Async"` print before `"End"` now? Why or why not?

---

## Part 1: JSON

### 1.1 What is JSON

### The problem

We need to send data as a string and convert it back to an object.

### 🔮 Predict

```javascript
const user = { name: "John", age: 30, active: true };
const json = JSON.stringify(user);
console.log(json);

const parsed = JSON.parse(json);
console.log(parsed.name);
```

What does `json` look like? What does `parsed.name` print?

### ✅ Result

Run it.

### 🧠 Why?

`JSON.stringify` turns a JavaScript value into a JSON **string** (perfect for sending over a network or saving). `JSON.parse` turns that string back into a real JavaScript object you can use.

### Challenge 1.1 — Stringify and Parse

Convert `const product = { name: "Laptop", price: 999 }` to a JSON string, then parse it back.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const product = { name: "Laptop", price: 999 };
const json = JSON.stringify(product);
const parsed = JSON.parse(json);
console.log(json);
console.log(parsed);
```

</details>

---

### 1.2 JSON Rules

### The problem

Not everything in a JavaScript object can become JSON.

### 🔮 Predict

```javascript
const object = {
  name: "John",
  greet: function() { return "Hello"; },
  address: undefined
};

console.log(JSON.stringify(object));
```

Which properties survive the conversion? Which disappear?

### ✅ Result

Run it.

### 🧠 Why?

JSON only supports data: strings, numbers, booleans, `null`, arrays, and plain objects. **Functions and `undefined` are silently dropped** — only `name` appears in the string.

### Challenge 1.2 — What is Lost?

Predict what `JSON.stringify` will produce for an object with a function and `undefined`, then test it.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const object = {
  name: "John",
  greet: function() { return "Hello"; },
  address: undefined
};
console.log(JSON.stringify(object));
// Functions and `undefined` are removed — only `name` appears in the JSON string.
```

</details>

---

## Part 2: Synchronous vs Asynchronous

### 2.1 Order of Execution

### The problem

Understand why async code does not run immediately — even with a zero delay.

### 🔮 Predict

```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
```

What order will `1`, `2`, `3` print in?

### ✅ Result

Run it.

### 🧠 Why?

Synchronous code runs first on the **call stack**. Async callbacks wait in the **queue**. The **event loop** moves them to the stack only when it is empty. That is why `2` prints last, even with `0` milliseconds.

### Challenge 2.1 — Predict Order

Predict the output, then run the code.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
// Output: 1, 3, 2
```

</details>

### 2.2 Event Loop

### 🧠 Remember

- **Call stack** — runs your synchronous code, one thing at a time.
- **Callback queue** — where finished async tasks wait.
- **Event loop** — the traffic controller that moves callbacks from the queue to the stack when the stack is empty.

---

## Part 3: Promises

### 3.1 Creating a Promise

### The problem

A function that will *eventually* succeed or fail — we don't know which yet.

### 🔮 Predict

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("Done");
  } else {
    reject("Failed");
  }
});

promise
  .then(result => console.log(result))
  .catch(error => console.log(error));
```

Which block runs — `.then` or `.catch`? What happens if you change `success` to `false`?

### ✅ Result

Run it.

### 🧠 Why?

A **Promise** is a placeholder for a future result. It is either **pending**, **fulfilled** (`resolve`), or **rejected** (`reject`). `.then` handles success; `.catch` handles failure.

### Challenge 3.1 — Promise with Timeout

Create a promise that resolves with `"Ready"` after 1 second.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const promise = new Promise(resolve => {
  setTimeout(() => resolve("Ready"), 1000);
});
promise.then(result => console.log(result));
```

</details>

**Hint:** use `setTimeout` inside the promise.

---

### 3.2 Promise Chaining

### The problem

Do several async steps in order — each one depends on the previous result.

### 🔮 Predict

```javascript
fetchUser(1)
  .then(user => {
    console.log(user.name);
    return fetchPosts(user.id);
  })
  .then(posts => console.log(posts.length))
  .catch(error => console.log(error.message));
```

How many `.then` blocks can run? What does `return fetchPosts(user.id)` do for the next `.then`?

### ✅ Result

Run it (you will need `fetchUser` and `fetchPosts` functions, or simulate them with promises).

### 🧠 Why?

Whatever you `return` from a `.then` becomes the input of the next `.then`. Returning a promise makes the chain wait for it. One `.catch` at the end catches errors from **any** step.

### Challenge 3.2 — Chain Promises

Create `step1()` that resolves `"Step 1"`, then chain `step2(result)` that returns `"Step 2: Step 1"`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function step1() {
  return Promise.resolve("Step 1");
}

function step2(result) {
  return `Step 2: ${result}`;
}

step1().then(step2).then(console.log);
```

</details>

---

## Bug Hunt 1

### The mission

Find the bug in this async code. Do not run it yet. Read and write what you think is wrong.

```javascript
function getData() {
  const data = fetch("https://jsonplaceholder.typicode.com/users/1");
  console.log(data.name);
}

getData();
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

`fetch` returns a `Promise`, not the actual data. You must `await` it (or use `.then`) and call `.json()`.

```javascript
async function getData() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const data = await response.json();
  console.log(data.name);
}

getData();
```

</details>

---

## Part 4: Fetch API

### 4.1 GET Request

### The problem

Get real data from an API — and handle the case where it fails.

### 🔮 Predict

```javascript
async function getUser(id) {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    console.log(data.name);
  } catch (error) {
    console.log("Error:", error.message);
  }
}

getUser(1);
```

Why are there **two** `await`s? What does `response.ok` check?

### ✅ Result

Run it.

### 🧠 Why?

`fetch` returns a promise for the **response**, not the data. `response.json()` is *also* async — it reads the body. `response.ok` is `false` for HTTP errors like 404 or 500 — `fetch` does **not** reject on those, so you must check yourself.

### Challenge 4.1 — Fetch Posts

Use `fetch` to get `https://jsonplaceholder.typicode.com/posts` and log the number of posts.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function getPosts() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const posts = await response.json();
  console.log(posts.length);
}

getPosts();
```

</details>

---

### 4.2 POST Request

### The problem

Send new data *to* the server, not just read from it.

### 🔮 Predict

```javascript
async function createPost(post) {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(post)
  });

  const data = await response.json();
  console.log(data);
}

createPost({ title: "Hello", body: "World", userId: 1 });
```

Why do we need `JSON.stringify(post)`? Why the `"Content-Type"` header?

### ✅ Result

Run it.

### 🧠 Why?

The `body` must be a **string** — that is where JSON comes in. The header tells the server "this body is JSON" so it can parse it correctly.

### Challenge 4.2 — POST User

Send a POST request to `https://jsonplaceholder.typicode.com/users` with a new user object.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function postUser() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "New User", email: "new@example.com" })
  });
  const data = await response.json();
  console.log(data);
}

postUser();
```

</details>

---

## Part 5: Working with Fetched Data

### 5.1 Looping and Filtering

### The problem

The API gives you an array — now shape it into what you need.

### 🔮 Predict

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();

  users.forEach(user => console.log(user.name));

  const topUsers = users.filter(user => user.id <= 5);
  console.log(topUsers.length);
}
```

How many names print? What number does the last `console.log` show?

### ✅ Result

Run it.

### 🧠 Why?

Once you `await response.json()`, `users` is a normal array — all your array methods (`forEach`, `filter`, `map`) work on it.

### Challenge 5.1 — Transform Data

Fetch users and create an array of `{ name, email }` objects.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const summaries = users.map(user => ({ name: user.name, email: user.email }));
  console.log(summaries);
}

getUsers();
```

</details>

**Hint:** `map` is your friend.

---

### 5.2 Promise Combinators

### The problem

Fetch several things at the same time instead of one after another.

### 🔮 Predict

```javascript
const p1 = fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json());
const p2 = fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json());

const [user1, user2] = await Promise.all([p1, p2]);
console.log(user1.name, user2.name);
```

Do `p1` and `p2` run one after the other, or in parallel? What does `Promise.all` give back?

### ✅ Result

Run it at the top level of a module, or wrap it in an `async` function.

### 🧠 Why?

`Promise.all` waits until **all** promises resolve and returns their results as an array. If any one rejects, the whole `Promise.all` rejects. Use it when the tasks are independent — it is much faster than awaiting them one by one.

### Challenge 5.2 — Fetch in Parallel

Use `Promise.all` to fetch users 1, 2, and 3 in parallel.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function getThree() {
  const ids = [1, 2, 3];
  const promises = ids.map(id =>
    fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => r.json())
  );
  const users = await Promise.all(promises);
  users.forEach(user => console.log(user.name));
}

getThree();
```

</details>

---

## Bug Hunt 2

### The mission

Find the bug in this fetch code. Do not run it yet.

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = response.json();
  console.log(users.length);
}

getUsers();
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

`response.json()` returns a promise — it must be awaited too.

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  console.log(users.length);
}

getUsers();
```

</details>

---

## Part 6: Async/Await

### 6.1 Making Async Code Readable

### The problem

Nested `.then` chains get hard to read when steps depend on each other.

### 🔮 Predict

```javascript
async function loadData() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    const user = await response.json();
    const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${user.id}`);
    const posts = await postsResponse.json();
    console.log(user.name, posts.length);
  } catch (error) {
    console.log("Error:", error.message);
  }
}
```

Why must the second `fetch` wait for `user.id`? Could you use `Promise.all` here?

### ✅ Result

Run it.

### 🧠 Why?

`await` pauses the `async` function until the promise resolves — the code *reads* top to bottom like synchronous code, but nothing outside the function is blocked. The second fetch **needs** `user.id`, so it must be sequential.

### Challenge 6.1 — User and Posts

Fetch user 1, then fetch their posts using `user.id`. Log the user's name and number of posts.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function load() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await response.json();
  const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${user.id}`);
  const posts = await postsResponse.json();
  console.log(user.name, posts.length);
}

load();
```

</details>

---

### 6.2 try/catch/finally

### The problem

Handle errors gracefully — and run cleanup code no matter what happens.

### 🔮 Predict

```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    return await response.json();
  } catch (error) {
    console.log("Failed:", error.message);
    return null;
  } finally {
    console.log("Request completed");
  }
}
```

If the fetch succeeds, does `"Request completed"` still print? What if it fails?

### ✅ Result

Run it with both a valid and an invalid URL.

### 🧠 Why?

`try/catch` handles errors from any `await` inside it. `finally` runs **always** — success or failure — which makes it perfect for hiding loading spinners or logging.

### Challenge 6.2 — Error Handling

Fetch from an invalid URL and log a friendly error message in `catch`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function fetchInvalid() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
    if (!response.ok) throw new Error("Not found");
  } catch (error) {
    console.log("Could not load data:", error.message);
  }
}

fetchInvalid();
```

</details>

---

## Group Challenge: API Race

If you are working in a group, split into teams of 2 or 3. Each team writes one task. Set a timer for 12 minutes.

### Tasks

1. Fetch `https://jsonplaceholder.typicode.com/users` and display the first 5 names.
2. Create a new post with POST and log the returned `id`.
3. Fetch user 1 and their posts in parallel using `Promise.all`.
4. Use `try/catch` to fetch from `https://jsonplaceholder.typicode.com/invalid` and log the error.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
// 1
async function getNames() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  users.slice(0, 5).forEach(user => console.log(user.name));
}

// 2
async function create() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Test", body: "Body", userId: 1 })
  });
  const data = await response.json();
  console.log(data.id);
}

// 3
async function userAndPosts() {
  const [user, posts] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json())
  ]);
  console.log(user.name, posts.length);
}

// 4
async function fetchInvalid() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
  } catch (error) {
    console.log(error.message);
  }
}
```

</details>

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: JSON

Convert `{ name: "John" }` to a JSON string and parse it back.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const json = JSON.stringify({ name: "John" });
const parsed = JSON.parse(json);
console.log(json);
console.log(parsed);
```

</details>

### Level 2: Promise

Create a promise that resolves after 1 second with `"Done"`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const p = new Promise(resolve => setTimeout(() => resolve("Done"), 1000));
p.then(console.log);
```

</details>

### Level 3: Fetch GET

Fetch a single user and log their `name` and `email`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function getUser() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await response.json();
  console.log(user.name, user.email);
}

getUser();
```

</details>

### Level 4: Fetch POST

POST a new comment to `/comments` and log the response.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function postComment() {
  const response = await fetch("https://jsonplaceholder.typicode.com/comments", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ postId: 1, name: "Test", email: "test@test.com", body: "Hello" })
  });
  const data = await response.json();
  console.log(data);
}

postComment();
```

</details>

### Level 5: Parallel

Use `Promise.all` to fetch 3 users at the same time and log their names.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function threeUsers() {
  const promises = [1, 2, 3].map(id =>
    fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => r.json())
  );
  const users = await Promise.all(promises);
  users.forEach(user => console.log(user.name));
}

threeUsers();
```

</details>

### Level 6: Error Handling

Fetch from an invalid endpoint and handle the error with `try/catch`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function handleError() {
  try {
    await fetch("https://jsonplaceholder.typicode.com/invalid");
  } catch (error) {
    console.log("Error:", error.message);
  }
}

handleError();
```

</details>

---

## Mini Project: User Dashboard with Fetch

### Goal

Build a page that fetches and displays user data from an API.

### Requirements

1. Create an HTML page with:
   - A button "Load Users"
   - A `div` to display users
   - A dropdown to select a user
   - A button "Load Posts" to show the selected user's posts

2. On clicking "Load Users", fetch `https://jsonplaceholder.typicode.com/users` and display each user's name and email.

3. On clicking "Load Posts", fetch `https://jsonplaceholder.typicode.com/posts?userId=` + user id.

4. Show the posts in a list.

5. Handle loading and error states.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>User Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .card { background: #f9f9f9; padding: 10px; margin: 10px 0; border-radius: 4px; }
    .output { margin-top: 20px; }
    .error { color: red; }
    .loading { color: blue; }
  </style>
</head>
<body>
  <h1>User Dashboard</h1>
  <button id="loadUsers">Load Users</button>
  <select id="userSelect" disabled></select>
  <button id="loadPosts" disabled>Load Posts</button>
  <div id="users" class="output"></div>
  <div id="posts" class="output"></div>

  <script>
    const API = "https://jsonplaceholder.typicode.com";

    async function loadUsers() {
      const usersDiv = document.getElementById("users");
      const select = document.getElementById("userSelect");
      usersDiv.innerHTML = "<p class='loading'>Loading...</p>";

      try {
        const response = await fetch(`${API}/users`);
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const users = await response.json();

        usersDiv.innerHTML = "";
        select.innerHTML = "";

        users.forEach(user => {
          const div = document.createElement("div");
          div.className = "card";
          div.textContent = `${user.name} — ${user.email}`;
          usersDiv.appendChild(div);

          const option = document.createElement("option");
          option.value = user.id;
          option.textContent = user.name;
          select.appendChild(option);
        });

        select.disabled = false;
        document.getElementById("loadPosts").disabled = false;
      } catch (error) {
        usersDiv.innerHTML = `<p class="error">Error: ${error.message}</p>`;
      }
    }

    async function loadPosts() {
      const userId = document.getElementById("userSelect").value;
      const postsDiv = document.getElementById("posts");
      postsDiv.innerHTML = "<p class='loading'>Loading posts...</p>";

      try {
        const response = await fetch(`${API}/posts?userId=${userId}`);
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const posts = await response.json();

        postsDiv.innerHTML = "";
        const ul = document.createElement("ul");
        posts.slice(0, 10).forEach(post => {
          const li = document.createElement("li");
          li.textContent = post.title;
          ul.appendChild(li);
        });
        postsDiv.appendChild(ul);
      } catch (error) {
        postsDiv.innerHTML = `<p class="error">Error: ${error.message}</p>`;
      }
    }

    document.getElementById("loadUsers").addEventListener("click", loadUsers);
    document.getElementById("loadPosts").addEventListener("click", loadPosts);
  </script>
</body>
</html>
```

### Questions to think about

1. Why do we use `await` on `response.json()`?
2. What does `!response.ok` check?
3. How can we show a loading state?

### Extension ideas

- Show each post's `body` under its title.
- Add a search box that filters the displayed users by name.
- Disable the buttons while a request is in flight.

---

## Review Questions

Answer these before you finish.

1. What is JSON?
   - [ ] A programming language
   - [ ] A data interchange format
   - [ ] A database
   - [ ] A framework

2. What does JSON.parse() do?
   - [ ] Converts object to string
   - [ ] Converts string to object
   - [ ] Validates JSON
   - [ ] Formats JSON

3. What is an API?
   - [ ] A programming interface for software communication
   - [ ] A database
   - [ ] A programming language
   - [ ] A UI framework

4. What is the difference between sync and async?
   - [ ] No difference
   - [ ] Sync blocks, async doesn't
   - [ ] Async blocks, sync doesn't
   - [ ] Both block

5. What is the event loop?
   - [ ] A looping construct
   - [ ] Mechanism that coordinates call stack and callback queue
   - [ ] A database query
   - [ ] A UI component

6. What is callback hell?
   - [ ] A place where callbacks go
   - [ ] Deeply nested callbacks that are hard to read
   - [ ] A callback that errors
   - [ ] A callback that never executes

7. What does Promise.all() do?
   - [ ] Resolves when all promises resolve
   - [ ] Resolves when first promise resolves
   - [ ] Resolves when all promises settle
   - [ ] Rejects immediately

8. What does async/await do?
   - [ ] Makes code synchronous
   - [ ] Allows writing async code that looks synchronous
   - [ ] Makes code faster
   - [ ] Prevents errors

9. What does try/catch do?
   - [ ] Tries to catch errors
   - [ ] Handles errors gracefully
   - [ ] Creates errors
   - [ ] Ignores errors

10. What does finally do?
    - [ ] Runs only on success
    - [ ] Runs only on error
    - [ ] Runs regardless of success or error
    - [ ] Never runs

---

## Additional Resources

- [MDN: JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [MDN: Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN: Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) - Free fake API for testing
- [JavaScript.info: Promises](https://javascript.info/promise-basics)
- [JavaScript.info: async/await](https://javascript.info/async)

---

<details>
<summary>Answer Key — Try everything first!</summary>

### Challenge 1.1

```javascript
const json = JSON.stringify(product);
const parsed = JSON.parse(json);
```

### Challenge 1.2

Functions and `undefined` are removed. Only `name` appears in the JSON string.

### Challenge 2.1

Output: `1`, `3`, `2`.

### Challenge 3.1

```javascript
const promise = new Promise(resolve => {
  setTimeout(() => resolve("Ready"), 1000);
});
```

### Challenge 3.2

```javascript
function step1() {
  return Promise.resolve("Step 1");
}

function step2(result) {
  return `Step 2: ${result}`;
}

step1().then(step2).then(console.log);
```

### Challenge 4.1

```javascript
async function getPosts() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const posts = await response.json();
  console.log(posts.length);
}
```

### Challenge 4.2

```javascript
async function postUser() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "New User", email: "new@example.com" })
  });
  const data = await response.json();
  console.log(data);
}
```

### Challenge 5.1

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const summaries = users.map(user => ({ name: user.name, email: user.email }));
  console.log(summaries);
}
```

### Challenge 5.2

```javascript
async function getThree() {
  const ids = [1, 2, 3];
  const promises = ids.map(id =>
    fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => r.json())
  );
  const users = await Promise.all(promises);
  users.forEach(user => console.log(user.name));
}
```

### Challenge 6.1

```javascript
async function load() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await response.json();
  const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${user.id}`);
  const posts = await postsResponse.json();
  console.log(user.name, posts.length);
}
```

### Challenge 6.2

```javascript
async function fetchInvalid() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
    if (!response.ok) throw new Error("Not found");
  } catch (error) {
    console.log("Could not load data:", error.message);
  }
}
```

### Bug Hunt 1 Fixed Version

`fetch` returns a `Promise`, not the actual data. You must `await` it (or use `.then`) and call `.json()`.

```javascript
async function getData() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const data = await response.json();
  console.log(data.name);
}

getData();
```

### Bug Hunt 2 Fixed Version

`response.json()` returns a promise — it must be awaited too.

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  console.log(users.length);
}
```

### Group Challenge Answer Key

```javascript
// 1
async function getNames() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  users.slice(0, 5).forEach(user => console.log(user.name));
}

// 2
async function create() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Test", body: "Body", userId: 1 })
  });
  const data = await response.json();
  console.log(data.id);
}

// 3
async function userAndPosts() {
  const [user, posts] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json())
  ]);
  console.log(user.name, posts.length);
}

// 4
async function fetchInvalid() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
  } catch (error) {
    console.log(error.message);
  }
}
```

### Individual Challenges Solutions

```javascript
// Level 1
const json = JSON.stringify({ name: "John" });
const parsed = JSON.parse(json);

// Level 2
const p = new Promise(resolve => setTimeout(() => resolve("Done"), 1000));
p.then(console.log);

// Level 3
async function getUser() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await response.json();
  console.log(user.name, user.email);
}

// Level 4
async function postComment() {
  const response = await fetch("https://jsonplaceholder.typicode.com/comments", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ postId: 1, name: "Test", email: "test@test.com", body: "Hello" })
  });
  const data = await response.json();
  console.log(data);
}

// Level 5
async function threeUsers() {
  const promises = [1, 2, 3].map(id =>
    fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => r.json())
  );
  const users = await Promise.all(promises);
  users.forEach(user => console.log(user.name));
}

// Level 6
async function handleError() {
  try {
    await fetch("https://jsonplaceholder.typicode.com/invalid");
  } catch (error) {
    console.log("Error:", error.message);
  }
}
```

</details>
