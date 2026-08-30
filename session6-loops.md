# Session 6: Loops — Active Learning Redesign

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

- "How many times will this loop run?"
- "What is the value of `i` in this iteration?"
- "Which loop should we use here?"
- "What happens if `i` never changes?"
- "Does this loop change the original array?"

---

## Part 0: Warm-Up — The Manual Counter (5 minutes)

### Problem

A teacher wants to print numbers `1` to `10` and their squares. A student writes this:

```javascript
console.log(1);
console.log(2);
console.log(3);
// ... all the way to 10
```

### Guess

Ask: "What if the teacher wants numbers `1` to `100`? Is this the best way?"

### Explain

Loops let us run the same code many times without writing it many times. The `for` loop has three parts: initialization, condition, and increment.

### Live Code

```javascript
for (let i = 1; i <= 10; i++) {
  console.log(i, i * i);
}
```

### Review

The loop ran 10 times. The variable `i` changed each time. This is the power of loops.

---

## Part 1: The For Loop

### 1.1 Basic Syntax

#### Problem

Count from `0` to `4` and show each number.

#### Live Code

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Iteration:", i);
}
```

#### Explain

```
for (initialization; condition; increment) { body }
```

- Initialization: `let i = 0` — happens once.
- Condition: `i < 5` — checked before each loop.
- Body: runs if condition is true.
- Increment: `i++` — runs after the body.

#### Challenge 1.1 — Count Up (individual, 3 minutes)

- **Requirement:** Print numbers `1` to `20` using a `for` loop.
- **Time limit:** 3 minutes

#### Challenge 1.2 — Count Down (individual, 3 minutes)

- **Requirement:** Print numbers `10` down to `1`.
- **Time limit:** 3 minutes
- **Hint:** `for (let i = 10; i >= 1; i--)`.

#### Challenge 1.3 — Custom Step (individual, 3 minutes)

- **Requirement:** Print all even numbers from `0` to `20`.
- **Time limit:** 3 minutes
- **Hint:** Use `i += 2`.

### 1.2 Summation and Accumulation

#### Problem

Add all numbers from `1` to `100`.

#### Live Code

```javascript
let sum = 0;
for (let i = 1; i <= 100; i++) {
  sum += i;
}
console.log("Sum:", sum); // 5050
```

#### Challenge 1.4 — Sum Even (individual, 4 minutes)

- **Requirement:** Calculate the sum of all even numbers from `1` to `100`.
- **Time limit:** 4 minutes
- **Hint:** Add an `if` inside the loop or use `i += 2`.

---

## Part 2: Looping Through Arrays, Strings, and Objects

### 2.1 Traditional For Loop Over an Array

#### Problem

Print each fruit in a list with its index.

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

for (let i = 0; i < fruits.length; i++) {
  console.log(`${i}: ${fruits[i]}`);
}
```

#### Challenge 2.1 — Sum Array (individual, 4 minutes)

- **Requirement:** Sum `let numbers = [10, 20, 30, 40, 50]` using a `for` loop.
- **Time limit:** 4 minutes

### 2.2 For...of

#### Problem

We only need the values, not the indexes.

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange"];

for (let fruit of fruits) {
  console.log(fruit);
}

// With strings
let text = "Hello";
for (let char of text) {
  console.log(char);
}

// With index and value
for (let [index, fruit] of fruits.entries()) {
  console.log(`${index}: ${fruit}`);
}
```

#### Challenge 2.2 — For...of Values (individual, 3 minutes)

- **Requirement:** Use `for...of` to print each item in `let colors = ["red", "green", "blue"]`.
- **Time limit:** 3 minutes

### 2.3 For...in

#### Problem

Loop through the keys of an object.

#### Live Code

```javascript
let user = {
  name: "John",
  age: 30,
  city: "New York"
};

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}
```

#### Explain

- `for...in` is for objects. Avoid using it on arrays because indexes are strings and order is not guaranteed.
- Use `for...of` for arrays and `for...in` for objects.

#### Challenge 2.3 — Object Printer (individual, 4 minutes)

- **Requirement:** Loop through `let book = { title: "JS", pages: 200, author: "Dev" }` and print each property.
- **Time limit:** 4 minutes

---

## Part 3: While and Do-While

### 3.1 While Loop

#### Problem

A user must enter the correct password, but we do not know how many attempts.

#### Live Code

```javascript
let attempts = 0;
let maxAttempts = 3;
let password = "";

while (password !== "secret" && attempts < maxAttempts) {
  attempts++;
  // In real life: password = prompt("Password:");
  password = attempts === 2 ? "secret" : "wrong";
  console.log(`Attempt ${attempts}: ${password}`);
}

if (password === "secret") {
  console.log("Access granted");
} else {
  console.log("Access denied");
}
```

#### Challenge 3.1 — While Count (individual, 4 minutes)

- **Requirement:** Use a `while` loop to print numbers `1` to `5`.
- **Time limit:** 4 minutes

### 3.2 Do-While Loop

#### Problem

A menu must show at least once before the user decides to exit.

#### Live Code

```javascript
let choice;
let count = 0;

do {
  count++;
  // Simulate user input
  choice = count === 3 ? 3 : Math.floor(Math.random() * 3) + 1;
  console.log(`Choice: ${choice}`);
  if (choice === 1) console.log("Viewing products...");
  if (choice === 2) console.log("Adding product...");
} while (choice !== 3);

console.log("Exiting menu...");
```

#### Challenge 3.2 — Do-Once (individual, 4 minutes)

- **Requirement:** Write a `do...while` that prints a number once even though the condition starts as `false`.
- **Time limit:** 4 minutes

#### Review

```javascript
let x = 10;
do {
  console.log("This runs once", x);
} while (x < 5);
```

---

## Part 4: Loop Control

### 4.1 break

#### Problem

Find the first number greater than `50` in an array. Stop once found.

#### Live Code

```javascript
let numbers = [10, 25, 30, 55, 60, 70];

for (let num of numbers) {
  if (num > 50) {
    console.log("First match:", num);
    break;
  }
}
```

#### Challenge 4.1 — Find First Even (individual, 3 minutes)

- **Requirement:** Loop through `[1, 3, 5, 7, 8, 9]` and print the first even number, then `break`.
- **Time limit:** 3 minutes

### 4.2 continue

#### Problem

Print numbers `1` to `20` but skip multiples of `3`.

#### Live Code

```javascript
for (let i = 1; i <= 20; i++) {
  if (i % 3 === 0) {
    continue;
  }
  console.log(i);
}
```

#### Challenge 4.2 — Skip Evens (individual, 3 minutes)

- **Requirement:** Print all odd numbers from `1` to `20` using `continue`.
- **Time limit:** 3 minutes

### 4.3 Labels

#### Problem

In nested loops, how can we stop the outer loop from inside the inner loop?

#### Live Code

```javascript
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      break outer;
    }
    console.log(`i: ${i}, j: ${j}`);
  }
}

// Continue outer
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      continue outer;
    }
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

#### Challenge 4.3 — Stop the Matrix (individual, 5 minutes)

- **Requirement:** Write a nested loop that prints pairs `i, j` for `0 <= i < 3` and `0 <= j < 3`, but stops the entire outer loop when `i === 1 && j === 1`.
- **Time limit:** 5 minutes

---

## Part 5: Nested Loops and Patterns

### 5.1 Basic Nested Loop

#### Problem

A grid of rows and columns. Each row has a set of columns.

#### Live Code

```javascript
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

#### Challenge 5.1 — Coordinate Pairs (individual, 4 minutes)

- **Requirement:** Print all pairs `(i, j)` where `i` is `0` to `2` and `j` is `0` to `2`.
- **Time limit:** 4 minutes

### 5.2 Matrix Traversal

#### Live Code

```javascript
let matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    console.log(`matrix[${i}][${j}] = ${matrix[i][j]}`);
  }
}
```

#### Challenge 5.2 — Sum Matrix (individual, 5 minutes)

- **Requirement:** Calculate the sum of all numbers in the matrix above.
- **Time limit:** 5 minutes

### 5.3 Pattern Printing

#### Live Code

```javascript
// Triangle
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += "* ";
  }
  console.log(line);
}

// Square
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= 5; j++) {
    line += "* ";
  }
  console.log(line);
}

// Number triangle
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += j + " ";
  }
  console.log(line);
}
```

#### Challenge 5.3 — Inverted Triangle (individual, 5 minutes)

- **Requirement:** Print this pattern:

```
* * * * *
* * * *
* * *
* *
*
```

- **Time limit:** 5 minutes
- **Hint:** The outer loop goes down from `5` to `1`.

### 5.4 Multiplication Table

#### Live Code

```javascript
for (let i = 1; i <= 5; i++) {
  let row = "";
  for (let j = 1; j <= 5; j++) {
    row += (i * j).toString().padStart(4, " ");
  }
  console.log(row);
}
```

#### Challenge 5.4 — Table 1 to 10 (individual, 6 minutes)

- **Requirement:** Generate a multiplication table from `1` to `10`.
- **Time limit:** 6 minutes

---

## Part 6: Practical Loop Programs

### 6.1 Prime Numbers

#### Live Code

```javascript
function isPrime(num) {
  if (num < 2) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}

for (let i = 1; i <= 100; i++) {
  if (isPrime(i)) {
    console.log(i);
  }
}
```

#### Challenge 6.1 — Is It Prime? (individual, 5 minutes)

- **Requirement:** Write a function `isPrime(num)` that returns `true` or `false`.
- **Time limit:** 5 minutes

### 6.2 Fibonacci Sequence

#### Live Code

```javascript
let fib = [0, 1];
for (let i = 2; i < 10; i++) {
  fib[i] = fib[i - 1] + fib[i - 2];
}
console.log(fib); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

#### Challenge 6.2 — First 15 Fibonacci (individual, 4 minutes)

- **Requirement:** Generate the first `15` Fibonacci numbers.
- **Time limit:** 4 minutes

### 6.3 Reverse an Array

#### Live Code

```javascript
let original = [1, 2, 3, 4, 5];
let reversed = [];

for (let i = original.length - 1; i >= 0; i--) {
  reversed.push(original[i]);
}

console.log("Original:", original);
console.log("Reversed:", reversed);
```

#### Challenge 6.3 — Reverse String (individual, 4 minutes)

- **Requirement:** Reverse `"hello"` using a loop.
- **Time limit:** 4 minutes
- **Hint:** Loop from the last index to `0` and build a new string.

### 6.4 Factorial

#### Live Code

```javascript
let num = 5;
let factorial = 1;

for (let i = 2; i <= num; i++) {
  factorial *= i;
}

console.log(`Factorial of ${num}:`, factorial); // 120
```

#### Challenge 6.4 — Factorial Function (individual, 4 minutes)

- **Requirement:** Write a function `factorial(n)`.
- **Time limit:** 4 minutes

### 6.5 Number Guessing Game

#### Live Code

```javascript
let target = Math.floor(Math.random() * 10) + 1;
let guess;
let attempts = 0;

while (true) {
  attempts++;
  // Simulate input
  guess = Math.floor(Math.random() * 10) + 1;
  console.log(`Attempt ${attempts}: ${guess}`);

  if (guess === target) {
    console.log(`Correct! Found ${target} in ${attempts} attempts.`);
    break;
  }

  if (guess < target) {
    console.log("Too low!");
  } else {
    console.log("Too high!");
  }
}
```

---

## Bug Hunt 1

### Problem

The following code has three deliberate bugs or surprises. Ask students to find them.

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = 0;

for (let i = 0; i <= numbers.length; i++) {
  sum += numbers[i];
}

console.log("Sum:", sum);

for (let i = 0; i < 10; i++); {
  console.log("Number:", i);
}

let count = 0;
while (count < 5) {
  console.log(count);
}
```

### Issues

1. `i <= numbers.length` tries to access `numbers[5]`, which is `undefined`. Should be `i < numbers.length`.
2. `for (let i = 0; i < 10; i++);` has a semicolon after the loop, so the block after it runs once with `i` being `10` (out of scope in some cases, or the variable `i` is accessible due to `var`? It uses `let`, so `i` is not accessible and an error may occur).
3. `while (count < 5)` does not increment `count`, causing an infinite loop.

### Fixed Version (for the instructor)

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = 0;

for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}
console.log("Sum:", sum);

for (let i = 0; i < 10; i++) {
  console.log("Number:", i);
}

let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}
```

### Points

1 point for each found bug.

---

## Part 7: When to Use Which Loop

### Live Code

```javascript
let numbers = [1, 2, 3, 4, 5];

// Use for when you need the index
console.log("For loop with index:");
for (let i = 0; i < numbers.length; i++) {
  console.log(`Index ${i}: ${numbers[i]}`);
}

// Use for...of when you only need values
console.log("\nFor...of values:");
for (let num of numbers) {
  console.log(num);
}

// Use forEach when you want to do something with each
console.log("\nforEach:");
numbers.forEach((num, index) => {
  console.log(`Processing ${num} at index ${index}`);
});

// Use while when you do not know how many iterations
console.log("\nWhile unknown:");
let total = 0;
let i = 1;
while (total < 50) {
  total += i;
  i++;
}
console.log("Total:", total);

// Use do...when when at least one run is required
console.log("\nDo-while:");
let x = 5;
do {
  console.log("At least once:", x);
  x++;
} while (x < 3);
```

---

## Bug Hunt 2

### Problem

The pattern printer has bugs. Ask students to find them.

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++); {
    line += "*";
  }
  console.log(line);
}
```

### Issues

1. The inner `for` loop has a semicolon `;` after it. The block `{ line += "*"; }` then runs once per outer iteration, not per inner iteration.
2. Because of the semicolon, `line` will only have one `*` each time.

### Fixed Version

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += "*";
  }
  console.log(line);
}
```

### Points

1 point for the semicolon bug, 1 point for explaining the output.

---

## Group Challenge: Pattern Race

- **Time:** 10 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team must print the requested pattern using nested loops.
- **Scoring:** 2 points for the first correct pattern, 1 point for each additional correct pattern within the time limit.

### Patterns

1. Right triangle of `*` with height `5`.
2. Square of `*` with side `5`.
3. Number triangle:

```
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

4. Inverted right triangle:

```
* * * * *
* * * *
* * *
* *
*
```

### Instructor Answer Key

```javascript
// 1
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) line += "*";
  console.log(line);
}

// 2
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= 5; j++) line += "*";
  console.log(line);
}

// 3
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) line += j + " ";
  console.log(line);
}

// 4
for (let i = 5; i >= 1; i--) {
  let line = "";
  for (let j = 1; j <= i; j++) line += "* ";
  console.log(line);
}
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Count by 3 (3 minutes)

- **Requirement:** Print numbers from `1` to `30`, skipping multiples of `3`.
- **Expected:** Use `continue`.

### Level 2: Sum of Odds (3 minutes)

- **Requirement:** Sum all odd numbers from `1` to `50`.
- **Expected:** Use a `for` loop.

### Level 3: Find Largest (4 minutes)

- **Requirement:** Find the largest number in `let numbers = [10, 5, 20, 8, 15]` using a loop.
- **Hint:** Track the largest value as you loop.

### Level 4: Count Occurrences (5 minutes)

- **Requirement:** Count how many times each item appears in `let items = ["apple", "banana", "apple", "orange", "banana", "apple"]`.
- **Hint:** Use an object as a counter.

### Level 5: Remove Duplicates (5 minutes)

- **Requirement:** Remove duplicates from `let duplicates = [1, 2, 3, 2, 4, 5, 3, 6]` using loops.
- **Hint:** Create a new array and use `includes` before pushing.

### Level 6: Number Guessing (6 minutes)

- **Requirement:** Generate a random number `1` to `10`. Use a `while` loop to guess until correct.
- **Hint:** Use `Math.random` and `break`.

---

## Mini Project: Pattern and Product Dashboard

### Time

25 minutes

### Goal

Combine `for`, `while`, nested loops, `break`, and `continue` into one small HTML page.

### Requirements for the Students

1. Create a page with two sections:
   - **Pattern Printer:** a dropdown to choose pattern (triangle, square, number triangle, multiplication table) and a button to render it.
   - **Number Guess:** a button that starts the game and shows the attempts.

2. Use `for` and nested `for` for patterns.

3. Use `while` for the number guessing game.

### Time Limit

25 minutes

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Loop Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    select, button, input { padding: 8px; margin: 5px; }
    pre { background: #f0f0f0; padding: 15px; border-radius: 4px; }
  </style>
</head>
<body>
  <h1>Loop Dashboard</h1>

  <div class="section">
    <h2>Pattern Printer</h2>
    <select id="pattern">
      <option value="triangle">Triangle</option>
      <option value="square">Square</option>
      <option value="numbers">Number Triangle</option>
      <option value="table">Multiplication Table</option>
    </select>
    <button onclick="printPattern()">Print</button>
    <pre id="patternOutput"></pre>
  </div>

  <div class="section">
    <h2>Number Guess Game</h2>
    <button onclick="playGame()">Play</button>
    <pre id="gameOutput"></pre>
  </div>

  <script>
    function printPattern() {
      let type = document.getElementById("pattern").value;
      let output = "";

      if (type === "triangle") {
        for (let i = 1; i <= 5; i++) {
          for (let j = 1; j <= i; j++) output += "* ";
          output += "\n";
        }
      } else if (type === "square") {
        for (let i = 1; i <= 5; i++) {
          for (let j = 1; j <= 5; j++) output += "* ";
          output += "\n";
        }
      } else if (type === "numbers") {
        for (let i = 1; i <= 5; i++) {
          for (let j = 1; j <= i; j++) output += j + " ";
          output += "\n";
        }
      } else if (type === "table") {
        for (let i = 1; i <= 5; i++) {
          for (let j = 1; j <= 5; j++) {
            output += (i * j).toString().padStart(4, " ");
          }
          output += "\n";
        }
      }

      document.getElementById("patternOutput").textContent = output;
    }

    function playGame() {
      let target = Math.floor(Math.random() * 10) + 1;
      let guess;
      let attempts = 0;
      let output = `Target: ${target}\n`;

      do {
        attempts++;
        guess = Math.floor(Math.random() * 10) + 1;
        output += `Attempt ${attempts}: ${guess}\n`;
      } while (guess !== target);

      output += `Correct in ${attempts} attempts!`;
      document.getElementById("gameOutput").textContent = output;
    }
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why does the pattern use nested `for` loops?"
- "What is the difference between `while` and `do...while` in the game?"
- "What happens if we forget `\n` inside the pattern loop?"

---

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.4

```javascript
let sum = 0;
for (let i = 2; i <= 100; i += 2) {
  sum += i;
}
console.log(sum);
```

### Challenge 2.1

```javascript
let numbers = [10, 20, 30, 40, 50];
let sum = 0;
for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}
console.log(sum);
```

### Challenge 2.2

```javascript
let colors = ["red", "green", "blue"];
for (let color of colors) {
  console.log(color);
}
```

### Challenge 2.3

```javascript
let book = { title: "JS", pages: 200, author: "Dev" };
for (let key in book) {
  console.log(`${key}: ${book[key]}`);
}
```

### Challenge 3.2

```javascript
let x = 10;
do {
  console.log("Run once", x);
} while (x < 5);
```

### Challenge 4.1

```javascript
let numbers = [1, 3, 5, 7, 8, 9];
for (let num of numbers) {
  if (num % 2 === 0) {
    console.log("First even:", num);
    break;
  }
}
```

### Challenge 4.2

```javascript
for (let i = 1; i <= 20; i++) {
  if (i % 2 === 0) continue;
  console.log(i);
}
```

### Challenge 5.2

```javascript
let matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];
let sum = 0;
for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    sum += matrix[i][j];
  }
}
console.log(sum); // 45
```

### Challenge 5.3

```javascript
for (let i = 5; i >= 1; i--) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += "* ";
  }
  console.log(line);
}
```

### Challenge 6.1

```javascript
function isPrime(num) {
  if (num < 2) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```

### Challenge 6.2

```javascript
let fib = [0, 1];
for (let i = 2; i < 15; i++) {
  fib[i] = fib[i - 1] + fib[i - 2];
}
console.log(fib);
```

### Challenge 6.3

```javascript
let text = "hello";
let reversed = "";
for (let i = text.length - 1; i >= 0; i--) {
  reversed += text[i];
}
console.log(reversed); // "olleh"
```

### Challenge 6.4

```javascript
function factorial(n) {
  let result = 1;
  for (let i = 2; i <= n; i++) {
    result *= i;
  }
  return result;
}
```

### Individual Challenges Solutions

```javascript
// Level 1
for (let i = 1; i <= 30; i++) {
  if (i % 3 === 0) continue;
  console.log(i);
}

// Level 2
let sum = 0;
for (let i = 1; i <= 50; i += 2) {
  sum += i;
}
console.log(sum);

// Level 3
let numbers = [10, 5, 20, 8, 15];
let largest = numbers[0];
for (let num of numbers) {
  if (num > largest) largest = num;
}
console.log(largest);

// Level 4
let items = ["apple", "banana", "apple", "orange", "banana", "apple"];
let counts = {};
for (let item of items) {
  counts[item] = (counts[item] || 0) + 1;
}
console.log(counts);

// Level 5
let duplicates = [1, 2, 3, 2, 4, 5, 3, 6];
let unique = [];
for (let num of duplicates) {
  if (!unique.includes(num)) unique.push(num);
}
console.log(unique);

// Level 6
let target = Math.floor(Math.random() * 10) + 1;
let guess;
let attempts = 0;
while (true) {
  attempts++;
  guess = Math.floor(Math.random() * 10) + 1;
  if (guess === target) {
    console.log(`Found ${target} in ${attempts} attempts`);
    break;
  }
}
```

---

## Review Questions

1. What is the correct syntax for a for loop?
   - [ ] for (i = 0; i < 5; i++)
   - [x] for (let i = 0; i < 5; i++)
   - [ ] for (i < 5; i++)
   - [ ] for (let i = 0; i < 5)

2. What does the `break` statement do?
   - [ ] Skips the current iteration
   - [x] Exits the loop immediately
   - [ ] Restarts the loop
   - [ ] Pauses the loop

3. What does the `continue` statement do?
   - [ ] Exits the loop
   - [x] Skips the current iteration
   - [ ] Restarts the loop
   - [ ] Pauses execution

4. Which loop is best for iterating over array values?
   - [ ] for loop
   - [ ] while loop
   - [x] for...of loop
   - [ ] do-while loop

5. What is the difference between while and do-while?
   - [ ] No difference
   - [x] do-while always executes at least once
   - [ ] while always executes at least once
   - [ ] do-while is faster

6. How do you exit a nested loop from the inner loop?
   - [ ] break
   - [ ] continue
   - [x] labeled break
   - [ ] return

7. What happens if the loop condition is initially false in a while loop?
   - [ ] Error
   - [ ] Loop executes once
   - [x] Loop doesn't execute
   - [ ] Infinite loop

8. Which loop would you use when you don't know the number of iterations?
   - [ ] for loop
   - [x] while loop
   - [ ] for...of loop
   - [ ] All of the above

9. What does `for...in` iterate over?
   - [ ] Array values
   - [x] Object properties
   - [ ] String characters
   - [ ] Map entries

10. How many times will this loop execute? `for (let i = 0; i < 5; i++)`
    - [ ] 4 times
    - [x] 5 times
    - [ ] 6 times
    - [ ] Infinite

---

## Additional Resources

- [MDN: for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)
- [MDN: while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while)
- [MDN: for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)
- [MDN: for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)
- [JavaScript.info: Loops](https://javascript.info/while-for)
- [JavaScript.info: for...of](https://javascript.info/for..of)
