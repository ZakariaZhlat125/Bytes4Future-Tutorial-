# Session 8: Scope, Arrow Functions & Higher-Order Functions — Active Learning Redesign

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

- "Where is this variable accessible?"
- "Will this arrow function have its own `this`?"
- "What does `map` do vs `filter` vs `reduce`?"
- "What should the callback return?"
- "Can we chain these HOFs?"

---

## Part 0: Warm-Up — The Missing Variable (5 minutes)

### Problem

A student writes this and gets an error:

```javascript
function sayHello() {
  let name = "John";
}
sayHello();
console.log(name);
```

### Guess

Ask: "Will this print `John`? Why or why not?"

### Explain

Variables are only visible in the scope where they are declared. A variable inside a function is not visible outside.

### Live Code

```javascript
let globalName = "Jane";

function sayHello() {
  let localName = "John";
  console.log("Inside:", globalName, localName);
}

sayHello();
console.log("Outside:", globalName);
// console.log(localName); // ReferenceError
```

### Review

`globalName` is visible everywhere. `localName` only exists inside the function.

---

## Part 1: Scope

### 1.1 Global Scope

#### Problem

A value needs to be used in many places.

#### Live Code

```javascript
let theme = "dark";

function showTheme() {
  console.log(theme);
}

showTheme();
console.log(theme);
```

#### Challenge 1.1 — Global or Local? (individual, 3 minutes)

- **Requirement:** Predict the output before running:

```javascript
let score = 100;

function updateScore() {
  score = 200;
}

updateScore();
console.log(score);
```

- **Time limit:** 3 minutes

### 1.2 Local and Block Scope

#### Live Code

```javascript
function example() {
  let localVar = "I am local";

  if (true) {
    let blockVar = "I am block-scoped";
    console.log(blockVar);
  }

  console.log(localVar);
  // console.log(blockVar); // ReferenceError
}

example();
// console.log(localVar); // ReferenceError
```

#### Challenge 1.2 — Block Scope (individual, 3 minutes)

- **Requirement:** Predict which line causes an error:

```javascript
if (true) {
  let x = 10;
  const y = 20;
  var z = 30;
}
console.log(x);
console.log(y);
console.log(z);
```

- **Time limit:** 3 minutes

### 1.3 Lexical Scope and Scope Chain

#### Live Code

```javascript
let globalVar = "global";

function level1() {
  let level1Var = "level 1";

  function level2() {
    let level2Var = "level 2";

    function level3() {
      console.log(globalVar);  // from global
      console.log(level1Var);  // from level1
      console.log(level2Var);  // from level2
    }

    level3();
  }

  level2();
}

level1();
```

#### Challenge 1.3 — Scope Chain (individual, 4 minutes)

- **Requirement:** Write three nested functions. The innermost should print a variable from the outermost.
- **Time limit:** 4 minutes

### 1.4 Variable Shadowing

#### Problem

Same variable name in different scopes.

#### Live Code

```javascript
let x = "global";

function example() {
  let x = "local";
  console.log(x); // "local"

  if (true) {
    let x = "block";
    console.log(x); // "block"
  }

  console.log(x); // "local"
}

example();
console.log(x); // "global"
```

#### Challenge 1.4 — Shadowing (individual, 4 minutes)

- **Requirement:** Predict the output, then run:

```javascript
let name = "World";

function greet() {
  let name = "Universe";
  console.log("Hello, " + name);
}

greet();
console.log("Hello, " + name);
```

- **Time limit:** 4 minutes

### 1.5 Hoisting

#### Problem

A function is called before it is defined.

#### Live Code

```javascript
// Function declarations are hoisted
sayHi();

function sayHi() {
  console.log("Hi!");
}

// var is hoisted (but undefined)
console.log(hoistedVar); // undefined
var hoistedVar = 5;

// let and const are not usable before declaration
// console.log(notHoisted); // ReferenceError
let notHoisted = 10;
```

#### Challenge 1.5 — Hoisting (individual, 3 minutes)

- **Requirement:** Predict the output:

```javascript
console.log(a);
var a = 1;

console.log(b);
let b = 2;
```

- **Time limit:** 3 minutes

---

## Part 2: Arrow Functions

### 2.1 Basic Syntax

#### Problem

Write a shorter function.

#### Live Code

```javascript
// Traditional
function add(a, b) {
  return a + b;
}

// Arrow
const add = (a, b) => a + b;

console.log(add(5, 3)); // 8
```

#### Challenge 2.1 — Convert to Arrow (individual, 3 minutes)

- **Requirement:** Convert `function multiply(a, b) { return a * b; }` to an arrow function.
- **Time limit:** 3 minutes

### 2.2 Variations

#### Live Code

```javascript
const sayHello = () => console.log("Hello!");
const square = x => x * x;
const add = (a, b) => a + b;

const process = (a, b) => {
  const sum = a + b;
  const product = a * b;
  return { sum, product };
};

console.log(square(4));      // 16
console.log(process(3, 5));  // { sum: 8, product: 15 }
```

#### Challenge 2.2 — Arrow Variations (individual, 4 minutes)

- **Requirement:** Write arrow functions for: no parameters returning `10`, single parameter returning `x * 3`, two parameters returning `a - b`, two parameters returning an object `{ a, b }`.
- **Time limit:** 4 minutes

### 2.3 Arrow and this

#### Problem

An arrow function inside an object does not get its own `this`.

#### Live Code

```javascript
const person = {
  name: "John",
  regularGreet: function() {
    console.log("Regular: " + this.name);
  },
  arrowGreet: () => {
    console.log("Arrow: " + this.name);
  }
};

person.regularGreet(); // "John"
person.arrowGreet();   // undefined

const person2 = {
  name: "Jane",
  delayedGreet: function() {
    setTimeout(() => {
      console.log("Delayed: " + this.name);
    }, 100);
  }
};

person2.delayedGreet(); // "Jane"
```

#### Challenge 2.3 — this in Arrow (individual, 4 minutes)

- **Requirement:** Predict the output of `person.arrowGreet()` and explain why.
- **Time limit:** 4 minutes

### 2.4 Returning Objects

#### Live Code

```javascript
const makeUser = (name, age) => ({ name, age });
console.log(makeUser("John", 30)); // { name: "John", age: 30 }

// Without parentheses, the braces become a block, not an object
const wrongUser = (name, age) => {
  name, age // no return, no object
};
console.log(wrongUser("John", 30)); // undefined
```

#### Challenge 2.4 — Return Object (individual, 3 minutes)

- **Requirement:** Write `const createBook = (title, pages) => ({ title, pages });` and call it.
- **Time limit:** 3 minutes

---

## Bug Hunt 1

### Problem

The following code has bugs. Ask students to find them.

```javascript
const person = {
  name: "John",
  greet: () => {
    console.log(`Hello, ${this.name}`);
  }
};

person.greet();

const makeUser = (name, age) => {
  name: name,
  age: age
};

console.log(makeUser("John", 30));

const numbers = [1, 2, 3];
const doubled = numbers.map(num => {
  num * 2;
});
console.log(doubled);
```

### Issues

1. `greet` arrow function loses `this`; use a regular function.
2. `makeUser` arrow function uses braces as a block but returns nothing. Wrap in parentheses or use `return`.
3. `map` callback does not return anything; `num * 2` is an expression without `return`.

### Fixed Version

```javascript
const person = {
  name: "John",
  greet: function() {
    console.log(`Hello, ${this.name}`);
  }
};

person.greet();

const makeUser = (name, age) => ({ name, age });
console.log(makeUser("John", 30)); // { name: "John", age: 30 }

const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6]
```

### Points

1 point per found bug.

---

## Part 3: Higher-Order Functions

### 3.1 forEach

#### Problem

Do something for every item in an array without a `for` loop.

#### Live Code

```javascript
const fruits = ["apple", "banana", "orange"];

fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});
```

#### Challenge 3.1 — forEach Sum (individual, 3 minutes)

- **Requirement:** Use `forEach` to sum `[10, 20, 30, 40]`.
- **Time limit:** 3 minutes

### 3.2 map

#### Problem

Create a new array from an existing one.

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

const users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 }
];
const names = users.map(user => user.name);
console.log(names); // ["John", "Jane"]
```

#### Challenge 3.2 — Map Objects (individual, 4 minutes)

- **Requirement:** Use `map` to get an array of `{ id, name }` from:

```javascript
const users = [
  { id: 1, name: "John", email: "john@example.com" },
  { id: 2, name: "Jane", email: "jane@example.com" }
];
```

- **Time limit:** 4 minutes

### 3.3 filter

#### Problem

Keep only some items from an array.

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const even = numbers.filter(num => num % 2 === 0);
const greaterThan5 = numbers.filter(num => num > 5);

console.log(even);
console.log(greaterThan5);
```

#### Challenge 3.3 — Filter Adults (individual, 4 minutes)

- **Requirement:** Use `filter` to keep users with `age >= 18`.
- **Time limit:** 4 minutes

### 3.4 reduce

#### Problem

Turn an array into one value (sum, product, object, etc.).

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((acc, num) => acc + num, 0);
const product = numbers.reduce((acc, num) => acc * num, 1);

console.log(sum);     // 15
console.log(product); // 120

const cart = [
  { item: "Book", price: 10, quantity: 2 },
  { item: "Pen", price: 2, quantity: 5 }
];

const total = cart.reduce((acc, item) => acc + item.price * item.quantity, 0);
console.log(total); // 30
```

#### Challenge 3.4 — Reduce to Object (individual, 5 minutes)

- **Requirement:** Use `reduce` to count how many times each fruit appears:

```javascript
const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];
```

- **Time limit:** 5 minutes
- **Hint:** `acc[fruit] = (acc[fruit] || 0) + 1`.

### 3.5 Chaining

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const result = numbers
  .filter(num => num % 2 === 0)
  .map(num => num * 2)
  .reduce((sum, num) => sum + num, 0);

console.log(result); // 60
```

#### Challenge 3.5 — Chain HOFs (individual, 5 minutes)

- **Requirement:** From `const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`, get the sum of the squares of even numbers.
- **Time limit:** 5 minutes

---

## Part 4: Practical HOF Data Processing

### 4.1 Processing Student Data

#### Live Code

```javascript
const students = [
  { name: "John", score: 85, grade: "A" },
  { name: "Jane", score: 92, grade: "A" },
  { name: "Bob", score: 78, grade: "C" },
  { name: "Alice", score: 65, grade: "D" },
  { name: "Charlie", score: 88, grade: "B" }
];

// Names of A students
const aStudents = students
  .filter(s => s.grade === "A")
  .map(s => s.name);

// Average score
const avg = students.reduce((sum, s) => sum + s.score, 0) / students.length;

// Group by grade
const byGrade = students.reduce((acc, s) => {
  if (!acc[s.grade]) acc[s.grade] = [];
  acc[s.grade].push(s.name);
  return acc;
}, {});

console.log(aStudents);
console.log(avg);
console.log(byGrade);
```

#### Challenge 4.1 — Top Student (individual, 5 minutes)

- **Requirement:** Use `reduce` to find the student with the highest score.
- **Time limit:** 5 minutes

### 4.2 Word Analysis

#### Live Code

```javascript
const text = "hello world hello javascript";
const words = text.toLowerCase().split(" ");

const uniqueWords = [...new Set(words)];
const wordCounts = words.reduce((acc, word) => {
  acc[word] = (acc[word] || 0) + 1;
  return acc;
}, {});

const longWords = words.filter(word => word.length > 4);

console.log(uniqueWords);
console.log(wordCounts);
console.log(longWords);
```

---

## Bug Hunt 2

### Problem

Find the bugs in this HOF code.

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(num => {
  num * 2;
});

const even = numbers.filter(num => {
  num % 2 === 0;
});

const sum = numbers.reduce((acc, num) => {
  acc + num;
}, 0);

console.log(doubled); // [undefined, undefined, undefined, undefined, undefined]
console.log(even);    // []
console.log(sum);     // 0
```

### Issues

1. `map` callback does not `return` the doubled value.
2. `filter` callback does not `return` the condition.
3. `reduce` callback does not `return` the accumulator.

### Fixed Version

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(num => num * 2);
const even = numbers.filter(num => num % 2 === 0);
const sum = numbers.reduce((acc, num) => acc + num, 0);

console.log(doubled); // [2, 4, 6, 8, 10]
console.log(even);    // [2, 4]
console.log(sum);     // 15
```

### Points

1 point per found bug.

---

## Group Challenge: HOF Pipeline Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team writes a single chained HOF pipeline for one of the tasks.
- **Scoring:** 2 points per correct pipeline. The first team to finish all four gets 2 bonus points.

### Tasks

1. From `const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`, get the sum of squares of odd numbers.
2. From the `students` array, get the average score of students with grade `B` or higher.
3. From `const words = ["hello", "world", "javascript", "code"]`, create an array of uppercase words with length > 4.
4. From the `cart` array, get an array of item names for items with total price > 15.

### Instructor Answer Key

```javascript
// 1
const sum = numbers
  .filter(n => n % 2 !== 0)
  .map(n => n * n)
  .reduce((s, n) => s + n, 0);

// 2
const goodStudents = students.filter(s => s.grade <= "B");
const avg = goodStudents.reduce((s, st) => s + st.score, 0) / goodStudents.length;

// 3
const longUpper = words
  .filter(w => w.length > 4)
  .map(w => w.toUpperCase());

// 4
const expensive = cart
  .filter(item => item.price * item.quantity > 15)
  .map(item => item.item);
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Arrow Basics (3 minutes)

- **Requirement:** Convert to arrow functions:

```javascript
function add(a, b) { return a + b; }
function square(x) { return x * x; }
function greet() { return "Hello"; }
```

### Level 2: Scope (3 minutes)

- **Requirement:** Predict the output:

```javascript
let x = 10;
function test() {
  let x = 5;
  console.log(x);
}
test();
console.log(x);
```

### Level 3: map (4 minutes)

- **Requirement:** Use `map` to create `[2, 4, 6, 8]` from `[1, 2, 3, 4]`.

### Level 4: filter (4 minutes)

- **Requirement:** Use `filter` to keep only numbers greater than `10`.

### Level 5: reduce (5 minutes)

- **Requirement:** Use `reduce` to find the product of `[1, 2, 3, 4, 5]`.

### Level 6: Chain (5 minutes)

- **Requirement:** Get the sum of even numbers multiplied by `3` from `[1, 2, 3, 4, 5, 6]`.

---

## Mini Project: Student Dashboard

### Time

25 minutes

### Goal

Combine scope, arrow functions, and HOFs in one HTML page.

### Requirements for the Students

1. Create an HTML page with a list of students:

```javascript
const students = [
  { name: "John", score: 85, grade: "A" },
  { name: "Jane", score: 92, grade: "A" },
  { name: "Bob", score: 78, grade: "C" },
  { name: "Alice", score: 65, grade: "D" },
  { name: "Charlie", score: 88, grade: "B" }
];
```

2. Add buttons:
   - **Average Score**
   - **A Students**
   - **Passing Students (>= 70)**
   - **Top Student**

3. Use arrow functions for callbacks.

4. Use `filter`, `map`, or `reduce` for each button.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    button { padding: 8px; margin: 5px; }
    .output { background: #f0f0f0; padding: 15px; border-radius: 4px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>Student Dashboard</h1>

  <div class="section">
    <button onclick="showAverage()">Average</button>
    <button onclick="showAStudents()">A Students</button>
    <button onclick="showPassing()">Passing</button>
    <button onclick="showTop()">Top Student</button>
    <div id="output" class="output"></div>
  </div>

  <script>
    const students = [
      { name: "John", score: 85, grade: "A" },
      { name: "Jane", score: 92, grade: "A" },
      { name: "Bob", score: 78, grade: "C" },
      { name: "Alice", score: 65, grade: "D" },
      { name: "Charlie", score: 88, grade: "B" }
    ];

    const showAverage = () => {
      const avg = students.reduce((sum, s) => sum + s.score, 0) / students.length;
      document.getElementById("output").textContent = `Average: ${avg}`;
    };

    const showAStudents = () => {
      const names = students
        .filter(s => s.grade === "A")
        .map(s => s.name)
        .join(", ");
      document.getElementById("output").textContent = `A Students: ${names}`;
    };

    const showPassing = () => {
      const names = students
        .filter(s => s.score >= 70)
        .map(s => s.name)
        .join(", ");
      document.getElementById("output").textContent = `Passing: ${names}`;
    };

    const showTop = () => {
      const top = students.reduce((max, s) => s.score > max.score ? s : max, students[0]);
      document.getElementById("output").textContent = `Top: ${top.name} (${top.score})`;
    };
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why do we use `reduce` for the average?"
- "What does `filter` return?"
- "What happens if we use an arrow function for an object method that needs `this`?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

Output: `200`. The function `updateScore` reassigns the global `score`.

### Challenge 1.2

`console.log(x)` and `console.log(y)` cause `ReferenceError`. `console.log(z)` prints `30` because `var` is not block-scoped.

### Challenge 1.4

```
Hello, Universe
Hello, World
```

The local `name` shadows the global one inside the function.

### Challenge 1.5

```
undefined
ReferenceError
```

### Challenge 2.1

```javascript
const multiply = (a, b) => a * b;
```

### Challenge 2.2

```javascript
const ten = () => 10;
const triple = x => x * 3;
const subtract = (a, b) => a - b;
const makePair = (a, b) => ({ a, b });
```

### Challenge 2.3

`person.arrowGreet()` prints `undefined` because arrow functions do not bind their own `this`.

### Challenge 3.1

```javascript
let sum = 0;
[10, 20, 30, 40].forEach(num => sum += num);
console.log(sum);
```

### Challenge 3.2

```javascript
const summaries = users.map(({ id, name }) => ({ id, name }));
console.log(summaries);
```

### Challenge 3.3

```javascript
const adults = users.filter(u => u.age >= 18);
```

### Challenge 3.4

```javascript
const counts = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(counts);
```

### Challenge 3.5

```javascript
const result = numbers
  .filter(n => n % 2 === 0)
  .map(n => n * n)
  .reduce((s, n) => s + n, 0);
```

### Challenge 4.1

```javascript
const topStudent = students.reduce((max, s) => s.score > max.score ? s : max, students[0]);
console.log(topStudent);
```

### Individual Challenges Solutions

```javascript
// Level 1
const add = (a, b) => a + b;
const square = x => x * x;
const greet = () => "Hello";

// Level 2
// 5, then 10

// Level 3
[1, 2, 3, 4].map(n => n * 2); // [2, 4, 6, 8]

// Level 4
numbers.filter(n => n > 10);

// Level 5
[1, 2, 3, 4, 5].reduce((p, n) => p * n, 1); // 120

// Level 6
[1, 2, 3, 4, 5, 6]
  .filter(n => n % 2 === 0)
  .map(n => n * 3)
  .reduce((s, n) => s + n, 0); // 36
```

</details>

---

## Review Questions

1. What is the difference between global and local scope?
   - [ ] No difference
   - [ ] Global scope is inside functions, local is outside
   - [x] Global scope is accessible everywhere, local is function-specific
   - [ ] Local scope is accessible everywhere, global is function-specific

2. What is lexical scope?
   - [ ] Scope based on where a function is called
   - [x] Scope based on where a function is defined
   - [ ] Scope that uses block scope only
   - [ ] Scope that's always global

3. Which is the correct arrow function syntax for one parameter?
   - [ ] (x) => x * 2
   - [x] x => x * 2
   - [ ] => x * 2
   - [ ] x -> x * 2

4. What is a higher-order function?
   - [ ] A function that's higher in the code
   - [x] A function that takes another function as argument or returns a function
   - [ ] A function with more parameters
   - [ ] A function that's called first

5. What does `map()` do?
   - [ ] Filters elements
   - [x] Transforms each element
   - [ ] Reduces to single value
   - [ ] Executes function for each element

6. What does `filter()` return?
   - [ ] A single value
   - [x] A new array with elements that pass the condition
   - [ ] The original array
   - [ ] Nothing

7. What does `reduce()` return?
   - [ ] A new array
   - [x] A single value
   - [ ] The original array
   - [ ] A boolean

8. What is the key difference between arrow functions and regular functions regarding `this`?
   - [ ] Arrow functions have their own `this`
   - [ ] Regular functions inherit `this` from surrounding scope
   - [x] Arrow functions inherit `this` from surrounding scope
   - [ ] No difference

9. Which HOF would you use to calculate the sum of an array?
   - [ ] map
   - [ ] filter
   - [x] reduce
   - [ ] forEach

10. What is block scope?
    - [ ] Scope within a function
    - [x] Scope within curly braces {}
    - [ ] Global scope
    - [ ] Scope within if statements only

---

## Additional Resources

- [MDN: Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN: Array.prototype.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [MDN: Array.prototype.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [MDN: Array.prototype.filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [MDN: Array.prototype.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- [JavaScript.info: Scope](https://javascript.info/closure)
- [JavaScript.info: Arrow Functions](https://javascript.info/arrow-functions-basics)
