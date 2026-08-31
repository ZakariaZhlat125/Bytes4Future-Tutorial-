# Session 14: JSON, APIs & Asynchronous JavaScript — Active Learning Redesign

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

- "What does `JSON.stringify` leave out?"
- "Why do we need `await`?"
- "What does `response.ok` check?"
- "When should we use `Promise.all`?"
- "What is the event loop doing here?"

---

## Part 0: Warm-Up — The Waiting Problem (5 minutes)

### Problem

A program fetches data from a server. The code should keep working while the data is loading.

### Guess

Ask: "What happens if we run `fetch` and then immediately try to use the result?"

### Explain

Network requests take time. JavaScript can continue doing other things while waiting.

### Live Code

```javascript
console.log("Start");
setTimeout(() => console.log("Async"), 1000);
console.log("End");
```

### Review

`setTimeout` does not block. Output: Start, End, Async.

---

## Part 1: JSON

### 1.1 What is JSON

#### Problem

We need to send data as a string and convert it back to an object.

#### Live Code

```javascript
const user = { name: "John", age: 30, active: true };
const json = JSON.stringify(user);
console.log(json);

const parsed = JSON.parse(json);
console.log(parsed.name);
```

#### Challenge 1.1 — Stringify and Parse (individual, 3 minutes)

- **Requirement:** Convert `const product = { name: "Laptop", price: 999 }` to a JSON string, then parse it back.
- **Time limit:** 3 minutes

### 1.2 JSON Rules

#### Live Code

```javascript
const object = {
  name: "John",
  greet: function() { return "Hello"; },
  address: undefined
};

console.log(JSON.stringify(object)); // Only name
```

#### Challenge 1.2 — What is Lost? (individual, 3 minutes)

- **Requirement:** Predict what `JSON.stringify` will produce for an object with a function and `undefined`, then test it.
- **Time limit:** 3 minutes

---

## Part 2: Synchronous vs Asynchronous

### 2.1 Order of Execution

#### Problem

Understand why async code does not run immediately.

#### Live Code

```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
```

#### Challenge 2.1 — Predict Order (individual, 2 minutes)

- **Requirement:** Predict the output, then run the code.
- **Time limit:** 2 minutes

### 2.2 Event Loop

#### Explain

Synchronous code runs first on the call stack. Async callbacks wait in the queue. The event loop moves them to the stack when it is empty.

---

## Part 3: Promises

### 3.1 Creating a Promise

#### Problem

A function that will eventually succeed or fail.

#### Live Code

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

#### Challenge 3.1 — Promise with Timeout (individual, 3 minutes)

- **Requirement:** Create a promise that resolves with `"Ready"` after 1 second.
- **Time limit:** 3 minutes

### 3.2 Promise Chaining

#### Live Code

```javascript
fetchUser(1)
  .then(user => {
    console.log(user.name);
    return fetchPosts(user.id);
  })
  .then(posts => console.log(posts.length))
  .catch(error => console.log(error.message));
```

#### Challenge 3.2 — Chain Promises (individual, 4 minutes)

- **Requirement:** Create `step1()` that resolves `"Step 1"`, then chain `step2(result)` that returns `"Step 2: Step 1"`.
- **Time limit:** 4 minutes

---

## Bug Hunt 1

### Problem

Find the bug in this async code.

```javascript
function getData() {
  const data = fetch("https://jsonplaceholder.typicode.com/users/1");
  console.log(data.name);
}

getData();
```

### Issues

1. `fetch` returns a `Promise`, not the actual data. Must use `.then` or `await` and call `.json()`.

### Fixed Version

```javascript
async function getData() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const data = await response.json();
  console.log(data.name);
}

getData();
```

### Points

1 point for finding the bug.

---

## Part 4: Fetch API

### 4.1 GET Request

#### Problem

Get data from an API.

#### Live Code

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

#### Challenge 4.1 — Fetch Posts (individual, 4 minutes)

- **Requirement:** Use `fetch` to get `https://jsonplaceholder.typicode.com/posts` and log the number of posts.
- **Time limit:** 4 minutes

### 4.2 POST Request

#### Live Code

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

#### Challenge 4.2 — POST User (individual, 4 minutes)

- **Requirement:** Send a POST request to `https://jsonplaceholder.typicode.com/users` with a new user object.
- **Time limit:** 4 minutes

---

## Part 5: Working with Fetched Data

### 5.1 Looping and Filtering

#### Live Code

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();

  users.forEach(user => console.log(user.name));

  const topUsers = users.filter(user => user.id <= 5);
  console.log(topUsers.length);
}
```

#### Challenge 5.1 — Transform Data (individual, 4 minutes)

- **Requirement:** Fetch users and create an array of `{ name, email }` objects.
- **Time limit:** 4 minutes

### 5.2 Promise Combinators

#### Live Code

```javascript
const p1 = fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json());
const p2 = fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json());

const [user1, user2] = await Promise.all([p1, p2]);
console.log(user1.name, user2.name);
```

#### Challenge 5.2 — Fetch in Parallel (individual, 4 minutes)

- **Requirement:** Use `Promise.all` to fetch users 1, 2, and 3 in parallel.
- **Time limit:** 4 minutes

---

## Bug Hunt 2

### Problem

Find the bug in this fetch code.

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = response.json();
  console.log(users.length);
}

getUsers();
```

### Issues

1. `response.json()` returns a promise. It must be awaited.

### Fixed Version

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  console.log(users.length);
}
```

### Points

1 point for finding the bug.

---

## Part 6: Async/Await

### 6.1 Making Async Code Readable

#### Problem

Nested `.then` is hard to read.

#### Live Code

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

#### Challenge 6.1 — User and Posts (individual, 5 minutes)

- **Requirement:** Fetch user 1, then fetch their posts using `user.id`. Log the user's name and number of posts.
- **Time limit:** 5 minutes

### 6.2 try/catch/finally

#### Live Code

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

#### Challenge 6.2 — Error Handling (individual, 4 minutes)

- **Requirement:** Fetch from an invalid URL and log a friendly error message in `catch`.
- **Time limit:** 4 minutes

---

## Group Challenge: API Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team writes one task.
- **Scoring:** 2 points per correct solution.

### Tasks

1. Fetch `https://jsonplaceholder.typicode.com/users` and display the first 5 names.
2. Create a new post with POST and log the returned `id`.
3. Fetch user 1 and their posts in parallel using `Promise.all`.
4. Use `try/catch` to fetch from `https://jsonplaceholder.typicode.com/invalid` and log the error.

### Instructor Answer Key

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

---

## Individual Challenges — Progressive Difficulty

### Level 1: JSON (3 minutes)

- **Requirement:** Convert `{ name: "John" }` to a JSON string and parse it back.

### Level 2: Promise (3 minutes)

- **Requirement:** Create a promise that resolves after 1 second with `"Done"`.

### Level 3: Fetch GET (4 minutes)

- **Requirement:** Fetch a single user and log their `name` and `email`.

### Level 4: Fetch POST (4 minutes)

- **Requirement:** POST a new comment to `/comments` and log the response.

### Level 5: Parallel (5 minutes)

- **Requirement:** Use `Promise.all` to fetch 3 users at the same time and log their names.

### Level 6: Error Handling (4 minutes)

- **Requirement:** Fetch from an invalid endpoint and handle the error with `try/catch`.

---

## Mini Project: User Dashboard with Fetch

### Time

25 minutes

### Goal

Build a page that fetches and displays user data from an API.

### Requirements for the Students

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

### Review Questions for the Mini Project

- "Why do we use `await` on `response.json()`?"
- "What does `!response.ok` check?"
- "How can we show a loading state?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

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

---

## Review Questions

1. What is JSON?
   - [ ] A programming language
   - [x] A data interchange format
   - [ ] A database
   - [ ] A framework

2. What does JSON.parse() do?
   - [ ] Converts object to string
   - [x] Converts string to object
   - [ ] Validates JSON
   - [ ] Formats JSON

3. What is an API?
   - [x] A programming interface for software communication
   - [ ] A database
   - [ ] A programming language
   - [ ] A UI framework

4. What is the difference between sync and async?
   - [ ] No difference
   - [x] Sync blocks, async doesn't
   - [ ] Async blocks, sync doesn't
   - [ ] Both block

5. What is the event loop?
   - [ ] A looping construct
   - [x] Mechanism that coordinates call stack and callback queue
   - [ ] A database query
   - [ ] A UI component

6. What is callback hell?
   - [ ] A place where callbacks go
   - [x] Deeply nested callbacks that are hard to read
   - [ ] A callback that errors
   - [ ] A callback that never executes

7. What does Promise.all() do?
   - [x] Resolves when all promises resolve
   - [ ] Resolves when first promise resolves
   - [ ] Resolves when all promises settle
   - [ ] Rejects immediately

8. What does async/await do?
   - [ ] Makes code synchronous
   - [x] Allows writing async code that looks synchronous
   - [ ] Makes code faster
   - [ ] Prevents errors

9. What does try/catch do?
   - [ ] Tries to catch errors
   - [x] Handles errors gracefully
   - [ ] Creates errors
   - [ ] Ignores errors

10. What does finally do?
    - [ ] Runs only on success
    - [ ] Runs only on error
    - [x] Runs regardless of success or error
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
