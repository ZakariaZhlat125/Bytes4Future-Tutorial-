# Session 7: Functions (Part 1) — Active Learning Redesign

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

- "What does this function return?"
- "What happens if we call the function without an argument?"
- "Should this task be a new function?"
- "Where should we use `return`?"
- "What is the difference between `return` and `console.log`?"

---

## Part 0: Warm-Up — The Repeated Code (5 minutes)

### Problem

A program needs to greet three different people. The student copies the same code:

```javascript
console.log("Hello, John!");
console.log("Hello, Jane!");
console.log("Hello, Bob!");
```

### Guess

Ask: "What if we need to greet 100 people? How can we write this once and use it many times?"

### Explain

A function is a reusable block of code. We define it once and call it whenever we need it.

### Live Code

```javascript
function greet(name) {
  console.log("Hello, " + name + "!");
}

greet("John");
greet("Jane");
greet("Bob");
```

### Review

Functions save time and reduce mistakes. The input `name` is called a parameter. The call `greet("John")` passes an argument.

---

## Part 1: Function Declarations and Calls

### 1.1 Basic Function

#### Problem

Add two numbers. We want to reuse the addition.

#### Live Code

```javascript
function add(a, b) {
  return a + b;
}

let result = add(5, 3);
console.log(result);        // 8
console.log(add(10, 20));   // 30
```

#### Challenge 1.1 — Multiply (individual, 3 minutes)

- **Requirement:** Write a function `multiply(a, b)` that returns the product.
- **Time limit:** 3 minutes

### 1.2 Function Without Return

#### Problem

A function that only prints a message.

#### Live Code

```javascript
function logMessage(message) {
  console.log(message);
}

let output = logMessage("Hello");
console.log(output); // undefined
```

#### Challenge 1.2 — Return vs Log (individual, 3 minutes)

- **Requirement:** Predict what `output` will be, then run the code. Explain why.
- **Time limit:** 3 minutes

### 1.3 Multiple Parameters

#### Live Code

```javascript
function createFullName(firstName, lastName, middleName) {
  if (middleName) {
    return `${firstName} ${middleName} ${lastName}`;
  }
  return `${firstName} ${lastName}`;
}

console.log(createFullName("John", "Doe"));                 // "John Doe"
console.log(createFullName("John", "Doe", "William"));      // "John William Doe"
```

#### Challenge 1.3 — Full Name (individual, 4 minutes)

- **Requirement:** Write a function `getInitials(firstName, lastName)` that returns `"J. Doe"`.
- **Time limit:** 4 minutes

---

## Part 2: Return Statement

### 2.1 Single Return

#### Problem

A function should give back a result that we can use later.

#### Live Code

```javascript
function square(num) {
  return num * num;
}

let area = square(4);
console.log(area); // 16
```

#### Challenge 2.1 — Rectangle Area (individual, 3 minutes)

- **Requirement:** Write a function `rectangleArea(width, height)` that returns the area.
- **Time limit:** 3 minutes

### 2.2 Multiple Returns

#### Problem

Categorize a number as positive, negative, or zero.

#### Live Code

```javascript
function getNumberStatus(num) {
  if (num > 0) return "positive";
  if (num < 0) return "negative";
  return "zero";
}

console.log(getNumberStatus(5));  // "positive"
console.log(getNumberStatus(-5)); // "negative"
console.log(getNumberStatus(0));  // "zero"
```

#### Challenge 2.2 — Grade (individual, 4 minutes)

- **Requirement:** Write a function `getGrade(score)` with early returns for invalid scores and grades A, B, C, D, F.
- **Time limit:** 4 minutes

### 2.3 Returning Different Types

#### Live Code

```javascript
function getValue(type) {
  switch (type) {
    case "number": return 42;
    case "string": return "Hello";
    case "boolean": return true;
    case "array": return [1, 2, 3];
    case "object": return { key: "value" };
    default: return null;
  }
}
```

#### Challenge 2.3 — Describe Value (individual, 4 minutes)

- **Requirement:** Write `describe(value)` that returns `"number"`, `"string"`, `"boolean"`, or `"other"` using `typeof`.
- **Time limit:** 4 minutes

---

## Part 3: Default Parameters

### 3.1 Basic Defaults

#### Problem

A greeting function should work even if the caller does not pass a name.

#### Live Code

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("John"); // "Hello, John!"
greet();       // "Hello, Guest!"
```

#### Challenge 3.1 — Default Country (individual, 3 minutes)

- **Requirement:** Write `createUser(name, country = "Unknown")` that returns an object.
- **Time limit:** 3 minutes

### 3.2 Multiple Defaults

#### Live Code

```javascript
function createUser(name = "Anonymous", age = 0, country = "Unknown") {
  return { name, age, country };
}

console.log(createUser("John", 30, "USA"));
console.log(createUser("John", 30));
console.log(createUser("John"));
console.log(createUser());
```

#### Challenge 3.2 — Default Price (individual, 4 minutes)

- **Requirement:** Write `calculateTotal(price, quantity = 1, tax = 0.1)` that returns the total.
- **Time limit:** 4 minutes
- **Hint:** `price * quantity * (1 + tax)`.

### 3.3 Defaults with undefined and Other Falsy Values

#### Live Code

```javascript
function setVolume(value = 50) {
  console.log(value);
}

setVolume();          // 50
setVolume(undefined); // 50
setVolume(null);      // null
setVolume(0);         // 0
setVolume("");        // ""
```

#### Explain

Only `undefined` triggers the default. `null`, `0`, and `""` are real values.

---

## Part 4: Rest Parameters

### 4.1 Basic Rest

#### Problem

Add any number of values without knowing how many in advance.

#### Live Code

```javascript
function sumAll(...numbers) {
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  return sum;
}

console.log(sumAll(1, 2, 3));      // 6
console.log(sumAll(1, 2, 3, 4, 5)); // 15
console.log(sumAll());              // 0
```

#### Challenge 4.1 — Average (individual, 4 minutes)

- **Requirement:** Write `average(...numbers)` that returns the average.
- **Time limit:** 4 minutes

### 4.2 Rest with Regular Parameters

#### Live Code

```javascript
function greetAll(greeting, ...names) {
  names.forEach(name => {
    console.log(`${greeting}, ${name}!`);
  });
}

greetAll("Hello", "John", "Jane", "Bob");
```

#### Challenge 4.2 — Build HTML (individual, 4 minutes)

- **Requirement:** Write `buildHTML(tag, ...content)` that returns `<tag>content joined</tag>`.
- **Time limit:** 4 minutes
- **Hint:** `content.join("")`.

### 4.3 Rest vs arguments

#### Live Code

```javascript
// Old way
function oldSum() {
  let sum = 0;
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}

// New way
function newSum(...numbers) {
  return numbers.reduce((sum, num) => sum + num, 0);
}

console.log(oldSum(1, 2, 3, 4, 5)); // 15
console.log(newSum(1, 2, 3, 4, 5)); // 15
```

---

## Bug Hunt 1

### Problem

Find the bugs in this function.

```javascript
function calculateDiscount(price, discount = 10) {
  if (price < 0) {
    return "Invalid";
  }
  let final = price - (price * discount / 100);
  console.log(final);
}

let result = calculateDiscount(100);
console.log("Result:", result);
```

### Issues

1. The function uses `console.log` instead of `return`, so `result` is `undefined`.
2. The discount should not be returned as a negative if `discount` is `0` or `100`. The validation is fine but the design may not match expectations.

### Fixed Version

```javascript
function calculateDiscount(price, discount = 10) {
  if (price < 0) {
    return "Invalid price";
  }
  let final = price - (price * discount / 100);
  return final;
}

console.log(calculateDiscount(100));        // 90
console.log(calculateDiscount(100, 20));    // 80
console.log(calculateDiscount(-50));        // "Invalid price"
```

### Points

1 point for each found issue.

---

## Part 5: Anonymous Functions and Function Expressions

### 5.1 Function Expression

#### Problem

Assign a function to a variable.

#### Live Code

```javascript
const multiply = function(a, b) {
  return a * b;
};

console.log(multiply(5, 3)); // 15
```

#### Challenge 5.1 — Anonymous Greeting (individual, 3 minutes)

- **Requirement:** Create an anonymous function assigned to `greet` and call it.
- **Time limit:** 3 minutes

### 5.2 IIFE

#### Problem

Run a function immediately to avoid global variables.

#### Live Code

```javascript
(function() {
  console.log("This function runs immediately!");
})();

(function(name) {
  console.log(`Hello, ${name}!`);
})("John");

const result = (function(a, b) {
  return a + b;
})(5, 3);
console.log(result); // 8
```

#### Challenge 5.2 — IIFE Sum (individual, 4 minutes)

- **Requirement:** Write an IIFE that prints the product of `7` and `8`.
- **Time limit:** 4 minutes

### 5.3 Functions in Objects

#### Live Code

```javascript
const calculator = {
  add: function(a, b) { return a + b; },
  subtract: function(a, b) { return a - b; },
  multiply: function(a, b) { return a * b; },
  divide: function(a, b) {
    if (b === 0) return "Cannot divide by zero";
    return a / b;
  }
};

console.log(calculator.add(5, 3));
console.log(calculator.divide(10, 0));
```

#### Challenge 5.3 — Object of Operations (individual, 5 minutes)

- **Requirement:** Create an object `math` with `power` and `root` functions.
- **Time limit:** 5 minutes

---

## Part 6: Nested Functions and Closures

### 6.1 Returning a Function

#### Problem

Create a function that makes other greeting functions.

#### Live Code

```javascript
function createGreeter(greeting) {
  return function(name) {
    return `${greeting}, ${name}!`;
  };
}

const sayHello = createGreeter("Hello");
const sayGoodbye = createGreeter("Goodbye");

console.log(sayHello("John"));    // "Hello, John!"
console.log(sayGoodbye("John"));  // "Goodbye, John!"
```

#### Challenge 6.1 — Function Factory (individual, 4 minutes)

- **Requirement:** Write `createMultiplier(factor)` that returns a function.
- **Time limit:** 4 minutes

### 6.2 Counter with Closure

#### Live Code

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() { return ++count; },
    decrement: function() { return --count; },
    getCount: function() { return count; }
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1.increment()); // 1
console.log(counter1.increment()); // 2
console.log(counter2.increment()); // 1
console.log(counter1.getCount());  // 2
```

#### Challenge 6.2 — Bank Account (individual, 6 minutes)

- **Requirement:** Write `createBankAccount(balance)` that returns `deposit(amount)`, `withdraw(amount)`, and `getBalance()`.
- **Time limit:** 6 minutes
- **Hint:** Use closure to keep `balance` private.

### 6.3 Private Data

#### Live Code

```javascript
function createBankAccount(initialBalance) {
  let balance = initialBalance;

  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        return `Deposited $${amount}. New balance: $${balance}`;
      }
      return "Invalid deposit";
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return `Withdrew $${amount}. New balance: $${balance}`;
      }
      return "Invalid withdrawal";
    },
    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount(100);
console.log(account.deposit(50));   // 150
console.log(account.withdraw(30));  // 120
console.log(account.getBalance());  // 120
```

---

## Part 7: Arrow Functions

### 7.1 Basic Arrow

#### Problem

Write shorter functions.

#### Live Code

```javascript
// Traditional
function add(a, b) {
  return a + b;
}

// Arrow
const addArrow = (a, b) => {
  return a + b;
};

// Concise
const addConcise = (a, b) => a + b;

console.log(addConcise(5, 3));
```

#### Challenge 7.1 — Arrow Square (individual, 3 minutes)

- **Requirement:** Write `const square = ...` as a concise arrow function.
- **Time limit:** 3 minutes

### 7.2 Single Parameter and No Parameters

#### Live Code

```javascript
const square = num => num * num;
const sayHello = () => console.log("Hello!");

console.log(square(4));
sayHello();
```

#### Challenge 7.2 — Arrow Greeting (individual, 3 minutes)

- **Requirement:** Write `const greet = name => ...` that returns a string.
- **Time limit:** 3 minutes

### 7.3 Returning Objects

#### Live Code

```javascript
const createUser = (name, age) => ({ name, age });
console.log(createUser("John", 30)); // { name: "John", age: 30 }
```

#### Challenge 7.3 — Arrow Object (individual, 4 minutes)

- **Requirement:** Write `const makeBook = (title, pages) => ...` that returns `{ title, pages }`.
- **Time limit:** 4 minutes

### 7.4 Arrow as Callback

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(function(num) {
  return num * 2;
});

const doubledArrow = numbers.map(num => num * 2);
console.log(doubledArrow); // [2, 4, 6, 8, 10]
```

#### Challenge 7.4 — Filter Adults (individual, 4 minutes)

- **Requirement:** Use an arrow function with `filter` to return users aged `18` or older.
- **Time limit:** 4 minutes

---

## Part 8: Practical Function Projects

### 8.1 Calculator

#### Live Code

```javascript
function calculator(operation, ...numbers) {
  if (numbers.length === 0) return "No numbers provided";

  switch (operation) {
    case "add":
      return numbers.reduce((sum, num) => sum + num, 0);
    case "subtract":
      return numbers.reduce((result, num) => result - num);
    case "multiply":
      return numbers.reduce((product, num) => product * num, 1);
    case "divide":
      if (numbers.includes(0)) return "Cannot divide by zero";
      return numbers.reduce((result, num) => result / num);
    default:
      return "Invalid operation";
  }
}

console.log(calculator("add", 1, 2, 3));
console.log(calculator("multiply", 2, 3, 4));
console.log(calculator("divide", 100, 2, 5));
```

### 8.2 Validator

#### Live Code

```javascript
function validateUser(user) {
  const errors = [];

  if (!user.name || user.name.trim() === "") {
    errors.push("Name is required");
  }

  if (!user.email || !user.email.includes("@")) {
    errors.push("Valid email is required");
  }

  if (!user.age || user.age < 0 || user.age > 120) {
    errors.push("Valid age (0-120) is required");
  }

  return {
    isValid: errors.length === 0,
    errors
  };
}

console.log(validateUser({ name: "John", email: "john@example.com", age: 30 }));
console.log(validateUser({ name: "", email: "invalid", age: -5 }));
```

### 8.3 Flexible Calculator

#### Live Code

```javascript
function flexibleCalculator(...args) {
  if (args.length === 0) return 0;

  const operation = args[0];
  const numbers = args.slice(1);

  if (numbers.length === 0) return 0;

  switch (operation) {
    case "+":
      return numbers.reduce((sum, n) => sum + n, 0);
    case "-":
      return numbers.reduce((result, n) => result - n);
    case "*":
      return numbers.reduce((product, n) => product * n, 1);
    case "/":
      return numbers.reduce((result, n) => result / n);
    default:
      return "Invalid operation";
  }
}

console.log(flexibleCalculator("+", 1, 2, 3, 4));
console.log(flexibleCalculator("*", 2, 3, 4));
```

---

## Bug Hunt 2

### Problem

Find the bugs in this closure code.

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() {
      count++;
    },
    getCount: function() {
      return count;
    }
  };
}

let c = createCounter();
c.increment();
c.increment();
console.log(c.count);
```

### Issues

1. `count` is private. `c.count` is `undefined` because the outer variable is not accessible from outside.
2. `increment` does not return the new count.

### Fixed Version

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() {
      return ++count;
    },
    getCount: function() {
      return count;
    }
  };
}

let c = createCounter();
console.log(c.increment()); // 1
console.log(c.increment()); // 2
console.log(c.getCount());  // 2
```

### Points

1 point for each found issue.

---

## Group Challenge: Function Builder

- **Time:** 10 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team must write one small function for each situation.
- **Scoring:** 2 points per correct function. The team with the most points wins.

### Situations

1. `celsiusToFahrenheit(c)`.
2. `getInitials(firstName, lastName)`.
3. `sumEven(...numbers)`.
4. `createMultiplier(factor)`.
5. `validateEmail(email)`.

### Instructor Answer Key

```javascript
function celsiusToFahrenheit(c) {
  return (c * 9 / 5) + 32;
}

function getInitials(firstName, lastName) {
  return `${firstName[0].toUpperCase()}. ${lastName}`;
}

function sumEven(...numbers) {
  return numbers.filter(n => n % 2 === 0).reduce((sum, n) => sum + n, 0);
}

function createMultiplier(factor) {
  return n => n * factor;
}

function validateEmail(email) {
  if (!email) return "Email is required";
  if (!email.includes("@")) return "Email must contain @";
  if (!email.includes(".")) return "Email must contain .";
  return "Valid";
}
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Add and Multiply (3 minutes)

- **Requirement:** Write `add(a, b)` and `multiply(a, b)`.

### Level 2: Default Tax (4 minutes)

- **Requirement:** Write `calculateTotal(price, tax = 0.1)`.

### Level 3: Sum Rest (4 minutes)

- **Requirement:** Write `sum(...numbers)` that adds all arguments.

### Level 4: Arrow Average (4 minutes)

- **Requirement:** Write `const average = ...` as an arrow function.

### Level 5: Validator Function (5 minutes)

- **Requirement:** Write `validatePassword(password)` that returns `true` if the password has at least `8` characters.

### Level 6: Bank Account (6 minutes)

- **Requirement:** Write a closure-based bank account with `deposit`, `withdraw`, and `getBalance`.

---

## Mini Project: Function Dashboard

### Time

25 minutes

### Goal

Combine function declarations, parameters, return, defaults, rest, arrow functions, and a simple closure in one HTML page.

### Requirements for the Students

1. Create an HTML page with sections:
   - **Greeting:** input for name, optional age; output a personalized greeting.
   - **Calculator:** inputs for numbers and operation; output the result.
   - **Counter:** buttons to `+`, `-`, and `show` the count using a closure.

2. Use these concepts:
   - Function declaration
   - Default parameters
   - Rest parameters
   - Arrow function as callback
   - Closure for the counter

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Function Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    input, select, button { padding: 8px; margin: 5px; }
    .output { background: #f0f0f0; padding: 15px; border-radius: 4px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>Function Dashboard</h1>

  <div class="section">
    <h2>Greeting</h2>
    <input type="text" id="greetName" placeholder="Name" value="John">
    <input type="number" id="greetAge" placeholder="Age">
    <button onclick="showGreeting()">Greet</button>
    <div id="greetingOutput" class="output"></div>
  </div>

  <div class="section">
    <h2>Calculator</h2>
    <input type="text" id="calcNumbers" placeholder="Numbers (comma separated)" value="1,2,3,4">
    <select id="calcOp">
      <option value="add">Add</option>
      <option value="subtract">Subtract</option>
      <option value="multiply">Multiply</option>
    </select>
    <button onclick="showCalc()">Calculate</button>
    <div id="calcOutput" class="output"></div>
  </div>

  <div class="section">
    <h2>Counter</h2>
    <button onclick="counter.increment(); showCounter()">+</button>
    <button onclick="counter.decrement(); showCounter()">-</button>
    <div class="output" id="counterOutput">0</div>
  </div>

  <script>
    // Greeting
    const makeGreeting = (name = "Guest", age) => {
      if (age && age >= 18) return `Hello, ${name}. You are an adult.`;
      return `Hello, ${name}.`;
    };

    function showGreeting() {
      let name = document.getElementById("greetName").value.trim();
      let age = parseInt(document.getElementById("greetAge").value);
      let text = makeGreeting(name || undefined, isNaN(age) ? undefined : age);
      document.getElementById("greetingOutput").textContent = text;
    }

    // Calculator
    function calculate(operation, ...numbers) {
      switch (operation) {
        case "add": return numbers.reduce((a, b) => a + b, 0);
        case "subtract": return numbers.reduce((a, b) => a - b);
        case "multiply": return numbers.reduce((a, b) => a * b, 1);
        default: return "Invalid";
      }
    }

    function showCalc() {
      let input = document.getElementById("calcNumbers").value;
      let numbers = input.split(",").map(n => parseFloat(n.trim())).filter(n => !isNaN(n));
      let op = document.getElementById("calcOp").value;
      let result = calculate(op, ...numbers);
      document.getElementById("calcOutput").textContent = `Result: ${result}`;
    }

    // Counter with closure
    function createCounter() {
      let count = 0;
      return {
        increment: () => count++,
        decrement: () => count--,
        getCount: () => count
      };
    }

    const counter = createCounter();

    function showCounter() {
      document.getElementById("counterOutput").textContent = counter.getCount();
    }
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "What is the difference between `name || undefined` and just `name` in the greeting?"
- "Which function uses rest parameters?"
- "Why is the counter shared between button clicks?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
function multiply(a, b) {
  return a * b;
}
```

### Challenge 1.2

`output` is `undefined` because `logMessage` does not return a value.

### Challenge 1.3

```javascript
function getInitials(firstName, lastName) {
  return `${firstName[0].toUpperCase()}. ${lastName}`;
}
```

### Challenge 2.1

```javascript
function rectangleArea(width, height) {
  return width * height;
}
```

### Challenge 2.2

```javascript
function getGrade(score) {
  if (score < 0 || score > 100) return "Invalid";
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}
```

### Challenge 2.3

```javascript
function describe(value) {
  if (typeof value === "number") return "number";
  if (typeof value === "string") return "string";
  if (typeof value === "boolean") return "boolean";
  return "other";
}
```

### Challenge 3.1

```javascript
function createUser(name, country = "Unknown") {
  return { name, country };
}
```

### Challenge 3.2

```javascript
function calculateTotal(price, quantity = 1, tax = 0.1) {
  return price * quantity * (1 + tax);
}
```

### Challenge 4.1

```javascript
function average(...numbers) {
  if (numbers.length === 0) return 0;
  return numbers.reduce((sum, n) => sum + n, 0) / numbers.length;
}
```

### Challenge 4.2

```javascript
function buildHTML(tag, ...content) {
  return `<${tag}>${content.join("")}</${tag}>`;
}
```

### Challenge 5.1

```javascript
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

### Challenge 5.2

```javascript
(() => console.log(7 * 8))();
```

### Challenge 5.3

```javascript
const math = {
  power: (a, b) => Math.pow(a, b),
  root: a => Math.sqrt(a)
};
```

### Challenge 6.1

```javascript
function createMultiplier(factor) {
  return n => n * factor;
}
```

### Challenge 6.2

```javascript
function createBankAccount(balance) {
  return {
    deposit(amount) {
      if (amount > 0) balance += amount;
      return balance;
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) balance -= amount;
      return balance;
    },
    getBalance() {
      return balance;
    }
  };
}
```

### Challenge 7.1

```javascript
const square = num => num * num;
```

### Challenge 7.2

```javascript
const greet = name => `Hello, ${name}!`;
```

### Challenge 7.3

```javascript
const makeBook = (title, pages) => ({ title, pages });
```

### Challenge 7.4

```javascript
let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 16 }
];
let adults = users.filter(u => u.age >= 18);
console.log(adults);
```

### Individual Challenges Solutions

```javascript
// Level 1
function add(a, b) { return a + b; }
function multiply(a, b) { return a * b; }

// Level 2
function calculateTotal(price, tax = 0.1) {
  return price * (1 + tax);
}

// Level 3
function sum(...numbers) {
  return numbers.reduce((s, n) => s + n, 0);
}

// Level 4
const average = (...numbers) =>
  numbers.length === 0 ? 0 : numbers.reduce((s, n) => s + n, 0) / numbers.length;

// Level 5
function validatePassword(password) {
  return password.length >= 8;
}

// Level 6
function createBankAccount(balance) {
  return {
    deposit(amount) { if (amount > 0) balance += amount; return balance; },
    withdraw(amount) { if (amount > 0 && amount <= balance) balance -= amount; return balance; },
    getBalance() { return balance; }
  };
}
```

</details>

---

## Review Questions

1. What is the correct syntax for a function declaration?
   - [x] function add(a, b) { return a + b; }
   - [ ] const add = function(a, b) { return a + b; }
   - [ ] const add = (a, b) => a + b;
   - [ ] All of the above

2. What does a function return if no return statement is provided?
   - [ ] null
   - [x] undefined
   - [ ] 0
   - [ ] Error

3. How do you set a default parameter value?
   - [x] function greet(name = "Guest") {}
   - [ ] function name = "Guest" {}
   - [ ] function name(default = "Guest") {}
   - [ ] function name("Guest") {}

4. What symbol is used for rest parameters?
   - [ ] *
   - [x] ...
   - [ ] &
   - [ ] #

5. What is a closure?
   - [ ] A way to close functions
   - [x] A function with access to outer scope variables
   - [ ] A type of loop
   - [ ] A method to end execution

6. Which arrow function syntax is correct for a single parameter?
   - [ ] (num) => num * 2
   - [x] num => num * 2
   - [ ] => num * 2
   - [ ] num -> num * 2

7. Can you have multiple rest parameters in one function?
   - [ ] Yes
   - [x] No
   - [ ] Only if they're the same type
   - [ ] Only in arrow functions

8. What is an IIFE?
   - [ ] A function that returns immediately
   - [x] A function that runs immediately after definition
   - [ ] A function inside another function
   - [ ] A function with no parameters

9. How do you return an object in a concise arrow function?
   - [ ] => { key: value }
   - [x] => ({ key: value })
   - [ ] => key: value
   - [ ] => return { key: value }

10. What is the main advantage of arrow functions?
    - [ ] They're always faster
    - [x] They have a shorter syntax and lexical this
    - [ ] They can be hoisted
    - [ ] They support more features

---

## Additional Resources

- [MDN: Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [MDN: Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [MDN: Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [JavaScript.info: Functions](https://javascript.info/function-basics)
- [JavaScript.info: Arrow Functions](https://javascript.info/arrow-functions-basics)
