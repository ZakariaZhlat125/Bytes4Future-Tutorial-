# Session 6: Loops — Student Active-Learning Lab

Welcome to this session. The goal today is not to copy code — it is to **predict, experiment, fix, and build**. Try every problem before you look at the answer.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Cover the code if you have to.
3. **Write your prediction** in your notebook or in a comment.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the mini-challenge before looking at the answer key.**

---

## Part 0: Warm-Up — The Manual Counter

### The problem

A teacher wants to print numbers `1` to `10` and their squares. A student writes this:

```javascript
console.log(1);
console.log(2);
console.log(3);
// ... all the way to 10
```

It works, but it is slow.

### 🤔 Think

What if the teacher wants numbers `1` to `100`? How many `console.log` lines would that be?

Write your answer here:

```text
Number of console.log lines needed for 1 to 100: __________
```

### 🔧 Try

Type the `1` to `10` version the long way if you want. Notice the pattern.

### 🧠 Discover

A **loop** runs the same block of code many times. A `for` loop has three parts: **start**, **condition**, and **change**.

```javascript
for (let i = 1; i <= 10; i++) {
  console.log(i, i * i);
}
```

### ✅ Check

Run it. Did you expect `i * i` to give the square each time?

### 🧪 Experiment

Change only `i <= 10` to `i <= 20`. What happens? Change `i++` to `i += 2`. What happens?

---

## Part 1: The `for` Loop

### 1.1 Basic Syntax

### The problem

Count from `0` to `4` and show each number.

### 🔮 Predict

Before you run it, answer:

- How many lines will print?
- What is the first number?
- What is the last number?

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Iteration:", i);
}
```

### ✅ Result

Run the code. Was your prediction right?

### 🧠 Why?

The `for` loop is built like this:

```javascript
for (initialization; condition; increment) {
  // body
}
```

- **Initialization:** `let i = 0` — runs once at the start.
- **Condition:** `i < 5` — checked before every loop.
- **Body:** runs if the condition is `true`.
- **Increment:** `i++` — runs after the body.

### 🧪 Experiment

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Iteration:", i);
}
```

1. Change `i < 5` to `i <= 5`. What is the new last value?
2. Change `let i = 0` to `let i = 5`. Does the loop still run? Why or why not?

---

### Challenge 1.1 — Count Up

Print numbers `1` to `20` using a `for` loop.

```javascript
// your code here
```

Set a timer for 3 minutes. Do not peek at the answer key.

### Challenge 1.2 — Count Down

Print numbers `10` down to `1`.

```javascript
// your code here
```

**Hint:** What should the condition and the update step be? Try `for (let i = 10; i >= 1; i--)` if you get stuck.

### Challenge 1.3 — Custom Step

Print all even numbers from `0` to `20`.

```javascript
// your code here
```

**Hint:** Use `i += 2`.

---

### 1.2 Summation and Accumulation

### The problem

Add all numbers from `1` to `100`.

### 🤔 Think

If you had to do this with paper and pencil, you would keep a running total. In JavaScript, we use a variable to keep that total.

### 🔮 Predict

What number do you think `sum` will be at the end? (Hint: the famous answer is `5050`.)

```javascript
let sum = 0;
for (let i = 1; i <= 100; i++) {
  sum += i;
}
console.log("Sum:", sum);
```

### ✅ Result

Run the code.

### 🧠 Why?

`sum += i` means `sum = sum + i`. Each time the loop runs, the current `i` is added to the running total.

### 🧪 Experiment

- Change `sum = 0` to `sum = 10`. What is the new result?
- Change `i <= 100` to `i <= 10`. What happens?

### Challenge 1.4 — Sum Even

Calculate the sum of all even numbers from `1` to `100`.

```javascript
// your code here
```

**Hint:** Add an `if` inside the loop, or use `i += 2`.

---

## Part 2: Looping Through Arrays, Strings, and Objects

### 2.1 Traditional `for` Loop Over an Array

### The problem

Print each fruit in a list with its index.

### 🔮 Predict

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

for (let i = 0; i < fruits.length; i++) {
  console.log(`${i}: ${fruits[i]}`);
}
```

What will the first line print? What will the last line print?

### ✅ Result

Run it.

### 🧠 Why?

`fruits.length` gives the number of items. We use the index `i` to read each value: `fruits[i]`.

### 🧪 Experiment

- Change `i < fruits.length` to `i <= fruits.length`. What happens to the last line?
- What does `fruits[4]` return when there are only four items?

### Challenge 2.1 — Sum Array

Sum the values in `let numbers = [10, 20, 30, 40, 50]` using a `for` loop.

```javascript
// your code here
```

---

### 2.2 `for...of`

### The problem

Sometimes we only need the values, not the indexes.

### 🔮 Predict

```javascript
let fruits = ["apple", "banana", "orange"];

for (let fruit of fruits) {
  console.log(fruit);
}
```

Will the output include numbers? Will it include `apple`, `banana`, `orange`? In what order?

### ✅ Result

Run it.

### 🧠 Why?

`for...of` reads each **value** in an array, one by one. It is perfect when you do not need the index.

### Try it with a string

```javascript
let text = "Hello";
for (let char of text) {
  console.log(char);
}
```

What will this print? How many lines?

### Get both index and value

```javascript
for (let [index, fruit] of fruits.entries()) {
  console.log(`${index}: ${fruit}`);
}
```

### Challenge 2.2 — `for...of` Values

Use `for...of` to print each item in `let colors = ["red", "green", "blue"]`.

```javascript
// your code here
```

---

### 2.3 `for...in`

### The problem

Loop through the keys of an object.

### 🔮 Predict

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

What will `key` be on each line? Will the order be the same as the object?

### ✅ Result

Run it.

### 🧠 Why?

- `for...in` is for **objects**.
- It gives you the **keys** (property names).
- `user[key]` gives you the value.
- Avoid using `for...in` on arrays — indexes are strings and order is not guaranteed.

### Challenge 2.3 — Object Printer

Loop through `let book = { title: "JS", pages: 200, author: "Dev" }` and print each property.

```javascript
// your code here
```

---

## Part 3: `while` and `do...while`

### 3.1 `while` Loop

### The problem

A user must enter the correct password, but we do not know how many attempts it will take.

### 🤔 Think

Can a `for` loop handle something where we do not know the number of tries? Why not?

### 🔮 Predict

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

How many times will the `while` loop run? What will the last message be?

### ✅ Result

Run it.

### 🧠 Why?

A `while` loop checks a condition **before** each run. It is useful when the number of iterations is unknown.

### Challenge 3.1 — `while` Count

Use a `while` loop to print numbers `1` to `5`.

```javascript
// your code here
```

---

### 3.2 `do...while` Loop

### The problem

A menu must show at least once before the user decides to exit.

### 🔮 Predict

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

Will this always print at least one menu? Could it print three menus?

### ✅ Result

Run it several times.

### 🧠 Why?

A `do...while` checks the condition **after** the first run. It always runs at least once.

### Challenge 3.2 — Do-Once

Write a `do...while` that prints a number once even though the condition starts as `false`.

```javascript
// your code here
```

### 🧪 Experiment

```javascript
let x = 10;
do {
  console.log("This runs once", x);
} while (x < 5);
```

What would happen if this was a `while` loop instead of a `do...while`?

---

## Part 4: Loop Control

### 4.1 `break`

### The problem

Find the first number greater than `50` in an array. Stop once found.

### 🔮 Predict

```javascript
let numbers = [10, 25, 30, 55, 60, 70];

for (let num of numbers) {
  if (num > 50) {
    console.log("First match:", num);
    break;
  }
}
```

Will the loop check `60`? Will it check `70`? Will it print more than one match?

### ✅ Result

Run it.

### 🧠 Why?

`break` exits the loop immediately. Use it when you have found what you need.

### Challenge 4.1 — Find First Even

Loop through `[1, 3, 5, 7, 8, 9]` and print the first even number, then `break`.

```javascript
// your code here
```

---

### 4.2 `continue`

### The problem

Print numbers `1` to `20` but skip multiples of `3`.

### 🔮 Predict

```javascript
for (let i = 1; i <= 20; i++) {
  if (i % 3 === 0) {
    continue;
  }
  console.log(i);
}
```

Will `3` print? Will `6` print? Will `20` print?

### ✅ Result

Run it.

### 🧠 Why?

`continue` skips the rest of the current iteration and goes to the next one.

### Challenge 4.2 — Skip Evens

Print all odd numbers from `1` to `20` using `continue`.

```javascript
// your code here
```

---

### 4.3 Labels

### The problem

In nested loops, how can we stop the outer loop from inside the inner loop?

### 🔮 Predict

```javascript
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      break outer;
    }
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

What will the last line print? Will `i: 2` appear?

### ✅ Result

Run it.

### 🧪 Experiment

Change `break outer;` to `continue outer;` in the code below. What changes?

```javascript
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      continue outer;
    }
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

### Challenge 4.3 — Stop the Matrix

Write a nested loop that prints pairs `i, j` for `0 <= i < 3` and `0 <= j < 3`, but stops the entire outer loop when `i === 1 && j === 1`.

```javascript
// your code here
```

---

## Part 5: Nested Loops and Patterns

### 5.1 Basic Nested Loop

### The problem

A grid has rows and columns. Each row has a set of columns.

### 🔮 Predict

```javascript
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

How many total lines will print? What is the order?

### ✅ Result

Run it.

### 🧠 Why?

The inner loop runs fully for every single step of the outer loop.

### 🧪 Experiment

Change `j < 3` to `j < i`. What happens?

### Challenge 5.1 — Coordinate Pairs

Print all pairs `(i, j)` where `i` is `0` to `2` and `j` is `0` to `2`.

```javascript
// your code here
```

---

### 5.2 Matrix Traversal

### The problem

Loop through every value in a 2D array (a matrix).

### 🔮 Predict

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

What is the first value printed? What is the last?

### ✅ Result

Run it.

### 🧠 Why?

`matrix[i]` is the inner array. `matrix[i][j]` is one item inside that inner array.

### Challenge 5.2 — Sum Matrix

Calculate the sum of all numbers in the matrix above.

```javascript
// your code here
```

---

### 5.3 Pattern Printing

### The problem

Print shapes using stars and numbers.

### Triangle

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += "* ";
  }
  console.log(line);
}
```

### 🔮 Predict

How many stars will the last line have? How many total lines?

### Square

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= 5; j++) {
    line += "* ";
  }
  console.log(line);
}
```

### Number triangle

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += j + " ";
  }
  console.log(line);
}
```

### 🧪 Experiment

Change `line += "* ";` to `line += i + " ";` in the triangle. What shape of numbers do you see?

### Challenge 5.3 — Inverted Triangle

Print this pattern:

```text
* * * * *
* * * *
* * *
* *
*
```

**Hint:** The outer loop should go down from `5` to `1`.

```javascript
// your code here
```

---

### 5.4 Multiplication Table

### The problem

Print a 5 by 5 multiplication table.

### 🔮 Predict

```javascript
for (let i = 1; i <= 5; i++) {
  let row = "";
  for (let j = 1; j <= 5; j++) {
    row += (i * j).toString().padStart(4, " ");
  }
  console.log(row);
}
```

What number will appear at the bottom-right corner? What does `padStart(4, " ")` do?

### ✅ Result

Run it.

### 🧠 Why?

The outer loop picks the row. The inner loop picks the column. `padStart(4, " ")` adds spaces so the numbers line up.

### 🧪 Experiment

Change `i <= 5` and `j <= 5` to `10`. Does the table still line up? Try `padStart(5, " ")`.

### Challenge 5.4 — Table 1 to 10

Generate a multiplication table from `1` to `10`.

```javascript
// your code here
```

---

## Part 6: Practical Loop Programs

### 6.1 Prime Numbers

### The problem

A prime number has no divisors except `1` and itself. How can we test for that?

### Try this function

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

### 🔮 Predict

Will `1` be printed? Will `2` be printed? Why do we stop at `Math.sqrt(num)`?

### 🧪 Experiment

- Remove `if (num < 2) return false;`. Does the function still work?
- Change `i <= Math.sqrt(num)` to `i < num`. Is it slower or faster? Does the answer change?

### Challenge 6.1 — Is It Prime?

Write a function `isPrime(num)` that returns `true` or `false`.

```javascript
// your code here
```

---

### 6.2 Fibonacci Sequence

### The problem

Each Fibonacci number is the sum of the two before it: `0, 1, 1, 2, 3, 5, 8, ...`

### 🔮 Predict

```javascript
let fib = [0, 1];
for (let i = 2; i < 10; i++) {
  fib[i] = fib[i - 1] + fib[i - 2];
}
console.log(fib);
```

What will the 10th value be? What is the 6th value?

### ✅ Result

Run it. The array should be `[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]`.

### 🧪 Experiment

Change `i < 10` to `i < 15`. How many numbers do you get?

### Challenge 6.2 — First 15 Fibonacci

Generate the first 15 Fibonacci numbers.

```javascript
// your code here
```

---

### 6.3 Reverse an Array

### The problem

Build a new array that is the reverse of the original.

### 🔮 Predict

```javascript
let original = [1, 2, 3, 4, 5];
let reversed = [];

for (let i = original.length - 1; i >= 0; i--) {
  reversed.push(original[i]);
}

console.log("Original:", original);
console.log("Reversed:", reversed);
```

What will `reversed[0]` be? What will `reversed[4]` be?

### ✅ Result

Run it.

### 🧠 Why?

We start at the last index and count backwards. `reversed.push` adds the value to the end of the new array.

### Challenge 6.3 — Reverse a String

Reverse `"hello"` using a loop.

```javascript
// your code here
```

**Hint:** Loop from the last index to `0` and build a new string.

---

### 6.4 Factorial

### The problem

`5! = 5 * 4 * 3 * 2 * 1 = 120`.

### 🔮 Predict

```javascript
let num = 5;
let factorial = 1;

for (let i = 2; i <= num; i++) {
  factorial *= i;
}

console.log(`Factorial of ${num}:`, factorial);
```

What is `factorial of 6`? Change `num` and predict before running.

### ✅ Result

Run it. `5!` is `120`.

### Challenge 6.4 — Factorial Function

Write a function `factorial(n)`.

```javascript
// your code here
```

---

### 6.5 Number Guessing Game

### The problem

The computer picks a random number. A loop guesses until it gets it right.

### 🔮 Predict

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

Will this loop always end? What does `break` do here? Could it take 100 attempts?

### ✅ Result

Run it a few times.

### 🧪 Experiment

Change `Math.random() * 10` to `Math.random() * 100` for the guess range. How does the number of attempts change?

---

## Part 7: When to Use Which Loop

### The goal

There is more than one way to loop. The right loop depends on the job.

### Compare these examples

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

// Use do...while when at least one run is required
console.log("\nDo-while:");
let x = 5;
do {
  console.log("At least once:", x);
  x++;
} while (x < 3);
```

### 🤔 Think

For each of these jobs, which loop would you pick? Write your answer.

1. Print every user name in an array: __________
2. Print the index and value of an array: __________
3. Keep asking for a password until it is correct: __________
4. Show a menu at least once before asking to exit: __________

---

## Bug Hunt 1

### The mission

The following code has three deliberate bugs or surprises. Find and fix them.

Do not run it yet. Read the whole code and write down what you think is wrong.

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

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________
3. _______________________________________________________________

### ✅ Fixed version

Write your fixed version below, then test it.

```javascript
// your fixed version here
```

---

## Bug Hunt 2

### The mission

The pattern printer has bugs. Find them and fix them.

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++); {
    line += "*";
  }
  console.log(line);
}
```

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

```javascript
// your fixed version here
```

---

## Group Challenge: Pattern Race

If you are working with a group, split into teams of 2 or 3.

Each team must print the requested pattern using nested loops.

### Patterns to build

1. Right triangle of `*` with height `5`.
2. Square of `*` with side `5`.
3. Number triangle:

```text
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

4. Inverted right triangle:

```text
* * * * *
* * * *
* * *
* *
*
```

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Count by 3

Print numbers from `1` to `30`, skipping multiples of `3`.

```javascript
// your code here
```

### Level 2: Sum of Odds

Sum all odd numbers from `1` to `50`.

```javascript
// your code here
```

### Level 3: Find Largest

Find the largest number in `let numbers = [10, 5, 20, 8, 15]` using a loop.

```javascript
// your code here
```

### Level 4: Count Occurrences

Count how many times each item appears in `let items = ["apple", "banana", "apple", "orange", "banana", "apple"]`.

**Hint:** Use an object as a counter.

```javascript
// your code here
```

### Level 5: Remove Duplicates

Remove duplicates from `let duplicates = [1, 2, 3, 2, 4, 5, 3, 6]` using loops.

**Hint:** Create a new array and use `includes` before pushing.

```javascript
// your code here
```

### Level 6: Number Guessing

Generate a random number `1` to `10`. Use a `while` loop to guess until correct.

**Hint:** Use `Math.random` and `break`.

```javascript
// your code here
```

---

## Mini Project: Pattern and Product Dashboard

### Time

25 minutes

### Goal

Combine `for`, `while`, nested loops, `break`, and `continue` into one small HTML page.

### Requirements

1. Create a page with two sections:
   - **Pattern Printer:** a dropdown to choose pattern (triangle, square, number triangle, multiplication table) and a button to render it.
   - **Number Guess:** a button that starts the game and shows the attempts.

2. Use `for` and nested `for` for patterns.

3. Use `while` for the number guessing game.

### Starter HTML

```html copy
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

### Extension ideas

- Add an option to choose the size of the pattern.
- Change the guessing game to give "too low / too high" hints.
- Add a `continue` or `break` challenge inside one of the patterns.

---

## Review Questions

Answer these before you finish.

1. What is the correct syntax for a `for` loop?
   - [ ] `for (i = 0; i < 5; i++)`
   - [ ] `for (let i = 0; i < 5; i++)`
   - [ ] `for (i < 5; i++)`
   - [ ] `for (let i = 0; i < 5)`

2. What does the `break` statement do?
   - [ ] Skips the current iteration
   - [ ] Exits the loop immediately
   - [ ] Restarts the loop
   - [ ] Pauses the loop

3. What does the `continue` statement do?
   - [ ] Exits the loop
   - [ ] Skips the current iteration
   - [ ] Restarts the loop
   - [ ] Pauses execution

4. Which loop is best for iterating over array values?
   - [ ] `for` loop
   - [ ] `while` loop
   - [ ] `for...of` loop
   - [ ] `do-while` loop

5. What is the difference between `while` and `do-while`?
   - [ ] No difference
   - [ ] `do-while` always executes at least once
   - [ ] `while` always executes at least once
   - [ ] `do-while` is faster

6. How do you exit a nested loop from the inner loop?
   - [ ] `break`
   - [ ] `continue`
   - [ ] labeled `break`
   - [ ] `return`

7. What happens if the loop condition is initially `false` in a `while` loop?
   - [ ] Error
   - [ ] Loop executes once
   - [ ] Loop doesn't execute
   - [ ] Infinite loop

8. Which loop would you use when you don't know the number of iterations?
   - [ ] `for` loop
   - [ ] `while` loop
   - [ ] `for...of` loop
   - [ ] All of the above

9. What does `for...in` iterate over?
   - [ ] Array values
   - [ ] Object properties
   - [ ] String characters
   - [ ] Map entries

10. How many times will this loop execute? `for (let i = 0; i < 5; i++)`
    - [ ] 4 times
    - [ ] 5 times
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

---

<details>
<summary>Answer Key — Try everything first!</summary>

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

### Bug Hunt 1 Fixed Version

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

### Bug Hunt 2 Fixed Version

```javascript
for (let i = 1; i <= 5; i++) {
  let line = "";
  for (let j = 1; j <= i; j++) {
    line += "*";
  }
  console.log(line);
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

</details>
