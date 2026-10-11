# Session 12: Advanced Mongoose, Security, Testing & Deployment

**Duration:** 3 hours
**Level:** Advanced
**Prerequisites:** Sessions 8–11 (Express, MongoDB, Mongoose CRUD, auth)
**Condensed from:** Node Sessions 9–10 — final session of the track

---

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
