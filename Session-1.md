# Session 1: TypeScript Essentials — Types, Interfaces, Functions, and Generics

**Duration:** 3 hours
**Level:** Beginner
**Prerequisites:** JavaScript fundamentals (variables, functions, objects, arrays). No tools needed — we install everything from zero in Part 0.

---

## Session Timeline

- **Part 0: Setup from Zero (30 min)** — install Node.js & npm, create a project, install TypeScript, compile and run the first file
- **Part 1: TypeScript Basics (40 min)** — what/why TypeScript, annotations, primitives, special types, arrays & tuples
- **Part 2: Objects and Interfaces (50 min)** — object types, interfaces, optional/readonly, extending, `type` vs `interface`
- **Part 3: Functions and Advanced Types (50 min)** — function annotations, unions, literals & narrowing, generics
- **Practice (10 min)** — exercises at the end

---

## Part 0: Setup from Zero

Browsers and Node.js only understand JavaScript, so TypeScript needs a **compiler** (`tsc`) that turns `.ts` files into `.js` files. We get that compiler through **npm**, and npm comes with **Node.js**.

```
index.ts  ──tsc──▶  index.js  ──node──▶  output
```

### 0.1 Install Node.js and npm

1. Go to [nodejs.org](https://nodejs.org) and download the **LTS** version.
2. Run the installer and keep the default options (npm is installed with it).
3. **Close and reopen** the terminal, then check both tools:

```bash
node -v    # v22.x.x or newer
npm -v     # 10.x.x or newer
```

| Tool | What it is |
|------|------------|
| **Node.js** | Runs JavaScript outside the browser |
| **npm** | Node Package Manager — downloads and manages packages (libraries and tools) |
| **npx** | Runs a package's command without installing it globally |

**Common errors:** `'node' is not recognized` → the terminal was open during the install; reopen it (or restart the PC). On Windows PowerShell, `npm` blocked by "running scripts is disabled" → run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` once.

---

### 0.2 Create the Project

```bash
mkdir ts-session-1
cd ts-session-1
npm init -y        # creates package.json
code .             # open the folder in VS Code
```

`package.json` is the project's ID card: its name, its scripts, and the list of packages it depends on.

---

### 0.3 Install TypeScript

```bash
npm install --save-dev typescript    # short form: npm i -D typescript
npx tsc -v                           # confirm the compiler works
```

After this command the project has three new things:

| File / folder | Purpose | Commit to Git? |
|---------------|---------|----------------|
| `node_modules/` | The downloaded packages | No — add it to `.gitignore` |
| `package-lock.json` | Exact installed versions | Yes |
| `devDependencies` in `package.json` | Tools needed only during development | Yes |

- `--save-dev` (`-D`) → development tool (TypeScript, test runners). Plain `npm install <name>` → a library the app needs at runtime.
- Anyone who clones the project runs just `npm install` to rebuild `node_modules/` from `package.json`.
- Install TypeScript **per project**, not globally (`-g`), so every project controls its own version.

---

### 0.4 Configure the Compiler: tsconfig.json

Create a file named `tsconfig.json` in the project root:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "nodenext",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true
  },
  "include": ["src"]
}
```

| Option | Meaning |
|--------|---------|
| `target` | Which JavaScript version to output |
| `module` | Module system of the output (`nodenext` follows Node.js rules) |
| `rootDir` | Where our `.ts` source files live |
| `outDir` | Where the compiled `.js` files go |
| `strict` | Turns on all strict type checks — always keep it `true` |

`npx tsc --init` generates a full file with every option explained in comments; the short version above is all we need for now.

---

### 0.5 First File: Compile and Run

Create `src/index.ts`:

```ts
const course: string = "TypeScript";
console.log(`Hello, ${course}!`);
```

```bash
npx tsc               # compiles src/ → dist/
node dist/index.js    # Hello, TypeScript!
```

Now break it on purpose — change the value to `123` and compile again:

```
src/index.ts:1:7 - error TS2322: Type 'number' is not assignable to type 'string'.
```

The error appears **before** the code runs. That is the whole point of TypeScript.

Add scripts to `package.json` so nobody has to remember the commands:

```json
"scripts": {
  "build": "tsc",
  "start": "node dist/index.js",
  "dev": "tsc --watch"
}
```

```bash
npm run build    # compile once
npm start        # run the compiled code
npm run dev      # recompile automatically on every save
```

Final project structure:

```
ts-session-1/
├── node_modules/
├── dist/              ← compiled JavaScript (generated)
│   └── index.js
├── src/               ← our TypeScript code
│   └── index.ts
├── .gitignore         ← node_modules and dist
├── package.json
├── package-lock.json
└── tsconfig.json
```

**Notes:** VS Code has TypeScript support built in — errors show as red underlines while typing, no extension needed. To try small snippets without any setup, use the [TypeScript Playground](https://www.typescriptlang.org/play).

**Common errors:** running `tsc` instead of `npx tsc` (not installed globally), running `node src/index.ts` instead of the compiled `dist/index.js`, editing files inside `dist/` (they are overwritten on every build).

---

## Part 1: TypeScript Basics

### 1. What is TypeScript

TypeScript is a strongly typed language built on JavaScript. It adds **static type checking at compile time**, catching errors during development instead of at runtime. Types are stripped during compilation — the runtime behavior is still plain JavaScript.

- Catches type bugs before running code
- Acts as living documentation for data shapes
- Powers IDE autocomplete and safe refactoring
- Makes large codebases easier to maintain

```ts
// JavaScript — no error until runtime
let age = 25;
age = "twenty-five";

// TypeScript — compile-time error
let age: number = 25;
age = "twenty-five"; // Error: Type 'string' is not assignable to type 'number'
```

```ts
// Real-world: function contracts
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}
calculateTotal("100", 5); // Compile-time error instead of NaN at runtime
```

**Best practices:** enable `strict` in `tsconfig.json`, avoid `any`, let TypeScript infer obvious types, annotate function signatures.

---

### 2. Type Annotations for Variables

Use `: type` after the variable name. TypeScript infers types from initial values, so annotate mainly when there's no initializer or the inferred type is wrong.

```ts
let username: string = "alice";
let score: number = 0;
let isActive: boolean = true;

username = 123; // Error

let apiUrl: string;        // annotation needed — no initial value
apiUrl = "https://api.example.com";
```

**Common errors:** over-annotating when inference works, forgetting annotations on uninitialized variables.

---

### 3. Primitive Types: string, number, boolean

```ts
let firstName: string = "John";
let fullName: string = `${firstName} Doe`;   // template literals produce strings

let quantity: number = 5;                    // all numbers are `number` (no int/float)
let total: number = quantity * 10.5;
console.log(total.toFixed(2));

let isLoggedIn: boolean = true;
if (isLoggedIn) console.log("Welcome back!");
```

Real-world example — typed domain logic:

```ts
interface CartItem {
  name: string;
  price: number;
  quantity: number;
}

function calculateTotal(items: CartItem[]): number {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
}
```

**Notes:** `NaN` and `Infinity` are `number`. Division by zero and floating-point precision are *not* caught by TypeScript. Name booleans with `is`/`has`/`can` prefixes.

---

### 4. Special Types: any, unknown, never, void

| Type | Meaning |
|------|---------|
| `any` | Disables type checking — avoid in new code |
| `unknown` | Safe `any` — must narrow before use |
| `never` | For functions that always throw / never return |
| `void` | For functions with no return value |

```ts
let flexible: any = 4;
flexible = "string";           // allowed — dangerous

let safe: unknown = 4;
// safe.toFixed();             // Error — must narrow first

function throwError(message: string): never {
  throw new Error(message);
}

function logMessage(message: string): void {
  console.log(message);
}
```

Narrowing `unknown` before use:

```ts
function processInput(input: unknown): string {
  if (typeof input === "string") {
    return input.toUpperCase();
  }
  return "Invalid input";
}
```

**Rule:** prefer `unknown` over `any`; use `never` for exhaustive checks.

---

### 5. Arrays and Tuples

```ts
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];   // equivalent, prefer type[]
let point: [number, number] = [10, 20];        // tuple: fixed length + types

let scores: number[] = [85, 90, 78];
scores.push(95);

let user: [string, number] = ["Alice", 25];    // user[0]: string, user[1]: number
```

```ts
interface Task {
  id: number;
  title: string;
  completed: boolean;
}

function getIncompleteTasks(tasks: Task[]): Task[] {
  return tasks.filter(task => !task.completed);
}
```

**Best practices:** tuples for fixed-size ordered data, `type[]` over `Array<type>`, `readonly` arrays when mutation isn't needed.

---

## Part 2: Objects and Interfaces

### 6. Object Type Annotations

```ts
let person: { name: string; age: number } = {
  name: "John",
  age: 30
};

function printUser(user: { name: string; email: string; age: number }): void {
  console.log(`${user.name} (${user.email}) is ${user.age}`);
}
```

Missing required properties, extra properties, and typos all produce compile-time errors. For reusable shapes, use an interface instead.

---

### 7. Interfaces

Interfaces define **reusable object structures**. Name them in PascalCase.

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

function getProductSummary(product: Product): string {
  return `${product.name} - $${product.price} (${product.category})`;
}

const laptop: Product = { id: 1, name: "MacBook Pro", price: 1999, category: "Electronics" };
```

---

### 8. Optional and Readonly Properties

- `?` after the name → optional (may be `undefined`)
- `readonly` before the name → cannot be reassigned

```ts
interface Config {
  apiUrl: string;
  timeout?: number;          // optional
  readonly version: string;  // immutable
}

function initializeApp(config: Config): void {
  const timeout = config.timeout ?? 5000;   // handle optional with a default
  console.log(`Connecting to ${config.apiUrl} (timeout ${timeout}ms)`);
}

initializeApp({ apiUrl: "https://api.example.com", version: "1.0.0" });
```

**Common errors:** mutating `readonly` props, forgetting `?`, not handling `undefined` for optionals.

---

### 9. Extending Interfaces

Use `extends` to model *is-a* relationships and avoid duplication. Keep base interfaces small.

```ts
interface BaseEntity {
  id: number;
  createdAt: Date;
}

interface User extends BaseEntity {
  name: string;
  email: string;
}

interface Post extends BaseEntity {
  title: string;
  content: string;
  authorId: number;
}
```

---

### 10. Type Aliases vs Interfaces

Both describe object shapes, but:

- `interface` — open/extendable, supports declaration merging; best for object shapes
- `type` — can express unions, intersections, tuples, primitives

```ts
interface Point {
  x: number;
  y: number;
}

// Declaration merging — interfaces can be reopened
interface Point {
  z?: number;
}

type ID = string | number;
type Status = "loading" | "success" | "error";

interface User {
  id: ID;
  status: Status;
}
```

**Rule of thumb:** `interface` for object shapes, `type` for unions/primitives/computed types. Be consistent.

---

## Part 3: Functions and Advanced Types

### 11. Function Type Annotations

Annotate every parameter and the return type (after the parentheses).

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}

const multiply = (a: number, b: number): number => a * b;

// Utility types compose well
interface User { id: number; name: string; }

function updateUser(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}
```

---

### 12. Union Types

A union (`|`) means "one of several types". You must **narrow** before using type-specific operations.

```ts
function printId(id: string | number): void {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

Discriminated unions model complex state safely:

```ts
type ApiResponse =
  | { status: "loading" }
  | { status: "success"; data: string[] }
  | { status: "error"; message: string };

function renderResponse(response: ApiResponse): string {
  if (response.status === "loading") return "Loading...";
  if (response.status === "success") return response.data.join(", ");
  return `Error: ${response.message}`;
}
```

Narrow with `typeof`, `in`, `instanceof`, or a discriminant property.

---

### 13. Literal Types and Type Narrowing

Literal types restrict a variable to exact values — great for config and state machines.

```ts
type Theme = "light" | "dark";

function applyTheme(theme: Theme): void {
  document.body.classList.toggle("dark", theme === "dark");
}

type Direction = "up" | "down" | "left" | "right";
function move(direction: Direction): void { /* ... */ }

move("up");
// move("forward"); // Error
```

---

### 14. Generics Introduction

Generics let you write reusable code that **preserves type information** — the type-safe alternative to `any`.

```ts
function identity<T>(value: T): T {
  return value;
}

function wrapInArray<T>(value: T): T[] {
  return [value];
}

const numbers = wrapInArray(5);    // number[]
const strings = wrapInArray("hi"); // string[]
```

Real-world — typed API responses:

```ts
interface ApiResponse<T> {
  data: T;
  status: number;
}

function fetchData<T>(url: string): Promise<ApiResponse<T>> {
  return fetch(url).then(res => res.json());
}

interface User { id: number; name: string; }

fetchData<User>("/api/users/1").then(res => console.log(res.data.name));
```

**Best practices:** use generics for reusable utilities, constrain with `extends` when needed (`<T extends { id: number }>`), let inference work where possible.

---

## Practice Exercises

0. **Setup** — create a new project from zero: `npm init -y`, install TypeScript, add `tsconfig.json`, print your name from `src/index.ts` using `npm run build` and `npm start`.
1. **Basic types** — declare `string`, `number`, `boolean`, `string[]`, `number[]` variables; write `sum(nums: number[]): number`.
2. **Interfaces** — define `Employee` (`id`, `name`, `department`, optional `managerId`); write a function that finds employees by department.
3. **Functions** — write `formatPrice(amount: number, currency?: string)` returning `"$19.99"` with `"$"` as default.
4. **Unions** — create a `Notification` union (email with `subject` | sms with `phone`); render each differently via narrowing.
5. **Generics** — write `lastItem<T>(arr: T[]): T | undefined`.

---

## Summary

- Setup: install Node.js (includes npm) → `npm init -y` → `npm i -D typescript` → `tsconfig.json` → `npx tsc` → `node dist/index.js`
- TypeScript adds compile-time type safety to JavaScript; types are erased at runtime
- Primitives: `string`, `number`, `boolean`; special types: `unknown` (prefer), `never`, `void`, avoid `any`
- Arrays: `type[]`; tuples: `[A, B]` for fixed shapes
- `interface` for object shapes (extendable, mergeable); `type` for unions and primitives
- `?` optional, `readonly` immutable properties
- Annotate function params + return types
- Narrow unions/literals before use; discriminated unions for complex state
- Generics (`<T>`) keep reusable code type-safe
