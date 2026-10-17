# Session 10: Mongoose & REST API CRUD

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 9 (middleware, MongoDB shell CRUD)
**Condensed from:** Node Sessions 5–6

---

> **How to use this session**
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> The exercises build one project step by step: a **Library API** with a `Book` model.

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

### ✍️ Your Turn: Parts 1 & 2

Start the Library API: `npm init -y`, `npm install express mongoose dotenv`, `npm install -D nodemon`.

```bash
# .env
MONGODB_URI=____        # your Atlas string, or local: mongodb://127.0.0.1:27017/library
PORT=3000
```

```bash
# .gitignore: which two lines are required?
____
____
```

```js
// config/db.js
const mongoose = require('____');

async function connectDB() {
  try {
    await mongoose.____(process.env.____);
    console.log('MongoDB connected');
  } catch (err) {
    console.error('DB connection failed:', err.message);
    process.____(1);       // stop the app: it can't work without a database
  }
}

module.exports = connectDB;
```

```js
// server.js
require('____').config();          // must be the FIRST line
const app = require('./app');
const connectDB = require('./config/db');

async function start() {
  await ____();                     // connect BEFORE listening
  const PORT = process.env.PORT || 3000;
  app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
}

start();
```

**TODO:** create `.env.example` with the variable names but no values, and commit it.

> Use `127.0.0.1` instead of `localhost` for a local MongoDB. On newer Node versions, `localhost` may resolve to IPv6 and fail to connect.

<details>
<summary>✅ Solution</summary>

```bash
# .gitignore
node_modules/
.env
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
    process.exit(1);
  }
}

module.exports = connectDB;

// server.js
require('dotenv').config();
// ...
await connectDB();
```

```bash
# .env.example
MONGODB_URI=
PORT=
```

</details>

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

### ✍️ Your Turn: Part 3

```js
// models/Book.js
const mongoose = require('mongoose');

const bookSchema = new mongoose.____(
  {
    title:       { type: ____, required: [true, 'Title is required'], trim: true },
    author:      { type: String, required: ____ },
    pages:       { type: ____, min: [1, 'A book needs at least 1 page'] },
    genre:       { type: String, ____: ['novel', 'science', 'history', 'kids'] },
    isAvailable: { type: Boolean, ____: true },
    tags:        ____,                         // a list of strings
    isbn:        { type: String, unique: ____, sparse: true },   // sparse: many books may have no isbn
  },
  { ____: true }                               // adds createdAt and updatedAt
);

module.exports = mongoose.____('Book', bookSchema);   // collection name: "books"
```

Test the model with a small script, `scripts/try-book.js`, and run it with `node scripts/try-book.js`:

```js
require('dotenv').config();
const mongoose = require('mongoose');
const Book = require('../models/Book');

async function main() {
  await mongoose.connect(process.env.MONGODB_URI);
  try {
    // TODO 1: create a valid book with Book.create({...}) and print it
    // TODO 2: create a book with pages: 0. What error message do you get?
    // TODO 3: create a book with genre: 'sport'. What error message do you get?
  } catch (err) {
    console.log(err.name, '→', err.message);
  } finally {
    await mongoose.disconnect();
  }
}

main();
```

<details>
<summary>✅ Solution</summary>

```js
const bookSchema = new mongoose.Schema(
  {
    title:       { type: String, required: [true, 'Title is required'], trim: true },
    author:      { type: String, required: true },
    pages:       { type: Number, min: [1, 'A book needs at least 1 page'] },
    genre:       { type: String, enum: ['novel', 'science', 'history', 'kids'] },
    isAvailable: { type: Boolean, default: true },
    tags:        [String],
    isbn:        { type: String, unique: true, sparse: true },
  },
  { timestamps: true }
);

module.exports = mongoose.model('Book', bookSchema);
```

```js
const book = await Book.create({ title: 'Clean Code', author: 'Robert Martin', pages: 464, genre: 'science' });
console.log(book);   // has _id, isAvailable: true, createdAt, updatedAt

await Book.create({ title: 'Bad', author: 'X', pages: 0 });
// ValidationError → Book validation failed: pages: A book needs at least 1 page

await Book.create({ title: 'Bad', author: 'X', genre: 'sport' });
// ValidationError → Book validation failed: genre: `sport` is not a valid enum value for path `genre`.
```

Run each failing `create` on its own (comment out the others), because the first error jumps straight to `catch`.

</details>

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

### ✍️ Your Turn: Part 4

```js
// routes/books.js: only maps URLs to controller functions
const router = require('express').____();
const controller = require('../controllers/____');

router.get('/', controller.____);         // list
router.get('/:id', controller.____);      // one book
router.post('/', controller.____);        // create
router.patch('/:id', controller.____);    // update
router.delete('/:id', controller.____);   // delete

module.exports = router;
```

```js
// app.js
const express = require('express');
const app = express();

app.use(express.json());
app.use('____', require('./routes/books'));
// TODO: 404 handler
// TODO: error handler (last)

module.exports = app;
```

**Question:** where does each one go: **routes**, **controllers** or **models**?
a) the Book schema · b) `router.get('/:id', ...)` · c) `await Book.findById(id)` + `res.json(book)`

<details>
<summary>✅ Solution</summary>

```js
const router = require('express').Router();
const controller = require('../controllers/bookController');

router.get('/', controller.getBooks);
router.get('/:id', controller.getBookById);
router.post('/', controller.createBook);
router.patch('/:id', controller.updateBook);
router.delete('/:id', controller.deleteBook);

module.exports = router;
```

```js
app.use('/api/books', require('./routes/books'));
app.use((req, res) => res.status(404).json({ error: 'Route not found' }));
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
});
```

a) models · b) routes · c) controllers

</details>

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

### ✍️ Your Turn: Part 5

```js
// controllers/bookController.js
const Book = require('../models/Book');

// GET /api/books?genre=novel
exports.getBooks = async (req, res, next) => {
  try {
    const filter = req.query.genre ? { genre: req.query.genre } : {};
    const books = await Book.____(filter).sort({ createdAt: ____ });   // newest first
    res.json(books);
  } catch (err) { next(err); }
};

// GET /api/books/:id
exports.getBookById = async (req, res, next) => {
  try {
    const book = await Book.____(req.params.id);
    // TODO: 404 { error: 'Book not found' } if book is null
    res.json(book);
  } catch (err) { next(err); }
};

// POST /api/books
exports.createBook = async (req, res, next) => {
  try {
    const book = await Book.____(req.body);
    res.status(____).json(book);
  } catch (err) {
    if (err.name === '____') return res.status(400).json({ error: err.message });
    next(err);
  }
};

// PATCH /api/books/:id
exports.updateBook = async (req, res, next) => {
  try {
    const book = await Book.findByIdAndUpdate(req.params.id, req.body, {
      ____: true,          // return the UPDATED document
      ____: true,          // run the schema validation
    });
    // TODO: 404 if book is null
    res.json(book);
  } catch (err) { next(err); }
};

// DELETE /api/books/:id
exports.deleteBook = async (req, res, next) => {
  // TODO: findByIdAndDelete → 404 if null → 204 with no body
};
```

**Test in Postman:** create 3 books → list → `?genre=novel` → get one → patch its pages → delete it → get it again (404). Then try `GET /api/books/123`. What happens, and why?

<details>
<summary>✅ Solution</summary>

```js
exports.getBooks = async (req, res, next) => {
  try {
    const filter = req.query.genre ? { genre: req.query.genre } : {};
    const books = await Book.find(filter).sort({ createdAt: -1 });
    res.json(books);
  } catch (err) { next(err); }
};

exports.getBookById = async (req, res, next) => {
  try {
    const book = await Book.findById(req.params.id);
    if (!book) return res.status(404).json({ error: 'Book not found' });
    res.json(book);
  } catch (err) { next(err); }
};

exports.createBook = async (req, res, next) => {
  try {
    const book = await Book.create(req.body);
    res.status(201).json(book);
  } catch (err) {
    if (err.name === 'ValidationError') return res.status(400).json({ error: err.message });
    next(err);
  }
};

exports.updateBook = async (req, res, next) => {
  try {
    const book = await Book.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true,
    });
    if (!book) return res.status(404).json({ error: 'Book not found' });
    res.json(book);
  } catch (err) { next(err); }
};

exports.deleteBook = async (req, res, next) => {
  try {
    const book = await Book.findByIdAndDelete(req.params.id);
    if (!book) return res.status(404).json({ error: 'Book not found' });
    res.status(204).send();
  } catch (err) { next(err); }
};
```

`GET /api/books/123`: `123` is not a valid ObjectId, so Mongoose throws a **CastError**, which reaches the error handler as a 500. Better: return 400 for it in the error handler:

```js
app.use((err, req, res, next) => {
  if (err.name === 'CastError') return res.status(400).json({ error: 'Invalid id' });
  if (err.name === 'ValidationError') return res.status(400).json({ error: err.message });
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
});
```

</details>

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

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is Mongoose, and what does it add on top of MongoDB?
2. What is the difference between a schema and a model?
3. If the model is named `'Book'`, what is the collection called?
4. Why do we keep the connection string in `.env`? Why do we commit `.env.example`?
5. Why must `require('dotenv').config()` be the first line in `server.js`?
6. What does `{ timestamps: true }` add?
7. Name 4 schema options that validate data.
8. What do `new: true` and `runValidators: true` do in `findByIdAndUpdate`?
9. What does `findById` return when nothing is found?
10. Which status code for: created · deleted · validation error · not found?

<details>
<summary>✅ Solution</summary>

1. A library (ODM) that adds schemas, validation, defaults and an easy API for working with MongoDB documents.
2. A schema describes the shape and rules of a document. A model is made from the schema, and we use it to create and query documents.
3. `books` (lowercase and plural).
4. It holds secrets that must not go to GitHub. `.env.example` shows other developers which variables they need, without the values.
5. Other files read `process.env`. If dotenv loads later, those values are `undefined`.
6. `createdAt` and `updatedAt` fields, filled in automatically.
7. `required`, `min`/`max`, `minlength`/`maxlength`, `enum`, `match` (also `unique` as an index).
8. `new: true` returns the updated document (not the old one). `runValidators: true` checks the schema rules on update.
9. `null`.
10. 201 · 204 · 400 · 404.

</details>

### B. Find the Bug

```js
// Bug 1: the response is not the list of books
exports.getBooks = async (req, res) => {
  const books = Book.find();
  res.json(books);
};

// Bug 2: the response shows the OLD title after the update
const book = await Book.findByIdAndUpdate(req.params.id, req.body);
res.json(book);

// Bug 3: "The `uri` parameter to `openUri()` must be a string, got undefined"
// server.js
const app = require('./app');
const connectDB = require('./config/db');
connectDB();
require('dotenv').config();

// Bug 4: PATCH with { "pages": -5 } is saved without an error (schema has min: 1)
await Book.findByIdAndUpdate(req.params.id, req.body, { new: true });

// Bug 5: the team pushed .env to GitHub. What 2 things must you do now?
```

<details>
<summary>✅ Solution</summary>

```js
// Bug 1: missing await (and add try/catch)
const books = await Book.find();

// Bug 2: add { new: true }
const book = await Book.findByIdAndUpdate(req.params.id, req.body, { new: true });

// Bug 3: load .env FIRST
require('dotenv').config();
const app = require('./app');
const connectDB = require('./config/db');
connectDB();

// Bug 4: validators don't run on update unless you ask
await Book.findByIdAndUpdate(req.params.id, req.body, { new: true, runValidators: true });
```

Bug 5: (1) Change the database password / secrets right away, because they are public now. (2) Add `.env` to `.gitignore` and remove it from git: `git rm --cached .env`.

</details>

### C. Coding Tasks

**Task 1: Contact model (easy).** `models/Contact.js` with:
- `name`: String, required, trim
- `email`: String, required, unique, lowercase, must match `/\S+@\S+\.\S+/`
- `phone`: String
- `favorite`: Boolean, default `false`
- timestamps

**Task 2: Contacts CRUD API (medium).** MVC structure (`routes/contacts.js`, `controllers/contactController.js`) with all 5 endpoints at `/api/contacts` and the correct status codes (200/201/204/400/404).

**Task 3: Filter + pagination (medium).** `GET /api/contacts?favorite=true&page=2&limit=5` returns `{ data, total, page, totalPages }`.

**Task 4: Search (easy).** `GET /api/contacts?search=ali` finds contacts whose name contains the text (ignore case). Hint: `{ name: { $regex: search, $options: 'i' } }`.

**Task 5: Toggle favorite (easy).** `PATCH /api/contacts/:id/favorite` flips `favorite` from true to false or false to true and returns the contact.

**Challenge: Borrow and return books.** In the Library API add:
- `PATCH /api/books/:id/borrow`: if the book is not available → `409 { error: 'Book is already borrowed' }`, otherwise set `isAvailable: false`
- `PATCH /api/books/:id/return`: the opposite, with `409 { error: 'Book is not borrowed' }`
- `GET /api/books/available`: only available books (careful: this route must come **before** `/:id`)

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```js
// Task 1: models/Contact.js
const mongoose = require('mongoose');

const contactSchema = new mongoose.Schema(
  {
    name: { type: String, required: [true, 'Name is required'], trim: true },
    email: {
      type: String,
      required: [true, 'Email is required'],
      unique: true,
      lowercase: true,
      match: [/\S+@\S+\.\S+/, 'Email is invalid'],
    },
    phone: String,
    favorite: { type: Boolean, default: false },
  },
  { timestamps: true }
);

module.exports = mongoose.model('Contact', contactSchema);
```

```js
// Tasks 2–5: controllers/contactController.js
const Contact = require('../models/Contact');

exports.getContacts = async (req, res, next) => {
  try {
    const { favorite, search } = req.query;
    const page = Math.max(Number(req.query.page) || 1, 1);
    const limit = Math.min(Number(req.query.limit) || 10, 50);

    const filter = {};
    if (favorite !== undefined) filter.favorite = favorite === 'true';
    if (search) filter.name = { $regex: search, $options: 'i' };

    const [data, total] = await Promise.all([
      Contact.find(filter).sort({ name: 1 }).skip((page - 1) * limit).limit(limit),
      Contact.countDocuments(filter),
    ]);

    res.json({ data, total, page, totalPages: Math.ceil(total / limit) });
  } catch (err) { next(err); }
};

exports.getContactById = async (req, res, next) => {
  try {
    const contact = await Contact.findById(req.params.id);
    if (!contact) return res.status(404).json({ error: 'Contact not found' });
    res.json(contact);
  } catch (err) { next(err); }
};

exports.createContact = async (req, res, next) => {
  try {
    const contact = await Contact.create(req.body);
    res.status(201).json(contact);
  } catch (err) {
    if (err.name === 'ValidationError') return res.status(400).json({ error: err.message });
    if (err.code === 11000) return res.status(400).json({ error: 'Email already exists' });
    next(err);
  }
};

exports.updateContact = async (req, res, next) => {
  try {
    const contact = await Contact.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true,
    });
    if (!contact) return res.status(404).json({ error: 'Contact not found' });
    res.json(contact);
  } catch (err) { next(err); }
};

exports.deleteContact = async (req, res, next) => {
  try {
    const contact = await Contact.findByIdAndDelete(req.params.id);
    if (!contact) return res.status(404).json({ error: 'Contact not found' });
    res.status(204).send();
  } catch (err) { next(err); }
};

exports.toggleFavorite = async (req, res, next) => {
  try {
    const contact = await Contact.findById(req.params.id);
    if (!contact) return res.status(404).json({ error: 'Contact not found' });
    contact.favorite = !contact.favorite;
    await contact.save();
    res.json(contact);
  } catch (err) { next(err); }
};
```

```js
// routes/contacts.js
const router = require('express').Router();
const c = require('../controllers/contactController');

router.get('/', c.getContacts);
router.get('/:id', c.getContactById);
router.post('/', c.createContact);
router.patch('/:id/favorite', c.toggleFavorite);
router.patch('/:id', c.updateContact);
router.delete('/:id', c.deleteContact);

module.exports = router;
// app.js → app.use('/api/contacts', require('./routes/contacts'));
```

```js
// Challenge: add to bookController.js
exports.getAvailableBooks = async (req, res, next) => {
  try {
    res.json(await Book.find({ isAvailable: true }));
  } catch (err) { next(err); }
};

exports.borrowBook = async (req, res, next) => {
  try {
    const book = await Book.findById(req.params.id);
    if (!book) return res.status(404).json({ error: 'Book not found' });
    if (!book.isAvailable) return res.status(409).json({ error: 'Book is already borrowed' });
    book.isAvailable = false;
    await book.save();
    res.json(book);
  } catch (err) { next(err); }
};

exports.returnBook = async (req, res, next) => {
  try {
    const book = await Book.findById(req.params.id);
    if (!book) return res.status(404).json({ error: 'Book not found' });
    if (book.isAvailable) return res.status(409).json({ error: 'Book is not borrowed' });
    book.isAvailable = true;
    await book.save();
    res.json(book);
  } catch (err) { next(err); }
};

// routes/books.js: /available BEFORE /:id, otherwise "available" is treated as an id
router.get('/available', controller.getAvailableBooks);
router.get('/:id', controller.getBookById);
router.patch('/:id/borrow', controller.borrowBook);
router.patch('/:id/return', controller.returnBook);
```

</details>

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
