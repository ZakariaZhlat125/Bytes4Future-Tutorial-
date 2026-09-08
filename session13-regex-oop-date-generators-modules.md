# Session 13: Regex, OOP, Date, Generators & Modules — Student Active-Learning Lab

Welcome to this session. Today you will unlock five powerful JavaScript tools: **regex** for pattern matching, **OOP** for reusable blueprints, `Date` for time, **generators** for controlled iteration, and **modules** for organizing code into files. Do not just read — predict, type, run, and fix.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Do not look at the code yet.
3. **Write your prediction** in a comment or notebook.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before you look at the answer key.**

---

## Part 0: Warm-Up — One Program, Five Problems

### The problem

A real program needs to: validate an email, create a reusable blueprint for users, work with dates, produce values one at a time, and split code into separate files.

### 🤔 Think

Which JavaScript features could help with each of these tasks?

```text
Validate an email:            __________
Blueprint for users:          __________
Work with dates:              __________
Values one at a time:         __________
Split code into files:        __________
```

### 🔮 Predict

Look at this code and write what you think will print:

```javascript
console.log(/hello/i.test("HELLO")); // ?
console.log(new Date().getFullYear()); // ?
```

### ✅ Result

Run the code.

### 🧠 Discover

- `/hello/i` is a **regular expression** — the `i` flag makes it case-insensitive, so it matches `"HELLO"`.
- `new Date()` creates a date object for "right now", and `getFullYear()` returns the current year.

These tools are powerful and used together in real applications.

### 🧪 Experiment

Change `/hello/i` to `/hello/` (no flag) and run again. Does it still match `"HELLO"`? Why or why not?

---

## Part 1: Regex

### 1.1 Creating and Testing Regex

### The problem

You need to check if a string contains a specific pattern — like checking whether a user's message contains the word "hello".

### 🔮 Predict

```javascript
const pattern = /hello/;
const text = "hello world";

console.log(pattern.test(text));    // ?
console.log(text.match(pattern));   // ?

const pattern2 = new RegExp("world");
console.log(pattern2.test(text));   // ?
```

What will each `console.log` print? What is the difference between `test` and `match`?

### ✅ Result

Run it.

### 🧠 Why?

- A regex can be written between slashes: `/hello/`, or with the constructor: `new RegExp("world")`.
- `pattern.test(text)` returns `true` or `false`.
- `text.match(pattern)` returns the matched text (or `null`).

### Challenge 1.1 — Test Pattern

Use `test` to check if `"javascript"` contains the pattern `/script/`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(/script/.test("javascript"));
```

</details>

---

### 1.2 Flags and Character Classes

### The problem

A single fixed word is not enough. You need to match digits, letters, spaces — and find every match, not just the first.

### 🔮 Predict

```javascript
console.log(/hello/i.test("HELLO"));        // ?
console.log("hello hello".match(/hello/g)); // ?

console.log(/[aeiou]/.test("apple"));       // ?
console.log(/\d/.test("abc123"));           // ?
console.log(/\w/.test("_"));                // ?
console.log(/\s/.test("a b"));              // ?
```

What will each line print? What do you think `i`, `g`, `\d`, `\w`, and `\s` mean?

### ✅ Result

Run it.

### 🧠 Why?

- `i` — case-insensitive. `g` — global (find all matches).
- `[aeiou]` — a **character class**: any one of these letters.
- `\d` — any digit. `\w` — any word character (letter, digit, `_`). `\s` — whitespace.

### 🧪 Experiment

Try `"HELLO HELLO".match(/hello/gi)`. How many matches do you get? What happens without `i`?

### Challenge 1.2 — Match Digits

Write a regex to test if a string has at least one digit, and use it on `"Room 42"`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(/\d/.test("Room 42"));
```

</details>

---

### 1.3 Quantifiers

### The problem

How do you say "exactly 3 digits" or "one or more letters" without writing the pattern over and over?

### 🔮 Predict

```javascript
console.log(/a*/.test(""));        // ?
console.log(/a+/.test("a"));       // ?
console.log(/a?/.test("b"));       // ?
console.log(/a{3}/.test("aaa"));   // ?
console.log(/\d{2,4}/.test("12")); // ?
```

For each line, what do you think will print? What do `*`, `+`, `?`, and `{n}` mean?

### ✅ Result

Run it.

### 🧠 Why?

- `*` — 0 or more. `+` — 1 or more. `?` — 0 or 1.
- `{3}` — exactly 3. `{2,4}` — between 2 and 4.

### Challenge 1.3 — Validate Phone Pattern

Use a regex to test if a string starts with exactly 3 digits followed by a dash.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(/^\d{3}-/.test("123-456"));
```

</details>

**Hint:** `^\d{3}-` (`^` means "start of the string")

---

## Part 2: OOP with Constructor Functions and Prototypes

### 2.1 Constructor Functions

### The problem

You need to create many objects with the same shape — 100 users, 50 products, 200 cars. Copying object literals does not scale.

### 🔮 Predict

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person("John", 30);
console.log(john.greet()); // ?
```

What will print? Where does `greet` come from — it is not inside `Person`!

### ✅ Result

Run it.

### 🧠 Why?

- A **constructor function** is a normal function used with `new`. Inside it, `this` is the new object being built.
- Methods go on `Person.prototype` so all instances **share** one copy instead of each carrying their own.

### Challenge 2.1 — Constructor and Prototype

Create a `Car(make, speed)` constructor and add `accelerate(amount)` and `brake(amount)` to its prototype.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function Car(make, speed) {
  this.make = make;
  this.speed = speed;
}

Car.prototype.accelerate = function(amount) {
  this.speed += amount;
  return this.speed;
};

Car.prototype.brake = function(amount) {
  this.speed = Math.max(0, this.speed - amount);
  return this.speed;
};
```

</details>

---

### 2.2 `hasOwnProperty` and Prototype Chain

### 🔮 Predict

```javascript
console.log(john.hasOwnProperty("name"));         // ?
console.log(john.hasOwnProperty("greet"));        // ?
console.log(john.__proto__ === Person.prototype); // ?
```

`john` can call `greet` — so why might `hasOwnProperty("greet")` be `false`?

### ✅ Result

Run it.

### 🧠 Why?

When you access `john.greet`, JavaScript first looks on `john` itself. If it is not found, it walks up the **prototype chain** to `Person.prototype`. `hasOwnProperty` only checks the object itself — `greet` lives on the prototype, not on `john`.

### 🧪 Experiment

Add `john.greet = () => "custom";` then check `john.hasOwnProperty("greet")` again. What changed and why?

---

## Part 3: ES6 Classes

### 3.1 Basic Classes

### The problem

Constructor functions plus `.prototype` work, but the syntax is scattered. There is a cleaner way to write the same thing.

### 🔮 Predict

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

const animal = new Animal("Buddy");
console.log(animal.speak()); // ?
```

What will print? How does this compare to the constructor function version?

### ✅ Result

Run it.

### 🧠 Why?

`class` is modern syntax for the same constructor + prototype idea. `constructor(name)` runs on `new`, and `speak()` is automatically placed on the prototype.

### Challenge 3.1 — Create a Class

Write a `Rectangle` class with `width`, `height`, and an `area()` method.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Rectangle {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }
  area() { return this.w * this.h; }
}
```

</details>

---

### 3.2 Inheritance and `super`

### The problem

A `Dog` **is** an `Animal` — it should reuse the parent's code and add its own. How do we connect them?

### 🔮 Predict

```javascript
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    return `${this.name} barks`;
  }
}

const dog = new Dog("Buddy", "Golden");
console.log(dog.speak()); // ?
```

What will print — `"makes a sound"` or `"barks"`? What does `super(name)` do?

### ✅ Result

Run it.

### 🧠 Why?

- `extends` makes `Dog` a **subclass** of `Animal` — it inherits everything.
- `super(name)` calls the parent constructor. You **must** call it before using `this` in a subclass.
- `Dog`'s own `speak()` **overrides** the parent's version.

### Challenge 3.2 — Inheritance

Create `Employee` that extends `Person` (with `name`, `salary`) and add a `getInfo()` method.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Employee extends Person {
  constructor(name, salary) {
    super(name);
    this.salary = salary;
  }
  getInfo() {
    return `${this.name} earns $${this.salary}`;
  }
}
```

</details>

---

## Part 4: Encapsulation

### 4.1 Private Fields

### The problem

A bank account's balance should not be changeable from outside — `account.balance = -9999` must be impossible.

### 🔮 Predict

```javascript
class BankAccount {
  #balance;

  constructor(initial) {
    this.#balance = initial;
  }

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount(100);
acc.deposit(50);
console.log(acc.getBalance()); // ?
// console.log(acc.#balance);  // what happens if you uncomment this?
```

What will `getBalance()` return? What happens if you try `acc.#balance` from outside?

### ✅ Result

Run it.

### 🧠 Why?

`#balance` is a **private field**. It exists only inside the class. Code outside cannot read or write it — trying to is a `SyntaxError`. The only way in is through the public methods you choose to provide.

### Challenge 4.1 — Private Field

Create a `Counter` class with a private `#count` and `increment()` and `getCount()` methods.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Counter {
  #count = 0;
  increment() { return ++this.#count; }
  getCount() { return this.#count; }
}
```

</details>

---

### 4.2 Closures for Privacy

### The problem

Before `#fields` existed, developers hid data with **closures**.

### 🔮 Predict

```javascript
function createCounter() {
  let count = 0;
  return {
    increment() { return ++count; },
    get() { return count; }
  };
}

const c = createCounter();
console.log(c.increment()); // ?
console.log(c.count);       // ?
```

What will each line print? Can you reach `count` directly?

### ✅ Result

Run it.

### 🧠 Why?

`count` lives inside `createCounter`'s scope. The returned methods are closures that remember it — but nothing outside can touch `count` directly.

---

### 4.3 Getters and Setters

### The problem

You want `t.fahrenheit` to *look* like a property but secretly *compute* a value — and assignments like `t.fahrenheit = 212` to update Celsius.

### 🔮 Predict

```javascript
class Temperature {
  constructor(c) {
    this._c = c;
  }

  get celsius() { return this._c; }
  set celsius(value) { this._c = value; }

  get fahrenheit() { return this._c * 9 / 5 + 32; }
  set fahrenheit(value) { this._c = (value - 32) * 5 / 9; }
}

const t = new Temperature(0);
console.log(t.fahrenheit); // ?
t.fahrenheit = 212;
console.log(t.celsius);    // ?
```

What will each line print? Notice — no parentheses on `t.fahrenheit`!

### ✅ Result

Run it.

### 🧠 Why?

`get` and `set` define **computed properties**. Reading `t.fahrenheit` runs the getter; assigning `t.fahrenheit = 212` runs the setter. The underscore in `this._c` is a *convention* meaning "treat as internal" (unlike `#`, it is not truly private).

### 🧪 Experiment

Set `t.celsius = 100` and read `t.fahrenheit`. Then set `t.fahrenheit = 32` and read `t.celsius`. Do the conversions work both ways?

---

## Bug Hunt 1

### The mission

Find the bug in this OOP code. Do not run it yet. Read and write what you think is wrong.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    this.breed = breed;
  }
}

const d = new Dog("Buddy", "Golden");
console.log(d.name);
```

### 🐛 What I think is wrong

1. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}
```

</details>

---

## Part 5: Date and Time

### 5.1 Creating and Reading Dates

### The problem

Your app needs to show "member since 2021" and count down to a deadline. You need to work with real dates.

### 🔮 Predict

```javascript
const now = new Date();
console.log(now.getFullYear()); // ?
console.log(now.getMonth());    // ?
console.log(now.getDate());     // ?
console.log(now.getDay());      // ?
```

Which of these returns a number from `0–11`? Which returns `0–6`? What is the difference between `getDate` and `getDay`?

### ✅ Result

Run it.

### 🧠 Why?

- `getMonth()` is **0-based**: January is `0`, December is `11`. (A classic bug source!)
- `getDate()` — day of the month (1–31). `getDay()` — day of the **week** (0 = Sunday, 6 = Saturday).

### Challenge 5.1 — Format Date

Create a date for `2025-12-25` and log the year, month, and day.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const date = new Date("2025-12-25");
console.log(date.getFullYear(), date.getMonth() + 1, date.getDate());
```

</details>

---

### 5.2 Date Arithmetic

### The problem

How many days until the deadline? Subtracting dates needs a trick.

### 🔮 Predict

```javascript
const now = new Date();
const tomorrow = new Date(now.getTime() + 24 * 60 * 60 * 1000);
const diff = tomorrow - now;
console.log(diff / (1000 * 60 * 60)); // ?
```

What unit is `diff` in? What will the last line print?

### ✅ Result

Run it.

### 🧠 Why?

`getTime()` returns **milliseconds** since Jan 1, 1970. Subtracting dates gives milliseconds — divide by `1000 * 60 * 60` for hours, or `1000 * 60 * 60 * 24` for days.

### Challenge 5.2 — Days Between

Calculate how many days are between `2024-01-01` and `2024-12-31`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const start = new Date("2024-01-01");
const end = new Date("2024-12-31");
const days = (end - start) / (1000 * 60 * 60 * 24);
console.log(days);
```

</details>

---

## Part 6: Generators

### 6.1 Basic Generator

### The problem

A normal function returns once and is done. You want a function that hands out values **one at a time**, pausing between each — like a vending machine.

### 🔮 Predict

```javascript
function* count() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = count();
console.log(gen.next().value); // ?
console.log(gen.next().value); // ?
console.log(gen.next().done);  // ?
```

Notice the `function*` — what do you think `yield` does? What is `gen.next()`?

### ✅ Result

Run it.

### 🧠 Why?

- `function*` declares a **generator**. Calling it does *not* run the body — it returns a generator object.
- Each `gen.next()` runs the code up to the next `yield`, then **pauses** and returns `{ value, done }`.
- `done` becomes `true` when the function finishes.

### 🧪 Experiment

Call `gen.next()` a fourth time. What are `value` and `done` now?

### Challenge 6.1 — Range Generator

Write a generator `range(start, end)` that yields numbers from `start` to `end`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}
```

</details>

---

## Bug Hunt 2

### The mission

Find the bug in this generator code. Do not run it yet. Read and write what you think is wrong.

```javascript
function* count() {
  let n = 0;
  while (n < 3) {
    return n;
    n++;
  }
}

for (const value of count()) {
  console.log(value);
}
```

### 🐛 What I think is wrong

1. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function* count() {
  let n = 0;
  while (n < 3) {
    yield n;
    n++;
  }
}
```

</details>

---

## Part 7: Modules

### 7.1 Named and Default Exports

### The problem

All your code in one giant file does not scale. You want to split reusable pieces into separate files and pull in only what you need.

### 🔮 Predict

```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }

// main.js
import multiply, { PI, add } from "./math.js";
console.log(add(2, 3));      // ?
console.log(PI);             // ?
console.log(multiply(2, 3)); // ?
```

What will each line print? Why is `multiply` outside the braces but `PI` and `add` inside?

### ✅ Result

Run it (you will need two files — `math.js` and `main.js` — and a browser page or Node with modules enabled).

### 🧠 Why?

- `export const` / `export function` — **named exports**. Import them by exact name inside `{ }`.
- `export default` — one **default export** per file. Import it with *any* name, no braces.
- In the browser, the script tag needs `type="module"`.

### Challenge 7.1 — Write a Module

Write a module that exports `greet(name)` as a named export and a `User` class as a default export. Then write the import statement.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
// utils.js
export function greet(name) { return `Hello, ${name}`; }
export default class User {}

// main.js
import User, { greet } from "./utils.js";
```

</details>

---

### 7.2 Import All

### The problem

A module has 20 exports and you want them all — writing 20 names in braces is painful.

### 🔮 Predict

```javascript
import * as math from "./math.js";
console.log(math.add(1, 2)); // ?
```

What is `math` here?

### ✅ Result

Run it.

### 🧠 Why?

`import * as math` bundles **all** exports into one namespace object. Access each one as `math.add`, `math.PI`, and so on.

### 🧪 Experiment

Add `console.log(Object.keys(math))`. What do you see?

---

## Group Challenge: Advanced JavaScript Race

If you are working in a group, split into teams of 2 or 3. Each team completes one task — or race to finish all four. Set a timer for 12 minutes.

### Tasks

1. Write a regex to test if a string is an email (simple: `something@something.something`).
2. Create a `Book` class that extends `Product` (with `title` and `price`).
3. Create a `Date` for next Monday and log it.
4. Write a generator that yields even numbers from 0 to 10.

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Regex

Test if `"JavaScript"` starts with `J` (case-sensitive).

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(/^J/.test("JavaScript"));
```

</details>

### Level 2: Class

Create a `Circle` class with `radius` and `area()`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Circle {
  constructor(r) { this.r = r; }
  area() { return Math.PI * this.r * this.r; }
}
```

</details>

### Level 3: Inheritance

Create `Manager` that extends `Employee` and adds `teamSize`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class Manager extends Employee {
  constructor(name, salary, teamSize) {
    super(name, salary);
    this.teamSize = teamSize;
  }
}
```

</details>

### Level 4: Private Field

Create a `SafeBox` class with a private `#value` and `add()` and `get()`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
class SafeBox {
  #value = 0;
  add(amount) { this.#value += amount; }
  get() { return this.#value; }
}
```

</details>

### Level 5: Date

Log the current date in `YYYY-MM-DD` format.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const now = new Date();
const formatted = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, "0")}-${String(now.getDate()).padStart(2, "0")}`;
console.log(formatted);
```

</details>

### Level 6: Generator

Write a generator that yields the letters of a string one at a time.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function* letters(str) {
  for (const char of str) {
    yield char;
  }
}
```

</details>

### Level 7: Module

Write the export and import for a `calculateTax(amount)` named export.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
// utils.js
export function calculateTax(amount) { return amount * 0.15; }

// main.js
import { calculateTax } from "./utils.js";
```

</details>

---

## Mini Project: User Dashboard

### Goal

Build a small user dashboard that uses regex, classes, dates, and modules — all together.

### Requirements

1. Create a `User` class with `name`, `email`, and `joinDate`.
2. Add an `isValidEmail()` method that uses a regex.
3. Add a `getYearsSinceJoin()` method that uses `Date`.
4. Create an `Admin` class that extends `User` with an `isAdmin` property.
5. Create a module `utils.js` that exports `formatDate(date)`.
6. Create an HTML page that lists users and shows validation results.

### Starter Files

**utils.js**

```javascript
export function formatDate(date) {
  return date.toISOString().split("T")[0];
}

export function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

**main.js**

```javascript
import { formatDate, validateEmail } from "./utils.js";

class User {
  #email;

  constructor(name, email, joinDate) {
    this.name = name;
    this.#email = email;
    this.joinDate = new Date(joinDate);
  }

  get email() {
    return this.#email;
  }

  isValidEmail() {
    return validateEmail(this.#email);
  }

  getYearsSinceJoin() {
    const now = new Date();
    return now.getFullYear() - this.joinDate.getFullYear();
  }

  getInfo() {
    return `${this.name} joined on ${formatDate(this.joinDate)}. Email valid: ${this.isValidEmail()}`;
  }
}

class Admin extends User {
  constructor(name, email, joinDate) {
    super(name, email, joinDate);
    this.isAdmin = true;
  }

  getInfo() {
    return super.getInfo() + " (Admin)";
  }
}

const users = [
  new User("John", "john@example.com", "2020-06-15"),
  new User("Jane", "jane-at-test.com", "2021-03-10"),
  new Admin("Bob", "bob@admin.com", "2019-01-01")
];

const list = document.getElementById("userList");
users.forEach(user => {
  const li = document.createElement("li");
  li.textContent = user.getInfo();
  list.appendChild(li);
});
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>User Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    li { margin: 10px 0; }
  </style>
</head>
<body>
  <h1>User Dashboard</h1>
  <ul id="userList"></ul>
  <script type="module" src="main.js"></script>
</body>
</html>
```

### Questions to think about

1. Why did we use `type="module"` in the script tag?
2. What is the purpose of the private `#email` field?
3. How does `getYearsSinceJoin` use the `Date` object?

### Extension ideas

- Show `getYearsSinceJoin()` in each list item.
- Style invalid emails in red.
- Add a `Guest` class that extends `User` with limited permissions.

---

## Review Questions

Answer these before you finish.

1. What does the regex modifier 'i' do?
   - [ ] Global search
   - [ ] Case insensitive
   - [ ] Multiline
   - [ ] Case sensitive

2. What is a constructor function?
   - [ ] A function that constructs strings
   - [ ] A function used with 'new' to create objects
   - [ ] A function that deletes objects
   - [ ] A function that validates input

3. What does 'extends' do in ES6 classes?
   - [ ] Extends array length
   - [ ] Creates a subclass that inherits from a parent class
   - [ ] Extends string length
   - [ ] Copies object properties

4. What is encapsulation in OOP?
   - [ ] Making everything public
   - [ ] Hiding internal state and requiring interaction through methods
   - [ ] Creating multiple objects
   - [ ] Inheriting from parent classes

5. What does getFullYear() return?
   - [ ] Year as 2 digits
   - [ ] Year as 4 digits
   - [ ] Month
   - [ ] Day

6. What is a generator function?
   - [ ] A function that generates random numbers
   - [ ] A function that can pause and resume execution
   - [ ] A function that generates HTML
   - [ ] A function that creates objects

7. What does 'yield' do in a generator?
   - [ ] Stops the function permanently
   - [ ] Pauses execution and returns a value
   - [ ] Throws an error
   - [ ] Returns from the function

8. What is a named export?
   - [ ] Export with no name
   - [ ] Export with a specific name that must be imported with that name
   - [ ] Export that exports everything
   - [ ] Export that only works in Node.js

9. What is a default export?
   - [ ] The first export in a file
   - [ ] A single export that can be imported with any name
   - [ ] An export that is automatically imported
   - [ ] An export that cannot be imported

10. What does 'super()' do in a subclass?
    - [ ] Calls the parent class constructor
    - [ ] Creates a super object
    - [ ] Deletes the parent class
    - [ ] Exports the class

---

## Additional Resources

- [MDN: Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [Regex101](https://regex101.com) - Interactive regex tester
- [MDN: Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN: Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [MDN: Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [MDN: Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

---

<details>
<summary>Answer Key — Try everything first!</summary>

### Challenge 1.1

```javascript
console.log(/script/.test("javascript"));
```

### Challenge 1.2

```javascript
console.log(/\d/.test("Room 42"));
```

### Challenge 1.3

```javascript
console.log(/^\d{3}-/.test("123-456"));
```

### Challenge 2.1

```javascript
function Car(make, speed) {
  this.make = make;
  this.speed = speed;
}

Car.prototype.accelerate = function(amount) {
  this.speed += amount;
  return this.speed;
};

Car.prototype.brake = function(amount) {
  this.speed = Math.max(0, this.speed - amount);
  return this.speed;
};
```

### Challenge 3.1

```javascript
class Rectangle {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }
  area() { return this.w * this.h; }
}
```

### Challenge 3.2

```javascript
class Employee extends Person {
  constructor(name, salary) {
    super(name);
    this.salary = salary;
  }
  getInfo() {
    return `${this.name} earns $${this.salary}`;
  }
}
```

### Challenge 4.1

```javascript
class Counter {
  #count = 0;
  increment() { return ++this.#count; }
  getCount() { return this.#count; }
}
```

### Challenge 5.1

```javascript
const date = new Date("2025-12-25");
console.log(date.getFullYear(), date.getMonth() + 1, date.getDate());
```

### Challenge 5.2

```javascript
const start = new Date("2024-01-01");
const end = new Date("2024-12-31");
const days = (end - start) / (1000 * 60 * 60 * 24);
console.log(days);
```

### Challenge 6.1

```javascript
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}
```

### Challenge 7.1

```javascript
// utils.js
export function greet(name) { return `Hello, ${name}`; }
export default class User {}

// main.js
import User, { greet } from "./utils.js";
```

### Bug Hunt 1 — What was wrong and the fix

The `Dog` constructor does not call `super(name)` before using `this`, causing an error.

```javascript
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}
```

### Bug Hunt 2 — What was wrong and the fix

`return` ends the generator completely after the first value. You must use `yield` to pause and produce each value, and increment `n` before looping.

```javascript
function* count() {
  let n = 0;
  while (n < 3) {
    yield n;
    n++;
  }
}
```

### Individual Challenges Solutions

```javascript
// Level 1
console.log(/^J/.test("JavaScript"));

// Level 2
class Circle {
  constructor(r) { this.r = r; }
  area() { return Math.PI * this.r * this.r; }
}

// Level 3
class Manager extends Employee {
  constructor(name, salary, teamSize) {
    super(name, salary);
    this.teamSize = teamSize;
  }
}

// Level 4
class SafeBox {
  #value = 0;
  add(amount) { this.#value += amount; }
  get() { return this.#value; }
}

// Level 5
const now = new Date();
const formatted = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, "0")}-${String(now.getDate()).padStart(2, "0")}`;
console.log(formatted);

// Level 6
function* letters(str) {
  for (const char of str) {
    yield char;
  }
}

// Level 7
// utils.js
export function calculateTax(amount) { return amount * 0.15; }

// main.js
import { calculateTax } from "./utils.js";
```

### Group Challenge Answer Key

```javascript
// 1
const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailPattern.test("test@example.com"));

// 2
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
}

class Book extends Product {
  constructor(title, price, author) {
    super(title, price);
    this.author = author;
  }
}

// 3
const now = new Date();
const nextMonday = new Date(now);
nextMonday.setDate(now.getDate() + ((1 + 7 - now.getDay()) % 7));
console.log(nextMonday.toDateString());

// 4
function* evens() {
  for (let i = 0; i <= 10; i += 2) {
    yield i;
  }
}
```

</details>
