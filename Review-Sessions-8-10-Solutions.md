# Review Session 8–10 — Solutions (Instructor Only)

> **Do not share this file with students before the session ends.**
> It contains: the answers to the "🤔 Think" questions, the full project code for every milestone, the mongosh seed script and queries, the grading rubric, and instructor notes.
>
> Student file: **Review-Sessions-8-10.md**

---

## Answers to the "🤔 Think" Questions

**Milestone 2: why is `bookedSeats` not in the DTOs?**
`bookedSeats` must only change through `book` / `cancel`, where the rules are checked. If a client could send it, they could set `bookedSeats: 0` on a full workshop. Because `main.ts` uses `whitelist` + `forbidNonWhitelisted`, sending it gives **400** `property bookedSeats should not exist`.

**Milestone 5: why 409 and not 400?**
The request is valid (`seats: 3` is a correct number). It conflicts with the current state of the data. That is what 409 means.

**Milestone 6: why can the guard be open when `ADMIN_API_KEY` is missing?**
No header = `undefined`, no config value = `undefined`. `undefined !== undefined` is `false`, so the check passes. The guard must refuse when the key is not configured (`!key || header !== key`).

**Milestone 7, test 13: why `WEB-301`, `bookedSeats: 0`, `isPublished: true`, `createdAt`?**
`uppercase: true` in the schema · `default: 0` / `default: true` in the schema · `timestamps: true` in `@Schema()`.

---

## Project Solution

### Milestone 0: Setup

```bash
nest new workshop-hub-api          # choose npm
cd workshop-hub-api
npm i @nestjs/mongoose mongoose @nestjs/config class-validator class-transformer helmet
```

```bash
# .env                              ← in .gitignore
MONGODB_URI=mongodb://127.0.0.1:27017/workshop-hub
PORT=3000
ADMIN_API_KEY=admin-secret-123

# .env.example                      ← committed
MONGODB_URI=
PORT=
ADMIN_API_KEY=
```

```ts
// src/main.ts (Milestone 6 adds more)
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }));
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

```ts
// src/app.module.ts (Milestone 6 has the final version)
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { MongooseModule } from '@nestjs/mongoose';
import { AppController } from './app.controller';

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
  controllers: [AppController],
})
export class AppModule {}
```

---

### Milestone 1: In-Memory Warm-Up

```ts
// src/app.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller()
export class AppController {
  @Get('health')
  health() {
    return { status: 'ok', uptime: process.uptime() };
  }

  @Get('info')
  info() {
    return { name: 'Workshop Hub API', version: '1.0.0', time: new Date().toISOString() };
  }
}
```

```ts
// src/categories/categories.data.ts
export const CATEGORIES = [
  { id: 1, slug: 'web', name: 'Web Development' },
  { id: 2, slug: 'mobile', name: 'Mobile Apps' },
  { id: 3, slug: 'design', name: 'UI/UX Design' },
  { id: 4, slug: 'data', name: 'Data & Databases' },
  { id: 5, slug: 'career', name: 'Career Skills' },
];

export const CATEGORY_SLUGS = CATEGORIES.map((c) => c.slug);   // ['web', 'mobile', ...]
```

```ts
// src/categories/categories.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { CATEGORIES } from './categories.data';

@Injectable()
export class CategoriesService {
  findAll() {
    return CATEGORIES;
  }

  findOne(id: number) {
    const category = CATEGORIES.find((c) => c.id === id);
    if (!category) throw new NotFoundException('Category not found');
    return category;
  }
}
```

```ts
// src/categories/categories.controller.ts
import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';
import { CategoriesService } from './categories.service';

@Controller('categories')
export class CategoriesController {
  constructor(private readonly categoriesService: CategoriesService) {}

  @Get()
  findAll() {
    return this.categoriesService.findAll();
  }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.categoriesService.findOne(id);
  }
}
```

```ts
// src/categories/categories.module.ts
import { Module } from '@nestjs/common';
import { CategoriesController } from './categories.controller';
import { CategoriesService } from './categories.service';

@Module({
  controllers: [CategoriesController],
  providers: [CategoriesService],
})
export class CategoriesModule {}
```

---

### Milestone 2: Schema, DTOs & Seed Data

```ts
// src/workshops/schemas/workshop.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument } from 'mongoose';
import { CATEGORY_SLUGS } from '../../categories/categories.data';

@Schema({ timestamps: true })
export class Workshop {
  @Prop({
    required: [true, 'Code is required'],
    unique: true,
    uppercase: true,
    trim: true,
    match: [/^[A-Z0-9-]{3,20}$/, 'Code must be 3-20 letters, digits or dashes'],
  })
  code: string;

  @Prop({ required: [true, 'Title is required'], trim: true, minlength: 3 })
  title: string;

  @Prop({ trim: true })
  description?: string;

  @Prop({ required: true, enum: CATEGORY_SLUGS })
  category: string;

  @Prop({ required: true, trim: true })
  city: string;

  @Prop({ required: true })
  date: Date;

  @Prop({ default: 0, min: [0, 'Price cannot be negative'] })
  price: number;

  @Prop({ required: true, min: [1, 'Capacity must be at least 1'] })
  capacity: number;

  @Prop({ default: 0, min: 0 })
  bookedSeats: number;

  @Prop({ default: true })
  isPublished: boolean;

  @Prop([String])
  tags: string[];
}

export type WorkshopDocument = HydratedDocument<Workshop>;
export const WorkshopSchema = SchemaFactory.createForClass(Workshop);
```

```ts
// src/workshops/dto/create-workshop.dto.ts
import {
  IsBoolean, IsDateString, IsIn, IsInt, IsNotEmpty, IsNumber,
  IsOptional, IsString, Matches, Min, MinLength,
} from 'class-validator';
import { CATEGORY_SLUGS } from '../../categories/categories.data';

export class CreateWorkshopDto {
  @Matches(/^[A-Za-z0-9-]{3,20}$/, { message: 'code must be 3-20 letters, digits or dashes' })
  code: string;

  @IsString()
  @MinLength(3)
  title: string;

  @IsOptional()
  @IsString()
  description?: string;

  @IsIn(CATEGORY_SLUGS, { message: `category must be one of: ${CATEGORY_SLUGS.join(', ')}` })
  category: string;

  @IsString()
  @IsNotEmpty()
  city: string;

  @IsDateString()
  date: string;

  @IsOptional()
  @IsNumber()
  @Min(0)
  price?: number;

  @IsInt()
  @Min(1)
  capacity: number;

  @IsOptional()
  @IsBoolean()
  isPublished?: boolean;

  @IsOptional()
  @IsString({ each: true })
  tags?: string[];
}
```

```ts
// src/workshops/dto/update-workshop.dto.ts — every field optional
// (same imports as create-workshop.dto.ts; Session 11 shows the PartialType shortcut)
export class UpdateWorkshopDto {
  @IsOptional() @Matches(/^[A-Za-z0-9-]{3,20}$/, { message: 'code must be 3-20 letters, digits or dashes' }) code?: string;
  @IsOptional() @IsString() @MinLength(3) title?: string;
  @IsOptional() @IsString() description?: string;
  @IsOptional() @IsIn(CATEGORY_SLUGS) category?: string;
  @IsOptional() @IsString() @IsNotEmpty() city?: string;
  @IsOptional() @IsDateString() date?: string;
  @IsOptional() @IsNumber() @Min(0) price?: number;
  @IsOptional() @IsInt() @Min(1) capacity?: number;
  @IsOptional() @IsBoolean() isPublished?: boolean;
  @IsOptional() @IsString({ each: true }) tags?: string[];
}
```

```ts
// src/workshops/dto/book-seats.dto.ts
import { IsInt, Max, Min } from 'class-validator';

export class BookSeatsDto {
  @IsInt()
  @Min(1)
  @Max(10, { message: 'You can book at most 10 seats at once' })
  seats: number;
}
```

```ts
// src/workshops/workshops.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { Workshop, WorkshopSchema } from './schemas/workshop.schema';
import { WorkshopsController } from './workshops.controller';
import { WorkshopsService } from './workshops.service';

@Module({
  imports: [MongooseModule.forFeature([{ name: Workshop.name, schema: WorkshopSchema }])],
  controllers: [WorkshopsController],
  providers: [WorkshopsService],
})
export class WorkshopsModule {}
```

**Seed script (mongosh):**

```js
use('workshop-hub');

db.workshops.insertMany([
  { code: 'WEB-101',  title: 'Intro to HTML & CSS',        category: 'web',    city: 'Amman', date: new Date('2026-11-20T17:00:00Z'), price: 0,  capacity: 30, bookedSeats: 12, isPublished: true,  tags: ['html', 'css', 'beginner'] },
  { code: 'WEB-201',  title: 'NestJS REST APIs',           category: 'web',    city: 'Amman', date: new Date('2026-12-05T17:00:00Z'), price: 15, capacity: 25, bookedSeats: 25, isPublished: true,  tags: ['nestjs', 'api'] },
  { code: 'MOB-101',  title: 'Flutter Basics',             category: 'mobile', city: 'Irbid', date: new Date('2026-11-28T16:00:00Z'), price: 10, capacity: 20, bookedSeats: 5,  isPublished: true,  tags: ['flutter', 'dart'] },
  { code: 'DES-101',  title: 'Figma for Beginners',        category: 'design', city: 'Zarqa', date: new Date('2026-10-20T15:00:00Z'), price: 0,  capacity: 40, bookedSeats: 38, isPublished: true,  tags: ['figma', 'ui'] },
  { code: 'DATA-101', title: 'MongoDB Queries Lab',        category: 'data',   city: 'Amman', date: new Date('2026-12-12T17:00:00Z'), price: 0,  capacity: 15, bookedSeats: 3,  isPublished: true,  tags: ['mongodb', 'database'] },
  { code: 'CAR-101',  title: 'Write a Developer CV',       category: 'career', city: 'Irbid', date: new Date('2026-12-01T15:00:00Z'), price: 5,  capacity: 50, bookedSeats: 0,  isPublished: false, tags: ['cv', 'jobs'] },
  { code: 'DATA-201', title: 'Python Data Analysis',       category: 'data',   city: 'Zarqa', date: new Date('2027-01-10T16:00:00Z'), price: 20, capacity: 20, bookedSeats: 7,  isPublished: true,  tags: ['python', 'pandas'] },
  { code: 'DES-201',  title: 'Design Systems in Practice', category: 'design', city: 'Amman', date: new Date('2027-01-18T16:00:00Z'), price: 25, capacity: 18, bookedSeats: 18, isPublished: true,  tags: ['figma', 'design-system'] },
]);

db.workshops.countDocuments();   // 8
```

---

### Milestones 3–5: Service (final version)

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
```

```ts
// src/workshops/workshops.service.ts
import { BadRequestException, ConflictException, Injectable, NotFoundException } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { CATEGORY_SLUGS } from '../categories/categories.data';
import { CreateWorkshopDto } from './dto/create-workshop.dto';
import { UpdateWorkshopDto } from './dto/update-workshop.dto';
import { Workshop } from './schemas/workshop.schema';

interface WorkshopQuery {
  page: number;
  limit: number;
  city?: string;
  category?: string;
  search?: string;
}

@Injectable()
export class WorkshopsService {
  constructor(@InjectModel(Workshop.name) private readonly workshopModel: Model<Workshop>) {}

  // MongoDB "duplicate key" (11000) → 409. Anything else is thrown again unchanged.
  private handleDuplicate(err: unknown): never {
    if ((err as { code?: number }).code === 11000) {
      throw new ConflictException('A workshop with this code already exists');
    }
    throw err;
  }

  // ---------- Milestone 4: filter + search + pagination ----------
  async findAll(query: WorkshopQuery) {
    const page = Math.max(query.page, 1);
    const limit = Math.min(Math.max(query.limit, 1), 50);

    const filter: Record<string, unknown> = {};
    if (query.city) filter.city = query.city;
    if (query.category) filter.category = query.category;
    if (query.search) filter.title = { $regex: query.search, $options: 'i' };

    const [data, total] = await Promise.all([
      this.workshopModel.find(filter).sort({ date: 1 }).skip((page - 1) * limit).limit(limit),
      this.workshopModel.countDocuments(filter),
    ]);

    return { data, total, page, totalPages: Math.ceil(total / limit) };
  }

  findUpcoming() {
    return this.workshopModel.find({ isPublished: true, date: { $gte: new Date() } }).sort({ date: 1 });
  }

  // ---------- Milestone 3: CRUD ----------
  async findOne(id: string) {
    const workshop = await this.workshopModel.findById(id);
    if (!workshop) throw new NotFoundException('Workshop not found');
    return workshop;
  }

  async create(dto: CreateWorkshopDto) {
    try {
      return await this.workshopModel.create(dto);
    } catch (err) {
      this.handleDuplicate(err);
    }
  }

  async update(id: string, dto: UpdateWorkshopDto) {
    if (dto.capacity !== undefined) {
      const current = await this.findOne(id);                 // also gives the 404
      if (dto.capacity < current.bookedSeats) {
        throw new BadRequestException(`Capacity cannot be less than booked seats (${current.bookedSeats})`);
      }
    }

    try {
      const workshop = await this.workshopModel.findByIdAndUpdate(id, dto, { new: true, runValidators: true });
      if (!workshop) throw new NotFoundException('Workshop not found');
      return workshop;
    } catch (err) {
      this.handleDuplicate(err);
    }
  }

  // Milestone 5: safe delete
  async remove(id: string) {
    const workshop = await this.findOne(id);
    if (workshop.bookedSeats > 0) throw new ConflictException('Cannot delete a workshop with bookings');
    await workshop.deleteOne();
  }

  // ---------- Milestone 5: business rules ----------
  async book(id: string, seats: number) {
    const workshop = await this.findOne(id);                              // 404

    if (!workshop.isPublished) throw new BadRequestException('Workshop is not open for booking');
    if (workshop.date < new Date()) throw new BadRequestException('Workshop has already happened');

    const seatsLeft = workshop.capacity - workshop.bookedSeats;
    if (seats > seatsLeft) throw new ConflictException(`Only ${seatsLeft} seats left`);

    workshop.bookedSeats += seats;
    return workshop.save();
  }

  async cancel(id: string, seats: number) {
    const workshop = await this.findOne(id);
    if (seats > workshop.bookedSeats) throw new ConflictException('Cannot cancel more seats than booked');
    workshop.bookedSeats -= seats;
    return workshop.save();
  }

  // ---------- Bonus ----------
  async stats() {
    const now = new Date();
    const [total, upcoming, free, perCategory] = await Promise.all([
      this.workshopModel.countDocuments(),
      this.workshopModel.countDocuments({ isPublished: true, date: { $gte: now } }),
      this.workshopModel.countDocuments({ price: 0 }),
      Promise.all(CATEGORY_SLUGS.map((slug) => this.workshopModel.countDocuments({ category: slug }))),
    ]);

    const byCategory = Object.fromEntries(CATEGORY_SLUGS.map((slug, i) => [slug, perCategory[i]]));
    return { total, upcoming, free, byCategory };
  }

  async seats(id: string) {
    const w = await this.findOne(id);
    const seatsLeft = w.capacity - w.bookedSeats;
    return { code: w.code, capacity: w.capacity, bookedSeats: w.bookedSeats, seatsLeft, isFull: seatsLeft === 0 };
  }
}
```

> **Note for the discussion:** if two people book the last seat at the same moment, both can pass the check. Real apps do the check and the update in **one** database operation.

### Controller (final version)

```ts
// src/workshops/workshops.controller.ts
import {
  Body, Controller, DefaultValuePipe, Delete, Get, HttpCode,
  Param, ParseIntPipe, Patch, Post, Query, UseGuards,
} from '@nestjs/common';
import { ApiKeyGuard } from '../common/guards/api-key.guard';
import { ParseObjectIdPipe } from '../common/pipes/parse-object-id.pipe';
import { BookSeatsDto } from './dto/book-seats.dto';
import { CreateWorkshopDto } from './dto/create-workshop.dto';
import { UpdateWorkshopDto } from './dto/update-workshop.dto';
import { WorkshopsService } from './workshops.service';

@Controller('workshops')
export class WorkshopsController {
  constructor(private readonly workshopsService: WorkshopsService) {}

  // page/limit first: TypeScript doesn't allow a required parameter after optional ones
  @Get()
  findAll(
    @Query('page', new DefaultValuePipe(1), ParseIntPipe) page: number,
    @Query('limit', new DefaultValuePipe(10), ParseIntPipe) limit: number,
    @Query('city') city?: string,
    @Query('category') category?: string,
    @Query('search') search?: string,
  ) {
    return this.workshopsService.findAll({ page, limit, city, category, search });
  }

  // fixed routes BEFORE ':id'
  @Get('upcoming')
  findUpcoming() {
    return this.workshopsService.findUpcoming();
  }

  @Get('stats')                                       // bonus
  stats() {
    return this.workshopsService.stats();
  }

  @Get(':id')
  findOne(@Param('id', ParseObjectIdPipe) id: string) {
    return this.workshopsService.findOne(id);
  }

  @Get(':id/seats')                                   // bonus
  seats(@Param('id', ParseObjectIdPipe) id: string) {
    return this.workshopsService.seats(id);
  }

  @Post()
  @UseGuards(ApiKeyGuard)
  create(@Body() dto: CreateWorkshopDto) {
    return this.workshopsService.create(dto);
  }

  @Patch(':id')
  @UseGuards(ApiKeyGuard)
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: UpdateWorkshopDto) {
    return this.workshopsService.update(id, dto);
  }

  @Delete(':id')
  @HttpCode(204)
  @UseGuards(ApiKeyGuard)
  remove(@Param('id', ParseObjectIdPipe) id: string) {
    return this.workshopsService.remove(id);
  }

  @Patch(':id/book')
  book(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: BookSeatsDto) {
    return this.workshopsService.book(id, dto.seats);
  }

  @Patch(':id/cancel')
  cancel(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: BookSeatsDto) {
    return this.workshopsService.cancel(id, dto.seats);
  }
}
```

---

### Milestone 6: The Request Lifecycle

```ts
// src/common/middleware/logger.middleware.ts
import { IncomingMessage, ServerResponse } from 'http';

export function logger(req: IncomingMessage, res: ServerResponse, next: () => void) {
  console.log(`${req.method} ${req.url}`);
  next();
}
```

```ts
// src/common/guards/api-key.guard.ts
import { CanActivate, ExecutionContext, Injectable, UnauthorizedException } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  constructor(private readonly config: ConfigService) {}

  canActivate(context: ExecutionContext): boolean {
    const req = context.switchToHttp().getRequest();
    const key = this.config.get<string>('ADMIN_API_KEY');

    if (!key || req.headers['x-api-key'] !== key) {      // !key → closed, not open
      throw new UnauthorizedException('Invalid API key');
    }
    return true;
  }
}
```

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

    return next.handle().pipe(
      tap(() => this.logger.log(`${req.method} ${req.url} ${res.statusCode} - ${Date.now() - start}ms`)),
    );
  }
}
```

```ts
// src/common/filters/all-exceptions.filter.ts
import { ArgumentsHost, Catch, ExceptionFilter, HttpException, HttpStatus, Logger } from '@nestjs/common';
import { HttpAdapterHost } from '@nestjs/core';

@Catch()
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
// src/app.module.ts — final
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { APP_FILTER } from '@nestjs/core';
import { MongooseModule } from '@nestjs/mongoose';
import { AppController } from './app.controller';
import { CategoriesModule } from './categories/categories.module';
import { AllExceptionsFilter } from './common/filters/all-exceptions.filter';
import { WorkshopsModule } from './workshops/workshops.module';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),
    MongooseModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        uri: config.get<string>('MONGODB_URI'),
      }),
    }),
    CategoriesModule,
    WorkshopsModule,
  ],
  controllers: [AppController],
  providers: [{ provide: APP_FILTER, useClass: AllExceptionsFilter }],
})
export class AppModule {}
```

```ts
// src/main.ts — final
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import helmet from 'helmet';
import { AppModule } from './app.module';
import { LoggingInterceptor } from './common/interceptors/logging.interceptor';
import { logger } from './common/middleware/logger.middleware';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');
  app.use(helmet());
  app.enableCors({ origin: 'http://localhost:5173' });
  app.use(logger);
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }));
  app.useGlobalInterceptors(new LoggingInterceptor());
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

---

### Milestone 7b: mongosh Queries

```js
use('workshop-hub');

db.workshops.find({ city: 'Irbid' }, { code: 1, title: 1, _id: 0 });                    // MOB-101, CAR-101
db.workshops.find({ price: 0, isPublished: true });                                     // WEB-101, DES-101, DATA-101
db.workshops.find({ tags: 'figma' });                                                   // DES-101, DES-201
db.workshops.find({}, { title: 1, price: 1, _id: 0 }).sort({ price: -1 }).limit(3);     // DES-201, DATA-201, WEB-201
db.workshops.updateMany({ date: { $lt: new Date() } }, { $set: { isPublished: false } }); // DES-101
db.workshops.countDocuments({ category: { $in: ['web', 'data'] } });                    // 4 (+ any created)
```

The expected results are for the seed data. Workshops created or deleted in Postman change them a little.

---

### Bonus 3: Request Counter

```ts
// src/stats/stats.service.ts
@Injectable()
export class StatsService {
  totalRequests = 0;
}

// src/common/interceptors/count.interceptor.ts
@Injectable()
export class CountInterceptor implements NestInterceptor {
  constructor(private readonly stats: StatsService) {}
  intercept(context: ExecutionContext, next: CallHandler) {
    this.stats.totalRequests++;
    return next.handle();
  }
}

// app.module.ts → providers: [StatsService, { provide: APP_INTERCEPTOR, useClass: CountInterceptor }, ...]
// app.controller.ts → constructor(private readonly stats: StatsService) {}
//                     info() { return { ..., totalRequests: this.stats.totalRequests }; }
```

---

## Grading Rubric (100 points + 10 bonus)

| Milestone | What we check | Points |
|-----------|---------------|--------|
| 0 Setup | `.env` + `.env.example`, `.env` gitignored, `forRootAsync`, prefix `api` | 10 |
| 1 In memory | health/info, categories with service + `ParseIntPipe` + 404 | 10 |
| 2 Schema & DTOs | every rule in the schema; DTOs match; `bookedSeats` not in DTOs; seed inserted | 15 |
| 3 CRUD | 5 endpoints, correct status codes, `ParseObjectIdPipe`, 409 on duplicate, `runValidators` | 20 |
| 4 Queries | filters combine, search ignores case, pagination object, `upcoming` before `:id` | 15 |
| 5 Business rules | book/cancel rules in the right order with the right codes, `save()`, safe delete | 15 |
| 6 Lifecycle | middleware, guard on admin routes only (and closed without a key), interceptor, one error shape, helmet + CORS | 15 |
| ⭐ Bonus | stats, seats, request counter | +10 |

**Code quality (deduct up to −10):** model queries inside the controller · plain `Error` for expected problems (→ 500) · secrets in the code · no commits per milestone.

---

## Instructor Notes

- **Most common problems during the project** (walk around and check for these):
  - MongoDB not running, or `localhost` instead of `127.0.0.1` in the URI
  - Schema not registered with `forFeature` → "can't resolve dependencies … WorkshopModel"
  - `upcoming` written after `:id` → 400 Invalid id
  - Missing `await` before `findById` → the 404 never happens
  - Missing `save()` after changing `bookedSeats`
  - Dates inserted in mongosh as text instead of `new Date(...)` → `upcoming` and the "already happened" rule don't work
  - The `x-api-key` header typed in the body instead of the **Headers** tab in Postman
  - Dates sent as `"05/12/2026"` → 400 from `@IsDateString()`. Use ISO: `"2026-12-05T17:00:00Z"`
- **Stuck students:** show them the solution for **one** file only (from this document), then let them continue alone.
- **Demo (last 10 min):** ask 2–3 students to run tests 13, 17, 19 and 23 live and explain each status code.
