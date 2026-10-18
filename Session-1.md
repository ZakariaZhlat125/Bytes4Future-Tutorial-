# Session 1: TypeScript Essentials — Types, Interfaces, Functions, and Generics

**Duration:** 3 hours
**Level:** Beginner
**Prerequisites:** JavaScript fundamentals (variables, functions, objects, arrays). No tools needed — we install everything from zero in Part 0.

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

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

> 💡 **In simple words:** Node.js lets your computer run JavaScript outside the browser. npm is like an app store for code: it downloads tools and libraries for you. You install both once per computer.

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

> 💡 **In simple words:** A project is just a folder. `npm init -y` creates `package.json`, a file that describes the project: its name, its commands, and the packages it uses. `-y` means "answer yes to all the questions".

```bash
mkdir ts-session-1
cd ts-session-1
npm init -y        # creates package.json
code .             # open the folder in VS Code
```

`package.json` is the project's ID card: its name, its scripts, and the list of packages it depends on.

---

### 0.3 Install TypeScript

> 💡 **In simple words:** TypeScript is a package too. We install it **inside** the project so everyone on the team uses the same version. npm puts the downloaded code in `node_modules/`. You never edit that folder by hand.

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

> 💡 **In simple words:** `tsconfig.json` is the settings file of the TypeScript compiler. It answers three questions: Where is my code? (`src`) Where should the JavaScript go? (`dist`) How strict should the checking be? (`strict: true`)

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

> 💡 **In simple words:** The workflow is always: write `.ts` → compile with `tsc` → run the `.js` file with `node`. The scripts in `package.json` are shortcuts, so you type `npm run build` instead of remembering long commands.

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

### ✍️ Your Turn: Part 0

**Task:** create a new project called `my-first-ts` on your own. Type the commands; don't copy them.

**1.** Fill the blanks in the commands:

```bash
mkdir my-first-ts
cd my-first-ts
npm ____ -y                    # create package.json
npm install ____ typescript    # install TypeScript as a development tool
npx ____ -v                    # check that the compiler works
```

**2.** Complete `tsconfig.json`:

```jsonc
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "nodenext",
    "rootDir": "____",   // folder that has your .ts files
    "outDir": "____",    // folder for the compiled .js files
    "strict": ____       // turn on all type checks
  },
  "include": ["src"]
}
```

**3.** Complete the scripts in `package.json`:

```json
"scripts": {
  "build": "____",
  "start": "node ____"
}
```

**4.** Create `src/index.ts` that prints: `My name is <your name> and I am <your age> years old.` Use one `string` variable and one `number` variable. Run it with `npm run build` then `npm start`.

<details>
<summary>✅ Solution</summary>

```bash
npm init -y
npm install --save-dev typescript    # or: npm i -D typescript
npx tsc -v
```

```jsonc
"rootDir": "src",
"outDir": "dist",
"strict": true
```

```json
"scripts": {
  "build": "tsc",
  "start": "node dist/index.js"
}
```

```ts
// src/index.ts
const myName: string = "Sara";
const myAge: number = 21;
console.log(`My name is ${myName} and I am ${myAge} years old.`);
```

</details>

---

## Part 1: TypeScript Basics

### 1. What is TypeScript

> 💡 **In simple words:** TypeScript = JavaScript + **types**. A type says what kind of value a variable holds: text, a number, true/false, and so on. TypeScript checks your code **before** it runs, like a spell-checker for programming mistakes. The final result is still normal JavaScript.

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

> 💡 **In simple words:** An annotation is a label on a variable. `let age: number` means "age can only hold numbers". When TypeScript can see the type from the value (`let age = 25`), you don't need to write it. This is called **inference**.

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

> 💡 **In simple words:** These are the three most basic kinds of values. `string` is text in quotes (`"Ali"`). `number` is any number, whole or decimal (`5`, `9.99`). `boolean` is only `true` or `false`, like a light switch.

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

> 💡 **In simple words:**
> - `any` means "I don't care what it is". TypeScript stops checking, which is dangerous.
> - `unknown` means "I don't know yet". You must check the type before you use it, which is safe.
> - `void` means the function returns nothing.
> - `never` means the function never finishes normally (for example, it always throws an error).

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

> 💡 **In simple words:** An **array** is a list of values of the same type: `number[]` is a list of numbers of any length. A **tuple** is a small array with a fixed size, where each position has its own type, like `[name, age]`.

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

### ✍️ Your Turn: Part 1

Write this in `src/index.ts`. Fill every `____` and finish every `// TODO`.

```ts
// 1. Add the correct type to each variable
let studentName: ____ = "Sara";
let age: ____ = 20;
let isPassed: ____ = true;
let grades: ____ = [90, 85, 77];
let student: ____ = ["Sara", 20];   // tuple: first the name, then the age

// 2. TypeScript shows an error on the next line. Why?
//    Fix the VALUE, not the type.
let price: number = "50";

// 3. TODO: finish the function. It returns the average of the numbers.
function average(nums: number[]): number {
  // TODO
}
console.log(average(grades)); // 84

// 4. TODO: write a function sayHi(name: string): void
//    that prints "Hi, <name>!" and returns nothing.

// 5. Narrow the unknown value before using it
function describe(value: unknown): string {
  if (typeof value === "____") {
    return `Number: ${value.toFixed(1)}`;
  }
  // TODO: if value is a string, return "Text: " + the value in UPPERCASE
  return "Something else";
}
console.log(describe(3.14159)); // Number: 3.1
console.log(describe("hello")); // Text: HELLO
console.log(describe(true));    // Something else
```

<details>
<summary>✅ Solution</summary>

```ts
let studentName: string = "Sara";
let age: number = 20;
let isPassed: boolean = true;
let grades: number[] = [90, 85, 77];
let student: [string, number] = ["Sara", 20];

// "50" has quotes, so it is a string. A number has no quotes.
let price: number = 50;

function average(nums: number[]): number {
  let sum = 0;
  for (const n of nums) {
    sum += n;
  }
  return sum / nums.length;
}

function sayHi(name: string): void {
  console.log(`Hi, ${name}!`);
}

function describe(value: unknown): string {
  if (typeof value === "number") {
    return `Number: ${value.toFixed(1)}`;
  }
  if (typeof value === "string") {
    return "Text: " + value.toUpperCase();
  }
  return "Something else";
}
```

</details>

---

## Part 2: Objects and Interfaces

### 6. Object Type Annotations

> 💡 **In simple words:** You can describe the **shape** of an object: which properties it has and the type of each one. If you forget a property or misspell one, TypeScript tells you right away.

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

> 💡 **In simple words:** An interface is a **named** description of an object, like a form template. Once you define `Product`, every product must have the same fields. You reuse the name everywhere instead of repeating the whole shape.

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

> 💡 **In simple words:** `?` means "this property may be missing", like an optional field in a form. `readonly` means "you set it when you create the object, and you can never change it later", like an ID number.

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

> 💡 **In simple words:** `extends` copies all the properties of one interface into another and lets you add more. `User extends BaseEntity` means a User has everything BaseEntity has, plus its own fields. You write the shared fields only once.

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

> 💡 **In simple words:** Both give a **name** to a type. Use `interface` for objects. Use `type` when you need something an interface can't do, like "this OR that" (`string | number`) or a list of allowed words (`"small" | "large"`).

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

### ✍️ Your Turn: Part 2

```ts
// 1. Complete the interface
interface Book {
  id: ____;
  title: ____;
  author: ____;
  pages: ____;
  isAvailable?: ____;   // optional: a book may not have this value
}

// 2. TODO: create two books that match the interface.
//    book1 has NO isAvailable value, book2 has isAvailable: false
const book1: Book = { /* TODO */ };
const book2: Book = { /* TODO */ };

// 3. TODO: write printBook(book: Book): void
//    It prints: "Clean Code by Robert Martin (464 pages)"

// 4. TODO: create an interface EBook that EXTENDS Book
//    and adds fileSizeMB: number. Then create one EBook object.

// 5. TODO: create a type alias Genre that can only be
//    "novel", "science" or "history". Then try: let g: Genre = "sport";
//    What error do you get?

// 6. Change `id` in Book to readonly. Then try: book1.id = 99;
//    What error do you get?
```

<details>
<summary>✅ Solution</summary>

```ts
interface Book {
  readonly id: number;        // step 6
  title: string;
  author: string;
  pages: number;
  isAvailable?: boolean;
}

const book1: Book = { id: 1, title: "Clean Code", author: "Robert Martin", pages: 464 };
const book2: Book = { id: 2, title: "The Alchemist", author: "Paulo Coelho", pages: 208, isAvailable: false };

function printBook(book: Book): void {
  console.log(`${book.title} by ${book.author} (${book.pages} pages)`);
}
printBook(book1); // Clean Code by Robert Martin (464 pages)

interface EBook extends Book {
  fileSizeMB: number;
}
const ebook: EBook = { id: 3, title: "TS Basics", author: "Ali", pages: 120, fileSizeMB: 4.5 };

type Genre = "novel" | "science" | "history";
let g: Genre = "science";
// let g2: Genre = "sport";  // Error: Type '"sport"' is not assignable to type 'Genre'

// book1.id = 99;            // Error: Cannot assign to 'id' because it is a read-only property
```

</details>

---

## Part 3: Functions and Advanced Types

### 11. Function Type Annotations

> 💡 **In simple words:** For a function, write the type of each **parameter** (the inputs) and the **return type** (the output) after the parentheses. Then TypeScript stops anyone from calling the function with the wrong inputs.

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

> 💡 **In simple words:** A union (`|`) means "one of these types": `string | number` can be text **or** a number. Before you use something that only works on one of them (like `.toUpperCase()`), check which one you have. That check is called **narrowing**.

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

> 💡 **In simple words:** A literal type allows only **exact** values. `"light" | "dark"` accepts only those two words. It's like a dropdown menu instead of a free text box, so a typo becomes an error.

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

> 💡 **In simple words:** Generics are type **variables**. `<T>` is a placeholder that gets filled in when the function is used. Call it with numbers and `T` becomes `number`; call it with strings and `T` becomes `string`. One function works for every type and is still type-safe.

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

### ✍️ Your Turn: Part 3

```ts
// 1. Add types to the parameters and to the return value
function add(a: ____, b: ____): ____ {
  return a + b;
}

// 2. Give `greeting` a default value so both calls work
function greet(name: string, greeting: string = ____): string {
  return `${greeting}, ${name}!`;
}
console.log(greet("Ali"));            // Hello, Ali!
console.log(greet("Ali", "Welcome")); // Welcome, Ali!

// 3. Union + narrowing
function formatId(id: string | number): string {
  // TODO: if id is a number → return "ID-" + id
  //       if id is a string → return id in UPPERCASE
}
console.log(formatId(7));      // ID-7
console.log(formatId("ab12")); // AB12

// 4. Literal types
type TrafficLight = ____;   // only "red", "yellow" or "green"

function action(light: TrafficLight): string {
  // TODO: return "Stop" for red, "Slow down" for yellow, "Go" for green
}

// 5. Generics: works with ANY array type
function firstItem<T>(items: ____): T | undefined {
  return items[0];
}
console.log(firstItem([10, 20, 30])); // 10
console.log(firstItem(["a", "b"]));   // a
```

<details>
<summary>✅ Solution</summary>

```ts
function add(a: number, b: number): number {
  return a + b;
}

function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}

function formatId(id: string | number): string {
  if (typeof id === "number") {
    return "ID-" + id;
  }
  return id.toUpperCase();   // here TypeScript knows id is a string
}

type TrafficLight = "red" | "yellow" | "green";

function action(light: TrafficLight): string {
  if (light === "red") return "Stop";
  if (light === "yellow") return "Slow down";
  return "Go";
}

function firstItem<T>(items: T[]): T | undefined {
  return items[0];
}
```

</details>

---

## Practice Exercises

0. **Setup** — create a new project from zero: `npm init -y`, install TypeScript, add `tsconfig.json`, print your name from `src/index.ts` using `npm run build` and `npm start`.
1. **Basic types** — declare `string`, `number`, `boolean`, `string[]`, `number[]` variables; write `sum(nums: number[]): number`.
2. **Interfaces** — define `Employee` (`id`, `name`, `department`, optional `managerId`); write a function that finds employees by department.
3. **Functions** — write `formatPrice(amount: number, currency?: string)` returning `"$19.99"` with `"$"` as default.
4. **Unions** — create a `Notification` union (email with `subject` | sms with `phone`); render each differently via narrowing.
5. **Generics** — write `lastItem<T>(arr: T[]): T | undefined`.

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. Browsers and Node.js can't run `.ts` files. What do we do so the code can run?
2. What is the difference between `dependencies` and `devDependencies`?
3. Why do we add `node_modules/` to `.gitignore`?
4. What is the difference between `any` and `unknown`? Which one is safer?
5. What is a tuple? Write one example.
6. In an interface, what does `?` mean? What does `readonly` mean?
7. When do you use `interface`, and when do you use `type`?
8. What is a union type? Give an example.
9. Why is a generic function (`<T>`) better than a function that uses `any`?
10. Do TypeScript types still exist when the code runs? Explain.

<details>
<summary>✅ Solution</summary>

1. We compile them with `tsc` (`npx tsc`). It turns `.ts` into `.js`, and then we run the `.js` file with `node`.
2. `dependencies` are needed when the app runs. `devDependencies` are only needed while developing (for example TypeScript).
3. It is very big and anyone can rebuild it with `npm install` from `package.json`.
4. `any` turns off type checking. `unknown` forces you to check the type before you use the value. `unknown` is safer.
5. An array with a fixed length and a fixed type at each position: `let point: [number, number] = [10, 20];`
6. `?` means the property is optional (it can be missing). `readonly` means you can't change it after creating the object.
7. `interface` for object shapes. `type` for unions, literal types and other non-object types.
8. A value that can be one of several types: `let id: string | number;`
9. A generic keeps the real type, so `firstItem([1, 2])` returns `number`. With `any` you lose all type checking.
10. No. Types are removed during compilation; the `.js` output is plain JavaScript.

</details>

### B. Find the Bug

Each snippet has **one** error. Write what the error is, then fix it.

```ts
// Bug 1
let total: number = 10;
total = "20";

// Bug 2
interface User { name: string; age: number; }
const u: User = { name: "Omar" };

// Bug 3
interface Config { readonly version: string; }
const config: Config = { version: "1.0" };
config.version = "2.0";

// Bug 4
function getLength(x: string | number): number {
  return x.length;
}

// Bug 5
let colors: string[] = ["red", "blue"];
colors.push(5);

// Bug 6
type Size = "small" | "medium" | "large";
let mySize: Size = "big";
```

<details>
<summary>✅ Solution</summary>

```ts
// Bug 1: a string can't go in a number variable
total = 20;

// Bug 2: `age` is required but missing
const u: User = { name: "Omar", age: 30 };

// Bug 3: `version` is readonly, so it can't be changed.
// Fix: create a new object instead
const newConfig: Config = { version: "2.0" };

// Bug 4: a number has no .length, so narrow first
function getLength(x: string | number): number {
  if (typeof x === "string") return x.length;
  return x.toString().length;
}

// Bug 5: the array only accepts strings
colors.push("green");

// Bug 6: "big" is not one of the allowed values
let mySize: Size = "large";
```

</details>

### C. Coding Tasks

**Task 1: Temperature converter (easy)**
Write `toFahrenheit(c: number): number` (F = C × 9 / 5 + 32) and `toCelsius(f: number): number`.
Test: `toFahrenheit(100)` → `212`, `toCelsius(32)` → `0`.

**Task 2: Student grades (easy)**

```ts
interface Student {
  name: string;
  grade: number;
}

const students: Student[] = [
  { name: "Ali", grade: 85 },
  { name: "Mona", grade: 42 },
  { name: "Omar", grade: 67 },
  { name: "Lina", grade: 95 },
];

// TODO 1: getPassed(students: Student[]): Student[]
//         → only students with grade >= 50
// TODO 2: getTopStudent(students: Student[]): Student
//         → the student with the highest grade
// TODO 3: getLetter(grade: number): "A" | "B" | "C" | "F"
//         → 90+ is A, 75+ is B, 50+ is C, otherwise F
```

Expected: `getPassed` → Ali, Omar, Lina · `getTopStudent` → Lina · `getLetter(67)` → `"C"`.

**Task 3: Shopping cart total (medium)**

```ts
interface CartItem {
  name: string;
  price: number;
  quantity: number;
  discount?: number;   // percent: 25 means 25% off
}

const cart: CartItem[] = [
  { name: "Pen", price: 2, quantity: 3 },
  { name: "Bag", price: 40, quantity: 1, discount: 25 },
];

// TODO: cartTotal(items: CartItem[]): number
// For each item: price × quantity, then subtract the discount if it exists.
console.log(cartTotal(cart)); // 36   (6 + 30)
```

**Task 4: Shapes (medium: union + narrowing)**

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number };

// TODO 1: area(shape: Shape): number
//         circle → Math.PI * r * r, rectangle → width * height
// TODO 2: add a third shape { kind: "square"; side: number } and support it in area()
```

**Task 5: Generics (medium)**

```ts
// TODO 1: lastItem<T>(arr: T[]): T | undefined
// TODO 2: makePair<A, B>(a: A, b: B): [A, B]
// TODO 3: findById<T extends { id: number }>(items: T[], id: number): T | undefined

lastItem([1, 2, 3]);                       // 3
makePair("age", 30);                       // ["age", 30]
findById([{ id: 1, name: "Ali" }], 1);     // { id: 1, name: "Ali" }
```

<details>
<summary>✅ Solution: Tasks 1–5</summary>

```ts
// Task 1
function toFahrenheit(c: number): number {
  return (c * 9) / 5 + 32;
}
function toCelsius(f: number): number {
  return ((f - 32) * 5) / 9;
}

// Task 2
function getPassed(students: Student[]): Student[] {
  return students.filter(s => s.grade >= 50);
}
function getTopStudent(students: Student[]): Student {
  return students.reduce((best, s) => (s.grade > best.grade ? s : best));
}
function getLetter(grade: number): "A" | "B" | "C" | "F" {
  if (grade >= 90) return "A";
  if (grade >= 75) return "B";
  if (grade >= 50) return "C";
  return "F";
}

// Task 3
function cartTotal(items: CartItem[]): number {
  let total = 0;
  for (const item of items) {
    let itemTotal = item.price * item.quantity;
    if (item.discount) {
      itemTotal = itemTotal - (itemTotal * item.discount) / 100;
    }
    total += itemTotal;
  }
  return total;
}

// Task 4
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number }
  | { kind: "square"; side: number };

function area(shape: Shape): number {
  if (shape.kind === "circle") return Math.PI * shape.radius * shape.radius;
  if (shape.kind === "rectangle") return shape.width * shape.height;
  return shape.side * shape.side;
}

// Task 5
function lastItem<T>(arr: T[]): T | undefined {
  return arr[arr.length - 1];
}
function makePair<A, B>(a: A, b: B): [A, B] {
  return [a, b];
}
function findById<T extends { id: number }>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}
```

</details>

### D. Challenge: Mini Library

```ts
interface LibraryBook {
  id: number;
  title: string;
  isBorrowed: boolean;
}

const library: LibraryBook[] = [
  { id: 1, title: "Clean Code", isBorrowed: false },
  { id: 2, title: "The Alchemist", isBorrowed: true },
];

// TODO 1: addBook(title: string): void
//         new id = last book's id + 1, isBorrowed = false
// TODO 2: borrowBook(id: number): string
//         returns "Borrowed: <title>", "Already borrowed" or "Book not found"
// TODO 3: returnBook(id: number): string
//         returns "Returned: <title>", "This book was not borrowed" or "Book not found"
// TODO 4: availableBooks(): LibraryBook[]
//         returns only books that are not borrowed
```

<details>
<summary>✅ Solution</summary>

```ts
function addBook(title: string): void {
  const nextId = library.length > 0 ? library[library.length - 1].id + 1 : 1;
  library.push({ id: nextId, title, isBorrowed: false });
}

function borrowBook(id: number): string {
  const book = library.find(b => b.id === id);
  if (!book) return "Book not found";
  if (book.isBorrowed) return "Already borrowed";
  book.isBorrowed = true;
  return `Borrowed: ${book.title}`;
}

function returnBook(id: number): string {
  const book = library.find(b => b.id === id);
  if (!book) return "Book not found";
  if (!book.isBorrowed) return "This book was not borrowed";
  book.isBorrowed = false;
  return `Returned: ${book.title}`;
}

function availableBooks(): LibraryBook[] {
  return library.filter(b => !b.isBorrowed);
}
```

</details>

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
