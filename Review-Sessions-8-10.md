# Review Session: Sessions 8–10 — Integrated Project

**Duration:** 3 hours
**Level:** Beginner–Intermediate
**Prerequisites:** Session 8 (NestJS, controllers, services, modules), Session 9 (request lifecycle, pipes, guards, interceptors, filters, MongoDB shell), Session 10 (Mongoose, config, CRUD)

---

> **How to use this session**
> - Today there are no new topics. You build **one project** that uses everything from Sessions 8–10.
> - You get the **requirements only**, no code. You write everything yourself, using your notes from Sessions 8–10.
> - The project has **7 milestones**. Each one has a time box, requirements, and a **✅ Done when** checklist to check your work.
> - The full solution is in a separate file for the instructor: **Review-Sessions-8-10-Solutions.md**.

---

## Project: Workshop Hub API

> 💡 **In simple words:** You will build the backend for a website where people find and book seats in training workshops (like "Intro to HTML" or "MongoDB Lab"). Visitors can list, search and book workshops. Only an admin (with an API key) can create, edit and delete them.
>
> **You get no code today.** Every milestone tells you **what** to build and how to check it. **How** to build it is your job, using your notes and Sessions 8–10.

### Rules

- Commit after **every** milestone (for example `milestone 3: CRUD`). If something breaks, you can go back.
- The controller only maps URLs to the service. All model calls live in the service.
- Expected problems are thrown as NestJS exceptions (400/404/409…), never as a plain `Error`.
- No secrets in the code: everything secret goes in `.env`.

### What you will build

| Method | Route | Admin key? | Description | Data |
|--------|-------|-----------|-------------|------|
| GET | `/api/health` | | `status: 'ok'` and the uptime | in memory |
| GET | `/api/info` | | name, version, current time | in memory |
| GET | `/api/categories` | | the 5 categories | in memory |
| GET | `/api/categories/:id` | | one category | in memory |
| GET | `/api/workshops` | | list with filter, search, pagination | MongoDB |
| GET | `/api/workshops/upcoming` | | published workshops in the future | MongoDB |
| GET | `/api/workshops/:id` | | one workshop | MongoDB |
| POST | `/api/workshops` | 🔑 | create | MongoDB |
| PATCH | `/api/workshops/:id` | 🔑 | update | MongoDB |
| DELETE | `/api/workshops/:id` | 🔑 | delete | MongoDB |
| PATCH | `/api/workshops/:id/book` | | book seats | MongoDB |
| PATCH | `/api/workshops/:id/cancel` | | cancel seats | MongoDB |

🔑 = the request needs the header `x-api-key` with the value of `ADMIN_API_KEY` from `.env`.

### Suggested folder structure

| Folder | Files |
|--------|-------|
| `src/` | `main.ts`, `app.module.ts`, `app.controller.ts` |
| `src/categories/` | the data file, service, controller, module |
| `src/common/` | `pipes/`, `middleware/`, `guards/`, `interceptors/`, `filters/` |
| `src/workshops/` | `schemas/`, `dto/` (create, update, book seats), service, controller, module |

### Time plan

| Milestone | Topic | Sessions | Time |
|-----------|-------|----------|------|
| 0 | Project setup, `.env`, database connection | 8, 10 | 15 min |
| 1 | In-memory warm-up: health, info, categories | 8 | 15 min |
| 2 | Schema, DTOs, seed data with mongosh | 9, 10 | 25 min |
| 3 | Full CRUD | 10 | 35 min |
| 4 | Filter, search, pagination, upcoming | 8, 10 | 20 min |
| 5 | Business rules: book, cancel, safe delete | 8, 10 | 20 min |
| 6 | Lifecycle: middleware, guard, interceptor, filter | 9 | 30 min |
| 7 | Final test in Postman + mongosh check | 9 | 20 min |
| ⭐ | Bonus | 9, 10 | if time is left |

---

### Milestone 0: Setup (15 min)

**Requirements:**

1. Create a new NestJS project named `workshop-hub-api`.
2. Install the packages for: MongoDB with Mongoose, config (`.env`), validation (two packages), and helmet.
3. Create a `.env` file with these variables, and a `.env.example` with the **names only**. Make sure `.env` is in `.gitignore`.

| Variable | Value |
|----------|-------|
| `MONGODB_URI` | your Atlas string, or local `mongodb://127.0.0.1:27017/workshop-hub` |
| `PORT` | `3000` |
| `ADMIN_API_KEY` | any secret you choose, for example `admin-secret-123` |

4. In `main.ts`: global prefix `api`, port from the environment (3000 as fallback), and a global `ValidationPipe` that removes unknown fields, **rejects** them with 400, and converts types.
5. In `app.module.ts`: the config module (available everywhere) and the Mongoose connection that reads `MONGODB_URI` **after** `.env` is loaded.
6. Delete `app.service.ts`, we don't need it.

**✅ Done when:** `npm run start:dev` starts with no errors and shows that the Mongoose module is initialized.

---

### Milestone 1: In-Memory Warm-Up (15 min) — Session 8

**Requirements:**

| Route | Returns | Errors |
|-------|---------|--------|
| `GET /api/health` | `status: 'ok'` and the server uptime in seconds | |
| `GET /api/info` | `name: 'Workshop Hub API'`, `version: '1.0.0'`, the current time as an ISO date | |
| `GET /api/categories` | the 5 categories below | |
| `GET /api/categories/:id` | one category | `abc` → 400 · `99` → 404 |

The categories (keep them in one data file inside `src/categories/`):

| id | slug | name |
|----|------|------|
| 1 | `web` | Web Development |
| 2 | `mobile` | Mobile Apps |
| 3 | `design` | UI/UX Design |
| 4 | `data` | Data & Databases |
| 5 | `career` | Career Skills |

- Build a `CategoriesModule` with a **service** (the logic) and a **controller** (HTTP only). Use the Nest CLI to generate them.
- In the same data file, also export a list of the **slugs only** (`web`, `mobile`, …). You will reuse it in Milestone 2 for the schema and the DTO, so the allowed categories are written in **one place only**.

**✅ Done when:** all 4 routes work in the browser, and the controller has no search logic inside it.

---

### Milestone 2: Schema, DTOs & Seed Data (25 min) — Sessions 9, 10

**2a. The `Workshop` schema** (with automatic `createdAt` / `updatedAt`):

| Field | Type | Rules |
|-------|------|-------|
| `code` | String | required, **unique**, saved in uppercase, trimmed, 3–20 letters, digits or dashes |
| `title` | String | required, trimmed, at least 3 characters |
| `description` | String | optional |
| `category` | String | required, must be one of the category slugs |
| `city` | String | required, trimmed |
| `date` | Date | required |
| `price` | Number | default `0`, cannot be negative |
| `capacity` | Number | required, at least `1` |
| `bookedSeats` | Number | default `0`, cannot be negative |
| `isPublished` | Boolean | default `true` |
| `tags` | list of strings | |

Register the schema in a `WorkshopsModule`.

**2b. The DTOs:**

| DTO | Fields |
|-----|--------|
| Create workshop | every field from the table **except `bookedSeats`**, with the same rules |
| Update workshop | the same fields as create, but **all optional** |
| Book seats | `seats`: a whole number from 1 to 10 |

> **New decorators you will need** (from `class-validator`, they work like the ones you know): `Matches` (the text must match a regex), `IsDateString` (an ISO date like `2026-12-05T17:00:00Z`), `Max` (the opposite of `Min`). For a list of strings, `IsString` accepts the option `each: true`.

**🤔 Think:** why is `bookedSeats` **not** in the create and update DTOs? What will happen if a client sends it?

**2c. Seed data.** Use **mongosh** to insert these 8 workshops into the `workshops` collection of your database, with one command. Remember: mongosh skips Mongoose, so write **every** field (including `bookedSeats` and `isPublished`), and save dates as real dates, not as text.

| code | title | category | city | date (UTC) | price | capacity | bookedSeats | isPublished | tags |
|------|-------|----------|------|-----------|-------|----------|-------------|-------------|------|
| WEB-101 | Intro to HTML & CSS | web | Amman | 2026-11-20 17:00 | 0 | 30 | 12 | true | html, css, beginner |
| WEB-201 | NestJS REST APIs | web | Amman | 2026-12-05 17:00 | 15 | 25 | 25 | true | nestjs, api |
| MOB-101 | Flutter Basics | mobile | Irbid | 2026-11-28 16:00 | 10 | 20 | 5 | true | flutter, dart |
| DES-101 | Figma for Beginners | design | Zarqa | 2026-10-20 15:00 | 0 | 40 | 38 | true | figma, ui |
| DATA-101 | MongoDB Queries Lab | data | Amman | 2026-12-12 17:00 | 0 | 15 | 3 | true | mongodb, database |
| CAR-101 | Write a Developer CV | career | Irbid | 2026-12-01 15:00 | 5 | 50 | 0 | **false** | cv, jobs |
| DATA-201 | Python Data Analysis | data | Zarqa | 2027-01-10 16:00 | 20 | 20 | 7 | true | python, pandas |
| DES-201 | Design Systems in Practice | design | Amman | 2027-01-18 16:00 | 25 | 18 | 18 | true | figma, design-system |

You will use these "special" workshops to test the rules: **WEB-201** and **DES-201** are full, **DES-101** is in the past, **CAR-101** is not published.

**✅ Done when:** the app starts with the schema registered, and counting the documents in mongosh gives 8.

---

### Milestone 3: Full CRUD (35 min) — Session 10

**Requirements:**

| Route | Success | Errors |
|-------|---------|--------|
| `GET /api/workshops` | 200, all workshops, soonest date first | |
| `GET /api/workshops/:id` | 200 | 400 invalid id · 404 not found |
| `POST /api/workshops` | **201** | 400 invalid body · **409** `code` already exists |
| `PATCH /api/workshops/:id` | 200, the **updated** document | 400 · 404 · 409 |
| `DELETE /api/workshops/:id` | **204** | 400 · 404 |

- Put the `ParseObjectIdPipe` from Session 10 in `src/common/pipes/`, and use it on **every** `:id`.
- Updates must return the **new** document and run the schema validation.
- A duplicate `code` must give **409** with a clear message, not 500. (Hint: MongoDB's duplicate error has the code `11000`.)
- **Rule:** if an update sets `capacity` lower than `bookedSeats` → **400** with the message `Capacity cannot be less than booked seats (12)` (the real number).

**✅ Done when:** you can create, read, update and delete a workshop in Postman with the correct status codes, and a duplicate `code` returns 409.

---

### Milestone 4: Filter, Search, Pagination & Upcoming (20 min) — Sessions 8, 10

**Requirements:**

`GET /api/workshops` accepts these query parameters. All of them are optional, and they can be combined:

| Param | Meaning | Example |
|-------|---------|---------|
| `city` | exact city | `?city=Amman` |
| `category` | exact category | `?category=design` |
| `search` | the title contains this text, ignoring upper/lower case | `?search=figma` |
| `page` | page number, default 1 | `?page=2` |
| `limit` | items per page, default 10, maximum 50 | `?limit=3` |

The response is an object with 4 fields: `data` (the workshops of this page), `total` (how many match the filter), `page`, and `totalPages`.

`GET /api/workshops/upcoming` returns only workshops that are **published** and whose date is **in the future**, soonest first.

> Hints: default values for query numbers (Session 9) · a regex filter with the "ignore case" option · run the find and the count at the same time (Session 10) · think about **where** the `upcoming` route must be in the controller.

**✅ Done when:** `?city=Amman` → 4 workshops · `?search=figma` → 1 · `?page=2&limit=3` → 3 items and `totalPages: 3` · `/upcoming` doesn't include DES-101 (past) or CAR-101 (not published).

---

### Milestone 5: Business Rules — Book, Cancel, Safe Delete (20 min) — Sessions 8, 10

**Book:** `PATCH /api/workshops/:id/book` with a body that has `seats` (uses the book-seats DTO). Check the rules **in this order**:

| # | Rule | Status | Message |
|---|------|--------|---------|
| 1 | The workshop exists | 404 | `Workshop not found` |
| 2 | It is published | 400 | `Workshop is not open for booking` |
| 3 | Its date is in the future | 400 | `Workshop has already happened` |
| 4 | `seats` is not more than the seats left (capacity − bookedSeats) | 409 | `Only 5 seats left` (the real number) |

If everything is OK: add the seats to `bookedSeats`, **save** it in the database, and return the workshop.

**Cancel:** `PATCH /api/workshops/:id/cancel` with `seats`. If `seats` is more than `bookedSeats` → **409** `Cannot cancel more seats than booked`. Otherwise subtract and save.

**Safe delete:** change DELETE so that a workshop with `bookedSeats` more than 0 cannot be deleted → **409** `Cannot delete a workshop with bookings`.

**🤔 Think:** why is "not enough seats" a **409** and not a **400**?

**✅ Done when:** booking 3 seats on WEB-101 works (12 → 15) · WEB-201 (full) gives 409 · DES-101 (past) gives 400 · CAR-101 (unpublished) gives 400 · `seats: 0` gives 400 from the DTO · deleting WEB-101 gives 409.

---

### Milestone 6: The Request Lifecycle (30 min) — Session 9

Make the API safe and consistent.

**Requirements:**

| # | Tool | What it must do | Where |
|---|------|-----------------|-------|
| 1 | Logger **middleware** (functional) | print the method and URL of every request | global |
| 2 | `ApiKeyGuard` | read `ADMIN_API_KEY` with `ConfigService`, compare it with the `x-api-key` header, otherwise **401** `Invalid API key` | only on POST, PATCH `:id` and DELETE |
| 3 | `LoggingInterceptor` | print method, URL, status code and time in ms, like `GET /api/workshops 200 - 12ms` | global |
| 4 | `AllExceptionsFilter` | **every** error has the same shape: `statusCode`, `message`, `path`, `timestamp` | global, registered with `APP_FILTER` |
| 5 | `helmet` + CORS | security headers + allow only `http://localhost:5173` | `main.ts` |

⚠️ **Security check:** the guard must stay **closed** if `ADMIN_API_KEY` is missing from `.env`. (Think: what is "no header" compared with "no config value"?)

Book and cancel stay **public** (no key).

**✅ Done when:** POST without the header → 401 · with the header → 201 · book without the header → still works · `/api/nothing` and an invalid id return the same error shape · every request shows a log line in the terminal.

---

### Milestone 7: Final Test + mongosh Check (20 min)

**7a. Postman.** Go through the test checklist below. Every line must give the expected status. Save all the requests in a Postman collection named `Workshop Hub`.

Get the real ids from `GET /api/workshops`. 🔑 = add the header `x-api-key`.

| # | Request | Expected |
|---|---------|----------|
| 1 | `GET /api/health` | 200 |
| 2 | `GET /api/categories/3` · `/99` · `/abc` | 200 · 404 · 400 |
| 3 | `GET /api/workshops` | 200, `total: 8`, sorted by date |
| 4 | `GET /api/workshops?city=Amman` | 200, 4 items |
| 5 | `GET /api/workshops?category=design&search=figma` | 200, 1 item (DES-101) |
| 6 | `GET /api/workshops?page=2&limit=3` | 200, 3 items, `page: 2`, `totalPages: 3` |
| 7 | `GET /api/workshops/upcoming` | 200, no DES-101, no CAR-101 |
| 8 | `GET /api/workshops/123` | 400 `Invalid id` |
| 9 | `GET /api/workshops/665f1c2e8a1b2c3d4e5f6a7b` | 404 `Workshop not found` |
| 10 | `POST /api/workshops` without the key | 401 |
| 11 | `POST /api/workshops` 🔑 with an empty body | 400 with a list of messages |
| 12 | `POST /api/workshops` 🔑 with `bookedSeats: 5` in the body | 400 `property bookedSeats should not exist` |
| 13 | `POST /api/workshops` 🔑 with the valid workshop below | **201** |
| 14 | The same request again | **409** code already exists |
| 15 | `PATCH` WEB-101 🔑 with `price: -5` | 400 |
| 16 | `PATCH` WEB-101 🔑 with `capacity: 2` | 400 capacity < booked seats |
| 17 | `PATCH` WEB-101 `/book` with `seats: 3` (no key) | 200, `bookedSeats: 15` |
| 18 | `PATCH` WEB-101 `/book` with `seats: 0` · `seats: 11` | 400 · 400 |
| 19 | `PATCH` WEB-201 `/book` with `seats: 1` | 409 `Only 0 seats left` |
| 20 | `PATCH` DES-101 `/book` with `seats: 1` | 400 already happened |
| 21 | `PATCH` CAR-101 `/book` with `seats: 1` | 400 not open for booking |
| 22 | `PATCH` MOB-101 `/cancel` with `seats: 6` · `seats: 2` | 409 · 200 with `bookedSeats: 3` |
| 23 | `DELETE` WEB-101 🔑 | 409 has bookings |
| 24 | `DELETE` the new workshop 🔑, then again | 204 → 404 |
| 25 | `GET /api/nothing` | 404 |
| 26 | Every error above | same shape: `statusCode`, `message`, `path`, `timestamp` |

The valid workshop for test 13:

| Field | Value |
|-------|-------|
| code | `web-301` (lowercase on purpose) |
| title | React Hooks Deep Dive |
| category | web |
| city | Amman |
| date | `2027-02-01T17:00:00Z` |
| price | 10 |
| capacity | 20 |
| tags | react, hooks |

Check the response of test 13: is `code` saved as `WEB-301`? Are `bookedSeats` 0 and `isPublished` true? Do `createdAt` and `updatedAt` exist? Explain **why** each one happened.

**7b. mongosh.** Write these queries yourself and check the results:

1. The code and title of the workshops in Irbid, without `_id`
2. Workshops that are free **and** published
3. Workshops with the tag `figma`
4. The 3 most expensive workshops (title and price only)
5. Unpublish every workshop whose date has passed (one command)
6. Count the workshops in the `web` **or** `data` category

---

### ⭐ Bonus (if you finish early)

| # | Route / feature | Requirement |
|---|-----------------|-------------|
| 1 | `GET /api/workshops/stats` | returns `total`, `upcoming`, `free` (price 0), and `byCategory` (how many workshops in each category). Expected with the seed data: 8, 6, 3, web 2 / mobile 1 / design 2 / data 2 / career 1 |
| 2 | `GET /api/workshops/:id/seats` | returns `code`, `capacity`, `bookedSeats`, `seatsLeft`, `isFull` |
| 3 | Request counter | a global interceptor (registered with `APP_INTERCEPTOR`) counts every request, and `/api/info` also returns `totalRequests` |

---

**Review session complete.** Next: Session 11 — Validation, Error Handling & Authentication. Keep your Workshop Hub API: we will add login with JWT to it.
