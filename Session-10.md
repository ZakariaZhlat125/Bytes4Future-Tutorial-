# Session 10: Mongoose with NestJS & REST API CRUD

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 9 (request lifecycle, pipes, MongoDB shell CRUD)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> The exercises build one project step by step: a **Library API** with a `Book` schema.

## Session Overview

- **Part 1:** Mongoose (ODM) and `@nestjs/mongoose`
- **Part 2:** Environment variables with `@nestjs/config`
- **Part 3:** Schemas with decorators — types, defaults, validation
- **Part 4:** NestJS feature architecture — module, controller, service, schema, DTO
- **Part 5:** Full CRUD with correct status codes
- **Lab:** Convert the in-memory to-do API to MongoDB-backed

---

## Part 1: Mongoose & ODM

> 💡 **In simple words:** MongoDB accepts any document, even one with typos or missing fields. Mongoose adds rules on top: you describe what a document must look like (a **schema**), and Mongoose checks every save. It also gives you easy functions like `bookModel.find()`. The package `@nestjs/mongoose` connects Mongoose to NestJS, so you can **inject** a model into a service.

**Mongoose** is an ODM (Object Data Modeling) library — it maps JS objects to MongoDB documents and adds what raw MongoDB lacks: **schemas** (enforced shape), **validation**, instance/static methods, middleware hooks, and a cleaner API.

```bash
npm install @nestjs/mongoose mongoose
```

```ts
// src/app.module.ts — the quickest connection (we improve it in Part 2)
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
  imports: [
    MongooseModule.forRoot('mongodb://127.0.0.1:27017/library'),
  ],
})
export class AppModule {}
```

When the app starts you see `MongooseModule dependencies initialized`. If MongoDB is not running, NestJS retries a few times and then **stops the app** with an error. That is what we want: the API must not run without its database.

**Schema vs model:** a **schema** defines document shape; a **model** is the compiled class bound to a collection (`Book` model → `books` collection). Models are what you query with.

---

## Part 2: Environment Variables

> 💡 **In simple words:** Environment variables are settings that live **outside** your code: passwords, secret keys, database addresses. The code reads them from a `.env` file on your computer and from the platform's settings on the server. That way you can push code to GitHub without pushing secrets.

Secrets (DB URIs, API keys, JWT secrets) belong in `.env` — never in code, never in git.

```bash
npm install @nestjs/config
```

```bash
# .env                        ← gitignored
MONGODB_URI=mongodb+srv://user:pass@cluster0.mongodb.net/library
PORT=3000
JWT_SECRET=change-me-in-prod

# .env.example                ← committed, documents required vars
MONGODB_URI=
PORT=
JWT_SECRET=
```

```ts
// src/app.module.ts
import { ConfigModule, ConfigService } from '@nestjs/config';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),          // loads .env; usable in every module

    MongooseModule.forRootAsync({                      // "Async" = wait for the config first
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        uri: config.get<string>('MONGODB_URI'),
      }),
    }),

    BooksModule,
  ],
})
export class AppModule {}
```

Read a value anywhere by injecting `ConfigService`:

```ts
constructor(private readonly config: ConfigService) {}
const secret = this.config.get<string>('JWT_SECRET');
```

**Rules:** `.env` in `.gitignore`; `.env.example` documents the shape; use `forRootAsync` so the URI is read **after** `.env` is loaded; separate dev/prod databases; rotate credentials.

---

### ✍️ Your Turn: Parts 1 & 2

Start the Library API: `nest new library-api`, then `npm install @nestjs/mongoose mongoose @nestjs/config class-validator class-transformer`.

```bash
# .env
MONGODB_URI=____        # your Atlas string, or local: mongodb://127.0.0.1:27017/library
PORT=3000
```

```bash
# .gitignore: which line must you ADD? (node_modules and dist are already there)
____
```

```ts
// src/app.module.ts
import { ____, ConfigService } from '@nestjs/config';
import { ____ } from '@nestjs/mongoose';

@Module({
  imports: [
    ConfigModule.____({ isGlobal: ____ }),
    MongooseModule.____({
      inject: [____],
      useFactory: (config: ConfigService) => ({
        uri: config.get<string>('____'),
      }),
    }),
  ],
})
export class AppModule {}
```

```ts
// src/main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
  await app.listen(process.env.____ ?? 3000);
}
```

**TODO:** create `.env.example` with the variable names but no values, and commit it. Then stop MongoDB (or write a wrong URI) and start the app. What happens?

> Use `127.0.0.1` instead of `localhost` for a local MongoDB. On newer Node versions, `localhost` may resolve to IPv6 and fail to connect.

<details>
<summary>✅ Solution</summary>

```bash
# .gitignore
.env
```

```ts
@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),
    MongooseModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        uri: config.get<string>('MONGODB_URI'),
      }),
    }),
  ],
})
export class AppModule {}

await app.listen(process.env.PORT ?? 3000);
```

```bash
# .env.example
MONGODB_URI=
PORT=
```

With a wrong URI, NestJS logs `Unable to connect to the database. Retrying (1)...` several times and then stops. The API never runs without a database.

</details>

---

## Part 3: Schemas — Types, Defaults, Validation

> 💡 **In simple words:** A **schema** is the blueprint of a document: field names, their types, which ones are required, default values, and rules (min, max, allowed values). In NestJS you write it as a **class** with decorators: `@Schema()` on the class, `@Prop()` on each field. `SchemaFactory` turns the class into a real Mongoose schema.

```ts
// src/products/schemas/product.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument } from 'mongoose';

@Schema({ timestamps: true })                 // auto adds createdAt + updatedAt
export class Product {
  @Prop({ required: [true, 'Name is required'], trim: true })
  name: string;

  @Prop({ required: true, min: 0 })
  price: number;

  @Prop({ enum: ['electronics', 'books', 'clothing'] })
  category?: string;

  @Prop({ unique: true, lowercase: true })      // unique → index
  supplierEmail?: string;

  @Prop({ default: true })
  inStock: boolean;

  @Prop([String])                               // an array of strings
  tags: string[];
}

export type ProductDocument = HydratedDocument<Product>;   // a Product with _id, save(), ...
export const ProductSchema = SchemaFactory.createForClass(Product);
```

Register the schema in its feature module, then inject the model:

```ts
// src/products/products.module.ts
@Module({
  imports: [MongooseModule.forFeature([{ name: Product.name, schema: ProductSchema }])],
  controllers: [ProductsController],
  providers: [ProductsService],
})
export class ProductsModule {}

// src/products/products.service.ts
@Injectable()
export class ProductsService {
  constructor(@InjectModel(Product.name) private readonly productModel: Model<Product>) {}
}
```

**Field options:** `type` (`String Number Boolean Date [Array] ObjectId Mixed` — usually read from the TypeScript type), `required` (value or `[true, 'msg']`), `default`, `min`/`max`, `minlength`/`maxlength`, `enum`, `unique`, `match` (regex), `trim`, `lowercase`.

**Gotchas:** class names are singular (Mongoose pluralizes the collection: `Product` → `products`); for arrays and unions write the type explicitly (`@Prop([String])`, `@Prop({ type: String })`); schema validation is the *last* line — still validate input with DTOs (Session 9).

---

### ✍️ Your Turn: Part 3

```bash
nest g module books && nest g controller books && nest g service books
```

```ts
// src/books/schemas/book.schema.ts
import { Prop, ____, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument } from 'mongoose';

@Schema({ ____: true })                         // adds createdAt and updatedAt
export class Book {
  @____({ required: [true, 'Title is required'], trim: true })
  title: string;

  @Prop({ required: ____ })
  author: string;

  @Prop({ min: [1, 'A book needs at least 1 page'] })
  pages?: number;

  @Prop({ ____: ['novel', 'science', 'history', 'kids'] })
  genre?: string;

  @Prop({ ____: true })
  isAvailable: boolean;

  @Prop(____)                                   // a list of strings
  tags: string[];

  @Prop({ unique: ____, sparse: true })         // sparse: many books may have no isbn
  isbn?: string;
}

export type BookDocument = HydratedDocument<Book>;
export const BookSchema = SchemaFactory.____(Book);
```

```ts
// src/books/books.module.ts
@Module({
  imports: [MongooseModule.____([{ name: ____.name, schema: ____ }])],
  controllers: [BooksController],
  providers: [BooksService],
})
export class BooksModule {}
```

Test the schema with a temporary route in `BooksController`:

```ts
constructor(@InjectModel(Book.name) private readonly bookModel: Model<Book>) {}

@Post('try')
async tryIt() {
  // TODO 1: create a valid book with this.bookModel.create({...}) and return it
  // TODO 2: create a book with pages: 0. What error do you get in the terminal?
  // TODO 3: create a book with genre: 'sport'. What error do you get?
}
```

<details>
<summary>✅ Solution</summary>

```ts
@Schema({ timestamps: true })
export class Book {
  @Prop({ required: [true, 'Title is required'], trim: true })
  title: string;

  @Prop({ required: true })
  author: string;

  @Prop({ min: [1, 'A book needs at least 1 page'] })
  pages?: number;

  @Prop({ enum: ['novel', 'science', 'history', 'kids'] })
  genre?: string;

  @Prop({ default: true })
  isAvailable: boolean;

  @Prop([String])
  tags: string[];

  @Prop({ unique: true, sparse: true })
  isbn?: string;
}

export type BookDocument = HydratedDocument<Book>;
export const BookSchema = SchemaFactory.createForClass(Book);

@Module({
  imports: [MongooseModule.forFeature([{ name: Book.name, schema: BookSchema }])],
  controllers: [BooksController],
  providers: [BooksService],
})
export class BooksModule {}
```

```ts
return this.bookModel.create({ title: 'Clean Code', author: 'Robert Martin', pages: 464, genre: 'science' });
// → has _id, isAvailable: true, createdAt, updatedAt

await this.bookModel.create({ title: 'Bad', author: 'X', pages: 0 });
// ValidationError: Book validation failed: pages: A book needs at least 1 page

await this.bookModel.create({ title: 'Bad', author: 'X', genre: 'sport' });
// ValidationError: Book validation failed: genre: `sport` is not a valid enum value for path `genre`.
```

The client gets a 500 for these errors (a Mongoose `ValidationError` is not an `HttpException`). DTO validation (Part 5) stops bad input earlier with a 400, and in Session 11 the global filter turns Mongoose errors into 400s too. Delete the `try` route when you're done.

</details>

---

## Part 4: NestJS Feature Architecture

> 💡 **In simple words:** Every feature (books, users, orders) is a folder with the same files, and each file has one job:
> - **Schema**: the data and its rules (Mongoose).
> - **DTO**: the shape and rules of the **input**.
> - **Service**: the logic: talks to the model.
> - **Controller**: the map from URL to service method.
> - **Module**: ties them together and registers the schema.
>
> This is the NestJS version of the classic MVC idea. Small files with one job each are easier to read, test and fix.

```
src/
├── main.ts                     # prefix, global pipes, listen
├── app.module.ts               # ConfigModule + MongooseModule + feature modules
├── common/                     # shared pipes, guards, filters
│   └── pipes/parse-object-id.pipe.ts
└── todos/
    ├── dto/
    │   ├── create-todo.dto.ts
    │   └── update-todo.dto.ts
    ├── schemas/todo.schema.ts
    ├── todos.controller.ts     # URL → service
    ├── todos.service.ts        # logic + model
    └── todos.module.ts         # forFeature + controller + service
```

```ts
// todos.controller.ts — thin: only maps routes to the service
@Controller('todos')
export class TodosController {
  constructor(private readonly todosService: TodosService) {}

  @Get()            findAll()                                  { return this.todosService.findAll(); }
  @Get(':id')       findOne(@Param('id') id: string)           { return this.todosService.findOne(id); }
  @Post()           create(@Body() dto: CreateTodoDto)         { return this.todosService.create(dto); }
  @Patch(':id')     update(@Param('id') id: string, @Body() dto: UpdateTodoDto) { return this.todosService.update(id, dto); }
  @Delete(':id') @HttpCode(204) remove(@Param('id') id: string) { return this.todosService.remove(id); }
}
```

**Anti-patterns:** model queries inside the controller; one giant module for everything; services that read `@Req()` or know about HTTP headers (pass plain values instead).

### A pipe for MongoDB ids

> 💡 **In simple words:** MongoDB ids are 24 hex characters, like `665f1c2e8a1b2c3d4e5f6a7b`. If someone sends `/api/books/123`, Mongoose throws a `CastError` and the client gets a 500. A small pipe checks the id first and answers a clean 400.

```ts
// src/common/pipes/parse-object-id.pipe.ts
import { BadRequestException, Injectable, PipeTransform } from '@nestjs/common';

@Injectable()
export class ParseObjectIdPipe implements PipeTransform<string, string> {
  transform(value: string): string {
    if (!/^[a-f\d]{24}$/i.test(value)) throw new BadRequestException('Invalid id');
    return value;
  }
}

// usage
@Get(':id')
findOne(@Param('id', ParseObjectIdPipe) id: string) { ... }
```

---

### ✍️ Your Turn: Part 4

```ts
// src/books/books.controller.ts: only maps URLs to service methods
@Controller('____')
export class BooksController {
  constructor(private readonly ____: BooksService) {}

  @Get()
  findAll(@Query('genre') genre?: string) { return this.booksService.____(genre); }

  @Get(':id')
  findOne(@Param('id', ____) id: string) { return this.booksService.____(id); }

  @Post()
  create(@Body() dto: ____) { return this.booksService.create(dto); }

  @Patch(':id')
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: UpdateBookDto) { return this.booksService.____(id, dto); }

  @Delete(':id')
  @HttpCode(____)
  remove(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.remove(id); }
}
```

```ts
// src/books/dto/create-book.dto.ts
export class CreateBookDto {
  @IsString() @IsNotEmpty() title: string;
  @IsString() @IsNotEmpty() author: string;
  @IsOptional() @IsInt() @Min(1) pages?: number;
  @IsOptional() @IsIn(['novel', 'science', 'history', 'kids']) genre?: string;
}

// src/books/dto/update-book.dto.ts — every field optional (Session 11 shows a shortcut)
export class UpdateBookDto {
  // TODO: the same four fields, all with @IsOptional()
}
```

**Question:** where does each one go: **schema**, **DTO**, **service** or **controller**?
a) "pages must be at least 1" when **saving** · b) `@Get(':id')` · c) `await this.bookModel.findById(id)` · d) "title is required" in the **request body**

<details>
<summary>✅ Solution</summary>

```ts
@Controller('books')
export class BooksController {
  constructor(private readonly booksService: BooksService) {}

  @Get()
  findAll(@Query('genre') genre?: string) { return this.booksService.findAll(genre); }

  @Get(':id')
  findOne(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.findOne(id); }

  @Post()
  create(@Body() dto: CreateBookDto) { return this.booksService.create(dto); }

  @Patch(':id')
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: UpdateBookDto) { return this.booksService.update(id, dto); }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.remove(id); }
}

export class UpdateBookDto {
  @IsOptional() @IsString() @IsNotEmpty() title?: string;
  @IsOptional() @IsString() @IsNotEmpty() author?: string;
  @IsOptional() @IsInt() @Min(1) pages?: number;
  @IsOptional() @IsIn(['novel', 'science', 'history', 'kids']) genre?: string;
}
```

a) schema · b) controller · c) service · d) DTO

</details>

---

## Part 5: CRUD with Mongoose

> 💡 **In simple words:** Every service method follows the same recipe:
> 1. Get the input (already validated by the DTO).
> 2. Ask the model.
> 3. Handle "not found" by throwing `NotFoundException`.
> 4. Return the result. The controller returns it, and NestJS sends it with the right status.
>
> Database calls take time, so they are `async` and need `await`. You don't need `try/catch`: if anything throws, NestJS catches it.

```ts
// src/todos/todos.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Todo } from './schemas/todo.schema';

@Injectable()
export class TodosService {
  constructor(@InjectModel(Todo.name) private readonly todoModel: Model<Todo>) {}

  // READ all — with filtering, sorting, pagination
  findAll(completed?: boolean, page = 1, limit = 10) {
    const filter = completed !== undefined ? { completed } : {};
    return this.todoModel
      .find(filter)
      .sort({ createdAt: -1 })
      .skip((page - 1) * limit)
      .limit(limit);
  }

  // READ one
  async findOne(id: string) {
    const todo = await this.todoModel.findById(id);
    if (!todo) throw new NotFoundException('Todo not found');
    return todo;
  }

  // CREATE — schema validation runs here
  create(dto: CreateTodoDto) {
    return this.todoModel.create(dto);
  }

  // UPDATE — findByIdAndUpdate(id, data, options)
  async update(id: string, dto: UpdateTodoDto) {
    const todo = await this.todoModel.findByIdAndUpdate(id, dto, {
      new: true,               // return the UPDATED doc (default returns pre-update)
      runValidators: true,     // run schema validation on update
    });
    if (!todo) throw new NotFoundException('Todo not found');
    return todo;
  }

  // DELETE
  async remove(id: string) {
    const todo = await this.todoModel.findByIdAndDelete(id);
    if (!todo) throw new NotFoundException('Todo not found');
  }
}
```

```ts
// controller: query params converted by pipes
@Get()
findAll(
  @Query('completed', new ParseBoolPipe({ optional: true })) completed?: boolean,
  @Query('page', new DefaultValuePipe(1), ParseIntPipe) page?: number,
  @Query('limit', new DefaultValuePipe(10), ParseIntPipe) limit?: number,
) {
  return this.todosService.findAll(completed, page, limit);
}
```

### Model methods cheat sheet

> 💡 **In simple words:** Keep this table open while you code. Almost every API you build uses only these methods.

| Task | Method |
|------|--------|
| Create | `model.create(data)` · `new this.model(data).save()` |
| Read all | `model.find(filter)` |
| Read one | `model.findById(id)` · `model.findOne(filter)` |
| Update | `model.findByIdAndUpdate(id, data, { new: true, runValidators: true })` |
| Delete | `model.findByIdAndDelete(id)` |
| Count | `model.countDocuments(filter)` |
| Chain | `.sort() .limit() .skip() .select('name price')` |

### HTTP status codes

> 💡 **In simple words:** Send the right code, so the frontend knows what happened without having to read the message.

| Code | When | In NestJS |
|------|------|-----------|
| 200 | Success (GET/PUT/PATCH) | default |
| 201 | Created (POST) | default for `@Post()` |
| 204 | Success, no body (DELETE) | `@HttpCode(204)` |
| 400 | Validation error / bad id | `ValidationPipe`, `ParseObjectIdPipe`, `BadRequestException` |
| 401/403 | Unauthenticated / unauthorized | guards (Session 11) |
| 404 | Resource not found | `NotFoundException` |
| 409 | Conflict (already exists, already borrowed) | `ConflictException` |
| 500 | Server/DB error | anything unexpected |

---

### ✍️ Your Turn: Part 5

```ts
// src/books/books.service.ts
@Injectable()
export class BooksService {
  constructor(@____(Book.name) private readonly bookModel: ____<Book>) {}

  // GET /api/books?genre=novel
  findAll(genre?: string) {
    const filter = genre ? { genre } : {};
    return this.bookModel.____(filter).sort({ createdAt: ____ });   // newest first
  }

  // GET /api/books/:id
  async findOne(id: string) {
    const book = await this.bookModel.____(id);
    // TODO: throw NotFoundException('Book not found') if book is null
    return book;
  }

  // POST /api/books
  create(dto: CreateBookDto) {
    return this.bookModel.____(dto);
  }

  // PATCH /api/books/:id
  async update(id: string, dto: UpdateBookDto) {
    const book = await this.bookModel.findByIdAndUpdate(id, dto, {
      ____: true,          // return the UPDATED document
      ____: true,          // run the schema validation
    });
    // TODO: 404 if book is null
    return book;
  }

  // DELETE /api/books/:id
  async remove(id: string) {
    // TODO: findByIdAndDelete → 404 if null (the controller already has @HttpCode(204))
  }
}
```

**Test in Postman:** create 3 books → list → `?genre=novel` → get one → patch its pages → delete it → get it again (404). Then try `GET /api/books/123`. What happens, and why? Then try `POST` with `{ "title": "X" }` (no author).

<details>
<summary>✅ Solution</summary>

```ts
@Injectable()
export class BooksService {
  constructor(@InjectModel(Book.name) private readonly bookModel: Model<Book>) {}

  findAll(genre?: string) {
    const filter = genre ? { genre } : {};
    return this.bookModel.find(filter).sort({ createdAt: -1 });
  }

  async findOne(id: string) {
    const book = await this.bookModel.findById(id);
    if (!book) throw new NotFoundException('Book not found');
    return book;
  }

  create(dto: CreateBookDto) {
    return this.bookModel.create(dto);
  }

  async update(id: string, dto: UpdateBookDto) {
    const book = await this.bookModel.findByIdAndUpdate(id, dto, {
      new: true,
      runValidators: true,
    });
    if (!book) throw new NotFoundException('Book not found');
    return book;
  }

  async remove(id: string) {
    const book = await this.bookModel.findByIdAndDelete(id);
    if (!book) throw new NotFoundException('Book not found');
  }
}
```

- `GET /api/books/123` → **400 Invalid id**, from `ParseObjectIdPipe`. Without the pipe, Mongoose would throw a `CastError` and the client would get a 500.
- `POST { "title": "X" }` → **400** from the `ValidationPipe` (`author should not be empty`). The database is never touched.

> `findAll` returns the query without `await`. That's fine: NestJS waits for anything "awaitable" that a controller returns. Inside a method that **uses** the result (like `findOne`), you must `await`.

</details>

---

## Lab: MongoDB-Backed To-Do API

Convert the Session 8/9 in-memory API to a real database:

```
1. npm install @nestjs/mongoose mongoose @nestjs/config; add .env + .gitignore + .env.example
2. app.module.ts      → ConfigModule.forRoot({ isGlobal: true }) + MongooseModule.forRootAsync
3. todos/schemas/todo.schema.ts → { text: required String, completed: default false } + timestamps
4. todos/dto          → CreateTodoDto, UpdateTodoDto (class-validator)
5. todos.service.ts   → all 5 CRUD methods with the injected model; NotFoundException on null
6. todos.controller.ts→ map verbs to the service; ParseObjectIdPipe on :id; @HttpCode(204) on DELETE
7. main.ts            → prefix, helmet, CORS, ValidationPipe; AllExceptionsFilter via APP_FILTER
8. Test every endpoint in Postman: list, create, get, update, delete
```

**Challenges:**
- `GET /api/todos?completed=true&page=2&limit=5` — query-string filtering + pagination with pipes
- `runValidators: true` on update — remove the `@IsNotEmpty()` from `UpdateTodoDto`, send `{ "text": "" }`, and see the schema stop it
- `?fields=text,completed` → `.select('text completed')` — projection via query params

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is Mongoose, and what does it add on top of MongoDB? What does `@nestjs/mongoose` add?
2. What is the difference between a schema and a model?
3. If the schema class is named `Book`, what is the collection called?
4. Why do we keep the connection string in `.env`? Why do we commit `.env.example`?
5. Why do we use `MongooseModule.forRootAsync` with `ConfigService` instead of `forRoot(process.env.MONGODB_URI)`?
6. What does `@Schema({ timestamps: true })` add?
7. What does `MongooseModule.forFeature(...)` do, and where do you put it?
8. What do `new: true` and `runValidators: true` do in `findByIdAndUpdate`?
9. What does `findById` return when nothing is found? What must the service do then?
10. Which status code for: created · deleted · validation error · not found · already exists?

<details>
<summary>✅ Solution</summary>

1. Mongoose is an ODM that adds schemas, validation, defaults and an easy API for MongoDB. `@nestjs/mongoose` connects it to NestJS: schemas as decorated classes, and models you can inject with `@InjectModel`.
2. A schema describes the shape and rules of a document. A model is made from the schema, and we use it to create and query documents.
3. `books` (lowercase and plural).
4. It holds secrets that must not go to GitHub. `.env.example` shows other developers which variables they need, without the values.
5. `forRootAsync` waits until `ConfigModule` has loaded `.env`. With `forRoot(process.env...)` the value may be read too early and be `undefined`.
6. `createdAt` and `updatedAt` fields, filled in automatically.
7. It registers the schema as a model in that feature's module, so the module's services can inject it. It goes in the feature module's `imports`.
8. `new: true` returns the updated document (not the old one). `runValidators: true` checks the schema rules on update.
9. `null`. The service must `throw new NotFoundException(...)`.
10. 201 · 204 · 400 · 404 · 409.

</details>

### B. Find the Bug

```ts
// Bug 1: GET /api/books/<an id that doesn't exist> returns 200 with {} instead of 404
async findOne(id: string) {
  const book = this.bookModel.findById(id);
  if (!book) throw new NotFoundException('Book not found');
  return book;
}

// Bug 2: the response shows the OLD title after the update
return this.bookModel.findByIdAndUpdate(id, dto);

// Bug 3: "Nest can't resolve dependencies of the BooksService (?). Please make sure that
//         the argument "BookModel" at index [0] is available in the BooksModule context."
@Module({ controllers: [BooksController], providers: [BooksService] })
export class BooksModule {}

// Bug 4: PATCH with { "pages": -5 } is saved (the schema has min: 1, the DTO forgot @Min)
await this.bookModel.findByIdAndUpdate(id, dto, { new: true });

// Bug 5: the app crashes at start with "The `uri` parameter to `openUri()` must be a string, got undefined"
@Module({
  imports: [
    MongooseModule.forRoot(process.env.MONGODB_URI),
    ConfigModule.forRoot(),
  ],
})

// Bug 6: the team pushed .env to GitHub. What 2 things must you do now?
```

<details>
<summary>✅ Solution</summary>

```ts
// Bug 1: missing await. A Query object is always "truthy", so the 404 never happens.
const book = await this.bookModel.findById(id);

// Bug 2: add { new: true }
return this.bookModel.findByIdAndUpdate(id, dto, { new: true, runValidators: true });

// Bug 3: register the schema in the module
@Module({
  imports: [MongooseModule.forFeature([{ name: Book.name, schema: BookSchema }])],
  controllers: [BooksController],
  providers: [BooksService],
})

// Bug 4: validators don't run on update unless you ask (and add @Min(1) to the DTO too)
await this.bookModel.findByIdAndUpdate(id, dto, { new: true, runValidators: true });

// Bug 5: process.env is read before .env is loaded. Use the async version:
imports: [
  ConfigModule.forRoot({ isGlobal: true }),
  MongooseModule.forRootAsync({
    inject: [ConfigService],
    useFactory: (config: ConfigService) => ({ uri: config.get<string>('MONGODB_URI') }),
  }),
],
```

Bug 6: (1) Change the database password / secrets right away, because they are public now. (2) Add `.env` to `.gitignore` and remove it from git: `git rm --cached .env`.

</details>

### C. Coding Tasks

**Task 1: Contact schema (easy).** `contacts/schemas/contact.schema.ts` with:
- `name`: String, required, trim
- `email`: String, required, unique, lowercase, must match `/\S+@\S+\.\S+/`
- `phone`: String
- `favorite`: Boolean, default `false`
- timestamps

**Task 2: Contacts CRUD API (medium).** A `ContactsModule` (`nest g resource contacts`) with DTOs, a service and all 5 endpoints at `/api/contacts`, with the correct status codes (200/201/204/400/404). A duplicate email returns **409**.

**Task 3: Filter + pagination (medium).** `GET /api/contacts?favorite=true&page=2&limit=5` returns `{ data, total, page, totalPages }`.

**Task 4: Search (easy).** `GET /api/contacts?search=ali` finds contacts whose name contains the text (ignore case). Hint: `{ name: { $regex: search, $options: 'i' } }`.

**Task 5: Toggle favorite (easy).** `PATCH /api/contacts/:id/favorite` flips `favorite` from true to false or false to true and returns the contact.

**Challenge: Borrow and return books.** In the Library API add:
- `PATCH /api/books/:id/borrow`: if the book is not available → `409 'Book is already borrowed'` (`ConflictException`), otherwise set `isAvailable: false`
- `PATCH /api/books/:id/return`: the opposite, with `409 'Book is not borrowed'`
- `GET /api/books/available`: only available books (careful: this route must come **before** `:id`)

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```ts
// Task 1: contacts/schemas/contact.schema.ts
@Schema({ timestamps: true })
export class Contact {
  @Prop({ required: [true, 'Name is required'], trim: true })
  name: string;

  @Prop({
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    match: [/\S+@\S+\.\S+/, 'Email is invalid'],
  })
  email: string;

  @Prop()
  phone?: string;

  @Prop({ default: false })
  favorite: boolean;
}

export type ContactDocument = HydratedDocument<Contact>;
export const ContactSchema = SchemaFactory.createForClass(Contact);
```

```ts
// Task 2: contacts/dto/create-contact.dto.ts
export class CreateContactDto {
  @IsString() @IsNotEmpty() name: string;
  @IsEmail() email: string;
  @IsOptional() @IsString() phone?: string;
  @IsOptional() @IsBoolean() favorite?: boolean;
}

export class UpdateContactDto {
  @IsOptional() @IsString() @IsNotEmpty() name?: string;
  @IsOptional() @IsEmail() email?: string;
  @IsOptional() @IsString() phone?: string;
  @IsOptional() @IsBoolean() favorite?: boolean;
}
```

```ts
// Tasks 2–5: contacts/contacts.service.ts
@Injectable()
export class ContactsService {
  constructor(@InjectModel(Contact.name) private readonly contactModel: Model<Contact>) {}

  async findAll(query: { favorite?: string; search?: string; page?: string; limit?: string }) {
    const page = Math.max(Number(query.page) || 1, 1);
    const limit = Math.min(Number(query.limit) || 10, 50);

    const filter: Record<string, unknown> = {};
    if (query.favorite !== undefined) filter.favorite = query.favorite === 'true';
    if (query.search) filter.name = { $regex: query.search, $options: 'i' };

    const [data, total] = await Promise.all([
      this.contactModel.find(filter).sort({ name: 1 }).skip((page - 1) * limit).limit(limit),
      this.contactModel.countDocuments(filter),
    ]);

    return { data, total, page, totalPages: Math.ceil(total / limit) };
  }

  async findOne(id: string) {
    const contact = await this.contactModel.findById(id);
    if (!contact) throw new NotFoundException('Contact not found');
    return contact;
  }

  async create(dto: CreateContactDto) {
    try {
      return await this.contactModel.create(dto);
    } catch (err) {
      if ((err as { code?: number }).code === 11000) throw new ConflictException('Email already exists');
      throw err;
    }
  }

  async update(id: string, dto: UpdateContactDto) {
    const contact = await this.contactModel.findByIdAndUpdate(id, dto, { new: true, runValidators: true });
    if (!contact) throw new NotFoundException('Contact not found');
    return contact;
  }

  async remove(id: string) {
    const contact = await this.contactModel.findByIdAndDelete(id);
    if (!contact) throw new NotFoundException('Contact not found');
  }

  async toggleFavorite(id: string) {
    const contact = await this.findOne(id);
    contact.favorite = !contact.favorite;
    return contact.save();
  }
}
```

```ts
// contacts/contacts.controller.ts
@Controller('contacts')
export class ContactsController {
  constructor(private readonly contactsService: ContactsService) {}

  @Get()
  findAll(@Query() query: { favorite?: string; search?: string; page?: string; limit?: string }) {
    return this.contactsService.findAll(query);
  }

  @Get(':id')
  findOne(@Param('id', ParseObjectIdPipe) id: string) { return this.contactsService.findOne(id); }

  @Post()
  create(@Body() dto: CreateContactDto) { return this.contactsService.create(dto); }

  @Patch(':id/favorite')
  toggleFavorite(@Param('id', ParseObjectIdPipe) id: string) { return this.contactsService.toggleFavorite(id); }

  @Patch(':id')
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: UpdateContactDto) {
    return this.contactsService.update(id, dto);
  }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id', ParseObjectIdPipe) id: string) { return this.contactsService.remove(id); }
}

// contacts.module.ts
@Module({
  imports: [MongooseModule.forFeature([{ name: Contact.name, schema: ContactSchema }])],
  controllers: [ContactsController],
  providers: [ContactsService],
})
export class ContactsModule {}
```

> Here `@Query()` with a plain type works because we don't validate it (every value is a string). In Session 12 you'll write a proper query DTO with `class-validator` instead.

```ts
// Challenge: books.service.ts
findAvailable() {
  return this.bookModel.find({ isAvailable: true });
}

async borrow(id: string) {
  const book = await this.findOne(id);
  if (!book.isAvailable) throw new ConflictException('Book is already borrowed');
  book.isAvailable = false;
  return book.save();
}

async giveBack(id: string) {
  const book = await this.findOne(id);
  if (book.isAvailable) throw new ConflictException('Book is not borrowed');
  book.isAvailable = true;
  return book.save();
}

// books.controller.ts: 'available' BEFORE ':id', otherwise "available" is treated as an id
@Get('available')
findAvailable() { return this.booksService.findAvailable(); }

@Get(':id')
findOne(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.findOne(id); }

@Patch(':id/borrow')
borrow(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.borrow(id); }

@Patch(':id/return')
giveBack(@Param('id', ParseObjectIdPipe) id: string) { return this.booksService.giveBack(id); }
```

`return` is a reserved word in JavaScript, so the method is called `giveBack`.

</details>

---

## Review Questions

1. What does an ODM add over the raw MongoDB driver?
2. Schema vs model — what's the relationship? Where does `@InjectModel` come in?
3. Why does the app stop when the DB connection fails? Why is that good?
4. What do `{ new: true }` and `{ runValidators: true }` do on update?
5. Why does `.env` go in `.gitignore`? What's `.env.example` for?
6. Why keep model queries out of controllers?
7. When is 404 correct vs 400 vs 409?
8. How do you filter + paginate a `find()`? Why `Promise.all` with `countDocuments`?

## Common Mistakes

- Forgetting `await` before using a query result — the `if (!book)` check never fires
- `findByIdAndUpdate` without `new: true` → returns the stale document
- Missing `runValidators` → updates bypass schema validation
- Not throwing `NotFoundException` on `null` → 200 with an empty body
- Malformed ObjectId → `CastError` → 500; use `ParseObjectIdPipe` for a 400
- Forgetting `MongooseModule.forFeature` → "can't resolve dependencies … BookModel"
- `forRoot(process.env.X)` before config is loaded → `undefined` URI
- Hardcoding `MONGODB_URI` in source
- Schema validation as the *only* validation layer

---

**Session 10 complete.** Next: Session 11 — Validation, Error Handling & Authentication.
