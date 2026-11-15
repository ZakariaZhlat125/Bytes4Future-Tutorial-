# Review Session (Advanced): Mini Store API — Solutions (Instructor Only)

> **Do not share this file with students.**
> It contains: the answers to the 🤔 Think questions, the full project code, a mongosh seed script, the grading rubric, and instructor notes.
>
> Student file: **Review-Sessions-8-10-Advanced.md**

---

## Answers to the 🤔 Think Questions

1. **Request id.** The user sends the id from the response (or the error), and you search the server logs for it. You find the exact request, its time, status and the real error, among thousands of lines.
2. **401, not 400.** Guards run **before** pipes. The `RolesGuard` stops the request before the `ValidationPipe` ever looks at the body.
3. **Soft delete.** Old orders point to products. If the product is really deleted, the order history breaks, and reports lose data. A soft delete hides it from customers but keeps the history.
4. **Read-check-save is not enough.** Between "read" and "save", another request can read the same stock. Both see "1 left", both save, and the store sells 2. A single conditional update (`stock >= qty` in the filter + `$inc`) is checked and applied by MongoDB in one step.
5. **Copy name and price.** Prices and names change later. An order must show what the customer actually bought, at the price they actually paid.
6. **Ignore client prices.** The client can send anything, for example a price of 0.01. Only the server knows the real price. (`forbidNonWhitelisted` already rejects an extra `price` field.)
7. **404, not 403.** 403 says "this order exists, but you can't see it", so anyone could test random ids and learn which orders exist. 404 reveals nothing.

---

## Project Solution

### Install

```bash
nest new mini-store-api
cd mini-store-api
npm i @nestjs/mongoose mongoose @nestjs/config class-validator class-transformer helmet
```

```bash
# .env.example
MONGODB_URI=
PORT=
ADMIN_API_KEY=
STAFF_API_KEY=
```

### Folder structure

```
src/
├── main.ts
├── app.module.ts
├── config/env.validation.ts
├── common/
│   ├── types.ts
│   ├── money.ts
│   ├── regex.ts
│   ├── middleware/request-id.middleware.ts
│   ├── auth/roles.decorator.ts
│   ├── auth/roles.guard.ts
│   ├── interceptors/response.interceptor.ts
│   ├── filters/all-exceptions.filter.ts
│   └── pipes/parse-object-id.pipe.ts
├── catalog/
│   ├── catalog.module.ts
│   ├── categories/{category.schema.ts, category.dto.ts, categories.service.ts, categories.controller.ts}
│   └── products/{product.schema.ts, product.dto.ts, products.service.ts, products.controller.ts}
└── orders/
    ├── order.schema.ts
    ├── order.dto.ts
    ├── orders.service.ts
    ├── orders.controller.ts
    └── orders.module.ts
```

---

### Milestone 0: Setup + Environment Validation

```ts
// src/config/env.validation.ts
const REQUIRED = ['MONGODB_URI', 'ADMIN_API_KEY', 'STAFF_API_KEY'];

export function validateEnv(env: Record<string, unknown>) {
  const missing = REQUIRED.filter((key) => !env[key]);
  if (missing.length) {
    throw new Error(`Missing environment variables: ${missing.join(', ')}`);
  }
  return env;
}
```

```ts
// src/main.ts
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import helmet from 'helmet';
import { AppModule } from './app.module';
import { requestId } from './common/middleware/request-id.middleware';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.setGlobalPrefix('api');
  app.use(helmet());
  app.enableCors({ origin: 'http://localhost:5173' });
  app.use(requestId);
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }));
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

```ts
// src/app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { APP_FILTER, APP_GUARD, APP_INTERCEPTOR } from '@nestjs/core';
import { MongooseModule } from '@nestjs/mongoose';
import { CatalogModule } from './catalog/catalog.module';
import { RolesGuard } from './common/auth/roles.guard';
import { AllExceptionsFilter } from './common/filters/all-exceptions.filter';
import { ResponseInterceptor } from './common/interceptors/response.interceptor';
import { validateEnv } from './config/env.validation';
import { OrdersModule } from './orders/orders.module';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true, validate: validateEnv }),
    MongooseModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({ uri: config.get<string>('MONGODB_URI') }),
    }),
    CatalogModule,
    OrdersModule,
  ],
  providers: [
    { provide: APP_GUARD, useClass: RolesGuard },
    { provide: APP_INTERCEPTOR, useClass: ResponseInterceptor },
    { provide: APP_FILTER, useClass: AllExceptionsFilter },
  ],
})
export class AppModule {}
```

---

### Milestone 1: The Common Layer

```ts
// src/common/types.ts
import { IncomingMessage } from 'http';

export type Role = 'admin' | 'staff';
export type AppRequest = IncomingMessage & { requestId?: string; role?: Role | null };
```

```ts
// src/common/money.ts
export const round2 = (n: number) => Math.round(n * 100) / 100;

// src/common/regex.ts — so "(" or "*" are searched as text, not as regex syntax
export const escapeRegex = (text: string) => text.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
```

```ts
// src/common/middleware/request-id.middleware.ts
import { randomUUID } from 'crypto';
import { ServerResponse } from 'http';
import { AppRequest } from '../types';

export function requestId(req: AppRequest, res: ServerResponse, next: () => void) {
  const id = randomUUID();
  req.requestId = id;
  res.setHeader('X-Request-Id', id);

  const start = Date.now();
  res.on('finish', () => {
    console.log(`[${id.slice(0, 8)}] ${req.method} ${req.url} ${res.statusCode} - ${Date.now() - start}ms`);
  });
  next();
}
```

```ts
// src/common/interceptors/response.interceptor.ts
import { CallHandler, ExecutionContext, Injectable, NestInterceptor } from '@nestjs/common';
import { Observable, map } from 'rxjs';

@Injectable()
export class ResponseInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<unknown> {
    return next.handle().pipe(
      map((result) => {
        const isPage = result !== null && typeof result === 'object' && 'data' in result && 'meta' in result;
        return isPage ? { success: true, ...result } : { success: true, data: result };
      }),
    );
  }
}
```

```ts
// src/common/filters/all-exceptions.filter.ts
import { ArgumentsHost, Catch, ExceptionFilter, HttpException, HttpStatus, Logger } from '@nestjs/common';
import { HttpAdapterHost } from '@nestjs/core';
import { Error as MongooseError } from 'mongoose';
import { AppRequest } from '../types';

function isDuplicateKey(e: unknown): e is { code: number; keyValue?: Record<string, unknown> } {
  return typeof e === 'object' && e !== null && (e as { code?: unknown }).code === 11000;
}

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger('Errors');

  constructor(private readonly adapterHost: HttpAdapterHost) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const { httpAdapter } = this.adapterHost;
    const ctx = host.switchToHttp();
    const req = ctx.getRequest<AppRequest>();

    let status: number = HttpStatus.INTERNAL_SERVER_ERROR;
    let message: unknown = 'Internal server error';

    if (exception instanceof HttpException) {
      status = exception.getStatus();
      const body = exception.getResponse();
      message = typeof body === 'string' ? body : (body as { message?: unknown }).message ?? exception.message;
    } else if (exception instanceof MongooseError.ValidationError) {
      status = HttpStatus.BAD_REQUEST;
      message = Object.values(exception.errors).map((e) => e.message);
    } else if (exception instanceof MongooseError.CastError) {
      status = HttpStatus.BAD_REQUEST;
      message = `Invalid ${exception.path}`;
    } else if (isDuplicateKey(exception)) {
      status = HttpStatus.CONFLICT;
      const field = Object.keys(exception.keyValue ?? {})[0] ?? 'value';
      message = `${field} already exists`;
    }

    if (status === 500) this.logger.error(exception);

    httpAdapter.reply(ctx.getResponse(), {
      success: false,
      statusCode: status,
      message,
      path: httpAdapter.getRequestUrl(req),
      requestId: req.requestId,
      timestamp: new Date().toISOString(),
    }, status);
  }
}
```

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

---

### Milestone 2: Roles

```ts
// src/common/auth/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';
import { Role } from '../types';

export const ROLES_KEY = 'roles';
export const Roles = (...roles: Role[]) => SetMetadata(ROLES_KEY, roles);
```

```ts
// src/common/auth/roles.guard.ts
import { CanActivate, ExecutionContext, ForbiddenException, Injectable, UnauthorizedException } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { Reflector } from '@nestjs/core';
import { AppRequest, Role } from '../types';
import { ROLES_KEY } from './roles.decorator';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(
    private readonly reflector: Reflector,
    private readonly config: ConfigService,
  ) {}

  canActivate(context: ExecutionContext): boolean {
    const req = context.switchToHttp().getRequest<AppRequest>();
    req.role = this.roleFor(req.headers['x-api-key']);       // saved for every request

    const required = this.reflector.getAllAndOverride<Role[]>(ROLES_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);
    if (!required) return true;                               // public route

    if (!req.role) throw new UnauthorizedException('Valid API key required');
    if (!required.includes(req.role)) throw new ForbiddenException('Your role cannot do this');
    return true;
  }

  private roleFor(key: unknown): Role | null {
    if (typeof key !== 'string' || !key) return null;
    if (key === this.config.get<string>('ADMIN_API_KEY')) return 'admin';
    if (key === this.config.get<string>('STAFF_API_KEY')) return 'staff';
    return null;
  }
}
```

---

### Milestone 3: Categories

```ts
// src/catalog/categories/category.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument } from 'mongoose';

@Schema({ timestamps: true })
export class Category {
  @Prop({ required: true, trim: true, minlength: 2, maxlength: 40 })
  name: string;

  @Prop({ required: true, unique: true, lowercase: true })
  slug: string;
}

export type CategoryDocument = HydratedDocument<Category>;
export const CategorySchema = SchemaFactory.createForClass(Category);

export const slugify = (text: string) =>
  text.trim().toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-+|-+$/g, '');
```

```ts
// src/catalog/categories/category.dto.ts — used for POST and PATCH (only one field)
import { IsString, Length } from 'class-validator';

export class CategoryDto {
  @IsString()
  @Length(2, 40)
  name: string;
}
```

```ts
// src/catalog/categories/categories.service.ts
import { BadRequestException, ConflictException, Injectable, NotFoundException } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Product } from '../products/product.schema';
import { CategoryDto } from './category.dto';
import { Category, slugify } from './category.schema';

@Injectable()
export class CategoriesService {
  constructor(
    @InjectModel(Category.name) private readonly categoryModel: Model<Category>,
    @InjectModel(Product.name) private readonly productModel: Model<Product>,
  ) {}

  async findAll() {
    const categories = await this.categoryModel.find().sort({ name: 1 }).lean();
    const counts = await Promise.all(
      categories.map((c) => this.productModel.countDocuments({ category: c._id, isActive: true })),
    );
    return categories.map((c, i) => ({ ...c, productsCount: counts[i] }));
  }

  async findOne(id: string) {
    const category = await this.categoryModel.findById(id);
    if (!category) throw new NotFoundException('Category not found');
    return category;
  }

  create(dto: CategoryDto) {
    return this.categoryModel.create({ name: dto.name, slug: this.slugOrFail(dto.name) });
    // duplicate slug → 11000 → 409 "slug already exists" from the global filter
  }

  async update(id: string, dto: CategoryDto) {
    const category = await this.categoryModel.findByIdAndUpdate(
      id,
      { name: dto.name, slug: this.slugOrFail(dto.name) },
      { new: true, runValidators: true },
    );
    if (!category) throw new NotFoundException('Category not found');
    return category;
  }

  async remove(id: string) {
    const category = await this.findOne(id);
    const used = await this.productModel.countDocuments({ category: id });   // active or not
    if (used > 0) throw new ConflictException(`Category has ${used} products`);
    await category.deleteOne();
  }

  private slugOrFail(name: string) {
    const slug = slugify(name);
    if (!slug) throw new BadRequestException('name must contain letters or digits');
    return slug;
  }
}
```

```ts
// src/catalog/categories/categories.controller.ts
import { Body, Controller, Delete, Get, HttpCode, Param, Patch, Post } from '@nestjs/common';
import { Roles } from '../../common/auth/roles.decorator';
import { ParseObjectIdPipe } from '../../common/pipes/parse-object-id.pipe';
import { CategoriesService } from './categories.service';
import { CategoryDto } from './category.dto';

@Controller('categories')
export class CategoriesController {
  constructor(private readonly categoriesService: CategoriesService) {}

  @Get()
  findAll() {
    return this.categoriesService.findAll();
  }

  @Post()
  @Roles('admin')
  create(@Body() dto: CategoryDto) {
    return this.categoriesService.create(dto);
  }

  @Patch(':id')
  @Roles('admin')
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: CategoryDto) {
    return this.categoriesService.update(id, dto);
  }

  @Delete(':id')
  @Roles('admin')
  @HttpCode(204)
  remove(@Param('id', ParseObjectIdPipe) id: string) {
    return this.categoriesService.remove(id);
  }
}
```

```ts
// src/catalog/catalog.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { CategoriesController } from './categories/categories.controller';
import { CategoriesService } from './categories/categories.service';
import { Category, CategorySchema } from './categories/category.schema';
import { Product, ProductSchema } from './products/product.schema';
import { ProductsController } from './products/products.controller';
import { ProductsService } from './products/products.service';

@Module({
  imports: [
    MongooseModule.forFeature([
      { name: Category.name, schema: CategorySchema },
      { name: Product.name, schema: ProductSchema },
    ]),
  ],
  controllers: [CategoriesController, ProductsController],
  providers: [CategoriesService, ProductsService],
  exports: [ProductsService],                          // OrdersModule needs it
})
export class CatalogModule {}
```

---

### Milestones 4–5: Products

```ts
// src/catalog/products/product.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import mongoose, { HydratedDocument, Types } from 'mongoose';

@Schema({ timestamps: true })
export class Product {
  @Prop({ required: true, unique: true, uppercase: true, trim: true, match: /^[A-Z]{3}-\d{4}$/ })
  sku: string;

  @Prop({ required: true, trim: true, minlength: 2, maxlength: 80 })
  name: string;

  @Prop({ trim: true, maxlength: 500 })
  description?: string;

  @Prop({ required: true, min: 0.01 })
  price: number;

  @Prop({ required: true, min: 0 })
  stock: number;

  @Prop({ type: mongoose.Schema.Types.ObjectId, ref: 'Category', required: true })
  category: Types.ObjectId;

  @Prop({ default: true })
  isActive: boolean;

  @Prop([String])
  tags: string[];
}

export type ProductDocument = HydratedDocument<Product>;
export const ProductSchema = SchemaFactory.createForClass(Product);
```

```ts
// src/catalog/products/product.dto.ts
import { Type } from 'class-transformer';
import {
  ArrayMaxSize, IsArray, IsBoolean, IsIn, IsInt, IsMongoId, IsNumber, IsOptional,
  IsString, Length, Matches, Max, MaxLength, Min, NotEquals,
} from 'class-validator';

const SKU = /^[A-Za-z]{3}-\d{4}$/;

export class CreateProductDto {
  @Matches(SKU, { message: 'sku must look like KEY-1001' }) sku: string;
  @IsString() @Length(2, 80) name: string;
  @IsOptional() @IsString() @MaxLength(500) description?: string;
  @IsNumber({ maxDecimalPlaces: 2 }) @Min(0.01) price: number;
  @IsInt() @Min(0) stock: number;
  @IsMongoId() category: string;
  @IsOptional() @IsBoolean() isActive?: boolean;
  @IsOptional() @IsArray() @ArrayMaxSize(10) @IsString({ each: true }) tags?: string[];
}

// no `stock` here → sending it gives 400 (forbidNonWhitelisted)
export class UpdateProductDto {
  @IsOptional() @Matches(SKU, { message: 'sku must look like KEY-1001' }) sku?: string;
  @IsOptional() @IsString() @Length(2, 80) name?: string;
  @IsOptional() @IsString() @MaxLength(500) description?: string;
  @IsOptional() @IsNumber({ maxDecimalPlaces: 2 }) @Min(0.01) price?: number;
  @IsOptional() @IsMongoId() category?: string;
  @IsOptional() @IsBoolean() isActive?: boolean;
  @IsOptional() @IsArray() @ArrayMaxSize(10) @IsString({ each: true }) tags?: string[];
}

export class AdjustStockDto {
  @IsInt() @Min(-1000) @Max(1000) @NotEquals(0) change: number;
}

export const PRODUCT_SORTS = ['price', '-price', 'name', '-name', 'createdAt', '-createdAt'];

export class ProductQueryDto {
  @IsOptional() @IsString() q?: string;
  @IsOptional() @IsMongoId() category?: string;
  @IsOptional() @Type(() => Number) @IsNumber() @Min(0) minPrice?: number;
  @IsOptional() @Type(() => Number) @IsNumber() @Min(0) maxPrice?: number;
  @IsOptional() @IsIn(['true', 'false']) inStock?: 'true' | 'false';
  @IsOptional() @IsIn(PRODUCT_SORTS) sort: string = 'name';
  @IsOptional() @Type(() => Number) @IsInt() @Min(1) page: number = 1;
  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @Max(50) limit: number = 10;
}
```

```ts
// src/catalog/products/products.service.ts
import { BadRequestException, ConflictException, Injectable, NotFoundException } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { escapeRegex } from '../../common/regex';
import { Category } from '../categories/category.schema';
import { CreateProductDto, ProductQueryDto, UpdateProductDto } from './product.dto';
import { Product } from './product.schema';

@Injectable()
export class ProductsService {
  constructor(
    @InjectModel(Product.name) private readonly productModel: Model<Product>,
    @InjectModel(Category.name) private readonly categoryModel: Model<Category>,
  ) {}

  // ---------- Milestone 5: list ----------
  async findAll(query: ProductQueryDto) {
    if (query.minPrice !== undefined && query.maxPrice !== undefined && query.minPrice > query.maxPrice) {
      throw new BadRequestException('minPrice cannot be greater than maxPrice');
    }

    const filter: Record<string, unknown> = { isActive: true };
    if (query.q) filter.name = { $regex: escapeRegex(query.q), $options: 'i' };
    if (query.category) filter.category = query.category;
    if (query.minPrice !== undefined || query.maxPrice !== undefined) {
      const price: Record<string, number> = {};
      if (query.minPrice !== undefined) price.$gte = query.minPrice;
      if (query.maxPrice !== undefined) price.$lte = query.maxPrice;
      filter.price = price;
    }
    if (query.inStock === 'true') filter.stock = { $gt: 0 };
    if (query.inStock === 'false') filter.stock = 0;

    const { page, limit } = query;
    const [items, total] = await Promise.all([
      this.productModel.find(filter).sort(query.sort).skip((page - 1) * limit).limit(limit).lean(),
      this.productModel.countDocuments(filter),
    ]);

    const totalPages = Math.ceil(total / limit);
    return {
      data: await this.withCategoryNames(items),
      meta: { page, limit, total, totalPages, hasNext: page < totalPages },
    };
  }

  // one extra query for the whole page ($in), instead of populate or one query per product
  private async withCategoryNames<T extends { category: unknown }>(items: T[]) {
    const ids = [...new Set(items.map((p) => String(p.category)))];
    const categories = await this.categoryModel.find({ _id: { $in: ids } }, { name: 1 }).lean();
    const names = new Map(categories.map((c) => [String(c._id), c.name]));
    return items.map((p) => ({
      ...p,
      category: { id: String(p.category), name: names.get(String(p.category)) ?? null },
    }));
  }

  async findOnePublic(id: string) {
    const product = await this.productModel.findOne({ _id: id, isActive: true }).lean();
    if (!product) throw new NotFoundException('Product not found');
    const [withCategory] = await this.withCategoryNames([product]);
    return withCategory;
  }

  lowStock(threshold: number) {
    return this.productModel
      .find({ isActive: true, stock: { $lte: threshold } }, { sku: 1, name: 1, stock: 1 })
      .sort({ stock: 1, name: 1 });
  }

  // ---------- Milestone 4: write ----------
  async create(dto: CreateProductDto) {
    await this.ensureCategory(dto.category);
    return this.productModel.create(dto);                 // duplicate sku → 409 from the filter
  }

  async update(id: string, dto: UpdateProductDto) {
    if (dto.category) await this.ensureCategory(dto.category);
    const product = await this.productModel.findByIdAndUpdate(id, dto, { new: true, runValidators: true });
    if (!product) throw new NotFoundException('Product not found');
    return product;
  }

  // check + update in ONE atomic operation
  async adjustStock(id: string, change: number) {
    const filter = change < 0 ? { _id: id, stock: { $gte: -change } } : { _id: id };
    const product = await this.productModel.findOneAndUpdate(filter, { $inc: { stock: change } }, { new: true });
    if (!product) {
      const exists = await this.productModel.exists({ _id: id });
      if (!exists) throw new NotFoundException('Product not found');
      throw new ConflictException('Not enough stock');
    }
    return product;
  }

  async softDelete(id: string) {
    const product = await this.productModel.findByIdAndUpdate(id, { isActive: false });
    if (!product) throw new NotFoundException('Product not found');
  }

  private async ensureCategory(id: string) {
    const exists = await this.categoryModel.exists({ _id: id });
    if (!exists) throw new BadRequestException('Category does not exist');
  }

  // ---------- used by OrdersModule ----------
  findActiveByIds(ids: string[]) {
    return this.productModel.find({ _id: { $in: ids }, isActive: true });
  }

  async reserve(productId: string, quantity: number): Promise<boolean> {
    const result = await this.productModel.updateOne(
      { _id: productId, isActive: true, stock: { $gte: quantity } },
      { $inc: { stock: -quantity } },
    );
    return result.modifiedCount === 1;
  }

  async release(productId: string, quantity: number) {
    await this.productModel.updateOne({ _id: productId }, { $inc: { stock: quantity } });
  }
}
```

```ts
// src/catalog/products/products.controller.ts
import {
  Body, Controller, DefaultValuePipe, Delete, Get, HttpCode,
  Param, ParseIntPipe, Patch, Post, Query,
} from '@nestjs/common';
import { Roles } from '../../common/auth/roles.decorator';
import { ParseObjectIdPipe } from '../../common/pipes/parse-object-id.pipe';
import { AdjustStockDto, CreateProductDto, ProductQueryDto, UpdateProductDto } from './product.dto';
import { ProductsService } from './products.service';

@Controller('products')
export class ProductsController {
  constructor(private readonly productsService: ProductsService) {}

  @Get()
  findAll(@Query() query: ProductQueryDto) {
    return this.productsService.findAll(query);
  }

  @Get('low-stock')                                         // BEFORE ':id'
  @Roles('admin', 'staff')
  lowStock(@Query('threshold', new DefaultValuePipe(5), ParseIntPipe) threshold: number) {
    return this.productsService.lowStock(threshold);
  }

  @Get(':id')
  findOne(@Param('id', ParseObjectIdPipe) id: string) {
    return this.productsService.findOnePublic(id);
  }

  @Post()
  @Roles('admin')
  create(@Body() dto: CreateProductDto) {
    return this.productsService.create(dto);
  }

  @Patch(':id')
  @Roles('admin')
  update(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: UpdateProductDto) {
    return this.productsService.update(id, dto);
  }

  @Patch(':id/stock')
  @Roles('admin')
  adjustStock(@Param('id', ParseObjectIdPipe) id: string, @Body() dto: AdjustStockDto) {
    return this.productsService.adjustStock(id, dto.change);
  }

  @Delete(':id')
  @Roles('admin')
  @HttpCode(204)
  remove(@Param('id', ParseObjectIdPipe) id: string) {
    return this.productsService.softDelete(id);
  }
}
```

> `low-stock` has a `threshold` query parameter read with a pipe, not with `ProductQueryDto`, so `forbidNonWhitelisted` doesn't apply to it.

---

### Milestones 6–7: Orders

```ts
// src/orders/order.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import mongoose, { HydratedDocument, Types } from 'mongoose';

export const ORDER_STATUSES = ['pending', 'paid', 'shipped', 'delivered', 'cancelled'] as const;
export type OrderStatus = (typeof ORDER_STATUSES)[number];

@Schema({ _id: false })
export class Customer {
  @Prop({ required: true, trim: true }) name: string;
  @Prop({ required: true, trim: true, lowercase: true }) email: string;
  @Prop() phone?: string;
}
const CustomerSchema = SchemaFactory.createForClass(Customer);

@Schema({ _id: false })
export class OrderItem {
  @Prop({ type: mongoose.Schema.Types.ObjectId, ref: 'Product', required: true }) product: Types.ObjectId;
  @Prop({ required: true }) sku: string;
  @Prop({ required: true }) name: string;
  @Prop({ required: true }) unitPrice: number;
  @Prop({ required: true, min: 1 }) quantity: number;
  @Prop({ required: true }) lineTotal: number;
}
const OrderItemSchema = SchemaFactory.createForClass(OrderItem);

@Schema({ _id: false })
export class StatusChange {
  @Prop({ type: String, required: true }) status: OrderStatus;
  @Prop({ required: true }) by: string;
  @Prop({ default: Date.now }) at: Date;
}
const StatusChangeSchema = SchemaFactory.createForClass(StatusChange);

@Schema({ timestamps: true })
export class Order {
  @Prop({ required: true, unique: true }) orderNumber: string;
  @Prop({ type: CustomerSchema, required: true }) customer: Customer;
  @Prop({ type: [OrderItemSchema], required: true }) items: OrderItem[];
  @Prop({ required: true }) subtotal: number;
  @Prop({ required: true }) shippingFee: number;
  @Prop({ required: true }) total: number;
  @Prop({ type: String, enum: [...ORDER_STATUSES], default: 'pending' }) status: OrderStatus;
  @Prop({ type: [StatusChangeSchema], default: [] }) statusHistory: StatusChange[];
}

export type OrderDocument = HydratedDocument<Order>;
export const OrderSchema = SchemaFactory.createForClass(Order);
```

```ts
// src/orders/order.dto.ts
import { Type } from 'class-transformer';
import {
  ArrayMaxSize, ArrayMinSize, IsArray, IsDateString, IsDefined, IsEmail, IsIn, IsInt,
  IsMongoId, IsOptional, IsString, Length, Matches, Max, Min, ValidateNested,
} from 'class-validator';
import { ORDER_STATUSES, OrderStatus } from './order.schema';

class CustomerDto {
  @IsString() @Length(2, 60) name: string;
  @IsEmail() email: string;
  @IsOptional() @Matches(/^\+?\d{7,15}$/, { message: 'phone must be 7-15 digits' }) phone?: string;
}

export class OrderItemDto {
  @IsMongoId() productId: string;
  @IsInt() @Min(1) @Max(10) quantity: number;
}

export class CreateOrderDto {
  @IsDefined()
  @ValidateNested()
  @Type(() => CustomerDto)
  customer: CustomerDto;

  @IsArray()
  @ArrayMinSize(1)
  @ArrayMaxSize(10)
  @ValidateNested({ each: true })
  @Type(() => OrderItemDto)
  items: OrderItemDto[];
}

export class UpdateStatusDto {
  @IsIn([...ORDER_STATUSES]) status: OrderStatus;
}

export class OrderQueryDto {
  @IsOptional() @IsIn([...ORDER_STATUSES]) status?: OrderStatus;
  @IsOptional() @IsEmail() email?: string;
  @IsOptional() @IsDateString() from?: string;
  @IsOptional() @IsDateString() to?: string;
  @IsOptional() @Type(() => Number) @IsInt() @Min(1) page: number = 1;
  @IsOptional() @Type(() => Number) @IsInt() @Min(1) @Max(50) limit: number = 10;
}
```

```ts
// src/orders/orders.service.ts
import { BadRequestException, ConflictException, ForbiddenException, Injectable, NotFoundException } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { ProductsService } from '../catalog/products/products.service';
import { round2 } from '../common/money';
import { Role } from '../common/types';
import { CreateOrderDto, OrderItemDto, OrderQueryDto } from './order.dto';
import { Order, OrderStatus } from './order.schema';

const TRANSITIONS: Record<OrderStatus, OrderStatus[]> = {
  pending: ['paid', 'cancelled'],
  paid: ['shipped', 'cancelled'],
  shipped: ['delivered'],
  delivered: [],
  cancelled: [],
};

function makeOrderNumber() {
  const date = new Date().toISOString().slice(0, 10).replace(/-/g, '');
  const random = Math.random().toString(36).slice(2, 6).toUpperCase().padEnd(4, '0');
  return `ORD-${date}-${random}`;
}

@Injectable()
export class OrdersService {
  constructor(
    @InjectModel(Order.name) private readonly orderModel: Model<Order>,
    private readonly productsService: ProductsService,
  ) {}

  // ---------- Milestone 6 ----------
  async create(dto: CreateOrderDto) {
    const ids = dto.items.map((i) => i.productId);

    // 1. no duplicates
    if (new Set(ids).size !== ids.length) {
      throw new BadRequestException('Each product can appear only once');
    }

    // 2. all exist and are active
    const products = await this.productsService.findActiveByIds(ids);
    const byId = new Map(products.map((p) => [String(p._id), p]));
    const missing = ids.filter((id) => !byId.has(id));
    if (missing.length) throw new BadRequestException(`Products not available: ${missing.join(', ')}`);

    // 3. enough stock (friendly message listing ALL short items)
    const short = dto.items.filter((i) => byId.get(i.productId)!.stock < i.quantity);
    if (short.length) {
      throw new ConflictException(`Not enough stock for: ${short.map((i) => byId.get(i.productId)!.sku).join(', ')}`);
    }

    // 4. take the stock atomically; roll back what we took if one item fails
    const reserved: OrderItemDto[] = [];
    const rollback = () => Promise.all(reserved.map((r) => this.productsService.release(r.productId, r.quantity)));

    for (const item of dto.items) {
      const ok = await this.productsService.reserve(item.productId, item.quantity);
      if (!ok) {
        await rollback();
        throw new ConflictException(`Not enough stock for: ${byId.get(item.productId)!.sku}`);
      }
      reserved.push(item);
    }

    // 5. prices from the database
    const items = dto.items.map((i) => {
      const p = byId.get(i.productId)!;
      return {
        product: p._id,
        sku: p.sku,
        name: p.name,
        unitPrice: p.price,
        quantity: i.quantity,
        lineTotal: round2(p.price * i.quantity),
      };
    });
    const subtotal = round2(items.reduce((sum, i) => sum + i.lineTotal, 0));
    const shippingFee = subtotal >= 50 ? 0 : 5;

    // 6. save (if saving fails, give the stock back)
    try {
      return await this.orderModel.create({
        orderNumber: makeOrderNumber(),
        customer: dto.customer,
        items,
        subtotal,
        shippingFee,
        total: round2(subtotal + shippingFee),
        statusHistory: [{ status: 'pending', by: 'customer', at: new Date() }],
      });
    } catch (err) {
      await rollback();
      throw err;
    }
  }

  // ---------- Milestone 7 ----------
  async findAll(query: OrderQueryDto) {
    const filter: Record<string, unknown> = {};
    if (query.status) filter.status = query.status;
    if (query.email) filter['customer.email'] = query.email.trim().toLowerCase();
    if (query.from || query.to) {
      const createdAt: Record<string, Date> = {};
      if (query.from) createdAt.$gte = new Date(query.from);
      if (query.to) createdAt.$lte = new Date(query.to);
      filter.createdAt = createdAt;
    }

    const { page, limit } = query;
    const [data, total] = await Promise.all([
      this.orderModel.find(filter).sort({ createdAt: -1 }).skip((page - 1) * limit).limit(limit),
      this.orderModel.countDocuments(filter),
    ]);
    const totalPages = Math.ceil(total / limit);
    return { data, meta: { page, limit, total, totalPages, hasNext: page < totalPages } };
  }

  async findForViewer(id: string, role: Role | null, email?: string) {
    const order = await this.orderModel.findById(id);
    const isOwner = !!email && order?.customer.email === email.trim().toLowerCase();
    if (!order || (!role && !isOwner)) throw new NotFoundException('Order not found');
    return order;
  }

  async updateStatus(id: string, to: OrderStatus, role: Role) {
    const order = await this.orderModel.findById(id);
    if (!order) throw new NotFoundException('Order not found');

    const from = order.status;
    if (!TRANSITIONS[from].includes(to)) {
      throw new ConflictException(`Cannot change status from ${from} to ${to}`);
    }
    if (from === 'paid' && to === 'cancelled' && role !== 'admin') {
      throw new ForbiddenException('Only an admin can cancel a paid order');
    }

    // only succeeds if nobody changed the status since we read it
    const updated = await this.orderModel.findOneAndUpdate(
      { _id: id, status: from },
      { $set: { status: to }, $push: { statusHistory: { status: to, by: role, at: new Date() } } },
      { new: true },
    );
    if (!updated) throw new ConflictException('Order was changed by someone else, try again');

    if (to === 'cancelled') {
      await Promise.all(order.items.map((i) => this.productsService.release(String(i.product), i.quantity)));
    }
    return updated;
  }
}
```

```ts
// src/orders/orders.controller.ts
import { Body, Controller, Get, Param, Patch, Post, Query, Req } from '@nestjs/common';
import { Roles } from '../common/auth/roles.decorator';
import { ParseObjectIdPipe } from '../common/pipes/parse-object-id.pipe';
import { AppRequest } from '../common/types';
import { CreateOrderDto, OrderQueryDto, UpdateStatusDto } from './order.dto';
import { OrdersService } from './orders.service';

@Controller('orders')
export class OrdersController {
  constructor(private readonly ordersService: OrdersService) {}

  @Post()
  create(@Body() dto: CreateOrderDto) {
    return this.ordersService.create(dto);
  }

  @Get()
  @Roles('admin', 'staff')
  findAll(@Query() query: OrderQueryDto) {
    return this.ordersService.findAll(query);
  }

  @Get(':id')                                    // public, but the guard already set req.role
  findOne(
    @Param('id', ParseObjectIdPipe) id: string,
    @Req() req: AppRequest,
    @Query('email') email?: string,
  ) {
    return this.ordersService.findForViewer(id, req.role ?? null, email);
  }

  @Patch(':id/status')
  @Roles('admin', 'staff')
  updateStatus(
    @Param('id', ParseObjectIdPipe) id: string,
    @Body() dto: UpdateStatusDto,
    @Req() req: AppRequest,
  ) {
    return this.ordersService.updateStatus(id, dto.status, req.role!);
  }
}
```

```ts
// src/orders/orders.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { CatalogModule } from '../catalog/catalog.module';
import { Order, OrderSchema } from './order.schema';
import { OrdersController } from './orders.controller';
import { OrdersService } from './orders.service';

@Module({
  imports: [
    CatalogModule,                                              // gives ProductsService
    MongooseModule.forFeature([{ name: Order.name, schema: OrderSchema }]),
  ],
  controllers: [OrdersController],
  providers: [OrdersService],
})
export class OrdersModule {}
```

---

### Seed Script (mongosh)

```js
use('mini-store');

const cat = db.categories.insertMany([
  { name: 'Keyboards',   slug: 'keyboards',   createdAt: new Date(), updatedAt: new Date() },
  { name: 'Mice',        slug: 'mice',        createdAt: new Date(), updatedAt: new Date() },
  { name: 'Monitors',    slug: 'monitors',    createdAt: new Date(), updatedAt: new Date() },
  { name: 'Accessories', slug: 'accessories', createdAt: new Date(), updatedAt: new Date() },
]).insertedIds;

const p = (sku, name, category, price, stock, isActive = true) =>
  ({ sku, name, category, price, stock, isActive, tags: [], createdAt: new Date(), updatedAt: new Date() });

db.products.insertMany([
  p('KEY-1001', 'Mechanical Keyboard K1', cat[0], 59.99, 12),
  p('KEY-1002', 'Wireless Keyboard Slim', cat[0], 34.50, 3),
  p('KEY-1003', 'Gaming Keyboard RGB',    cat[0], 89.00, 0),
  p('MOU-2001', 'Wireless Mouse M2',      cat[1], 19.99, 40),
  p('MOU-2002', 'Gaming Mouse Pro',       cat[1], 49.00, 5),
  p('MON-3001', 'FHD Monitor 24',         cat[2], 129.00, 7),
  p('MON-3002', '4K Monitor 27',          cat[2], 299.00, 2),
  p('ACC-4001', 'USB-C Hub 7-in-1',       cat[3], 25.00, 25),
  p('ACC-4002', 'VGA Cable',              cat[3], 4.99, 50, false),
]);

db.products.countDocuments({ isActive: true });   // 8
```

---

### Bonus 1: Reports

```ts
// add to OrdersService (also import ORDER_STATUSES from './order.schema').
// No new injection needed: low stock comes from ProductsService.
async summary() {
  const [counts, delivered, active, lowStock] = await Promise.all([
    Promise.all(ORDER_STATUSES.map((s) => this.orderModel.countDocuments({ status: s }))),
    this.orderModel.find({ status: 'delivered' }, { total: 1 }).lean(),
    this.orderModel.find({ status: { $ne: 'cancelled' } }, { items: 1 }).lean(),
    this.productsService.lowStock(5),
  ]);

  const sold = new Map<string, number>();
  for (const order of active) {
    for (const item of order.items) sold.set(item.sku, (sold.get(item.sku) ?? 0) + item.quantity);
  }
  const topProducts = [...sold.entries()]
    .sort((a, b) => b[1] - a[1])
    .slice(0, 3)
    .map(([sku, quantity]) => ({ sku, quantity }));

  return {
    ordersByStatus: Object.fromEntries(ORDER_STATUSES.map((s, i) => [s, counts[i]])),
    revenue: round2(delivered.reduce((sum, o) => sum + o.total, 0)),
    lowStockCount: lowStock.length,
    topProducts,
  };
}
```

```ts
// src/orders/reports.controller.ts → add to OrdersModule.controllers
@Controller('reports')
@Roles('admin')
export class ReportsController {
  constructor(private readonly ordersService: OrdersService) {}

  @Get('summary')
  summary() {
    return this.ordersService.summary();
  }
}
```

### Bonus 2: Rate Limit Middleware

```ts
// src/orders/order-rate-limit.middleware.ts
import { HttpException, HttpStatus, Injectable, NestMiddleware } from '@nestjs/common';
import { IncomingMessage, ServerResponse } from 'http';

@Injectable()
export class OrderRateLimitMiddleware implements NestMiddleware {
  private readonly hits = new Map<string, number[]>();

  use(req: IncomingMessage, res: ServerResponse, next: () => void) {
    if (req.method !== 'POST') return next();

    const ip = req.socket.remoteAddress ?? 'unknown';
    const now = Date.now();
    const recent = (this.hits.get(ip) ?? []).filter((t) => now - t < 60_000);

    if (recent.length >= 5) {
      throw new HttpException('Too many orders, try again in a minute', HttpStatus.TOO_MANY_REQUESTS);
    }
    recent.push(now);
    this.hits.set(ip, recent);
    next();
  }
}

// orders.module.ts
export class OrdersModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(OrderRateLimitMiddleware).forRoutes(OrdersController);
  }
}
```

> Behind a proxy (Nginx, Cloudflare) every request has the proxy's IP. Real apps read the client IP from a trusted header. This in-memory version also resets when the server restarts and doesn't work with several servers: in Session 12 you'll see `@nestjs/throttler`.

---

## Grading Rubric (100 points + 10 bonus)

| Milestone | What we check | Points |
|-----------|---------------|--------|
| 0 Setup | env validation names every missing variable; `.env` gitignored; helmet, CORS, strict `ValidationPipe` | 5 |
| 1 Common layer | request id in header + log + error body; one success envelope (with `meta` at the top for lists); filter maps 11000 / ValidationError / CastError; no try/catch for duplicates in services | 15 |
| 2 Roles | `@Roles` on route and controller level; global guard; 401 vs 403 correct; role saved on the request | 10 |
| 3 Categories | slug made by the server; 409 duplicate from the filter; `productsCount`; delete blocked with the real count | 10 |
| 4 Products | schema rules; `stock` not updatable through PATCH; atomic `/stock`; soft delete; `low-stock` before `:id` | 15 |
| 5 Query | every filter + validation; regex escaped; sort whitelist; `meta` with `hasNext`; category names with one `$in` query | 10 |
| 6 Orders | checks in the right order; prices from the DB; rounding; shipping rule; atomic reserve **with rollback**; `ProductsService` exported, no product model in orders | 20 |
| 7 Workflow | transition table; 403 for staff cancelling paid; status in the update filter; history; stock back on cancel; tracking 404 | 15 |
| ⭐ Bonus | reports, rate limit | +10 |

**Deduct up to −10:** model queries in controllers · circular module imports with `forwardRef` instead of a clean design · plain `Error` for expected problems · secrets in the code.

---

## Instructor Notes

- **Level check:** this project is for students who finished the first review project (Workshop Hub) without needing the solution. Students who struggled with Workshop Hub should finish it first.
- **Expect most students to reach Milestone 6.** Milestone 7 and the bonus separate the strongest students. Milestones 0–2 are the foundation: don't let anyone skip them to "get to the features".
- **Where students get stuck** (walk around and check):
  - The response envelope wraps paginated lists twice (`data.data`) → check for `data` + `meta`
  - Circular import between a categories module and a products module → the `CatalogModule` hint
  - `@ValidateNested` without `@Type` → nested fields are never validated, and extra fields inside items are not rejected
  - Query DTO numbers stay strings → missing `@Type(() => Number)`
  - `?q=(` → 500 "Invalid regular expression" → the text must be escaped
  - Money like `64.97999999999999` → round with `Math.round(n * 100) / 100`
  - "Check then save" for stock → show two Postman tabs ordering the last item at the same time, then explain the conditional update
  - `req.role` is `undefined` in the orders controller → the guard must set it **before** the "no `@Roles`" early return
- **Live race demo (5 min, recommended):** set a product's stock to 1, then send two orders for it at the same time (Postman Runner or two terminals with `curl`). With "check then save" both succeed; with the conditional update only one does.
- **Demo (last 10 min):** ask students to show Milestone 6 test 4 (the rollback) and Milestone 7 (the workflow) live, and to explain Think 2 and Think 4.
