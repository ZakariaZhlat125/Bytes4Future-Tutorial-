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

> **Big idea:** `fetch()` is the browser's built-in way to make HTTP requests. It always returns a **Promise** that resolves to a **`Response` object** — *not* the data. The data lives inside the body and you must read it with `.json()`, `.text()`, or `.blob()`.

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

### 📦 The Response object — what's inside?

A `Response` carries useful metadata you can read **before** touching the body:

| Property        | Meaning                                              |
| --------------- | ---------------------------------------------------- |
| `response.ok`   | `true` if status is 200–299                           |
| `response.status` | The HTTP status code (200, 404, 500, …)             |
| `response.headers` | The response headers (a `Headers` object)        |
| `response.url`  | The final URL after redirects                        |
| `response.type` | `"basic"`, `"cors"`, `"opaque"`                       |
| `response.json()` | Reads body as JSON (returns a Promise)             |
| `response.text()` | Reads body as plain text (returns a Promise)       |
| `response.blob()` | Reads body as binary (image, file)                 |

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
console.log(response.status);   // 200
console.log(response.ok);       // true
console.log(response.headers.get("content-type")); // "application/json; charset=utf-8"
```

### 🧪 Experiment

Call `getUser(999)` (a user that does not exist). What status do you get? Does the `catch` run, or the `if (!response.ok)` throw? Why?

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
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  const posts = await response.json();
  console.log(posts.length);
}

getPosts();
```

</details>

### Challenge 4.1b — Inspect the Response

Fetch a single post and log its `status`, `ok`, and the `content-type` header — *without* calling `.json()`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function inspect() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  console.log("status:", response.status);
  console.log("ok:", response.ok);
  console.log("content-type:", response.headers.get("content-type"));
}

inspect();
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

### 📝 The fetch options object

`fetch(url, options)` accepts a second argument with many fields:

| Option       | Purpose                                            |
| ------------ | -------------------------------------------------- |
| `method`     | `"GET"`, `"POST"`, `"PUT"`, `"PATCH"`, `"DELETE"`  |
| `headers`    | An object of request headers                       |
| `body`       | The request body (must be a string for JSON)        |
| `mode`       | `"cors"`, `"no-cors"`, `"same-origin"`             |
| `credentials`| `"omit"`, `"same-origin"`, `"include"` (cookies)   |
| `cache`      | `"default"`, `"no-cache"`, `"reload"`              |
| `redirect`   | `"follow"`, `"error"`, `"manual"`                  |
| `signal`     | An `AbortSignal` to cancel the request (see 6.3)  |

### 🧪 Experiment

Remove the `headers` line and run the POST again. Does the request still succeed? Does the server still understand the body? (Try logging `response.status`.)

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

### 4.3 Other HTTP Methods (PUT, PATCH, DELETE)

### The problem

A real API is not just GET and POST — you also **update** and **remove** data.

### 🔮 Predict

```javascript
// PUT — replace the whole resource
async function replacePost(id, post) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(post)
  });
  return response.json();
}

// PATCH — update only some fields
async function patchPost(id, changes) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(changes)
  });
  return response.json();
}

// DELETE — remove the resource
async function deletePost(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "DELETE"
  });
  console.log("Deleted?", response.ok, "status:", response.status);
}
```

What is the difference between `PUT` and `PATCH`? Does `DELETE` need a `body`?

### ✅ Result

Run each one and inspect the response.

### 🧠 Why?

- **PUT** replaces the *entire* resource — you send the full new version.
- **PATCH** changes only the fields you send — the rest stay the same.
- **DELETE** usually has no body; success is indicated by `response.ok` or status `200`/`204`.

### Challenge 4.3 — Update and Delete

Patch post `1` to change its `title` to `"Updated"`, then delete post `2`. Log both responses.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function updateAndDelete() {
  const patched = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Updated" })
  }).then(r => r.json());
  console.log("Patched:", patched);

  const deleteRes = await fetch("https://jsonplaceholder.typicode.com/posts/2", {
    method: "DELETE"
  });
  console.log("Delete status:", deleteRes.status);
}

updateAndDelete();
```

</details>

---

### 4.4 Query Parameters and Headers

### The problem

APIs often filter data through **query strings** (`?userId=1`) and require **auth headers** (`Authorization`).

### 🔮 Predict

```javascript
// Build a URL with query params
const params = new URLSearchParams({ userId: 1, _limit: 3 });
const url = `https://jsonplaceholder.typicode.com/posts?${params}`;
console.log(url);
// -> https://jsonplaceholder.typicode.com/posts?userId=1&_limit=3

// Send an auth header
const response = await fetch(url, {
  headers: {
    "Authorization": "Bearer my-token-123",
    "Accept": "application/json"
  }
});
const posts = await response.json();
console.log(posts.length);
```

What does `URLSearchParams` do for you? Why is it safer than string concatenation?

### ✅ Result

Run it.

### 🧠 Why?

`URLSearchParams` correctly **encodes** special characters (`&`, `=`, spaces, Arabic letters) so your URL never breaks. Building query strings by hand with `+` is a common source of bugs. Headers like `Authorization` and `Accept` tell the server who you are and what shape of response you want.

### 🧪 Experiment

Try `new URLSearchParams({ q: "hello world & more" }).toString()` — see how the space and `&` are encoded.

### Challenge 4.4 — Filter and Auth

Fetch the first 5 posts of `userId=2` using `URLSearchParams`, sending a fake `Authorization` header.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function filtered() {
  const params = new URLSearchParams({ userId: 2, _limit: 5 });
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts?${params}`, {
    headers: { "Authorization": "Bearer fake-token" }
  });
  const posts = await response.json();
  console.log(posts);
}

filtered();
```

</details>

---

## Part 5: Working with Fetched Data

> **Big idea:** Once you `await response.json()`, you have a **plain JavaScript value** — usually an array or object. Everything you already know (`map`, `filter`, `reduce`, destructuring) works on it. The async part is *over*; the data part is just JavaScript.

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

### 🛠️ Common data-shaping patterns

```javascript
// 1. Pick only the fields you need (map)
const summaries = users.map(u => ({ id: u.id, name: u.name }));

// 2. Filter by a condition
const withLongNames = users.filter(u => u.name.length > 10);

// 3. Find a single item
const leanne = users.find(u => u.name === "Leanne Graham");

// 4. Sort (returns a new array; use spread to avoid mutating)
const byName = [...users].sort((a, b) => a.name.localeCompare(b.name));

// 5. Group by a key (reduce)
const byCity = users.reduce((groups, u) => {
  const city = u.address.city;
  (groups[city] ??= []).push(u);
  return groups;
}, {});

// 6. Aggregate a number
const totalZipCodes = users.reduce((sum, u) => sum + Number(u.address.zipcode.length), 0);
```

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

### Challenge 5.1b — Group by City

Fetch all users and build an object that groups them by `address.city`. Log the number of users in each city.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function groupByCity() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const byCity = users.reduce((groups, u) => {
    const city = u.address.city;
    (groups[city] ??= []).push(u);
    return groups;
  }, {});
  for (const city in byCity) {
    console.log(city, ":", byCity[city].length);
  }
}

groupByCity();
```

</details>

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

### 📚 The four combinators compared

| Combinator         | Resolves when…                          | Rejects when…                          |
| ------------------ | --------------------------------------- | -------------------------------------- |
| `Promise.all`      | **all** promises resolve                | any promise rejects (fast-fail)        |
| `Promise.allSettled`| **all** promises settle (resolve/reject) | never — always resolves                 |
| `Promise.race`     | the **first** promise settles           | the first promise rejects               |
| `Promise.any`      | the **first** promise **resolves**      | all promises reject                     |

```javascript
// allSettled — never throws, gives {status, value/reason}
const results = await Promise.allSettled([
  fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
  fetch("https://jsonplaceholder.typicode.com/invalid-endpoint").then(r => r.json())
]);
results.forEach(r => {
  if (r.status === "fulfilled") console.log("ok:", r.value.name);
  else console.log("failed:", r.reason.message);
});

// race — first one wins (useful for timeouts)
const fastest = await Promise.race([
  fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
  new Promise((_, reject) => setTimeout(() => reject(new Error("timeout")), 100))
]);

// any — first success wins, ignores failures
const firstOk = await Promise.any([
  fetch("https://jsonplaceholder.typicode.com/invalid").then(r => r.json()),
  fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json())
]);
```

### 🧪 Experiment

Replace `Promise.all` with `Promise.allSettled` in the predict example and make one of the URLs invalid. Does the whole thing still resolve? What shape does each result have?

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

### Challenge 5.2b — Tolerant Fetch

Fetch users 1, 2, and 999 (invalid) using `Promise.allSettled`. Log the names that succeeded and the reasons that failed.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function tolerant() {
  const ids = [1, 2, 999];
  const results = await Promise.allSettled(
    ids.map(id =>
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
    )
  );
  results.forEach((r, i) => {
    if (r.status === "fulfilled") console.log(`user ${ids[i]}:`, r.value.name);
    else console.log(`user ${ids[i]} failed:`, r.reason.message);
  });
}

tolerant();
```

</details>

### Challenge 5.2c — Timeout with race

Fetch user 1, but cancel with an error if it takes longer than 50 ms (use `Promise.race` with a `setTimeout` reject).

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function withTimeout() {
  try {
    const user = await Promise.race([
      fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
      new Promise((_, reject) => setTimeout(() => reject(new Error("timeout")), 50))
    ]);
    console.log(user.name);
  } catch (error) {
    console.log("Failed:", error.message);
  }
}

withTimeout();
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

> **Big idea:** `async`/`await` is **syntactic sugar** over promises. An `async` function *always* returns a promise. `await` pauses that function until the promise settles — but it does **not** block the rest of your program. The result reads like synchronous code while staying non-blocking.

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

### Challenge 6.2b — Loading Spinner with finally

Simulate a UI: set `isLoading = true` before a fetch, and **always** set it back to `false` in `finally`, even if the request fails.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
let isLoading = false;

async function loadUser() {
  isLoading = true;
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    const user = await response.json();
    console.log("Loaded:", user.name);
  } catch (error) {
    console.log("Error:", error.message);
  } finally {
    isLoading = false;
    console.log("isLoading is now", isLoading);
  }
}

loadUser();
```

</details>

---

### 6.3 Sequential vs Parallel await

### The problem

Two independent requests, written the obvious way, run **one after the other** — wasting time.

### 🔮 Predict

```javascript
// Sequential (slow) — second request waits for the first
async function slow() {
  const a = await fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json());
  const b = await fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json());
  console.log(a.name, b.name);
}

// Parallel (fast) — both requests start at the same time
async function fast() {
  const [a, b] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json())
  ]);
  console.log(a.name, b.name);
}
```

Which one finishes first? Why? When is sequential the *right* choice?

### ✅ Result

Run both and compare.

### 🧠 Why?

`await` one after the other means the second `fetch` doesn't even **start** until the first finishes. Starting them together (without awaiting) and then `await Promise.all` lets them run in parallel. Use **sequential** only when the second call **needs** data from the first.

### Challenge 6.3 — Make It Parallel

Rewrite this so the two fetches run in parallel.

```javascript
async function slow() {
  const posts = await fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json());
  const albums = await fetch("https://jsonplaceholder.typicode.com/albums?userId=1").then(r => r.json());
  console.log(posts.length, albums.length);
}
```

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function fast() {
  const [posts, albums] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/albums?userId=1").then(r => r.json())
  ]);
  console.log(posts.length, albums.length);
}

fast();
```

</details>

---

### 6.4 Aborting Requests with AbortController

### The problem

A user clicks "Load", then clicks again, or navigates away. The first request is still flying — you want to **cancel** it.

### 🔮 Predict

```javascript
const controller = new AbortController();

async function load() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users", {
      signal: controller.signal
    });
    const users = await response.json();
    console.log(users.length);
  } catch (error) {
    if (error.name === "AbortError") {
      console.log("Request was cancelled");
    } else {
      console.log("Other error:", error.message);
    }
  }
}

load();
// Cancel it almost immediately:
setTimeout(() => controller.abort(), 10);
```

What happens to the `fetch` when `abort()` is called? How can you tell a cancellation apart from a network error?

### ✅ Result

Run it.

### 🧠 Why?

`AbortController` lets you pass a `signal` to `fetch`. Calling `controller.abort()` rejects the fetch promise with an `AbortError`. Always check `error.name === "AbortError"` so you don't show a scary error message for a *deliberate* cancellation.

### 🧪 Experiment

Increase the `setTimeout` delay to 2000 ms. Does the request complete normally now?

### Challenge 6.4 — Cancel on Re-click

Create a button that fetches users. If the button is clicked again while a request is in flight, **abort** the previous one before starting a new one.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
let currentController = null;

async function loadUsers() {
  currentController?.abort();                 // cancel any in-flight request
  currentController = new AbortController();

  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users", {
      signal: currentController.signal
    });
    const users = await response.json();
    console.log("got", users.length, "users");
  } catch (error) {
    if (error.name !== "AbortError") console.log("Error:", error.message);
  } finally {
    currentController = null;
  }
}

// Simulate two quick clicks:
loadUsers();
setTimeout(loadUsers, 10);
```

</details>

---

### 6.5 Retrying Failed Requests

### The problem

Networks fail. A good app tries again a few times before giving up.

### 🔮 Predict

```javascript
async function fetchWithRetry(url, retries = 3) {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      if (attempt === retries) throw error;
      console.log(`Attempt ${attempt} failed, retrying...`);
      await new Promise(r => setTimeout(r, 200 * attempt)); // backoff
    }
  }
}

fetchWithRetry("https://jsonplaceholder.typicode.com/users/1")
  .then(data => console.log("Got:", data.name));
```

How many times will it try? What is the "backoff" for?

### ✅ Result

Run it, then try it with an invalid URL to watch the retries.

### 🧠 Why?

A `for` loop with `try/catch` lets you retry on failure. The `setTimeout` delay grows with each attempt (exponential-ish **backoff**) so you don't hammer a struggling server. Only re-throw on the **last** attempt so the caller still sees the error.

### Challenge 6.5 — Retry with a Limit

Wrap `fetch` so it retries up to **2** times, then logs `"giving up"` if all attempts fail.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function retry(url, max = 2) {
  for (let i = 1; i <= max; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      console.log(`try ${i} failed: ${error.message}`);
      if (i === max) console.log("giving up");
    }
  }
}

retry("https://jsonplaceholder.typicode.com/invalid");
```

</details>

---

### 6.6 Top-Level await

### The problem

In an ES **module** (`.js` with `type: "module"`), you can `await` at the **top level** — no wrapper `async` function needed.

### 🔮 Predict

```javascript
// top-level await, only in ES modules
const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
const user = await response.json();
console.log(user.name);
```

Where does this work? Where does it throw a `SyntaxError`?

### ✅ Result

Save it as a `.mjs` file (or set `"type": "module"` in `package.json`) and run with `node`.

### 🧠 Why?

Top-level `await` is allowed in ES modules but **not** in CommonJS (`.js` without `type: "module"`). In older scripts you still need an IIFE: `(async () => { ... })();`.

### Challenge 6.6 — Top-Level Load

Using top-level `await`, fetch and log the title of post `1` — with no `async function`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
// Save as .mjs
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
const post = await response.json();
console.log(post.title);
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
5. PATCH post `1` to change its `title`, then DELETE post `2`. Log both statuses.
6. Use `Promise.allSettled` to fetch users `1`, `2`, and `999` and report which succeeded.
7. Build a `fetchWithRetry` helper that retries a failing URL up to 3 times.
8. Use `AbortController` to cancel a fetch after 20 ms and log `"cancelled"`.

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

// 5
async function updateAndDelete() {
  const patched = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Group Title" })
  });
  console.log("PATCH status:", patched.status);

  const deleted = await fetch("https://jsonplaceholder.typicode.com/posts/2", {
    method: "DELETE"
  });
  console.log("DELETE status:", deleted.status);
}

// 6
async function tolerant() {
  const results = await Promise.allSettled(
    [1, 2, 999].map(id =>
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
    )
  );
  results.forEach((r, i) => {
    if (r.status === "fulfilled") console.log(`user ${[1,2,999][i]}:`, r.value.name);
    else console.log(`user ${[1,2,999][i]} failed:`, r.reason.message);
  });
}

// 7
async function fetchWithRetry(url, retries = 3) {
  for (let i = 1; i <= retries; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      if (i === retries) throw error;
      console.log(`retry ${i}`);
    }
  }
}

// 8
async function cancelable() {
  const controller = new AbortController();
  setTimeout(() => controller.abort(), 20);
  try {
    await fetch("https://jsonplaceholder.typicode.com/users", { signal: controller.signal });
  } catch (error) {
    if (error.name === "AbortError") console.log("cancelled");
    else console.log("other error:", error.message);
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

### Level 7: Data Shaping

Fetch all users and log an array of `{ name, city }` (city comes from `user.address.city`).

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function shape() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const shaped = users.map(u => ({ name: u.name, city: u.address.city }));
  console.log(shaped);
}

shape();
```

</details>

### Level 8: PUT / PATCH

PATCH post `1` to change its `title` to `"Patched"`, then log the updated post.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function patch() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Patched" })
  });
  const data = await response.json();
  console.log(data);
}

patch();
```

</details>

### Level 9: allSettled

Use `Promise.allSettled` to fetch users `1`, `2`, and `999`. Log how many succeeded and how many failed.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function settled() {
  const results = await Promise.allSettled(
    [1, 2, 999].map(id =>
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
    )
  );
  const ok = results.filter(r => r.status === "fulfilled").length;
  console.log("succeeded:", ok, "failed:", results.length - ok);
}

settled();
```

</details>

### Level 10: Retry + Abort

Write `fetchWithAbort(url, ms)` that aborts the request if it takes longer than `ms` milliseconds, and retries once on `AbortError`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
async function fetchWithAbort(url, ms) {
  for (let attempt = 1; attempt <= 2; attempt++) {
    const controller = new AbortController();
    const timer = setTimeout(() => controller.abort(), ms);
    try {
      const response = await fetch(url, { signal: controller.signal });
      clearTimeout(timer);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      clearTimeout(timer);
      if (error.name === "AbortError" && attempt === 1) {
        console.log("timed out, retrying...");
        continue;
      }
      throw error;
    }
  }
}

fetchWithAbort("https://jsonplaceholder.typicode.com/users/1", 50)
  .then(d => console.log("ok:", d.name))
  .catch(e => console.log("failed:", e.message));
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
4. What happens if the user clicks "Load Posts" before any user is selected?
5. How would you cancel an in-flight request if the user clicks "Load Users" twice?

### Extension ideas

- Show each post's `body` under its title.
- Add a search box that filters the displayed users by name.
- Disable the buttons while a request is in flight.
- Use `AbortController` to cancel the previous request when a new one starts.
- Add a retry button next to error messages that re-runs the failed request.
- Fetch each user's posts in parallel with `Promise.all` after the users load.
- Show a count of posts per user next to their name in the dropdown.
- Add a "Refresh" button that re-fetches users without reloading the page.

---

## Mini Project 2: Parallel Profile Cards

### Goal

Load several user profiles **at the same time** and render them as cards — handling partial failures gracefully.

### Requirements

1. A button "Load 5 Profiles".
2. On click, fetch users `1` through `5` **in parallel** with `Promise.allSettled`.
3. Render a card for each *successful* result with the user's name, email, and company.
4. For each *failed* result, render a card with the error message and a "Retry" button.
5. Show a single loading indicator while all requests are in flight.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Parallel Profile Cards</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 20px auto; }
    .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .card { background: #f9f9f9; padding: 12px; border-radius: 6px; border: 1px solid #ddd; }
    .card.error { background: #fff0f0; border-color: #f99; }
    .loading { color: blue; }
    button { margin: 4px 0; }
  </style>
</head>
<body>
  <h1>Parallel Profile Cards</h1>
  <button id="load">Load 5 Profiles</button>
  <div id="status"></div>
  <div id="cards" class="grid"></div>

  <script>
    const API = "https://jsonplaceholder.typicode.com";

    async function fetchUser(id) {
      const response = await fetch(`${API}/users/${id}`);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return response.json();
    }

    function cardFor(user) {
      const div = document.createElement("div");
      div.className = "card";
      div.innerHTML = `
        <h3>${user.name}</h3>
        <p>${user.email}</p>
        <p><small>${user.company.name}</small></p>
      `;
      return div;
    }

    function errorCardFor(id, message) {
      const div = document.createElement("div");
      div.className = "card error";
      div.innerHTML = `
        <h3>User ${id} failed</h3>
        <p>${message}</p>
        <button data-retry="${id}">Retry</button>
      `;
      return div;
    }

    async function loadAll() {
      const status = document.getElementById("status");
      const cards = document.getElementById("cards");
      status.textContent = "Loading...";
      cards.innerHTML = "";

      const ids = [1, 2, 3, 4, 5];
      const results = await Promise.allSettled(ids.map(fetchUser));

      status.textContent = `Done — ${results.filter(r => r.status === "fulfilled").length}/${ids.length} succeeded`;
      results.forEach((r, i) => {
        cards.appendChild(
          r.status === "fulfilled" ? cardFor(r.value) : errorCardFor(ids[i], r.reason.message)
        );
      });
    }

    document.getElementById("load").addEventListener("click", loadAll);

    document.getElementById("cards").addEventListener("click", async (e) => {
      if (e.target.dataset.retry) {
        const id = e.target.dataset.retry;
        const card = e.target.closest(".card");
        card.innerHTML = "Retrying...";
        try {
          const user = await fetchUser(id);
          card.replaceWith(cardFor(user));
        } catch (error) {
          card.replaceWith(errorCardFor(id, error.message));
        }
      }
    });
  </script>
</body>
</html>
```

### Questions to think about

1. Why `Promise.allSettled` instead of `Promise.all` here?
2. What does `e.target.closest(".card")` do?
3. How would you add a per-card loading spinner during retry?

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

11. What does `response.ok` represent?
    - [ ] Whether the response body is valid JSON
    - [ ] Whether the HTTP status is in the 200–299 range
    - [ ] Whether the request used HTTPS
    - [ ] Whether the server is online

12. Why must you `await response.json()` separately from `await fetch(...)`?
    - [ ] Because `fetch` returns the body as a string
    - [ ] Because `fetch` resolves with a Response object and reading the body is itself async
    - [ ] Because JSON parsing is synchronous and slow
    - [ ] You don't — `fetch` already returns the parsed data

13. What is the difference between `Promise.all` and `Promise.allSettled`?
    - [ ] They are identical
    - [ ] `all` rejects on first rejection; `allSettled` always resolves with status objects
    - [ ] `allSettled` is faster
    - [ ] `all` only works with `fetch`

14. What does `AbortController` let you do?
    - [ ] Pause a promise
    - [ ] Cancel an in-flight `fetch` request
    - [ ] Retry a failed request
    - [ ] Cache a response

15. What is the difference between PUT and PATCH?
    - [ ] PUT creates, PATCH deletes
    - [ ] PUT replaces the whole resource; PATCH updates only the sent fields
    - [ ] They are the same method
    - [ ] PATCH is for JSON only

16. Where is top-level `await` allowed?
    - [ ] Anywhere
    - [ ] Only inside `async` functions
    - [ ] In ES modules (and CommonJS with a loader that supports it)
    - [ ] Only in the browser console

---

## Additional Resources

- [MDN: JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [MDN: Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN: Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN: AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)
- [MDN: URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
- [MDN: Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) - Free fake API for testing
- [JavaScript.info: Promises](https://javascript.info/promise-basics)
- [JavaScript.info: async/await](https://javascript.info/async)
- [JavaScript.info: Fetch](https://javascript.info/fetch)
- [Lydia Hallie: JavaScript Visualized — Promises & Async/Await](https://lydiahallie.github.io/javascript-questions/questions/async/)
- [HTTP status codes reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

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

### Challenge 4.1b — Inspect the Response

```javascript
async function inspect() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  console.log("status:", response.status);
  console.log("ok:", response.ok);
  console.log("content-type:", response.headers.get("content-type"));
}
```

### Challenge 4.3 — Update and Delete

```javascript
async function updateAndDelete() {
  const patched = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Updated" })
  }).then(r => r.json());
  console.log("Patched:", patched);

  const deleteRes = await fetch("https://jsonplaceholder.typicode.com/posts/2", {
    method: "DELETE"
  });
  console.log("Delete status:", deleteRes.status);
}
```

### Challenge 4.4 — Filter and Auth

```javascript
async function filtered() {
  const params = new URLSearchParams({ userId: 2, _limit: 5 });
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts?${params}`, {
    headers: { "Authorization": "Bearer fake-token" }
  });
  const posts = await response.json();
  console.log(posts);
}
```

### Challenge 5.1b — Group by City

```javascript
async function groupByCity() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const byCity = users.reduce((groups, u) => {
    const city = u.address.city;
    (groups[city] ??= []).push(u);
    return groups;
  }, {});
  for (const city in byCity) console.log(city, ":", byCity[city].length);
}
```

### Challenge 5.2b — Tolerant Fetch

```javascript
async function tolerant() {
  const ids = [1, 2, 999];
  const results = await Promise.allSettled(
    ids.map(id =>
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
    )
  );
  results.forEach((r, i) => {
    if (r.status === "fulfilled") console.log(`user ${ids[i]}:`, r.value.name);
    else console.log(`user ${ids[i]} failed:`, r.reason.message);
  });
}
```

### Challenge 5.2c — Timeout with race

```javascript
async function withTimeout() {
  try {
    const user = await Promise.race([
      fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
      new Promise((_, reject) => setTimeout(() => reject(new Error("timeout")), 50))
    ]);
    console.log(user.name);
  } catch (error) {
    console.log("Failed:", error.message);
  }
}
```

### Challenge 6.2b — Loading Spinner with finally

```javascript
let isLoading = false;

async function loadUser() {
  isLoading = true;
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    const user = await response.json();
    console.log("Loaded:", user.name);
  } catch (error) {
    console.log("Error:", error.message);
  } finally {
    isLoading = false;
    console.log("isLoading is now", isLoading);
  }
}
```

### Challenge 6.3 — Make It Parallel

```javascript
async function fast() {
  const [posts, albums] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/albums?userId=1").then(r => r.json())
  ]);
  console.log(posts.length, albums.length);
}
```

### Challenge 6.4 — Cancel on Re-click

```javascript
let currentController = null;

async function loadUsers() {
  currentController?.abort();
  currentController = new AbortController();
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users", {
      signal: currentController.signal
    });
    const users = await response.json();
    console.log("got", users.length, "users");
  } catch (error) {
    if (error.name !== "AbortError") console.log("Error:", error.message);
  } finally {
    currentController = null;
  }
}
```

### Challenge 6.5 — Retry with a Limit

```javascript
async function retry(url, max = 2) {
  for (let i = 1; i <= max; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      console.log(`try ${i} failed: ${error.message}`);
      if (i === max) console.log("giving up");
    }
  }
}
```

### Challenge 6.6 — Top-Level Load

```javascript
// Save as .mjs
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
const post = await response.json();
console.log(post.title);
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

// Level 7
async function shape() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();
  const shaped = users.map(u => ({ name: u.name, city: u.address.city }));
  console.log(shaped);
}

// Level 8
async function patch() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title: "Patched" })
  });
  const data = await response.json();
  console.log(data);
}

// Level 9
async function settled() {
  const results = await Promise.allSettled(
    [1, 2, 999].map(id =>
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`).then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
    )
  );
  const ok = results.filter(r => r.status === "fulfilled").length;
  console.log("succeeded:", ok, "failed:", results.length - ok);
}

// Level 10
async function fetchWithAbort(url, ms) {
  for (let attempt = 1; attempt <= 2; attempt++) {
    const controller = new AbortController();
    const timer = setTimeout(() => controller.abort(), ms);
    try {
      const response = await fetch(url, { signal: controller.signal });
      clearTimeout(timer);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      clearTimeout(timer);
      if (error.name === "AbortError" && attempt === 1) {
        console.log("timed out, retrying...");
        continue;
      }
      throw error;
    }
  }
}
```

</details>
