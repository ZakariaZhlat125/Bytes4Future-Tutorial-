# Session 12: Advanced Mongoose, Security, Testing & Deployment

**Duration:** 3 hours
**Level:** Advanced
**Prerequisites:** Sessions 8–11 (Express, MongoDB, Mongoose CRUD, auth)
**Condensed from:** Node Sessions 9–10 — final session of the track

---

> **How to use this session**
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> Keep building the **Library API** (Sessions 10–11) and add a small blog (`Post`) to practice relationships.

## Session Overview

- **Part 1:** Relationships — embed vs reference, `populate`
- **Part 2:** Advanced queries — pagination, search, indexes, aggregation
- **Part 3:** Security hardening — helmet, rate limiting, sanitization, CORS, secrets
- **Part 4:** Testing — Jest + Supertest
- **Part 5:** Deployment — platform setup, Atlas prod, checklist
- **Capstone:** Build & deploy a complete REST API

---

## Part 1: Relationships in MongoDB

Two modeling choices — pick based on access patterns:

| **Embed** (subdocuments) | **Reference** (`ObjectId` + `ref`) |
|--------------------------|-------------------------------------|
| Data read together, rarely changes | Shared data, updated independently |
| e.g., post + comments, user + addresses | e.g., post → author, order → user |
| One query gets everything | Needs `populate()`; avoids duplication |
| Watch 16MB doc limit | Many-to-one / many-to-many friendly |

```js
// Embedding
const postSchema = new mongoose.Schema({
  title: String,
  comments: [{ text: String, author: String }],
});

// Referencing
const postSchema = new mongoose.Schema({
  title: { type: String, required: true },
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  tags: [String],
});
```

### `populate()` — the MongoDB "join"

```js
// replaces the ObjectId with the actual document
const posts = await Post.find({ published: true })
  .populate('author', 'name email')      // ALWAYS limit fields
  .sort({ createdAt: -1 });

// nested: populate a ref inside an embedded array
.populate('comments.user', 'name')
```

**Rules:** `ref` must be set in the schema; handle `null` refs (deleted users); avoid deep populate chains; `.lean()` for read-only queries (returns plain objects, skips Mongoose overhead — faster).

---

### ✍️ Your Turn: Part 1

```js
// models/Post.js
const postSchema = new mongoose.Schema(
  {
    title: { type: String, required: true },
    author: { type: mongoose.Schema.Types.____, ref: '____', required: true },   // reference
    comments: [{ text: String, name: String }],                                  // embedded
  },
  { timestamps: true }
);
module.exports = mongoose.model('Post', postSchema);
```

```js
// Try these in a script or a controller:

// 1. Create a post for an existing user
const user = await User.findOne({ email: 'sara@test.com' });
const post = await Post.create({ title: 'My first post', author: user.____ });

// 2. Get the posts WITH the author's name and email (NOT the password!)
const posts = await Post.find().____('author', '____');

// 3. Add a comment (embedded) to the post
await Post.findByIdAndUpdate(post._id, { ____: { comments: { text: 'Nice!', name: 'Ali' } } });

// 4. Embed or reference?
//    a) a user's addresses (max 3, always shown with the user)   → ____
//    b) the customer of an order (one customer, many orders)     → ____
//    c) the steps of a recipe                                    → ____
//    d) a product's category (shared by many products)           → ____
```

<details>
<summary>✅ Solution</summary>

```js
author: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },

const post = await Post.create({ title: 'My first post', author: user._id });

const posts = await Post.find().populate('author', 'name email');
// posts[0].author → { _id, name, email } instead of just the id

await Post.findByIdAndUpdate(post._id, { $push: { comments: { text: 'Nice!', name: 'Ali' } } });
```

4. a) embed · b) reference · c) embed · d) reference

</details>

---

## Part 2: Advanced Queries

### Pagination + filtering + search + sort — the standard list endpoint

```js
exports.getPosts = async (req, res, next) => {
  try {
    const { page = 1, limit = 10, search, sort = 'createdAt' } = req.query;
    const filter = search ? { title: { $regex: search, $options: 'i' } } : {};

    const posts = await Post.find(filter)
      .populate('author', 'name email')
      .sort({ [sort]: -1 })
      .skip((page - 1) * Number(limit))
      .limit(Math.min(Number(limit), 50));      // cap the limit

    const total = await Post.countDocuments(filter);
    res.json({ data: posts, meta: { total, page: Number(page), totalPages: Math.ceil(total / limit) } });
  } catch (err) { next(err); }
};
```

Query params arrive as strings — `Number()` them. For huge datasets prefer cursor pagination (`_id > lastSeenId`) over `skip` (which scans past skipped docs).

### Indexes

```js
userSchema.index({ email: 1 });                          // single
postSchema.index({ author: 1, createdAt: -1 });          // compound — match query shape
postSchema.index({ title: 'text', content: 'text' });    // text search index
```

- Index fields you filter/sort on — a query without an index is a full collection scan
- Every index slows writes — index deliberately, not everywhere
- `unique: true` creates an index automatically
- Verify with `Model.find(q).explain('executionStats')` — look for `COLLSCAN` (bad) vs `IXSCAN` (index used)

### Aggregation pipeline

Documents flow through stages; each stage transforms the stream:

```js
// Total revenue by order status
const stats = await Order.aggregate([
  { $match: { status: 'delivered' } },                       // filter FIRST (uses index)
  { $group: { _id: '$category', total: { $sum: '$total' } } },// group + accumulate
  { $sort: { total: -1 } },
  { $limit: 10 },
]);

// Posts per author
Post.aggregate([
  { $group: { _id: '$author', count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $lookup: { from: 'users', localField: '_id', foreignField: '_id', as: 'author' } }, // join
]);
```

**Stage cheat sheet:** `$match` filter · `$group` group+accumulate (`$sum $avg $min $max $push`) · `$sort` · `$project` shape fields · `$lookup` join · `$unwind` flatten arrays · `$skip`/`$limit` paginate. Field references need `$` (`'$total'`), stage names too (`$group`).

---

### ✍️ Your Turn: Part 2

```js
// 1. Pagination + search + total count
exports.getPosts = async (req, res, next) => {
  try {
    const page = Math.max(Number(req.query.page) || 1, 1);
    const limit = Math.min(Number(req.query.limit) || 10, ____);     // never more than 50
    const filter = req.query.search
      ? { title: { $regex: req.query.search, $options: '____' } }    // ignore upper/lower case
      : {};

    const [posts, total] = await Promise.all([
      Post.find(filter).sort({ createdAt: -1 }).____((page - 1) * limit).____(limit),
      Post.____(filter),
    ]);

    res.json({ data: posts, meta: { total, page, totalPages: Math.____(total / limit) } });
  } catch (err) { next(err); }
};

// 2. Index for "the posts of one author, newest first"
postSchema.index({ author: ____, createdAt: ____ });

// 3. Aggregation: number of posts per author, the most first
const stats = await Post.aggregate([
  { $____: { _id: '$author', count: { $sum: ____ } } },
  { $sort: { count: ____ } },
]);

// 4. TODO: orders look like { status: 'delivered', total: 35 }.
//    Write an aggregation: the total money per status,
//    counting only orders with total > 20.
```

<details>
<summary>✅ Solution</summary>

```js
const limit = Math.min(Number(req.query.limit) || 10, 50);
const filter = req.query.search ? { title: { $regex: req.query.search, $options: 'i' } } : {};

const [posts, total] = await Promise.all([
  Post.find(filter).sort({ createdAt: -1 }).skip((page - 1) * limit).limit(limit),
  Post.countDocuments(filter),
]);
res.json({ data: posts, meta: { total, page, totalPages: Math.ceil(total / limit) } });

postSchema.index({ author: 1, createdAt: -1 });

const stats = await Post.aggregate([
  { $group: { _id: '$author', count: { $sum: 1 } } },
  { $sort: { count: -1 } },
]);

// 4
const moneyPerStatus = await Order.aggregate([
  { $match: { total: { $gt: 20 } } },                       // filter first
  { $group: { _id: '$status', money: { $sum: '$total' } } },
  { $sort: { money: -1 } },
]);
```

</details>

---

## Part 3: Security Hardening

```bash
npm install helmet express-rate-limit express-mongo-sanitize
```

```js
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const mongoSanitize = require('express-mongo-sanitize');
const cors = require('cors');

app.use(helmet());                                          // ~15 security headers

app.use('/api', rateLimit({                                 // brute-force/abuse protection
  windowMs: 15 * 60 * 1000,
  max: 100,
}));
app.use('/api/auth', rateLimit({ windowMs: 15 * 60 * 1000, max: 10 })); // stricter on auth

app.use(mongoSanitize());                                   // strips $ . from req data → NoSQL injection
app.use(cors({ origin: process.env.CLIENT_URL }));          // never * in production
app.use(express.json({ limit: '10kb' }));                   // body-size cap
```

**Why each matters:**
- **NoSQL injection:** `{ "email": { "$gt": "" } }` matches *any* user — `mongoSanitize` removes `$` operators from input
- **Rate limiting:** stops credential-stuffing on `/login` — keep auth limits tight
- **CORS:** browser-enforced origin allowlist — configure your real frontend origin
- **Secrets:** `JWT_SECRET`, `MONGODB_URI` in env vars only; different values per environment; validate on startup:

```js
['PORT', 'MONGODB_URI', 'JWT_SECRET', 'CLIENT_URL'].forEach((key) => {
  if (!process.env[key]) throw new Error(`Missing env var: ${key}`);
});
```

---

### ✍️ Your Turn: Part 3

```js
const express = require('express');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const cors = require('cors');
const mongoose = require('mongoose');

const app = express();

// 1. Security headers
app.use(____());

// 2. At most 100 requests per 15 minutes for the whole API
app.use('/api', rateLimit({ windowMs: ____ * 60 * 1000, limit: ____ }));

// 3. At most 5 login attempts per 15 minutes
app.use('/api/auth/login', rateLimit({ windowMs: 15 * 60 * 1000, limit: ____ }));

// 4. Only our frontend may call the API
app.use(cors({ origin: process.env.____ }));

// 5. Reject request bodies bigger than 10kb
app.use(express.json({ ____: '10kb' }));

// 6. NoSQL-injection protection inside Mongoose:
//    objects with $-operators in a filter are treated as plain values
mongoose.set('____', true);

// 7. TODO (in server.js): stop the app at startup if MONGODB_URI or JWT_SECRET is missing

// 8. Attack test: send POST /api/auth/login with this body:
//    { "email": { "$gt": "" }, "password": "anything" }
//    What does User.findOne({ email }) return before and after step 6?
```

> `express-mongo-sanitize` (used above) works with **Express 4** only. On **Express 5**, use `mongoose.set('sanitizeFilter', true)` and validate input types (Joi / express-validator).

<details>
<summary>✅ Solution</summary>

```js
app.use(helmet());
app.use('/api', rateLimit({ windowMs: 15 * 60 * 1000, limit: 100 }));
app.use('/api/auth/login', rateLimit({ windowMs: 15 * 60 * 1000, limit: 5 }));
app.use(cors({ origin: process.env.CLIENT_URL }));
app.use(express.json({ limit: '10kb' }));
mongoose.set('sanitizeFilter', true);

// 7: server.js, right after require('dotenv').config()
['MONGODB_URI', 'JWT_SECRET'].forEach((key) => {
  if (!process.env[key]) {
    console.error(`Missing env var: ${key}`);
    process.exit(1);
  }
});
```

8. **Before:** `{ $gt: "" }` means "any email bigger than an empty string", so MongoDB returns the **first user** in the collection. bcrypt still blocks this login, but the same trick on a query with no password check (for example "find user by reset token") would leak data.
**After:** Mongoose searches for an email literally equal to `{ "$gt": "" }`, finds nothing, and the API returns 401. Request validation (email must be a string) also blocks it.

</details>

---

## Part 4: Testing with Jest + Supertest

```bash
npm install --save-dev jest supertest
```

```json
// package.json
{ "scripts": { "test": "jest --testEnvironment=node --runInBand" } }
```

**Why `app.js`/`server.js` split pays off:** tests import `app` without binding a port.

```js
// tests/todos.test.js
const request = require('supertest');
const mongoose = require('mongoose');
const app = require('../app');

beforeAll(async () => {
  await mongoose.connect(process.env.TEST_MONGODB_URI);      // separate test DB!
});
afterAll(async () => {
  await mongoose.connection.dropDatabase();
  await mongoose.connection.close();
});

describe('Todos API', () => {
  it('lists todos', async () => {
    const res = await request(app).get('/api/todos');
    expect(res.status).toBe(200);
    expect(Array.isArray(res.body)).toBe(true);
  });

  it('creates a todo', async () => {
    const res = await request(app).post('/api/todos').send({ text: 'Test' });
    expect(res.status).toBe(201);
    expect(res.body.text).toBe('Test');
  });

  it('rejects invalid input', async () => {
    const res = await request(app).post('/api/todos').send({});
    expect(res.status).toBe(400);
  });
});

describe('Auth', () => {
  it('rejects unauthenticated requests', async () => {
    const res = await request(app).get('/api/todos');
    expect(res.status).toBe(401);
  });
});
```

**Rules:** test happy + unhappy paths; separate test database; `beforeEach`/`afterEach` reset state; keep tests independent (no order dependence); always `await` requests.

---

### ✍️ Your Turn: Part 4

Add `TEST_MONGODB_URI=mongodb://127.0.0.1:27017/library_test` to `.env`. It must be a **different** database: the tests delete it at the end.

```js
// tests/books.test.js
require('dotenv').config();
const request = require('____');
const mongoose = require('mongoose');
const app = require('../____');          // the app WITHOUT app.listen

beforeAll(async () => {
  await mongoose.connect(process.env.____);
});

afterAll(async () => {
  await mongoose.connection.____();       // delete the test data
  await mongoose.connection.____();       // close the connection so Jest can exit
});

describe('Books API', () => {
  it('GET /api/books returns an array', async () => {
    const res = await request(app).____('/api/books');
    expect(res.____).toBe(200);
    expect(Array.isArray(res.body)).toBe(____);
  });

  it('POST /api/books without a token returns 401', async () => {
    // TODO
  });

  it('GET /api/books/abc returns 400 (invalid id)', async () => {
    // TODO
  });

  it('GET a valid id that does not exist returns 404', async () => {
    const fakeId = new mongoose.Types.ObjectId();
    // TODO
  });
});
```

Run with `npm test`.

<details>
<summary>✅ Solution</summary>

```js
require('dotenv').config();
const request = require('supertest');
const mongoose = require('mongoose');
const app = require('../app');

beforeAll(async () => {
  await mongoose.connect(process.env.TEST_MONGODB_URI);
});

afterAll(async () => {
  await mongoose.connection.dropDatabase();
  await mongoose.connection.close();
});

describe('Books API', () => {
  it('GET /api/books returns an array', async () => {
    const res = await request(app).get('/api/books');
    expect(res.status).toBe(200);
    expect(Array.isArray(res.body)).toBe(true);
  });

  it('POST /api/books without a token returns 401', async () => {
    const res = await request(app).post('/api/books').send({ title: 'X', author: 'Y' });
    expect(res.status).toBe(401);
  });

  it('GET /api/books/abc returns 400 (invalid id)', async () => {
    const res = await request(app).get('/api/books/abc');
    expect(res.status).toBe(400);
  });

  it('GET a valid id that does not exist returns 404', async () => {
    const fakeId = new mongoose.Types.ObjectId();
    const res = await request(app).get(`/api/books/${fakeId}`);
    expect(res.status).toBe(404);
  });
});
```

</details>

---

## Part 5: Deployment

### Flow (Render / Railway / Fly.io — same pattern)

1. Push to GitHub
2. New web service → connect repo → build `npm install`, start `node server.js`
3. Set env vars in the platform dashboard: `PORT` (usually auto), `MONGODB_URI` (Atlas prod DB), `JWT_SECRET`, `CLIENT_URL`, `NODE_ENV=production`
4. Atlas: production cluster → network access (allow your host's IPs or `0.0.0.0/0` for platforms without static IPs) → copy `mongodb+srv://` URI

### Deployment checklist

- [ ] `process.env.PORT` — never hardcode; `start` script = `node server.js`
- [ ] All required env vars set + validated at startup
- [ ] `.env` gitignored; `.env.example` committed
- [ ] helmet + restricted CORS + rate limiting + sanitization active
- [ ] Prod database ≠ dev database
- [ ] Error handler returns clean JSON, no stack traces (`NODE_ENV=production`)
- [ ] `npm ci` installs cleanly; devDeps not needed at runtime
- [ ] Health endpoint (`GET /api/health`) for platform monitoring
- [ ] Tests pass in CI before deploy

---

### ✍️ Your Turn: Part 5

Fill the blanks before you deploy:

```text
1. In package.json, "start" must be:       "____"           (not nodemon)
2. The port line:                          const PORT = ____ || 3000;
3. Env vars to set on the platform:        ____, ____, ____, NODE_ENV=____
4. Atlas → Network Access must allow:      ____              (the platform has no fixed IP)
5. A route the platform calls to check that the app is alive:   GET ____
```

```js
// 6. TODO: write GET /api/health
//    → 200 { status: 'ok', db: 'connected' }  when Mongo is connected
//    → 503 { status: 'error', db: 'disconnected' }  otherwise
//    hint: mongoose.connection.readyState === 1 means connected
```

<details>
<summary>✅ Solution</summary>

```text
1. "start": "node server.js"
2. const PORT = process.env.PORT || 3000;
3. MONGODB_URI, JWT_SECRET, CLIENT_URL, NODE_ENV=production
4. 0.0.0.0/0 (or the platform's IP list, if it gives one)
5. GET /api/health
```

```js
app.get('/api/health', (req, res) => {
  const connected = mongoose.connection.readyState === 1;
  res.status(connected ? 200 : 503).json({
    status: connected ? 'ok' : 'error',
    db: connected ? 'connected' : 'disconnected',
  });
});
```

</details>

---

## Capstone: Build & Deploy a Complete REST API

Pick one: **blog** (users, posts, comments), **e-commerce** (products, categories, orders), or **task manager** (projects, tasks, users).

### Requirements checklist

- [ ] CRUD on ≥2 related resources (`Post` ↔ `User` via `ref` + `populate`)
- [ ] `POST /auth/register` + `/auth/login` → JWT; bcrypt pre-save hook
- [ ] `protect` + `authorize('admin')` middleware on sensitive routes
- [ ] express-validator or Joi on write endpoints
- [ ] `AppError` + centralized error handler + 404 handler
- [ ] Pagination + search + sort on list endpoints
- [ ] At least one aggregation endpoint (`/api/stats`)
- [ ] Indexes on filtered/sorted fields
- [ ] Security stack (helmet, rate limit, mongoSanitize, CORS)
- [ ] Jest + Supertest suite covering CRUD + auth paths
- [ ] Deployed to a public URL with Atlas prod DB
- [ ] README: setup steps, env vars, endpoint docs, Postman collection

### Evaluation

Correct CRUD · secure auth · validation + error handling · clean MVC structure · tests · live deployment.

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. When do you embed data, and when do you use a reference?
2. What does `populate()` do? Why do we pass a list of fields to it?
3. What does `skip((page - 1) * limit).limit(limit)` do for page 3 with limit 10?
4. What is an index? What is the downside of adding too many?
5. In an aggregation, why do we put `$match` first?
6. What do `$group` and `$sum: 1` do together?
7. What problem does rate limiting solve? Why is the limit on `/login` stricter?
8. What is NoSQL injection? Give the classic example payload.
9. Why do tests need a separate database?
10. Name 4 things you must check before deploying.

<details>
<summary>✅ Solution</summary>

1. Embed data that is small and always read together with the parent (comments, addresses). Use a reference for data that is shared, large, or changed on its own (author, customer, category).
2. It replaces the stored ObjectId with the real document. The field list (`'name email'`) returns only what we need and never leaks fields like the password.
3. It skips the first 20 documents and returns documents 21–30.
4. A sorted lookup structure that makes searches and sorts fast. Every index makes writes slower and uses memory.
5. Fewer documents go to the next stages, so it's faster, and `$match` at the start can use an index.
6. They group the documents by a field and count how many are in each group.
7. It stops attackers from sending thousands of requests (for example guessing passwords). Login is where password guessing happens.
8. Sending MongoDB operators instead of plain values, so the query matches something it shouldn't: `{ "email": { "$gt": "" } }`.
9. Tests create and delete data (`dropDatabase`). On the real database they would destroy real data.
10. Any four of: env vars set · `start` uses `node` · port from `process.env.PORT` · CORS limited to the frontend · rate limit + helmet on · production DB is not the dev DB · no stack traces in responses · health endpoint · tests pass.

</details>

### B. Find the Bug

```js
// Bug 1: the API response shows every author's password hash
const posts = await Post.find().populate('author');

// Bug 2: page 1 shows the SECOND set of results
const posts = await Post.find().skip(page * limit).limit(limit);

// Bug 3: the result is one group with the total count of ALL posts
await Post.aggregate([{ $group: { _id: 'author', count: { $sum: 1 } } }]);

// Bug 4: after running the tests, all the development data is gone
beforeAll(() => mongoose.connect(process.env.MONGODB_URI));
afterAll(() => mongoose.connection.dropDatabase());

// Bug 5: Jest prints "Jest did not exit one second after the test run has completed"
afterAll(async () => {
  await mongoose.connection.dropDatabase();
});

// Bug 6: in production, any website can call our API
app.use(cors());
```

<details>
<summary>✅ Solution</summary>

```js
// Bug 1: choose the fields
const posts = await Post.find().populate('author', 'name email');

// Bug 2: pages start at 1
const posts = await Post.find().skip((page - 1) * limit).limit(limit);

// Bug 3: field names need a $ ('author' is just a fixed text, so everything is one group)
await Post.aggregate([{ $group: { _id: '$author', count: { $sum: 1 } } }]);

// Bug 4: use the TEST database
beforeAll(() => mongoose.connect(process.env.TEST_MONGODB_URI));

// Bug 5: close the connection
afterAll(async () => {
  await mongoose.connection.dropDatabase();
  await mongoose.connection.close();
});

// Bug 6: allow only your frontend
app.use(cors({ origin: process.env.CLIENT_URL }));
```

</details>

### C. Coding Tasks

**Task 1: Blog relationships (medium).** `Post` references `User` (author) and embeds `comments`. Endpoints:
- `POST /api/posts` (protected): the author comes from the token
- `GET /api/posts/:id`: returns the post with the author's `name` only
- `POST /api/posts/:id/comments` (protected): adds `{ text, name }` to the comments

**Task 2: List endpoint (medium).** `GET /api/books?search=code&genre=science&sort=pages&page=1&limit=5` returns `{ data, meta: { total, page, totalPages } }`. `sort` can be `pages` or `createdAt` (default `createdAt`, newest first).

**Task 3: Stats endpoint (medium).** `GET /api/stats/books-per-genre` returns, with one aggregation: `[{ genre: 'science', count: 4, avgPages: 320 }, ...]`, sorted by `count`.

**Task 4: Security pass (easy).** Add helmet, rate limiting (stricter on `/api/auth/login`), CORS limited to `CLIENT_URL`, a 10kb body limit, `sanitizeFilter`, and a startup env check. Then test: send the 6th login request within 15 minutes. What status code do you get?

**Task 5: Auth tests (medium).** Write 5 Supertest tests: register → 201 · register again → 400 · login with a wrong password → 401 · correct login returns a token · `GET /api/auth/me` with the token → 200 and the right email.

**Challenge: Deploy it.** Deploy the Library API to Render (or Railway) with an Atlas database. Requirements: `/api/health` works on the public URL, all env vars are set on the platform (none in the code), and the README has the live URL, setup steps and a list of endpoints.

<details>
<summary>✅ Solution: Tasks 1–5</summary>

```js
// Task 1: controllers/postController.js
exports.createPost = asyncHandler(async (req, res) => {
  const post = await Post.create({ title: req.body.title, author: req.user.userId });
  res.status(201).json(post);
});

exports.getPost = asyncHandler(async (req, res) => {
  const post = await Post.findById(req.params.id).populate('author', 'name');
  if (!post) throw new AppError('Post not found', 404);
  res.json(post);
});

exports.addComment = asyncHandler(async (req, res) => {
  const { text, name } = req.body;
  if (!text) throw new AppError('text is required', 400);
  const post = await Post.findByIdAndUpdate(
    req.params.id,
    { $push: { comments: { text, name } } },
    { new: true }
  );
  if (!post) throw new AppError('Post not found', 404);
  res.status(201).json(post);
});

// routes/posts.js
router.post('/', protect, createPost);
router.get('/:id', getPost);
router.post('/:id/comments', protect, addComment);
```

```js
// Task 2
exports.getBooks = asyncHandler(async (req, res) => {
  const page = Math.max(Number(req.query.page) || 1, 1);
  const limit = Math.min(Number(req.query.limit) || 10, 50);

  const filter = {};
  if (req.query.search) filter.title = { $regex: req.query.search, $options: 'i' };
  if (req.query.genre) filter.genre = req.query.genre;

  const sortField = req.query.sort === 'pages' ? 'pages' : 'createdAt';
  const sortOrder = sortField === 'pages' ? 1 : -1;

  const [data, total] = await Promise.all([
    Book.find(filter).sort({ [sortField]: sortOrder }).skip((page - 1) * limit).limit(limit),
    Book.countDocuments(filter),
  ]);

  res.json({ data, meta: { total, page, totalPages: Math.ceil(total / limit) } });
});
```

```js
// Task 3
exports.booksPerGenre = asyncHandler(async (req, res) => {
  const stats = await Book.aggregate([
    { $group: { _id: '$genre', count: { $sum: 1 }, avgPages: { $avg: '$pages' } } },
    { $sort: { count: -1 } },
    { $project: { _id: 0, genre: '$_id', count: 1, avgPages: { $round: ['$avgPages', 0] } } },
  ]);
  res.json(stats);
});
// routes/stats.js → router.get('/books-per-genre', booksPerGenre);
```

Task 4: use the solution of "Your Turn: Part 3". The 6th login attempt returns **429 Too Many Requests**.

```js
// Task 5: tests/auth.test.js
require('dotenv').config();
const request = require('supertest');
const mongoose = require('mongoose');
const app = require('../app');

beforeAll(async () => {
  await mongoose.connect(process.env.TEST_MONGODB_URI);
});

afterAll(async () => {
  await mongoose.connection.dropDatabase();
  await mongoose.connection.close();
});

// These tests build on each other (a flow), so they stay in one describe and run in order.
describe('Auth flow', () => {
  const user = { name: 'Test User', email: 'test@test.com', password: 'secret123' };
  let token;

  it('registers a new user', async () => {
    const res = await request(app).post('/api/auth/register').send(user);
    expect(res.status).toBe(201);
    expect(res.body.password).toBeUndefined();
  });

  it('rejects a duplicate email', async () => {
    const res = await request(app).post('/api/auth/register').send(user);
    expect(res.status).toBe(400);
  });

  it('rejects a wrong password', async () => {
    const res = await request(app).post('/api/auth/login').send({ email: user.email, password: 'wrong' });
    expect(res.status).toBe(401);
  });

  it('logs in and returns a token', async () => {
    const res = await request(app).post('/api/auth/login').send({ email: user.email, password: user.password });
    expect(res.status).toBe(200);
    expect(res.body.token).toBeDefined();
    token = res.body.token;
  });

  it('returns the current user with the token', async () => {
    const res = await request(app).get('/api/auth/me').set('Authorization', `Bearer ${token}`);
    expect(res.status).toBe(200);
    expect(res.body.email).toBe(user.email);
  });
});
```

</details>

---

## Review Questions

1. Embed vs reference — what decides?
2. Why always `.populate('author', 'name email')` instead of plain `.populate('author')`?
3. What does `skip`+`limit` cost on large collections? What's the alternative?
4. Why `$match` first in a pipeline?
5. What does `explain('executionStats')` tell you? `COLLSCAN` vs `IXSCAN`?
6. What does `express-mongo-sanitize` prevent? Show the attack payload.
7. Why stricter rate limits on `/api/auth/*`?
8. Why a separate test database? What breaks without it?
9. `app.js`/`server.js` split — how does it enable testing?
10. Walk through a deploy: code → platform → env vars → Atlas. What's forgotten most often?

## Common Mistakes

- Over-embedding (16MB limit) or orphaned refs (no cleanup on delete)
- `populate` without field selection — hauling whole user docs
- `skip` on millions of docs; regex search without a text index
- `COLLSCAN` queries in prod — no indexes
- Wide-open CORS, no rate limit on login, unsanitized operator input
- Stack traces in prod responses; missing `NODE_ENV`
- Tests hitting the dev DB; open DB connections hanging Jest
- Hardcoded `localhost:3000`, missing env vars, `npm start` running nodemon

---

## Additional Resources

- [Mongoose docs — populate/indexes/aggregation](https://mongoosejs.com/docs/)
- [MongoDB Atlas](https://www.mongodb.com/atlas) · [Express prod best practices](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Jest](https://jestjs.io/) · [Supertest](https://github.com/ladjs/supertest)

---

**Session 12 complete — and so is the track.** You now hold the full stack: TypeScript → React → Node/Express/MongoDB. The React frontend (Session 7) + this REST API = a production-ready full-stack application. Ship it.
