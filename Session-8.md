# Session 8: Node.js Foundations, NestJS Setup & Controllers

**Duration:** 3 hours
**Level:** Beginner
**Prerequisites:** JavaScript ES6+, TypeScript basics (Session 1), command line, npm basics

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> **Testing your API:** GET routes work in the browser. For POST/PUT/PATCH/DELETE use **Postman** or the **Thunder Client** extension in VS Code, with the body type set to **JSON**.

## Session Overview

- **Part 1:** NestJS — setup, project anatomy, first controller, request/response
- **Part 2:** HTTP methods — GET, POST, PUT, PATCH, DELETE
- **Part 3:** Route params, query strings, request body
- **Part 4:** Modules, services (providers) and dependency injection
- **Lab:** In-memory CRUD API (to-do list)

---

## Part 1: NestJS Setup & First Server

> 💡 **In simple words:** A **server** is a program that waits for requests and sends back responses. In NestJS, a **controller** is a class whose methods answer requests. You put a decorator like `@Get('health')` on a method, and NestJS calls that method when a GET request comes to `/health`. Whatever the method **returns** is sent back to the client.

### Project setup

> 💡 **In simple words:** The Nest CLI is a command-line tool that creates the project and generates files for you. `npm run start:dev` starts the server in **watch mode**: it restarts automatically every time you save a file.

```bash
npm install -g @nestjs/cli     # the Nest CLI (once per computer)
nest new my-api                # choose npm when asked
cd my-api
npm run start:dev              # watch mode: restarts on every save
```

Open `http://localhost:3000` → `Hello World!`

### Project anatomy

```
my-api/
├── src/
│   ├── main.ts               # entry point: creates the app and starts listening
│   ├── app.module.ts         # the ROOT module: lists controllers + providers
│   ├── app.controller.ts     # routes (answers requests)
│   ├── app.service.ts        # logic (used by the controller)
│   └── app.controller.spec.ts# a unit test (Session 12)
├── test/                     # end-to-end tests (Session 12)
├── nest-cli.json
├── tsconfig.json
└── package.json              # scripts: start, start:dev, build, start:prod, test
```

| Script | What it does |
|--------|--------------|
| `npm run start:dev` | Development: compile + watch + restart on save |
| `npm run build` | Compile TypeScript into `dist/` |
| `npm run start:prod` | Production: `node dist/main` |

`.gitignore` already contains `node_modules/` and `dist/`. Add `.env` (Session 10). Never commit dependencies or secrets.

### First server

```ts
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');                 // every route starts with /api
  const port = process.env.PORT ?? 3000;      // env port for deployment, fallback for dev
  await app.listen(port);
  console.log(`Server running on http://localhost:${port}/api`);
}
bootstrap();
```

```ts
// src/app.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller()                                 // no path → routes start at /api
export class AppController {
  @Get()                                      // GET /api
  getHello(): string {
    return 'Hello NestJS';                    // a string is sent as text
  }

  @Get('health')                              // GET /api/health
  health() {
    return { status: 'ok', uptime: process.uptime() };   // an object is sent as JSON
  }
}
```

```ts
// src/app.module.ts — every controller must be listed in a module
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';

@Module({
  controllers: [AppController],
  providers: [],
})
export class AppModule {}
```

**Common mistakes:** a controller not listed in any module (all its routes return 404), forgetting `await app.listen`, hardcoding the port, forgetting the `api` prefix when testing (`/health` instead of `/api/health`).

### Request & response

> 💡 **In simple words:** In NestJS you don't dig into a big `req` object. You **ask** for the piece you need with a decorator in the method's parameters: `@Param('id')` for the path, `@Query('page')` for the part after `?`, `@Body()` for the content, `@Headers('authorization')` for a header. To answer, just `return` the data.

| Decorator | Gives you | Example URL / request |
|-----------|-----------|-----------------------|
| `@Param('id')` | Route placeholder | `/users/:id` → `'42'` |
| `@Query('page')` / `@Query()` | Query string value / all of it | `?page=2&q=abc` → `'2'` / `{ page: '2', q: 'abc' }` |
| `@Body()` | Parsed JSON body | `{"name":"x"}` |
| `@Headers('authorization')` | One request header | `Bearer eyJ...` |

| To… | In NestJS |
|-----|-----------|
| Send JSON | `return { ... }` or `return [ ... ]` |
| Set the success status | `@HttpCode(204)` on the method (defaults: 200, and **201 for POST**) |
| Send an error | `throw new NotFoundException('Todo not found')` |
| Redirect | `@Redirect('https://nestjs.com', 301)` |

**Rule:** for errors, **throw** an exception. NestJS turns it into the right status code and a JSON body:

```ts
import { Controller, Get, NotFoundException, Param } from '@nestjs/common';

@Get('todos/:id')
findOne(@Param('id') id: string) {
  const todo = todos.find(t => t.id === Number(id));
  if (!todo) throw new NotFoundException('Todo not found');   // → 404
  return todo;                                                 // → 200
}
```

```json
// The 404 response NestJS sends:
{ "statusCode": 404, "message": "Todo not found", "error": "Not Found" }
```

| Exception | Status |
|-----------|--------|
| `BadRequestException` | 400 |
| `UnauthorizedException` | 401 |
| `ForbiddenException` | 403 |
| `NotFoundException` | 404 |
| `ConflictException` | 409 |
| `InternalServerErrorException` | 500 |
| `ServiceUnavailableException` | 503 |

All of them are imported from `@nestjs/common`.

---

### ✍️ Your Turn: Part 1

Create the project (`nest new my-api`), then replace `src/app.controller.ts`:

```ts
import { Controller, ____ } from '@nestjs/common';

@____()
export class AppController {
  // 1. GET /api → the text "Welcome to my API"
  @Get()
  welcome() {
    ____ 'Welcome to my API';
  }

  // 2. TODO: GET /api/about → JSON { name: "My API", version: "1.0.0" }

  // 3. TODO: GET /api/time  → JSON { now: <current date as ISO string> }
}
```

```ts
// src/main.ts
async function bootstrap() {
  const app = await NestFactory.create(____);
  app.setGlobalPrefix('____');
  await app.listen(process.env.PORT ?? ____);
}
```

**4.** Delete `app.service.ts` and remove it from `app.module.ts` (we don't need it yet). Run `npm run start:dev`. Change a message and save: does the server restart by itself?

<details>
<summary>✅ Solution</summary>

```ts
import { Controller, Get } from '@nestjs/common';

@Controller()
export class AppController {
  @Get()
  welcome() {
    return 'Welcome to my API';
  }

  @Get('about')
  about() {
    return { name: 'My API', version: '1.0.0' };
  }

  @Get('time')
  time() {
    return { now: new Date().toISOString() };
  }
}
```

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');
  await app.listen(process.env.PORT ?? 3000);
}
```

```ts
// app.module.ts
@Module({ controllers: [AppController], providers: [] })
export class AppModule {}
```

Yes: `start:dev` watches the files and restarts on save.

</details>

---

## Part 2: HTTP Methods & REST

> 💡 **In simple words:** This is the same idea as Session 4, now from the server's side: the server decides what each method does and which status code it answers with. In NestJS each method has its own decorator: `@Get`, `@Post`, `@Put`, `@Patch`, `@Delete`.

| Method | Decorator | Purpose | Success status | Notes |
|--------|-----------|---------|----------------|-------|
| `GET` | `@Get()` | Read | 200 | Safe + idempotent; no body; never mutate state |
| `POST` | `@Post()` | Create | 201 (Nest default) | Body carries the new resource |
| `PUT` | `@Put(':id')` | Replace (full update) | 200 | Idempotent |
| `PATCH` | `@Patch(':id')` | Update (partial) | 200 | Only provided fields change |
| `DELETE` | `@Delete(':id')` | Remove | 204 (`@HttpCode(204)`) or 200 | 204 = no content |

```ts
// src/todos/todos.controller.ts
import { Body, Controller, Delete, Get, HttpCode, NotFoundException, Param, Patch, Post, Put } from '@nestjs/common';

interface Todo { id: number; text: string; completed: boolean }

let nextId = 3;
const todos: Todo[] = [
  { id: 1, text: 'Learn NestJS', completed: false },
  { id: 2, text: 'Build API', completed: false },
];

function find(id: string): Todo {
  const todo = todos.find(t => t.id === Number(id));
  if (!todo) throw new NotFoundException('Todo not found');
  return todo;
}

@Controller('todos')                            // all routes start with /api/todos
export class TodosController {
  @Get()
  findAll() {
    return todos;
  }

  // CREATE → 201 automatically, return the new resource
  @Post()
  create(@Body() body: { text: string }) {
    const todo = { id: nextId++, text: body.text, completed: false };
    todos.push(todo);
    return todo;
  }

  // UPDATE (full vs partial)
  @Put(':id')                                   // replace every field
  replace(@Param('id') id: string, @Body() body: { text: string; completed: boolean }) {
    const todo = find(id);
    todo.text = body.text;
    todo.completed = body.completed;
    return todo;
  }

  @Patch(':id')                                 // merge provided fields
  update(@Param('id') id: string, @Body() body: Partial<Todo>) {
    const todo = find(id);
    Object.assign(todo, body);
    return todo;
  }

  // DELETE → 204, return nothing
  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id') id: string) {
    const i = todos.findIndex(t => t.id === Number(id));
    if (i === -1) throw new NotFoundException('Todo not found');
    todos.splice(i, 1);
  }
}
```

Register it: `@Module({ controllers: [AppController, TodosController] })`.

**Best practices:** GET never mutates; always 404 on missing resources; compare IDs with `Number()`/`String()` to avoid type bugs; validate the body before creating (by hand today, with DTOs + `ValidationPipe` in Session 9).

---

### ✍️ Your Turn: Part 2

Create `src/books/books.controller.ts` (data in memory) and add it to `AppModule`.

```ts
import { Body, Controller, Delete, Get, HttpCode, BadRequestException, NotFoundException, Param, Patch, Post } from '@nestjs/common';

interface Book { id: number; title: string; author: string }

let nextId = 3;
const books: Book[] = [
  { id: 1, title: 'Clean Code', author: 'Robert Martin' },
  { id: 2, title: 'Atomic Habits', author: 'James Clear' },
];

@Controller('____')
export class BooksController {
  // READ all
  @____()
  findAll() {
    return books;
  }

  // READ one
  @Get('____')
  findOne(@____('id') id: string) {
    const book = books.find(b => b.id === ____(id));
    // TODO: throw NotFoundException('Book not found') if missing, otherwise return the book
  }

  // CREATE
  @____()
  create(@____() body: { title?: string; author?: string }) {
    // TODO: if title is missing → throw BadRequestException('title is required')
    const book = { id: nextId++, title: body.title!, author: body.author ?? '' };
    books.push(book);
    return book;                 // which status code does the client get?
  }

  // DELETE
  @Delete(':id')
  @HttpCode(____)
  remove(@Param('id') id: string) {
    const index = books.findIndex(b => b.id === Number(id));
    if (index === -1) throw new ____('Book not found');
    books.splice(index, 1);
  }

  // TODO: PATCH /api/books/:id → change only the fields that were sent (404 if missing)
}
```

**Test in Postman:** list → create → get the new one → patch its title → delete it → get it again (should be 404).

<details>
<summary>✅ Solution</summary>

```ts
@Controller('books')
export class BooksController {
  @Get()
  findAll() {
    return books;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    const book = books.find(b => b.id === Number(id));
    if (!book) throw new NotFoundException('Book not found');
    return book;
  }

  @Post()
  create(@Body() body: { title?: string; author?: string }) {
    if (!body.title) throw new BadRequestException('title is required');
    const book = { id: nextId++, title: body.title, author: body.author ?? '' };
    books.push(book);
    return book;                 // 201 Created (the default for POST)
  }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id') id: string) {
    const index = books.findIndex(b => b.id === Number(id));
    if (index === -1) throw new NotFoundException('Book not found');
    books.splice(index, 1);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() body: { title?: string; author?: string }) {
    const book = books.find(b => b.id === Number(id));
    if (!book) throw new NotFoundException('Book not found');
    if (body.title !== undefined) book.title = body.title;
    if (body.author !== undefined) book.author = body.author;
    return book;
  }
}
```

</details>

---

## Part 3: Params, Query, Body

> 💡 **In simple words:** There are three ways the client sends information:
> - **Params** say **which** thing: `/users/42`.
> - **Query** adds options: `?page=2&sort=name`.
> - **Body** carries the data to save, like the new user's name and email.
>
> Params and query values always arrive as **strings**. A **pipe** like `ParseIntPipe` converts them for you, and answers 400 automatically if the value is not a number.

```ts
import { Body, Controller, Get, Param, ParseIntPipe, Post, Query } from '@nestjs/common';

@Controller()
export class DemoController {
  // Route params — identify a resource: /api/users/42
  @Get('users/:id')
  findUser(@Param('id', ParseIntPipe) id: number) {   // '42' → 42, 'abc' → 400
    return { id, type: typeof id };                  // { id: 42, type: 'number' }
  }

  // Query strings — optional modifiers: /api/todos?completed=true
  @Get('todos')
  findTodos(@Query('completed') completed?: string) {
    if (completed === undefined) return todos;
    return todos.filter(t => t.completed === (completed === 'true'));
  }

  // Body — structured payloads for POST/PUT/PATCH
  @Post('echo')
  echo(@Body() body: Record<string, unknown>) {
    return body;
  }
}
```

### DTOs: describing the body with a class

> 💡 **In simple words:** A **DTO** (Data Transfer Object) is a small class that describes what the client must send. Today it only gives you TypeScript types. In Session 9 you add validation rules to it, and NestJS checks every request automatically.

```ts
// src/books/dto/create-book.dto.ts
export class CreateBookDto {
  title: string;
  author: string;
  pages?: number;
}

// in the controller
@Post()
create(@Body() dto: CreateBookDto) { /* dto.title is typed */ }
```

- Params = *which* resource; query = *how* to return it (filter/sort/paginate/search)
- Both arrive as strings — convert with a pipe (`ParseIntPipe`) or `Number()`
- Provide defaults for optional query params (`DefaultValuePipe` in Session 9)
- Use a class (DTO) for the body, not an inline type, once it has more than 2 fields

---

### ✍️ Your Turn: Part 3

Add these to a new `DemoController` (and register it in `AppModule`).

```ts
// 1. Route params: GET /api/greet/Sara → { message: "Hello, Sara!" }
@Get('greet/:____')
greet(@Param('____') name: string) {
  return { message: `Hello, ${name}!` };
}

// 2. Query string: GET /api/sum?a=5&b=7 → { result: 12 }
@Get('sum')
sum(@Query('a') a: string, @Query('b') b: string) {
  const x = ____(a);
  const y = ____(b);
  // TODO: if x or y is not a number → throw BadRequestException('a and b must be numbers')
  //       hint: Number.isNaN(x)
  return { result: ____ };
}

// 3. Body: POST /api/login with { "username": "admin", "password": "1234" }
class LoginDto {
  username: string;
  password: string;
}

@Post('login')
@HttpCode(200)                       // login doesn't create anything, so not 201
login(@____() dto: LoginDto) {
  // TODO: correct → { message: 'Welcome admin' }
  //       wrong   → throw UnauthorizedException('Wrong username or password')
}

// 4. TODO (in BooksController): GET /api/books?author=james → only books whose author contains
//    the text (ignore upper/lower case). Without ?author → all books.
//    Change your findAll() from Part 2.

// 5. TODO: GET /api/square/:n → { result: n * n } using ParseIntPipe.
//    What does /api/square/abc return?
```

<details>
<summary>✅ Solution</summary>

```ts
@Get('greet/:name')
greet(@Param('name') name: string) {
  return { message: `Hello, ${name}!` };
}

@Get('sum')
sum(@Query('a') a: string, @Query('b') b: string) {
  const x = Number(a);
  const y = Number(b);
  if (Number.isNaN(x) || Number.isNaN(y)) {
    throw new BadRequestException('a and b must be numbers');
  }
  return { result: x + y };
}

@Post('login')
@HttpCode(200)
login(@Body() dto: LoginDto) {
  if (dto.username === 'admin' && dto.password === '1234') {
    return { message: 'Welcome admin' };
  }
  throw new UnauthorizedException('Wrong username or password');
}

// BooksController
@Get()
findAll(@Query('author') author?: string) {
  if (!author) return books;
  return books.filter(b => b.author.toLowerCase().includes(author.toLowerCase()));
}

@Get('square/:n')
square(@Param('n', ParseIntPipe) n: number) {
  return { result: n * n };
}
```

5. `/api/square/abc` → `400 { "message": "Validation failed (numeric string is expected)", "error": "Bad Request", "statusCode": 400 }`. The pipe rejects it before your method runs.

</details>

---

## Part 4: Modules, Services & Dependency Injection

### Services (providers)

> 💡 **In simple words:** A controller should only deal with HTTP: read the input and return the answer. The real work (finding, saving, calculating) goes into a **service**. The controller doesn't create the service with `new`. It just **asks** for it in its constructor, and NestJS hands it over. This is called **dependency injection**.

```ts
// src/todos/todos.service.ts — logic + data
import { Injectable, NotFoundException } from '@nestjs/common';

@Injectable()                                   // NestJS can inject this class
export class TodosService {
  private nextId = 3;
  private todos = [
    { id: 1, text: 'Learn NestJS', completed: false },
    { id: 2, text: 'Build API', completed: false },
  ];

  findAll() {
    return this.todos;
  }

  findOne(id: number) {
    const todo = this.todos.find(t => t.id === id);
    if (!todo) throw new NotFoundException('Todo not found');
    return todo;
  }

  create(text: string) {
    const todo = { id: this.nextId++, text, completed: false };
    this.todos.push(todo);
    return todo;
  }
}
```

```ts
// src/todos/todos.controller.ts — HTTP only
@Controller('todos')
export class TodosController {
  constructor(private readonly todosService: TodosService) {}   // injected by NestJS

  @Get()
  findAll() {
    return this.todosService.findAll();
  }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.todosService.findOne(id);
  }

  @Post()
  create(@Body() dto: CreateTodoDto) {
    return this.todosService.create(dto.text);
  }
}
```

### Modules — one per feature

> 💡 **In simple words:** A **module** is a box for one feature (todos, books, users). It lists the feature's controllers and services. The root `AppModule` then imports each feature module. The result is a tidy app where every feature lives in its own folder.

```ts
// src/todos/todos.module.ts
import { Module } from '@nestjs/common';
import { TodosController } from './todos.controller';
import { TodosService } from './todos.service';

@Module({
  controllers: [TodosController],
  providers: [TodosService],
  exports: [TodosService],          // only if OTHER modules need this service
})
export class TodosModule {}

// src/app.module.ts
@Module({ imports: [TodosModule] })
export class AppModule {}
```

### Generate it with the CLI

```bash
nest g resource todos        # choose "REST API" and "Yes" for CRUD entry points
# creates todos.module.ts, todos.controller.ts, todos.service.ts, dto/, entities/
# and adds TodosModule to AppModule for you
nest g module books          # or one piece at a time
nest g controller books
nest g service books
```

### Scalable structure

> 💡 **In simple words:** Each feature has its own folder, and inside it every file has one job:
> - **controller**: which URL and what to return.
> - **service**: the logic and the data.
> - **dto**: the shape of the input.
> - **module**: ties them together.
>
> You'll add **schemas** (database models, Session 10), **guards** (auth, Session 11) and more in the next sessions.

```
my-api/src/
├── main.ts                 # creates the app, global prefix, listen
├── app.module.ts           # imports every feature module
├── todos/
│   ├── dto/create-todo.dto.ts
│   ├── todos.controller.ts
│   ├── todos.service.ts
│   └── todos.module.ts
├── books/                  # same pattern
└── common/                 # shared pipes, guards, filters (Sessions 9–11)
```

**Watch mode** (`npm run start:dev`) restarts the server on every save. **Dev only** — production runs the compiled code with `npm run start:prod`.

---

### ✍️ Your Turn: Part 4

Move the books logic out of the controller into a service, and the whole feature into its own module.

```ts
// src/books/books.service.ts
import { ____, NotFoundException } from '@nestjs/common';

@____()
export class BooksService {
  private nextId = 3;
  private books = [
    { id: 1, title: 'Clean Code', author: 'Robert Martin' },
    { id: 2, title: 'Atomic Habits', author: 'James Clear' },
  ];

  findAll() { /* TODO */ }
  findOne(id: number) { /* TODO: 404 if missing */ }
  create(title: string, author: string) { /* TODO */ }
}
```

```ts
// src/books/books.controller.ts
@Controller('books')
export class BooksController {
  constructor(private readonly ____: BooksService) {}

  @Get()
  findAll() { return this.booksService.____(); }

  @Get(':id')
  findOne(@Param('id', ____) id: number) { return this.booksService.findOne(id); }

  @Post()
  create(@Body() dto: CreateBookDto) { /* TODO: 400 if no title, then call the service */ }
}
```

```ts
// src/books/books.module.ts
@Module({
  ____: [BooksController],
  ____: [BooksService],
})
export class BooksModule {}

// src/app.module.ts
@Module({ ____: [BooksModule] })
export class AppModule {}
```

**Question:** remove `BooksService` from `providers` and save. What error do you get, and what does it mean?

<details>
<summary>✅ Solution</summary>

```ts
@Injectable()
export class BooksService {
  private nextId = 3;
  private books = [
    { id: 1, title: 'Clean Code', author: 'Robert Martin' },
    { id: 2, title: 'Atomic Habits', author: 'James Clear' },
  ];

  findAll() {
    return this.books;
  }

  findOne(id: number) {
    const book = this.books.find(b => b.id === id);
    if (!book) throw new NotFoundException('Book not found');
    return book;
  }

  create(title: string, author: string) {
    const book = { id: this.nextId++, title, author };
    this.books.push(book);
    return book;
  }
}

@Controller('books')
export class BooksController {
  constructor(private readonly booksService: BooksService) {}

  @Get()
  findAll() { return this.booksService.findAll(); }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) { return this.booksService.findOne(id); }

  @Post()
  create(@Body() dto: CreateBookDto) {
    if (!dto.title) throw new BadRequestException('title is required');
    return this.booksService.create(dto.title, dto.author);
  }
}

@Module({ controllers: [BooksController], providers: [BooksService] })
export class BooksModule {}

@Module({ imports: [BooksModule] })
export class AppModule {}
```

The error is: `Nest can't resolve dependencies of the BooksController (?). Please make sure that the argument BooksService at index [0] is available in the BooksModule context.` NestJS can only inject classes that are listed in `providers` (or exported by an imported module).

</details>

---

## Lab: In-Memory CRUD To-Do API

**Requirements:**

```
GET    /api/todos           list all (+ ?completed=true filter)
GET    /api/todos/:id       one todo (404 if missing, 400 if the id is not a number)
POST   /api/todos           create (validate `text`) → 201
PUT    /api/todos/:id       full replace
PATCH  /api/todos/:id       partial update
DELETE /api/todos/:id       delete → 204
```

```bash
nest g resource todos --no-spec     # REST API, yes to CRUD entry points
```

```ts
// src/todos/todos.service.ts — starter
@Injectable()
export class TodosService {
  private nextId = 3;
  private todos = [
    { id: 1, text: 'Learn NestJS', completed: false },
    { id: 2, text: 'Build API', completed: false },
  ];

  findAll(completed?: string) {
    if (completed === undefined) return this.todos;
    return this.todos.filter(t => t.completed === (completed === 'true'));
  }

  findOne(id: number) {
    const todo = this.todos.find(t => t.id === id);
    if (!todo) throw new NotFoundException('Todo not found');
    return todo;
  }

  create(dto: CreateTodoDto) {
    if (!dto.text) throw new BadRequestException('text is required');
    const todo = { id: this.nextId++, text: dto.text, completed: false };
    this.todos.push(todo);
    return todo;
  }

  // implement replace, update, remove…
}
```

**Challenges:**
1. All logic in `TodosService`; `TodosController` only reads input and calls the service
2. `ParseIntPipe` on every `:id`
3. `@HttpCode(204)` on DELETE, and try an unknown route like `/api/nothing`: what does NestJS return? (A 404 JSON: you get it for free.)

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is Node.js? Why can it run JavaScript outside the browser?
2. Why do we use NestJS instead of the built-in `http` module?
3. What does a **controller** do? What does a **service** do?
4. What happens to the value a controller method returns?
5. Which HTTP method and status code do you use to **create** something? To **delete** something? How do you set 204 in NestJS?
6. In `/api/users/42?sort=name`, which decorator gives you `42` and which gives you `name`?
7. What does `ParseIntPipe` do?
8. How do you send a 404 error in NestJS?
9. What is a **module** used for?
10. What is dependency injection? Where do you see it in a controller?

<details>
<summary>✅ Solution</summary>

1. A program that runs JavaScript outside the browser. It uses Chrome's V8 engine.
2. NestJS gives routing with decorators, a clear project structure (modules, controllers, services), TypeScript and dependency injection, so you write much less code and every project looks the same.
3. A controller answers HTTP requests (which URL, what to return). A service holds the logic and the data.
4. NestJS sends it as the response: objects and arrays as JSON, strings as text.
5. Create: `POST` → `201` (the default). Delete: `DELETE` → `204`, set with `@HttpCode(204)`.
6. `@Param('id')` gives `'42'`, `@Query('sort')` gives `'name'`.
7. It converts the string to a number, and answers 400 automatically if it is not a number.
8. `throw new NotFoundException('message')`.
9. To group the controllers and services of one feature, so the app is split into clear parts.
10. NestJS creates the objects a class needs and passes them in. In a controller: `constructor(private readonly booksService: BooksService) {}`.

</details>

### B. Find the Bug

```ts
// Bug 1: `dto` is always undefined
@Post()
create(dto: CreateBookDto) {
  return this.booksService.create(dto.title, dto.author);
}

// Bug 2: GET /api/users/1 always returns 404
@Get(':id')
findOne(@Param('id') id: string) {
  const user = users.find(u => u.id === id);       // users have numeric ids
  if (!user) throw new NotFoundException('Not found');
  return user;
}

// Bug 3: GET /api/users/99 (does not exist) returns 200 with an empty body
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  return users.find(u => u.id === id);
}

// Bug 4: every route of this controller returns 404
@Controller('reports')
export class ReportsController {
  @Get()
  findAll() { return []; }
}
// app.module.ts
@Module({ controllers: [AppController] })
export class AppModule {}

// Bug 5: "Nest can't resolve dependencies of the BooksController (?)"
@Module({ controllers: [BooksController] })
export class BooksModule {}

// Bug 6: works on my laptop, but not after deploying
await app.listen(3000);
```

<details>
<summary>✅ Solution</summary>

```ts
// Bug 1: the parameter needs the @Body() decorator
create(@Body() dto: CreateBookDto) { ... }

// Bug 2: params are strings; convert them
findOne(@Param('id', ParseIntPipe) id: number) {
  const user = users.find(u => u.id === id);
  ...
}

// Bug 3: find() returns undefined → NestJS sends 200 with no body. Throw instead:
const user = users.find(u => u.id === id);
if (!user) throw new NotFoundException('User not found');
return user;

// Bug 4: the controller is not registered in any module
@Module({ controllers: [AppController, ReportsController] })

// Bug 5: the service is missing from providers
@Module({ controllers: [BooksController], providers: [BooksService] })

// Bug 6: hosting platforms give the port in an environment variable
await app.listen(process.env.PORT ?? 3000);
```

</details>

### C. Coding Tasks

**Task 1: Calculator API (easy).** `GET /api/calc/:op?a=10&b=5` where `op` is `add`, `subtract`, `multiply` or `divide` → `{ result }`. Return 400 if `a`/`b` are not numbers or when dividing by zero, and 404 for an unknown `op`.

**Task 2: Students API (medium).** A `StudentsModule` (controller + service) with full CRUD in memory for `{ id, name, grade }`:
- `GET /api/students` (optional `?minGrade=50`), `GET /api/students/:id`
- `POST /api/students` and `PUT /api/students/:id`: `name` is required and `grade` must be a number from 0 to 100, otherwise 400
- `DELETE /api/students/:id` → 204
- 404 for unknown ids, 400 for ids that are not numbers

**Task 3: Products module (medium).** Generate a `products` module, controller and service with the CLI. `GET /api/products` supports `?category=books` and `?sort=price` (cheapest first).

**Task 4: Quotes API (easy).** `GET /api/quotes` (all), `GET /api/quotes/random` (one random quote), `POST /api/quotes` with `{ text }` (400 if empty). Careful: `random` is not an id.

**Challenge: Notes with search + pagination.** Create 23 notes in memory. `GET /api/notes?q=1&page=2&limit=5` returns `{ data, total, page }`. `q` filters by text, `page` starts at 1, and `limit` defaults to 5 (max 50).

<details>
<summary>✅ Solution: Tasks 1–4 and Challenge</summary>

```ts
// Task 1: calc.controller.ts
@Controller('calc')
export class CalcController {
  @Get(':op')
  calc(@Param('op') op: string, @Query('a') a: string, @Query('b') b: string) {
    const x = Number(a);
    const y = Number(b);
    if (Number.isNaN(x) || Number.isNaN(y)) {
      throw new BadRequestException('a and b must be numbers');
    }
    if (op === 'add') return { result: x + y };
    if (op === 'subtract') return { result: x - y };
    if (op === 'multiply') return { result: x * y };
    if (op === 'divide') {
      if (y === 0) throw new BadRequestException('Cannot divide by zero');
      return { result: x / y };
    }
    throw new NotFoundException(`Unknown operation: ${op}`);
  }
}
```

```ts
// Task 2: students/dto/student.dto.ts
export class StudentDto {
  name: string;
  grade: number;
}

// students/students.service.ts
@Injectable()
export class StudentsService {
  private nextId = 3;
  private students = [
    { id: 1, name: 'Ali', grade: 85 },
    { id: 2, name: 'Mona', grade: 42 },
  ];

  private validate(dto: StudentDto = {} as StudentDto) {
    if (!dto.name || !dto.name.trim()) throw new BadRequestException('name is required');
    if (typeof dto.grade !== 'number' || dto.grade < 0 || dto.grade > 100) {
      throw new BadRequestException('grade must be a number from 0 to 100');
    }
  }

  findAll(minGrade?: string) {
    if (minGrade === undefined) return this.students;
    return this.students.filter(s => s.grade >= Number(minGrade));
  }

  findOne(id: number) {
    const student = this.students.find(s => s.id === id);
    if (!student) throw new NotFoundException('Student not found');
    return student;
  }

  create(dto: StudentDto) {
    this.validate(dto);
    const student = { id: this.nextId++, name: dto.name.trim(), grade: dto.grade };
    this.students.push(student);
    return student;
  }

  replace(id: number, dto: StudentDto) {
    const student = this.findOne(id);
    this.validate(dto);
    student.name = dto.name.trim();
    student.grade = dto.grade;
    return student;
  }

  remove(id: number) {
    const index = this.students.findIndex(s => s.id === id);
    if (index === -1) throw new NotFoundException('Student not found');
    this.students.splice(index, 1);
  }
}

// students/students.controller.ts
@Controller('students')
export class StudentsController {
  constructor(private readonly studentsService: StudentsService) {}

  @Get()
  findAll(@Query('minGrade') minGrade?: string) {
    return this.studentsService.findAll(minGrade);
  }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.studentsService.findOne(id);
  }

  @Post()
  create(@Body() dto: StudentDto) {
    return this.studentsService.create(dto);
  }

  @Put(':id')
  replace(@Param('id', ParseIntPipe) id: number, @Body() dto: StudentDto) {
    return this.studentsService.replace(id, dto);
  }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id', ParseIntPipe) id: number) {
    this.studentsService.remove(id);
  }
}
// students.module.ts → controllers: [StudentsController], providers: [StudentsService]
```

```ts
// Task 3: nest g module products && nest g controller products && nest g service products
@Injectable()
export class ProductsService {
  private products = [
    { id: 1, name: 'Laptop', price: 999, category: 'electronics' },
    { id: 2, name: 'Book', price: 15, category: 'books' },
    { id: 3, name: 'Mouse', price: 25, category: 'electronics' },
  ];

  findAll(category?: string, sort?: string) {
    let result = [...this.products];                       // copy before sorting
    if (category) result = result.filter(p => p.category === category);
    if (sort === 'price') result.sort((a, b) => a.price - b.price);
    return result;
  }
}

@Controller('products')
export class ProductsController {
  constructor(private readonly productsService: ProductsService) {}

  @Get()
  findAll(@Query('category') category?: string, @Query('sort') sort?: string) {
    return this.productsService.findAll(category, sort);
  }
}
```

```ts
// Task 4: 'random' must be declared BEFORE any ':id' route, or it would be read as an id
@Controller('quotes')
export class QuotesController {
  private quotes = ['Talk is cheap. Show me the code.', 'First, solve the problem. Then, write the code.'];

  @Get()
  findAll() {
    return this.quotes;
  }

  @Get('random')
  random() {
    const i = Math.floor(Math.random() * this.quotes.length);
    return { quote: this.quotes[i] };
  }

  @Post()
  create(@Body() body: { text?: string }) {
    const text = body?.text?.trim();
    if (!text) throw new BadRequestException('text is required');
    this.quotes.push(text);
    return { quote: text };
  }
}
```

```ts
// Challenge
@Controller('notes')
export class NotesController {
  private notes = Array.from({ length: 23 }, (_, i) => ({ id: i + 1, text: `Note number ${i + 1}` }));

  @Get()
  findAll(@Query('q') q = '', @Query('page') pageRaw?: string, @Query('limit') limitRaw?: string) {
    const page = Math.max(Number(pageRaw) || 1, 1);
    const limit = Math.min(Math.max(Number(limitRaw) || 5, 1), 50);

    const filtered = this.notes.filter(n => n.text.toLowerCase().includes(q.toLowerCase()));
    const start = (page - 1) * limit;

    return { data: filtered.slice(start, start + limit), total: filtered.length, page };
  }
}
```

</details>

---

## Review Questions

1. Which decorators read params, query, body and headers?
2. Why does `@Param('id')` need `ParseIntPipe` (or `Number()`) before comparing to `t.id`?
3. PUT vs PATCH — which for partial updates?
4. Why `201` for POST and `204` for DELETE? Which one does NestJS give by default?
5. Why should controllers stay thin and call services?
6. What is the job of a module? What happens if a controller is not in any module?
7. What is dependency injection, and what does `@Injectable()` mean?

## Common Mistakes

- Forgetting `@Body()` / `@Param()` → the parameter is `undefined`
- Returning `undefined` instead of throwing `NotFoundException` → 200 with an empty body
- Controller not listed in a module → 404 on every route; service not in `providers` → "can't resolve dependencies"
- String-vs-number ID comparisons silently failing (use `ParseIntPipe`)
- Putting logic and data in the controller instead of the service
- A fixed route (`random`) declared after `':id'`
- Using GET for mutations
- Hardcoding the port; committing `node_modules/`, `dist/` or `.env`

---

**Session 8 complete.** Next: Session 9 — The NestJS request lifecycle (middleware, pipes, guards, interceptors, filters) & MongoDB.
