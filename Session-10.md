# Session 10: Mongoose & REST API CRUD

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 9 (middleware, MongoDB shell CRUD)
**Condensed from:** Node Sessions 5–6

---

## Session Overview

- **Part 1:** Mongoose (ODM) — why it exists, install, connect
- **Part 2:** Environment variables with dotenv
- **Part 3:** Schemas, types, defaults, validation
- **Part 4:** MVC structure — models, controllers, routes
- **Part 5:** Full CRUD with correct status codes
- **Lab:** Convert the in-memory to-do API to MongoDB-backed

---

## Part 1: Mongoose & ODM

**Mongoose** is an ODM (Object Data Modeling) library — it maps JS objects to MongoDB documents and adds what raw MongoDB lacks: **schemas** (enforced shape), **validation**, instance/static methods, middleware hooks, and a cleaner API.

```bash
npm install mongoose dotenv
```

```js
// config/db.js
const mongoose = require('mongoose');

async function connectDB() {
  try {
    await mongoose.connect(process.env.MONGODB_URI);
    console.log('MongoDB connected');
  } catch (err) {
    console.error('DB connection failed:', err.message);
    process.exit(1);                     // don't run the server without a DB
  }
}
module.exports = connectDB;
```

```js
// server.js — connect BEFORE listening
require('dotenv').config();
const app = require('./app');
const connectDB = require('./config/db');

connectDB();
app.listen(process.env.PORT || 3000);
```

**Schema vs model:** a **schema** defines document shape; a **model** is the compiled class bound to a collection (`User` model → `users` collection). Models are what you query with.

---

## Part 2: Environment Variables

Secrets (DB URIs, API keys, JWT secrets) belong in `.env` — never in code, never in git.

```bash
# .env                        ← gitignored
MONGODB_URI=mongodb+srv://user:pass@cluster0.mongodb.net/mydb
PORT=3000
JWT_SECRET=change-me-in-prod

# .env.example                ← committed, documents required vars
MONGODB_URI=
PORT=
JWT_SECRET=
```

```js
require('dotenv').config();           // FIRST thing in the entry file
const uri = process.env.MONGODB_URI;
```

**Rules:** `dotenv.config()` before anything reads `process.env`; `.env` in `.gitignore`; `.env.example` documents the shape; separate dev/prod databases; rotate credentials.

---

## Part 3: Schemas — Types, Defaults, Validation

```js
// models/Product.js
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema(
  {
    name:     { type: String, required: [true, 'Name is required'], trim: true },
    price:    { type: Number, required: true, min: 0 },
    category: { type: String, enum: ['electronics', 'books', 'clothing'] },
    email:    { type: String, unique: true, lowercase: true },   // unique → index
    inStock:  { type: Boolean, default: true },
    tags:     [String],
    createdAt:{ type: Date, default: Date.now },
  },
  { timestamps: true }               // auto adds createdAt + updatedAt
);

module.exports = mongoose.model('Product', productSchema);   // singular name → 'products' collection
```

**Field options:** `type` (`String Number Boolean Date [Array] ObjectId Mixed`), `required` (value or `[true, 'msg']`), `default`, `min`/`max`, `minlength`/`maxlength`, `enum`, `unique`, `match` (regex), `trim`, `lowercase`.

**Gotchas:** model names are singular (Mongoose pluralizes the collection); define schemas before server start; schema validation is the *last* line — still validate inputs at the route level.

---

## Part 4: MVC Structure

Separate concerns so each layer has one job:

```
project/
├── app.js                 # app setup: middleware, routes, error handler — exports app
├── server.js              # dotenv + connectDB + app.listen only
├── config/db.js           # connection logic
├── models/                # schemas → Todo.js, User.js
├── controllers/           # request handlers (call models, send responses)
├── routes/                # URL → controller mapping only
├── middleware/            # auth, validation, errorHandler
└── .env / .env.example / .gitignore
```

```js
// routes/todos.js — thin: only maps routes to controllers
const router = require('express').Router();
const controller = require('../controllers/todoController');

router.get('/', controller.getTodos);
router.get('/:id', controller.getTodoById);
router.post('/', controller.createTodo);
router.put('/:id', controller.updateTodo);      // or PATCH
router.delete('/:id', controller.deleteTodo);

module.exports = router;

// app.js
app.use('/api/todos', require('./routes/todos'));
```

**Anti-patterns:** business logic inside route files; routes importing other routes; controllers that talk HTTP *and* do heavy logic (push real work to services/models as it grows).

---

## Part 5: CRUD with Mongoose

```js
// controllers/todoController.js
const Todo = require('../models/Todo');

// READ all — with filtering, sorting, pagination
exports.getTodos = async (req, res, next) => {
  try {
    const { completed, page = 1, limit = 10 } = req.query;
    const filter = completed !== undefined ? { completed: completed === 'true' } : {};
    const todos = await Todo.find(filter)
      .sort({ createdAt: -1 })
      .skip((page - 1) * limit)
      .limit(Number(limit));
    res.json(todos);
  } catch (err) { next(err); }
};

// READ one
exports.getTodoById = async (req, res, next) => {
  try {
    const todo = await Todo.findById(req.params.id);
    if (!todo) return res.status(404).json({ error: 'Todo not found' });
    res.json(todo);
  } catch (err) { next(err); }          // CastError on malformed ObjectId
};

// CREATE
exports.createTodo = async (req, res, next) => {
  try {
    const todo = await Todo.create(req.body);      // schema validation runs here
    res.status(201).json(todo);
  } catch (err) {
    if (err.name === 'ValidationError') return res.status(400).json({ error: err.message });
    next(err);
  }
};

// UPDATE — findByIdAndUpdate(id, data, options)
exports.updateTodo = async (req, res, next) => {
  try {
    const todo = await Todo.findByIdAndUpdate(req.params.id, req.body, {
      new: true,               // return the UPDATED doc (default returns pre-update)
      runValidators: true,     // run schema validation on update
    });
    if (!todo) return res.status(404).json({ error: 'Todo not found' });
    res.json(todo);
  } catch (err) { next(err); }
};

// DELETE
exports.deleteTodo = async (req, res, next) => {
  try {
    const todo = await Todo.findByIdAndDelete(req.params.id);
    if (!todo) return res.status(404).json({ error: 'Todo not found' });
    res.status(204).send();
  } catch (err) { next(err); }
};
```

### Model methods cheat sheet

| Task | Method |
|------|--------|
| Create | `Model.create(data)` · `new Model(data).save()` |
| Read all | `Model.find(filter)` |
| Read one | `Model.findById(id)` · `Model.findOne(filter)` |
| Update | `Model.findByIdAndUpdate(id, data, { new: true, runValidators: true })` |
| Delete | `Model.findByIdAndDelete(id)` |
| Chain | `.sort() .limit() .skip() .select('name price')` |

### HTTP status codes

| Code | When |
|------|------|
| 200 | Success (GET/PUT/PATCH) |
| 201 | Created (POST) |
| 204 | Success, no body (DELETE) |
| 400 | Validation error / bad request |
| 401/403 | Unauthenticated / unauthorized |
| 404 | Resource not found |
| 500 | Server/DB error |

---

## Lab: MongoDB-Backed To-Do API

Convert the Session 8/9 in-memory API to a real database:

```
1. npm install mongoose dotenv; add .env + .gitignore + .env.example
2. config/db.js       → connectDB() (connect, log, exit(1) on fail)
3. models/Todo.js     → { text: required String, completed: default false } + timestamps
4. controllers/todoController.js → all 5 CRUD handlers with try/catch + next(err)
5. routes/todos.js    → map verbs to controller
6. app.js             → helmet, cors, morgan, express.json, mount router, 404 + error handler
7. server.js          → dotenv, connectDB, listen
8. Test every endpoint in Postman: list, create, get, update, delete
```

**Challenges:**
- `GET /api/todos?completed=true&page=2&limit=5` — query-string filtering + pagination
- `runValidators: true` on update — try sending `{ text: "" }` and confirm 400
- `.select('text completed')` — projection via query params

---

## Review Questions

1. What does an ODM add over the raw MongoDB driver?
2. Schema vs model — what's the relationship?
3. Why `process.exit(1)` when the DB connection fails?
4. What do `{ new: true }` and `{ runValidators: true }` do on update?
5. Why does `.env` go in `.gitignore`? What's `.env.example` for?
6. Why keep controllers out of route files?
7. When is 404 correct vs 400?
8. How do you filter + paginate a `find()`?

## Common Mistakes

- Forgetting `await` — returning a pending query instead of data
- `findByIdAndUpdate` without `new: true` → returns the stale document
- Missing `runValidators` → updates bypass schema validation
- Not handling `null` from `findById` → crashes or wrong response
- Malformed ObjectId throws `CastError` → catch it (500/400), don't crash
- Hardcoding `MONGODB_URI` in source
- Listening before the DB is connected
- Schema validation as the *only* validation layer

---

**Session 10 complete.** Next: Session 11 — Validation, Error Handling & Authentication.
