# Session 8: Node.js Foundations, Express Setup & Routing

**Duration:** 3 hours
**Level:** Beginner
**Prerequisites:** JavaScript ES6+, command line, npm basics
**Condensed from:** Node Sessions 1–2

---

> **How to use this session**
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> **Testing your API:** GET routes work in the browser. For POST/PUT/PATCH/DELETE use **Postman** or the **Thunder Client** extension in VS Code, with the body type set to **JSON**.

## Session Overview

- **Part 1:** What Node.js is and why it fits backends
- **Part 2:** Express — setup, first server, request/response
- **Part 3:** HTTP methods — GET, POST, PUT, PATCH, DELETE
- **Part 4:** Route params, query strings, request body
- **Part 5:** `express.Router()`, modular routes, project structure
- **Lab:** In-memory CRUD API (to-do list)

---

## Part 1: Node.js

**Node.js** is a JavaScript runtime built on Chrome's V8 engine — it runs JS outside the browser, so one language covers frontend and backend.

**Why it's good for backends:**
- Non-blocking, event-driven I/O → handles many concurrent connections
- JSON-native data handling
- Massive ecosystem via npm
- Great for REST APIs and real-time apps

**How it works:** your code runs in a single process with an event loop; async work is handled via callbacks, promises, and `async/await`. Modules load with `require` (CommonJS) or `import` (ESM).

```js
const fs = require('fs');
fs.readFile('data.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

**Gotchas:** browser-only APIs (`window`, `document`) don't exist; don't write blocking code (it stalls the event loop); always handle async errors.

### Raw `http` vs Express

Node's built-in `http` module can serve requests, but it's verbose:

```js
const http = require('http');
const server = http.createServer((req, res) => {
  if (req.url === '/api/users') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify([{ id: 1, name: 'Alice' }]));
  } else {
    res.writeHead(404); res.end('Not found');
  }
});
server.listen(3000);
```

Express wraps this with clean routing, middleware, and response helpers — use it for anything non-trivial.

---

### ✍️ Your Turn: Part 1

Create `hello.js` and run it with `node hello.js`.

```js
// 1. Print a message and the Node version
console.log('Hello from Node!');
console.log('Node version:', process.____);

// 2. Use a built-in module: write a file, then read it back
const fs = require('____');
fs.writeFileSync('note.txt', 'My first file from Node');
const text = fs.____('note.txt', 'utf8');
console.log(text);

// 3. TODO: use the built-in `os` module to print the platform (os.platform())
//    and the number of CPUs (os.cpus().length)

// 4. Question: which of these do NOT exist in Node?
//    window, process, document, require, console
```

<details>
<summary>✅ Solution</summary>

```js
console.log('Hello from Node!');
console.log('Node version:', process.version);

const fs = require('fs');
fs.writeFileSync('note.txt', 'My first file from Node');
const text = fs.readFileSync('note.txt', 'utf8');
console.log(text);

const os = require('os');
console.log('Platform:', os.platform());
console.log('CPUs:', os.cpus().length);
```

4. `window` and `document`. They are browser-only.

</details>

---

## Part 2: Express Setup & First Server

**Express** = minimal web framework for Node: routing, middleware, request/response conveniences, huge ecosystem.

### Project setup

```bash
mkdir my-api && cd my-api
npm init -y                    # package.json
npm install express            # runtime dependency
npm install --save-dev nodemon # dev-only: auto-restart on file change
```

```json
// package.json scripts
{ "start": "node app.js", "dev": "nodemon app.js" }
```

`.gitignore`: `node_modules/`, `.env`. Never commit dependencies or secrets.

### First server

```js
const express = require('express');
const app = express();

app.use(express.json());                    // parses JSON request bodies — MUST come before routes

app.get('/', (req, res) => {
  res.send('Hello Express');
});

app.get('/api/health', (req, res) => {
  res.json({ status: 'ok', uptime: process.uptime() });
});

const PORT = process.env.PORT || 3000;      // env port for deployment, fallback for dev
app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
```

**Common mistakes:** forgetting `app.listen`, forgetting `express.json()` (so `req.body` is `undefined` on POST), middleware registered *after* routes, hardcoding the port.

### Request & response objects

| `req` property | Contains | Example URL |
|----------------|----------|-------------|
| `req.params` | Route placeholders | `/users/:id` → `{ id: '42' }` |
| `req.query` | Query string | `?page=2&q=abc` → `{ page: '2', q: 'abc' }` |
| `req.body` | Parsed body (needs `express.json()`) | `{"name":"x"}` |
| `req.headers` | Request headers | `authorization`, `content-type` |

| `res` method | Purpose |
|--------------|---------|
| `res.send(x)` | String/buffer/HTML |
| `res.json(x)` | JSON response (use this for APIs) |
| `res.status(n)` | Set HTTP status — chainable: `res.status(404).json(...)` |
| `res.redirect(url)` | Redirect |

**Rule:** always `return` after sending an early response, and never send two responses for one request.

```js
app.get('/api/todos/:id', (req, res) => {
  const todo = todos.find(t => t.id === Number(req.params.id));
  if (!todo) return res.status(404).json({ error: 'Todo not found' }); // return!
  res.json(todo);
});
```

---

### ✍️ Your Turn: Part 2

Create a project `my-api` (`npm init -y`, `npm install express`, `npm install -D nodemon`), then `app.js`:

```js
const express = require('____');
const app = ____();

app.use(express.____());     // so req.body works for JSON

// 1. GET / → the text "Welcome to my API"
app.get('/', (req, res) => {
  res.____('Welcome to my API');
});

// 2. TODO: GET /api/about → JSON { name: "My API", version: "1.0.0" }

// 3. TODO: GET /api/time  → JSON { now: <current date as ISO string> }

const PORT = process.env.PORT || ____;
app.____(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
```

**4.** Add a `dev` script that runs the server with nodemon, then start it with `npm run dev`. Change a message and save: does the server restart by itself?

> **Note:** `npm install express` now installs **Express 5**. In Express 5, `req.body` is `undefined` when a request has no JSON body, so always send a JSON body (and `Content-Type: application/json`) for POST/PUT/PATCH.

<details>
<summary>✅ Solution</summary>

```js
const express = require('express');
const app = express();

app.use(express.json());

app.get('/', (req, res) => {
  res.send('Welcome to my API');
});

app.get('/api/about', (req, res) => {
  res.json({ name: 'My API', version: '1.0.0' });
});

app.get('/api/time', (req, res) => {
  res.json({ now: new Date().toISOString() });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
```

```json
"scripts": {
  "start": "node app.js",
  "dev": "nodemon app.js"
}
```

</details>

---

## Part 3: HTTP Methods & REST

| Method | Purpose | Success status | Notes |
|--------|---------|----------------|-------|
| `GET` | Read | 200 | Safe + idempotent; no body; never mutate state |
| `POST` | Create | 201 | Body carries the new resource |
| `PUT` | Replace (full update) | 200 | Idempotent |
| `PATCH` | Update (partial) | 200 | Only provided fields change |
| `DELETE` | Remove | 204 or 200 | 204 = no content |

```js
// CREATE
app.post('/api/todos', (req, res) => {
  const todo = { id: nextId++, text: req.body.text, completed: false };
  todos.push(todo);
  res.status(201).json(todo);          // 201 Created + return the resource
});

// UPDATE (full vs partial)
app.put('/api/todos/:id', (req, res) => {              // replace every field
  const todo = find(req.params.id);
  if (!todo) return res.status(404).json({ error: 'Not found' });
  todo.text = req.body.text; todo.completed = req.body.completed;
  res.json(todo);
});
app.patch('/api/todos/:id', (req, res) => {            // merge provided fields
  const todo = find(req.params.id);
  if (!todo) return res.status(404).json({ error: 'Not found' });
  Object.assign(todo, req.body);
  res.json(todo);
});

// DELETE
app.delete('/api/todos/:id', (req, res) => {
  const i = todos.findIndex(t => t.id === Number(req.params.id));
  if (i === -1) return res.status(404).json({ error: 'Not found' });
  todos.splice(i, 1);
  res.status(204).send();
});
```

**Best practices:** GET never mutates; always 404 on missing resources; compare IDs with `Number()`/`String()` to avoid type coercion bugs; validate body before creating.

---

### ✍️ Your Turn: Part 3

Add a books API to `app.js` (data in memory).

```js
let nextId = 3;
let books = [
  { id: 1, title: 'Clean Code', author: 'Robert Martin' },
  { id: 2, title: 'Atomic Habits', author: 'James Clear' },
];

// READ all
app.____('/api/books', (req, res) => {
  res.json(books);
});

// READ one
app.get('/api/books/:id', (req, res) => {
  const book = books.find(b => b.id === ____(req.params.id));
  // TODO: 404 { error: 'Book not found' } if missing, otherwise send the book
});

// CREATE
app.____('/api/books', (req, res) => {
  // TODO: if title is missing → 400 { error: 'title is required' }
  const book = { id: nextId++, title: req.body.title, author: req.body.author };
  books.push(book);
  res.status(____).json(book);
});

// DELETE
app.____('/api/books/:id', (req, res) => {
  const index = books.findIndex(b => b.id === Number(req.params.id));
  if (index === -1) return res.status(____).json({ error: 'Book not found' });
  books.splice(index, 1);
  res.status(____).send();
});

// TODO: PATCH /api/books/:id → change only the fields that were sent (404 if missing)
```

**Test in Postman:** list → create → get the new one → patch its title → delete it → get it again (should be 404).

<details>
<summary>✅ Solution</summary>

```js
app.get('/api/books', (req, res) => {
  res.json(books);
});

app.get('/api/books/:id', (req, res) => {
  const book = books.find(b => b.id === Number(req.params.id));
  if (!book) return res.status(404).json({ error: 'Book not found' });
  res.json(book);
});

app.post('/api/books', (req, res) => {
  if (!req.body.title) return res.status(400).json({ error: 'title is required' });
  const book = { id: nextId++, title: req.body.title, author: req.body.author };
  books.push(book);
  res.status(201).json(book);
});

app.delete('/api/books/:id', (req, res) => {
  const index = books.findIndex(b => b.id === Number(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'Book not found' });
  books.splice(index, 1);
  res.status(204).send();
});

app.patch('/api/books/:id', (req, res) => {
  const book = books.find(b => b.id === Number(req.params.id));
  if (!book) return res.status(404).json({ error: 'Book not found' });
  if (req.body.title !== undefined) book.title = req.body.title;
  if (req.body.author !== undefined) book.author = req.body.author;
  res.json(book);
});
```

</details>

---

## Part 4: Params, Query, Body

```js
// Route params — identify a resource: /users/42
app.get('/users/:id', (req, res) => {
  const id = Number(req.params.id);   // params are STRINGS — convert
});

// Query strings — optional modifiers: /api/todos?completed=true&page=2
app.get('/api/todos', (req, res) => {
  let result = todos;
  if (req.query.completed !== undefined) {
    result = result.filter(t => t.completed === (req.query.completed === 'true'));
  }
  res.json(result);
});

// Body — structured payloads for POST/PUT/PATCH
app.post('/echo', (req, res) => res.json(req.body));
```

- Params = *which* resource; query = *how* to return it (filter/sort/paginate/search)
- Both arrive as strings — convert explicitly
- Provide defaults for optional query params

---

### ✍️ Your Turn: Part 4

```js
// 1. Route params: GET /api/greet/Sara → { message: "Hello, Sara!" }
app.get('/api/greet/:____', (req, res) => {
  res.json({ message: `Hello, ${req.params.____}!` });
});

// 2. Query string: GET /api/sum?a=5&b=7 → { result: 12 }
app.get('/api/sum', (req, res) => {
  const a = ____(req.query.a);
  const b = ____(req.query.b);
  // TODO: if a or b is not a number → 400 { error: 'a and b must be numbers' }
  //       hint: Number.isNaN(a)
  res.json({ result: ____ });
});

// 3. Body: POST /api/login with { "username": "admin", "password": "1234" }
app.post('/api/login', (req, res) => {
  const { username, password } = req.____;
  // TODO: correct → { message: 'Welcome admin' }
  //       wrong   → 401 { error: 'Wrong username or password' }
});

// 4. TODO: GET /api/books?author=james → only books whose author contains the text
//    (ignore upper/lower case). Without ?author → all books.
//    Replace your GET /api/books route from Part 3 (Express uses the FIRST matching route).
```

<details>
<summary>✅ Solution</summary>

```js
app.get('/api/greet/:name', (req, res) => {
  res.json({ message: `Hello, ${req.params.name}!` });
});

app.get('/api/sum', (req, res) => {
  const a = Number(req.query.a);
  const b = Number(req.query.b);
  if (Number.isNaN(a) || Number.isNaN(b)) {
    return res.status(400).json({ error: 'a and b must be numbers' });
  }
  res.json({ result: a + b });
});

app.post('/api/login', (req, res) => {
  const { username, password } = req.body;
  if (username === 'admin' && password === '1234') {
    return res.json({ message: 'Welcome admin' });
  }
  res.status(401).json({ error: 'Wrong username or password' });
});

app.get('/api/books', (req, res) => {
  const { author } = req.query;
  if (!author) return res.json(books);
  const result = books.filter(b => b.author.toLowerCase().includes(author.toLowerCase()));
  res.json(result);
});
```

</details>

---

## Part 5: Express Router & Project Structure

### `express.Router()` — mountable route modules

```js
// routes/todos.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => res.json(todos));        // mounted at /api/todos → GET /api/todos
router.get('/:id', (req, res) => { /* … */ });
router.post('/', (req, res) => { /* … */ });

module.exports = router;

// app.js
const todoRoutes = require('./routes/todos');
app.use('/api/todos', todoRoutes);
```

One router file per resource; routers map paths → controllers handle logic.

### Scalable structure

```
my-api/
├── app.js              # app setup, middleware, mounts routers
├── server.js           # only starts the server (separation → testable app)
├── routes/             # users.js, products.js, index.js
├── controllers/        # business logic per resource
├── models/             # data layer (Session 9+)
├── middleware/         # auth, logger, error handler
├── .env                # config/secrets — NEVER commit
├── .gitignore
└── package.json
```

`app.js` vs `server.js` split:

```js
// app.js — export the configured app (tests can import it)
const app = express();
app.use(express.json());
app.use('/api/todos', todoRoutes);
module.exports = app;

// server.js — starts listening
const app = require('./app');
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server on :${PORT}`));
```

### Nodemon

Dev-time auto-restart: `npm i -D nodemon`, `"dev": "nodemon server.js"`. **Dev only** — production uses plain `node`.

---

### ✍️ Your Turn: Part 5

Move the books routes out of `app.js`.

```js
// routes/books.js
const express = require('express');
const router = express.____();

let nextId = 3;
let books = [
  { id: 1, title: 'Clean Code', author: 'Robert Martin' },
  { id: 2, title: 'Atomic Habits', author: 'James Clear' },
];

router.get('____', (req, res) => res.json(books));       // GET  /api/books
router.get('____', (req, res) => { /* TODO */ });         // GET  /api/books/:id
router.post('____', (req, res) => { /* TODO */ });        // POST /api/books

module.exports = ____;
```

```js
// app.js
const express = require('express');
const bookRoutes = require('./routes/____');

const app = express();
app.use(express.json());
app.use('____', bookRoutes);

// TODO: a 404 handler for unknown routes → { error: 'Route not found' } (must be LAST)

module.exports = app;
```

```js
// server.js
const app = require('____');
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server on :${PORT}`));
```

<details>
<summary>✅ Solution</summary>

```js
// routes/books.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => res.json(books));

router.get('/:id', (req, res) => {
  const book = books.find(b => b.id === Number(req.params.id));
  if (!book) return res.status(404).json({ error: 'Book not found' });
  res.json(book);
});

router.post('/', (req, res) => {
  if (!req.body.title) return res.status(400).json({ error: 'title is required' });
  const book = { id: nextId++, title: req.body.title, author: req.body.author };
  books.push(book);
  res.status(201).json(book);
});

module.exports = router;
```

```js
// app.js
const bookRoutes = require('./routes/books');
app.use('/api/books', bookRoutes);
app.use((req, res) => res.status(404).json({ error: 'Route not found' }));
module.exports = app;

// server.js
const app = require('./app');
```

Change the `dev` script to `nodemon server.js`.

</details>

---

## Lab: In-Memory CRUD To-Do API

**Requirements:**

```
GET    /api/todos           list all (+ ?completed=true filter)
GET    /api/todos/:id       one todo (404 if missing)
POST   /api/todos           create (validate `text`)
PUT    /api/todos/:id       full replace
PATCH  /api/todos/:id       partial update
DELETE /api/todos/:id       delete (204)
```

```js
// server.js — starter
const express = require('express');
const app = express();
app.use(express.json());

let nextId = 3;
let todos = [
  { id: 1, text: 'Learn Express', completed: false },
  { id: 2, text: 'Build API', completed: false },
];

app.get('/api/todos', (req, res) => {
  let result = todos;
  if (req.query.completed !== undefined) {
    const want = req.query.completed === 'true';
    result = result.filter(t => t.completed === want);
  }
  res.json(result);
});

app.get('/api/todos/:id', (req, res) => {
  const todo = todos.find(t => t.id === Number(req.params.id));
  if (!todo) return res.status(404).json({ error: 'Todo not found' });
  res.json(todo);
});

app.post('/api/todos', (req, res) => {
  if (!req.body.text) return res.status(400).json({ error: 'text is required' });
  const todo = { id: nextId++, text: req.body.text, completed: false };
  todos.push(todo);
  res.status(201).json(todo);
});

// implement PUT, PATCH, DELETE…

app.listen(process.env.PORT || 3000);
```

**Challenges:**
1. Move routes into `routes/todos.js` + mount with `app.use`
2. Split `app.js`/`server.js`
3. Add a 404 handler for unknown routes (`app.use((req, res) => res.status(404).json({ error: 'Route not found' }))` — last, after all routes)

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is Node.js? Why can it run JavaScript outside the browser?
2. Why do we use Express instead of the built-in `http` module?
3. What does `app.use(express.json())` do?
4. What is the difference between `res.send()` and `res.json()`?
5. Which HTTP method and status code do you use to **create** something? To **delete** something?
6. In `/api/users/42?sort=name`, what is in `req.params` and what is in `req.query`?
7. Why do we write `Number(req.params.id)`?
8. Why must you `return` after sending an error response?
9. What is `express.Router()` used for?
10. Why should `nodemon` be a dev dependency?

<details>
<summary>✅ Solution</summary>

1. A program that runs JavaScript outside the browser. It uses Chrome's V8 engine.
2. Express gives simple routing, middleware and helpers like `res.json()`, so you write much less code.
3. It reads the JSON body of the request and puts it in `req.body`.
4. `res.send()` sends text/HTML (or any data). `res.json()` sends JSON and sets the JSON content type, which is what APIs use.
5. Create: `POST` → `201`. Delete: `DELETE` → `204` (or `200`).
6. `req.params` = `{ id: '42' }`, `req.query` = `{ sort: 'name' }`.
7. Params are always strings, and our ids are numbers. `'1' === 1` is `false`.
8. Without `return`, the code continues and tries to send a second response, which is an error.
9. To put the routes of one resource (for example books) in their own file and mount them on a path.
10. It's only needed while developing (to restart on save). Production runs `node`.

</details>

### B. Find the Bug

```js
// Bug 1: req.body is undefined
const app = express();
app.post('/api/users', (req, res) => res.json(req.body));

// Bug 2: GET /api/users/1 always returns 404
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === req.params.id);
  if (!user) return res.status(404).json({ error: 'Not found' });
  res.json(user);
});

// Bug 3: "Cannot set headers after they are sent to the client"
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === Number(req.params.id));
  if (!user) res.status(404).json({ error: 'Not found' });
  res.json(user);
});

// Bug 4: the request never finishes (the browser keeps loading)
app.get('/api/hello', (req, res) => {
  const message = 'Hello';
});

// Bug 5: works on my laptop, but not after deploying
app.listen(3000);
```

<details>
<summary>✅ Solution</summary>

```js
// Bug 1: add the JSON body parser BEFORE the routes
app.use(express.json());

// Bug 2: compare numbers with numbers
const user = users.find(u => u.id === Number(req.params.id));

// Bug 3: add return
if (!user) return res.status(404).json({ error: 'Not found' });

// Bug 4: no response was sent
app.get('/api/hello', (req, res) => {
  const message = 'Hello';
  res.json({ message });
});

// Bug 5: hosting platforms give the port in an environment variable
const PORT = process.env.PORT || 3000;
app.listen(PORT);
```

</details>

### C. Coding Tasks

**Task 1: Calculator API (easy).** `GET /api/calc/:op?a=10&b=5` where `op` is `add`, `subtract`, `multiply` or `divide` → `{ result }`. Return 400 if `a`/`b` are not numbers or when dividing by zero, and 404 for an unknown `op`.

**Task 2: Students API (medium).** Full CRUD in memory for `{ id, name, grade }`:
- `GET /api/students` (optional `?minGrade=50`), `GET /api/students/:id`
- `POST /api/students` and `PUT /api/students/:id`: `name` is required and `grade` must be a number from 0 to 100, otherwise 400
- `DELETE /api/students/:id` → 204
- 404 for unknown ids

**Task 3: Products router (medium).** Put a products API in `routes/products.js` and mount it at `/api/products`. `GET /` supports `?category=books` and `?sort=price` (cheapest first).

**Task 4: Quotes API (easy).** `GET /api/quotes` (all), `GET /api/quotes/random` (one random quote), `POST /api/quotes` with `{ text }` (400 if empty).

**Challenge: Notes with search + pagination.** Create 23 notes in memory. `GET /api/notes?q=1&page=2&limit=5` returns `{ data, total, page }`. `q` filters by text, `page` starts at 1, and `limit` defaults to 5 (max 50).

<details>
<summary>✅ Solution: Tasks 1–4 and Challenge</summary>

```js
// Task 1
app.get('/api/calc/:op', (req, res) => {
  const a = Number(req.query.a);
  const b = Number(req.query.b);
  if (Number.isNaN(a) || Number.isNaN(b)) {
    return res.status(400).json({ error: 'a and b must be numbers' });
  }
  const { op } = req.params;
  if (op === 'add') return res.json({ result: a + b });
  if (op === 'subtract') return res.json({ result: a - b });
  if (op === 'multiply') return res.json({ result: a * b });
  if (op === 'divide') {
    if (b === 0) return res.status(400).json({ error: 'Cannot divide by zero' });
    return res.json({ result: a / b });
  }
  res.status(404).json({ error: `Unknown operation: ${op}` });
});

// Task 2
let nextStudentId = 3;
let students = [
  { id: 1, name: 'Ali', grade: 85 },
  { id: 2, name: 'Mona', grade: 42 },
];

function validateStudent(body = {}) {
  if (!body.name || !body.name.trim()) return 'name is required';
  if (typeof body.grade !== 'number' || body.grade < 0 || body.grade > 100) {
    return 'grade must be a number from 0 to 100';
  }
  return null;
}

app.get('/api/students', (req, res) => {
  let result = students;
  if (req.query.minGrade !== undefined) {
    const min = Number(req.query.minGrade);
    result = result.filter(s => s.grade >= min);
  }
  res.json(result);
});

app.get('/api/students/:id', (req, res) => {
  const student = students.find(s => s.id === Number(req.params.id));
  if (!student) return res.status(404).json({ error: 'Student not found' });
  res.json(student);
});

app.post('/api/students', (req, res) => {
  const error = validateStudent(req.body);
  if (error) return res.status(400).json({ error });
  const student = { id: nextStudentId++, name: req.body.name.trim(), grade: req.body.grade };
  students.push(student);
  res.status(201).json(student);
});

app.put('/api/students/:id', (req, res) => {
  const student = students.find(s => s.id === Number(req.params.id));
  if (!student) return res.status(404).json({ error: 'Student not found' });
  const error = validateStudent(req.body);
  if (error) return res.status(400).json({ error });
  student.name = req.body.name.trim();
  student.grade = req.body.grade;
  res.json(student);
});

app.delete('/api/students/:id', (req, res) => {
  const index = students.findIndex(s => s.id === Number(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'Student not found' });
  students.splice(index, 1);
  res.status(204).send();
});

// Task 3: routes/products.js
const router = require('express').Router();

const products = [
  { id: 1, name: 'Laptop', price: 999, category: 'electronics' },
  { id: 2, name: 'Book', price: 15, category: 'books' },
  { id: 3, name: 'Mouse', price: 25, category: 'electronics' },
];

router.get('/', (req, res) => {
  let result = [...products];                       // copy before sorting
  if (req.query.category) result = result.filter(p => p.category === req.query.category);
  if (req.query.sort === 'price') result.sort((a, b) => a.price - b.price);
  res.json(result);
});

module.exports = router;
// app.js → app.use('/api/products', require('./routes/products'));

// Task 4
const quotes = ['Talk is cheap. Show me the code.', 'First, solve the problem. Then, write the code.'];

app.get('/api/quotes', (req, res) => res.json(quotes));

app.get('/api/quotes/random', (req, res) => {
  const i = Math.floor(Math.random() * quotes.length);
  res.json({ quote: quotes[i] });
});

app.post('/api/quotes', (req, res) => {
  const text = req.body?.text?.trim();
  if (!text) return res.status(400).json({ error: 'text is required' });
  quotes.push(text);
  res.status(201).json({ quote: text });
});

// Challenge
const notes = Array.from({ length: 23 }, (_, i) => ({ id: i + 1, text: `Note number ${i + 1}` }));

app.get('/api/notes', (req, res) => {
  const q = String(req.query.q ?? '').toLowerCase();
  const page = Math.max(Number(req.query.page) || 1, 1);
  const limit = Math.min(Math.max(Number(req.query.limit) || 5, 1), 50);

  const filtered = notes.filter(n => n.text.toLowerCase().includes(q));
  const start = (page - 1) * limit;

  res.json({ data: filtered.slice(start, start + limit), total: filtered.length, page });
});
```

</details>

---

## Review Questions

1. What's the difference between `req.params`, `req.query`, and `req.body`?
2. Why does `req.params.id` need `Number()` before comparing to `t.id`?
3. PUT vs PATCH — which for partial updates?
4. Why `201` for POST and `204` for DELETE?
5. What does `express.json()` do, and why must it be registered before routes?
6. Why split `app.js` and `server.js`?
7. What does `express.Router()` give you?

## Common Mistakes

- Missing `app.use(express.json())` → `req.body` undefined
- Sending a response twice, or forgetting `return` after an error response
- Registering middleware after routes (it never runs for them)
- String-vs-number ID comparisons silently failing
- Using GET for mutations
- Nodemon in production
- Committing `node_modules/` or `.env`

---

**Session 8 complete.** Next: Session 9 — Middleware & MongoDB.
