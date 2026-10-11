# Session 9: Express Middleware & MongoDB Fundamentals

**Duration:** 3 hours
**Level:** Beginner–Intermediate
**Prerequisites:** Session 8 (Express basics, routing, req/res)
**Condensed from:** Node Sessions 3–4

---

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
