# Review Session (Advanced): Sessions 8–10 — Mini Store API

**Duration:** 3 hours
**Level:** Intermediate–Advanced
**Prerequisites:** Sessions 8–10, and the first review project (Workshop Hub API)

---

> **How to use this session**
> - This is the **harder** version of the review project. Same rules: you get the **requirements only**, no code. You write everything yourself.
> - It uses everything from Sessions 8–10, **plus a few small things you must look up yourself** (marked 🔍). Reading the docs is part of the exercise.
> - The project has **8 milestones**. Each one has a time box, requirements, and a **✅ Done when** checklist.
> - Some milestones end with **🤔 Think** questions. Write your answers in a `NOTES.md` file in the project. The instructor will ask about them.
> - The full solution is in a separate file for the instructor.

---

## The Project: Mini Store API

> 💡 **In simple words:** You build the backend of a small online store that sells computer accessories. Visitors browse products and place orders **without an account**. **Staff** handle the orders (paid → shipped → delivered). An **admin** manages the catalog. The API must never sell more items than are in stock, even when two people order the last item at the same moment.

### What makes this project harder than Workshop Hub

| Workshop Hub | Mini Store |
|--------------|------------|
| One API key, one role | Two keys → two roles (`admin`, `staff`) with a custom `@Roles()` decorator |
| One main collection | Three collections that refer to each other (categories → products → orders) |
| Errors handled in each service | Database errors (duplicate, validation) handled **once**, in the global filter |
| Simple body | Nested body (an object + an array of objects) validated deeply |
| Check, then update | Check **and** update in **one** atomic database operation (no overselling) |
| One status field | A **status workflow** with allowed transitions and a history |
| Plain responses | Every response has the same envelope, with a request id |

### Rules

- Commit after **every** milestone.
- Controllers only map URLs to services. All model calls live in services.
- Expected problems are NestJS exceptions (400/401/403/404/409), never a plain `Error`.
- Money is always rounded to **2 decimals**.
- The client **never** decides prices, totals or stock. The server computes them.

### Roles

| Role | How the request proves it | Can do |
|------|---------------------------|--------|
| visitor | no `x-api-key` header | browse categories and products, place an order, track **their own** order |
| staff | `x-api-key` = `STAFF_API_KEY` | everything a visitor can + list orders, change order status, see low-stock products |
| admin | `x-api-key` = `ADMIN_API_KEY` | everything + manage categories and products, cancel paid orders, reports |

### All endpoints

| Method | Route | Role | Description |
|--------|-------|------|-------------|
| GET | `/api/categories` | public | all categories, with how many active products each has |
| POST | `/api/categories` | admin | create |
| PATCH | `/api/categories/:id` | admin | rename |
| DELETE | `/api/categories/:id` | admin | delete (409 if any product uses it) |
| GET | `/api/products` | public | active products: search, filters, sort, pagination |
| GET | `/api/products/low-stock` | staff, admin | active products with low stock |
| GET | `/api/products/:id` | public | one active product |
| POST | `/api/products` | admin | create |
| PATCH | `/api/products/:id` | admin | update (but **not** the stock) |
| PATCH | `/api/products/:id/stock` | admin | add or remove stock |
| DELETE | `/api/products/:id` | admin | **soft** delete |
| POST | `/api/orders` | public | place an order |
| GET | `/api/orders` | staff, admin | list orders with filters |
| GET | `/api/orders/:id` | public* | one order (*a visitor must give the right email) |
| PATCH | `/api/orders/:id/status` | staff, admin | change the status |

### Suggested folder structure

| Folder | What goes inside |
|--------|------------------|
| `src/config/` | environment validation |
| `src/common/` | middleware, the roles decorator + guard, the response interceptor, the exceptions filter, pipes, small helpers |
| `src/catalog/` | **one** module with both categories and products (see the hint in Milestone 3) |
| `src/orders/` | the orders module |

### Time plan

| Milestone | Topic | Time |
|-----------|-------|------|
| 0 | Setup + environment validation | 10 min |
| 1 | Common layer: request id, logging, response envelope, error filter | 25 min |
| 2 | Roles: `@Roles()` decorator + global guard | 20 min |
| 3 | Categories | 15 min |
| 4 | Products: schema, CRUD, stock, soft delete, seed | 25 min |
| 5 | Products: search, filters, sort, pagination | 20 min |
| 6 | Orders: place an order without overselling | 35 min |
| 7 | Orders: list, track, status workflow | 20 min |
| 8 | Final test | 10 min |
| ⭐ | Bonus | if time is left |

---

### Milestone 0: Setup + Environment Validation (10 min)

**Requirements:**

1. New NestJS project `mini-store-api`. Install: Mongoose, config, the two validation packages, helmet.
2. `.env` (gitignored) and `.env.example` (names only):

| Variable | Example value | Required? |
|----------|---------------|-----------|
| `MONGODB_URI` | `mongodb://127.0.0.1:27017/mini-store` | yes |
| `PORT` | `3000` | no (fallback 3000) |
| `ADMIN_API_KEY` | `admin-key-123` | yes |
| `STAFF_API_KEY` | `staff-key-456` | yes |

3. **The app must refuse to start** if a required variable is missing, with a clear message that names **every** missing variable (for example `Missing environment variables: STAFF_API_KEY`).
   🔍 Look up the `validate` option of `ConfigModule.forRoot`.
4. `main.ts`: prefix `api`, helmet, CORS for `http://localhost:5173`, and a global `ValidationPipe` that removes unknown fields, **rejects** them with 400, and converts types.

**✅ Done when:** the app starts normally · if you comment out `STAFF_API_KEY` in `.env`, it stops with the message above.

---

### Milestone 1: The Common Layer (25 min) — Session 9

Every request and every response in this API looks the same. Build this **before** any feature.

**1a. Request id + logging middleware** (global)
- Give every request a unique id (🔍 `randomUUID` from Node's `crypto` module).
- Save it on the request object, and send it back in the response header `X-Request-Id`.
- When the response is finished, print one line: the first 8 characters of the id, method, URL, status code and time in ms.

**1b. Response envelope** (global interceptor, registered with `APP_INTERCEPTOR`)

| The handler returns | The client receives |
|---------------------|---------------------|
| a document or an array | `success: true` and `data` (the value) |
| an object that has **both** `data` and `meta` (paginated lists) | `success: true`, `data` and `meta` at the top level (not nested twice) |

**1c. Error envelope** (global filter, registered with `APP_FILTER`)

Every error has the shape: `success: false`, `statusCode`, `message`, `path`, `requestId`, `timestamp`. The filter also turns database errors into clean responses, so **services never need try/catch for them**:

| Error | Status | Message |
|-------|--------|---------|
| any NestJS `HttpException` | its own status | its own message |
| MongoDB duplicate key (code `11000`) | **409** | `<field> already exists` (the real field name, for example `sku already exists`) |
| Mongoose `ValidationError` | **400** | the list of validation messages |
| Mongoose `CastError` | **400** | `Invalid <field>` |
| anything else | **500** | `Internal server error` (log the real error on the server only) |

**1d.** `ParseObjectIdPipe` from Session 10, in `src/common/pipes/`.

**✅ Done when:** `GET /api/nothing` returns a 404 in the error envelope, its `requestId` equals the `X-Request-Id` header, and the terminal shows one log line for it.

**🤔 Think 1:** why is the request id useful when a user reports "I got an error"?

---

### Milestone 2: Roles (20 min) — Session 9

**Requirements:**

1. A `@Roles(...)` decorator that you can put on a route or on a controller, for example "only admin" or "admin and staff". 🔍 `SetMetadata` (Session 9, coding task 3).
2. A `RolesGuard`, registered **globally** with `APP_GUARD`. For **every** request it:
   - reads `x-api-key` and works out the role (`admin`, `staff`, or none) by comparing with the keys from `ConfigService`,
   - **saves the role on the request** (other parts of the app need it later),
   - reads the roles required by the route (🔍 `Reflector`, `getAllAndOverride`).

| Situation | Result |
|-----------|--------|
| The route has no `@Roles` | allowed for everyone |
| The route has `@Roles`, no key or a wrong key | **401** `Valid API key required` |
| The route has `@Roles`, the key is valid but the role is not in the list | **403** `Your role cannot do this` |
| The role is in the list | allowed |

**✅ Done when:** you can test it on a temporary route with `@Roles('admin')`: no key → 401, wrong key → 401, staff key → 403, admin key → 200. Then delete the temporary route.

**🤔 Think 2:** `POST /api/products` with **no key** and an **empty body**: will the client get 401 or 400? Why? (Use the lifecycle order.)

---

### Milestone 3: Categories (15 min) — Session 10

**Schema `Category`** (with timestamps):

| Field | Rules |
|-------|-------|
| `name` | required, trimmed, 2–40 characters |
| `slug` | required, **unique**, lowercase. **Made by the server** from the name: lowercase, every group of characters that are not `a-z`/`0-9` becomes one `-`, no `-` at the start or end. Example: `"  Gaming & Mice "` → `gaming-mice` |

**Endpoints:**

| Route | Rules |
|-------|-------|
| `GET /api/categories` | sorted by name; each category also has `productsCount` = number of **active** products in it |
| `POST /api/categories` | body: `name` only. If the slug comes out empty (for example the name is `!!!`) → 400. A duplicate slug → 409 (from the filter, not from the service) |
| `PATCH /api/categories/:id` | rename (the slug changes too); 404 if missing |
| `DELETE /api/categories/:id` | 204. **409** `Category has 2 products` (the real number) if **any** product (active or not) uses it |

> **Design hint:** categories need to count products, and products need to check that a category exists. If you make two modules that import each other, you get a circular dependency. Put categories and products in **one** `CatalogModule` (two controllers, two services, both schemas), and later **export** only what the orders module needs.

**✅ Done when:** you can create, rename and delete a category; creating `Keyboards` twice (or ` keyboards `) gives 409 `slug already exists`.

---

### Milestone 4: Products — Schema, CRUD, Stock, Soft Delete (25 min) — Session 10

**Schema `Product`** (with timestamps):

| Field | Rules |
|-------|-------|
| `sku` | required, **unique**, uppercase, format: 3 letters, a dash, 4 digits (`KEY-1001`) |
| `name` | required, trimmed, 2–80 characters |
| `description` | optional, max 500 characters |
| `price` | required, at least 0.01, at most 2 decimals |
| `stock` | required, whole number, at least 0 |
| `category` | required, the **ObjectId** of a category |
| `isActive` | default `true` |
| `tags` | list of strings, max 10 |

**Endpoints:**

| Route | Rules |
|-------|-------|
| `GET /api/products/:id` | only **active** products; inactive or missing → 404. The `category` field must be returned as an object with `id` and `name` (see Milestone 5) |
| `POST /api/products` | the category must exist → otherwise **400** `Category does not exist`. Duplicate sku → 409 (filter) |
| `PATCH /api/products/:id` | every field optional **except that `stock` is not allowed at all** (sending it → 400). Same category check |
| `PATCH /api/products/:id/stock` | body: `change`, a whole number from −1000 to 1000, **not 0**. Adds `change` to the stock. If the stock would go below 0 → **409** `Not enough stock`. 🔍 This must be **one** database operation: look up `$inc` and how a filter condition in `findOneAndUpdate` can make the update happen only when there is enough stock |
| `DELETE /api/products/:id` | **soft delete**: sets `isActive: false`, returns 204. The product disappears from the public routes but stays in the database |
| `GET /api/products/low-stock` | staff/admin. Query `threshold` (default 5). Active products with stock ≤ threshold, lowest stock first, only `sku`, `name`, `stock` |

**Seed data.** Create these categories and products, either through your API (with the admin key) or with mongosh. If you use mongosh, write the slugs yourself, and use the real `_id` of each category in the products.

| Category | Slug |
|----------|------|
| Keyboards | keyboards |
| Mice | mice |
| Monitors | monitors |
| Accessories | accessories |

| sku | name | category | price | stock | isActive |
|-----|------|----------|-------|-------|----------|
| KEY-1001 | Mechanical Keyboard K1 | Keyboards | 59.99 | 12 | true |
| KEY-1002 | Wireless Keyboard Slim | Keyboards | 34.50 | 3 | true |
| KEY-1003 | Gaming Keyboard RGB | Keyboards | 89.00 | 0 | true |
| MOU-2001 | Wireless Mouse M2 | Mice | 19.99 | 40 | true |
| MOU-2002 | Gaming Mouse Pro | Mice | 49.00 | 5 | true |
| MON-3001 | FHD Monitor 24 | Monitors | 129.00 | 7 | true |
| MON-3002 | 4K Monitor 27 | Monitors | 299.00 | 2 | true |
| ACC-4001 | USB-C Hub 7-in-1 | Accessories | 25.00 | 25 | true |
| ACC-4002 | VGA Cable | Accessories | 4.99 | 50 | **false** |

**✅ Done when:** all routes above work; `GET /api/categories` shows `productsCount` 3 / 2 / 2 / 1 (Keyboards / Mice / Monitors / Accessories); low-stock with threshold 5 returns KEY-1003, MON-3002, KEY-1002, MOU-2002 in this order.

**🤔 Think 3:** why do we soft-delete products instead of really deleting them? (Think about Milestone 6.)
**🤔 Think 4:** for the stock change, why is "read the product, check the stock, then save" **not** good enough?

---

### Milestone 5: Products — Search, Filters, Sort, Pagination (20 min) — Sessions 8, 10

`GET /api/products` returns **active** products only. All query parameters are optional and can be combined. **Any unknown parameter → 400.**

| Param | Rule | Invalid → 400 when |
|-------|------|--------------------|
| `q` | name contains the text, ignore case. Special characters like `(` or `*` are searched **as text**, they must not crash the server | — |
| `category` | category id | not a valid ObjectId |
| `minPrice`, `maxPrice` | price range, both included | not a number, below 0, or `minPrice` > `maxPrice` |
| `inStock` | `true` → stock > 0 · `false` → stock = 0 | anything other than `true` / `false` |
| `sort` | one of `price`, `-price`, `name`, `-name`, `createdAt`, `-createdAt` (`-` = descending). Default `name` | any other value |
| `page` | whole number ≥ 1, default 1 | not a whole number, or < 1 |
| `limit` | whole number 1–50, default 10 | outside 1–50 (**reject**, don't silently fix it) |

🔍 Instead of many pipes, you can write **one query DTO** class and use it with `@Query()`. Look up `@Type(() => Number)` from `class-transformer` (query values are always strings).

**Response:** `success`, `data` (the products), and `meta` with `page`, `limit`, `total`, `totalPages`, `hasNext`.

**Category names without populate:** every product in `data` must have `category` as an object with `id` and `name`. Do it with **one extra query** for the whole page (🔍 the `$in` operator), not one query per product.

**✅ Done when:**

| Request | Expected |
|---------|----------|
| no params | 8 products (VGA Cable is inactive), sorted by name |
| `?q=wireless` | 2 |
| `?minPrice=20&maxPrice=60` | 4 |
| `?inStock=false` | 1 (KEY-1003) |
| `?category=<Mice id>` | 2 |
| `?sort=-price&limit=3` | MON-3002, MON-3001, KEY-1003 |
| `?page=2&limit=5` | 3 products, `totalPages: 2`, `hasNext: false` |
| `?q=(` | 200 with an empty list (not 500) |
| `?minPrice=100&maxPrice=50` · `?sort=color` · `?limit=100` · `?category=123` · `?color=red` | 400 each |

---

### Milestone 6: Place an Order Without Overselling (35 min) — Sessions 8–10

**Schema `Order`** (with timestamps):

| Field | Content |
|-------|---------|
| `orderNumber` | unique, made by the server: `ORD-` + today's date `YYYYMMDD` + `-` + 4 random uppercase letters/digits (example `ORD-20261111-K7Q2`) |
| `customer` | an **embedded** object: `name`, `email` (saved lowercase), `phone` (optional) |
| `items` | a list. Each item **copies** from the product at order time: product id, `sku`, `name`, `unitPrice`, plus `quantity` and `lineTotal` |
| `subtotal`, `shippingFee`, `total` | computed by the server |
| `status` | one of `pending`, `paid`, `shipped`, `delivered`, `cancelled`. Default `pending` |
| `statusHistory` | a list of `status`, `by` (`customer` / `staff` / `admin`), `at` (date) |

**Request body** of `POST /api/orders` (public):

| Field | Rules |
|-------|-------|
| `customer.name` | text, 2–60 characters |
| `customer.email` | a valid email |
| `customer.phone` | optional, 7–15 digits, may start with `+` |
| `items` | a list with 1–10 entries |
| `items[].productId` | a valid ObjectId |
| `items[].quantity` | whole number 1–10 |

🔍 Nested validation: `@ValidateNested`, `@Type` (class-transformer), `@IsMongoId`, `@ArrayMinSize`, `@ArrayMaxSize`. Unknown fields **inside** the items (like a `price` the client tries to send) must also give 400.

**Rules, checked in this order:**

| # | Rule | Status | Message |
|---|------|--------|---------|
| 1 | The same product appears only once in `items` | 400 | `Each product can appear only once` |
| 2 | Every product exists **and** is active | 400 | `Products not available: <ids>` (all of them) |
| 3 | Every product has enough stock | 409 | `Not enough stock for: <skus>` (all of them) |
| 4 | Take the stock **atomically**, item by item. If one item fails (someone else bought it a millisecond ago), **put back** the stock you already took for the other items | 409 | `Not enough stock for: <sku>` |
| 5 | Prices come from the database. `lineTotal` = unitPrice × quantity. `shippingFee` = **0** if subtotal ≥ 50, otherwise **5**. Round money to 2 decimals | | |
| 6 | Save the order with status `pending` and one history entry (`pending`, by `customer`) | **201** | |

> **Design hint:** the orders module must not touch the product model directly. Give `ProductsService` small methods for what orders need (find active products by ids, take stock, put stock back), **export** `ProductsService` from `CatalogModule`, and import `CatalogModule` in `OrdersModule`.

**✅ Done when:**

| Order | Expected |
|-------|----------|
| MOU-2001 × 2 + ACC-4001 × 1 | 201 · subtotal 64.98 · shipping 0 · total 64.98 · stock MOU-2001 = 38, ACC-4001 = 24 |
| MOU-2002 × 1 | 201 · subtotal 49 · shipping 5 · total 54 |
| KEY-1003 × 1 | 409 `Not enough stock for: KEY-1003` |
| MOU-2001 × 1 + MON-3002 × 3 | 409 for MON-3002 · and MOU-2001 stock is **still 38** |
| ACC-4002 × 1 (inactive) | 400 not available |
| MOU-2001 twice in the list | 400 |
| quantity 11 · empty items list · no customer | 400 each |
| an item with an extra `price` field | 400 `property price should not exist` (or similar) |

**🤔 Think 5:** why do we copy `name` and `unitPrice` into the order instead of only saving the product id?
**🤔 Think 6:** why do we ignore any price the client sends?

---

### Milestone 7: List, Track, Status Workflow (20 min)

**`GET /api/orders`** (staff, admin): newest first, paginated with the same `meta` as products. Optional filters: `status` (must be a valid status), `email`, `from` / `to` (ISO dates, on `createdAt`). Invalid values → 400.

**`GET /api/orders/:id`** (public, with a twist):

| Who asks | Result |
|----------|--------|
| staff or admin | the order |
| visitor with `?email=` equal to the order's email (ignore case and spaces) | the order |
| visitor with a wrong email, or no email | **404** `Order not found` (**not** 403) |

Hint: the guard already saved the role on the request.

**`PATCH /api/orders/:id/status`** (staff, admin), body `status`. Allowed changes:

| From | Allowed to |
|------|-----------|
| `pending` | `paid`, `cancelled` |
| `paid` | `shipped`, `cancelled` |
| `shipped` | `delivered` |
| `delivered` | — |
| `cancelled` | — |

| Rule | Status | Message |
|------|--------|---------|
| Order missing | 404 | |
| Change not in the table (also the same status again) | 409 | `Cannot change status from paid to pending` (the real values) |
| `paid` → `cancelled` by **staff** | 403 | `Only an admin can cancel a paid order` |
| Two people change the same order at the same moment: only the first wins (🔍 put the **current** status in the update filter) | 409 | `Order was changed by someone else, try again` |
| Success | 200 | add a history entry with the new status and the role. On **cancel**, put the stock of every item back |

**✅ Done when:** using the orders from Milestone 6:

| Request | Expected |
|---------|----------|
| staff: order 1 `pending` → `shipped` | 409 |
| staff: order 1 → `paid` | 200, history has 2 entries |
| staff: order 1 → `cancelled` | 403 |
| admin: order 1 → `cancelled` | 200, stock back: MOU-2001 = 40, ACC-4001 = 25 |
| admin: order 1 → `paid` | 409 |
| staff: order 2 → `paid` → `shipped` → `delivered` | 200 each |
| staff: order 2 → `cancelled` | 409 |
| no key: change status | 401 |
| staff: status `lost` | 400 |
| visitor: order 2 with the right email · wrong email · no email | 200 · 404 · 404 |
| staff: `GET /api/orders?status=cancelled` | 1 order |

**🤔 Think 7:** why 404 and not 403 when a visitor gives the wrong email?

---

### Milestone 8: Final Test (10 min)

Go through these extra checks, then save everything in a Postman collection named `Mini Store`, with two environment variables for the keys.

| # | Check | Expected |
|---|-------|----------|
| 1 | `POST /api/products` with no key and an empty body | 401 (see Think 2) |
| 2 | `POST /api/products` with the staff key | 403 |
| 3 | `POST /api/products` with a duplicate sku | 409 `sku already exists` |
| 4 | `POST /api/products` with a category id that doesn't exist | 400 `Category does not exist` |
| 5 | `PATCH /api/products/:id` with `stock: 5` | 400 |
| 6 | `PATCH` KEY-1002 `/stock` with `change: -5` · `-3` · `0` · `10` | 409 · 200 (stock 0) · 400 · 200 (stock 10) |
| 7 | `DELETE` MON-3001 (admin), then `GET` it | 204 → 404; it's gone from the list; Monitors `productsCount` is now 1 |
| 8 | `DELETE /api/categories/<Mice>` | 409 `Category has 2 products` |
| 9 | Every error so far | the same envelope, with a `requestId` |
| 10 | Remove `ADMIN_API_KEY` from `.env` and restart | the app doesn't start |

---

### ⭐ Bonus (if you finish early)

| # | Feature | Requirement |
|---|---------|-------------|
| 1 | `GET /api/reports/summary` (admin) | `ordersByStatus` (a count for each of the 5 statuses), `revenue` (sum of `total` of **delivered** orders), `lowStockCount` (active products with stock ≤ 5), `topProducts` (top 3 skus by quantity in orders that are **not cancelled**, with their quantity) |
| 2 | Rate limit on placing orders | max **5** `POST /api/orders` per IP per minute; the 6th → **429** `Too many orders, try again in a minute`. Write it as **class middleware** applied to the orders controller only |

---

**Review session complete.** Next: Session 11 — Validation, Error Handling & Authentication. In Session 11 you'll replace the API keys with real users and JWT login.
