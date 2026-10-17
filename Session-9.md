# Session 9: Express Middleware & MongoDB Fundamentals

**Duration:** 3 hours
**Level:** Beginner–Intermediate
**Prerequisites:** Session 8 (Express basics, routing, req/res)
**Condensed from:** Node Sessions 3–4

---

> **How to use this session**
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

## Session Overview

- **Part 1:** Middleware — what it is, request lifecycle, ordering
- **Part 2:** Built-in middleware — `express.json`, `urlencoded`, `static`
- **Part 3:** Third-party middleware — morgan, cors, helmet
- **Part 4:** Custom middleware + error middleware
- **Part 5:** MongoDB — NoSQL concepts, documents & collections, setup
- **Part 6:** CRUD in the shell — insert/find/update/delete, operators, projection
- **Lab:** Middleware-hardened API + MongoDB query practice

---

## Part 1: Middleware & the Request Lifecycle

**Middleware** = functions that sit between the incoming request and your route handler. Each gets `(req, res, next)` and can inspect/modify the request, end the response, or pass control with `next()`.

```
Request → middleware 1 → middleware 2 → route handler → Response
```

```js
// Global middleware — runs for EVERY request
app.use((req, res, next) => {
  req.requestTime = Date.now();        // attach data for later handlers
  console.log(`${req.method} ${req.url}`);
  next();                               // REQUIRED — or the request hangs
});

// Route-scoped middleware — only on this route
function authCheck(req, res, next) {
  if (!req.headers.authorization) {
    return res.status(401).json({ error: 'Unauthorized' });  // ends cycle, no next()
  }
  next();
}
app.use('/api/admin', authCheck);                          // path-scoped
app.post('/api/todos', validateTodo, createTodo);          // per-route
```

**Critical rules:**
- Middleware runs in **registration order** — `express.json()` before routes, error handler last
- Always `next()` **or** send a response — never both, never neither
- `return` after sending an error response
- Keep each middleware small and single-purpose

---

### ✍️ Your Turn: Part 1

Use your `my-api` project from Session 8.

```js
// 1. Logger: print "GET /api/books" for every request, then continue
app.use((req, res, ____) => {
  console.log(`${req.____} ${req.____}`);
  ____();
});

// 2. Timer: print how long each request took
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {           // runs when the response has been sent
    console.log(`Took ${____} ms`);
  });
  next();
});

// 3. API key check, ONLY for /api/admin
function checkApiKey(req, res, next) {
  // TODO: if req.headers['x-api-key'] !== 'secret123' → 401 { error: 'Invalid API key' }
  // TODO: otherwise continue
}
app.use('/api/admin', ____);
app.get('/api/admin/stats', (req, res) => res.json({ users: 10 }));

// 4. Question: what happens if a middleware never calls next() and never sends a response?
```

**Test:** call `/api/admin/stats` in Postman without a header (401), then with the header `x-api-key: secret123` (200).

<details>
<summary>✅ Solution</summary>

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    console.log(`Took ${Date.now() - start} ms`);
  });
  next();
});

function checkApiKey(req, res, next) {
  if (req.headers['x-api-key'] !== 'secret123') {
    return res.status(401).json({ error: 'Invalid API key' });
  }
  next();
}
app.use('/api/admin', checkApiKey);
```

4. The request hangs: the client waits until it times out.

</details>

---

## Part 2: Built-in Middleware

```js
app.use(express.json());                          // parses JSON bodies → req.body (REQUIRED for APIs)
app.use(express.urlencoded({ extended: true }));  // parses HTML form posts (x-www-form-urlencoded)
app.use(express.static('public'));                // serves files from ./public at /
```

- `express.json()` — without it, `req.body` is `undefined` on POST/PUT/PATCH. Client must send `Content-Type: application/json`.
- `express.static()` + SPA fallback:

```js
app.use(express.static('dist'));
app.get('*', (req, res) => res.sendFile(path.join(__dirname, 'dist', 'index.html')));
```

---

## Part 3: Third-Party Middleware

```bash
npm install morgan cors helmet
```

```js
const morgan = require('morgan');
const cors = require('cors');
const helmet = require('helmet');

app.use(helmet());                       // security headers — first
app.use(cors({                           // cross-origin access
  origin: 'http://localhost:5173',       // restrict in production!
  methods: ['GET', 'POST'],
}));
app.use(morgan('dev'));                  // request logging — dev format
app.use(express.json());
```

| Middleware | Does | Notes |
|-----------|------|-------|
| `morgan` | Logs `METHOD URL STATUS - ms` | `'dev'`/`'tiny'` dev, `'combined'` prod; never log secrets |
| `cors` | Adds CORS headers | `app.use(cors())` allows **all** origins — fine for dev, restrict in prod |
| `helmet` | Sets ~15 security headers (CSP, X-Content-Type-Options…) | Apply early; always in production |

---

### ✍️ Your Turn: Parts 2 & 3

Run `npm install morgan cors helmet` first.

```js
const express = require('express');
const ____ = require('morgan');
const cors = require('____');
const helmet = require('helmet');

const app = express();

// 1. Put them in the correct order:
//    express.json(), helmet(), morgan('dev'), cors({ origin: ... })
app.use(____);
app.use(____);
app.use(____);
app.use(____);
// 2. In the cors(...) line above, allow ONLY the React app: http://localhost:5173

// 3. Serve the files in the "public" folder
app.use(express.____('public'));
// TODO: create public/index.html with <h1>Hello from static files</h1>
//       and open http://localhost:3000/

// 4. Read HTML form posts: <form method="POST" action="/contact">
app.use(express.____({ extended: true }));
app.post('/contact', (req, res) => res.send(`Thanks, ${req.body.name}!`));
// TODO: add a form with an input name="name" to public/index.html and submit it
```

<details>
<summary>✅ Solution</summary>

```js
const express = require('express');
const morgan = require('morgan');
const cors = require('cors');
const helmet = require('helmet');

const app = express();

app.use(helmet());
app.use(cors({ origin: 'http://localhost:5173' }));
app.use(morgan('dev'));
app.use(express.json());

app.use(express.static('public'));

app.use(express.urlencoded({ extended: true }));
app.post('/contact', (req, res) => res.send(`Thanks, ${req.body.name}!`));
```

```html
<!-- public/index.html -->
<h1>Hello from static files</h1>
<form method="POST" action="/contact">
  <input name="name" placeholder="Your name" />
  <button type="submit">Send</button>
</form>
```

</details>

---

## Part 4: Custom Middleware

```js
// Attaching data to the request
const requestTime = (req, res, next) => {
  req.requestTime = new Date().toISOString();
  next();
};
app.use(requestTime);

// Validation
function validateTodo(req, res, next) {
  if (!req.body.text?.trim()) {
    return res.status(400).json({ error: 'text is required' });
  }
  next();
}
app.post('/api/todos', validateTodo, (req, res) => { /* create */ });
```

### Error-handling middleware — signature is `(err, req, res, next)`

4 parameters = error handler. **Register it LAST**, after all routes:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
});
```

---

### ✍️ Your Turn: Part 4

```js
// 1. Validation middleware for books
function validateBook(req, res, next) {
  const { title, pages } = req.body;
  // TODO: title missing or empty → 400 { error: 'title is required' }
  // TODO: pages was sent but is not a positive number → 400 { error: 'pages must be a positive number' }
  ____();
}
app.post('/api/books', ____, (req, res) => {
  res.status(201).json({ id: Date.now(), ...req.body });
});

// 2. A route that fails on purpose
app.get('/api/crash', (req, res, next) => {
  const err = new Error('Something broke!');
  err.status = 500;
  ____(err);         // send the error to the error handler
});

// 3. 404 handler for unknown routes (after all routes)
app.use((req, res) => {
  res.status(____).json({ error: `Route ${req.originalUrl} not found` });
});

// 4. Error handler: 4 parameters, registered LAST
app.use((____, req, res, next) => {
  console.error(err.message);
  res.status(err.status || ____).json({ error: err.message || 'Internal server error' });
});
```

<details>
<summary>✅ Solution</summary>

```js
function validateBook(req, res, next) {
  const { title, pages } = req.body;
  if (!title || !title.trim()) {
    return res.status(400).json({ error: 'title is required' });
  }
  if (pages !== undefined && (typeof pages !== 'number' || pages <= 0)) {
    return res.status(400).json({ error: 'pages must be a positive number' });
  }
  next();
}
app.post('/api/books', validateBook, (req, res) => {
  res.status(201).json({ id: Date.now(), ...req.body });
});

app.get('/api/crash', (req, res, next) => {
  const err = new Error('Something broke!');
  err.status = 500;
  next(err);
});

app.use((req, res) => {
  res.status(404).json({ error: `Route ${req.originalUrl} not found` });
});

app.use((err, req, res, next) => {
  console.error(err.message);
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
});
```

</details>

---

## Part 5: MongoDB Fundamentals

### NoSQL vs SQL

| SQL | MongoDB (NoSQL) |
|-----|-----------------|
| Tables with fixed schema | Collections of flexible documents |
| Rows | Documents (BSON — JSON-like) |
| Joins/normalization | Embedded docs or references |
| Vertical scaling | Horizontal scaling |

Documents map 1:1 to JavaScript objects — natural fit for Node:

```js
{ _id: ObjectId("..."), name: "Alice", age: 25, tags: ["developer", "node"] }
```

**Design principle:** embed data that's read together (post + comments), reference data that's shared or updated often (users). Design around your **query patterns**, not normalization rules. `_id` is auto-generated; keep docs under 16MB.

### Setup options

- **Local:** install MongoDB Community Server → `mongod` → `mongodb://localhost:27017`
- **Atlas (recommended):** free managed cluster → connection string `mongodb+srv://user:pass@cluster0.mongodb.net/mydb` → whitelist your IP → put the URI in `.env` (`MONGODB_URI=`)

**Tools:** `mongosh` (CLI — `use mydb`, `db.users.find()`) and **Compass** (GUI for browsing/querying visually). Learn both.

---

## Part 6: CRUD in mongosh

```js
use shopdb;

// CREATE
db.products.insertOne({ name: 'Laptop', price: 999, tags: ['electronics'] });
db.products.insertMany([{ name: 'Book', price: 15 }, { name: 'Mouse', price: 25 }]);

// READ — find(filter, projection)
db.products.find();                                        // all
db.products.find({ category: 'electronics' });             // equality
db.products.find({ price: { $gt: 50, $lt: 500 } });        // range
db.products.find({ category: { $in: ['books', 'games'] } });// match any
db.products.find({ $and: [ { price: { $gt: 20 } }, { stock: { $gt: 0 } } ] });
db.products.find({ $or:  [ { price: { $lt: 10 } }, { onSale: true } ] });

// projection + sort + limit + skip
db.products.find(
  { category: 'electronics' },
  { name: 1, price: 1, _id: 0 }                            // 1 = include, 0 = exclude
).sort({ price: -1 }).skip(10).limit(5);                   // descending, page 2

// UPDATE — always use $set for partial updates (no $set = REPLACE whole doc)
db.products.updateOne(
  { name: 'Mouse' },
  { $set: { price: 30 }, $push: { history: 'price updated' } }
);
db.products.updateMany({ category: 'books' }, { $set: { onSale: true } });

// DELETE — always filter by _id when possible
db.products.deleteOne({ name: 'Book' });
db.products.deleteMany({ discontinued: true });
```

**Common operator set:** `$gt $gte $lt $lte $eq $ne $in $nin $and $or $not $exists`

**Gotchas:**
- Forgetting `$set` in `updateOne` **replaces the entire document** — data loss
- `deleteOne({})`/`deleteOne()` deletes the first document it finds
- `find` returns a cursor — `.toArray()` in drivers, iterate in shell
- Use indexes on frequently-queried fields; never scan whole collections in production

---

### ✍️ Your Turn: Parts 5 & 6

Open `mongosh` (or the shell inside Compass) and run:

```js
use school;

// 1. Insert 5 students at once
db.students.____([
  { name: 'Ali',  age: 20, grade: 85, city: 'Amman', courses: ['math', 'english'] },
  { name: 'Mona', age: 22, grade: 42, city: 'Irbid', courses: ['art'] },
  { name: 'Omar', age: 19, grade: 67, city: 'Amman', courses: ['math'] },
  { name: 'Lina', age: 21, grade: 95, city: 'Zarqa', courses: ['science', 'math'] },
  { name: 'Sami', age: 23, grade: 55, city: 'Irbid', courses: [] },
]);

// 2. All students from Amman
db.students.find({ city: ____ });

// 3. Students with grade 60 or more
db.students.find({ grade: { ____: 60 } });

// 4. Students from Amman OR Zarqa
db.students.find({ city: { ____: ['Amman', 'Zarqa'] } });

// 5. Only name and grade (no _id), best grade first, top 3
db.students.find({}, { name: 1, grade: 1, _id: ____ }).sort({ grade: ____ }).limit(____);

// 6. Change ONLY Mona's grade to 52
db.students.updateOne({ name: 'Mona' }, { ____: { grade: 52 } });

// 7. TODO: add 'math' to Sami's courses (use $push)

// 8. TODO: delete all students with grade below 50

// 9. TODO: count the students who take 'math'
//    hint: db.students.countDocuments({ ... })
```

<details>
<summary>✅ Solution</summary>

```js
db.students.insertMany([ /* ... */ ]);

db.students.find({ city: 'Amman' });                                  // Ali, Omar
db.students.find({ grade: { $gte: 60 } });                            // Ali, Omar, Lina
db.students.find({ city: { $in: ['Amman', 'Zarqa'] } });              // Ali, Omar, Lina
db.students.find({}, { name: 1, grade: 1, _id: 0 }).sort({ grade: -1 }).limit(3);  // Lina, Ali, Omar

db.students.updateOne({ name: 'Mona' }, { $set: { grade: 52 } });
db.students.updateOne({ name: 'Sami' }, { $push: { courses: 'math' } });
db.students.deleteMany({ grade: { $lt: 50 } });                       // nobody left now (Mona has 52)
db.students.countDocuments({ courses: 'math' });                      // 4 (Ali, Omar, Lina, Sami)
```

Searching an array field with one value (`{ courses: 'math' }`) matches documents whose array **contains** that value.

</details>

---

## Lab

### Part A — Middleware-hardened to-do API

Extend the Session 8 lab:

```js
const app = express();
app.use(helmet());
app.use(cors());
app.use(morgan('dev'));
app.use(express.json());
app.use(requestTime);                       // custom: stamps req.requestTime

app.get('/api/todos', ...);
app.post('/api/todos', validateTodo, ...);  // 400 on empty text
// …PUT/PATCH/DELETE

app.use((req, res) => res.status(404).json({ error: 'Route not found' }));  // 404 handler
app.use((err, req, res, next) => { /* error handler */ });                  // last!
```

**Challenges:** apply `validateTodo` to PUT/PATCH; write `authCheck` middleware guarding DELETE; add a global error handler.

### Part B — MongoDB practice

```js
use shop;
db.products.insertMany([
  { name: 'Laptop', price: 999, category: 'electronics' },
  { name: 'Book', price: 15, category: 'books' },
  { name: 'Mouse', price: 25, category: 'electronics' },
  { name: 'Monitor', price: 200, category: 'electronics' },
]);

// Tasks:
// 1. products with price > 50
// 2. electronics under 300
// 3. name + price only, sorted by price desc, top 5
// 4. bump Mouse price to 30 with $set
// 5. delete the Book
```

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is middleware? What three things can it do?
2. Why does the order of `app.use(...)` matter?
3. What is the difference between global middleware and route middleware?
4. How does Express know that a middleware is an error handler?
5. What do `morgan`, `cors` and `helmet` do (one line each)?
6. What does `express.static('public')` do?
7. What is the difference between SQL tables and MongoDB collections?
8. What is a document? What is `_id`?
9. What does `{ price: { $gte: 10, $lte: 50 } }` find?
10. What is projection? Write one example.

<details>
<summary>✅ Solution</summary>

1. A function `(req, res, next)` that runs before the route handler. It can read or change `req`/`res`, end the request with a response, or call `next()` to continue.
2. Middleware runs in the order it was added. For example, `express.json()` must come before the routes that read `req.body`, and the error handler must be last.
3. Global middleware (`app.use(fn)`) runs for every request. Route middleware runs only for one path or route: `app.post('/x', fn, handler)`.
4. It has **4 parameters**: `(err, req, res, next)`.
5. `morgan` logs every request. `cors` lets other origins (like your React app) call the API. `helmet` adds security headers.
6. It serves the files in the `public` folder directly (HTML, CSS, images).
7. Tables have a fixed schema with rows. Collections hold flexible JSON-like documents that can have different fields.
8. A document is one record (like a JS object). `_id` is its unique id, which MongoDB creates automatically.
9. Documents whose price is between 10 and 50 (both included).
10. Choosing which fields to return: `db.users.find({}, { name: 1, email: 1, _id: 0 })`.

</details>

### B. Find the Bug

```js
// Bug 1: errors are never caught by the handler
app.use((err, req, res, next) => res.status(500).json({ error: err.message }));
app.get('/api/users', getUsers);

// Bug 2: every request hangs forever
app.use((req, res) => {
  console.log(req.method, req.url);
});

// Bug 3: this "error handler" never receives errors
app.use((err, req, res) => {
  res.status(500).json({ error: err.message });
});

// Bug 4 (mongosh): fails with "Update document requires atomic operators"
db.products.updateOne({ name: 'Mouse' }, { price: 30 });

// Bug 5 (mongosh): we wanted to delete the Book, but another document was deleted
db.products.deleteOne({});
```

<details>
<summary>✅ Solution</summary>

```js
// Bug 1: the error handler must be registered AFTER the routes
app.get('/api/users', getUsers);
app.use((err, req, res, next) => res.status(500).json({ error: err.message }));

// Bug 2: call next()
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});

// Bug 3: an error handler needs exactly 4 parameters
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});

// Bug 4: use $set to change one field
db.products.updateOne({ name: 'Mouse' }, { $set: { price: 30 } });

// Bug 5: an empty filter matches the first document, so always filter
db.products.deleteOne({ name: 'Book' });
```

</details>

### C. Coding Tasks

**Task 1: Request counter (easy).** A middleware that counts all requests. `GET /api/stats` returns `{ totalRequests: 12 }`.

**Task 2: Maintenance mode (easy).** If `process.env.MAINTENANCE === 'true'`, every request gets `503 { error: 'We are under maintenance' }`. Test it with `MAINTENANCE=true npm run dev` (on PowerShell: `$env:MAINTENANCE='true'; npm run dev`).

**Task 3: Role middleware (medium).** Write `requireRole(role)`, which returns a middleware. It reads the header `x-role` and returns 403 `{ error: 'Forbidden' }` if it doesn't match. Use it: `app.delete('/api/books/:id', requireRole('admin'), ...)`.

**Task 4: Divide with error handling (medium).** `GET /api/divide?a=10&b=0`. When `b` is 0, create an `Error`, set `err.status = 400` and call `next(err)`. The global error handler sends `{ error }` with the right status.

**Task 5: MongoDB queries (medium).** Insert these movies, then write the queries:

```js
db.movies.insertMany([
  { title: 'Inception', year: 2010, rating: 8.8, genres: ['action', 'sci-fi'] },
  { title: 'Up', year: 2009, rating: 8.3, genres: ['animation', 'family'] },
  { title: 'Interstellar', year: 2014, rating: 8.7, genres: ['sci-fi', 'drama'] },
  { title: 'Frozen', year: 2013, rating: 7.4, genres: ['animation', 'family'] },
  { title: 'The Dark Knight', year: 2008, rating: 9.0, genres: ['action', 'drama'] },
  { title: 'Coco', year: 2017, rating: 8.4, genres: ['animation'] },
]);
```

a) movies after 2010 · b) sci-fi movies · c) animation movies with rating ≥ 8 · d) only titles, sorted by rating (best first) · e) set `watched: true` on all action movies · f) delete movies with rating < 8 · g) count the movies left

**Challenge: Request logger to a file.** A middleware that adds one line per request to `logs.txt`: `2026-10-17T10:00:00.000Z GET /api/books 200 12ms`. Hint: `fs.appendFile` + `res.on('finish', ...)` + `res.statusCode`.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```js
// Task 1
let totalRequests = 0;
app.use((req, res, next) => {
  totalRequests++;
  next();
});
app.get('/api/stats', (req, res) => res.json({ totalRequests }));

// Task 2 (put it near the top, before the routes)
app.use((req, res, next) => {
  if (process.env.MAINTENANCE === 'true') {
    return res.status(503).json({ error: 'We are under maintenance' });
  }
  next();
});

// Task 3
function requireRole(role) {
  return (req, res, next) => {
    if (req.headers['x-role'] !== role) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
}
app.delete('/api/books/:id', requireRole('admin'), (req, res) => {
  res.status(204).send();
});

// Task 4
app.get('/api/divide', (req, res, next) => {
  const a = Number(req.query.a);
  const b = Number(req.query.b);
  if (b === 0) {
    const err = new Error('Cannot divide by zero');
    err.status = 400;
    return next(err);
  }
  res.json({ result: a / b });
});

app.use((err, req, res, next) => {
  res.status(err.status || 500).json({ error: err.message });
});

// Challenge
const fs = require('fs');
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    const line = `${new Date().toISOString()} ${req.method} ${req.originalUrl} ${res.statusCode} ${Date.now() - start}ms\n`;
    fs.appendFile('logs.txt', line, (err) => {
      if (err) console.error('Could not write log:', err.message);
    });
  });
  next();
});
```

```js
// Task 5 (mongosh)
db.movies.find({ year: { $gt: 2010 } });                              // Interstellar, Frozen, Coco
db.movies.find({ genres: 'sci-fi' });                                 // Inception, Interstellar
db.movies.find({ genres: 'animation', rating: { $gte: 8 } });         // Up, Coco
db.movies.find({}, { title: 1, _id: 0 }).sort({ rating: -1 });
db.movies.updateMany({ genres: 'action' }, { $set: { watched: true } });
db.movies.deleteMany({ rating: { $lt: 8 } });                         // deletes Frozen
db.movies.countDocuments();                                           // 5
```

</details>

---

## Review Questions

1. What happens if middleware forgets `next()` and doesn't send a response?
2. Why does middleware registration order matter? Give a broken example.
3. What distinguishes error-handling middleware?
4. `express.json()` vs `express.urlencoded()` — which requests does each parse?
5. Why put `helmet()` and `cors()` before `express.json()`?
6. Documents vs collections vs databases — hierarchy?
7. What does `{ price: { $gt: 50 } }` mean? Write "price between 20 and 100 OR category books".
8. What happens without `$set` in `updateOne`?
9. Embed vs reference — when does each fit?
10. What do projection, `sort`, `skip`, `limit` do?

## Common Mistakes

- Middleware after routes / error handler before routes
- `cors()` wide open in production
- Missing `extended: true` on urlencoded when sending nested form data
- `updateOne` without `$set` → document replaced
- Vague delete filters (`deleteOne({})`)
- Treating MongoDB relationally — over-normalizing
- Hardcoding the Atlas URI instead of `.env`

---

**Session 9 complete.** Next: Session 10 — Mongoose & REST API CRUD.
