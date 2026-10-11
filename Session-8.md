# Session 8: Node.js Foundations, Express Setup & Routing

**Duration:** 3 hours
**Level:** Beginner
**Prerequisites:** JavaScript ES6+, command line, npm basics
**Condensed from:** Node Sessions 1–2

---

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
