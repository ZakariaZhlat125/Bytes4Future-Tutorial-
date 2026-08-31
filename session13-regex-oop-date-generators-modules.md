# Session 13: Regex, OOP, Date, Generators & Modules — Active Learning Redesign

## Session Plan for the Instructor

- **Total time:** approximately 90 to 120 minutes
- **Pacing rule:** never explain theory continuously for more than 15–20 minutes. After every concept, students must predict, write, or fix code.
- **Pedagogical pattern for every topic:**
  1. **Problem** — a realistic mini-situation
  2. **Guess** — ask: "What do you expect to happen?"
  3. **Explain** — the shortest rule that fixes the problem
  4. **Code** — live code written in front of the students, step by step
  5. **Challenge** — students solve a small task on their own or in groups
  6. **Review** — discuss the answer and the most common mistake

### Competition and Points

- 1 point per correct prediction in the "Guess" phase.
- 1–3 points per completed challenge, depending on difficulty.
- A "Bug Hunter" badge for each student who finds and fixes an intentional error.
- Keep a simple tally on a shared board or in the chat.

### Instructor Questions to Ask During the Session

- "What does this regex pattern match?"
- "What does `super()` do?"
- "How do we keep data private?"
- "What is the difference between `getDate` and `getDay`?"
- "What does `yield` do?"

---

## Part 0: Warm-Up — Many Concepts (5 minutes)

### Problem

A program needs to validate an email, create a reusable blueprint for users, work with dates, and split code into separate files.

### Guess

Ask: "Which JavaScript features help with each of these tasks?"

### Explain

Session 13 covers many advanced tools: regex for pattern matching, OOP for reusable structures, `Date` for time, generators for controlled iteration, and modules for organization.

### Live Code

```javascript
console.log(/hello/i.test("HELLO")); // true
console.log(new Date().getFullYear()); // current year
```

### Review

These tools are powerful and used together in real applications.

---

## Part 1: Regex

### 1.1 Creating and Testing Regex

#### Problem

Check if a string contains a pattern.

#### Live Code

```javascript
const pattern = /hello/;
const text = "hello world";

console.log(pattern.test(text));    // true
console.log(text.match(pattern));   // ["hello"]

const pattern2 = new RegExp("world");
console.log(pattern2.test(text));   // true
```

#### Challenge 1.1 — Test Pattern (individual, 3 minutes)

- **Requirement:** Use `test` to check if `"javascript"` contains the pattern `/script/`.
- **Time limit:** 3 minutes

### 1.2 Flags and Character Classes

#### Live Code

```javascript
console.log(/hello/i.test("HELLO"));       // true
console.log("hello hello".match(/hello/g)); // ["hello", "hello"]

console.log(/[aeiou]/.test("apple"));      // true
console.log(/\d/.test("abc123"));          // true
console.log(/\w/.test("_"));               // true
console.log(/\s/.test("a b"));             // true
```

#### Challenge 1.2 — Match Digits (individual, 3 minutes)

- **Requirement:** Write a regex to test if a string has at least one digit, and use it on `"Room 42"`.
- **Time limit:** 3 minutes

### 1.3 Quantifiers

#### Live Code

```javascript
console.log(/a*/.test(""));      // true (0 or more)
console.log(/a+/.test("a"));     // true (1 or more)
console.log(/a?/.test("b"));     // true (0 or 1)
console.log(/a{3}/.test("aaa")); // true (exactly 3)
console.log(/\d{2,4}/.test("12")); // true (2 to 4 digits)
```

#### Challenge 1.3 — Validate Phone Pattern (individual, 4 minutes)

- **Requirement:** Use a regex to test if a string starts with exactly 3 digits followed by a dash.
- **Time limit:** 4 minutes
- **Hint:** `^\d{3}-`

---

## Part 2: OOP with Constructor Functions and Prototypes

### 2.1 Constructor Functions

#### Problem

Create many objects with the same shape.

#### Live Code

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person("John", 30);
console.log(john.greet());
```

#### Challenge 2.1 — Constructor and Prototype (individual, 4 minutes)

- **Requirement:** Create a `Car(make, speed)` constructor and add `accelerate(amount)` and `brake(amount)` to its prototype.
- **Time limit:** 4 minutes

### 2.2 `hasOwnProperty` and Prototype Chain

#### Live Code

```javascript
console.log(john.hasOwnProperty("name"));   // true
console.log(john.hasOwnProperty("greet"));  // false
console.log(john.__proto__ === Person.prototype); // true
```

---

## Part 3: ES6 Classes

### 3.1 Basic Classes

#### Problem

A cleaner way to write constructors and methods.

#### Live Code

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
console.log(animal.speak());
```

#### Challenge 3.1 — Create a Class (individual, 3 minutes)

- **Requirement:** Write a `Rectangle` class with `width`, `height`, and `area()` method.
- **Time limit:** 3 minutes

### 3.2 Inheritance and `super`

#### Live Code

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
console.log(dog.speak());
```

#### Challenge 3.2 — Inheritance (individual, 5 minutes)

- **Requirement:** Create `Employee` extends `Person` (with `name`, `salary`) and add `getInfo()`.
- **Time limit:** 5 minutes

---

## Part 4: Encapsulation

### 4.1 Private Fields

#### Problem

Hide data so it cannot be changed directly.

#### Live Code

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
console.log(acc.getBalance()); // 150
// console.log(acc.#balance);   // SyntaxError
```

#### Challenge 4.1 — Private Field (individual, 4 minutes)

- **Requirement:** Create a `Counter` class with a private `#count` and `increment()` and `getCount()` methods.
- **Time limit:** 4 minutes

### 4.2 Closures for Privacy

#### Live Code

```javascript
function createCounter() {
  let count = 0;
  return {
    increment() { return ++count; },
    get() { return count; }
  };
}

const c = createCounter();
console.log(c.increment()); // 1
```

### 4.3 Getters and Setters

#### Live Code

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
console.log(t.fahrenheit);
t.fahrenheit = 212;
console.log(t.celsius);
```

---

## Bug Hunt 1

### Problem

Find the bug in this OOP code.

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

### Issues

1. The `Dog` constructor does not call `super(name)` before using `this`, causing an error.

### Fixed Version

```javascript
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}
```

### Points

1 point for finding the bug.

---

## Part 5: Date and Time

### 5.1 Creating and Reading Dates

#### Problem

Work with dates in JavaScript.

#### Live Code

```javascript
const now = new Date();
console.log(now.getFullYear());
console.log(now.getMonth());    // 0-11
console.log(now.getDate());
console.log(now.getDay());      // 0-6
```

#### Challenge 5.1 — Format Date (individual, 3 minutes)

- **Requirement:** Create a date for `2025-12-25` and log the year, month, and day.
- **Time limit:** 3 minutes

### 5.2 Date Arithmetic

#### Live Code

```javascript
const now = new Date();
const tomorrow = new Date(now.getTime() + 24 * 60 * 60 * 1000);
const diff = tomorrow - now;
console.log(diff / (1000 * 60 * 60)); // hours
```

#### Challenge 5.2 — Days Between (individual, 4 minutes)

- **Requirement:** Calculate how many days are between `2024-01-01` and `2024-12-31`.
- **Time limit:** 4 minutes

---

## Part 6: Generators

### 6.1 Basic Generator

#### Problem

Create a function that returns multiple values one at a time.

#### Live Code

```javascript
function* count() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = count();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().done);  // false
```

#### Challenge 6.1 — Range Generator (individual, 4 minutes)

- **Requirement:** Write a generator `range(start, end)` that yields numbers from `start` to `end`.
- **Time limit:** 4 minutes

---

## Bug Hunt 2

### Problem

Find the bug in this generator code.

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

### Issues

1. `return` ends the generator completely. Should use `yield` to pause and return a value.

### Fixed Version

```javascript
function* count() {
  let n = 0;
  while (n < 3) {
    yield n;
    n++;
  }
}
```

### Points

1 point for finding the bug.

---

## Part 7: Modules

### 7.1 Named and Default Exports

#### Problem

Split code into reusable files.

#### Live Code

```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }

// main.js
import multiply, { PI, add } from "./math.js";
console.log(add(2, 3));
console.log(PI);
console.log(multiply(2, 3));
```

#### Challenge 7.1 — Write a Module (individual, 4 minutes)

- **Requirement:** Write a module that exports `greet(name)` as a named export and `User` class as a default export. Then write the import statement.
- **Time limit:** 4 minutes

### 7.2 Import All

#### Live Code

```javascript
import * as math from "./math.js";
console.log(math.add(1, 2));
```

---

## Group Challenge: Advanced JavaScript Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team completes one task.
- **Scoring:** 2 points per correct solution.

### Tasks

1. Write a regex to test if a string is an email (simple: `something@something.something`).
2. Create a `Book` class that extends `Product` (with `title` and `price`).
3. Create a `Date` for next Monday and log it.
4. Write a generator that yields even numbers from 0 to 10.

### Instructor Answer Key

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

---

## Individual Challenges — Progressive Difficulty

### Level 1: Regex (3 minutes)

- **Requirement:** Test if `"JavaScript"` starts with `J` (case-sensitive).

### Level 2: Class (3 minutes)

- **Requirement:** Create a `Circle` class with `radius` and `area()`.

### Level 3: Inheritance (4 minutes)

- **Requirement:** Create `Manager` that extends `Employee` and adds `teamSize`.

### Level 4: Private Field (4 minutes)

- **Requirement:** Create a `SafeBox` class with a private `#value` and `add()` and `get()`.

### Level 5: Date (4 minutes)

- **Requirement:** Log the current date in `YYYY-MM-DD` format.

### Level 6: Generator (5 minutes)

- **Requirement:** Write a generator that yields the letters of a string one at a time.

### Level 7: Module (4 minutes)

- **Requirement:** Write the export and import for a `calculateTax(amount)` named export.

---

## Mini Project: User Dashboard

### Time

25 minutes

### Goal

Build a small user dashboard that uses regex, classes, dates, and modules.

### Requirements for the Students

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

### Review Questions for the Mini Project

- "Why did we use `type="module"` in the script tag?"
- "What is the purpose of the private `#email` field?"
- "How does `getYearsSinceJoin` use the `Date` object?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

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

</details>

---

## Review Questions

1. What does the regex modifier 'i' do?
   - [ ] Global search
   - [x] Case insensitive
   - [ ] Multiline
   - [ ] Case sensitive

2. What is a constructor function?
   - [ ] A function that constructs strings
   - [x] A function used with 'new' to create objects
   - [ ] A function that deletes objects
   - [ ] A function that validates input

3. What does 'extends' do in ES6 classes?
   - [ ] Extends array length
   - [x] Creates a subclass that inherits from a parent class
   - [ ] Extends string length
   - [ ] Copies object properties

4. What is encapsulation in OOP?
   - [ ] Making everything public
   - [x] Hiding internal state and requiring interaction through methods
   - [ ] Creating multiple objects
   - [ ] Inheriting from parent classes

5. What does getFullYear() return?
   - [ ] Year as 2 digits
   - [x] Year as 4 digits
   - [ ] Month
   - [ ] Day

6. What is a generator function?
   - [ ] A function that generates random numbers
   - [x] A function that can pause and resume execution
   - [ ] A function that generates HTML
   - [ ] A function that creates objects

7. What does 'yield' do in a generator?
   - [ ] Stops the function permanently
   - [x] Pauses execution and returns a value
   - [ ] Throws an error
   - [ ] Returns from the function

8. What is a named export?
   - [ ] Export with no name
   - [x] Export with a specific name that must be imported with that name
   - [ ] Export that exports everything
   - [ ] Export that only works in Node.js

9. What is a default export?
   - [ ] The first export in a file
   - [x] A single export that can be imported with any name
   - [ ] An export that is automatically imported
   - [ ] An export that cannot be imported

10. What does 'super()' do in a subclass?
    - [x] Calls the parent class constructor
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
