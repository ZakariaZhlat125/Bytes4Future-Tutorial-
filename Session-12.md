# Session 12: Advanced Mongoose, Security, Testing & Deployment

**Duration:** 3 hours
**Level:** Advanced
**Prerequisites:** Sessions 8–11 (NestJS, MongoDB, Mongoose CRUD, auth)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> Keep building the **Library API** (Sessions 10–11) and add a small blog (`Post`) to practice relationships.

## Session Overview

- **Part 1:** Relationships — embed vs reference, `populate`
- **Part 2:** Advanced queries — query DTOs, pagination, search, indexes, aggregation
- **Part 3:** Security hardening — helmet, rate limiting (`@nestjs/throttler`), sanitization, CORS, config validation
- **Part 4:** Testing — unit tests with `Test.createTestingModule`, e2e tests with Supertest
- **Part 5:** Deployment — build, platform setup, Atlas prod, health check, checklist
- **Capstone:** Build & deploy a complete REST API

---

## Part 1: Relationships in MongoDB

> 💡 **In simple words:** Real data is connected: a post has an author, an order has a customer. You can connect data in two ways:
> - **Embed**: store the related data inside the document, like a comment inside its post.
> - **Reference**: store only the other document's id, like writing down the author's ID number.

Two modeling choices — pick based on access patterns:

| **Embed** (subdocuments) | **Reference** (`ObjectId` + `ref`) |
|--------------------------|-------------------------------------|
| Data read together, rarely changes | Shared data, updated independently |
| e.g., post + comments, user + addresses | e.g., post → author, order → user |
| One query gets everything | Needs `populate()`; avoids duplication |
| Watch 16MB doc limit | Many-to-one / many-to-many friendly |

```ts
// src/posts/schemas/post.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument, Types } from 'mongoose';
import { User } from '../../users/schemas/user.schema';

@Schema({ _id: false })                      // a subdocument: no own _id needed
export class Comment {
  @Prop({ required: true }) text: string;
  @Prop() name: string;
}
export const CommentSchema = SchemaFactory.createForClass(Comment);

@Schema({ timestamps: true })
export class Post {
  @Prop({ required: true })
  title: string;

  // Referencing: store the author's id
  @Prop({ type: Types.ObjectId, ref: User.name, required: true })
  author: Types.ObjectId;

  // Embedding: store the comments inside the post
  @Prop({ type: [CommentSchema], default: [] })
  comments: Comment[];

  @Prop([String])
  tags: string[];
}

export type PostDocument = HydratedDocument<Post>;
export const PostSchema = SchemaFactory.createForClass(Post);
```

### `populate()` — the MongoDB "join"

> 💡 **In simple words:** With a reference, the post stores only the author's id. `populate('author')` looks up that id and puts the real user document in the result instead, like following a link.

```ts
// posts.service.ts — replaces the ObjectId with the actual document
findPublished() {
  return this.postModel
    .find({ published: true })
    .populate('author', 'name email')      // ALWAYS limit fields
    .sort({ createdAt: -1 });
}

// nested: populate a ref inside an embedded array
.populate('comments.user', 'name')
```

**Rules:** `ref` must be set in the schema; handle `null` refs (deleted users); avoid deep populate chains; `.lean()` for read-only queries (returns plain objects, skips Mongoose overhead — faster).

---

### ✍️ Your Turn: Part 1

```bash
nest g resource posts --no-spec     # REST API, yes to CRUD entry points
```

```ts
// src/posts/schemas/post.schema.ts
@Schema({ timestamps: true })
export class Post {
  @Prop({ required: true })
  title: string;

  @Prop({ type: Types.____, ref: '____', required: true })   // reference
  author: Types.ObjectId;

  @Prop({ type: [CommentSchema], default: [] })              // embedded
  comments: Comment[];
}
```

```ts
// Try these in PostsService:

// 1. Create a post for the logged-in user (the id comes from the token)
create(title: string, authorId: string) {
  return this.postModel.create({ title, author: ____ });
}

// 2. Get the posts WITH the author's name and email (NOT the password!)
findAll() {
  return this.postModel.find().____('author', '____');
}

// 3. Add a comment (embedded) to the post
addComment(postId: string, text: string, name: string) {
  return this.postModel.findByIdAndUpdate(postId, { ____: { comments: { text, name } } }, { new: true });
}

// 4. Embed or reference?
//    a) a user's addresses (max 3, always shown with the user)   → ____
//    b) the customer of an order (one customer, many orders)     → ____
//    c) the steps of a recipe                                    → ____
//    d) a product's category (shared by many products)           → ____
```

<details>
<summary>✅ Solution</summary>

```ts
@Prop({ type: Types.ObjectId, ref: 'User', required: true })
author: Types.ObjectId;

create(title: string, authorId: string) {
  return this.postModel.create({ title, author: authorId });
}

findAll() {
  return this.postModel.find().populate('author', 'name email');
  // posts[0].author → { _id, name, email } instead of just the id
}

addComment(postId: string, text: string, name: string) {
  return this.postModel.findByIdAndUpdate(postId, { $push: { comments: { text, name } } }, { new: true });
}
```

4. a) embed · b) reference · c) embed · d) reference

</details>

---

## Part 2: Advanced Queries

### Query DTOs — validated, typed query strings

> 💡 **In simple words:** Query values arrive as strings. A **query DTO** describes them like a body DTO: which ones exist, their types and limits, and their defaults. With `transform: true`, `@Type(() => Number)` turns `'2'` into `2` for you, and bad values get a 400.

```ts
// src/common/dto/list-query.dto.ts
import { Type } from 'class-transformer';
import { IsIn, IsInt, IsOptional, IsString, Max, Min } from 'class-validator';

export class ListQueryDto {
  @IsOptional() @Type(() => Number) @IsInt() @Min(1)
  page: number = 1;

  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @Max(50)   // cap the limit
  limit: number = 10;

  @IsOptional() @IsString()
  search?: string;

  @IsOptional() @IsIn(['createdAt', 'title'])                   // never trust a raw sort field
  sort: string = 'createdAt';
}

// controller
@Get()
findAll(@Query() query: ListQueryDto) {
  return this.postsService.findAll(query);
}
```

### Pagination + filtering + search + sort — the standard list endpoint

> 💡 **In simple words:** Never send thousands of documents at once. **Pagination** sends one page at a time: `skip` jumps over the earlier pages, and `limit` sets the page size. Search and filters narrow the results, sort decides the order, and `total` lets the frontend show page numbers.

```ts
// posts.service.ts
async findAll({ page, limit, search, sort }: ListQueryDto) {
  const filter = search ? { title: { $regex: search, $options: 'i' } } : {};

  const [data, total] = await Promise.all([
    this.postModel
      .find(filter)
      .populate('author', 'name email')
      .sort({ [sort]: sort === 'title' ? 1 : -1 })
      .skip((page - 1) * limit)
      .limit(limit),
    this.postModel.countDocuments(filter),
  ]);

  return { data, meta: { total, page, limit, totalPages: Math.ceil(total / limit) } };
}
```

For huge datasets prefer cursor pagination (`_id > lastSeenId`) over `skip` (which scans past skipped docs).

### Indexes

> 💡 **In simple words:** An index is like the index at the back of a book: instead of reading every page to find a word, you jump straight to it. Without an index, MongoDB checks every document one by one.

```ts
// in the schema file, after SchemaFactory.createForClass(...)
UserSchema.index({ email: 1 });                          // single
PostSchema.index({ author: 1, createdAt: -1 });          // compound — match query shape
PostSchema.index({ title: 'text', content: 'text' });    // text search index

// or on one field: @Prop({ index: true }) genre: string;
```

- Index fields you filter/sort on — a query without an index is a full collection scan
- Every index slows writes — index deliberately, not everywhere
- `unique: true` creates an index automatically
- Verify with `this.model.find(q).explain('executionStats')` — look for `COLLSCAN` (bad) vs `IXSCAN` (index used)

### Aggregation pipeline

> 💡 **In simple words:** An aggregation pipeline is like a factory line. Documents enter at the top, and each **stage** does one job: filter (`$match`), group and count (`$group`), sort, limit. It's used for statistics and reports.

Documents flow through stages; each stage transforms the stream:

```ts
// Total revenue per category of delivered orders
this.orderModel.aggregate([
  { $match: { status: 'delivered' } },                        // filter FIRST (uses index)
  { $group: { _id: '$category', total: { $sum: '$total' } } },// group + accumulate
  { $sort: { total: -1 } },
  { $limit: 10 },
]);

// Posts per author, with the author's name
this.postModel.aggregate([
  { $group: { _id: '$author', count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $lookup: { from: 'users', localField: '_id', foreignField: '_id', as: 'author' } }, // join
  { $unwind: '$author' },
  { $project: { _id: 0, name: '$author.name', count: 1 } },
]);
```

**Stage cheat sheet:** `$match` filter · `$group` group+accumulate (`$sum $avg $min $max $push`) · `$sort` · `$project` shape fields · `$lookup` join · `$unwind` flatten arrays · `$skip`/`$limit` paginate. Field references need `$` (`'$total'`), stage names too (`$group`).

---

### ✍️ Your Turn: Part 2

```ts
// 1. Query DTO
export class ListQueryDto {
  @IsOptional() @____(() => Number) @IsInt() @Min(1)
  page: number = ____;

  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @____(50)    // never more than 50
  limit: number = 10;

  @IsOptional() @IsString()
  search?: string;
}

// 2. Pagination + search + total count
async findAll({ page, limit, search }: ListQueryDto) {
  const filter = search
    ? { title: { $regex: search, $options: '____' } }   // ignore upper/lower case
    : {};

  const [data, total] = await Promise.all([
    this.postModel.find(filter).sort({ createdAt: -1 }).____((page - 1) * limit).____(limit),
    this.postModel.____(filter),
  ]);

  return { data, meta: { total, page, totalPages: Math.____(total / limit) } };
}

// 3. Index for "the posts of one author, newest first"
PostSchema.index({ author: ____, createdAt: ____ });

// 4. Aggregation: number of posts per author, the most first
postsPerAuthor() {
  return this.postModel.aggregate([
    { $____: { _id: '$author', count: { $sum: ____ } } },
    { $sort: { count: ____ } },
  ]);
}

// 5. TODO: orders look like { status: 'delivered', total: 35 }.
//    Write an aggregation: the total money per status,
//    counting only orders with total > 20.
```

**Test:** `GET /api/posts?page=0` and `?limit=500`. What do you get?

<details>
<summary>✅ Solution</summary>

```ts
export class ListQueryDto {
  @IsOptional() @Type(() => Number) @IsInt() @Min(1)
  page: number = 1;

  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @Max(50)
  limit: number = 10;

  @IsOptional() @IsString()
  search?: string;
}

const filter = search ? { title: { $regex: search, $options: 'i' } } : {};
const [data, total] = await Promise.all([
  this.postModel.find(filter).sort({ createdAt: -1 }).skip((page - 1) * limit).limit(limit),
  this.postModel.countDocuments(filter),
]);
return { data, meta: { total, page, totalPages: Math.ceil(total / limit) } };

PostSchema.index({ author: 1, createdAt: -1 });

postsPerAuthor() {
  return this.postModel.aggregate([
    { $group: { _id: '$author', count: { $sum: 1 } } },
    { $sort: { count: -1 } },
  ]);
}

// 5
moneyPerStatus() {
  return this.orderModel.aggregate([
    { $match: { total: { $gt: 20 } } },                       // filter first
    { $group: { _id: '$status', money: { $sum: '$total' } } },
    { $sort: { money: -1 } },
  ]);
}
```

`?page=0` → 400 `page must not be less than 1`. `?limit=500` → 400 `limit must not be greater than 50`. The DTO protects the database from huge or broken queries.

</details>

---

## Part 3: Security Hardening

> 💡 **In simple words:** Bots will attack a public API, and quickly. Each line below closes one common door:
> - risky default headers
> - too many requests
> - unknown websites calling your API
> - unexpected fields and huge values in the body
> - database operators injected into queries
> - starting with missing secrets

```bash
npm install helmet @nestjs/throttler
```

```ts
// src/main.ts
import helmet from 'helmet';

const app = await NestFactory.create(AppModule);
app.setGlobalPrefix('api');
app.use(helmet());                                          // ~15 security headers
app.enableCors({ origin: process.env.CLIENT_URL });         // never * in production
await app.listen(process.env.PORT ?? 3000);
```

```ts
// src/app.module.ts
import { ThrottlerGuard, ThrottlerModule } from '@nestjs/throttler';
import * as mongoose from 'mongoose';

mongoose.set('sanitizeFilter', true);                       // $-operators in filters become plain values

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true, validate: validateEnv }),
    ThrottlerModule.forRoot([{ ttl: 15 * 60_000, limit: 100 }]),   // 100 requests / 15 min per IP
    // ...
  ],
  providers: [
    { provide: APP_GUARD, useClass: ThrottlerGuard },      // rate limit every route
    { provide: APP_PIPE, useValue: new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }) },
    { provide: APP_FILTER, useClass: AllExceptionsFilter },
  ],
})
export class AppModule {}
```

```ts
// auth.controller.ts — stricter on login
import { Throttle } from '@nestjs/throttler';

@Post('login')
@HttpCode(200)
@Throttle({ default: { limit: 5, ttl: 15 * 60_000 } })     // 5 attempts / 15 min
login(@Body() dto: LoginDto) { ... }

// health.controller.ts — monitoring must never be rate limited
@SkipThrottle()
@Controller('health')
export class HealthController { ... }
```

```ts
// src/config/validate-env.ts — refuse to start with missing secrets
export function validateEnv(config: Record<string, unknown>) {
  for (const key of ['MONGODB_URI', 'JWT_SECRET', 'CLIENT_URL']) {
    if (!config[key]) throw new Error(`Missing env var: ${key}`);
  }
  return config;
}
```

> Registering the pipe and filter with `APP_PIPE` / `APP_FILTER` (instead of `app.useGlobal...` in `main.ts`) means they also run in your **tests** (Part 4), which create the app from `AppModule` without `main.ts`.

**Why each matters:**
- **Mass assignment:** `whitelist` + `forbidNonWhitelisted` drop or reject fields like `"role": "admin"`; `@MaxLength()` caps long strings (NestJS also caps JSON bodies at 100kb by default)
- **NoSQL injection:** `{ "email": { "$gt": "" } }` matches *any* user — `sanitizeFilter` treats it as a literal value, and `@IsEmail()` rejects it even earlier
- **Rate limiting:** stops credential-stuffing on `/login` — keep auth limits tight; over the limit → **429 Too Many Requests**
- **CORS:** browser-enforced origin allowlist — configure your real frontend origin
- **Secrets:** `JWT_SECRET`, `MONGODB_URI` in env vars only; different values per environment; validated on startup

---

### ✍️ Your Turn: Part 3

Run `npm install helmet @nestjs/throttler` first.

```ts
// main.ts
// 1. Security headers
app.use(____());

// 2. Only our frontend may call the API
app.enableCors({ origin: process.env.____ });
```

```ts
// app.module.ts
// 3. At most 100 requests per 15 minutes for the whole API
ThrottlerModule.forRoot([{ ttl: ____ * 60_000, limit: ____ }]),

// 4. ...and apply it to every route
providers: [{ provide: ____, useClass: ThrottlerGuard }],

// 5. NoSQL-injection protection inside Mongoose:
//    objects with $-operators in a filter are treated as plain values
mongoose.set('____', true);

// 6. Refuse to start if MONGODB_URI or JWT_SECRET is missing
ConfigModule.forRoot({ isGlobal: true, ____: validateEnv }),
```

```ts
// auth.controller.ts
// 7. At most 5 login attempts per 15 minutes
@Post('login')
@____({ default: { limit: ____, ttl: 15 * 60_000 } })
login(@Body() dto: LoginDto) { ... }

// 8. Attack test: temporarily remove @IsEmail() from LoginDto and send
//    POST /api/auth/login with { "email": { "$gt": "" }, "password": "anything" }
//    What does findOne({ email }) return before and after step 5?
```

<details>
<summary>✅ Solution</summary>

```ts
app.use(helmet());
app.enableCors({ origin: process.env.CLIENT_URL });

ThrottlerModule.forRoot([{ ttl: 15 * 60_000, limit: 100 }]),
providers: [{ provide: APP_GUARD, useClass: ThrottlerGuard }],
mongoose.set('sanitizeFilter', true);
ConfigModule.forRoot({ isGlobal: true, validate: validateEnv }),

@Post('login')
@HttpCode(200)
@Throttle({ default: { limit: 5, ttl: 15 * 60_000 } })
login(@Body() dto: LoginDto) { ... }
```

8. **Before:** `{ $gt: "" }` means "any email bigger than an empty string", so MongoDB returns the **first user** in the collection. bcrypt still blocks this login, but the same trick on a query with no password check (for example "find user by reset token") would leak data.
**After:** Mongoose searches for an email literally equal to `{ "$gt": "" }`, finds nothing, and the API returns 401. Put `@IsEmail()` back: it blocks the payload with a 400 before the database is even asked.

</details>

---

## Part 4: Testing

> 💡 **In simple words:** Automated tests are code that checks your code. NestJS projects come with **Jest** already set up. There are two kinds of tests:
> - **Unit tests** check one class alone (a service), with fake versions of what it needs (a fake model).
> - **End-to-end (e2e) tests** start the whole app and send real HTTP requests with **Supertest**: `expect(res.status).toBe(200)`.
>
> Run them before every deploy, and you'll know immediately if a change broke something.

| Script | Runs | Files |
|--------|------|-------|
| `npm test` | unit tests | `src/**/*.spec.ts` |
| `npm run test:e2e` | e2e tests | `test/**/*.e2e-spec.ts` |
| `npm run test:cov` | unit tests + coverage report | |

### Unit test — a service with a fake model

```ts
// src/books/books.service.spec.ts
import { NotFoundException } from '@nestjs/common';
import { getModelToken } from '@nestjs/mongoose';
import { Test } from '@nestjs/testing';
import { Book } from './schemas/book.schema';
import { BooksService } from './books.service';

describe('BooksService', () => {
  let service: BooksService;
  const bookModel = { findById: jest.fn(), create: jest.fn() };   // a fake model

  beforeEach(async () => {
    const moduleRef = await Test.createTestingModule({
      providers: [
        BooksService,
        { provide: getModelToken(Book.name), useValue: bookModel },  // inject the fake
      ],
    }).compile();

    service = moduleRef.get(BooksService);
    jest.clearAllMocks();
  });

  it('returns the book when it exists', async () => {
    bookModel.findById.mockResolvedValue({ title: 'Clean Code' });
    await expect(service.findOne('665f1c2e8a1b2c3d4e5f6a7b')).resolves.toEqual({ title: 'Clean Code' });
  });

  it('throws NotFoundException when the book does not exist', async () => {
    bookModel.findById.mockResolvedValue(null);
    await expect(service.findOne('665f1c2e8a1b2c3d4e5f6a7b')).rejects.toThrow(NotFoundException);
  });
});
```

### E2E test — the real app, a separate test database

```bash
# .env.test — a DIFFERENT database: the tests delete it at the end
MONGODB_URI=mongodb://127.0.0.1:27017/library_test
JWT_SECRET=test-secret
CLIENT_URL=http://localhost:5173
```

```ts
// app.module.ts — Jest sets NODE_ENV=test, so tests read .env.test
ConfigModule.forRoot({
  isGlobal: true,
  envFilePath: process.env.NODE_ENV === 'test' ? '.env.test' : '.env',
  validate: validateEnv,
}),
```

```ts
// test/books.e2e-spec.ts
import { INestApplication } from '@nestjs/common';
import { getConnectionToken } from '@nestjs/mongoose';
import { Test } from '@nestjs/testing';
import { Connection } from 'mongoose';
import request from 'supertest';
import { AppModule } from '../src/app.module';

describe('Books API (e2e)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleRef = await Test.createTestingModule({ imports: [AppModule] }).compile();
    app = moduleRef.createNestApplication();
    app.setGlobalPrefix('api');                 // the same setup as main.ts
    await app.init();
  });

  afterAll(async () => {
    await app.get<Connection>(getConnectionToken()).dropDatabase();   // delete the test data
    await app.close();                                                 // close the connection so Jest can exit
  });

  it('GET /api/books returns an array', async () => {
    const res = await request(app.getHttpServer()).get('/api/books');
    expect(res.status).toBe(200);
    expect(Array.isArray(res.body)).toBe(true);
  });

  it('returns 404 for an unknown route', async () => {
    const res = await request(app.getHttpServer()).get('/api/nothing');
    expect(res.status).toBe(404);
  });
});
```

**Rules:** test happy + unhappy paths; separate test database; reset state in `beforeEach`/`afterAll`; keep tests independent (no order dependence) unless they are one flow; always `await` requests; repeat any `main.ts` setup that isn't in `AppModule` (like the global prefix).

---

### ✍️ Your Turn: Part 4

Create `.env.test` with `MONGODB_URI=mongodb://127.0.0.1:27017/library_test` (and the other variables).

```ts
// 1. Unit test: BooksService.remove throws 404 when nothing was deleted
describe('BooksService', () => {
  let service: BooksService;
  const bookModel = { findByIdAndDelete: jest.fn() };

  beforeEach(async () => {
    const moduleRef = await Test.____({
      providers: [BooksService, { provide: ____(Book.name), useValue: bookModel }],
    }).compile();
    service = moduleRef.____(BooksService);
  });

  it('throws NotFoundException when the book does not exist', async () => {
    bookModel.findByIdAndDelete.____(null);
    await expect(service.remove('665f1c2e8a1b2c3d4e5f6a7b')).____.toThrow(____);
  });
});
```

```ts
// 2. E2E: test/books.e2e-spec.ts
describe('Books API (e2e)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleRef = await Test.createTestingModule({ imports: [____] }).compile();
    app = moduleRef.____();
    app.setGlobalPrefix('____');
    await app.____();
  });

  afterAll(async () => {
    await app.get<Connection>(getConnectionToken()).____();   // delete the test data
    await app.____();                                         // close so Jest can exit
  });

  it('GET /api/books returns an array', async () => {
    const res = await request(app.____()).get('/api/books');
    expect(res.____).toBe(200);
  });

  it('POST /api/books without a token returns 401', async () => {
    // TODO
  });

  it('GET /api/books/abc returns 400 (invalid id)', async () => {
    // TODO
  });

  it('GET a valid id that does not exist returns 404', async () => {
    const fakeId = new Types.ObjectId().toString();
    // TODO
  });
});
```

Run with `npm test` and `npm run test:e2e`.

<details>
<summary>✅ Solution</summary>

```ts
beforeEach(async () => {
  const moduleRef = await Test.createTestingModule({
    providers: [BooksService, { provide: getModelToken(Book.name), useValue: bookModel }],
  }).compile();
  service = moduleRef.get(BooksService);
});

it('throws NotFoundException when the book does not exist', async () => {
  bookModel.findByIdAndDelete.mockResolvedValue(null);
  await expect(service.remove('665f1c2e8a1b2c3d4e5f6a7b')).rejects.toThrow(NotFoundException);
});
```

```ts
beforeAll(async () => {
  const moduleRef = await Test.createTestingModule({ imports: [AppModule] }).compile();
  app = moduleRef.createNestApplication();
  app.setGlobalPrefix('api');
  await app.init();
});

afterAll(async () => {
  await app.get<Connection>(getConnectionToken()).dropDatabase();
  await app.close();
});

it('GET /api/books returns an array', async () => {
  const res = await request(app.getHttpServer()).get('/api/books');
  expect(res.status).toBe(200);
});

it('POST /api/books without a token returns 401', async () => {
  const res = await request(app.getHttpServer()).post('/api/books').send({ title: 'X', author: 'Y' });
  expect(res.status).toBe(401);
});

it('GET /api/books/abc returns 400 (invalid id)', async () => {
  const res = await request(app.getHttpServer()).get('/api/books/abc');
  expect(res.status).toBe(400);
});

it('GET a valid id that does not exist returns 404', async () => {
  const fakeId = new Types.ObjectId().toString();
  const res = await request(app.getHttpServer()).get(`/api/books/${fakeId}`);
  expect(res.status).toBe(404);
});
```

</details>

---

## Part 5: Deployment

### Flow (Render / Railway / Fly.io — same pattern)

> 💡 **In simple words:** **Deploying** means putting your API on a computer on the internet so anyone can call it. A NestJS app is TypeScript, so the platform first **builds** it (`npm run build` → JavaScript in `dist/`), then **starts** the compiled code (`npm run start:prod`). Platforms like Render take your code from GitHub, run these commands, and give you a public URL.

1. Push to GitHub
2. New web service → connect repo
   - **Build:** `npm ci --include=dev && npm run build` (the Nest CLI and TypeScript are dev dependencies, and the build needs them)
   - **Start:** `npm run start:prod` (= `node dist/main`)
3. Set env vars in the platform dashboard: `PORT` (usually auto), `MONGODB_URI` (Atlas prod DB), `JWT_SECRET`, `CLIENT_URL`, `NODE_ENV=production`
4. Atlas: production cluster → network access (allow your host's IPs or `0.0.0.0/0` for platforms without static IPs) → copy `mongodb+srv://` URI
5. Health check path in the platform: `/api/health`

### Health check

```ts
// src/health/health.controller.ts
import { Controller, Get, ServiceUnavailableException } from '@nestjs/common';
import { InjectConnection } from '@nestjs/mongoose';
import { SkipThrottle } from '@nestjs/throttler';
import { Connection } from 'mongoose';

@SkipThrottle()
@Controller('health')
export class HealthController {
  constructor(@InjectConnection() private readonly connection: Connection) {}

  @Get()
  check() {
    const connected = this.connection.readyState === 1;      // 1 = connected
    if (!connected) throw new ServiceUnavailableException({ status: 'error', db: 'disconnected' });
    return { status: 'ok', db: 'connected' };
  }
}
```

### Deployment checklist

> 💡 **In simple words:** Most deployment failures come from small forgotten settings. Go through this list one line at a time.

- [ ] `npm run build` works locally with no TypeScript errors
- [ ] Start command = `npm run start:prod` (never `start:dev` on a server)
- [ ] `process.env.PORT` — never hardcode
- [ ] All required env vars set on the platform + validated at startup (`validate` in `ConfigModule`)
- [ ] `.env` gitignored; `.env.example` committed
- [ ] helmet + restricted CORS + throttler + `sanitizeFilter` + `whitelist` active
- [ ] Prod database ≠ dev database ≠ test database
- [ ] Error responses are clean JSON, no stack traces
- [ ] Health endpoint (`GET /api/health`) for platform monitoring
- [ ] Unit + e2e tests pass in CI before deploy

---

### ✍️ Your Turn: Part 5

Fill the blanks before you deploy:

```text
1. Build command on the platform:          ____ && npm run build
2. Start command:                          npm run ____            (not start:dev)
3. The port line in main.ts:               await app.listen(____ ?? 3000);
4. Env vars to set on the platform:        ____, ____, ____, NODE_ENV=____
5. Atlas → Network Access must allow:      ____              (the platform has no fixed IP)
6. A route the platform calls to check that the app is alive:   GET ____
```

```ts
// 7. TODO: write HealthController (GET /api/health)
//    → 200 { status: 'ok', db: 'connected' }  when Mongo is connected
//    → 503 { status: 'error', db: 'disconnected' }  otherwise
//    hint: @InjectConnection() + connection.readyState === 1
//    Don't forget: add it to a module, and skip the rate limit for it.
```

<details>
<summary>✅ Solution</summary>

```text
1. npm ci --include=dev && npm run build
2. npm run start:prod
3. await app.listen(process.env.PORT ?? 3000);
4. MONGODB_URI, JWT_SECRET, CLIENT_URL, NODE_ENV=production
5. 0.0.0.0/0 (or the platform's IP list, if it gives one)
6. GET /api/health
```

7. The `HealthController` above, listed in `AppModule`'s `controllers`, with `@SkipThrottle()`.

</details>

---

## Capstone: Build & Deploy a Complete REST API

Pick one: **blog** (users, posts, comments), **e-commerce** (products, categories, orders), or **task manager** (projects, tasks, users).

### Requirements checklist

- [ ] One NestJS module per feature (module + controller + service + schema + DTOs)
- [ ] CRUD on ≥2 related resources (`Post` ↔ `User` via `ref` + `populate`)
- [ ] `POST /auth/register` + `/auth/login` → JWT; bcrypt pre-save hook; `select: false` password
- [ ] `JwtAuthGuard` + `RolesGuard` + `@Roles('admin')` on sensitive routes
- [ ] `class-validator` DTOs on every write endpoint; global `ValidationPipe` with `whitelist`
- [ ] Custom exceptions + global `AllExceptionsFilter` (Mongoose errors → 400/409)
- [ ] Query DTO with pagination + search + sort on list endpoints
- [ ] At least one aggregation endpoint (`/api/stats`)
- [ ] Indexes on filtered/sorted fields
- [ ] Security stack (helmet, throttler, `sanitizeFilter`, CORS, env validation)
- [ ] Unit tests for services + e2e tests covering CRUD + auth paths
- [ ] Deployed to a public URL with Atlas prod DB
- [ ] README: setup steps, env vars, endpoint docs, Postman collection

### Evaluation

Correct CRUD · secure auth · validation + error handling · clean module structure · tests · live deployment.

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. When do you embed data, and when do you use a reference?
2. What does `populate()` do? Why do we pass a list of fields to it?
3. What does `skip((page - 1) * limit).limit(limit)` do for page 3 with limit 10?
4. Why do we use a query DTO with `@Type(() => Number)` and `@Max(50)`?
5. What is an index? What is the downside of adding too many?
6. In an aggregation, why do we put `$match` first? What do `$group` and `$sum: 1` do together?
7. What problem does rate limiting solve? Why is the limit on `/login` stricter? Which status code do you get over the limit?
8. What is NoSQL injection? Give the classic example payload. Name two NestJS-side protections.
9. What is the difference between a unit test and an e2e test? Why do e2e tests need a separate database?
10. Why does the build command need the dev dependencies? Name 4 more things you must check before deploying.

<details>
<summary>✅ Solution</summary>

1. Embed data that is small and always read together with the parent (comments, addresses). Use a reference for data that is shared, large, or changed on its own (author, customer, category).
2. It replaces the stored ObjectId with the real document. The field list (`'name email'`) returns only what we need and never leaks fields like the password.
3. It skips the first 20 documents and returns documents 21–30.
4. Query values are strings: `@Type` converts them to numbers. `@Max(50)` stops a client from asking for millions of documents at once.
5. A sorted lookup structure that makes searches and sorts fast. Every index makes writes slower and uses memory.
6. Fewer documents go to the next stages, so it's faster, and `$match` at the start can use an index. `$group` + `$sum: 1` count how many documents are in each group.
7. It stops attackers from sending thousands of requests (for example guessing passwords). Login is where password guessing happens. **429 Too Many Requests**.
8. Sending MongoDB operators instead of plain values, so the query matches something it shouldn't: `{ "email": { "$gt": "" } }`. Protections: DTO validation (`@IsEmail()` / `@IsString()`) and `mongoose.set('sanitizeFilter', true)`.
9. A unit test checks one class alone with fakes; an e2e test starts the whole app and sends real requests. E2E tests create and delete data (`dropDatabase`): on the real database they would destroy real data.
10. TypeScript and the Nest CLI are dev dependencies, and `npm run build` uses them. Any four of: env vars set · start uses `start:prod` · port from `process.env.PORT` · CORS limited to the frontend · throttler + helmet on · production DB is not the dev DB · no stack traces in responses · health endpoint · tests pass.

</details>

### B. Find the Bug

```ts
// Bug 1: the API response shows every author's password hash
//        (the User schema has no select: false on password)
return this.postModel.find().populate('author');

// Bug 2: page 1 shows the SECOND set of results
return this.postModel.find().skip(page * limit).limit(limit);

// Bug 3: the result is one group with the total count of ALL posts
return this.postModel.aggregate([{ $group: { _id: 'author', count: { $sum: 1 } } }]);

// Bug 4: after running the e2e tests, all the development data is gone
ConfigModule.forRoot({ isGlobal: true })          // always reads .env
// ... afterAll → dropDatabase()

// Bug 5: Jest prints "Jest did not exit one second after the test run has completed"
afterAll(async () => {
  await app.get<Connection>(getConnectionToken()).dropDatabase();
});

// Bug 6: in production, any website can call our API
app.enableCors();

// Bug 7: in the e2e test every request returns 404 "Cannot GET /api/books"
app = moduleRef.createNestApplication();
await app.init();
```

<details>
<summary>✅ Solution</summary>

```ts
// Bug 1: choose the fields (and add select: false to the password)
return this.postModel.find().populate('author', 'name email');

// Bug 2: pages start at 1
return this.postModel.find().skip((page - 1) * limit).limit(limit);

// Bug 3: field names need a $ ('author' is just a fixed text, so everything is one group)
return this.postModel.aggregate([{ $group: { _id: '$author', count: { $sum: 1 } } }]);

// Bug 4: use the TEST database when NODE_ENV is 'test'
ConfigModule.forRoot({
  isGlobal: true,
  envFilePath: process.env.NODE_ENV === 'test' ? '.env.test' : '.env',
})

// Bug 5: close the app (and with it the database connection)
afterAll(async () => {
  await app.get<Connection>(getConnectionToken()).dropDatabase();
  await app.close();
});

// Bug 6: allow only your frontend
app.enableCors({ origin: process.env.CLIENT_URL });

// Bug 7: main.ts doesn't run in tests, so the prefix is missing
app = moduleRef.createNestApplication();
app.setGlobalPrefix('api');
await app.init();
```

</details>

### C. Coding Tasks

**Task 1: Blog relationships (medium).** `Post` references `User` (author) and embeds `comments`. Endpoints:
- `POST /api/posts` (`JwtAuthGuard`): the author comes from the token (`@CurrentUser()`)
- `GET /api/posts/:id`: returns the post with the author's `name` only
- `POST /api/posts/:id/comments` (`JwtAuthGuard`): adds `{ text, name }` to the comments (DTO: `text` required)

**Task 2: List endpoint (medium).** `GET /api/books?search=code&genre=science&sort=pages&page=1&limit=5` returns `{ data, meta: { total, page, totalPages } }`. Write a `BookQueryDto`: `sort` can be `pages` or `createdAt` (default `createdAt`, newest first), `genre` must be a valid genre.

**Task 3: Stats endpoint (medium).** `GET /api/stats/books-per-genre` (admin only) returns, with one aggregation: `[{ genre: 'science', count: 4, avgPages: 320 }, ...]`, sorted by `count`.

**Task 4: Security pass (easy).** Add helmet, the throttler (stricter on `/api/auth/login`), CORS limited to `CLIENT_URL`, `whitelist` + `forbidNonWhitelisted`, `sanitizeFilter`, and a startup env check. Then test: send the 6th login request within 15 minutes. What status code do you get?

**Task 5: Auth e2e tests (medium).** Write 5 Supertest tests: register → 201 · register again → 409 · login with a wrong password → 401 · correct login returns a token · `GET /api/auth/me` with the token → 200 and the right email.

**Challenge: Deploy it.** Deploy the Library API to Render (or Railway) with an Atlas database. Requirements: `/api/health` works on the public URL, all env vars are set on the platform (none in the code), and the README has the live URL, setup steps and a list of endpoints.

<details>
<summary>✅ Solution: Tasks 1–5</summary>

```ts
// Task 1: posts/dto/create-comment.dto.ts
export class CreateCommentDto {
  @IsString() @IsNotEmpty() text: string;
  @IsOptional() @IsString() name?: string;
}

// posts/posts.service.ts
@Injectable()
export class PostsService {
  constructor(@InjectModel(Post.name) private readonly postModel: Model<Post>) {}

  create(title: string, authorId: string) {
    return this.postModel.create({ title, author: authorId });
  }

  async findOne(id: string) {
    const post = await this.postModel.findById(id).populate('author', 'name');
    if (!post) throw new NotFoundException('Post not found');
    return post;
  }

  async addComment(id: string, dto: CreateCommentDto) {
    const post = await this.postModel.findByIdAndUpdate(id, { $push: { comments: dto } }, { new: true });
    if (!post) throw new NotFoundException('Post not found');
    return post;
  }
}

// posts/posts.controller.ts
@Controller('posts')
export class PostsController {
  constructor(private readonly postsService: PostsService) {}

  @Post()
  @UseGuards(JwtAuthGuard)
  create(@Body() dto: CreatePostDto, @CurrentUser() user: JwtPayload) {
    return this.postsService.create(dto.title, user.userId);
  }

  @Get(':id')
  findOne(@Param('id', ParseObjectIdPipe) id: string) {
    return this.postsService.findOne(id);
  }

  @Post(':id/comments')
  @UseGuards(JwtAuthGuard)
  addComment(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: CreateCommentDto) {
    return this.postsService.addComment(id, dto);
  }
}
```

```ts
// Task 2: books/dto/book-query.dto.ts
export class BookQueryDto {
  @IsOptional() @Type(() => Number) @IsInt() @Min(1)
  page: number = 1;

  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @Max(50)
  limit: number = 10;

  @IsOptional() @IsString()
  search?: string;

  @IsOptional() @IsIn(['novel', 'science', 'history', 'kids'])
  genre?: string;

  @IsOptional() @IsIn(['pages', 'createdAt'])
  sort: 'pages' | 'createdAt' = 'createdAt';
}

// books.service.ts
async findAll({ page, limit, search, genre, sort }: BookQueryDto) {
  const filter: Record<string, unknown> = {};
  if (search) filter.title = { $regex: search, $options: 'i' };
  if (genre) filter.genre = genre;

  const sortOrder = sort === 'pages' ? 1 : -1;

  const [data, total] = await Promise.all([
    this.bookModel.find(filter).sort({ [sort]: sortOrder }).skip((page - 1) * limit).limit(limit),
    this.bookModel.countDocuments(filter),
  ]);

  return { data, meta: { total, page, totalPages: Math.ceil(total / limit) } };
}

// books.controller.ts
@Get()
findAll(@Query() query: BookQueryDto) {
  return this.booksService.findAll(query);
}
```

```ts
// Task 3: stats/stats.service.ts (StatsModule imports the Book schema with forFeature)
booksPerGenre() {
  return this.bookModel.aggregate([
    { $group: { _id: '$genre', count: { $sum: 1 }, avgPages: { $avg: '$pages' } } },
    { $sort: { count: -1 } },
    { $project: { _id: 0, genre: '$_id', count: 1, avgPages: { $round: ['$avgPages', 0] } } },
  ]);
}

// stats/stats.controller.ts
@Controller('stats')
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
export class StatsController {
  constructor(private readonly statsService: StatsService) {}

  @Get('books-per-genre')
  booksPerGenre() {
    return this.statsService.booksPerGenre();
  }
}
```

Task 4: use the solution of "Your Turn: Part 3". The 6th login attempt returns **429 Too Many Requests** (`ThrottlerException: Too Many Requests`).

```ts
// Task 5: test/auth.e2e-spec.ts
import { INestApplication } from '@nestjs/common';
import { getConnectionToken } from '@nestjs/mongoose';
import { Test } from '@nestjs/testing';
import { Connection } from 'mongoose';
import request from 'supertest';
import { AppModule } from '../src/app.module';

// These tests build on each other (a flow), so they stay in one describe and run in order.
describe('Auth flow (e2e)', () => {
  let app: INestApplication;
  const user = { name: 'Test User', email: 'test@test.com', password: 'secret123' };
  let token: string;

  beforeAll(async () => {
    const moduleRef = await Test.createTestingModule({ imports: [AppModule] }).compile();
    app = moduleRef.createNestApplication();
    app.setGlobalPrefix('api');
    await app.init();
  });

  afterAll(async () => {
    await app.get<Connection>(getConnectionToken()).dropDatabase();
    await app.close();
  });

  it('registers a new user', async () => {
    const res = await request(app.getHttpServer()).post('/api/auth/register').send(user);
    expect(res.status).toBe(201);
    expect(res.body.password).toBeUndefined();
  });

  it('rejects a duplicate email', async () => {
    const res = await request(app.getHttpServer()).post('/api/auth/register').send(user);
    expect(res.status).toBe(409);
  });

  it('rejects a wrong password', async () => {
    const res = await request(app.getHttpServer())
      .post('/api/auth/login')
      .send({ email: user.email, password: 'wrong-password' });
    expect(res.status).toBe(401);
  });

  it('logs in and returns a token', async () => {
    const res = await request(app.getHttpServer())
      .post('/api/auth/login')
      .send({ email: user.email, password: user.password });
    expect(res.status).toBe(200);
    expect(res.body.token).toBeDefined();
    token = res.body.token;
  });

  it('returns the current user with the token', async () => {
    const res = await request(app.getHttpServer())
      .get('/api/auth/me')
      .set('Authorization', `Bearer ${token}`);
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
6. What do `whitelist` and `sanitizeFilter` prevent? Show the attack payload.
7. Why stricter rate limits on `/api/auth/login`? How do you skip the limit for `/api/health`?
8. Why register the `ValidationPipe` and filter with `APP_PIPE` / `APP_FILTER`?
9. Unit vs e2e test — what does each check? How do you give a service a fake model?
10. Walk through a deploy: code → build → start → env vars → Atlas. What's forgotten most often?

## Common Mistakes

- Over-embedding (16MB limit) or orphaned refs (no cleanup on delete)
- `populate` without field selection — hauling whole user docs
- `skip` on millions of docs; regex search without a text index
- `COLLSCAN` queries in prod — no indexes
- Wide-open CORS, no rate limit on login, `ValidationPipe` without `whitelist`
- Global setup only in `main.ts` → missing in tests
- E2E tests hitting the dev DB; `app.close()` forgotten → Jest hangs
- Start command `start:dev` in production; build failing because dev dependencies weren't installed
- Hardcoded `localhost:3000`, missing env vars

---

## Additional Resources

- [NestJS docs](https://docs.nestjs.com/) — [MongoDB (Mongoose)](https://docs.nestjs.com/techniques/mongodb) · [Validation](https://docs.nestjs.com/techniques/validation) · [Authentication](https://docs.nestjs.com/security/authentication) · [Rate limiting](https://docs.nestjs.com/security/rate-limiting) · [Testing](https://docs.nestjs.com/fundamentals/testing)
- [Mongoose docs — populate/indexes/aggregation](https://mongoosejs.com/docs/)
- [MongoDB Atlas](https://www.mongodb.com/atlas) · [class-validator decorators](https://github.com/typestack/class-validator#validation-decorators)
- [Jest](https://jestjs.io/) · [Supertest](https://github.com/ladjs/supertest)

---

**Session 12 complete — and so is the track.** You now hold the full stack: TypeScript → React → NestJS/MongoDB. The React frontend (Session 7) + this REST API = a production-ready full-stack application. Ship it.
