# Session 11: Validation, Error Handling & Authentication

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 10 (Mongoose models, CRUD controllers)
**Condensed from:** Node Sessions 7–8

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> Keep building the **Library API** from Session 10. If `npm install bcrypt` fails on Windows, use `npm install bcryptjs` and `require('bcryptjs')` instead (same functions).

## Session Overview

- **Part 1:** Schema-level validation — built-in + custom validators
- **Part 2:** Request-level validation — express-validator & Joi
- **Part 3:** Centralized error handling — error middleware, custom error classes, 404
- **Part 4:** Authentication — bcrypt hashing, register/login, JWT, protected routes, RBAC
- **Lab:** Auth-protected to-do API

---

## Part 1: Schema Validation (Last Line of Defense)

> 💡 **In simple words:** Never trust data that comes from users. Schema validation is the final guard right before data enters the database: if a field breaks a rule, Mongoose refuses to save it.

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

### ✍️ Your Turn: Part 1

```js
// models/User.js
const userSchema = new mongoose.Schema({
  name:  { type: String, required: [true, '____'], ____: 2 },     // at least 2 characters
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    ____: [/\S+@\S+\.\S+/, 'Invalid email'],
  },
  age:   { type: Number, ____: [13, 'You must be at least 13'] },
  role:  { type: String, ____: ['user', 'admin'], default: 'user' },
  skills: {
    type: [String],
    validate: {
      validator: (v) => ____,              // at most 3 skills
      message: 'You can add at most 3 skills',
    },
  },
});

// Question: this rule never works. Why?
// name: { type: String, min: 2 }
```

<details>
<summary>✅ Solution</summary>

```js
const userSchema = new mongoose.Schema({
  name:  { type: String, required: [true, 'Name is required'], minlength: 2 },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: [/\S+@\S+\.\S+/, 'Invalid email'],
  },
  age:   { type: Number, min: [13, 'You must be at least 13'] },
  role:  { type: String, enum: ['user', 'admin'], default: 'user' },
  skills: {
    type: [String],
    validate: {
      validator: (v) => v.length <= 3,
      message: 'You can add at most 3 skills',
    },
  },
});
```

`min` is for **numbers and dates**. For strings, use `minlength`.

</details>

---

## Part 2: Request Validation (First Line of Defense)

> 💡 **In simple words:** Request validation checks the input as soon as the request arrives, before any database work. It gives clear error messages and stops bad data early. Think of it as the guard at the front door, while the schema is the guard at the safe.

Validate at the boundary — before the data ever reaches your model.

### express-validator

> 💡 **In simple words:**
> 1. Describe the rules for each field as a chain: `body('email').isEmail()`.
> 2. Put the rules before your controller.
> 3. Check `validationResult(req)` for errors.

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

> 💡 **In simple words:** With Joi, you describe the whole expected object in one schema and check `req.body` against it. You can reuse the same schema on many routes.

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

### ✍️ Your Turn: Part 2

```js
// A) express-validator
const { body, validationResult } = require('express-validator');

// 1. Rules for registering
const registerRules = [
  body('name').trim().____().withMessage('name is required'),
  body('email').____().withMessage('email is invalid'),
  body('password').isLength({ ____: 6 }).withMessage('password needs at least 6 characters'),
];

// 2. One reusable middleware that checks the result
function handleValidation(req, res, next) {
  const errors = ____(req);
  if (!errors.isEmpty()) {
    return res.status(____).json({ errors: errors.array() });
  }
  ____();
}

router.post('/register', registerRules, handleValidation, register);
```

```js
// B) The same rules with Joi
const Joi = require('joi');

const registerSchema = Joi.object({
  name: Joi.string().min(2).____(),
  email: Joi.string().____().required(),
  password: Joi.string().min(____).required(),
});

// A middleware factory: works with ANY Joi schema
function validate(schema) {
  return (req, res, next) => {
    const { error } = schema.____(req.body);
    // TODO: if error → 400 { error: error.details[0].message }
    // TODO: otherwise → next()
  };
}

router.post('/register', validate(registerSchema), register);

// C) TODO: write bookSchema with Joi: title (required), author (required),
//    pages (number, integer, min 1, optional), genre (one of: novel, science, history, kids)
```

<details>
<summary>✅ Solution</summary>

```js
const registerRules = [
  body('name').trim().notEmpty().withMessage('name is required'),
  body('email').isEmail().withMessage('email is invalid'),
  body('password').isLength({ min: 6 }).withMessage('password needs at least 6 characters'),
];

function handleValidation(req, res, next) {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  next();
}
```

```js
const registerSchema = Joi.object({
  name: Joi.string().min(2).required(),
  email: Joi.string().email().required(),
  password: Joi.string().min(6).required(),
});

function validate(schema) {
  return (req, res, next) => {
    const { error } = schema.validate(req.body);
    if (error) return res.status(400).json({ error: error.details[0].message });
    next();
  };
}

const bookSchema = Joi.object({
  title: Joi.string().required(),
  author: Joi.string().required(),
  pages: Joi.number().integer().min(1),
  genre: Joi.string().valid('novel', 'science', 'history', 'kids'),
});
```

</details>

---

## Part 3: Centralized Error Handling

### The error middleware — 4 params, registered LAST

> 💡 **In simple words:** Instead of writing error responses in every controller, send every error to **one** handler. One place decides the format and the status code, so errors are consistent and easy to change.

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

> 💡 **In simple words:** `AppError` is our own type of error that also carries an HTTP status. In a controller you just write `throw new AppError('Not found', 404)` (or pass it to `next(...)`), and the error handler does the rest.

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

> 💡 **In simple words:** In Express 4, an error inside an `async` function is not caught automatically, so the request hangs. `asyncHandler` wraps the function and passes any error to `next`. Express 5 does this for you.

Express 4 doesn't catch rejected promises — either wrap every async handler in `try/catch → next(err)`, or use a wrapper:

```js
const asyncHandler = (fn) => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next);

router.get('/:id', asyncHandler(controller.getTodoById));   // no try/catch needed
```

---

### ✍️ Your Turn: Part 3

```js
// utils/AppError.js
class AppError extends ____ {
  constructor(message, status) {
    ____(message);
    this.status = ____;
  }
}
module.exports = AppError;
```

```js
// utils/asyncHandler.js: passes any error to next(), so no try/catch is needed
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).____(next);

module.exports = asyncHandler;
```

```js
// controllers/bookController.js: rewrite getBookById with both helpers
exports.getBookById = asyncHandler(async (req, res) => {
  const book = await Book.findById(req.params.id);
  if (!book) throw new ____('Book not found', ____);
  res.json(book);
});
```

```js
// middleware/errorHandler.js
module.exports = (err, req, res, next) => {
  // TODO: CastError           → 400 { error: 'Invalid id' }
  // TODO: ValidationError     → 400 { error: err.message }
  // TODO: err.code === 11000  → 400 { error: 'Already exists' }   (duplicate unique value)
  // TODO: anything else       → err.status or 500, and err.message
};
```

<details>
<summary>✅ Solution</summary>

```js
class AppError extends Error {
  constructor(message, status) {
    super(message);
    this.status = status;
  }
}

const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

exports.getBookById = asyncHandler(async (req, res) => {
  const book = await Book.findById(req.params.id);
  if (!book) throw new AppError('Book not found', 404);
  res.json(book);
});

module.exports = (err, req, res, next) => {
  if (err.name === 'CastError') return res.status(400).json({ error: 'Invalid id' });
  if (err.name === 'ValidationError') return res.status(400).json({ error: err.message });
  if (err.code === 11000) return res.status(400).json({ error: 'Already exists' });
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
};
```

> **Express 5** already sends errors from `async` handlers to the error handler, so `asyncHandler` is only required on Express 4. It is still fine (and clear) to use it.

</details>

---

## Part 4: Authentication & Authorization

### Password hashing — bcrypt

> 💡 **In simple words:** **Hashing** turns a password into scrambled text that can't be turned back. At login, we hash what the user typed and compare it with the saved hash. A **salt** (random extra text) makes the same password produce different hashes, so attackers can't use ready-made lists of common passwords and their hashes.

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

> 💡 **In simple words:** Registering is three steps:
> 1. Check that the email isn't already taken.
> 2. Save the new user (the pre-save hook hashes the password).
> 3. Reply 201, **without** the password.

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

> 💡 **In simple words:** To log in, find the user by email, then compare the password with bcrypt. If it's correct, create a **JWT**: a signed token that contains the user's id and role. The signature, made with your secret key, proves that your server created the token and nobody changed it.

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

> 💡 **In simple words:** `protect` runs before the protected routes. It reads the token from the `Authorization` header, verifies it, and puts the user's info in `req.user` for the next functions. No valid token → 401.

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

> 💡 **In simple words:** With role-based access control, permissions depend on the user's **role**. `authorize('admin')` runs **after** `protect` and only lets admins continue; everyone else gets 403.

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

### ✍️ Your Turn: Part 4

Add `JWT_SECRET=some-long-random-text` to `.env` first.

```js
// models/User.js: 1. Hash the password automatically before saving
userSchema.pre('save', async function () {        // a normal function, NOT an arrow (we need `this`)
  if (!this.____('password')) return;             // only when the password changed
  this.password = await bcrypt.____(this.password, 10);
});
```

```js
// controllers/authController.js
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const AppError = require('../utils/AppError');
const asyncHandler = require('../utils/asyncHandler');

// 2. Register
exports.register = asyncHandler(async (req, res) => {
  const { name, email, password } = req.body;
  const exists = await User.____({ email });
  if (exists) throw new AppError('Email already registered', ____);

  const user = await User.create({ name, email, password });
  res.status(____).json({ id: user._id, name: user.name, email: user.email });  // never send the password
});

// 3. Login
exports.login = asyncHandler(async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  const ok = user && (await bcrypt.____(password, user.password));
  if (!ok) throw new AppError('____', 401);   // the SAME message for a wrong email or a wrong password

  const token = jwt.____(
    { userId: user._id, role: user.role },
    process.env.____,
    { expiresIn: '1d' }
  );
  res.json({ token });
});
```

```js
// middleware/auth.js
// 4. protect: only logged-in users
exports.protect = (req, res, next) => {
  const header = req.headers.____;
  if (!header || !header.startsWith('____ ')) {
    return res.status(401).json({ error: 'Not logged in' });
  }
  try {
    req.user = jwt.____(header.split(' ')[1], process.env.JWT_SECRET);
    next();
  } catch {
    res.status(401).json({ error: 'Invalid or expired token' });
  }
};

// 5. authorize: only some roles
exports.authorize = (...roles) => (req, res, next) => {
  if (!roles.includes(req.user.____)) {
    return res.status(____).json({ error: 'Forbidden' });
  }
  next();
};

// 6. TODO: GET /api/auth/me (protected) → the logged-in user WITHOUT the password
//    hint: User.findById(req.user.userId).select('-password')

// 7. TODO: protect the book routes: anyone can GET, logged-in users can POST,
//    only admins can DELETE
```

<details>
<summary>✅ Solution</summary>

```js
userSchema.pre('save', async function () {
  if (!this.isModified('password')) return;
  this.password = await bcrypt.hash(this.password, 10);
});

exports.register = asyncHandler(async (req, res) => {
  const { name, email, password } = req.body;
  const exists = await User.findOne({ email });
  if (exists) throw new AppError('Email already registered', 400);
  const user = await User.create({ name, email, password });
  res.status(201).json({ id: user._id, name: user.name, email: user.email });
});

exports.login = asyncHandler(async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  const ok = user && (await bcrypt.compare(password, user.password));
  if (!ok) throw new AppError('Invalid credentials', 401);
  const token = jwt.sign({ userId: user._id, role: user.role }, process.env.JWT_SECRET, { expiresIn: '1d' });
  res.json({ token });
});

exports.protect = (req, res, next) => {
  const header = req.headers.authorization;
  if (!header || !header.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Not logged in' });
  }
  try {
    req.user = jwt.verify(header.split(' ')[1], process.env.JWT_SECRET);
    next();
  } catch {
    res.status(401).json({ error: 'Invalid or expired token' });
  }
};

exports.authorize = (...roles) => (req, res, next) => {
  if (!roles.includes(req.user.role)) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  next();
};

// 6
exports.me = asyncHandler(async (req, res) => {
  const user = await User.findById(req.user.userId).select('-password');
  if (!user) throw new AppError('User not found', 404);
  res.json(user);
});
// routes/auth.js → router.get('/me', protect, me);

// 7: routes/books.js
router.get('/', controller.getBooks);
router.get('/:id', controller.getBookById);
router.post('/', protect, controller.createBook);
router.delete('/:id', protect, authorize('admin'), controller.deleteBook);
```

To make yourself an admin while testing, change your user's `role` to `"admin"` in Compass, then log in again to get a new token.

</details>

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

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. Why do we validate data twice: in the request (Joi / express-validator) and in the schema?
2. What is the difference between `min` and `minlength`?
3. What does `next(err)` do? How is it different from `next()`?
4. Why do we create an `AppError` class?
5. Why must we never save passwords as plain text? What does bcrypt do?
6. Why does login say "Invalid credentials" and not "Wrong password"?
7. What are the three parts of a JWT? Can anyone read the payload?
8. How does the client send the token to the server?
9. What is the difference between 401 and 403?
10. Why do we check `isModified('password')` in the pre-save hook?

<details>
<summary>✅ Solution</summary>

1. Request validation stops bad data early with clear messages. Schema validation is the last protection: it also covers data saved from other places in the code.
2. `min` is for numbers/dates. `minlength` is for string length.
3. `next(err)` skips the normal routes and goes straight to the error handler. `next()` continues to the next middleware.
4. To throw an error together with its status code (`new AppError('Not found', 404)`) and handle all errors in one place.
5. If the database leaks, everyone's password is exposed. bcrypt makes a one-way hash with a salt, so the original password can't be read back.
6. So attackers can't find out which emails are registered.
7. Header, payload and signature. Yes, the payload is only base64-encoded (not encrypted), so never put secrets in it. The signature only proves it wasn't changed.
8. In a header: `Authorization: Bearer <token>`.
9. 401 means you are not logged in (or the token is bad). 403 means you are logged in but not allowed to do this.
10. So the password isn't hashed again (double-hashed) when the user updates other fields like the name.

</details>

### B. Find the Bug

```js
// Bug 1: passwords appear as plain text in the database
const user = await User.create({ email, password });   // the schema has no pre-save hook

// Bug 2: security problem
if (!user) return res.status(401).json({ error: 'Email not found' });
if (!(await bcrypt.compare(password, user.password))) {
  return res.status(401).json({ error: 'Wrong password' });
}

// Bug 3: three problems in one line
const token = jwt.sign({ userId: user._id, password: user.password }, 'secret');

// Bug 4: every request with a valid token gets "Invalid or expired token"
const token = req.headers.authorization;
req.user = jwt.verify(token, process.env.JWT_SECRET);

// Bug 5: "Cannot set properties of undefined (setting 'password')"
userSchema.pre('save', async () => {
  this.password = await bcrypt.hash(this.password, 10);
});

// Bug 6: "Cannot read properties of undefined (reading 'role')"
router.delete('/:id', authorize('admin'), protect, deleteBook);
```

<details>
<summary>✅ Solution</summary>

1. Add the pre-save hook (or hash before `create`):
   ```js
   userSchema.pre('save', async function () {
     if (!this.isModified('password')) return;
     this.password = await bcrypt.hash(this.password, 10);
   });
   ```
2. Use the same message for both cases: `'Invalid credentials'`.
3. (a) the password hash is in the payload (anyone can read it) (b) the secret is hardcoded and weak (c) no expiry.
   ```js
   const token = jwt.sign({ userId: user._id, role: user.role }, process.env.JWT_SECRET, { expiresIn: '1d' });
   ```
4. The header is `Bearer <token>`, so remove the `Bearer ` part:
   ```js
   const token = req.headers.authorization?.split(' ')[1];
   ```
5. Arrow functions have no `this`. Use `async function () { ... }`.
6. `authorize` runs before `protect`, so `req.user` doesn't exist yet. Swap them:
   ```js
   router.delete('/:id', protect, authorize('admin'), deleteBook);
   ```

</details>

### C. Coding Tasks

**Task 1: Joi for books (easy).** Use the `validate(bookSchema)` middleware on `POST /api/books` and `PATCH /api/books/:id`. For PATCH, every field is optional, but at least one must be sent (`Joi.object({...}).min(1)`).

**Task 2: Clean controllers (medium).** Rewrite all 5 book controllers with `asyncHandler` + `AppError`: no `try/catch` and no `res.status(404)` inside the controllers. All errors go to `errorHandler.js`.

**Task 3: Register + login flow (medium).** Build `POST /api/auth/register` and `POST /api/auth/login`, then test this flow in Postman:

```
register → 201          register same email again → 400
login wrong password → 401          login correct → { token }
GET /api/auth/me without token → 401          with token → user (no password)
```

**Task 4: My notes (medium).** A `Note` model `{ text, owner: { type: ObjectId, ref: 'User' } }`. All note routes are protected:
- `POST /api/notes` sets `owner` from `req.user.userId` (never from the body)
- `GET /api/notes` returns only **my** notes
- `DELETE /api/notes/:id` deletes only if it's my note, otherwise 404

**Task 5: Admin users list (easy).** `GET /api/users` is admin only and returns all users without passwords.

**Challenge: Change password.** `PATCH /api/auth/password` (protected) with `{ currentPassword, newPassword }`. Check the current password (401 if wrong), require a new password of at least 6 characters (400), then save. The pre-save hook hashes it. Log in with the new password to prove it works.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```js
// Task 1
const bookCreateSchema = Joi.object({
  title: Joi.string().required(),
  author: Joi.string().required(),
  pages: Joi.number().integer().min(1),
  genre: Joi.string().valid('novel', 'science', 'history', 'kids'),
});

const bookUpdateSchema = Joi.object({
  title: Joi.string(),
  author: Joi.string(),
  pages: Joi.number().integer().min(1),
  genre: Joi.string().valid('novel', 'science', 'history', 'kids'),
}).min(1);

router.post('/', protect, validate(bookCreateSchema), controller.createBook);
router.patch('/:id', protect, validate(bookUpdateSchema), controller.updateBook);
```

```js
// Task 2
exports.getBooks = asyncHandler(async (req, res) => {
  const filter = req.query.genre ? { genre: req.query.genre } : {};
  res.json(await Book.find(filter).sort({ createdAt: -1 }));
});

exports.getBookById = asyncHandler(async (req, res) => {
  const book = await Book.findById(req.params.id);
  if (!book) throw new AppError('Book not found', 404);
  res.json(book);
});

exports.createBook = asyncHandler(async (req, res) => {
  res.status(201).json(await Book.create(req.body));
});

exports.updateBook = asyncHandler(async (req, res) => {
  const book = await Book.findByIdAndUpdate(req.params.id, req.body, { new: true, runValidators: true });
  if (!book) throw new AppError('Book not found', 404);
  res.json(book);
});

exports.deleteBook = asyncHandler(async (req, res) => {
  const book = await Book.findByIdAndDelete(req.params.id);
  if (!book) throw new AppError('Book not found', 404);
  res.status(204).send();
});
```

```js
// Task 3: register, login and me are the solution of "Your Turn: Part 4"
// routes/auth.js
const router = require('express').Router();
const { register, login, me } = require('../controllers/authController');
const { protect } = require('../middleware/auth');

router.post('/register', validate(registerSchema), register);
router.post('/login', login);
router.get('/me', protect, me);

module.exports = router;
// app.js → app.use('/api/auth', require('./routes/auth'));
```

```js
// Task 4: models/Note.js
const noteSchema = new mongoose.Schema(
  {
    text: { type: String, required: true, trim: true },
    owner: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  },
  { timestamps: true }
);
module.exports = mongoose.model('Note', noteSchema);

// controllers/noteController.js
exports.createNote = asyncHandler(async (req, res) => {
  const note = await Note.create({ text: req.body.text, owner: req.user.userId });
  res.status(201).json(note);
});

exports.getMyNotes = asyncHandler(async (req, res) => {
  res.json(await Note.find({ owner: req.user.userId }).sort({ createdAt: -1 }));
});

exports.deleteNote = asyncHandler(async (req, res) => {
  const note = await Note.findOneAndDelete({ _id: req.params.id, owner: req.user.userId });
  if (!note) throw new AppError('Note not found', 404);
  res.status(204).send();
});

// routes/notes.js
router.use(protect);              // every route below is protected
router.get('/', getMyNotes);
router.post('/', createNote);
router.delete('/:id', deleteNote);
```

```js
// Task 5
exports.getUsers = asyncHandler(async (req, res) => {
  res.json(await User.find().select('-password'));
});
router.get('/', protect, authorize('admin'), getUsers);   // mounted at /api/users
```

```js
// Challenge
exports.changePassword = asyncHandler(async (req, res) => {
  const { currentPassword, newPassword } = req.body;
  if (!newPassword || newPassword.length < 6) {
    throw new AppError('New password needs at least 6 characters', 400);
  }

  const user = await User.findById(req.user.userId);
  if (!user) throw new AppError('User not found', 404);

  const ok = await bcrypt.compare(currentPassword ?? '', user.password);
  if (!ok) throw new AppError('Current password is wrong', 401);

  user.password = newPassword;    // the pre-save hook hashes it
  await user.save();
  res.json({ message: 'Password changed' });
});
// routes/auth.js → router.patch('/password', protect, changePassword);
```

</details>

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
