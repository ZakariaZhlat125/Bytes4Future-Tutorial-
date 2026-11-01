# Session 11: Validation, Error Handling & Authentication

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Session 10 (Mongoose schemas, services, CRUD)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> Keep building the **Library API** from Session 10. If `npm install bcrypt` fails on Windows, use `npm install bcryptjs` and `import * as bcrypt from 'bcryptjs'` instead (same functions).

## Session Overview

- **Part 1:** Schema-level validation — built-in + custom validators
- **Part 2:** Request-level validation — DTOs, `class-validator`, `ValidationPipe`, `PartialType`
- **Part 3:** Centralized error handling — built-in exceptions, custom exceptions, a global filter for database errors
- **Part 4:** Authentication — bcrypt hashing, register/login, JWT, `JwtAuthGuard`, roles
- **Lab:** Auth-protected to-do API

---

## Part 1: Schema Validation (Last Line of Defense)

> 💡 **In simple words:** Never trust data that comes from users. Schema validation is the final guard right before data enters the database: if a field breaks a rule, Mongoose refuses to save it.

```ts
@Schema()
export class Product {
  @Prop({ required: [true, 'Name is required'], minlength: 2 })
  name: string;

  @Prop({ required: true, min: [0, 'Price cannot be negative'] })
  price: number;

  @Prop({ unique: true })
  sku: string;

  @Prop({ enum: ['electronics', 'books', 'clothing'] })
  category: string;

  @Prop({ lowercase: true, match: /\S+@\S+\.\S+/ })
  supplierEmail: string;

  @Prop({
    type: [String],
    validate: {                                        // custom validator
      validator: (v: string[]) => v.length <= 5,
      message: 'A product can have at most 5 tags',
    },
  })
  tags: string[];
}
```

- `min`/`max` → numbers & dates; `minlength`/`maxlength` → strings (don't confuse them)
- Custom `validate` — return `true`/`false`; use **regular functions** if you need `this` (arrow functions lose it)
- **Updates don't validate by default** — always pass `runValidators: true` to `findByIdAndUpdate`
- `unique` is an index, not a validator — handle the duplicate-key error (code 11000) yourself (Part 3)

---

### ✍️ Your Turn: Part 1

```ts
// src/users/schemas/user.schema.ts
@Schema({ timestamps: true })
export class User {
  @Prop({ required: [true, '____'], ____: 2 })     // at least 2 characters
  name: string;

  @Prop({
    required: true,
    unique: true,
    lowercase: true,
    ____: [/\S+@\S+\.\S+/, 'Invalid email'],
  })
  email: string;

  @Prop({ ____: [13, 'You must be at least 13'] })
  age?: number;

  @Prop({ ____: ['user', 'admin'], default: 'user' })
  role: string;

  @Prop({
    type: [String],
    validate: {
      validator: (v: string[]) => ____,              // at most 3 skills
      message: 'You can add at most 3 skills',
    },
  })
  skills: string[];
}

// Question: this rule never works. Why?
// @Prop({ min: 2 }) name: string;
```

<details>
<summary>✅ Solution</summary>

```ts
@Schema({ timestamps: true })
export class User {
  @Prop({ required: [true, 'Name is required'], minlength: 2 })
  name: string;

  @Prop({ required: true, unique: true, lowercase: true, match: [/\S+@\S+\.\S+/, 'Invalid email'] })
  email: string;

  @Prop({ min: [13, 'You must be at least 13'] })
  age?: number;

  @Prop({ enum: ['user', 'admin'], default: 'user' })
  role: string;

  @Prop({
    type: [String],
    validate: {
      validator: (v: string[]) => v.length <= 3,
      message: 'You can add at most 3 skills',
    },
  })
  skills: string[];
}
```

`min` is for **numbers and dates**. For strings, use `minlength`.

</details>

---

## Part 2: Request Validation (First Line of Defense)

> 💡 **In simple words:** Request validation checks the input as soon as the request arrives, before any database work. It gives clear error messages and stops bad data early. Think of it as the guard at the front door, while the schema is the guard at the safe. In NestJS the front door is the **DTO** + `ValidationPipe` you met in Session 9.

```ts
// src/main.ts — once for the whole app
app.useGlobalPipes(new ValidationPipe({
  whitelist: true,               // strip unknown fields (blocks { "role": "admin" } on register!)
  forbidNonWhitelisted: true,    // ...or reject them
  transform: true,               // plain JSON → DTO instance; '5' → 5 for typed query/params
}));
```

### Common `class-validator` decorators

| Decorator | Checks |
|-----------|--------|
| `@IsString()` `@IsInt()` `@IsNumber()` `@IsBoolean()` `@IsDateString()` | type |
| `@IsNotEmpty()` `@IsOptional()` | required / optional |
| `@MinLength(6)` `@MaxLength(200)` | string length |
| `@Min(1)` `@Max(100)` | number range |
| `@IsEmail()` `@IsUrl()` `@IsMongoId()` | formats |
| `@IsIn(['a', 'b'])` `@IsEnum(Role)` | allowed values |
| `@Matches(/regex/)` | custom pattern |
| `@IsArray()` `@ArrayMaxSize(3)` | arrays |

Every decorator accepts `{ message: '...' }` for your own text.

```ts
// src/auth/dto/register.dto.ts
import { IsEmail, IsNotEmpty, IsString, MinLength } from 'class-validator';

export class RegisterDto {
  @IsString()
  @IsNotEmpty({ message: 'name is required' })
  name: string;

  @IsEmail({}, { message: 'email is invalid' })
  email: string;

  @IsString()
  @MinLength(6, { message: 'password needs at least 6 characters' })
  password: string;
}
```

### Update DTOs with `PartialType`

> 💡 **In simple words:** An update DTO usually has the same fields as the create DTO, but all optional. `PartialType` builds it for you, with all the rules copied, so you never write the same rules twice.

```bash
npm install @nestjs/mapped-types
```

```ts
import { PartialType, PickType, OmitType } from '@nestjs/mapped-types';

export class UpdateBookDto extends PartialType(CreateBookDto) {}            // all fields optional
export class LoginDto extends PickType(RegisterDto, ['email', 'password']) {} // only these two
export class PublicUserDto extends OmitType(RegisterDto, ['password']) {}   // everything but password
```

```json
// POST /api/auth/register with { "email": "bad" } →
{
  "statusCode": 400,
  "message": ["name is required", "email is invalid", "password needs at least 6 characters"],
  "error": "Bad Request"
}
```

---

### ✍️ Your Turn: Part 2

```ts
// 1. Register DTO
export class RegisterDto {
  @IsString()
  @____({ message: 'name is required' })
  name: string;

  @____({}, { message: 'email is invalid' })
  email: string;

  @IsString()
  @____(6, { message: 'password needs at least 6 characters' })
  password: string;
}

// 2. Login DTO: only email + password, with the same rules (don't repeat them)
export class LoginDto extends ____(RegisterDto, ['____', '____']) {}

// 3. Book DTOs
export class CreateBookDto {
  @IsString() @IsNotEmpty() title: string;
  @IsString() @IsNotEmpty() author: string;
  // TODO: pages → optional, integer, min 1
  // TODO: genre → optional, one of: novel, science, history, kids
}

export class UpdateBookDto extends ____(CreateBookDto) {}

// 4. main.ts: strip unknown fields AND reject requests that send them
app.useGlobalPipes(new ValidationPipe({ ____: true, ____: true, transform: true }));
```

**Test:** register with `{ "name": "Sara", "email": "sara@test.com", "password": "secret1", "role": "admin" }`. What happens? Why does this matter?

<details>
<summary>✅ Solution</summary>

```ts
export class RegisterDto {
  @IsString()
  @IsNotEmpty({ message: 'name is required' })
  name: string;

  @IsEmail({}, { message: 'email is invalid' })
  email: string;

  @IsString()
  @MinLength(6, { message: 'password needs at least 6 characters' })
  password: string;
}

export class LoginDto extends PickType(RegisterDto, ['email', 'password']) {}

export class CreateBookDto {
  @IsString() @IsNotEmpty() title: string;
  @IsString() @IsNotEmpty() author: string;
  @IsOptional() @IsInt() @Min(1) pages?: number;
  @IsOptional() @IsIn(['novel', 'science', 'history', 'kids']) genre?: string;
}

export class UpdateBookDto extends PartialType(CreateBookDto) {}

app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }));
```

You get `400 ["property role should not exist"]`. Without `whitelist`, anyone could register as an admin just by sending `"role": "admin"`.

</details>

---

## Part 3: Centralized Error Handling

> 💡 **In simple words:** In NestJS you already have centralized error handling: you **throw**, and NestJS turns the exception into a response. Errors from `async` methods are caught automatically, so you need no `try/catch` in services. What's left is (1) clear exceptions for your own rules and (2) one global filter that also converts **database** errors into proper status codes.

### Built-in exceptions — throw, don't build responses

```ts
if (!book) throw new NotFoundException('Book not found');           // 404
if (exists) throw new ConflictException('Email already registered'); // 409
if (!ok) throw new UnauthorizedException('Invalid credentials');     // 401
throw new ForbiddenException('Only admins can do this');             // 403
```

### Custom exceptions — a name for a business rule

```ts
// src/books/exceptions/book-not-available.exception.ts
import { ConflictException } from '@nestjs/common';

export class BookNotAvailableException extends ConflictException {
  constructor(title: string) {
    super(`"${title}" is already borrowed`);       // status 409 comes from the parent
  }
}

// in the service
if (!book.isAvailable) throw new BookNotAvailableException(book.title);
```

### A global filter that understands Mongoose errors

> 💡 **In simple words:** Mongoose throws its own errors (bad id, failed schema rule, duplicate email). They are not `HttpException`s, so NestJS would answer 500. Our filter recognizes them and answers 400 or 409 instead, so services stay clean.

```ts
// src/common/filters/all-exceptions.filter.ts
import { ArgumentsHost, Catch, ExceptionFilter, HttpException, Logger } from '@nestjs/common';
import { HttpAdapterHost } from '@nestjs/core';

type MongooseLikeError = {
  name?: string;
  code?: number;
  errors?: Record<string, { message: string }>;
};

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger('Errors');

  constructor(private readonly adapterHost: HttpAdapterHost) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const { httpAdapter } = this.adapterHost;
    const ctx = host.switchToHttp();
    const { status, message } = this.toResponse(exception);

    if (status === 500) this.logger.error(exception);    // log real bugs, never send them

    httpAdapter.reply(ctx.getResponse(), { statusCode: status, message }, status);
  }

  private toResponse(exception: unknown): { status: number; message: string | string[] } {
    if (exception instanceof HttpException) {
      const body = exception.getResponse();
      const message = typeof body === 'string' ? body : (body as { message: string | string[] }).message;
      return { status: exception.getStatus(), message };
    }

    const err = exception as MongooseLikeError;
    if (err.name === 'CastError') return { status: 400, message: 'Invalid id' };
    if (err.name === 'ValidationError') {
      return { status: 400, message: Object.values(err.errors ?? {}).map((e) => e.message) };
    }
    if (err.code === 11000) return { status: 409, message: 'Already exists' };

    return { status: 500, message: 'Internal server error' };
  }
}

// src/app.module.ts
providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }],
```

Now every error in the API has the same shape: `{ statusCode, message }`, where `message` is a string or a list of strings.

---

### ✍️ Your Turn: Part 3

```ts
// 1. A custom exception for "you can borrow at most 3 books"
export class TooManyBooksException extends ____ {      // 400
  constructor(max: number) {
    ____(`You can borrow at most ${max} books`);
  }
}

// 2. Rewrite findOne with an exception (no try/catch!)
async findOne(id: string) {
  const book = await this.bookModel.findById(id);
  if (!book) ____ new ____('Book not found');
  return book;
}

// 3. Complete toResponse() in the filter
private toResponse(exception: unknown) {
  if (exception instanceof ____) {
    // TODO: status from exception.getStatus(), message from exception.getResponse()
  }
  const err = exception as MongooseLikeError;
  // TODO: CastError           → 400 'Invalid id'
  // TODO: ValidationError     → 400 with the list of messages
  // TODO: err.code === 11000  → 409 'Already exists'   (duplicate unique value)
  // TODO: anything else       → 500 'Internal server error'
}

// 4. Register it for the whole app
providers: [{ provide: ____, useClass: AllExceptionsFilter }]
```

**Test:** create two contacts with the same email (Session 10). Before the filter you had to catch `11000` in the service. Remove that `try/catch`: what does the client get now?

<details>
<summary>✅ Solution</summary>

```ts
export class TooManyBooksException extends BadRequestException {
  constructor(max: number) {
    super(`You can borrow at most ${max} books`);
  }
}

async findOne(id: string) {
  const book = await this.bookModel.findById(id);
  if (!book) throw new NotFoundException('Book not found');
  return book;
}

private toResponse(exception: unknown): { status: number; message: string | string[] } {
  if (exception instanceof HttpException) {
    const body = exception.getResponse();
    const message = typeof body === 'string' ? body : (body as { message: string | string[] }).message;
    return { status: exception.getStatus(), message };
  }
  const err = exception as MongooseLikeError;
  if (err.name === 'CastError') return { status: 400, message: 'Invalid id' };
  if (err.name === 'ValidationError') {
    return { status: 400, message: Object.values(err.errors ?? {}).map((e) => e.message) };
  }
  if (err.code === 11000) return { status: 409, message: 'Already exists' };
  return { status: 500, message: 'Internal server error' };
}

providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }]
```

The second contact now gets `409 { statusCode: 409, message: 'Already exists' }`, and the service has no `try/catch`.

</details>

---

## Part 4: Authentication & Authorization

### Password hashing — bcrypt

> 💡 **In simple words:** **Hashing** turns a password into scrambled text that can't be turned back. At login, we hash what the user typed and compare it with the saved hash. A **salt** (random extra text) makes the same password produce different hashes, so attackers can't use ready-made lists of common passwords and their hashes.

Never store plain-text passwords. bcrypt = one-way hash + salt (defeats rainbow tables).

```bash
npm install bcrypt @nestjs/jwt
npm install -D @types/bcrypt
```

```ts
import * as bcrypt from 'bcrypt';

const hash = await bcrypt.hash(password, 10);              // store this
const ok   = await bcrypt.compare(password, user.password); // verify at login
```

Best practice — hash in a schema `pre('save')` hook, so no code path forgets it, and hide the password from queries with `select: false`:

```ts
// src/users/schemas/user.schema.ts
@Schema({ timestamps: true })
export class User {
  @Prop({ required: true, trim: true, minlength: 2 })
  name: string;

  @Prop({ required: true, unique: true, lowercase: true })
  email: string;

  @Prop({ required: true, select: false })        // never returned unless you ask with '+password'
  password: string;

  @Prop({ enum: ['user', 'admin'], default: 'user' })
  role: string;
}

export type UserDocument = HydratedDocument<User>;
export const UserSchema = SchemaFactory.createForClass(User);

UserSchema.pre('save', async function () {
  if (!this.isModified('password')) return;       // don't re-hash on unrelated updates
  this.password = await bcrypt.hash(this.password, 10);
});
```

### The modules

> 💡 **In simple words:** Two features, two modules:
> - **UsersModule** owns the `User` schema and a `UsersService` (create, find).
> - **AuthModule** handles register, login and tokens. It **imports** UsersModule to use `UsersService`.

```ts
// src/users/users.service.ts
@Injectable()
export class UsersService {
  constructor(@InjectModel(User.name) private readonly userModel: Model<User>) {}

  create(data: { name: string; email: string; password: string }) {
    return this.userModel.create(data);                       // pre-save hook hashes it
  }

  findByEmailWithPassword(email: string) {
    return this.userModel.findOne({ email }).select('+password');
  }

  findById(id: string) {
    return this.userModel.findById(id);                       // no password (select: false)
  }

  findAll() {
    return this.userModel.find();
  }
}

// src/users/users.module.ts
@Module({
  imports: [MongooseModule.forFeature([{ name: User.name, schema: UserSchema }])],
  providers: [UsersService],
  exports: [UsersService],                                    // AuthModule needs it
})
export class UsersModule {}
```

```ts
// src/auth/auth.module.ts
import { JwtModule } from '@nestjs/jwt';

@Module({
  imports: [
    UsersModule,
    JwtModule.registerAsync({
      global: true,                                           // JwtService usable in every module
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        secret: config.get<string>('JWT_SECRET'),
        signOptions: { expiresIn: '1d' },
      }),
    }),
  ],
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```

### Register

> 💡 **In simple words:** Registering is three steps:
> 1. Check that the email isn't already taken.
> 2. Save the new user (the pre-save hook hashes the password).
> 3. Reply 201, **without** the password.

```ts
// src/auth/auth.service.ts
@Injectable()
export class AuthService {
  constructor(
    private readonly usersService: UsersService,
    private readonly jwtService: JwtService,
  ) {}

  async register(dto: RegisterDto) {
    const existing = await this.usersService.findByEmailWithPassword(dto.email);
    if (existing) throw new ConflictException('Email already registered');

    const user = await this.usersService.create(dto);
    return { id: user._id, name: user.name, email: user.email, role: user.role };  // never the hash
  }
```

> `select: false` only hides the field in **queries**. The document returned by `create()` still contains the hash, so pick the fields you send back.

### Login → JWT

> 💡 **In simple words:** To log in, find the user by email, then compare the password with bcrypt. If it's correct, create a **JWT**: a signed token that contains the user's id and role. The signature, made with your secret key, proves that your server created the token and nobody changed it.

JWT = self-contained signed token (`header.payload.signature`). Client sends it as `Authorization: Bearer <token>` on every protected request — the server verifies the signature; no session storage needed.

```ts
  async login(dto: LoginDto) {
    const user = await this.usersService.findByEmailWithPassword(dto.email);

    // Generic message — never reveal WHICH field was wrong
    if (!user || !(await bcrypt.compare(dto.password, user.password))) {
      throw new UnauthorizedException('Invalid credentials');
    }

    const payload: JwtPayload = { userId: user._id.toString(), role: user.role };  // minimal — no secrets
    return { token: await this.jwtService.signAsync(payload) };
  }
}

// src/auth/jwt-payload.ts
export interface JwtPayload {
  userId: string;
  role: string;
}
```

```ts
// src/auth/auth.controller.ts
@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('register')
  register(@Body() dto: RegisterDto) {          // 201
    return this.authService.register(dto);
  }

  @Post('login')
  @HttpCode(200)                                // login creates nothing
  login(@Body() dto: LoginDto) {
    return this.authService.login(dto);
  }
}
```

### `JwtAuthGuard` — only logged-in users

> 💡 **In simple words:** The guard runs before the protected route. It reads the token from the `Authorization` header, verifies it, and puts the payload in `req.user` for the next steps. No valid token → 401.

```ts
// src/auth/guards/jwt-auth.guard.ts
@Injectable()
export class JwtAuthGuard implements CanActivate {
  constructor(private readonly jwtService: JwtService) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const req = context.switchToHttp().getRequest();
    const [type, token] = req.headers.authorization?.split(' ') ?? [];
    if (type !== 'Bearer' || !token) throw new UnauthorizedException('Not logged in');

    try {
      req.user = await this.jwtService.verifyAsync<JwtPayload>(token);   // { userId, role, iat, exp }
    } catch {
      throw new UnauthorizedException('Invalid or expired token');
    }
    return true;
  }
}
```

```ts
// src/auth/decorators/current-user.decorator.ts — read req.user nicely
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const CurrentUser = createParamDecorator(
  (_data: unknown, ctx: ExecutionContext) => ctx.switchToHttp().getRequest().user,
);

// usage
@Get('me')
@UseGuards(JwtAuthGuard)
me(@CurrentUser() user: JwtPayload) {
  return this.usersService.findById(user.userId);
}
```

### Role-based access control (RBAC)

> 💡 **In simple words:** With role-based access control, permissions depend on the user's **role**. `@Roles('admin')` writes "admins only" on a route, and `RolesGuard` reads it. The roles guard runs **after** `JwtAuthGuard`, because it needs `req.user`. Everyone without the role gets 403.

**401** = not authenticated (who are you?). **403** = authenticated but not permitted (you can't do this).

```ts
// src/auth/decorators/roles.decorator.ts
export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);

// src/auth/guards/roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private readonly reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.getAllAndOverride<string[]>(ROLES_KEY, [context.getHandler(), context.getClass()]);
    if (!roles) return true;

    const user: JwtPayload = context.switchToHttp().getRequest().user;
    if (!roles.includes(user.role)) throw new ForbiddenException('Forbidden');
    return true;
  }
}

// usage — guards run left to right: first "who are you?", then "are you allowed?"
@Delete(':id')
@HttpCode(204)
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
remove(@Param('id', ParseObjectIdPipe) id: string) { ... }
```

---

### ✍️ Your Turn: Part 4

Add `JWT_SECRET=some-long-random-text` to `.env` first.

```ts
// 1. users/schemas/user.schema.ts: hash the password automatically before saving
UserSchema.pre('save', async function () {        // a normal function, NOT an arrow (we need `this`)
  if (!this.____('password')) return;             // only when the password changed
  this.password = await bcrypt.____(this.password, 10);
});
```

```ts
// 2. auth/auth.service.ts
async register(dto: RegisterDto) {
  const exists = await this.usersService.findByEmailWithPassword(dto.email);
  if (exists) throw new ____('Email already registered');          // 409

  const user = await this.usersService.create(dto);
  return { id: user._id, name: user.name, email: user.email };      // never send the password
}

// 3. Login
async login(dto: LoginDto) {
  const user = await this.usersService.findByEmailWithPassword(dto.email);
  if (!user || !(await bcrypt.____(dto.password, user.password))) {
    throw new UnauthorizedException('____');   // the SAME message for a wrong email or a wrong password
  }

  const payload = { userId: user._id.toString(), role: user.role };
  return { token: await this.jwtService.____(payload) };
}
```

```ts
// 4. auth/guards/jwt-auth.guard.ts
async canActivate(context: ExecutionContext) {
  const req = context.switchToHttp().getRequest();
  const [type, token] = req.headers.____?.split(' ') ?? [];
  if (type !== '____' || !token) throw new UnauthorizedException('Not logged in');
  try {
    req.____ = await this.jwtService.____(token);
  } catch {
    throw new UnauthorizedException('Invalid or expired token');
  }
  return true;
}

// 5. auth/guards/roles.guard.ts
const user = context.switchToHttp().getRequest().user;
if (!roles.includes(user.____)) throw new ____('Forbidden');       // 403

// 6. TODO: GET /api/auth/me (protected) → the logged-in user WITHOUT the password
//    hint: @UseGuards(JwtAuthGuard) + @CurrentUser() + usersService.findById(user.userId)

// 7. TODO: protect the book routes: anyone can GET, logged-in users can POST,
//    only admins can DELETE
```

<details>
<summary>✅ Solution</summary>

```ts
UserSchema.pre('save', async function () {
  if (!this.isModified('password')) return;
  this.password = await bcrypt.hash(this.password, 10);
});

async register(dto: RegisterDto) {
  const exists = await this.usersService.findByEmailWithPassword(dto.email);
  if (exists) throw new ConflictException('Email already registered');
  const user = await this.usersService.create(dto);
  return { id: user._id, name: user.name, email: user.email };
}

async login(dto: LoginDto) {
  const user = await this.usersService.findByEmailWithPassword(dto.email);
  if (!user || !(await bcrypt.compare(dto.password, user.password))) {
    throw new UnauthorizedException('Invalid credentials');
  }
  const payload = { userId: user._id.toString(), role: user.role };
  return { token: await this.jwtService.signAsync(payload) };
}

// guard
const [type, token] = req.headers.authorization?.split(' ') ?? [];
if (type !== 'Bearer' || !token) throw new UnauthorizedException('Not logged in');
req.user = await this.jwtService.verifyAsync(token);

// roles guard
if (!roles.includes(user.role)) throw new ForbiddenException('Forbidden');
```

```ts
// 6: auth.controller.ts (AuthController injects UsersService too)
@Get('me')
@UseGuards(JwtAuthGuard)
async me(@CurrentUser() user: JwtPayload) {
  const found = await this.usersService.findById(user.userId);
  if (!found) throw new NotFoundException('User not found');
  return found;
}

// 7: books.controller.ts
@Get()      findAll() { ... }                                  // public
@Get(':id') findOne(...) { ... }                               // public

@Post()
@UseGuards(JwtAuthGuard)
create(@Body() dto: CreateBookDto) { ... }

@Delete(':id')
@HttpCode(204)
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
remove(@Param('id', ParseObjectIdPipe) id: string) { ... }
```

To make yourself an admin while testing, change your user's `role` to `"admin"` in Compass, then log in again to get a new token (the role is inside the token).

</details>

---

## Lab: Auth-Protected To-Do API

```
1. users/        → User schema { name, email (unique), password (select: false, hashed via pre-save),
                   role: enum ['user','admin'] default 'user' } + UsersService (exported)
2. auth/         → RegisterDto, LoginDto, AuthService (register, login → JwtService.signAsync),
                   AuthController (POST /api/auth/register, POST /api/auth/login, GET /api/auth/me)
3. auth/guards   → JwtAuthGuard + RolesGuard; auth/decorators → @CurrentUser, @Roles
4. todos         → @UseGuards(JwtAuthGuard) on the whole controller; DELETE = admin only
5. All errors flow into AllExceptionsFilter (registered with APP_FILTER)
```

**Test flow in Postman:**
```
POST /api/auth/register  { name, email, password }  → 201
POST /api/auth/register  (same email again)         → 409
POST /api/auth/login     { email, password }        → { token }
GET  /api/todos          (no token)                  → 401
GET  /api/todos          Authorization: Bearer <t>  → 200
DELETE /api/todos/<id>   (role: user)                → 403
DELETE /api/todos/<id>   (role: admin)               → 204
```

**Challenges:** `GET /api/auth/me`; a refresh-token endpoint; a password-strength rule with `@Matches(/^(?=.*[A-Za-z])(?=.*\d).{8,}$/)`.

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. Why do we validate data twice: in the request (DTO) and in the schema?
2. What is the difference between `min` and `minlength`? Between `@Min()` and `@MinLength()`?
3. What does `PartialType(CreateBookDto)` give you?
4. Why don't NestJS services need `try/catch` around database calls?
5. Why do we write a global exception filter for Mongoose errors?
6. Why must we never save passwords as plain text? What does bcrypt do?
7. Why does login say "Invalid credentials" and not "Wrong password"?
8. What are the three parts of a JWT? Can anyone read the payload?
9. What is the difference between 401 and 403? Which guard gives each one?
10. Why must `JwtAuthGuard` come before `RolesGuard` in `@UseGuards(...)`?

<details>
<summary>✅ Solution</summary>

1. Request validation stops bad data early with clear messages. Schema validation is the last protection: it also covers data saved from other places in the code.
2. `min` is for numbers/dates, `minlength` for string length (schema). The same idea in DTOs: `@Min()` for numbers, `@MinLength()` for strings.
3. A new DTO class with the same fields and rules, but every field optional. Perfect for PATCH.
4. NestJS catches errors thrown in `async` methods and passes them to the exception filters.
5. Mongoose errors (`CastError`, `ValidationError`, duplicate key 11000) are not `HttpException`s, so NestJS would answer 500. The filter turns them into 400/409 with clear messages.
6. If the database leaks, everyone's password is exposed. bcrypt makes a one-way hash with a salt, so the original password can't be read back.
7. So attackers can't find out which emails are registered.
8. Header, payload and signature. Yes, the payload is only base64-encoded (not encrypted), so never put secrets in it. The signature only proves it wasn't changed.
9. 401 means you are not logged in (or the token is bad) → `JwtAuthGuard`. 403 means you are logged in but not allowed → `RolesGuard`.
10. Guards run left to right. `RolesGuard` reads `req.user`, which only exists after `JwtAuthGuard` has verified the token.

</details>

### B. Find the Bug

```ts
// Bug 1: passwords appear as plain text in the database
const user = await this.userModel.create({ email, password });   // the schema has no pre-save hook

// Bug 2: security problem
if (!user) throw new UnauthorizedException('Email not found');
if (!(await bcrypt.compare(password, user.password))) {
  throw new UnauthorizedException('Wrong password');
}

// Bug 3: three problems in one module
JwtModule.register({ secret: 'secret' })
// ...
this.jwtService.signAsync({ userId: user._id, password: user.password });

// Bug 4: login always says "Invalid credentials", even with the right password.
//        In Compass the user has a hashed password. (The schema has select: false.)
const user = await this.userModel.findOne({ email });
if (!user || !(await bcrypt.compare(password, user.password))) throw new UnauthorizedException('Invalid credentials');

// Bug 5: "Cannot set properties of undefined (setting 'password')"
UserSchema.pre('save', async () => {
  this.password = await bcrypt.hash(this.password, 10);
});

// Bug 6: "Cannot read properties of undefined (reading 'role')"
@UseGuards(RolesGuard, JwtAuthGuard)
@Roles('admin')
@Delete(':id')
remove() { ... }

// Bug 7: anyone can register as admin with { ..., "role": "admin" }
app.useGlobalPipes(new ValidationPipe());
// RegisterDto has no role field; the service does userModel.create(dto)
```

<details>
<summary>✅ Solution</summary>

1. Add the pre-save hook (or hash before `create`):
   ```ts
   UserSchema.pre('save', async function () {
     if (!this.isModified('password')) return;
     this.password = await bcrypt.hash(this.password, 10);
   });
   ```
2. Use the same message for both cases: `'Invalid credentials'`.
3. (a) the secret is hardcoded and weak (b) no `expiresIn` (c) the password hash is in the payload (anyone can read it).
   ```ts
   JwtModule.registerAsync({
     global: true,
     inject: [ConfigService],
     useFactory: (config: ConfigService) => ({ secret: config.get('JWT_SECRET'), signOptions: { expiresIn: '1d' } }),
   })
   this.jwtService.signAsync({ userId: user._id.toString(), role: user.role });
   ```
4. `select: false` hides the password, so `user.password` is `undefined`. Ask for it: `.findOne({ email }).select('+password')`.
5. Arrow functions have no `this`. Use `async function () { ... }`.
6. `RolesGuard` runs before `JwtAuthGuard`, so `req.user` doesn't exist yet. Swap them: `@UseGuards(JwtAuthGuard, RolesGuard)`.
7. Without `whitelist: true`, unknown fields stay in the body and go straight into `create()`. Use `new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true })`.

</details>

### C. Coding Tasks

**Task 1: DTOs for books (easy).** `CreateBookDto` and `UpdateBookDto extends PartialType(CreateBookDto)` on `POST /api/books` and `PATCH /api/books/:id`. For PATCH, every field is optional, but at least one must be sent: write a `NonEmptyBodyPipe` and use it as `@Body(NonEmptyBodyPipe) dto: UpdateBookDto`.

**Task 2: Clean services (medium).** Register the `AllExceptionsFilter` from Part 3 and remove every `try/catch` from the books and contacts services. A duplicate ISBN or email must still give 409, a bad id 400, and a schema error 400.

**Task 3: Register + login flow (medium).** Build `POST /api/auth/register`, `POST /api/auth/login` and `GET /api/auth/me`, then test this flow in Postman:

```
register → 201          register same email again → 409
login wrong password → 401          login correct → { token }
GET /api/auth/me without token → 401          with token → user (no password)
```

**Task 4: My notes (medium).** A `Note` schema `{ text, owner: { type: Types.ObjectId, ref: 'User' } }` in a `NotesModule`. The whole controller is protected with `@UseGuards(JwtAuthGuard)`:
- `POST /api/notes` sets `owner` from the token (`@CurrentUser()`), never from the body
- `GET /api/notes` returns only **my** notes
- `DELETE /api/notes/:id` deletes only if it's my note, otherwise 404

**Task 5: Admin users list (easy).** `GET /api/users` is admin only and returns all users without passwords.

**Challenge: Change password.** `PATCH /api/auth/password` (protected) with `{ currentPassword, newPassword }` (DTO: new password at least 6 characters). Check the current password (401 if wrong), then save. The pre-save hook hashes it. Log in with the new password to prove it works.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```ts
// Task 1: common/pipes/non-empty-body.pipe.ts
@Injectable()
export class NonEmptyBodyPipe implements PipeTransform {
  transform(value: unknown) {
    if (!value || typeof value !== 'object' || Object.keys(value).length === 0) {
      throw new BadRequestException('Send at least one field to update');
    }
    return value;
  }
}

// books.controller.ts
@Post()
@UseGuards(JwtAuthGuard)
create(@Body() dto: CreateBookDto) { return this.booksService.create(dto); }

@Patch(':id')
@UseGuards(JwtAuthGuard)
update(@Param('id', ParseObjectIdPipe) id: string, @Body(NonEmptyBodyPipe) dto: UpdateBookDto) {
  return this.booksService.update(id, dto);
}
```

The global `ValidationPipe` runs first and strips unknown fields, so `{ "hacker": 1 }` becomes `{}` and `NonEmptyBodyPipe` rejects it.

```ts
// Task 2: app.module.ts
providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }]

// contacts.service.ts — no try/catch any more
create(dto: CreateContactDto) {
  return this.contactModel.create(dto);       // duplicate email → 11000 → filter → 409
}
```

```ts
// Task 3: auth.controller.ts
@Controller('auth')
export class AuthController {
  constructor(
    private readonly authService: AuthService,
    private readonly usersService: UsersService,
  ) {}

  @Post('register')
  register(@Body() dto: RegisterDto) { return this.authService.register(dto); }

  @Post('login')
  @HttpCode(200)
  login(@Body() dto: LoginDto) { return this.authService.login(dto); }

  @Get('me')
  @UseGuards(JwtAuthGuard)
  async me(@CurrentUser() user: JwtPayload) {
    const found = await this.usersService.findById(user.userId);
    if (!found) throw new NotFoundException('User not found');
    return found;
  }
}
```

```ts
// Task 4: notes/schemas/note.schema.ts
@Schema({ timestamps: true })
export class Note {
  @Prop({ required: true, trim: true })
  text: string;

  @Prop({ type: Types.ObjectId, ref: 'User', required: true })
  owner: Types.ObjectId;
}
export const NoteSchema = SchemaFactory.createForClass(Note);

// notes/notes.service.ts
@Injectable()
export class NotesService {
  constructor(@InjectModel(Note.name) private readonly noteModel: Model<Note>) {}

  create(text: string, ownerId: string) {
    return this.noteModel.create({ text, owner: ownerId });
  }

  findMine(ownerId: string) {
    return this.noteModel.find({ owner: ownerId }).sort({ createdAt: -1 });
  }

  async removeMine(id: string, ownerId: string) {
    const note = await this.noteModel.findOneAndDelete({ _id: id, owner: ownerId });
    if (!note) throw new NotFoundException('Note not found');
  }
}

// notes/notes.controller.ts
@Controller('notes')
@UseGuards(JwtAuthGuard)                 // every route below is protected
export class NotesController {
  constructor(private readonly notesService: NotesService) {}

  @Get()
  findMine(@CurrentUser() user: JwtPayload) {
    return this.notesService.findMine(user.userId);
  }

  @Post()
  create(@Body() dto: CreateNoteDto, @CurrentUser() user: JwtPayload) {
    return this.notesService.create(dto.text, user.userId);
  }

  @Delete(':id')
  @HttpCode(204)
  remove(@Param('id', ParseObjectIdPipe) id: string, @CurrentUser() user: JwtPayload) {
    return this.notesService.removeMine(id, user.userId);
  }
}

export class CreateNoteDto {
  @IsString() @IsNotEmpty() text: string;
}
```

```ts
// Task 5: users/users.controller.ts (add it to UsersModule's controllers)
@Controller('users')
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();     // select: false → no passwords
  }
}
```

```ts
// Challenge
export class ChangePasswordDto {
  @IsString() currentPassword: string;
  @IsString() @MinLength(6, { message: 'New password needs at least 6 characters' }) newPassword: string;
}

// users.service.ts
findByIdWithPassword(id: string) {
  return this.userModel.findById(id).select('+password');
}

// auth.service.ts
async changePassword(userId: string, dto: ChangePasswordDto) {
  const user = await this.usersService.findByIdWithPassword(userId);
  if (!user) throw new NotFoundException('User not found');

  const ok = await bcrypt.compare(dto.currentPassword, user.password);
  if (!ok) throw new UnauthorizedException('Current password is wrong');

  user.password = dto.newPassword;          // the pre-save hook hashes it
  await user.save();
  return { message: 'Password changed' };
}

// auth.controller.ts
@Patch('password')
@UseGuards(JwtAuthGuard)
changePassword(@CurrentUser() user: JwtPayload, @Body() dto: ChangePasswordDto) {
  return this.authService.changePassword(user.userId, dto);
}
```

</details>

---

## Review Questions

1. Why validate at BOTH request and schema level? What does each catch?
2. `min` vs `minlength` — which fields do they apply to?
3. What do `whitelist`, `forbidNonWhitelisted` and `transform` do on the `ValidationPipe`?
4. Why do NestJS services need no `try/catch` around async database calls?
5. How does the global filter turn a duplicate email into 409?
6. Why is bcrypt better than SHA-256 for passwords?
7. Why generic "Invalid credentials" instead of "wrong password"?
8. JWT: what's in the three segments? Why shouldn't the payload contain secrets?
9. 401 vs 403? Which guard produces each?
10. What does the pre-save hook's `isModified('password')` check prevent? Why `select: false` + `.select('+password')`?

## Common Mistakes

- Plain-text passwords anywhere (DB, logs, responses) — or returning the document from `create()` with the hash in it
- Revealing whether email or password failed
- JWT secret hardcoded or weak; sensitive data in payload; no `expiresIn`
- Checking the `Bearer` prefix incorrectly or forgetting it
- `@UseGuards(RolesGuard, JwtAuthGuard)` — wrong order
- `ValidationPipe` without `whitelist` → mass assignment (`role: 'admin'`)
- Forgetting `runValidators` on updates
- `select: false` on the password but forgetting `.select('+password')` at login
- Catching errors in every service instead of one global filter

---

**Session 11 complete.** Next: Session 12 — Advanced Mongoose, Security, Testing & Deployment.
