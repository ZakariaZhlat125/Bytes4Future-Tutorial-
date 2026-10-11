# Session 11: Validation, Error Handling & Authentication

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 10 (Mongoose models, CRUD controllers)
**Condensed from:** Node Sessions 7–8

---

## Session Overview

- **Part 1:** Schema-level validation — built-in + custom validators
- **Part 2:** Request-level validation — express-validator & Joi
- **Part 3:** Centralized error handling — error middleware, custom error classes, 404
- **Part 4:** Authentication — bcrypt hashing, register/login, JWT, protected routes, RBAC
- **Lab:** Auth-protected to-do API

---

## Part 1: Schema Validation (Last Line of Defense)

```js
const productSchema = new mongoose.Schema({
  name:     { type: String, required: [true, 'Name is required'], minlength: 2 },
  price:    { type: Number, required: true, min: [0, 'Price cannot be negative'] },
  sku:      { type: String, unique: true },
  category: { type: String, enum: ['electronics', 'books', 'clothing'] },
  email:    { type: String, lowercase: true, match: /\S+@\S+\.\S+/ },
  tags: {
    type: [String],
    validate: {                                        // custom validator
      validator: (v) => v.length <= 5,
      message: 'A product can have at most 5 tags',
    },
  },
});
```

- `min`/`max` → numbers & dates; `minlength`/`maxlength` → strings (don't confuse them)
- Custom `validate` — return `true`/`false`; use **regular functions** if you need `this` (arrow functions lose it)
- **Updates don't validate by default** — always pass `runValidators: true` to `findByIdAndUpdate`
- `unique` is an index, not a validator — handle the duplicate-key error (code 11000) yourself

---

## Part 2: Request Validation (First Line of Defense)

Validate at the boundary — before the data ever reaches your model.

### express-validator

```bash
npm install express-validator
```

```js
const { body, validationResult } = require('express-validator');

const createTodoRules = [
  body('text').notEmpty().trim().withMessage('text is required'),
  body('completed').optional().isBoolean(),
];

exports.createTodo = [
  ...createTodoRules,
  async (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // … create
  },
];
```

### Joi — schema-based validation (framework-agnostic)

```bash
npm install joi
```

```js
const Joi = require('joi');

const todoSchema = Joi.object({
  text: Joi.string().min(1).required(),
  completed: Joi.boolean().default(false),
});

exports.validateTodo = (req, res, next) => {
  const { error } = todoSchema.validate(req.body);
  if (error) return res.status(400).json({ error: error.details[0].message });
  next();
};

app.post('/api/todos', validateTodo, createTodo);
```

**Pick one, be consistent:** express-validator chains suit per-field rules; Joi suits validating whole payloads with reusable schema files.

---

## Part 3: Centralized Error Handling

### The error middleware — 4 params, registered LAST

```js
// middleware/errorHandler.js
module.exports = (err, req, res, next) => {
  const status = err.status || err.statusCode || 500;
  res.status(status).json({
    error: err.message || 'Internal Server Error',
    ...(process.env.NODE_ENV !== 'production' && { stack: err.stack }), // no stack leaks in prod
  });
};

// app.js — ORDER MATTERS:
app.use('/api/todos', todoRoutes);
app.use((req, res) => res.status(404).json({ error: 'Route not found' })); // unknown routes
app.use(errorHandler);                                                      // errors — always last
```

Controllers forward errors instead of handling them ad hoc:

```js
exports.getTodoById = async (req, res, next) => {
  try {
    const todo = await Todo.findById(req.params.id);
    if (!todo) return res.status(404).json({ error: 'Todo not found' });
    res.json(todo);
  } catch (err) {
    next(err);              // CastError etc. → centralized handler
  }
};
```

### Custom error classes — one `throw` replaces status+message plumbing

```js
// utils/AppError.js
class AppError extends Error {
  constructor(message, status = 500) {
    super(message);
    this.status = status;
    this.isOperational = true;      // expected error vs bug
  }
}

// usage in a controller — works beautifully with next(err)
if (!todo) return next(new AppError('Todo not found', 404));
if (existing) return next(new AppError('Email already registered', 400));
```

### Async + Express 4 caveat

Express 4 doesn't catch rejected promises — either wrap every async handler in `try/catch → next(err)`, or use a wrapper:

```js
const asyncHandler = (fn) => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next);

router.get('/:id', asyncHandler(controller.getTodoById));   // no try/catch needed
```

---

## Part 4: Authentication & Authorization

### Password hashing — bcrypt

Never store plain-text passwords. bcrypt = one-way hash + salt (defeats rainbow tables).

```bash
npm install bcrypt jsonwebtoken
```

```js
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 10;

const hash = await bcrypt.hash(password, SALT_ROUNDS);      // store this
const ok   = await bcrypt.compare(password, user.password); // verify at login
```

Best practice — hash in a Mongoose pre-save hook so no code path forgets it:

```js
userSchema.pre('save', async function (next) {
  if (!this.isModified('password')) return next();   // don't re-hash on unrelated updates
  this.password = await bcrypt.hash(this.password, 10);
  next();
});
```

### Register

```js
exports.register = async (req, res, next) => {
  try {
    const { email, password, name } = req.body;

    const existing = await User.findOne({ email });
    if (existing) return next(new AppError('Email already registered', 400));

    const user = await User.create({ name, email, password });  // pre-save hook hashes it
    res.status(201).json({ id: user._id, email: user.email });  // never return the hash
  } catch (err) { next(err); }
};
```

### Login → JWT

JWT = self-contained signed token (`header.payload.signature`). Client sends it as `Authorization: Bearer <token>` on every protected request — the server verifies the signature; no session storage needed.

```js
const jwt = require('jsonwebtoken');

exports.login = async (req, res, next) => {
  try {
    const { email, password } = req.body;
    const user = await User.findOne({ email });

    // Generic message — never reveal WHICH field was wrong
    if (!user || !(await bcrypt.compare(password, user.password))) {
      return next(new AppError('Invalid credentials', 401));
    }

    const token = jwt.sign(
      { userId: user._id, role: user.role },        // minimal payload — no secrets
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    res.json({ token });
  } catch (err) { next(err); }
};
```

### Protect middleware

```js
// middleware/auth.js
exports.protect = (req, res, next) => {
  const header = req.headers.authorization;
  if (!header?.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  try {
    req.user = jwt.verify(header.split(' ')[1], process.env.JWT_SECRET); // { userId, role }
    next();
  } catch {
    res.status(401).json({ error: 'Invalid or expired token' });
  }
};

// mount it
app.use('/api/todos', protect, todoRoutes);          // whole resource
router.delete('/:id', protect, deleteTodo);          // or per-route
```

### Role-based access control (RBAC)

**401** = not authenticated (who are you?). **403** = authenticated but not permitted (you can't do this).

```js
exports.authorize = (...roles) => (req, res, next) => {
  if (!roles.includes(req.user.role)) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  next();
};

router.delete('/users/:id', protect, authorize('admin'), deleteUser);
router.post('/products',    protect, authorize('admin', 'moderator'), createProduct);
```

---

## Lab: Auth-Protected To-Do API

```
1. models/User.js   → { name, email (unique), password (hashed via pre-save),
                        role: enum ['user','admin'] default 'user' }
2. controllers/authController.js → register, login (bcrypt.compare → jwt.sign)
3. middleware/auth.js → protect + authorize
4. routes/auth.js     → POST /api/auth/register, POST /api/auth/login
5. Protect /api/todos with protect; DELETE /api/todos/:id = admin only
6. All errors flow into the centralized handler via AppError
```

**Test flow in Postman:**
```
POST /api/auth/register  { name, email, password }  → 201
POST /api/auth/login     { email, password }        → { token }
GET  /api/todos          (no token)                  → 401
GET  /api/todos          Authorization: Bearer <t>  → 200
DELETE /api/todos/1      (role: user)                → 403
DELETE /api/todos/1      (role: admin)               → 204
```

**Challenges:** `GET /api/auth/me` returning the current user; refresh-token endpoint; password-strength validation via Joi.

---

## Review Questions

1. Why validate at BOTH request and schema level? What does each catch?
2. `min` vs `minlength` — which fields do they apply to?
3. Why must error middleware have 4 params and be registered last?
4. What does `next(err)` do vs `next()`?
5. Why does Express 4 need `try/catch` or `asyncHandler` around async controllers?
6. Why is bcrypt better than SHA-256 for passwords?
7. Why generic "Invalid credentials" instead of "wrong password"?
8. JWT: what's in the three segments? Why shouldn't the payload contain secrets?
9. 401 vs 403?
10. What does the pre-save hook's `isModified('password')` check prevent?

## Common Mistakes

- Plain-text passwords anywhere (DB, logs, responses)
- Revealing whether email or password failed
- JWT secret hardcoded or weak; sensitive data in payload; no `expiresIn`
- Checking `Bearer ` prefix incorrectly or forgetting it
- Error handler registered before routes; 404 handler before routes
- Forgetting `runValidators` on updates
- `bcrypt.compare` on a user that doesn't exist → crash on `user.password`
- `req.user` accessed before `protect` ran

---

**Session 11 complete.** Next: Session 12 — Advanced Mongoose, Security, Testing & Deployment.
