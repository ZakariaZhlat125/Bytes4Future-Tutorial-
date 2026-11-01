# Session 9: NestJS Request Lifecycle & MongoDB Fundamentals

**Duration:** 3 hours
**Level:** Beginner–Intermediate
**Prerequisites:** Session 8 (NestJS basics, controllers, services, modules)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

## Session Overview

- **Part 1:** The request lifecycle + middleware
- **Part 2:** Pipes & validation — built-in pipes, DTOs with `class-validator`, CORS, static files
- **Part 3:** Interceptors + security headers (helmet) + logging
- **Part 4:** Guards, exceptions & exception filters
- **Part 5:** MongoDB — NoSQL concepts, documents & collections, setup
- **Part 6:** CRUD in the shell — insert/find/update/delete, operators, projection
- **Lab:** Hardened to-do API + MongoDB query practice

---

## Part 1: The Request Lifecycle & Middleware

> 💡 **In simple words:** Before a request reaches your controller, it passes several checkpoints, like the security checks at an airport. Each checkpoint has one job. NestJS gives every job its own tool:
> - **Middleware**: looks at every request first (logging, adding data).
> - **Guards**: decide "may this request continue?" (auth, roles).
> - **Interceptors**: run code before **and** after the handler (timing, changing the response).
> - **Pipes**: check and convert the input (validation, `'42'` → `42`).
> - **Exception filters**: turn errors into clean JSON responses.

```
Request
  → Middleware        (app.use / MiddlewareConsumer)
  → Guards            (@UseGuards)          can stop with 401/403
  → Interceptors      (before)
  → Pipes             (@Body, @Param ...)   can stop with 400
  → Controller method
  → Interceptors      (after)               can change the response
  → Exception filters (only if something threw)
Response
```

### Middleware

Middleware is a function `(req, res, next)`. It can inspect or change the request, or end the response. It **must** call `next()` to continue.

```ts
// src/common/middleware/logger.middleware.ts — functional middleware
import { IncomingMessage, ServerResponse } from 'http';

export function logger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  console.log(`${req.method} ${req.url}`);
  next();                                   // REQUIRED — or the request hangs
}

// src/main.ts — global: runs for EVERY request
app.use(logger);
```

Class middleware can use dependency injection and is applied per controller or path in a module:

```ts
// src/common/middleware/request-time.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { IncomingMessage, ServerResponse } from 'http';

export type AppRequest = IncomingMessage & { requestTime?: string };

@Injectable()
export class RequestTimeMiddleware implements NestMiddleware {
  use(req: AppRequest, res: ServerResponse, next: () => void) {
    req.requestTime = new Date().toISOString();   // attach data for later handlers
    next();
  }
}

// src/app.module.ts
import { MiddlewareConsumer, Module, NestModule } from '@nestjs/common';

@Module({ imports: [BooksModule] })
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(RequestTimeMiddleware).forRoutes(BooksController);   // only books routes
  }
}

// read it in a controller
@Get()
findAll(@Req() req: AppRequest) {
  return { requestTime: req.requestTime, books: this.booksService.findAll() };
}
```

**Critical rules:**
- Middleware runs **first**, in registration order
- Always `next()` **or** end the response — never neither
- Use middleware for "every request" jobs (logging). For "may it continue?" use a **guard**; for "is the input valid?" use a **pipe**
- Keep each one small and single-purpose

---

### ✍️ Your Turn: Part 1

Use your `my-api` project from Session 8.

```ts
// 1. Logger: print "GET /api/books" for every request, then continue
export function logger(req: IncomingMessage, res: ServerResponse, ____: () => void) {
  console.log(`${req.____} ${req.____}`);
  ____();
}

// 2. Timer: print how long each request took
export function timer(req: IncomingMessage, res: ServerResponse, next: () => void) {
  const start = Date.now();
  res.on('finish', () => {            // runs when the response has been sent
    console.log(`Took ${____} ms`);
  });
  next();
}

// main.ts
app.use(____);
app.use(____);

// 3. Class middleware that adds req.requestTime, applied ONLY to BooksController
@____()
export class RequestTimeMiddleware implements ____ {
  use(req: AppRequest, res: ServerResponse, next: () => void) {
    // TODO
  }
}

export class AppModule implements NestModule {
  configure(consumer: ____) {
    consumer.apply(____).forRoutes(____);
  }
}

// 4. Question: what happens if a middleware never calls next() and never sends a response?
```

<details>
<summary>✅ Solution</summary>

```ts
export function logger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  console.log(`${req.method} ${req.url}`);
  next();
}

export function timer(req: IncomingMessage, res: ServerResponse, next: () => void) {
  const start = Date.now();
  res.on('finish', () => {
    console.log(`Took ${Date.now() - start} ms`);
  });
  next();
}

app.use(logger);
app.use(timer);

@Injectable()
export class RequestTimeMiddleware implements NestMiddleware {
  use(req: AppRequest, res: ServerResponse, next: () => void) {
    req.requestTime = new Date().toISOString();
    next();
  }
}

export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(RequestTimeMiddleware).forRoutes(BooksController);
  }
}
```

4. The request hangs: the client waits until it times out.

</details>

---

## Part 2: Pipes & Validation

> 💡 **In simple words:** A pipe sits right in front of your method and works on its **input**. It can convert a value (`'42'` → `42`) or reject it with a 400 before your code even runs. The `ValidationPipe` checks the body against rules written on the DTO class, so you stop writing `if (!body.title)` by hand.

### Built-in pipes

| Pipe | Does |
|------|------|
| `ParseIntPipe` / `ParseFloatPipe` | `'42'` → `42`, otherwise 400 |
| `ParseBoolPipe` | `'true'` → `true` |
| `ParseEnumPipe` | value must be one of an enum |
| `DefaultValuePipe` | default when the query value is missing |
| `ValidationPipe` | validates a DTO with `class-validator` |

```ts
@Get()
findAll(
  @Query('page', new DefaultValuePipe(1), ParseIntPipe) page: number,     // ?page missing → 1
  @Query('completed', new ParseBoolPipe({ optional: true })) completed?: boolean,
) { ... }
```

### DTO validation with `class-validator`

```bash
npm install class-validator class-transformer
```

```ts
// src/todos/dto/create-todo.dto.ts
import { IsBoolean, IsNotEmpty, IsOptional, IsString } from 'class-validator';

export class CreateTodoDto {
  @IsString()
  @IsNotEmpty({ message: 'text is required' })
  text: string;

  @IsOptional()
  @IsBoolean()
  completed?: boolean;
}
```

```ts
// src/main.ts — turn validation on for the whole app
import { ValidationPipe } from '@nestjs/common';

app.useGlobalPipes(new ValidationPipe({
  whitelist: true,               // remove fields that are not in the DTO
  forbidNonWhitelisted: true,    // ...or reject them with 400
  transform: true,               // turn the body into a real CreateTodoDto instance
}));
```

```json
// POST /api/todos with {} →
{ "statusCode": 400, "message": ["text is required", "text must be a string"], "error": "Bad Request" }
```

Session 11 goes deeper (more decorators, update DTOs, custom messages).

### CORS and static files

> 💡 **In simple words:** **CORS** is the browser's rule that a website may only call an API on another address if the API allows it. Your React app runs on `localhost:5173` and your API on `localhost:3000`, so the API must say "5173 is allowed". **Static files** are files sent as they are (HTML, CSS, images), with no code in between.

```ts
// src/main.ts
app.enableCors({
  origin: 'http://localhost:5173',     // restrict in production!
  methods: ['GET', 'POST', 'PATCH', 'DELETE'],
});
```

```bash
npm install @nestjs/serve-static
```

```ts
// src/app.module.ts — serve ./public at /
import { ServeStaticModule } from '@nestjs/serve-static';
import { join } from 'path';

@Module({
  imports: [
    ServeStaticModule.forRoot({
      rootPath: join(__dirname, '..', 'public'),
      exclude: ['/api/{*path}'],         // never serve files for API routes
    }),
    BooksModule,
  ],
})
export class AppModule {}
```

---

### ✍️ Your Turn: Part 2

Run `npm install class-validator class-transformer @nestjs/serve-static` first.

```ts
// 1. A DTO for books
import { IsInt, IsNotEmpty, IsOptional, IsString, Min } from 'class-validator';

export class CreateBookDto {
  @____()
  @IsNotEmpty({ message: '____' })
  title: string;

  @IsString()
  @IsOptional()
  author?: string;

  @____()                // optional
  @IsInt()
  @____(1, { message: 'pages must be a positive number' })
  pages?: number;
}

// 2. main.ts: validate every body, strip unknown fields, convert types
app.useGlobalPipes(new ____({ whitelist: ____, transform: ____ }));

// 3. main.ts: allow ONLY the React app http://localhost:5173
app.____({ origin: '____' });

// 4. GET /api/books?page=2 → page is a number, default 1
@Get()
findAll(@Query('page', new ____(1), ____) page: number) {
  return { page, type: typeof page };
}

// 5. TODO: serve public/index.html (<h1>Hello from static files</h1>) at http://localhost:3000/
```

**Test:** `POST /api/books` with `{}`, then `{ "title": "X", "pages": -3 }`, then `{ "title": "X", "hacker": true }`. What do you get each time?

<details>
<summary>✅ Solution</summary>

```ts
export class CreateBookDto {
  @IsString()
  @IsNotEmpty({ message: 'title is required' })
  title: string;

  @IsString()
  @IsOptional()
  author?: string;

  @IsOptional()
  @IsInt()
  @Min(1, { message: 'pages must be a positive number' })
  pages?: number;
}

app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
app.enableCors({ origin: 'http://localhost:5173' });

@Get()
findAll(@Query('page', new DefaultValuePipe(1), ParseIntPipe) page: number) {
  return { page, type: typeof page };
}

// app.module.ts
ServeStaticModule.forRoot({
  rootPath: join(__dirname, '..', 'public'),
  exclude: ['/api/{*path}'],
}),
```

```html
<!-- public/index.html (in the project root, next to src/) -->
<h1>Hello from static files</h1>
```

- `{}` → 400 `["title is required", "title must be a string"]`
- `pages: -3` → 400 `["pages must be a positive number"]`
- `hacker: true` → 201, and `hacker` is **removed** (`whitelist`). With `forbidNonWhitelisted: true` you'd get 400 `property hacker should not exist`.

</details>

---

## Part 3: Interceptors, Helmet & Logging

> 💡 **In simple words:** An interceptor wraps your handler: it runs some code **before** it, lets it run, and then runs code **after** it, with access to the result. That makes it perfect for measuring time, logging the status code, or wrapping every response in the same shape.

```ts
// src/common/interceptors/logging.interceptor.ts
import { CallHandler, ExecutionContext, Injectable, Logger, NestInterceptor } from '@nestjs/common';
import { Observable, tap } from 'rxjs';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  private readonly logger = new Logger('HTTP');

  intercept(context: ExecutionContext, next: CallHandler): Observable<unknown> {
    const req = context.switchToHttp().getRequest();
    const res = context.switchToHttp().getResponse();
    const start = Date.now();

    return next.handle().pipe(                         // next.handle() = run the handler
      tap(() => this.logger.log(`${req.method} ${req.url} ${res.statusCode} - ${Date.now() - start}ms`)),
    );
  }
}

// main.ts
app.useGlobalInterceptors(new LoggingInterceptor());
```

```ts
// Change the response: wrap everything in { data: ... }
import { map } from 'rxjs';

@Injectable()
export class WrapInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    return next.handle().pipe(map((data) => ({ data })));
  }
}
```

> NestJS uses **RxJS** here: `next.handle()` returns an Observable (a stream with one value: your result). You only need two operators: `tap` (look at it) and `map` (change it).

### Helmet — security headers

```bash
npm install helmet
```

```ts
// main.ts — apply early
import helmet from 'helmet';
app.use(helmet());        // sets ~15 security headers (CSP, X-Content-Type-Options…)
```

| Tool | Does | Notes |
|------|------|-------|
| `Logger` / `LoggingInterceptor` | Logs `METHOD URL STATUS - ms` | Never log passwords or tokens |
| `app.enableCors()` | Adds CORS headers | Without `origin` it allows **all** origins — fine for dev, restrict in prod |
| `helmet()` | Security headers | Always in production |

---

### ✍️ Your Turn: Part 3

Run `npm install helmet` first.

```ts
// 1. Complete the logging interceptor
@____()
export class LoggingInterceptor implements ____ {
  intercept(context: ExecutionContext, next: ____): Observable<unknown> {
    const req = context.____().getRequest();
    const start = Date.now();
    return next.____().pipe(
      ____(() => console.log(`${req.method} ${req.url} - ${Date.now() - start}ms`)),
    );
  }
}

// 2. main.ts: put these in a sensible order
//    app.useGlobalPipes(...), app.use(helmet()), app.enableCors(...), app.useGlobalInterceptors(...)

// 3. TODO: a TimestampInterceptor that adds { servedAt: <ISO date> } to every object response
//    hint: map((data) => ({ ...data, servedAt: new Date().toISOString() }))
//    Apply it to ONE controller only with @UseInterceptors(TimestampInterceptor)

// 4. Question: open DevTools → Network on any API response. Which new headers did helmet add?
```

<details>
<summary>✅ Solution</summary>

```ts
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<unknown> {
    const req = context.switchToHttp().getRequest();
    const start = Date.now();
    return next.handle().pipe(
      tap(() => console.log(`${req.method} ${req.url} - ${Date.now() - start}ms`)),
    );
  }
}

// main.ts
app.use(helmet());                     // security headers first
app.enableCors({ origin: 'http://localhost:5173' });
app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
app.useGlobalInterceptors(new LoggingInterceptor());

@Injectable()
export class TimestampInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    return next.handle().pipe(map((data) => ({ ...data, servedAt: new Date().toISOString() })));
  }
}

@Controller('about')
@UseInterceptors(TimestampInterceptor)
export class AboutController { ... }
```

4. For example `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: SAMEORIGIN`, `Strict-Transport-Security`. The `X-Powered-By` header is removed.

</details>

---

## Part 4: Guards, Exceptions & Exception Filters

### Guards

> 💡 **In simple words:** A guard answers one question: **may this request continue?** It returns `true` (continue) or throws an exception (stop). Guards are the right place for checks like "is there a valid API key?" or "is this user an admin?".

```ts
// src/common/guards/api-key.guard.ts
import { CanActivate, ExecutionContext, Injectable, UnauthorizedException } from '@nestjs/common';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const req = context.switchToHttp().getRequest();
    if (req.headers['x-api-key'] !== 'secret123') {
      throw new UnauthorizedException('Invalid API key');   // stop with 401
    }
    return true;                                            // continue
  }
}

// whole controller
@Controller('admin')
@UseGuards(ApiKeyGuard)
export class AdminController {
  @Get('stats')
  stats() { return { users: 10 }; }
}

// or one route only:  @Post() @UseGuards(ApiKeyGuard) create() { ... }
```

If a guard returns `false` instead of throwing, NestJS answers `403 Forbidden resource`.

### Exceptions

> 💡 **In simple words:** When something goes wrong, **throw**. NestJS stops everything, skips the rest of the handler, and sends an error response. Errors that are not `HttpException`s (a bug, a crash) become a safe `500 Internal server error`, so internal details don't leak.

```ts
throw new NotFoundException('Book not found');                  // 404
throw new BadRequestException('Cannot divide by zero');          // 400
throw new HttpException('I am a teapot', 418);                   // any status
throw new Error('boom');                                         // → 500 { message: 'Internal server error' }
```

Unknown routes get a 404 automatically: `{ "statusCode": 404, "message": "Cannot GET /api/nothing", "error": "Not Found" }`.

### Exception filters — one place for the error format

> 💡 **In simple words:** An exception filter catches errors and decides how the error response looks. With one global filter, every error in your API has the same shape, and you can log it in one place.

```ts
// src/common/filters/all-exceptions.filter.ts
import { ArgumentsHost, Catch, ExceptionFilter, HttpException, HttpStatus, Logger } from '@nestjs/common';
import { HttpAdapterHost } from '@nestjs/core';

@Catch()                                       // empty = catch EVERYTHING
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger('Errors');

  constructor(private readonly adapterHost: HttpAdapterHost) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const { httpAdapter } = this.adapterHost;
    const ctx = host.switchToHttp();

    const status = exception instanceof HttpException ? exception.getStatus() : HttpStatus.INTERNAL_SERVER_ERROR;
    const message = exception instanceof HttpException
      ? (exception.getResponse() as { message?: unknown }).message ?? exception.message
      : 'Internal server error';

    if (status === 500) this.logger.error(exception);

    httpAdapter.reply(ctx.getResponse(), {
      statusCode: status,
      message,
      path: httpAdapter.getRequestUrl(ctx.getRequest()),
      timestamp: new Date().toISOString(),
    }, status);
  }
}
```

```ts
// src/app.module.ts — register it globally (with dependency injection)
import { APP_FILTER } from '@nestjs/core';

@Module({
  providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }],
})
export class AppModule {}
```

| Tool | Answers | Typical status |
|------|---------|----------------|
| Guard | May it continue? | 401 / 403 |
| Pipe | Is the input valid? | 400 |
| Exception filter | How does the error look? | any |

---

### ✍️ Your Turn: Part 4

```ts
// 1. API key guard for /api/admin
@Injectable()
export class ApiKeyGuard implements ____ {
  canActivate(context: ____): boolean {
    const req = context.switchToHttp().____();
    // TODO: header x-api-key !== 'secret123' → throw UnauthorizedException('Invalid API key')
    return ____;
  }
}

@Controller('admin')
@____(ApiKeyGuard)
export class AdminController {
  @Get('stats')
  stats() { return { users: 10 }; }
}

// 2. A route that fails on purpose with a normal Error
@Get('crash')
crash() {
  throw new ____('Something broke!');
}
// What does the client receive? What do you see in the terminal?

// 3. A route that fails with a 400
@Get('divide')
divide(@Query('a', ParseFloatPipe) a: number, @Query('b', ParseFloatPipe) b: number) {
  // TODO: b === 0 → BadRequestException('Cannot divide by zero')
  return { result: a / b };
}

// 4. Complete the global filter so every error looks like:
//    { statusCode, message, path, timestamp }
@____()
export class AllExceptionsFilter implements ExceptionFilter {
  constructor(private readonly adapterHost: HttpAdapterHost) {}
  catch(exception: unknown, host: ArgumentsHost) {
    const status = exception instanceof ____ ? exception.____() : 500;
    // TODO
  }
}
// app.module.ts → providers: [{ provide: ____, useClass: AllExceptionsFilter }]
```

**Test:** call `/api/admin/stats` without a header (401), with `x-api-key: secret123` (200), `/api/crash` (500), `/api/divide?a=1&b=0` (400) and `/api/nothing` (404). All errors must have the same shape.

<details>
<summary>✅ Solution</summary>

```ts
@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const req = context.switchToHttp().getRequest();
    if (req.headers['x-api-key'] !== 'secret123') {
      throw new UnauthorizedException('Invalid API key');
    }
    return true;
  }
}

@Controller('admin')
@UseGuards(ApiKeyGuard)
export class AdminController {
  @Get('stats')
  stats() { return { users: 10 }; }
}

@Get('crash')
crash() {
  throw new Error('Something broke!');
}
// Client: 500 { message: 'Internal server error' }. Terminal: the real error + stack.

@Get('divide')
divide(@Query('a', ParseFloatPipe) a: number, @Query('b', ParseFloatPipe) b: number) {
  if (b === 0) throw new BadRequestException('Cannot divide by zero');
  return { result: a / b };
}
```

The full filter is the `AllExceptionsFilter` example above, registered with `{ provide: APP_FILTER, useClass: AllExceptionsFilter }`.

</details>

---

## Part 5: MongoDB Fundamentals

> 💡 **In simple words:** A database stores data **permanently**: it's still there after the server restarts. Our in-memory arrays were lost every time. MongoDB stores data as documents that look like JavaScript objects, grouped in collections.

### NoSQL vs SQL

> 💡 **In simple words:** A SQL database is like an Excel sheet: fixed columns, and every row must fit them. MongoDB is like a folder of JSON files: each document can have its own fields. The hierarchy is **database** → **collections** (like "users" or "products") → **documents** (one user).

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

> 💡 **In simple words:** You can run MongoDB on your own computer, or use **Atlas**, MongoDB in the cloud with a free tier. Atlas is easier for beginners: there's nothing to install, and you get a **connection string** (an address that includes a username and password) to connect from your code.

- **Local:** install MongoDB Community Server → `mongod` → `mongodb://127.0.0.1:27017`
- **Atlas (recommended):** free managed cluster → connection string `mongodb+srv://user:pass@cluster0.mongodb.net/mydb` → whitelist your IP → put the URI in `.env` (`MONGODB_URI=`, Session 10)

**Tools:** `mongosh` (CLI — `use mydb`, `db.users.find()`) and **Compass** (GUI for browsing/querying visually). Learn both.

---

## Part 6: CRUD in mongosh

> 💡 **In simple words:** CRUD stands for **C**reate, **R**ead, **U**pdate, **D**elete: the four things every app does with data. In mongosh, each one is a function on a collection: `insertOne`, `find`, `updateOne`, `deleteOne`. A **filter** is an object that describes which documents you want.

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
).sort({ price: -1 }).skip(10).limit(5);                   // descending, page 3 of 5-per-page

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
- Forgetting `$set` in `updateOne` **replaces the entire document** — data loss (mongosh refuses it with "Update document requires atomic operators")
- `deleteOne({})`/`deleteOne()` deletes the first document it finds
- `find` returns a cursor — iterate it in the shell; Mongoose handles it for you (Session 10)
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

### Part A — Hardened to-do API

Extend the Session 8 lab:

```ts
// main.ts
const app = await NestFactory.create(AppModule);
app.setGlobalPrefix('api');
app.use(helmet());
app.enableCors({ origin: 'http://localhost:5173' });
app.use(logger);                                            // functional middleware
app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
app.useGlobalInterceptors(new LoggingInterceptor());
await app.listen(process.env.PORT ?? 3000);

// app.module.ts
providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }]   // one error format

// todos
POST  /api/todos   → CreateTodoDto (400 on empty text)
PATCH /api/todos/:id → UpdateTodoDto (every field optional)
```

**Challenges:** `CreateTodoDto` and `UpdateTodoDto` with `class-validator`; an `ApiKeyGuard` guarding DELETE only; every error (400/401/404/500) has the same JSON shape.

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

1. Name the steps of the NestJS request lifecycle in order.
2. When do you use middleware, when a guard, and when a pipe?
3. What does `ValidationPipe` with `whitelist: true` do?
4. What happens when a guard throws `UnauthorizedException`? When it returns `false`?
5. What can an interceptor do that middleware can't?
6. What happens when your code throws a plain `new Error()`?
7. What is an exception filter for? How do you register one globally?
8. What do `helmet()` and `enableCors()` do (one line each)?
9. What is the difference between SQL tables and MongoDB collections? What is `_id`?
10. What does `{ price: { $gte: 10, $lte: 50 } }` find? What is projection?

<details>
<summary>✅ Solution</summary>

1. Middleware → guards → interceptors (before) → pipes → controller method → interceptors (after) → exception filters (on errors).
2. Middleware for jobs on every request (logging). A guard for "may it continue?" (auth, roles). A pipe for checking and converting input (validation).
3. It removes every body field that has no decorator in the DTO, so clients can't sneak in extra fields like `role: 'admin'`.
4. Throwing → that exception's status (401). Returning `false` → `403 Forbidden resource`.
5. It runs code **after** the handler too, with access to the result, so it can measure time or change the response.
6. NestJS sends `500 { message: 'Internal server error' }` and logs the real error. The client never sees internal details.
7. To control how error responses look (and log them) in one place. `{ provide: APP_FILTER, useClass: MyFilter }` in a module's providers (or `app.useGlobalFilters(...)`).
8. `helmet()` adds security headers. `enableCors()` lets other origins (like your React app) call the API.
9. Tables have a fixed schema with rows; collections hold flexible JSON-like documents. `_id` is the document's unique id, created automatically.
10. Documents whose price is between 10 and 50 (both included). Projection chooses which fields to return: `db.users.find({}, { name: 1, email: 1, _id: 0 })`.

</details>

### B. Find the Bug

```ts
// Bug 1: every request hangs forever
export function logger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  console.log(req.method, req.url);
}

// Bug 2: POST /api/books with { "pages": "abc" } is saved without an error
export class CreateBookDto {
  @IsInt() @Min(1) pages?: number;
}
// main.ts has no app.useGlobalPipes(...)

// Bug 3: the guard is written, but the route is still open to everyone
@Injectable()
export class ApiKeyGuard implements CanActivate { ... }

@Controller('admin')
export class AdminController {
  @Get('stats') stats() { return { users: 10 }; }
}

// Bug 4: the client gets 500 "Internal server error" instead of 400
@Get('divide')
divide(@Query('b', ParseFloatPipe) b: number) {
  if (b === 0) throw new Error('Cannot divide by zero');
}

// Bug 5 (mongosh): fails with "Update document requires atomic operators"
db.products.updateOne({ name: 'Mouse' }, { price: 30 });

// Bug 6 (mongosh): we wanted to delete the Book, but another document was deleted
db.products.deleteOne({});
```

<details>
<summary>✅ Solution</summary>

```ts
// Bug 1: call next()
export function logger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  console.log(req.method, req.url);
  next();
}

// Bug 2: the decorators do nothing without the ValidationPipe
app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));

// Bug 3: attach the guard
@Controller('admin')
@UseGuards(ApiKeyGuard)
export class AdminController { ... }

// Bug 4: a plain Error is "unexpected" → 500. Throw an HttpException:
if (b === 0) throw new BadRequestException('Cannot divide by zero');
```

```js
// Bug 5: use $set to change one field
db.products.updateOne({ name: 'Mouse' }, { $set: { price: 30 } });

// Bug 6: an empty filter matches the first document, so always filter
db.products.deleteOne({ name: 'Book' });
```

</details>

### C. Coding Tasks

**Task 1: Request counter (easy).** A `StatsService` holds `totalRequests`. A global interceptor (registered with `APP_INTERCEPTOR`, so it can inject `StatsService`) adds 1 for every request. `GET /api/stats` returns `{ totalRequests: 12 }`.

**Task 2: Maintenance mode (easy).** A global guard (`APP_GUARD`): if `process.env.MAINTENANCE === 'true'`, every request gets `503 { message: 'We are under maintenance' }`. Test it with `MAINTENANCE=true npm run start:dev` (on PowerShell: `$env:MAINTENANCE='true'; npm run start:dev`).

**Task 3: Roles guard (medium).** Write a `@Roles('admin')` decorator and a `RolesGuard` that reads the header `x-role` and throws `ForbiddenException` if it doesn't match. Routes without `@Roles` stay open. Use it on `DELETE /api/books/:id`.

**Task 4: Divide with error handling (medium).** `GET /api/divide?a=10&b=0` with `ParseFloatPipe`. When `b` is 0, throw a `BadRequestException`. `?a=x` must also give 400. The global filter sends `{ statusCode, message, path, timestamp }`.

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

**Challenge: Request logger to a file.** A middleware that adds one line per request to `logs.txt`: `2026-10-17T10:00:00.000Z GET /api/books 200 12ms`. Hint: `fs.appendFile` + `res.on('finish', ...)` + `res.statusCode`. Why is middleware a better place for this than an interceptor? (Think about 404s for unknown routes.)

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```ts
// Task 1
@Injectable()
export class StatsService {
  totalRequests = 0;
}

@Injectable()
export class CountInterceptor implements NestInterceptor {
  constructor(private readonly stats: StatsService) {}
  intercept(context: ExecutionContext, next: CallHandler) {
    this.stats.totalRequests++;
    return next.handle();
  }
}

@Controller('stats')
export class StatsController {
  constructor(private readonly stats: StatsService) {}
  @Get()
  get() { return { totalRequests: this.stats.totalRequests }; }
}

// app.module.ts
@Module({
  controllers: [StatsController],
  providers: [StatsService, { provide: APP_INTERCEPTOR, useClass: CountInterceptor }],
})
export class AppModule {}
```

```ts
// Task 2
@Injectable()
export class MaintenanceGuard implements CanActivate {
  canActivate(): boolean {
    if (process.env.MAINTENANCE === 'true') {
      throw new ServiceUnavailableException('We are under maintenance');
    }
    return true;
  }
}
// app.module.ts → providers: [{ provide: APP_GUARD, useClass: MaintenanceGuard }]
```

```ts
// Task 3: common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';
export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);

// common/guards/roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private readonly reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.getAllAndOverride<string[]>(ROLES_KEY, [
      context.getHandler(),        // @Roles on the method
      context.getClass(),          // or on the controller
    ]);
    if (!roles) return true;       // no @Roles → open route

    const req = context.switchToHttp().getRequest();
    if (!roles.includes(req.headers['x-role'])) throw new ForbiddenException('Forbidden');
    return true;
  }
}

// books.controller.ts
@Delete(':id')
@HttpCode(204)
@UseGuards(RolesGuard)
@Roles('admin')
remove(@Param('id', ParseIntPipe) id: number) {
  this.booksService.remove(id);
}
```

```ts
// Task 4
@Get('divide')
divide(@Query('a', ParseFloatPipe) a: number, @Query('b', ParseFloatPipe) b: number) {
  if (b === 0) throw new BadRequestException('Cannot divide by zero');
  return { result: a / b };
}
// + AllExceptionsFilter from Part 4, registered with APP_FILTER
```

```ts
// Challenge: main.ts → app.use(fileLogger)
import { appendFile } from 'fs';

export function fileLogger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  const start = Date.now();
  res.on('finish', () => {
    const line = `${new Date().toISOString()} ${req.method} ${req.url} ${res.statusCode} ${Date.now() - start}ms\n`;
    appendFile('logs.txt', line, (err) => {
      if (err) console.error('Could not write log:', err.message);
    });
  });
  next();
}
```

Middleware runs for **every** request, even unknown routes. Interceptors only run when a controller method matched, so 404s for unknown URLs would be missing from the log.

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

1. Draw the request lifecycle. Where can a request be stopped, and with which status?
2. What happens if middleware forgets `next()`?
3. Guard vs pipe vs interceptor vs filter — one job each.
4. Why `whitelist: true` on the `ValidationPipe`? What attack does it block?
5. Why register global filters/guards/interceptors with `APP_FILTER` / `APP_GUARD` / `APP_INTERCEPTOR` instead of `new` in `main.ts`?
6. Documents vs collections vs databases — hierarchy?
7. What does `{ price: { $gt: 50 } }` mean? Write "price between 20 and 100 OR category books".
8. What happens without `$set` in `updateOne`?
9. Embed vs reference — when does each fit?
10. What do projection, `sort`, `skip`, `limit` do?

## Common Mistakes

- Middleware that never calls `next()`
- DTO decorators without a global `ValidationPipe` (they do nothing)
- Throwing plain `Error` for expected problems → 500 instead of 400/404
- Writing a guard but forgetting `@UseGuards(...)`
- `enableCors()` with no `origin` in production
- Using middleware for auth checks (use guards) or for validation (use pipes)
- `updateOne` without `$set`; vague delete filters (`deleteOne({})`)
- Treating MongoDB relationally — over-normalizing
- Hardcoding the Atlas URI instead of `.env`

---

**Session 9 complete.** Next: Session 10 — Mongoose with NestJS & REST API CRUD.
